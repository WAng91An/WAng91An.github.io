---

layout: post

title:  how to solve  url request encoding  problems

date: 2016-09-12 00:00:00 +0300

description:  url请求中文乱码问题  # Add post description (optional)

img: how-to-start.jpg # Add image post (optional)

tags: [SSM] # add tag

---


We may have trouble with the code, and we can try to solve the problem by using the following method.



### 解决乱码问题



比如我们请求夏目按的接口，但是传递的参数有中文可能导致写入数据库乱码，这样应该怎么解决呢？









```

    http://localhost:8080/manage/category/add_category.do?parentId=0&categoryName=家庭用品



```



1. 请求上面一的url时候，写入数据库时乱码的，数据库编码设置成utf-8,打开my.ini 方法：



    ```

    [client]



    default_character_set = utf8



    [mysqld]



    port= 3306



    character-set-server = utf8

    collation-server = utf8_general_ci

    [mysql]

    default_character_set = utf8

    ```



2. 配置过滤器



```

<filter>  

      <filter-name>encodingFilter</filter-name>  

      <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>  

      <init-param>  

        <param-name>encoding</param-name>  

        <param-value>UTF-8</param-value>  

      </init-param>  

      <init-param>  

        <param-name>forceEncoding</param-name>  

        <param-value>true</param-value>  

      </init-param>  

    </filter>  

    <filter-mapping>  

      <filter-name>encodingFilter</filter-name>  

      <url-pattern>/*</url-pattern>  

    </filter-mapping>  

```



3. 修改tomcat的编码！！



```

<Connector connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443" URIEncoding="UTF-8"/>



```









