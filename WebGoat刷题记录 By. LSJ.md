# General

## HTTP Basics

### 2. Try It!

通过浏览器的开发者工具或代理抓包软件观察在输入框输入“name ”后点击 “Go” 按钮发出的POST请求与收到的响应。

![1.1](/General/HTTP%20Basics/1.1.png)

![1](/General/HTTP%20Basics/1.png)

![2-请求](/General/HTTP%20Basics/2-请求.png)

![3-响应](/General/HTTP%20Basics/3-响应.png)

### 3. The Quiz

通过上一步知道使用了 POST，而从之前的请求和响应中无法找到 magic number 相关信息，我们直接点 Go。

![4](/General/HTTP%20Basics/4.png)

![1.2](/General/HTTP%20Basics/1.2.png)

![5-请求](/General/HTTP%20Basics/5-请求.png)

观察到请求中的 Body 部分存在 magic_num ，填入即通关

## HTTP Proxies

### 6. Intercept and modify a request

按要求先点击 Submit 按钮，查看拦截到的 POST 请求

![1](General/HTTP%20Proxies/1.png)

按要求对其进行修改，将 POST 改为 GET，将 Body 中的参数删去，在URL后添加 Query string parameter， 为```?changeMe=Requests+are+tampered+easily```， 并在头部添加字段 ```X-Request-Intercepted: True```，发送后即可通关

![2](General/HTTP%20Proxies/2.png)

![3](General/HTTP%20Proxies/3.png)

## Developer Tools

### 4. Try It! Using the console

按要求在控制台输入命令即可看到结果

![1](General\Developer Tools\1.png)

![2](General\Developer Tools\2.png)

### 6. Try It! Working with the Network tab

点击”Go“按钮，在 ”网络“ 标签下监测，查看名为 network 的 POST请求，在 Payload 中即可找到 networkNum

![3](General\Developer Tools\3.png)

![4](General\Developer Tools\4.png)

## Crypto Basiscs

### 2. Base64 Encoding

通过 Base64 解码，即可获得用户名和密码

![1](General\Crypto Basic\1.png)

![2](General\Crypto Basic\2.png)

### 3. Other Encoding

在 Google 搜索 WebSphere Encoder 找到对应加解密网页，输入即可得到解密内容

![4](General\Crypto Basic\4.png)

![3](General\Crypto Basic\3.png)

### 4. Plain Hashing

根据位数推测两个哈希值的算法，可知上面为 MD5，下面为 SHA256，通过尝试几个弱口令比对其哈希值即可得出答案

![5](General\Crypto Basic\5.png)

![6](General\Crypto Basic\6.png)

### 6. Signatures

这题是遇到的第一道有一定难度的题，需要掌握 openssl 的相关操作后才有做对这题的基础

![7](General\Crypto Basic\7.png)

首先，我们对于给出的以“-----BEGIN PRIVATE KEY-----”开头，以“-----END PRIVATE KEY-----”结尾的私钥要有一个明确的认识，这是 PEM 格式的密钥，相关知识自行查询了解

首先，我们将给出的私钥保存到文件“test.key”中，注意，包含头部和尾部。

![8](General\Crypto Basic\8.png)

使用命令

```bash
openssl rsa -modulus -in test.key -out test.modulus -noout
```

将模数打印到文件“test.modulus”中

![9](General\Crypto Basic\9.png)

将文件中开头的"Modulus="和结尾的回车删除，只保留模数的数值

![10](General\Crypto Basic\10.png)

使用命令

```bash
openssl dgst -sign test.key -sha256 -out modulus.signature test.modulus
```

将模数经私钥签名后存入文件“modulus.signature”

可以看到签名后的结果无法正常显示出来

![11](General\Crypto Basic\11.png)

使用命令

```bash
openssl enc -base64 -in modulus.signature -out signature.base64
```

将签名后的结果进行 base64 编码存入文件“signature.base64”

![12](General\Crypto Basic\12.png)

将模数和签名后经过 base64 编码后的结果填入题目空格处即可完成

### 8. Assignment

本题可以说是绪论部分的压轴题，有相当的难度

![13](General\Crypto Basic\13.png)

首先使用给定指令安装实验 docker 容器

![14](General\Crypto Basic\14.png)

通过命令

```bash
sudo docker exec -it <Container ID> bash 
```

打开容器的 bash 终端

尝试打开 /root 提示权限不够

