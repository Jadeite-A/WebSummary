# HTML

### 1.http和https

#### 区别

http传输的数据都是未加密的，也就是明文的，ttps协议是由http和ssl协议构建的可进行加密传输和身份认证的网络协议，比http协议的安全性更高。
主要的区别如下：

> Https协议需要ca证书，费用较高。

> http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。

> 使用不同的链接方式，端口也不同，一般而言，http协议的端口为80，https的端口为443

> http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全。

#### https优点
> 使用HTTPS协议可认证用户和服务器，确保数据发送到正确的客户机和服务器；

> HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，要比http协议安全，可防止数据在传输过程中不被窃取、改变，确保数据的完整性。

> HTTPS是现行架构下最安全的解决方案，虽然不是绝对安全，但它大幅增加了中间人攻击的成本。

#### https缺点
> https握手阶段比较费时，会使页面加载时间延长50%，增加10%~20%的耗电。

> https缓存不如http高效，会增加数据开销。

> SSL证书也需要钱，功能越强大的证书费用越高。

> SSL证书需要绑定IP，不能再同一个ip上绑定多个域名，ipv4资源支持不了这种消耗。

### 2.TCP和UDP
#### tcp三次握手、四次挥手

所谓的三次握手即TCP连接的建立。这个连接必须是一方主动打开，另一方被动打开的。详细过程如下：
> **第一次握手：**建立连接时，客户端发送syn包（seq=j）到服务器，并进入SYN_SENT状态，等待服务器确认;

> **第二次握手：**服务器收到syn包，必须确认客户的SYN（ack=j+1），同时自己也发送一个SYN包（seq=k），即SYN+ACK包，此时服务器进入SYN_RECV状态;

