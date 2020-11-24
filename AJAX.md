##  AJAX

#### 全局刷新

![image-20201120161325976](D:\myself\Study_work\phone\image-20201120161325976.png)

![image-20201120161415478](D:\myself\Study_work\phone\image-20201120161415478.png)

![image-20201120161543076](D:\myself\Study_work\phone\3)

#### 局部刷新

![image-20201120161705915](D:\myself\Study_work\phone\4)

#### Ajax介绍

这个一部对象上存在浏览器内部内存中的，使用js语法创建和使用该对象

+ 异步--一次可以处理多个请求

+ Javascrip--javascrip的脚本，在浏览器中运行，创建异步对象发送请求更新页面的dom对象

+ xml--网络中传输的数据格式，使用json替换了xml的数据格式

  ##### XML
  
  可扩展的标记语言， 被设计用来传输和存储数据，标签名字均为自定义，被JSON替代
  
  ##### 缺点
  
  没有后退可言，存在跨域问题(同源处理),SEO不友好（爬虫爬不动结果）
  
#### HTTP协议

超文本传输协议，详细规定了浏览器和万维网服务器之间的相互通信规则约定

+ 请求报文

  行    POST /  URL  HTTP/1.1(请求版本类型)

  头    Host：atguigu.com

  ​        Cookie：name=guigu

  ​        Content-type：application/x-www-form-urlencoded

  ​        User-Agent：chrome 83

  空行

  体     username=admin&&password=admin(get可以省略)

  

+ 响应报文

  行   HTTP/1.1  200 OK   (404--找不到403--没有权限401--未授权500--内部错误)

  头   Content-Type：text/html;charset=utf-8

  ​       Content-length：2048

  ​       Content-encoding：gzip

  空行

  体    html的内容

#### Express框架

  是用来搭建了一个express.js的服务器可以用来访问本地的请求地址

### 异步

#### 异步对象的实现步骤

+ 创建对象

  ```
  var xmlHttp=new XMLHttpRequest();
  ```

+ 给异步对象绑定一个事件onreadystatechange

     当异步对象发起请求，获取了数据都会触发这个事件，这个事件需要一个函数来处理状态的变化

  ```
  btn.onclick=fun1()
  //这个函数会被sent(),open()分别进行回调执行
  xmlHttp.onreadystatechange=function(){
    处理请求的状态变化
    if(xmlHttp.readyState==4&&xmlHttp.status==200){
      //可以处理服务器端的数据，更新当前页面
    var data=xmlHttp.responseText;
      //数据更新部分可以使用jquery去实现
    document.getElementById("name").value=data;
    }
}
  ```

     异步对象的属性readyState表示异步对象请求状态的变化

  + 0：请求未初始化，创建异步请求对象 var xmlHttp=new XMLHttpRequest()

  + 1：初始化异步请求对象，xmlHttp.open(请求方式，请求地址，true（异步)）

  + 2：异步对象发送请求 xmlHttp.send()
  
  + 3：异步对象接收应答数据，从服务端返回数据。内部处理
  
  + 4：异步请求对象已经将数据解析完毕。可以读取数据，更新页面
  
  
  
     异步对象的属性status表示网络请求的状况 200（成功）404（未访问到）500（出错）
  
+ 初始异步请求对象

     异步的方法open()

     ``` 
     xmlHttp.open(请求方式get|post，“请求服务器端的地址”，true（异步)）
     xmlHttp.open（“get”，“loginServlet?name=zs&&pwd=123”,true);
     ```

     + 设置请求头

       ``` 
       xml.sendRequestHeader('Content-Type','application/x-www-form-urlencoded')
       ```
       
     + 设置自定义请求头

       + 解决跨域问题

         ```
          response.setHeader('Access-Control-Allow-Origin', '*');
         ```

       + 解决自定义Header请求头

         ```
         response.setHeader('Access-Control-Allow-Headers', '*');
         ```

+ 异步对象发送请求

     xmlHttp.send()

     获取服务器端返回的数据，使用异步对象的属性responseText



#### 使用JSON实现请求

+ 传送数据

  ```
  const data={name:'aixuexi'}
  let str=JSON.stringify(data);
  response.send(str)
  ```