查看配置文件夹 /etc 中“passwd”和"shadow"的权限，发现“passwd”对普通用户只读，“shadow”只对”root“用户可读写

![15](General\Crypto Basic\15.png)

鉴于容器内不具备获取 root 权限的条件，我们退出容器，使用命令

```bash
sudo docker cp <Container ID>:/etc/shadow passwd
```

将容器中的"passwd"文件拷出

![17](General\Crypto Basic\17.png)

接下来，我们试图将手动计算出的一个密码对应的哈希值写入“passwd”中，并将其拷回覆盖掉原有”passwd“实现容器中 root 身份的登录

首先，通过如下命令

```bash
mkpasswd -m sha-512 password
```

生成一段密码“password”对应的哈希值

将这段哈希值拷入“passwd”文件中，替换掉"root:"字段后的字母“x”，保存退出

![18](General\Crypto Basic\18.png)

输入命令

```bash
sudo docker cp passwd <Container ID>:/etc/passwd
```

将修改后的“passwd”拷回容器，覆盖掉原有文件

重新进入容器打开 bash，查看确认“passwd”文件修改情况

![19](General\Crypto Basic\19.png)

现在，输入命令```su```，输入密码```password```，即可获取 root 身份

![20](General\Crypto Basic\20.png)

接下来，进入 /root 目录，可以看到文件“default_secret”，查看其内容即可获取秘密

![21](General\Crypto Basic\21.png)

最后，根据给出的命令，在结尾加上密钥即可得到最终的信息

![22](General\Crypto Basic\22.png)

## Writing new lesson

### 6. Add an assignment to your lesson

本题考查对 Java 和 HTML 源码的基础阅读能力

![1](General\Writing new lesson\1.png)

通过阅读 HTML 源码，我们可以看到，任务中输入的两个参数被分别存入“param1”和“param2”两个变量中，并随表单传回后端服务器

![2](General\Writing new lesson\2.png)

在后端函数中，我们可以看到，"SampleAttack"类中定义了一个常量“secretValue”，并在其中的函数“completed”中，将"param1"与“secretValue”比较，相同则任务完成，因此，将任务中的“parameter 1”中填入"secr37Value"，"parameter 2"不填或填入任意值即可通过

# (A1) Injection

## SQL Injection (intro)

### 2. It is your turn!

![1]((A1) Injection\SQL Injection (intro)\1.png)

根据页面所给的数据表，以及题目中的要求，可以写出下列语句

```sql
SELECT department FROM employees WHERE first_name='Bob' AND last_name='Franco';
```

![2]((A1) Injection\SQL Injection (intro)\2.png)

### 3. It is your turn!

![3]((A1) Injection\SQL Injection (intro)\3.png)

根据题目要求，写出下列语句

```sql
UPDATE employees SET department='Sales' WHERE first_name='Tobi' AND last_name='Barnett';
```

![4]((A1) Injection\SQL Injection (intro)\4.png)

### 4. Data Definition Language (DDL)

![5]((A1) Injection\SQL Injection (intro)\5.png)

根据题目要求，写出下列语句

```sql
ALTER TABLE employees ADD phone varchar(20)
```

![6]((A1) Injection\SQL Injection (intro)\6.png)

### 5. Data Control Language (DCL)

![7]((A1) Injection\SQL Injection (intro)\7.png)

根据题目要求，写出下列语句

```sql
GRANT ALTER TABLE TO UnauthorizedUser
```

![8]((A1) Injection\SQL Injection (intro)\8.png)

### 9. Try It! String SQL injection

![9]((A1) Injection\SQL Injection (intro)\9.png)

根据上文讲解，选择第一项为```Smith'```，第二项为```or```，第三项为```'1' = '1```，即可完成注入

![10]((A1) Injection\SQL Injection (intro)\10.png)

### 10. Try It! Numeric SQL injection

![11]((A1) Injection\SQL Injection (intro)\11.png)

本题需要注意一点，关于 NOT, OR 和 AND 的优先级关系，如下

```NOT``` > ```AND``` > ```OR```

因此，若 SQL 语句构造如下

```sql
SELECT * From user_data WHERE Login_Count = 1 OR 1 = 1 AND userid= 1
```

则优先运算```1 = 1 AND userid= 1```得到 FALSE，计算```Login_Count = 1```得到 FALSE，后计算```FALSE OR FALSE```得到 FALSE，因此注入失败