> **第三次握手：**客户端收到服务器的SYN+ACK包，向服务器发送确认包ACK(ack=k+1），此包发送完毕，客户端和服务器进入ESTABLISHED（TCP连接成功）状态，完成三次握手.

完成三次握手，客户端与服务器开始传送数据.
数据传输完毕后，需解除TCP的连接，即四次挥手：
> 1）首先客户端想要释放连接，向服务器端发送一段TCP报文

> 2）服务器端接收到从客户端发出的TCP报文之后，确认了客户端想要释放连接，随后服务器端结束ESTABLISHED阶段，进入CLOSE-WAIT阶段（半关闭状态）并返回一段TCP报文

> 3）服务器端自从发出ACK确认报文之后，经过CLOSED-WAIT阶段，做好了释放服务器端到客户端方向上的连接准备，再次向客户端发出一段TCP报文

> 4）客户端收到从服务器端发出的TCP报文，确认了服务器端已做好释放连接的准备，结束FIN-WAIT-2阶段，进入TIME-WAIT阶段，并向服务器端发送一段报文

#### TCP和UDP的区别
（1）TCP是面向连接的，udp是无连接的即发送数据前不需要先建立链接。

（2）TCP提供可靠的服务。也就是说，通过TCP连接传送的数据，无差错，不丢失，不重复，且按序到达;UDP尽最大努力交付，即不保证可靠交付。 并且因为tcp可靠，面向连接，不会丢失数据因此适合大数据量的交换。

（3）TCP是面向字节流，UDP面向报文，并且网络出现拥塞不会使得发送速率降低（因此会出现丢包，对实时的应用比如IP电话和视频会议等）。

（4）TCP只能是1对1的，UDP支持1对1,1对多。

（5）TCP的首部较大为20字节，而UDP只有8字节。

（6）TCP是面向连接的可靠性传输，而UDP是不可靠的。

### 3.在地址栏里输入一个URL,到这个页面呈现出来，中间会发生什么？
(1)输入url之后，首先需要找到这个url域名的服务器的ip，使用DNS地址解析协议。***寻找的路径依次是：*** **浏览器缓存**—>**本地系统缓存**–>**网络服务提供商服务器**–>**根DNS服务器**

(2)获取到ip地址后，浏览器根据ip以及端口号等，构造一个http请求。

> 这个请求报文会包括这次请求的信息，主要是请求方法（GET/POST/HEAD/OPTIONS/PUT/ DELETE/TRACE/CONNECT等8种），请求说明和请求附带的数据，并将这个http请求封装在一个tcp包中，这个tcp包会依次经过传输层，网络层，数据链路层，物理层到达服务器

(3)找到服务器之后，开始通过TCP协议建立传输连接(tcp三次握手)。

(4)服务器解析这个请求来作出响应，返回相应的html给浏览器。然后断开TCP连接（TCP四次挥手）。
> http响应报文包括，响应行、响应头和响应主体。返回的http状态码有其对应的规则。

> 1xx：指示信息–表示请求已接收，继续处理。

> 2xx：成功–表示请求已被成功接收、理解、接受。

> 3xx：重定向–要完成请求必须进行更进一步的操作。

> 4xx：客户端错误–请求有语法错误或请求无法实现。

> 5xx：服务器端错误–服务器未能实现合法的请求。

(5)浏览器解析html结构。
①解析与构建DOM树
> 在DOM树构建的过程中，由于浏览器自上而下进行解析，如果遇到js脚本和外部js连接，就会停止构建dom树来执行和下载相应的代码，这会造成阻塞。

②构建渲染树
> 浏览器根据外部样式、内部样式和内联样式构建一个CSS对象模型树，CSSOM树。构建完成以后和DOM树合并为渲染树（RenderTree），这里主要做的是排除非视觉节点，比如script，meta标签和排除display为none的节点还会涉及到CSS层叠的问题

③布局和绘制
> 布局主要是确定各个元素的位置和尺寸，之后是渲染页面。因为html文件中会含有图片，视频，音频等资源，在解析DOM的过程中，遇到这些都会进行并行下载

**说一下浏览器缓存**
> 缓存分为两种：强缓存和协商缓存，根据响应的header内容来决定。
强缓存相关字段有expires，cache-control。如果cache-control与expires同时存在的话，cache-control的优先级高于expires。

> 协商缓存相关字段有Last-Modified/If-Modified-Since，Etag/If-None-Match

其中还需要注意和缓存相关的，Cache-Control和Expires的区别在于Cache-Control使用相对时间，Expires使用的是基于服务器 端的绝对时间，因为存在时差问题，一般采用Cache-Control，在请求这些有设置了缓存的数据时，会先 查看是否过期，如果没有过期则直接使用本地缓存，过期则请求并在服务器校验文件是否修改，如果上一次 响应设置了ETag值会在这次请求的时候作为If-None-Match的值交给服务器校验，如果一致，继续校验 Last-Modified，没有设置ETag则直接验证Last-Modified，再决定是否返回304

### 4.两种HTTP请求方法：GET和POST
在客户机和服务器之间进行请求-响应时，两种最常被用到的方法是：**GET** 和 **POST**。

- GET - 从指定的资源请求数据。
- POST - 向指定的资源提交要被处理的数据。

#### (1).GET方法
**注意，查询字符串（名称/值对）是在 GET 请求的 URL 中发送的：**
` /test/demo_form.php?name1=value1&name2=value2`
有关 GET 请求的其他一些注释：

> GET 请求可被缓存

> GET 请求保留在浏览器历史记录中

> GET 请求可被收藏为书签

> GET 请求不应在处理敏感数据时使用

> GET 请求有长度限制

> GET 请求只应当用于取回数据

#### (2).POST 方法
**注意，查询字符串（名称/值对）是在 POST 请求的 HTTP 消息主体中发送的：**

```
POST /test/demo_form.php HTTP/1.1
Host: runoob.com
name1=value1&name2=value2
```
有关 POST 请求的其他一些注释：
> POST 请求不会被缓存

> POST 请求不会保留在浏览器历史记录中

> POST 不能被收藏为书签

> POST 请求对数据长度没有要求

#### (3).比较 GET 与 POST
|         | GET()   |  POST()  |
| --------   | -----  | ----  |
| 后退按钮/刷新      | 无害   |   数据会被重新提交（浏览器应该告知用户数据会被重新提交）     |
| 书签        |   可收藏为书签   |   不可收藏为书签   |
| 缓存        |    能被缓存    |  不能被缓存  |
| 编码类型        |    application/x-www-form-urlencoded    |  application/x-www-form-urlencoded or multipart/form-data。为二进制数据使用多重编码。  |
| 历史        |    参数保留在浏览器历史中    |  参数不会保存在浏览器历史中  |
| 对数据长度的限制       |    有限制。当发送数据时，GET 方法向 URL 添加数据，而URL 的长度是受限制的（URL 的最大长度是 2048 个字符）    |  无限制  |
| 对数据类型的限制        |    只允许 ASCII 字符    |  没有限制。也允许二进制数据  |
| 安全性        |    与 POST 相比，GET 的安全性较差，因为所发送的数据是 URL 的一部分。在发送密码或其他敏感信息时绝不要使用    |  POST 比 GET 更安全，因为参数不会被保存在浏览器历史或 web 服务器日志中  |
| 可见性        |    数据在 URL 中对所有人都是可见的    |  数据不会显示在 URL 中  |

#### (4).其他 HTTP 请求方法
|方法         |  描述   | 
| --------   | -----  | 
| HEAD|      与 GET 相同，但只返回 HTTP 报头，不返回文档主体 |  
| PUT   |   上传指定的 URI 表示   |   
| DELETE      |        删除指定资源    |  
| OPTIONS      |     返回服务器支持的 HTTP 方法    |  
| CONNECT      |     把请求连接转换到透明的 TCP/IP 通道   |  

### 5.块元素、行内元素、行内块元素
#### (1)块元素 block element
##### ①特性
- 独霸一行，总是在新行上开始
- 宽度缺省是它父级元素的100%，除非设定一个宽度
- 高度、行高、外边距、内边距都可以设置
- 可以容纳其他内联元素或者其他块元素

##### ②常见块级元素
> address – 地址

> blockquote – 块引用

> center – 举中对齐块

> dir – 目录列表

> div – 常用块级标签，也是CSS layout的主要标签

> dl – 定义列表

> fieldset – form控制组

> form – 交互表单

> h1 – 大标题

> h2 – 副标题

> h3 – 3级标题

> h4 – 4级标题

> h5 – 5级标题

> h6 – 6级标题

> hr – 水平分隔线

> isindex – input prompt

> menu – 菜单列表

> noframes – frames可选内容，（对于不支持frame的浏览器显示此区块内容)