+ 在html页面对数据进行修改

  + 手动对数据进行修改

    ```
    let data = JSON.parse(xhr.response)
    console.log(data);
    result.innerHTML = data.name;
    ```
    
  + 利用xhr属性进行自动修改
  
    ```
    xhr.responseType='json';
    result.innerHTML = xhr.response.name;//直接可以获取需要的内容
    ```
  
####  IE缓存问题

 当第一次对服务器发送请求会将信息放在存储器中，第二次发请求可能从存储器中去取，这对时效性是不好的，所以需要解决这个问题

```
http://127.0.0.1:8000/ie?t=+Date.now()//根据时间戳的不同就会不断发请求给服务器
```

#### 超时和网络异常

```
xml.timeout = 2000;
//延迟回调
xml.ontimeout = function() {
  alert("网络异常，稍后重试");
};
//网络异常回调
xml.onerror = function() {
  aler("你的网络似乎出了问题");
}
```
#### 请求取消

```
x.abort();
```
#### 取消重复的访问服务器

  添加一个标识变量去判断是否正在进行请求

```
let issending = false;
```

  当new一个对象的时候就是true，send（)结束后就是false，如果刚开始true，则打断x.abort()

### 用Jquary实现发送请求

​    html页面中的JS文件实现  

```
 //使用bootstrap设置样式，eq(0)表示第几个按钮下标
 $('button').eq(0).click(function() {
            //使用的get请求方式
            $.get('http://127.0.0.1:8000/jquery-server', {
                a: 100,
                b: 200
            }, function(data) {
                console.log(data);
            }, 'json'); //可以转化为对象的格式，不加则是字符串
        });
```

​    JS中的express框架中JSON对内容的处理

```
const data = { name: '张黑马加油' };
response.send(JSON.stringify(data));
```

​    通用Ajax的请求，可以去设置很多的返回信息

```
$('button').eq(2).click(function() {
            $.ajax({
                //url访问的地址
                url: 'http://127.0.0.1:8000/delay',
                //参数
                data: {
                    a: 100,
                    b: 200
                },
                //请求类型
                type: 'GET',
                //响应体结果
                dataType: 'json',
                //成功DE回调
                success: function(data) {
                    console.log(data);
                },
                //超时时间
                timeout: 2000,
                //失败的回调
                error: function() {
                    console.log("出错啦");
                }
            })
        })
```

#### 用Axios实现发送请求

  可以直接发送json数据类型

+ GET方式

```
axios.defaults.baseURL = 'http://127.0.0.1:8000';
        btns[0].onclick = function() {
            axios.get('/axios-server', {
                //url参数
                params: {
                    id: 100,
                    vip: 7
                },
                //请求头信息
                headers: {
                    name: 'aixuexi',
                    age: 20
                }
            }).then(value => {
                console.log(value);
            })
        }
```

+ POST方式

```
 btns[1].onclick = function() {
            axios.post( //url参数
                'axios-server', {
                    username: 'admin',
                    password: 'admin'
                }, {
                    params: {
                        id: 200,
                        vip: 9
                    },
                    //请求头信息
                    headers: {
                        heigh: 180,
                        weigh: 180,
                    }
                },
            );
        };
```

+ AJAX方法

```
btns[2].onclick = function() {
            axios({
                //请求方式
                type: 'GET',
                //url参数
                url: 'axios-server',
                params: {
                    id: 300,
                    vip: 8
                },
                headers: {
                    a: 400,
                    b: 500
                },
                data: {
                    username: 'zll',
                    password: 'zll'
                }
            }).then(response => {
                console.log(response);
            })
        }
```

+ 使用fetch函数发送对象



#### 详谈跨域问题

##### 同源策略

  安全策略，请求的url和服务器的url 的协议，域名，端口号，必须完全相同

##### 跨域问题

  违背同源策略就是跨域，a.com向b.com发送；3000向8000端发送请求

##### JSONP解决跨域问题

  就是使用一段js代码来返回我们想要的数据，这个js代码处理是在服务器端实现的

##### CORS跨域解决方案

  通过设置一个HTTP响应头来告诉浏览器该访问需要跨域