能够成功注入的正确的 SQL 语句如下

```sql
SELECT * From user_data WHERE Login_Count = 1 AND userid= 1 OR 1 = 1
```

优先运算```Login_Count = 1 AND userid= 1```得到 FALSE，计算```1 = 1```得到 TRUE，后计算```FALSE OR TRUE```得到 TRUE，因此注入成功

![12]((A1) Injection\SQL Injection (intro)\12.png)

### 11. It is your turn!

![13]((A1) Injection\SQL Injection (intro)\13.png)

如图在第一空填入任意值，第二空填入 ```任意值 + ' OR '1' = '1```即可成功注入

![14]((A1) Injection\SQL Injection (intro)\14.png)

### 12. It is your turn!

![15]((A1) Injection\SQL Injection (intro)\15.png)

根据题目要求中所描述的，我们需要将 John 的工资调整为员工中最高的，通过查看之前的表格，可以看到，将 工资Salary 设置为 90000 就可以使 John 的工资位列所有雇员之首，因此我们在第一空填写任意内容，第二空填写```'; UPDATE employees SET SALARY=90000 WHERE first_name='John```即可完成注入

![16]((A1) Injection\SQL Injection (intro)\16.png)

### 13. It is your turn!

![17]((A1) Injection\SQL Injection (intro)\17.png)

首先，猜测本题中所给输入框跟前面几道题一样，会被填写回 SQL 语句中 SELECT 的 WHERE 部分，选择不同的输入内容进行尝试，观察其返回内容，可以印证猜测

因此，我们用```'```把前面的单引号闭合，用```--```把后面的单引号注释掉，然后中间写题中要求的语句即可完成

```sql
'; DROP TABLE access_log; --
```

![18]((A1) Injection\SQL Injection (intro)\18.png)



## SQL Injection (advanced)

### 3. Try It! Pulling data from other tables

获取表 user_system_data 有两种方法

第一种使用 UNION，在输入框中填入如下语句

```sql
' UNION SELECT userid, user_name, password, cookie, 'test1', '2', 3 FROM user_system_data;--
```

结果如图：

![1]((A1) Injection\SQL Injection (advanced)\1.png)

需要注意两点：

1. UNION 连接前后两个查询结果，表头数目须保持一致，且数据类型也需要一一对应相同，其中varchar无需考虑括号内长度。
2. SELECT语句中，为了达到格式匹配的目的，在可以使用数字或字符串代替表头名称占位

第二种使用独立SQL语句，在输入框中填入如下语句

```sql
';SELECT * FROM user_system_data;--
```

结果如图：

![2]((A1) Injection\SQL Injection (advanced)\2.png)

将所得密码填入其中，即可过关

### 5.

本题模拟真实场景，对SQL注入基础能力进行综合考察。本题可直接采用sqlmap进行扫描注入，此处我们借助ZAP或Burpsuite进行半手工注入。

首先测试登陆界面，用户名输入```' or '1'='1```，密码任意，点击登录，提示不匹配，推测此处可能不存在注入点，暂时跳过。

![3]((A1) Injection\SQL Injection (advanced)\3.png)

接下来测试注册页面，尝试以用户名"tom"进行注册，提示该用户已注册，符合预期。

![4]((A1) Injection\SQL Injection (advanced)\4.png)

接着在用户名处输入```tom' or '1'='1```和```tom' or '1'='2```，均返回如下提示

![5]((A1) Injection\SQL Injection (advanced)\5.png)

再在用户名处输入```abc' or '1'='2```，返回如下提示

![6]((A1) Injection\SQL Injection (advanced)\6.png)

再在用户名处输入```tom' or '1'='2```，返回如下提示

![6.5]((A1) Injection\SQL Injection (advanced)\6.5.png)

综上，推测此处存在注入点

接下来寻找存储密码的表头，观察注册时发送的PUT请求，猜测密码表头可能为“password”

![7]((A1) Injection\SQL Injection (advanced)\7.png)

构造Payload：```tom' AND length(password)>0 --```，如果字段存在，则 length(password) 的值为 tom 的 password 的长度，实际得到如下返回：

![5]((A1) Injection\SQL Injection (advanced)\5.png)

说明猜测正确，存储密码的表头名为 password

接下来通过布尔盲注得到 tom 的 password，此处以 ZAP 作为辅助工具。

