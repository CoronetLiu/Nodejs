# Nodejs

---
### Node.js安装
[官方下载路径](https://nodejs.org/en/download/releases/)
```
node -v   //查看node版本
```

---
### Node.js核心模块
  核心模块由一些精简而高效的库组成，为 Node.js 提供了基本的API。主要包括：
  
  #### 1.events模块
  `events`模块只提供了一个对象：`events.EventEmitter`。
  
  `EventEmitter`的核心就是事件发射与事件监听器功能的封装。
  
  `EventEmitter`的每个事件由一个事件名和若干个参数组成，事件名是一个字符串，通常表达一定的语义。
  
  对于每个事件，`EventEmitter`支持若干个事件监听器。当事件发射时，注册到这个事件的事件监听器被依次调用，事件参数作为回调函数参数传递`
  
  ```
  const EventEmitter = require("events").EventEmitter;
  const event = new EventEmitter();
  event.on("some_event", function(){
      console.log("some_event start1");
  });
  event.on("some_event", function () {
      console.log("some_event start2")
  });
  
  event.emit("some_event");
  
  //输出
  some_event start1
  some_event start2
  ```
  - `EventEmitter.on(event, listener)`为指定事件注册一个监听器，接受一个字符串`event`和一个回调函数`listener`。
  - `EventEmitter.emit(event, [arg1], [arg2], [...])`发射`event`事件，传递若干可选参数到事件监听器的参数表。
  - `EventEmitter.once(event, listener)`为指定事件注册一个单次监听器，即监听器最多只会触发一次，触发后立刻解除该监听。
  - `EventEmitter.removeListener(event, listener)`移除指定事件的某个监听器，`listener`必须是该事件已经注册过的监听器。
  - `EventEmitter.removeAllListeners([event])`移除所有事件的所有监听器，如果指定`event`，则移除指定事件的所有监听器。
  
  ```
  const EventEmitter = require("events").EventEmitter;
  const event = new EventEmitter();
  event.on("some_event", function(){
      console.log("some_event start1");
  });
  event.on("some_event", function () {
      console.log("some_event start2")
  });
  
  event.emit("some_event");
  
  setTimeout(function(){
      event.emit("some_event");
  }, 1000);
  
  event.removeAllListeners("some_event");
  event.emit("some_event");
  
  //输出
  some_event start1
  some_event start2
  ```
  
  #### 2.Global模块
  全局共享，不需要导入模块即可以使用的属性
  ```
  __dirname    //文件所在的文件夹路径
  __filename   //文件所在位置的绝对路径
  require()    //导入模块
     
  console  //调试工具
    console.log([data][, ...])  //标准输出
    console.info([data][, ...])   //返回消息性消息
    console.error([data][, ...])  //输出错误消息
    console.warn([data][, ...])   //输出警告
    console.dir(obj[, options])   //检查对象
  ```
    
        
  #### 3.Path模块
  操作文件的路径,为文件操作服务
  ```
  const path = require("path")
  
  path.join([path1],[path2])    //连接路径 
  path.resolve([from ...], to)  //拼接绝对路径
    path.resolve('/pages/index', './a')   //=>'/pages/index/a'
    path.resolve('/pages/index', '/js/index/')  //=>'/js/index'
    path.resolve('root', 'images/png/', '../gif/image.gif')
    // 若当前工作目录为 D:/node
    // =>'D:/node/root/images/gif/image.gif'
  ```
     
  #### 4.File System模块   
  在服务端来操作文件,可能是需要将浏览器上传的图片保存到服务器,也可能是需要将服务器的资源读取之后返回给浏览器
    
  `注意`：目录 `fs`它是不会自动帮我们创建的需要我们自己手工创建文件如果不存在,调用`writeFile`与`appendFile`方法会自动帮我们创建
    
  ```
  const fs = require("fs");
  
  【异步读取】
  fs.readFile('input.txt', function (err, data) {
     if (err) {
         return console.error(err);
     }
     console.log("异步读取: " + data.toString());
  });
  
  【同步读取】
  let data = fs.readFileSync('input.txt');
  console.log("同步读取: " + data.toString());
  
  【异步获取文件信息】
  fs.stat('/Users/demo/fs.js', function (err, stats) {
    console.log(stats);
    console.log(stats.isFile())          //如果是文件返回 true，否则返回 false。
    console.log(stats.isDirectory())     //如果是目录返回 true，否则返回 false。
    console.log(stats.isBlockDevice())   //如果是块设备返回 true，否则返回 false。
    console.log(stats.isCharacterDevice())   //如果是字符设备返回 true，否则返回 false。
    console.log(stats.isSymbolicLink())      //如果是软链接返回 true，否则返回 false。
    console.log(stats.isFIFO())      //如果是FIFO，返回true，否则返回 false。FIFO是UNIX中的一种特殊类型的命令管道。
    console.log(stats.isSocket())    //如果是 Socket 返回 true，否则返回 false。
  })
  
  【异步写入】
  fs.writeFile('input.txt', '我是通过fs.writeFile写入文件的内容',  function(err) {
     if (err) {
         return console.error(err);
     }
     console.log("数据写入成功！");
     fs.readFile('input.txt', function (err, data) {
        if (err) {
           return console.error(err);
        }
        console.log("异步读取文件数据: " + data.toString());
     });
  });
  
  【创建目录】
  fs.mkdir("/User/test/",function(err){
     if (err) {
         return console.error(err);
     }
     console.log("目录创建成功");
  });
  
  【读取目录】
  fs.readdir("/User/",function(err, files){
     if (err) {
         return console.error(err);
     }
     files.forEach(function (file){
         console.log(file);
     });
  });
  
  【删除目录】
  fs.rmdir("/User/test",function(err){
     if (err) {
         return console.error(err);
     }
     fs.readdir("/User/",function(err, files){
        if (err) {
            return console.error(err);
        }
        files.forEach(function (file){
            console.log(file);
        });
     });
  });
  ```
  
  #### 5.URL模块
  处理客户端请求过来的URL
  ```
  操作流程:
    1.导入URL核心模块
    2.导入HTTP核心模块
    3.监听客户端的请求
    在这中间处理客户端请求过来的URL
    4.处理客户端的请求
    5.开启服务器
  ```
  
  #### 6.HTTP模块
  http 模块主要用于搭建HTTP服务端和客户端，使用HTTP服务器或客户端功能必须调用http模块
  ###### server.js
  ```
  const http = require('http');
  const fs = require('fs');
  const url = require('url');
   
  // 创建服务器
  http.createServer( function (request, response) {  
     // 解析请求，包括文件名
     let pathname = url.parse(request.url).pathname;
     
     // 输出请求的文件名
     console.log("Request for " + pathname + " received.");
     
     // 从文件系统中读取请求的文件内容
     fs.readFile(pathname.substr(1), function (err, data) {
        if (err) {
           console.log(err);
           // HTTP 状态码: 404 : NOT FOUND
           response.writeHead(404, {'Content-Type': 'text/html'});
        }else{             
           // HTTP 状态码: 200 : OK
           response.writeHead(200, {'Content-Type': 'text/html'});    
           
           // 响应文件内容
           response.write(data.toString());        
        }
        //  发送响应数据
        response.end();
     });   
  }).listen(8080);
   
  // 控制台会输出以下信息
  console.log('Server running at http://127.0.0.1:8080/');
  ```
  ###### client.js
  ```
  const http = require('http');
   
  // 用于请求的选项
  let options = {
     host: 'localhost',
     port: '8080',
     path: '/index.html'  
  };
   
  // 处理响应的回调函数
  let callback = function(response){
     // 不断更新数据
     let body = '';
     response.on('data', function(data) {
        body += data;
     });
     
     response.on('end', function() {
        // 数据接收完成
        console.log(body);
     });
  }
  // 向服务端发送请求
  let req = http.request(options, callback);
  req.end();
  ```
  


---
#### NPM包管理工具
```
npm init  //初始化package.json
npm install <moudle> -S  //安装生产所需依赖
npm install <moudle> -D  //安装开发所需依赖
npm install  //根据package.json下载依赖包(会自动生成package-lock.json)
npm uninstall <moudle> -S  //卸载包
npm uninstall <moudle> -D  //卸载包
npm install -g cnpm --registry=https://registry.npm.taobao.org  //安装cnpm使用淘宝镜像
npm config set registry https://registry.npm.taobao.org  //切换淘宝镜像下载源
npm config set registry https://www.npmjs.org  //切换官方下载源
npm ls  //查看下载源
npm test  //测试下载源
npm cache clear  //清除缓存
```
