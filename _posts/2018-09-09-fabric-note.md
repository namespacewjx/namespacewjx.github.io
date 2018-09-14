---
layout:     post
title:      "Hyperledger Fabric学习笔记"
author:     "吴俊贤"
tags:
    - Fabric
    - Chaincode
catalog: true
---

下列内容基于[1.2版本文档](https://hyperledger-fabric.readthedocs.io/en/release-1.2/whatis.html)编写而成。

# 系统架构

## 交易流程

### 1. 初始化交易

首先构建一个交易请求（transaction proposal），可以使用SDK创建请求。交易请求是调用chaincode函数来读写账本上的数据的请求。SDK会使用用户的密码学密钥给交易请求签名。

### 2. 背书节点验证签名并执行交易

背书节点验证：

1. 交易请求格式正确
2. 过去没有被提交（防止重放攻击）
3. 签名无误
4. 提交请求的人在channel上有适当的权限，即是频道上的Writer

验证完毕后，执行chaincode，使用当前的状态数据库的数据，并且产生返回值、读取数据集，写入数据集。但是**当前还没有更新账本**。然后，就会把这些集合，以及背书节点的签名，称为交易请求响应（Proposal Response），返回给发起者（的SDK）。

### 3. 检查交易请求响应

发起者的应用程序验证背书节点的签名，检查从多个背书节点返回的交易请求响应，查看是否一致。若chaincode只是查询结果，则只检查结果，而不会提交到排序服务。若需要更新账本，则先检查当前交易是否满足背书策略，然后提交交易到排序服务，更新账本。

如果应用程序没有检查请求响应，或者提交了一个没有背书的交易，背书策略将会被其他节点执行，并暂停提交验证流程。

### 4. 客户端将背书数据放入交易中

应用程序通过在发送给排序服务的信息中包含交易信息，向节点广播交易请求和交易响应。排序服务不需要检查交易的所有内容，只需要接收交易信息，然后给每个channel的交易按时间顺序排序（orders them chronologically by channel // TODO 没有进一步解释这句话真的不太懂），然后给每个channel创建交易区块。

### 5. 验证与提交交易

交易区块被发送到channel上所有节点。需要验证块内的交易，确保

1. 满足背书策略
2. 满足目前读取集合中的变量，在账本上没有被改变。

### 6. 更新账本

每个节点把区块加到链后面，每个有效的交易的写集合变量，提交其更改到账本数据库，然后通知客户端，所做的交易已经被提交到链上，无法更改，同时通知交易有效还是无效。

# 链码chaincode

## 链码的生命周期

链码的生命周期，目前（v1.2）有四个：

- package
- install
- instantiate
- upgrade

目前版本中，还没有实现start和stop两个方法，用于控制链码的开始运行和停止运行。因此，只要链码被实例化之后，链码就会一直在运行。

## Package

打包过程中，包含三个主要的流程：

- 定义`ChaincodeDeploymentSpec`，或CDS。CDS定义了Chaincode代码、名称及版本。
- 定义实例化策略，与背书策略（Endorsement Policy）语法上一致
- 这份Chaincode所有者（一个或多个）的签名

## 创建Chaincode Package

打包命令如下

```bash
peer chaincode package -n mycc -p <chaincode包名> -v 0 -s -S -i "<实例化策略>" <输出文件名>
```

其中的选项含义如下：

- `-s`：指定该package能够被多个所有者签名，而不是只是单纯创建一个CDS。当本选项设置了之后，必须设置`-S`，才能让其他的所有者继续签名，否则，将会创建一个签名了的CDS，仅仅包含实例化策略（Otherwise, the process will create a SignedCDS that includes only the instantiation policy in addition to the CDS）// TODO 这里还是有点疑问？究竟是有没有签名在里面？
- `-S`：指定使用本peer配置文件`core.yaml`中的`localMspid`，给CDS签名。若不签名，则无法被其他的所有者使用`signpackage`命令签名
- `-i`：指定实例化策略，本策略语法与背书策略语法一致

## 给package签名

Chaincode的所有者可以用这个命令给Chaincode签名

```bash
peer chaincode signpackage <输入文件> <输出文件>
```

## 安装Chaincode

`install`交易将Chaincode源代码打包到一个预定义的格式中，名为ChaincodeDeploymentSpec，或CDS，并安装到运行Chaincode的一个节点上。

注意，*需要将Chaincode安装到频道中的每个运行Chaincode的背书节点上。这是为了保护Chaincode的逻辑。对于其他的节点，虽然无法运行Chaincode，但是仍能够提交交易到账本或者验证账本上的交易。*

如下的示例命令部署一个Chaincode

```bash
peer chaincode install -n <chaincode name> -v <version> -p <package name>
```

安装操作在内部创建一个已签名的CDS，并发送到本地的peer，随后peer会调用`lifecycle system chaincode`（LSCC）（具体看[这里](https://hyperledger-fabric.readthedocs.io/en/latest/chaincode4noah.html#system-chaincode)）的`Install`方法。

## Instantiate实例化

实例化交易调用LSCC，在channel上创建并初始化chaincode。

这个过程将Chaincode与channel绑定起来，一个chaincode能够安装到多个channel上，并且不同channel的chaincode相互独立，互不影响。

Chaincode的创建者必须符合SignedCDS中的实例化策略，并且在channel上有写权限，以防止恶意节点部署恶意chaincode，或煽动其他成员在一个无任何限制的channel上执行chaincode。

实例化交易同时会给该chaincode创建channel上的背书策略，描述chaincode结果需要哪些证明才能被channel的成员接受。

如下是实例化的示例命令

```bash
peer chaincode instantiate -n <chaincode name> -v <version> -c <arguments> -P <endorsement policy>
```

实例化以后，chaincode进入激活状态，准备好处理类型为[ENDORSER_TRANSACTION](https://github.com/hyperledger/fabric/blob/master/protos/common/common.proto#L48)的交易请求。// TODO 这里也是不太懂的

## 升级Chaincode

只需要修改版本号，就能够升级Chaincode。除了名字是必有的外，所有者和实例化策略都是可选的。

升级之前，需要把新版本的chaincode安装在必要的背书节点上。升级交易与实例化交易是类似的，只是绑定新版本的chaincode到channel上。其他绑定了旧版本chaincode的通道，同样会运行旧版本的。换句话说，**升级事务仅仅影响提交了升级事务的channel**。

另外，由于多个版本的chaincode可能都处于激活状态，因此升级过程并不会移除旧版本chaincode。

升级chaincode时，仅检查当前实例化策略，并不会检查是否满足新的实例化策略（命令行中指定的），保证原有的成员都能够升级chaincode。

注意，升级时，chaincode的`Init`函数会被调用，执行数据升级或重新初始化的步骤，因此需要 *注意防止在升级过程中，重置了当前的状态（世界状态，也就是账本数据库的数据）*。

# 系统chaincode

系统chaincode编码模型是与普通chaincode相同，只是其直接运行在peer进程上，也就是，系统chaincode内置在了peer程序中。

系统chaincode是为了降低gRPC调用的开销而产生的，但是丧失了一点灵活性。更新系统Chaincode必须更新peer程序，参数是定死的，并且没背书策略。

# 私有数据的使用

## 私有数据介绍

为实现将部分数据仅能够被channel上的部分组织所使用，从1.2开始，引入了私有数据集合的功能，使部分的组织能够提交、查询或给私有数据背书。

私有数据集合包含两个主要元素：

- 实际的私有数据：只有授权节点才可以查看的数据，保存在了授权节点的私有数据库中，也叫side database。排序节点不参与这些事务且无法看到这些数据。
- 数据的哈希：背书、排序并写入所有节点上的账本中。用于作为交易的证据、状态的验证以及审计。

若需要将私有数据传给第三方，第三方只需要检查账本上的哈希是否是私有数据的哈希，即可验证真假。

## 私有数据对比私有频道

私有频道是指在一个公开的频道的基础上，需要共享私有数据的多个组织，另外加入一个频道，在该频道上写入私有数据。

若整个交易和账本都需要对其他的节点保密时，使用私有频道。

若交易需要共享，且仅有部分节点有权限看到部分的数据的时候，使用私有数据集合。另外，由于私有数据是点对点传输的，而不是在区块中传输，因此如果要对排序服务保密，也需要使用私有数据集合。
