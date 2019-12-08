# Git/RESTful API/命令行

## Git

#### Git 常用命令
- ```git clone```
- ```git remote add origin```
- ```git push -u origin master```
- ```git log```
- ```git status```
- ```git diff```
- ```git add *```
- ```git commit -m "message"```
- ```git push```
- ```git pull```

#### Git 撤销与回滚
- **暂存区**：```git add```之后commit之前存在的区域；**工作区**：```git commit```之后存在的区域；**远程仓库**：```git push```之后；
- 作了修改，但还没```git add```，撤销到上一次提交：```git checkout --filename```；```git checkout --.```
- 作了修改，并且已经```git add```，但还没```git commit```：
    - 先将暂存区的修改撤销：```git reset HEAD filename```/```git reset HEAD```；此时修改只存在于工作区，变为了 "unstaged changes"；
    - 再利用上面的checkout命令从工作区撤销修改
- ```git add```之后，作了修改，想丢弃这次修改：```git checkout --filename```会回到最近一次```git add```
- 作了修改，并且已经```git commit```了，想撤销这次的修改：
    - ```git revert commitID```. 其实，```git revert```可以用来撤销任意一次的修改，不一定要是最近一次
    - ```git reset --hard commitID```/```git reset --hard HEAD^```（HEAD表示当前版本，几个^表示倒数第几个版本，倒数第100个版本可以用HEAD~100）；参数```--hard```：强制将暂存区和工作区都同步到指定的版本
    - ```git reset```和```git revert```的区别是：reset是用来回滚的，将HEAD的指针指向了想要回滚的版本，作为最新的版本，而后面的版本也都没有了；而revert只是用来撤销某一次更改，对之后的更改并没有影响
    - 然后再用```git push -f```提交到远程仓库

#### Git 分支管理

## RESTful API
REST指Representational State Transfer，可以翻译为“表现层状态转化”

#### 主要思想
- 对网络上的所有资源，都有一个**统一资源标识符** URI(Uniform Resource Identifier)；
- 这些资源可以有多种表现形式，即REST中的“表现层”Representation，比如，文本可以用txt格式表现，也可以用HTML格式、XML格式、JSON格式表现。URI只代表资源的实体，不代表它的形式；
- “无状态(Stateless)”思想：服务端不应该保存客户端状态，只需要处理当前的请求，不需了解请求的历史，客户端每一次请求中包含处理该请求所需的一切信息；
- 客户端使用HTTP协议中的 GET/POST/PUT/DELETE 方法对服务器的资源进行操作，即REST中的”状态转化“

#### 设计原则
- URL设计
    - 最好只使用名词，而使用 GET/POST/PUT/DELETE 方法的不同表示不同的操作；比如使用```POST /user```代替```/user/create```
    - GET：获取资源；POST：新建/更新资源；PUT：更新资源；DELETE：删除资源；
    - 对于只支持GET/POST的客户端，使用```X-HTTP-Method-Override```属性，覆盖POST方法；
    - 避免多级URL，比如使用```GET /authors/12?categories=2```代替```GET /authors/12/categories/2```；
    - 避免在URI中带上版本号。不同的版本，可以理解成同一种资源的不同表现形式，所以应该采用同一个URI，版本号可以在HTTP请求头信息的Accept字段中进行区分
- 状态码：服务器应该返回尽可能精确的状态码，客户端只需查看状态码，就可以判断出发生了什么情况。见计算机网络部分 -- [HTTP请求有哪些常见状态码？](Computer%20Network.md#HTTP请求有哪些常见状态码)
- 服务器回应：在响应中放上其它API的链接，方便用户寻找

### 参考
- [Git教程 - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/896043488029600)
- [RESTful API 最佳实践 - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2018/10/restful-api-best-practices.html)
- [GitHub - jlevy/the-art-of-command-line: Master the command line, in one page](https://github.com/jlevy/the-art-of-command-line/blob/master/README-zh.md)