# mybatis 学习笔记

### 分页插件的使用步骤

1.引入pageHelper
```
<dependency>
      <groupId>com.github.pagehelper</groupId>
      <artifactId>pagehelper</artifactId>
      <version>5.0.0</version>
</dependency>
```
2.mybatis-config注册
```
 <plugins>
        <plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
</plugins>
```
3.查询之前只需要调用,传入页码，每页的大小
```
PageHelper.startPage(pn,5);
```
4.startPage后面紧跟的查询是一个分页查询
```
List<Employee> employeeList = iEmployeeService.getAll();
```
5.使用pageInfo包装查询后的结果
```
PageInfo pageInfo = new PageInfo(employeeList);
```