构造如下 Payload ：```tom' AND substr(password,1,1)='t```，使用 ZAP 拦截 PUT 请求，在数据包内容界面单击鼠标右键选择“Fuzz”进入暴破模式

![8]((A1) Injection\SQL Injection (advanced)\8.png)

删除右侧已有的暴破位置，选中左侧 Body 部分中的字母 “t”，点击右边的 “Add” 按键将其作为暴破变量，在弹出的窗口中点击 “Add” ，类型选择为 “File Fuzzers” ，具体选择其下的 “jbrofuzz -> Alphabets -> ASCII 85 Alphabet” ，点击右下角添加，再点击确定，完成暴破位置创建，可以看到，创建好的暴破位置已被对应着色。

![9]((A1) Injection\SQL Injection (advanced)\9.png)

![10]((A1) Injection\SQL Injection (advanced)\10.png)

点击 “选项” ，将 “Delay” 一栏设为 200ms ，避免请求过快出现异常。

![11]((A1) Injection\SQL Injection (advanced)\11.png)

点击右下角 “Start Fuzzer” 开始暴破，暴破结束后，点击表头中的 “Size Resp. Body” 使其按照返回数据包的 Body 长度进行升序排列，可以看到其中 Payload 为字符 “t” 的表项 Body 大小明显与其他不同，因此 password 的第一个字符为 “t”

![12]((A1) Injection\SQL Injection (advanced)\12.png)

同理，依次改变数据包中 substr(password,2,1) 函数参数，分别进行暴破，即可得到完整密码。

如果希望提前获得password的长度，可通过修改 Payload 中函数为 length(password)=1 ，将 1 的位置设置为暴破位置，暴破字典选择为0到100的数字，即可暴破得出 password 的长度为23位

最终得出的密码为：“thisisasecretfortomonly”

## SQL Injection (mitigation)

### 5. Try it! Writing safe code

如图所示填入即可通过

![1]((A1) Injection\SQL Injection (mitigation)\1.png)

### 6. Try it! Writing safe code

给出一个示例

```java
String userName = "peter";
try {
    Connection conn = DriverManager.getConnection(DBURL, DBUSER, DBPW);
    PreparedStatement ps = conn.prepareStatement("SELECT * FROM users WHERE name = ?");
    ps.setString(1, userName);
    ResultSet results = ps.executeQuery();
    System.out.println(results.next());
} catch (Exception e) {
    System.out.println("Oops, Something went wrong!");
}
```

![2]((A1) Injection\SQL Injection (mitigation)\2.png)

### 9. Input validation alone is not enough!!

本题承接自 SQL Injection (advanced) 部分的第三节。

首先尝试原 Payload ：```';SELECT * FROM user_system_data;--```，返回如下错误信息，判断系统加入了空格过滤

![3]((A1) Injection\SQL Injection (mitigation)\3.png)

因此考虑对应的绕过手段，这里我选择使用注释符/**/代替空格尝试绕过

将上述 Payload 改为```';SELECT/**/*/**/FROM/**/user_system_data;--```，即可成功绕过完成注入

![4]((A1) Injection\SQL Injection (mitigation)\4.png)

### 10. Input validation alone is not enough!!

首先尝试将上一题的 Payload 带入测试，得到如下错误回显

![5]((A1) Injection\SQL Injection (mitigation)\5.png)

可以观察到，在实际执行的语句中，关键词SELECT和FROM均被移除，判断其进行了关键字过滤，我们可以使用针对手段对其绕过，这里选择使用双关键字法

构造 Payload：```';SELselectECT/**/*/**/FRfromOM/**/user_system_data;--```，即可成功绕过完成注入

![6]((A1) Injection\SQL Injection (mitigation)\6.png)

### 12.

题目中提示本题需要根据排序部分（ORDER BY）进行注入，因此，我们尝试点击不同字段的排序按钮，使用抓包工具监测数据包，这里以 ZAP 为例

![7]((A1) Injection\SQL Injection (mitigation)\7.png)

可以看到，发出的请求带有 Payload：```column=ip```，因此，我们尝试构造新的 Payload：```column=(CASE WHEN (TRUE) THEN ip ELSE hostname END)```，测试注入可行性（注意，前页教学中所给语法有误，完整准确的 CASE 结构为：CASE WHEN THEN ELSE END）

![8]((A1) Injection\SQL Injection (mitigation)\8.png)

