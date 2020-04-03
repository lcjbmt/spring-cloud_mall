详细步骤：
1：创建WebServer的类，创建一个start方法来运行程序，创建min方法，
      在min满方法中把WebServer对象创建出来，然后把start方法运行起来。
2：在WebServe中将server属性创建出来，在构造方法中将server实例化，将端口创建出来
3：在start中利用Socket accept阻塞方法等待浏览器连接，为了使多个客户端能与服务器连接，我们将连接放入while循环
4：为了提高效率我们将获取的输入流放到线程ClientHander中去运行
5：在ClientHander中创建Socket属性，然后再构造方法中实例出来。
6：在RUN方法中处理客户端反送过来的请求，获取输入流，将输入流中的内容读出来
7：为了代码复用，我们将客户端发送过来的请求放到HttpRequest类中去写
8: 在HttpRequest类中我们创建三个属性：请求行、消息头、消息正文。在构造方法中将其实例化
9：将三个属性的get方法创建出来
10：在WebRequest类start中创建HttpRequest对象，将获取的输入流给进去，在HttpRequest类构造方法中传入输入流。
11：解析请求行，创建一个请求行方法把输入流传进去，创建一个StringBuilder的对象，然后去读，将读出来的请求行按照空格
           进行拆分，将拆分出来的三部分分别设置到对应的属性上
12：在WebSERVER的run方法中响应客户端。响应客户端分为三部：向客服端，发送响应状态、响应头、响应正文。
13：首先获取文件，然后判断文件是否存在，如果文件存在，首先发送响应状态
14: 创建输出流，编写状态"HTTP/1.1 200 OK"，将其转换为"ISO8859-1"格式回复给客户端
15：其次发送响应头，编写响应头"Content-Type:text/html"，将其转换为"ISO8859-1"格式回复给客户端，并且将正文长度也回复给客户端
16：最后发送响应正文，利用文件流去读取file文件，然后将其写出来
17：为了代码的复用，我们将响应客户端的代码放到HttpResponse类中去执行
18: 在HttpResponse类中我们创建二个属性：向客户端响应文件的输出流“out”和响应给客户端的文件资源"entity"。
         创建构造方法，将输出流传入进来，在构造方法中将两个属性实例化。
19：将entity的get方法写出来
20: 创建Flush的类去响应客户端
21：创建三个类：状态行信息类、响应头信息、响应正文
22：为了减少代码量我们将三各类都要做的事放到另一个类"println()"中去执行
23：在状态信息类中我们调用println方法将状态恢复回去，在响应头信息中调用println将响应头信息恢复回去，
        在响应正文中了利用文件流读取文件资源entity，然后将其写出
24: 在flush方法中去调用上面三个方法。在WebServer的run方法中创建HttpResponse对象。
25：新建一个Context的类用于定义相关的Http协议的内容
26：在这个类里首先我们创建一个mimeTypeMapping的属性，用于充当相应的介质类型映射和两个静态常量CR\LF.
27: 创建一个init的类作为初始化介质类型映射，然后创建一个静态块，在静态块里去调用init方法，当我们程序运行起来后，
静态块就被自动加载。
28：创建initMimeTypeMapping的类作为初始化介质类型映射，然后在init类里去调用initMimeTypeMapping方法
29：在initMimeTypeMapping这个类里我们创建一个Map集合，集合里添加4个类似"text/html"的后缀
30：改动
	思路：1：首先当客户端访问我们服务端时就会进入WebServer的ClintHander类，进入之后就会进入RUN方法
	2:进入run方法后创建了两个对象：Response和Request。
	3：当Response对象创建出来后，里面有个属性headers,新建了一个Map集合，是空的。
	4：通过uri我们新建了file文件出来，首先判断文件是否存在，如果存在，开始相应客户端
	5：响应分为三部：设置状态行，设置响应头，设置响应正文
	   ①：设置响应头：设置content-Type和contentlength
	   	a：contentType：首先获取文件后缀，然后通过文件后缀找到content-Type的值，
	   	此时我们就要调用HttpContext.getContentTypeByMime(name)方法
	   	去获取。然后进入HttpContext这个类，进入到这个类后，静态块就开始加载，就开始掉init方法，
	   	进入到init类方法后开始调ContentTypeByMime类的方法初始化所有映射
	   	进入到ContentTypeByMime类，我们在Map中放入4个"html", "text/html"这样类型的东西，
	   	此时ContentTypeByMime就初始化好了
	   	然后我门回到run方法，此时我们通过HttpContext去掉getContentTypeByMime方法，他会返回一个值给我们
	   	我们进到这个方法，我门把刚截取出来的文件后缀给进去，他就会把contentType的值返回给我们
	   	b:随后我们回到run方法，开始设置content-Type，我们通过response去调用setContentType(contentType)方法
	   	我们进入到setContentType类，我们把contentType添加到key里，把我们通过后缀拿到的值给到value，
	   	然后我们又调了contentlength()方法，我们进入到setContentType类，我们把contentlength添加到key里,
	   	我们把用户请求文件的长度添加到value中，最后将其返回。
	    c:然后调用setEntity()方法,然后把想要请求的文件设置进去
	   	d:调用flush()方法，然后把响应头中的信息发送给客户端
	   		①：首先，调用sendStatusLine()，进入到sendStatusLine类，我们把状态发过去
	   		②：然后调用sendHeaders();我们进入到sendHeaders类，我们调用的Map的set方法，
	   		通过遍历headers把key和value拼接在一起发送出去
	   		3：最后调用sendContent()，发送正文，设置文件的所有字节
	   		最后把设置好的响应头发送个客户端

