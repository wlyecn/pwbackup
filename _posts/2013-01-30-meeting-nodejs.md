---

title: Node.js奇遇记

layout: post

---
JavaScript(JS)是一门神奇的语言，它有时候很简单，你甚至不会将它视为编程语言，有时候又很复杂，可以从前端到后端实现各种匪夷所思的功能。

JQuery, Dojo, YUI, Mootools...这些流行的前端JS框架固然美观实用，却总让我提不起兴趣，直到我邂逅了Node.js([Node][1])，传说中运行在服务端的JS, 终于让我有了重拾JS的热情。

##**GOAL**
无论是编写简单的JS代码实现数据验证，或是使用各种JS框架简化交互，JS始终扮演着客户端脚本这一角色，然而近几年，以Node为代表的JS库开始在服务端流行起来（你没有看错，这货居然可以脱离浏览器直接在服务端运行！），与传统而强大的Java和PHP不同，Node关注的是网络编程或者说服务器的请求和响应，Node这样宣称它的目标：

>Node's goal is to provide an easy way to build scalable network programs.

提供一种简单的构建可伸缩网络程序的方法，那么，传统的Java或PHP有什么问题呢？可以说问题多多，至少Node想解决它的并发问题，由于Java和PHP对每个连接都生成新线程并分配内存，并且这些线程默认情况下共享服务器上的相同资源，因此服务器所能处理的最大并发数成为网络的瓶颈。

##**NODE**
Node如何解决这个问题？它利用JS语言的[事件驱动][2]特性，将每个连接映射成为到Node引擎的一个事件，而非生成新线程分配内存。事实上，Node能在服务器上运行是由于它本质是一个由C++语言编写的JS运行时环境（Google的[V8 JavaScript引擎][3]）。

Node采用模块架构，可以利用NPM安装模块实现各式各样的功能，使用require引入模块，如使用require('http')引入自带的HTTP模块。Node的Hello World长这样:

<pre><code class="javascript">
var http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(1337, '127.0.0.1');
</code></pre>

函数式编程的神奇之处就在于可以将函数作为普通参数传递，甚至是像上面的匿名函数，这种「基于事件驱动的回调」正是Node的主要工作方式，问题是，我们对于传统的B/S交互都是Server的监听线程收到连接请求后新建一个线程处理交互，而现在我们的服务器跑在一个单线程中如何处理这些异步请求？

看看这个大神的[文章][4]，Node在创建服务器的同时向创建方法传递了一个函数，服务器在每次收到请求时都会调用这个函数。我来试着解释一下Node/JavaScript的事件驱动设计。

首先看一下什么是阻塞：

<pre><code>
var res = db.query("select * from T");
output(res);
</code></pre>

进程在执行数据库查询语句后会挂起等待查询结束才进行下一条语句，当查询过于复杂或者查询进程过多时就会造成阻塞。

基于事件驱动的设计如何实现非阻塞：

<pre><code>
db.query("select... ", function (res){
    output(res);
});
//do something else
</code></pre>

进程在查询后并不挂起等待而继续执行其他语句，而查询结束后会自动调用结果处理函数。

##**DEMO**
有关Node的教程Google老师给了很多，我照着[这本书][5]粗略学习了下，里面关于代码的组织和模块架构讲解得十分清楚。另外，JS的休眠方法很有趣：

<pre><code>
function sleep(milliSeconds) {
	var startTime = new Date().getTime();
    while (new Date().getTime() < startTime + milliSeconds);
}
sleep(10000);
</code></pre>

写了几个小Demo，仅仅用了几行代码就实现了一个高效的文件上传，感叹Node强大的同时却无法领会各种奥妙，先暂且入个门吧，函数式编程的古怪确实让我措手不及，我想我得找几本书看看了：（

[1]:http://nodejs.org/
[2]:http://en.wikipedia.org/wiki/Event-driven_programming
[3]:http://code.google.com/p/v8/
[4]:http://debuggable.com/posts/understanding-node-js:4bd98440-45e4-4a9a-8ef7-0f7ecbdd56cb
[5]:http://www.nodebeginner.org/index-zh-cn.html