# SQL注入

<!-- TOC -->

- [SQL注入](#sql注入)
    - [SQL基本概念](#sql基本概念)
    - [SQL注入基本概念](#sql注入基本概念)
    - [SQL注入能做到的事情](#sql注入能做到的事情)
    - [SQL注入的后果](#sql注入的后果)
    - [SQL注入的例子](#sql注入的例子)
        - [应用中动态查询](#应用中动态查询)
        - [攻击者提供的非预期输入](#攻击者提供的非预期输入)
        - [应用执行的查询](#应用执行的查询)
    - [动手实践](#动手实践)
    - [如何缓解SQL注入](#如何缓解sql注入)
    - [输入验证](#输入验证)
    - [补充](#补充)

<!-- /TOC -->

## SQL基本概念

- 结构化查询语言（Structured Query Language，SQL）
  - 不是“Standard Query Language”
- 数据处理语言（Data Manipulation Language，DML）
  - SELECT, INSERT, UPDATE, DELETE, …
- 数据定义语言（Data Definition Language，DDL）
  - CREATE, ALTER, DROP,TRUNCATE,…  
- 数据控制语言（Data Control Language，DCL）
  - GRANT, REVOKE, …

## SQL注入基本概念

- SQL注入攻击一般通过从客户端到应用程序的SQL查询输入插入或“注入”恶意数据
- SQL注入的**本质**：**把用户输入的数据当做代码执行**
  - **1.用户能够控制输入**
  - **2.原本程序要执行的代码，拼接了用户输入的数据**

## SQL注入能做到的事情

- **从数据库中读取和修改敏感数据**
- 对数据库执行管理操作
- 关闭审核或DBMS
- 截断表和日志
- 添加用户
- 恢复DBMS文件系统上存在的给定文件的内容
- **向操作系统发出命令**

## SQL注入的后果

SQL注入攻击允许攻击者：

- 欺骗身份
- 篡改现有数据
- 导致拒绝问题，例如取消交易或更改余额
- 允许完整披露系统上的所有数据
- 销毁数据或使其无法使用
- 成为数据库服务器的管理员

## SQL注入的例子

### 应用中动态查询

- 潜在的字符串注入

    ```SQL
    "select * from users where name = '" + userName + "'";
    ```

- 潜在的数字型注入

    ```SQL
    "select * from users where employee_id = "  + userID;
    ```

### 攻击者提供的非预期输入

- userName = Smith' or '1'='1
- userName =' or 1=1 --
- userID = 1234567 or 1=1
- UserName = Smith’;drop table users; truncate audit_log;--

### 应用执行的查询

- select * from users where name = 'Smith' or '1' = '1'
  - select * from users where name = 'Smith' or TRUE
- select * from users where employee_id = 1234567 or 1=1

## 动手实践

- 字符型注入
[WebGoat String SQL Injection](http://192.168.11.3:8080/WebGoat/start.mvc#lesson/SqlInjection.lesson/6)
  - Smith' or 1=1 --
- 数字型注入
[WebGoat Numeric SQL Injection](http://192.168.11.3:8080/WebGoat/start.mvc#lesson/SqlInjection.lesson/7)
  - 101 or 1=1 --

## 如何缓解SQL注入

- 静态查询

    ```SQL
    select * from products;
    ```

    ```SQL
    select * from users where user = "'" + session.getAttribute("UserID") + "'";
    ```

- 参数化查询/预编译SQL语句

    ```SQL
    String query = "SELECT * FROM users WHERE last_name = ?";
    PreparedStatement statement = connection.prepareStatement(query);
    statement.setString(1, accountName);
    ResultSet results = statement.executeQuery();
    ```

- 存储过程（仅当存储过程不生成动态SQL时）
  - 安全存储过程

    ```SQL
    CREATE PROCEDURE ListCustomers(@Country nvarchar(30))
    AS
    SELECT City, COUNT(*)
    FROM Customers
    WHERE Country LIKE @Country GROUP BY City

    EXEC ListCustomers ‘USA’
    ```

  - 可注入存储过程

    ```SQL
    CREATE PROEDURE getUser(@lastName nvarchar(25))
    AS
    declare @sql nvarchar(255)
    set @sql = 'select * from users where
                LastName = + @LastName + '
    exec sp_executesql @sql
    ```

## 输入验证

由于我的查询不再可注射，我还需要验证我的输入吗？

是的！

防止其他类型的攻击存储在数据库中

- 存储XSS
- 信息泄漏
- 逻辑错误 - 业务规则验证
- SQL注入

通常，数据库被认为是可信的

## 补充

预编译的SQL语句总能够防止SQL注入么？

不，不是的。

一些字符及问题可能会导致预编译的SQL语句被绕过，所以应该**统一数据库、操作系统、Web应用所使用的字符集，以避免各层对字符的理解存在差异**。