> noscript – 可选脚本内容（对于不支持script的浏览器显示此内容）

> ol – 有序表单

> p – 段落

> pre – 格式化文本

> table – 表格

> ul – 无序列表

#### (2)行内元素 inline element

##### ①特性
- 和其他元素都在一行上，遇到父级元素边界会自动换行
- 高、行高以及内外边距都不可以改变
- 宽度与内容一样宽，且不可改变
- 行内元素只能容纳文本或者其他行内元素

**对于行内元素，需要注意的是：设置宽度width无效，设置高度无效，可以通过设置line-height来设置，设置margin只有左右有效，上下无效，设置padding只有左右有效，上下无效**

##### ②常见的行内元素
> a – 锚点

> abbr – 缩写

> acronym – 首字

> b – 粗体(不推荐)

> bdo – bidi override

> big – 大字体

> br – 换行

> cite – 引用

> code – 计算机代码(在引用源码的时候需要)

> dfn – 定义字段

> em – 强调

> font – 字体设定(不推荐)

> i – 斜体

> img – 图片

> input – 输入框

> kbd – 定义键盘文本

> label – 表格标签

> q – 短引用

> s – 中划线(不推荐)

> samp – 定义范例计算机代码

> select – 项目选择

> small – 小字体文本

> span – 常用内联容器，定义文本内区块

> strike – 中划线

> strong – 粗体强调

> sub – 下标

> sup – 上标

> textarea – 多行文本输入框

> tt – 电传文本

> u – 下划线

