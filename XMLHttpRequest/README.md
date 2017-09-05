## XMLHttpRequest

---

之前也很少尝试过直接使用原生的`xhr`对象去做网络请求，对于网络请求的掌握也只是基本会向后台请求数据，一知半解，这里来总结一下

---

``` javascript
var xhr = new XMLHttpRequest()
```

创建对象之后，可以通过`xhr`对象上提供的很多接口来进行网络请求。

### setRequestHeader

我们可以通过`xhr.setRequestHeader(header, value)`来进行请求头的修改或者增加。

> 需要注意的是：`Content-Type`的默认值与具体发送的数据类型有关

比如：

- 如果data是 Document 类型，同时也是HTML Document类型，则content-type默认值为text/html;charset=UTF-8;否则为application/xml;charset=UTF-8
- 如果data是 DOMString 类型，content-type默认值为text/plain;charset=UTF-8；
- 如果data是 FormData 类型，content-type默认值为multipart/form-data; boundary=[xxx]
- 如果data是其他类型，则不会设置content-type的默认值

> 也就是说传不同类型的参数的时候，`content-type`的默认值也会不一样。不过我们也可以显示使用`xhr.setRequestHeader`来修改请求的`content-type`头部

### getAllResponseHeaders和getResponseHeader

> getAllResponseHeaders()返回所有头部，getResponseHeader(header)传入header的key获取对应头部的值

调用这两个方法来获取头部的时候，因为浏览器安全限制，只能获取到`simple response header`：Cache-Control,Content-Language,Content-Type,Expires,Last-Modified,Pragma;
和`Access-Control-Expose-Headers`

### xhr.responseType

这个属性用于过滤返回结果，可以让我们直接通过`xhr.response`获得对应的数据类型

> 可以为`xhr.responseType`指定的值有："", "text", "document", "json", "blob", "arrayBuffer"

设置之后可以直接在`xhr.response`获得对应的数据类型

### xhr.onprocess和xhr.upload.onprocess

`xhr.onprocess`：用于添加显示下载进度的回调
`xhr.upload.onprocess`：用于添加显示上传进度的回调

### xhr.withCredentials

> 我们都知道，在发同域请求时，浏览器会将cookie自动加在request header中。但大家是否遇到过这样的场景：在发送跨域请求时，cookie并没有自动加在request header中。

`xhr.withCredentials`开启后才能在跨域请求中做带有`cookies`的处理（同时服务端也需要设置`Access-Control-Allow-Credentials: true`）

在客户端设置`xhr.withCredentials`为`true`表示允许客户端接受服务端返回的`cookies`并且在客户端做对应的处理。默认为`false`

> 不过需要注意的是：如果跨域的请求带有`cookies`认证信息的话，`Access-Control-Allow-Origin`就不能设为星号，必须指定明确的、与请求网页一致的域名。不然请求依然会报错。同时，Cookie依然遵循同源政策，只有用服务器域名设置的Cookie才会上传，其他域名的Cookie并不会上传，且（跨源）原网页代码中的`document.cookie`也无法读取服务器域名下的Cookie。

---