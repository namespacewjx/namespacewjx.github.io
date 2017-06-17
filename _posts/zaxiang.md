printf函数真慢啊，搞得我程序全部超时。换了之后，就通过了。。。

Collections.binraySearch函数啊，在有相同元素的情况下，不能保证找到的是哪一个元素。

Java日期时间类不是线程安全的，如果需要在多线程环境处理，推荐joda-Time库。

使用Easyui库时，若用ajax加载新的包含easyui组件的页面，需要用parser函数解析一遍那个页面，参数是那个页面的根元素就行。