##### ③行内元素的间距问题
两个行内元素在一起，会出现一定的间距，即使将border、padding、margin都设置为零也无济于事，那么怎么才能去除这些间距呢？

**Ⅰ.重设字体**
将行内元素的直接父级设置font-size=0px;再给行内元素设置字体大小就可以解决。
**Ⅱ.借助HTML注释**
在两个行内元素之间加入HTML注释，告诉浏览器这中间不是换行也不是空格，而是连接在一起的，这样也可以解决。
#### (3)行内块元素
##### 行内块元素的特性
- 元素排列在一行
- 宽度默认由内容决定
- 元素间默认有间距
- 支持宽高、外边距、内边距的所有样式的设置

### 6.浏览器内核

主要分成两部分：***渲染引擎(layout engineer或Rendering Engine)***和***JS引擎***。
渲染引擎：负责取得网页的内容（HTML、XML、图像等等）、整理讯息（例如加入CSS等），以及计算网页的显示方式，然后会输出至显示器或打印机。浏览器的内核的不同对于网页的语法解释会有不同，渲染的效果也不相同。

JS引擎则：解析和执行javascript来实现网页的动态效果。

**最开始渲染引擎和JS引擎并没有区分的很明确，后来JS引擎越来越独立，内核就倾向于只指渲染引擎。**

#### 常见的浏览器内核有哪些？
```
Trident内核：IE,MaxThon,TT,The World,360,搜狗浏览器等。[又称MSHTML]
Gecko内核：Netscape6及以上版本，FF,MozillaSuite/SeaMonkey等
Presto内核：Opera7及以上。      [Opera内核原为：Presto，现为：Blink;]
Webkit内核：Safari,Chrome等。   [ Chrome的：Blink（WebKit的分支）]
```

### 7.HTML5

#### (1).新增的元素

HTML5 现在已经不是 SGML 的子集，主要是关于图像，位置，存储，多任务等功能的增加。
> 绘画 canvas;
> 用于媒介回放的 video 和 audio 元素;
> 本地离线存储 localStorage 长期存储数据，浏览器关闭后数据不丢失;
> sessionStorage 的数据在浏览器关闭后自动删除;
> 语意化更好的内容元素，比如 article、footer、header、nav、section;
> 表单控件，calendar、date、time、email、url、search;
> 新的技术webworker, websocket, Geolocation;

#### (2).移除的元素：

> 纯表现的元素：basefont，big，center，font, s，strike，tt，u;
> 对可用性产生负面影响的元素：frame，frameset，noframes；

#### (3).离线存储

在用户没有与因特网连接时，可以正常访问站点或应用，在用户与因特网连接时，更新用户机器上的缓存文件。
原理：HTML5的离线存储是基于一个新建的.appcache文件的缓存机制(不是存储技术)，通过这个文件上的解析清单离线存储资源，这些资源就会像cookie一样被存储了下来。之后当网络在处于离线状态下时，浏览器会通过被离线存储的数据进行页面展示。

如何使用：
①、页面头部像下面一样加入一个manifest的属性；

```javascript
<!DOCTYPE HTML>
<html manifest = "cache.manifest">
...
</html>
```

②、在cache.manifest文件的编写离线存储的资源；

```javascript
CACHE MANIFEST
#v0.11
CACHE:
js/app.js
css/style.css
NETWORK:
resourse/logo.png
FALLBACK:
/ /offline.html
```
③、在离线状态时，操作window.applicationCache进行需求实现。

#### 补充：cookies，sessionStorage 和 localStorage 的区别

cookie是网站为了标示用户身份而储存在用户本地终端（Client Side）上的数据（通常经过加密）。
cookie数据始终在同源的http请求中携带（即使不需要），记会在浏览器和服务器间来回传递。
sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存。

**存储大小：**
    cookie数据大小不能超过4k。
    sessionStorage和localStorage 虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大。

**有期时间：**
    localStorage    存储持久数据，浏览器关闭后数据不丢失除非主动删除数据；
    sessionStorage  数据在当前浏览器窗口关闭后自动删除。
    cookie          设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭

