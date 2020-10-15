---
title: 基于Node.js搭建Socket聊天室
tags:
  - 前端
  - Node.js
excerpt: |2-

      
        
        
          <h5 id="前言"><a href="#前言" class="headerlink" title="前言"></a>前言</h5><p>可以毫不夸张的说，Node.js的出现带动了整个前端界的繁荣发展，自此进入百花齐放，百家争鸣的时代。时至今天，我们能用Node.js实现很多
        
      
      
date: 2018-02-07 14:06:12
---

##### [](#前言 "前言")前言

可以毫不夸张的说，Node.js的出现带动了整个前端界的繁荣发展，自此进入百花齐放，百家争鸣的时代。时至今天，我们能用Node.js实现很多
<!-- more -->
##### [](#前言 "前言")前言

可以毫不夸张的说，Node.js的出现带动了整个前端界的繁荣发展，自此进入百花齐放，百家争鸣的时代。时至今天，我们能用Node.js实现很多依靠传统服务器端编程语言才能实现的事，甚至更加简便、更加高效。  
本案例使用Node.js的net模块，建立服务端与客户端的Socket连接，简单实现了客户端广播消息通信和点对点通信。

##### [](#项目结构 "项目结构")项目结构

###### [](#server-js-服务端 "server.js 服务端")server.js 服务端

项目下的server.js是服务端的执行文件，用于创建服务端的Socket服务，监听指定端口；接收客户端传过来的数据，解析数据并按照数据中附带的协议进行广播消息通信或点对点通信。  
**具体代码如下：**

1  
2  
3  
4  
5  
6  
7  
8  
9  
10  
11  
12  
13  
14  
15  
16  
17  
18  
19  
20  
21  
22  
23  
24  
25  
26  
27  
28  
29  
30  
31  
32  
33  
34  
35  
36  
37  
38  
39  
40  
41  
42  
43  
44  
45  
46  
47  
48  
49  
50  
51  
52  
53  
54  
55  
56  
57  
58  
59  
60  
61  
62  
63  
64  
65  
66  
67  
68  
69  
70  
71  
72  
73  
74  
75  
76  
77  
78  
79  
80  
81  
82  
83  
84  
85  
86  
87  
88  
89  
90  
91  
92  
93  
94  
95  
96  
97  
98  
99  
100  
101  
102  
103  
104  
105  
106  
107  
108  
109  
110  
111  
112  
113  
114  
115  
116  
117  
118  
119  
120  
121  
122  
123  
124  
125  
126  
127  

// Socket聊天室 - server 服务端  
const net = require('net');  
  
// 定义clients（键值对集合），用于存储所有的客户端连接，通过用户名去索引客户端socket  
let clients = {};  
  
