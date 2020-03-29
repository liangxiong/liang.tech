# restful指南

### 规划

#### 域名

- 尽量放到新域名下：https://api.example.com

- 通过contextPath区别： https://example.org/api/



#### 版本

- 放在path中

- 放在 http head中



#### 路径 endpoint

- 资源 ， 可对应数据库的一种表名





#### 动作

- GET（SELECT：从服务器取出资源（一项或多项）。
- POST（CREATE）：在服务器新建一个资源。

- PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。

- PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）。

- DELETE（DELETE）：从服务器删除资源。

- HEAD : 获取资源的元数据

- OPTIONS : 获取信息，关于资源的哪些属性是客户端可以改变的。



/zoos/ID/animals/ID：删除某个指定动物园的指定动物



#### 过滤条件

- ?limit=10：指定返回记录的数量



#### 状态码 （Status Codes）

- 200 ok [ get ] 返回用户的数据，幂等的（Idempotent）

- 201 created [ post / put / patch ] 新建 修改  成功

- 202 accepted [ * ] 请求已进入后台队列（异步任务）

- 400 INVALID REQUEST - [POST/PUT/PATCH]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作，该操作是幂等的

- 401 Unauthorized - [*]：表示用户没有权限（令牌、用户名、密码错误）

- 403 Forbidden - [*] 表示用户得到授权（与401错误相对），但是访问是被禁止的

- 404 NOT FOUND - [*]：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的

- 406 Not Acceptable - [GET]：用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式）

- 410 Gone -[GET]：用户请求的资源被永久删除，且不会再得到的

- 422 Unprocesable entity - [POST/PUT/PATCH] 当创建一个对象时，发生一个验证错误

- 500 INTERNAL SERVER ERROR - [*]：服务器发生错误，用户将无法判断发出的请求是否成功。



#### 错误处理

{    error: "Invalid API key" }



#### 返回结果

- GET /collection：idnex 返回资源对象的列表（数组）

- GET /collection/resource：show 返回单个资源对象

~ POST /collection：create 返回新生成的资源对象

- PUT /collection/resource：update 返回完整的资源对象

- PATCH /collection/resource：update 返回完整的资源对象

- DELETE /collection/resource：destroy 返回一个空文档



####  Hypermedia API (HATEOAS)

向api.example.com的根目录发出请求可得到api