32：在HttpResponse类里修改发送响应头类，首先我们调用Map的set方法将Map集合遍历，然后我们将遍历出来的key和value
拼在一起，最后发送每一个消息头
33：解析消息头
代码改动：
我们发现在解析消息头和请求行的时候都会用到按行读取字符串，所以我们将他提到一个方法中单独去做。
创建一个redLing方法去读取字符串
34：在HttpResponse类里我们添加一个headers的属性并以Map集合将其初始化，作用是包含所有响应头信息
35：创建一个parseHeaders的类然后将输入流引入
36：在这个类里使用while循环去读输入流，首先判断是否读到了空字符串，如果不是我们去从查找读出来的内容是否包含":"
        如果包含我们按照“：”，将左边的类容截取出来赋值给(key)name,将右边的内容截取出来赋值给value
        然后调用Map集合的Set方法把截取出来的内容通过遍历添加到Map集合中。
37 解析Uri
37 将他提到方法里面单独去做
38 首先创建一个param的属性并初始化
39 在方法里面先判断uri里是否包含“？”，如果不包含我们将uri的内容赋值到requestLine属性上
40 如果包含：第一步，我们将uri里的内容从开始截取到“？”处，然后赋值给requestLine属性
41 第二步，我们将uri里的内容从“？”下一位开始截取到末尾，然后赋值给自己创建的变量queryStr
42 第三步，我们将queryStr里的内容按照“&”进行拆分得到一个数组，然后进行遍历
43 第四部，我们将遍历后的内容再次按照“=”进行拆分，然后加到map集合里
44 完成注册功能：
45 用户再发送请求时不一定是网页资源，还可能是某些功能，下面我们就以注册为实例写
46 首先我们进行判断用户发过来的是资源还是功能，如果是资源我们按以上步骤返回给客户
47 如果是功能，首先我们新建一个页面，把用户输入的注册信息设置好，然后回到WebSerer类
48 然后获取用户输入的注册信息，建立一个文件流用于存放用户注册的信息，在文件流上街上转换流和缓冲流将其写出
49 最后在finally里将转换流关闭
50 代码改动：
51 修改HttpContext类，将初始化介质映射的方法修改
52 在项目目录中添加conf目录，并在里面添加web.xml文件（该文件直接使用tomcat根目录中
   conf目录中的这个文件）
53 进入HttpContext类，进入mimeTypeMapping类，我们创建SAXReader字节流去读文件，再读的过程中用文件流去读conf文件下的web.xml文档
54 返回一个Document的实例，然后我们通过这个实例去获取根元素（跟标签），返回一个Element的实例，通过Element提供的方法获取所有与mime-mapping
55 同名的子标签，最后我们遍历所有子标签，将web.xml文件中mime-mapping标签下的extendinos作为key值加入mimeTypeMapping集合，将web.xml文件中mime-type
56 作为value存入mimeTypeMapping集合中

