[Content-Type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type)用于client和server之间告知对方交互数据的格式。Client发送给Server数据，和Server返回给Client的数据，都需要优先确认格式，再解析数据。



**Content-Type的语法Syntax**

```plain
Content-Type: text/html; charset=utf-8
Content-Type: multipart/form-data; boundary=something
```

- [media-type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types)：数据格式（主类型/子类型）

- 主类型有：application、audio、video、image、text、font、model、example；message、multipart

- charset：编码格式，大小写敏感
- boundary：用于对multipart数据格式进行分隔的符号。（每部分以“--boundary”分隔，消息主体以“--boundary--”结尾）



**常见的media-type**

| **media-type**                                               | **请求截图**                                                 |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| multipart/form-data：表单上传文件的格式，需指定enctype等于该格式。消息主体里按照字段个数又分为多个结构类似的部分，每部分都是以 --boundary 开始，紧接着内容描述信息，然后是回车，最后是字段具体内容（文本或二进制）。如果传输的是文件，还要包含文件名和文件类型信息。消息主体最后以 --boundary-- 表示结束 | ![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/网络/images/HTTP头Content-Type说明/1711005489708-7a59f4f6-3c62-43cf-9bbf-0fe8ea81370b.png) |
| application/x-www-form-urlencoded：表单提交默认格式。提交的数据按照 key1=val1&key2=val2 的方式进行编码，key和val会进行URL转码 | ![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/网络/images/HTTP头Content-Type说明/1711005472632-f3b8f72f-5f81-4297-a1c5-b06250eb473d.png) |
| application/json：适用于Restful接口，传输标准json格式，推荐使用。 | ![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/网络/images/HTTP头Content-Type说明/1711005800297-8718f443-a77d-4083-bdde-7d4428c8c2a7.png) |
| text/xml：和json差不多，xml更复杂，兼容性没有json强。        | ![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/网络/images/HTTP头Content-Type说明/1711005914919-33ec7a75-7c91-49b3-91f0-d7e76679b36b.png) |
| text/plain：直接传输未加工的文本信息                         |                                                              |



**扩展**

Client请求头中还包含Accept，表示Client能理解的数据格式，希望Server选择一种返回。

HeaderThe **Accept** request HTTP header indicates which content types, expressed as [MIME types](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types), the client is able to understand. The server uses [content negotiation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Content_negotiation) to select one of the proposals and informs the client of the choice with the [Content-Type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type) response header

```plain
Accept: <MIME_type>/<MIME_subtype>
Accept: <MIME_type>/*
Accept: */*

// Multiple types, weighted with the quality value syntax:
Accept: text/html, application/xhtml+xml, application/xml;q=0.9, image/webp, */*;q=0.8
```