let server = net.createServer((socket) => {  
  
 // 客户端登入  
 function signin(clientDataContent) {  
 clientDataContent = JSON.parse(clientDataContent);  
 let username = clientDataContent.from;  
  
 // 如果clients（客户端 socket 集合）中有1个以上的成员，就广播通知所有人谁谁上线了，除了他本身  
 if (Object.keys(clients).length) {  
 let onlineNotice = {  // 组成上线通知消息数据格式  
 protocol: 'online',  
 online: username,  
 onlineCount: Object.keys(clients).length + 1  
 };  
 // 遍历 clients ，给除了自身的所有客户端发送消息  
 for (let username in clients) {  
 if (clients.hasOwnProperty(username)) {  
 let noticeClient = clients\[username\];  
 noticeClient.write(JSON.stringify(onlineNotice));  
 }  
 }  
 }  
  
 // 将新连接的客户端socket存储于clients  
 clients\[username\] = socket;  
  
 // 服务端打印上线提示消息  
 console.log(\`欢迎 ${socket.remoteAddress}:${socket.remotePort}【${username}】，加入聊天室，当前在线：${Object.keys(clients).length}\`);  
 }  
  
 // 广播消息通信  
 function broadcast(clientDataContent) {  
 // 广播出去消息数据格式 json  
 let sendClientData = JSON.parse(clientDataContent);  
  
 // 遍历clients对象（for in），给所有的客户端socket广播消息  
 for (let username in clients) {  
 if (clients.hasOwnProperty(username)) {  
 let client = clients\[username\];  
 client.write(JSON.stringify(sendClientData));  
 }  
 }  
 }  
  
 // p2p 点对点通信  
 function p2p(clientDataContent) {  
 let p2pClientData = JSON.parse(clientDataContent);  
 // 给指定的客户端发送消息  
 clients\[p2pClientData.to\].write(JSON.stringify(p2pClientData));  
 }  
  
 // 给每一个连接服务端的客户端socket注册data事件  
 socket.on('data', (chunk) => {  
 try {  
 // 对客户端传过来的数据chunk（json数据）进行序列化  
 let clientDataContent = chunk.toString().trim();  
  
 // 获取协议  
 let protocol = JSON.parse(clientDataContent).protocol;  
 switch (protocol) {  
 case 'signin':  
 signin(clientDataContent);  
 break;  
 case 'broadcast':  
 broadcast(clientDataContent);  
 break;  
 case 'p2p':  
 p2p(clientDataContent);  
 break;  
 default:  
 socket.write('错误！未能识别的通信协议！');  
 break;  
 }  
 } catch (error) {  
 socket.write('出现错误了哦~');  
 throw error;  
 }  
 });  
  
 // 给每一个连接服务端的客户端socket注册error事件，如果连接中断，则触发此事件  
 socket.on('error', (error) => {  
 // 在客户端对象中，将连接中断的那个客户端删除  
 let deletekey = null;  
  
 // 遍历clients对象，找到下线的socket，并将其删除  
 for (let username in clients) {  
 if (clients.hasOwnProperty(username)) {  
 let client = clients\[username\];  
 if (socket === client) deletekey = username;  
 }  
 }  
 delete clients\[deletekey\];  
  
 // 广播通知所有人，谁谁下线了  
 let offlineNotice = {  // 组成下线通知消息数据格式  
 protocol: 'offline',  
 offline: deletekey,  
 onlineCount: Object.keys(clients).length  
 };  
 for (let username in clients) {  
 if (clients.hasOwnProperty(username)) {  
 let noticeClient = clients\[username\];  
 noticeClient.write(JSON.stringify(offlineNotice));  
 }  
 }  
  
 // server 消息  
 console.log(\`${deletekey} 下线了，当前在线：${Object.keys(clients).length}\`);  
 });  
});  
  
  
// 监听指定端口  
let port = 2018;  
server.listen(port, (error) => {  
 if (error) {  
 console.log(\`${port}端口被占用！\`);  
 } else {  
 console.log(\`服务器端正常启动，正在监听${port}端口\`);  
 }  
});  

###### [](#client-js-客户端 "client.js 客户端")client.js 客户端

项目下的client.js是客户端的执行文件，用于创建客户端的Socket服务。将用户输入的内容按特定的格式组成数据结构发给服务端，同时打印出服务端指定的内容。  
**具体代码如下：**

1  
2  
3  
4  
5  
6  
7  
8  
9  
10  
11  
12  
13  
14  
15  
16  
17  
18  
19  
20  
21  
22  
23  
24  
25  
26  
27  
28  
29  
30  
31  
32  
33  
34  
35  
36  
37  
38  
39  
40  
41  
42  
43  
44  
45  
46  
47  
48  
49  
50  
51  
52  
53  
54  
55  
56  
57  
58  
59  
60  
61  
62  
63  
64  
65  
66  
67  
68  
69  
70  
71  
72  
73  
74  
75  
76  
77  
78  
79  
80  
81  
82  
83  
84  
85  
86  
87  
88  
89  
90  
91  
92  
93  
94  
95  
96  
97  
98  

// Socket聊天室 - 客户端 client  
const net = require('net');  
const readline = require('readline');  
const rl = readline.createInterface(process.stdin, process.stdout);  
  
rl.question('请输入聊天昵称：', (nickname) => {  
 nickname = nickname.trim();  
 if (!nickname) {  
 throw new Error('昵称不能为空！');  
 }  
  
 // 创建与服务端的连接  
 // 设置正确的服务端的ip地址和端口  
 let server = net.connect({port: 2018, host: '127.0.0.1'}, () => {  
  
 // 登入操作  
 let user = {  
 protocol: 'signin',  
 from: nickname  
 };  
  
 // 往服务端传送数据  
 server.write(JSON.stringify(user));  
 console.log(\`【系统通知】已成功加入聊天室，尽情畅聊吧~\`);  
  
 // 监听服务端发送过来的数据  
 server.on('data', (chunk) => {  
 try {  
 let serverDataContent = JSON.parse(chunk.toString().trim());  
 let protocol = serverDataContent.protocol;  
 switch (protocol) {  
 case 'online':  
 console.log(\`\\n【系统通知】欢迎：${serverDataContent.online}，加入聊天室，当前在线人数：${serverDataContent.onlineCount}\\n\`);  
 rl.prompt();  
 break;  
 case 'offline':  
 console.log(\`\\n【系统通知】${serverDataContent.offline}下线了，当前在线人数：${serverDataContent.onlineCount}\\n\`);  
 rl.prompt();  
 break;  
 case 'broadcast':  
 console.log(\`\\n\[@所有人\] ${serverDataContent.from}\> ${serverDataContent.message}\\n\`);  
 rl.prompt();  
 break;  
 case 'p2p':  
 console.log(\`\\n\[@${serverDataContent.to}\] ${serverDataContent.from}\> ${serverDataContent.message}\\n\`);  
 rl.prompt();  
 break;  
 default:  
 server.write('错误！未能识别的通信协议！');  
 break;  
 }  
 } catch (error) {  
 server.write('出现错误了哦~');  
 throw error;  
 }  
  
 });  
  
 rl.setPrompt(nickname + '> ');  // 此时没有写入控制台  
 rl.prompt(); // 写入控制台  
  
 // 输入一行内容敲回车  
 rl.on('line', (line) => {  
 line = line.toString().trim();  
  
 // 内容： user1:我只跟你说话  表示，客户端用户只跟user1通信  
 // 根据客户端用户输入的内容按“:”分割成两部分  
 let arrString = line.split(':');  
 let sendServerData = null;  
  
 // 组成往服务端发送的数据格式  
 if (arrString.length === 2) {  
 // 点对点  
 sendServerData = {  
 protocol: 'p2p',  
 from: nickname,  
 to: arrString\[0\],  
 message: arrString\[1\]  
 };  
 } else {  
 // 广播消息  
 sendServerData = {  
 protocol: 'broadcast',  
 from: nickname,  
 message: line  
 };  
 }  
  
 // 往服务端发送数据  
 server.write(JSON.stringify(sendServerData));  
 rl.prompt(); // 写入控制台  
 });  
  
 rl.on('close', () => {  
  
 });  
 });  
});  

##### [](#实现功能 "实现功能")实现功能

目前已实现：客户端广播消息通信、点对点通信。

##### [](#项目使用 "项目使用")项目使用

1.  启动服务端，`$ node server.js`  
    ![启动服务端](https://user-images.githubusercontent.com/24516169/36706538-325fc724-1ba5-11e8-8596-08b28f52ecc2.jpg)
2.  启动第一个客户端，`$ node client.js`  
    ![启动客户端](https://user-images.githubusercontent.com/24516169/36706534-27154c7c-1ba5-11e8-8c36-f0478296563d.jpg)
3.  启动第二个客户端，`$ node client.js`
4.  ……

##### [](#演示视频 "演示视频")演示视频

![Demo演示gif](https://user-images.githubusercontent.com/24516169/36706486-f24b8e48-1ba4-11e8-8627-f236d924330d.gif)

##### [](#项目下载 "项目下载")项目下载

本案例源代码托管于GitHub，下载：[传送门](https://github.com/XPoet/node.js-chartroom)