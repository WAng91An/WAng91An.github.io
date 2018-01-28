---
img: how-to-start.jpg # Add image post (optional)
---

为了能让Web服务器与Web应用这两个不同的软件系统协作，需要一套标准接口，Servlet就是其中最主要的一个接口,SUN公司（现在被Oracle收购了……）制定了Web应用于Web服务器进行协作的一系列标准Java接口（统称为Java Servlet API）。

### 关于servlet的姐扫

servelt，servlet容器的概念，以及生命周期


### Servlet是什么

- 为了能让Web服务器与Web应用这两个不同的软件系统协作，需要一套标准接口，Servlet就是其中最主要的一个接口。

- 规定：

 - Web服务器可以访问任意一个Web应用中实现Servlet接口的类。

 - Web应用中用于被Web服务器动态调用的程序代码位于Servlet接口的实现类中。

 - SUN公司（现在被Oracle收购了……）制定了Web应用于Web服务器进行协作的一系列标准Java接口（统称为Java Servlet API）。

 - SUN公司还对Web服务器发布及运行Web应用的一些细节做了规约。SUN公司把这一系列标准Java接口和规约统称为Servlet规范。

 - Servlet是一种运行在服务器上的小插件。

### Servlet容器是什么

- 在Servlet规范中，把能够发布和运行JavaWeb应用的Web服务器称为Servlet容器，他的最主要特称是动态执行JavaWeb应用中的Servlet实现类中的程序代码。

### Tomcat是什么

- Tomcat是Servlet容器，同时也是轻量级的Web服务器。这是它的两个身份！

- Apache Server、Microsoft IIS、Apache Tomcat都是Web服务器。

- Tomcat作为Web服务器时，主要负责实现HTTP传输等工作。

- Tomcat作为Servlet容器时，主要负责解析Request，生成ServletRequest、ServletResponse，将其传给相应的Servlet（调用service( )方法），再将Servlet的相应结果返回。

### 热键

- Ctrl O 找函数名

- 按着Ctrl + 鼠标左键 (看源码)

- Ctrl + shift +R (打开资源)

- Ctrl + F11 (运行)

- F4 查看函数

### doGet doPost 

- 和协议相关

### Servlet生命周期

- 修改Java文件要再一次部署

 - - 间隔扫描
 - - 热部署？


### servletcontext
- servletcontext context = new servletcontext();
- context.setAttribute();公用一个作用域
	

- 修改Servletload


- load-on-startup 服务器加载调用 xml

- publish重新部署，新改过的文件发布到服务器

- wtpwebapps （eclipse提供的web插件）

- server.xml

### 绑定服务器

- servlet 出现HttpServlet 报错

 - - 需要绑定服务器  Build path -> Libraries 

- 属性->Project Facets->勾选动态web java->出现Deployment Descriptor 

## servlet 生命周期？

 所谓生命周期，指的是servlet容器如何创建servlet实例、分配其资源、调用其方法、并销毁其实例的整个过程。

 1.  实例化（就是创建servlet对象,调用构造器）

 在如下两种情况下会进行对象实例化。

  第一种情况：

当请求到达容器时，容器查找该servlet对象是否存在，如果不存在，才会创建实例。

第二种情况：

容器在启动时，或者新部署了某个应用时，会检查web.xml当中，servlet是否有 load-on-starup配置。如果有，则会创建该servlet实例。

load-on-starup参数值越小，优先级越高（最小值为0，优先级最高）。

 2.  初始化

- 为servlet分配资源，调用init(ServletConfig config);方法

- config对象可以用来访问servlet的初始化参数。

- 初始化参数是使用init-param配置的参数。

- init可以override。

 3.  就绪/调用

- 有请求到达容器，容器调用servlet对象的service()方法。

- HttpServlet的service()方法，会依据请求方式来调用doGet()或者doPost()方法。但是，这两个do方法默认情况下，会抛出异常，需要子类去override。

 4.  销毁

容器依据自身的算法，将不再需要的servlet对象删除掉。

在删除之前，会调用servlet对象的destroy()方法。

destroy()方法用于释放资源。

在servlet的整个生命周期当中，init,destroy只会执行一次，而service方法会执行多次。



