

## 浏览器是如何对页面进行渲染的

1. 解析html文件构成**DOM树**

2. 解析css文件构成**渲染树**

3. **边解析，边渲染**

4. JS单线程运行，**JS有可能修改DOM结构**

   意味着JS执行完成前，后续所有资源的下载是没有必要的

   所以JS是单线程，会阻塞后续资源下载

- ##### 浏览器解析HTML代码，并请求代码中的资源

  1. 浏览器拿到html文件后，就开始解析其中的html代码

  2. 遇到静态资源(js/css/image)会向服务器去请求下载（会使用多线程下载，每个浏览器的线程数不一样）

     这时用上keep-alive特性，建立一次http连接，可以请求多个资源，下载的顺序就是按照代码的顺序，

     但是由于每个资源的大小不同，浏览器又是多线程请求资源，所有最后显示的顺序不一定是代码的顺序

- ##### 浏览器对页面进行渲染呈现给用户

  浏览器把请求的静态资源和html代码进行渲染。浏览器是一个边解析边渲染的过程

  浏览器解析html文件构建DOM树

  解析CSS文件构建渲染树

  渲染树构建完成后，浏览器开始布局渲染树并将其绘制到屏幕上

  其中涉及**回流(reflow)**和**重绘(repain)**

  - DOM节点的各个元素都是以盒模型的形式存在，这些都需要浏览器去计算其位置和大小等，这个过程称为reflow
  - 当盒模型的位置，大小以及其他属性，如字体，颜色等都确定下来之后，浏览器便开始绘制内容，这个过程称为repain
  - 页面首次加载时，必然会经济reflow和repain
  - 回流和重绘过程非常消耗性能，尤其在移动设备上，会破坏用户体验，造成页面卡顿，应该尽可能减少回流和重绘

  js的解析是由浏览器中的JS解析引擎完成的

  js是单线程运行，js有可能修改DOM结构，意味着js执行完成之前，后续所有资源的下载是没有必要的，所有JS是单线程，会阻塞后续资源下载









# HTTP缓存控制

## WEB缓存

web缓存大致可分为：数据库缓存，服务器端缓存（代理服务器缓存、CDN缓存），浏览器缓存

浏览器缓存大致包含：HTTP缓存，indexDB，cookie，localStorage等等

HTTP缓存能够帮助服务器提高并发性能，很多资源不需要重复请求直接从浏览器中拿缓存

常用术语

- ##### 缓存命中率

  从缓存中得到数据的请求数与所有请求数的比率。理想状态是越高越好

- ##### 过期内容

  超过设置的有效时间，被标记为陈旧的内容。

  通常过期内容不能用于回复客户端的请求，必须重新向源服务器请求新的内容或者验证缓存的内容是否仍然准备

- ##### 验证

  验证缓存中的过期内容是否仍然有效，验证通过的话刷新过期时间

- ##### 失效

  失效就是把内容从缓存中移除。当内容发生改变时就必须移除失效的内容

浏览器缓存主要是HTTP协议定义的缓存机制。

HTML meta标签：

```html
<META HTTP-EQUIV="Pragma" CONTENT="no-store">
```

让浏览器不缓存当前页面。但是代理服务器不解析HTML内容，一般应用广泛的是用HTTP头信息控制缓存



## HTML缓存分类

浏览器缓存分为强缓存和协商缓存，浏览器加载一个页面的简单流程如下

1. 浏览器先根据这个资源的http头信息来判断是否命中强缓存

   如果命中则直接加载缓存中的资源，并不会将请求发送到服务器

2. 如果未命中强缓存，则浏览器会将资源加载请求发送到服务器

   服务器来判断浏览器本地缓存是否失效

   若可以使用，则服务器并不会返回资源信息，浏览器继续从缓存加载资源

3. 如果未命中协商缓存，则服务器会将完整的资源返回给浏览器，浏览器加载新资源，并更新缓存

浏览器第一次请求

<img src="HTTP.assets/image-20220327010231396.png" alt="image-20220327010231396" style="zoom: 40%;" /> 

浏览器第二次请求

<img src="HTTP.assets/image-20220327010452736.png" alt="image-20220327010452736" style="zoom: 40%;" /> 



### 强缓存

命中强缓存时，浏览器并不会将请求发送给服务器。

http的返回码是200，但是在size列会显示为from cache

<img src="HTTP.assets/image-20220327012917250.png" alt="image-20220327012917250" style="zoom:67%;" /> 

强缓存是利用http的返回头中的Expires或者Cache-Control两个字段来控制的，用来表示资源的缓存时间

#### Expires

缓存过期的时间，用来指定资源到期的时间，是服务器端的具体的时间点。也就是说，Expires=max-age+请求时间，需要和Last-modified结合使用。但在上面我们提到过，cache-control的优先级更高。Expires是Web服务器响应消息头字段，在响应http请求时告诉浏览器在过期时间前浏览器可以直接从浏览器缓存取数据，而无需再次请求