发送后得到如下响应，观察到返回的表项是根据 ip 顺序进行排序的，因此做出判断，此处确实存在注入点

![9]((A1) Injection\SQL Injection (mitigation)\9.png)

在进行最后的注入之前，我们还需要确定表名，这里我们直接选取一个错误的 Payload ，观察其返回的报错信息即可得知表名为 “servers”

![10]((A1) Injection\SQL Injection (mitigation)\10.png)

![11]((A1) Injection\SQL Injection (mitigation)\11.png)

之后构造 Payload：```column=(CASE WHEN (SELECT substr(ip,1,1)='1' FROM servers WHERE hostname='webgoat-prd') THEN ip ELSE hostname END)```，对 ip 地址的前三个位置分别使用数字字典进行暴破

![12]((A1) Injection\SQL Injection (mitigation)\12.png)

结果如图所示，找到三个位置的所有响应中唯一以 IP 顺序排列表项的那个数据包，对应的数字即为暴破结果

![13]((A1) Injection\SQL Injection (mitigation)\13.png)

因此，webgoat-prd 对应的 IP 为 “104.130.219.202”

## Path traversal

### 2. Path traversal while uploading files

本题未设置任何防御措施

首先点击头像，随意选择一个文件后，下面参数默认，提交，查看结果

![1]((A1) Injection\Path traversal\1.png)

发现所上传的文件被存放在了个人用户目录下的“test”文件夹中

将“Full Name”字段改为“abc”，再次尝试上传

![2]((A1) Injection\Path traversal\2.png)

发现上传的文件被存放在了个人用户目录下的“abc”文件夹中，据此推断，文件存放路径与“Full Name”字段直接相关

将 Payload 改为```../test```，再次上传，即可通关

![3]((A1) Injection\Path traversal\3.png)

### 3. Path traversal while uploading files

首先尝试上题 Payload：```../test```

![4]((A1) Injection\Path traversal\4.png)

发现文件未被上传至指定位置，结合题目描述可以得出，本题会将发现的“../”删去，因此考虑构造双关键字，输入新的 Payload：```....//test```，即可通关

![5]((A1) Injection\Path traversal\5.png)

### 4. Path traversal while uploading files

阅读题面，得知整个“Full Name”字段将会被仔细检查，经测试也可发现，之前的 Payload 均无法生效

观察默认参数的提交结果，文件被直接存放在了用户目录下，与 Full Name 彻底无关

![6]((A1) Injection\Path traversal\6.png)

但由此，我们可以猜想其路径是否与文件名直接相关，由于文件名中直接输入“../”会提示非法字符，因此我们通过抓包，将文件名称前手动添加```../```，重新发送，即可通关

![7]((A1) Injection\Path traversal\7.png)

### 5. Retrieving other files with a path traversal

点击按钮可以随即展示一张图片，进行抓包后发现，在其响应中，显示出了```?id=7.jpg```这一参数

![8]((A1) Injection\Path traversal\8.png)

![9]((A1) Injection\Path traversal\9.png)

我们尝试重发请求，在GET请求尾部添加```?id=7.jpg```，观察其响应，发现响应 404，查看路径字段，发现其中中多出一个“.jpg”，判断出 id 字段的参数无需添加 ".jpg"

![10]((A1) Injection\Path traversal\10.png)

删去请求中的“.jpg”，再次发送，可以看到正常响应，说明我们可以通过 id 字段控制随机脚本

![11]((A1) Injection\Path traversal\11.png)

我们现在尝试直接将题中要求找到的图片名作为参数填入 id 字段，提示请求错误，说明图片不存在

![12]((A1) Injection\Path traversal\12.png)

由于本题考查路径遍历，我们尝试去上层目录查询，在图片名前添加“../”组成新的 Payload：```../path-traversal-secret```，发送请求后查看其响应，发现其提示参数中存在非法字符

![13]((A1) Injection\Path traversal\13.png)

考虑将“../”转换为“%2e%2e%2f”，再次发送请求，错误提示消失，但图片仍不存在

![14]((A1) Injection\Path traversal\14.png)

继续向上级目录查找，提交请求后发现成功获得响应

![15]((A1) Injection\Path traversal\15.png)

根据相应内容，我们可以看到，本题的最终答案，就是将登录用的 username 进行 SHA-512 取哈希，计算后填入答案框中即可通关

![16]((A1) Injection\Path traversal\16.png)

