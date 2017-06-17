今天遇到一个非常难搞的bug，原来是我对jQuery的post方法不了解。

post方法会把data实参的Content-Type设置为application/x-www-form-urlencoded，这是表单的格式啊。

但是post参数中dataType指定的是希望**服务器回复**的数据格式，并不是我发送过去的data的数据格式。

调试好半天，查了文档才知道原因。

Servlet，在用response.getWriter()方法之前，要先设置"Content-Type"，不然不会更改最终报文的Content-Type。