<img src="HTTP.assets/image-20220327191313961.png" alt="image-20220327191313961" style="zoom: 67%;" /> 

Expires字段会返回一个时间，这个时间代表着这个资源的失效时间

由于失效时间是一个**绝对时间**，当客户端本地的时间被修改以后，**服务器和客户端时间不同步，会导致缓存混乱**

#### Cache-Control

Cache-Control是一个**相对时间**，例如Cache-Control:3600，代表着资源的有效期是3600秒。

由于是相对时间，并且都是与客户端时间比较，所以服务器与客户端有时间偏差也不会导致问题

Cahce-Control与Expires可以在服务器配置同时启用或者启用任意一个，同时启用的时候**Cache-Control优先级高**

Cache-Control可以由多个字段组合而成

1. ##### max-age

   指定一个时间长度，在这个时间段内缓存是有效的，单位是s

   `Cache-Control:max-age=31536000`，缓存有效期为(31536000/24/60*60)天，第一次访问这个资源时，服务器端也返回了Expires字段，并且过期时间是一年后

   <img src="HTTP.assets/image-20220327194137445.png" alt="image-20220327194137445" style="zoom:50%;" /> 

2. ##### s-maxage

   同max-age，覆盖max-age、Expires，但仅适用于共享缓存，在私有缓存中被忽略

3. ##### public

   表明响应可以被任何对象（发送请求的客户端、代理服务器等）缓存

4. ##### private

   表明响应只能被单个用户（可能是操作系统用户、浏览器用户）缓存，是非共享的，不能被代理服务器缓存

5. ##### no-cache

   强制所有缓存了该响应的用户，在使用已缓存的数据前，发送带验证器的请求到服务器。不是字面意思上的不缓存

6. ##### no-store

   禁止缓存，每次请求都要向服务器重新获取数据

7. ##### must-revalidate

   指定如果页面是过期的，则去服务器进行获取。不常用

#### Nginx强缓存配置

```nginx
# nginx.conf
location / {
    
    # add_header    Cache-Control  max-age=100;
    add_header    Cache-Control  max-age=6048000;
    # add_header    Cache-Control  no-cache;
    # add_header    Cache-Control  private;
    # add_header    Cache-Control  max-age=315360000;
    # add_header    Cache-Control  no-cache;
    # expires 30d;
    # add_header    Cache-Control  max-age=3600;
    
    if ($request_filename ~* ^.*?\.(gif|jpg|jepg|png|bmp|swf)$){
        # add_header    Cache-Control  no-cache;
        add_header    Cache-Control  max-age=3600;
        # expires 30d;
    }
    index  index.html index.htm;
}
```



### 协商缓存

若未命中强缓存，则浏览器会将请求发送至服务器。

服务器根据http头信息中的Last-Modify/If-Modify-Since或Etag/If-None-Match来判断是否命中协商缓存

如果命中，则**http返回码为304**，浏览器从缓存中加载资源

协商缓存中，浏览器会询问服务器浏览器中缓存的文件有没有更新，如果没有更新就使用缓存文件返回状态码304，如果更新了，就从服务器取新的文件返回状态码200

#### Last-Modify/If-Modify-Since

浏览器第一次请求一个资源的时候，服务器返回的header中会加上Last-Modify

Last-Modify是一个时间标识该资源的最后修改时间，例如`Last-Modify: Thu,31 Dec 2037 23:59:59 GMT`

第一次请求<img src="HTTP.assets/image-20220327221944977.png" alt="image-20220327221944977" style="zoom:67%;" /> 

当浏览器再次请求该资源时，发送的请求头中会包含If-Modify-Since，该值为缓存之前返回的Last-Modify

服务器收到If-Modify-Since后，根据资源的最后修改时间判断是否命中缓存

第二次请求<img src="HTTP.assets/image-20220327222725464.png" alt="image-20220327222725464" style="zoom:67%;" /> 

如果命中缓存，则返回304，并且不会返回资源内容，并且不会返回Last-Modify

由于对比的服务端时间，所有客户端与服务器时间差距不会导致问题

但是有时候通过**最后修改时间来判断资源是否修改还是不太准确**（资源变化了最后修改时间也可以一致），于是出现了ETag/If-None-Match

#### Etag/If-None-Match

与Last-Modify/If-Modify-Since不同的是，Etag/If-None-Match返回的是一个**校验码**（Etag:entity tag）

Etag可以保证每一个资源是唯一的，资源变化都会导致Etag变化

Etag值的变更则说明资源状态已经被修改。服务器根据浏览器上发送的If-None-Match值来判断是否命中缓存

<img src="HTTP.assets/image-20220327234316894.png" alt="image-20220327234316894" style="zoom:67%;" /> 

Last-Modified可以和Etag一起使用，服务器会**优先验证Etag**，一致的情况下才会继续对比Last-Modified

 
