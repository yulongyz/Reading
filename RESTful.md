Fielding：改变应用程序的互动风格比改变互动协议，对整体表现有更大的影响。

# 起源 #

REST这个词，是Roy Thomas Fielding在他2000年的博士论文中提出的。

	Fielding是一个非常重要的人，他是HTTP协议（1.0版和1.1版）的主要设计者、Apache服务器软件的作者之一、Apache基金会的第一任主席。

# 名称 #

Fielding将他对互联网软件的架构原则，定名为REST，即Representational State Transfer的缩写。我对这个词组的翻译是"表现层状态转化"。

如果一个架构符合REST原则，就称它为RESTful架构。

# 资源 #

REST的名称"表现层状态转化"中，省略了主语。"表现层"其实指的是"资源"（Resources）的"表现层"。

	所谓"资源"，就是网络上的一个实体，或者说是网络上的一个具体信息。它可以是一段文本、一张图片、一首歌曲、一种服务，总之就是一个具体的实在。

# 表现层 #

"资源"是一种信息实体，它可以有多种外在表现形式。我们把"资源"具体呈现出来的形式，叫做它的"表现层"（Representation）。

	URI只代表资源的实体，不代表它的形式。严格地说，有些网址最后的".html"后缀名是不必要的，因为这个后缀名表示格式，属于"表现层"范畴，而URI应该只代表"资源"的位置。它的具体表现形式，应该在HTTP请求的头信息中用Accept和Content-Type字段指定，这两个字段才是对"表现层"的描述。

# 状态转化 #

访问一个网站，就代表了客户端和服务器的一个互动过程。在这个过程中，势必涉及到数据和状态的变化。

	客户端用到的手段，只能是HTTP协议。具体来说，就是HTTP协议里面，四个表示操作方式的动词：GET、POST、PUT、DELETE。它们分别对应四种基本操作：GET用来获取资源，POST用来新建资源（也可以用于更新资源），PUT用来更新资源，DELETE用来删除资源。

# 综述 #

1. 每一个URI代表一种资源；
2. 客户端和服务器之间，传递这种资源的某种表现层；
3. 客户端通过四个HTTP动词，对服务器端资源进行操作，实现"表现层状态转化"。


----------

# 协议 #

API与用户的通信协议，总是使用HTTPs协议。

	https://api.example.com

# 域名 #

应该尽量将API部署在专用域名之下。

# 版本 #

应该将API的版本号放入URL。

	https://api.example.com/v1/

# 路径 #

在RESTful架构中，每个网址代表一种资源（resource），所以网址中不能有动词，只能有名词，而且所用的名词往往与数据库的表格名对应。

	https://api.example.com/v1/zoos

# HTTP动词 #

对于资源的具体操作类型，由HTTP动词表示。

	GET（SELECT）：从服务器取出资源（一项或多项）。
	POST（CREATE）：在服务器新建一个资源。
	PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。
	PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）。
	DELETE（DELETE）：从服务器删除资源。

还有两个不常用的HTTP动词。
	
	HEAD：获取资源的元数据。
	OPTIONS：获取信息，关于资源的哪些属性是客户端可以改变的。

示例
	
	GET /zoos：列出所有动物园
	POST /zoos：新建一个动物园
	GET /zoos/ID：获取某个指定动物园的信息
	PUT /zoos/ID：更新某个指定动物园的信息（提供该动物园的全部信息）
	PATCH /zoos/ID：更新某个指定动物园的信息（提供该动物园的部分信息）
	DELETE /zoos/ID：删除某个动物园
	GET /zoos/ID/animals：列出某个指定动物园的所有动物
	DELETE /zoos/ID/animals/ID：删除某个指定动物园的指定动物

# 过滤信息 #

下面是一些常见的参数。

	?limit=10：指定返回记录的数量
	?offset=10：指定返回记录的开始位置。
	?page=2&per_page=100：指定第几页，以及每页的记录数。
	?sortby=name&order=asc：指定返回结果按照哪个属性排序，以及排序顺序。
	?animal_type_id=1：指定筛选条件

# 状态码 #

服务器向用户返回的状态码和提示信息，常见的有以下一些（方括号中是该状态码对应的HTTP动词）。

	200 OK - [GET]：服务器成功返回用户请求的数据，该操作是幂等的（Idempotent）。
	201 CREATED - [POST/PUT/PATCH]：用户新建或修改数据成功。
	202 Accepted - [*]：表示一个请求已经进入后台排队（异步任务）
	204 NO CONTENT - [DELETE]：用户删除数据成功。
	400 INVALID REQUEST - [POST/PUT/PATCH]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作，该操作是幂等的。
	401 Unauthorized - [*]：表示用户没有权限（令牌、用户名、密码错误）。
	403 Forbidden - [*] 表示用户得到授权（与401错误相对），但是访问是被禁止的。
	404 NOT FOUND - [*]：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的。
	406 Not Acceptable - [GET]：用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式）。
	410 Gone -[GET]：用户请求的资源被永久删除，且不会再得到的。
	422 Unprocesable entity - [POST/PUT/PATCH] 当创建一个对象时，发生一个验证错误。
	500 INTERNAL SERVER ERROR - [*]：服务器发生错误，用户将无法判断发出的请求是否成功。

# 错误处理 #

如果状态码是4xx，就应该向用户返回出错信息。一般来说，返回的信息中将error作为键名，出错信息作为键值即可。

	{
    	error: "Invalid API key"
	}

# 返回结果 #

针对不同操作，服务器向用户返回的结果应该符合以下规范。

	GET /collection：返回资源对象的列表（数组）
	GET /collection/resource：返回单个资源对象
	POST /collection：返回新生成的资源对象
	PUT /collection/resource：返回完整的资源对象
	PATCH /collection/resource：返回完整的资源对象
	DELETE /collection/resource：返回一个空文档

# 关联 #

关于如何处理资源之间的管理REST原则也有相关的描述：

	GET /tickets/12/messages- Retrieves list of messages for ticket #12
	GET /tickets/12/messages/5- Retrieves message #5 for ticket #12
	POST /tickets/12/messages- Creates a new message in ticket #12
	PUT /tickets/12/messages/5- Updates message #5 for ticket #12
	PATCH /tickets/12/messages/5- Partially updates message #5 for ticket #12
	DELETE /tickets/12/messages/5- Deletes message #5 for ticket #12

# 限制返回值的域 #

可以使用fields查询参数来限制返回的域例如
	
	GET ticketsfields=id,subject,customer_name,updated_at&state=open&sort=-updated_at

# 其他 #

1. API的身份认证应该使用OAuth 2.0框架。
2. 服务器返回的数据格式，应该尽量使用JSON，避免使用XML。