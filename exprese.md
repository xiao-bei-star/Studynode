#### 基于express服务器实现前后台交互

##### express简介

+ express是基于 [Node.js](https://nodejs.org/en/) 平台，快速、开放、极简的 Web 开发框架，可以在小demo的练习中作为服务器向前端发送数据，用作访问本地的请求地址。

+ express的安装

  ```
  npm install express
  ```

+ 引入express框架

  ```
  const express = require('express');
  //创建应用对象
  const app = express();
  ```

##### Ajax简介

+ 是可以通过异步实现在不刷新整个浏览器的情况下，进行局部的数据更新
+ 缺点是存在跨域问题(需要具体解决)

##### 使用JSON实现请求

  在之前的ajax请求使用的是xml作为网络中的数据传输，后来被JSON替代

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
  

##### 小练习的具体说明

+ 使用jQuery来获取按键button的行为，然后绑定Ajax请求访问

+ 使用Ajax发送请求时，定义格式url访问地址，要传参数，请求的类型，响应体结果类型以及成功和失败的回调函数

+ 将后台的数据返回到html页面中去，使用原生的js进行异步对象的实现

  + 创建对象

    ```
    const xhr = new XMLHttpRequest();
    ```

  + 给异步对象绑定事件

    ```
     xhr.onreadystatechange = function() {}
    ```

  + 异步请求对象

    ```
    xmlHttp.open(请求方式get|post，“请求服务器端的地址”，true（异步)）
    ```

  + 异步发送对象

    ```
    xmlHttp.send()
    ```

+ 还有一点关于解决访问请求的跨域问题

   当前端请求的url和后台服务器的url，协议，域名，端口号不完全一致的时候，存在无法进行访问的问题

  + jsonp方法，在服务器端通过代码实现
  + CORS通过设置HTTP响应头来告诉浏览器该访问需要跨域