---
title: RESTful API
date: 2018-08-19 18:25:46
tags:
cover: https://res.cloudinary.com/practicaldev/image/fetch/s--RK-AgEnh--/c_imagga_scale,f_auto,fl_progressive,h_900,q_auto,w_1600/https://dev-to-uploads.s3.amazonaws.com/i/1s3bedypkt7zm8maikzg.png
---

# RESTful API

## 什么是 RESTful API

REST 全称为 Representational State Transfer(表现层状态转换)。REST 基于 HTTP 的设计风格，是一种万维网软件架构风格，目的是便于不同软件/程序在网络中互相传递信息。
符合 REST 设计风格的 Web API 称为 RESTful API。RESTful API 是目前比较成熟的一套互联网应用程序的API设计理论。可以为不同的客户端提供统一的接口。

## RESTful API 设计规范

### API版本

可以将 API 部署在专用的子域名下，或者放入主域名的 URL 中  
`https://api.example.com` 或 `https://example.com/api`  
将版本信息放入 HTTP header 头中，或 URL 中  
`https://api.example.com/v1`  
API 版本需向后兼容,使用新版本的 API 的同时,要保证旧版本的 API 可以正常使用

### URL

RESTful 的每个 URL 代表一种资源（resource），所有的动作都是对指定资源进行操作。URL 中不能有动词，只能有名词并且使用复数而且所用的名词往往与数据库的表格名对应，只能用小写。

``` js
/example.com/resources/
```

### HTTP 方法用动词

使用不同的 [HTTP](http://kjay.me/2018/03/02/HTTP/) 方法表示对资源进行不同的操作。

常用的 HTTP 方法有以下五种：

- `GET`:    获取资源
- `POST`:   添加资源
- `DELETE`: 删除资源
- `PUT`:    修改资源（整体替换）
- `PATCH`:  修改资源（部分修改）

``` js
GET /notebooks  // 获取所有笔记本（对一组资源的操作）
GET /notebooks/:ID  // 获取某个笔记本信息（对单个资源的操作）
POST /notebooks // 创建一个笔记本
DELETE /notebooks/:ID //删除某个笔记本
PUT /notebooks/:ID  // 更新某个笔记本（更新全部笔记本信息）
PATCH /notebooks/:ID  // 更新某个笔记本（更新部分笔记本信息）
```

### 响应状态码

请求的响应应返回相应的 [HTTP](http://kjay.me/2018/03/02/HTTP/) 状态码。

- **`200 OK`** 表示客户端发来的请求在服务器端被正常处理了。
- **`204 No Content`** 该状态码代表服务器接收的请求已成功处理，但在返回的响应报文中不含实体的主体部分。
- **`206 Partial Content`** 该状态码表示客户端进行了范围请求，而服务器成功执行了这部分的 GET 请求。
- **`301 Moved Permanently`** 永久性重定向。该状态码表示请求的资源已被分配了新的 URI，以后应使用资源现在所指的 URI。
- **`302 Found`** 临时性重定向。该状态码表示请求的资源已被分配了新的 URI，希望用户能使用新的 URI 访问。
- **`303 See Other`** 该状态码表示由于请求对应的资源存在着另一个 URI，应使用 GET 光谷定向获取请求的资源。
- **`400 Bad Request`** 请求出现语法错误。
- **`404 Not Found`** 服务器上无法找到请求的资源。
- **`500 Internal Server Error`** 服务器端在执行请求时发生了错误。也有可能是 Web 应用存在的 bug 或某些临时的故障。
- **`503 Service Unavailable`** 服务器暂时处于超负荷或正在进行停机维护，现在无法处理请求。

### 过滤参数

如果记录数量很多，服务器不可能都将它们返回给用户。API 应该 提供参数，过滤返回结果。下面是一些常见的参数。

- ?limit=10：指定返回记录的数量
- ?offset=10：指定返回记录的开始位置。
- ?page=2&per_page=100：指定第几页，以及每页的记录数。
- ?sortby=name&order=asc：指定返回结果按照哪个属性排序，以及排序顺序。
- ?animal_type_id=1：指定筛选条件

参数的设计允许存在冗余，即允许 API 路径和 URL 参数偶尔有重复。比如，GET /zoo/ID/animals 与 GET /animals?zoo_id=ID 的含义是相同的。

### 返回信息

针对不同操作，服务器向用户返回的结果应该符合以下规范。

- GET /notebooks：返回资源对象的列表（数组）
- GET /notebooks/notes：返回单个资源对象
- POST /notebooks：返回新生成的资源对象
- PUT /notebooks/notes：返回完整的资源对象
- PATCH /notebooks/notes：返回完整的资源对象
- DELETE /notebooks/notes：返回一个空文档

### 错误处理

如果出现错误应返回明确的错误信息

``` js
{
  code: '123'
  error: '权限不足'
}
```