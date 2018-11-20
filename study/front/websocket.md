概述
----

## WebSocket 是什么

   WebSocket 是一种网络通信协议。[RFC6455](https://tools.ietf.org/html/rfc6455) 定义了它的通讯协议。

   WebSocket 是Html5 开始提供的一种在单个TCP 连接上进行全双工通讯的协议。

> **WebSocket由来**

   Http 协议是一种无状态的、无连接的、单向的应用层协议。它采用了请求/响应模型。通讯请求只能由客户端发起，服务端对请求做出应答处理。

   *因此 Http 协议无法实现服务器主动向客户端发起消息*

   这种单向请求的特点，注定了如果服务器有连续的状态变化，客户端要获知就非常麻烦。大多数 Web 应用程序将通过频繁的异步JavaScript和XML（AJAX）请求实现长轮询。轮询的效率低，非常浪费资源（因为必须不停连接，或者 HTTP 连接始终打开）

![Alt ajax-long-polling.png](../../_media/websocket/ajax-long-polling.png)

   因此，工程师们一直在思考，有没有更好的方法。WebSocket 就是这样发明的。WebSocket 连接允许客户端和服务器之间进行全双工通信，以便任一方都可以通过建立的连接将数据推送到另一端。WebSocket 只需要建立一次连接，就可以一直保持连接状态。这相比于轮询方式的不停建立连接显然效率要大大提高


![Alt ajax-long-polling.png](../../_media/websocket/websockets-flow.png)



## SocketIO 

   [参考文档 w3cschool](https://www.w3cschool.cn/socket/socket-ulbj2eii.html)

   [项目地址 github](https://github.com/wantstudy/chat-example)

   	WebSocket是SocketIO的一个子集
   	
> **前端代码**`index.html`
	
	<!doctype html>
	<html>
	<head>
	    <title>Socket.IO chat</title>
	    <style>
	        * { margin: 0; padding: 0; box-sizing: border-box; }
	        body { font: 13px Helvetica, Arial; }
	        form { background: #000; padding: 3px; position: fixed; bottom: 0; width: 100%; }
	        form input { border: 0; padding: 10px; width: 90%; margin-right: .5%; }
	        form button { width: 9%; background: rgb(130, 224, 255); border: none; padding: 10px; }
	        #messages { list-style-type: none; margin: 0; padding: 0; }
	        #messages li { padding: 5px 10px; }
	        #messages li:nth-child(odd) { background: #eee; }
	    </style>
	</head>
	<body>
	<ul id="messages"></ul>

	<form action="">
	    <input id="m" autocomplete="off" /><button>Send</button>
	</form>

	<script src="/socket.io/socket.io.js"></script>
	<script src="https://code.jquery.com/jquery-1.11.1.js"></script>
	<script>
	    $(function () {
	        var socket = io();
	        var msg ;
	        $('form').submit(function(){
	            msg = $('#m').val();
	            socket.emit('chat message', msg);
	            $('#messages').append($('<li>').text(msg));
	            $('#m').val('');
	            return false;
	        });
	        socket.on('chat message', function(data){
	            $('#messages').append($('<li>').text(data.msg));
	        });
	    });
	</script>
	</body>
	</html>
		


> **NodeJS 服务端**`index.js`

	var app = require('express')();
	var http = require('http').Server(app);
	var io = require('socket.io')(http);

	app.get('/', function(req, res){
	    res.sendFile(__dirname+"/index.html");
	});

	io.on("connection",function (socket) {
	    console.log('new connect');

	    socket.on('disconnect', function(){
	        console.log('a user disconnect');
	    });

	    socket.on('chat message', function(msg){
	        console.log('message: ' + msg);
	        socket.broadcast.emit('chat message',{msg:msg});
	    });

	    socket.on("test",function (msg) {
	        console.log(msg);
	    })

	});

	http.listen(3001, function(){
	    console.log('listening on *:3001');
	});

> **NodeJS 依赖**`package.json`

	{
	  "name": "socket-chat-example",
	  "version": "0.0.1",
	  "description": "my first socket.io app",
	  "dependencies": {
	    "express": "^4.15.2",
	    "socket.io": "^2.1.1"
	  }
	}