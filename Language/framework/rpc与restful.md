# 什么是 restful

- [知乎 - 怎样用通俗的语言解释REST，以及RESTful？](https://www.zhihu.com/question/28557115/answer/47846156)

首先要明确一点：REST 实际上只是一种设计风格，它并不是标准。（所以你可以看到网上一大堆的各种最佳实践，设计指南，但是没有人说设计标准）。[aisuhua/restful-api-design-references · GitHub](https://link.zhihu.com/?target=https%3A//github.com/aisuhua/restful-api-design-references)

REST -- REpresentational State Transfer 首先，之所以晦涩是因为前面主语被去掉了，全称是 Resource Representational State Transfer：通俗来讲就是：**资源在网络中以某种表现形式进行状态转移**。分解开来：

- Resource：资源，即数据（前面说过网络的核心）。比如 newsfeed，friends等；
- Representational：某种表现形式，比如用JSON，XML，JPEG等；
- State Transfer：状态变化。通过HTTP动词实现。



**1、REST 是面向资源的，这个概念非常重要，而资源是通过 URI 进行暴露。**
URI 的设计只要负责把资源通过合理方式暴露出来就可以了。对资源的操作与它无关，操作是通过 HTTP动词来体现，所以REST 通过 URI 暴露资源时，会强调不要在 URI 中出现动词。

比如：左边是错误的设计，而右边是正确的

```text
GET /rest/api/getDogs --> GET /rest/api/dogs 获取所有小狗狗 
GET /rest/api/addDogs --> POST /rest/api/dogs 添加一个小狗狗 
GET /rest/api/editDogs/:dog_id --> PUT /rest/api/dogs/:dog_id 修改一个小狗狗 
GET /rest/api/deleteDogs/:dog_id --> DELETE /rest/api/dogs/:dog_id 删除一个小狗狗 
```

左边的这种设计，很明显不符合REST风格，上面已经说了，URI 只负责准确无误的暴露资源，而 getDogs/addDogs...已经包含了对资源的操作，这是不对的。相反右边却满足了，它的操作是使用标准的HTTP动词来体现。

**2、REST很好地利用了HTTP本身就有的一些特征，如HTTP动词、HTTP状态码、HTTP报头等等**
REST API 是基于 HTTP的，所以你的API应该去使用 HTTP的一些标准。这样所有的HTTP客户端（如浏览器）才能够直接理解你的API（当然还有其他好处，如利于缓存等等）。REST 实际上也非常强调应该利用好 HTTP本来就有的特征，而不是只把 HTTP当成一个传输层这么简单了。

HTTP动词：

```text
GET     获取一个资源 
POST    添加一个资源 
PUT     修改一个资源 
DELETE  删除一个资源 
```

实际上，这四个动词实际上就对应着增删改查四个操作，这就利用了HTTP动词来表示对资源的操作。

HTTP状态码：

```text
200 OK 
400 Bad Request 
500 Internal Server Error
```

在 APP 与 API 的交互当中，其结果无非就三种状态：

- 所有事情都按预期正确执行完毕 - 成功
- APP 发生了一些错误 – 客户端错误
- API 发生了一些错误 – 服务器端错误

这三种状态与上面的状态码是一一对应的。

HTTP报头：

```text
Authorization 认证报头 
Cache-Control 缓存报头 
Cnotent-Type  消息体类型报头 
......
```

报头还有很多，不一一列举。HTTP报头是描述HTTP请求或响应的元数据，它的作用是客户端 与 服务器端进行相互通信时，告诉对方应该如何处理本次请求。

**如何设计才能满足 Restful 的要求呢？** 有如下几点以及一些最佳实践：

- URL root

  ```
  https://example.org/api/v1/*
  https://api.example.com/v1/*
  ```

- API versioning，可以放在URL里面，也可以用HTTP的header，如：`/api/v1/`

- URI使用名词而不是动词，且推荐用复数

  ```
  BAD
  /getProducts
  /listOrders
  /retrieveClientByOrder?orderId=1
  
  GOOD
  GET /products : will return the list of all products
  POST /products : will add a product to the collection
  GET /products/4 : will retrieve product #4
  PATCH/PUT /products/4 : will update product #4
  ```

- 保证 HEAD 和 GET 方法是安全的，不会对资源状态有所改变（污染）。比如严格杜绝如下情况：`GET /deleteProduct?id=1`

-  资源的地址推荐用嵌套结构。比如：

  ```
  GET /friends/10375923/profile
  UPDATE /profile/primaryAddress/city
  ```

- 警惕返回结果的大小。如果过大，及时进行分页（pagination）或者加入限制（limit）。HTTP协议支持分页（Pagination）操作，在Header中使用 Link 即可。

-  使用正确的HTTP Status Code表示访问状态：[HTTP/1.1: Status Code Definitions](HTTP/1.1: Status Code Definitions)

- 在返回结果用明确易懂的文本（String。注意返回的错误是要给人看的，避免用 1001 这种错误信息），而且适当地加入注释。

- 关于安全：自己的接口就用https，加上一个key做一次hash放在最后即可。考虑到国情，HTTPS在无线网络里不稳定，可以使用Application Level的加密手段把整个HTTP的payload加密。有兴趣的朋友可以用手机连上电脑的共享Wi-Fi，然后用Charles监听微信的网络请求（发照片或者刷朋友圈）。
  如果是平台的API，可以用成熟但是复杂的OAuth2，新浪微博这篇：[授权机制说明](https://open.weibo.com/wiki/%E6%8E%88%E6%9D%83%E6%9C%BA%E5%88%B6%E8%AF%B4%E6%98%8E)



# RPC与REST的区别

- [Web Service实践之REST vs RPC(转)](https://www.cnblogs.com/Tim-Yi/archive/2011/11/03/2234230.html)

如果你想只记住一点，那么就请记住 **RPC是以动词为中心的， REST是以名词为中心的**， 此处的动词指的是一些方法， 名词是指资源。

- 以动词为中心，意味着，当你要需要加入新功能时，你必须要添加更多的动词， 这时候服务器端需要实现相应的动词(方法)， 客户端需要知道这个新的动词并进行调用。

- 而以名词为中心， 假使我请求的是 `hostname/friends/`， 无论这个URI对应的服务怎么变化，客户端是无需 关注和更新的，而这种变化对客户端也是透明的。

至于其它的区别，如对实现语言的依赖， 耦合性等，这些都是上面提到的这个根本区别所衍生的。

让我们回到引入部分的2个问题. 当你每天使用HTTP冲浪时，你都在使用 [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) 与远程的服务器进行亲密接触. 当你使用Gtalk和同事朋友沟通时，你则是在享受着 [RPC](http://en.wikipedia.org/wiki/Remote_procedure_call) 的便利.

推荐阅读 [Restful User Experience](http://www.slideshare.net/trilancer/restful-user-experience-1421793) (这个slide是个人认为解释的最好的) 还有 [ReST vs SOA(P)](http://www.slideshare.net/ozten/rest-vs-soap-yawn).







