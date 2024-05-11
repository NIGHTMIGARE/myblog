****node特点 优缺点

事件循环调用机制 异步io
优点 
可以处理高并发的io
事件驱动
可以根据进入队列的顺序 高并发处理事件

node是单线程 不适合单核cpu io密集型应用
代码发生问题 容易整个项目崩溃


node应用场景

node更适合io异步的操作
并发的io 内部不需要非常复杂的逻辑
websock

node可用来
后台管理系统 实时交互系统 高并发
web canvas多人游戏
ws 实时多人聊天互动平台
db操作 封装request请求 基于json的api方式
单页面浏览器应用创建服务

node fs模块
fs filesystem
通过node 操作文档文件夹
const fs = require('fs') 
通过sync同步操作
通过cli创建模板 根据用户交互选择结果 vue-cli 创建router vuex creat eslint babels




文件拷贝实现
copyFile
copyFileSync
同步
fs.copyFileSync('1.txt','2.txt')
fs.copyFileSync()
异步
fs.copyFileSync('1.txt','2.txt'，(err)=>{

})
创建目录
mkdirSync
fs.mkdirSync('a/b/c') 没有返回值 保证ab路径存在 没找到路径创建不了 会抛出异常 trycatch

fs.mkdir('a/b/c',(err)=>{

})


buffer
定义 二进制文件
buffer 内存开辟空间 存储二进制的数据 缓冲
js
const buffer =Buffer.from('123') buffer类型不可读 需要.toString去读
'123' -> 16进制 -》buffer一个字节
const b1 =Buffer.from('12')
const b2 =Buffer.alioc(10,1) 创建大小是10个字节的缓冲区 所有值以1填充 10个字节 每个字节值为1
buffer的解码编码不一样会乱码
utf-8
ascli
base64
hex 两个16进制的字符
buffer是临时文件 是缓存 可以作为请求的临时处理优化性能

jwt鉴权机制
json web token字符串规范 用户 服务器传可靠信息
服务器验证客户身份没问题后会发token
用户访问用过token access鉴权
token 三部分 通过.关联起来
头部 header 声明算法
{
    alg :'h5356',
    type:'JWT'
    }
base64编码

负载 payload

{
    sub：'lin',
    time:1236
}
base64编码
签名 signature
{
    secretkey=h5356(base64Url(header))+.base64Url(payload).secretkey
}

token 生成 验证
1 登录成功生成token
jswebtoken
token 本地缓存
axios放在请求头里

2 请求资源验证token
koa-jwt
app.use(koa.jwt({
secret: 'test-token'
}).unless({
'path':[/api/login] 不校验
}))
jwt
优缺点
json 通用性 服务端不用存储
保护好加密密钥
payload简单加密 不能存储敏感信息



node实现分页

m-n条数据
totalcount:998
total page:89
currentPage:1
data；[]

const start =(page-1)*pagesize
const sql ='SELECT * FROM record limit ${pagesize} OFFSET ${start}'
start -> start +pagesize
node stream 流  数据传输手段 端到端的信息交互方式
借助buffer作为数据单位
不需要完整读取文件 不是完整将文件存在内存中再处理 逐块读取
source deft pipe
pipe 管道
source.pipe(dest) 借助pipe 数据从source流向dest
单向传输数据结构 要判断上次从哪传输 传输了哪些  下次接着传

stream 种类 
四种流式结构
可写流
fs.createWriteStream
可读流
fs.createReadStream
双工流 能读能写
net.Socket
转换流 在数据写入 读取时修改数据的流 可以做解压

http
 request 可读流
 response 可写流
 websocket双向流
	 const { Duplex} =require('stream')
	 const demo = new Duplex({
	read size(){}
	write(chunk,encoding,cb){
	} 
	 })

stream 应用场景
操作 http fs
分段的io操作 数据像水管一样流动 pipe
get返回文件给客户端 stream返回
const server=http.createServer(function (req,res){
const method =req.method;
if(method === 'GET'){
const filename =path.resolve(_dirname,'data.json');
let stream =fs.createReadStream(filename);
stream.pipe(res)
}
})
server.listen(8000)

process 全局变量
控制node进程

启动一个js文件 开启一个服务进程 有独立内存空间处理 其他进程不干扰不同进程通信才能数据共享
js 执行线程 渲染线程同时只能执行一个 js是单线程
node运行一个js文件  服务只有一个主进程 进程断了需要进程保护

process.env 环境变量
process.nexttick eventloop 事件循环中使用下一次事件循环
process.pid 当前进程的id ppid 当前进程的父进程的id
process.cwd( ) 当前进程的执行目录
process.stdout()  stdin stderr标准输出 输入 错误输出

cwd()
a npm b包含a
a操作b模块的目录
a process.cwd()
process.argv 当前执行命令入参 返回数组
const args = process.argv.slice(2)
0 node路径 1 js文件路径 2 真实参数从2开始

node  eventemitter node执行的基本
node异步io 通过事件驱动执行
事件驱动的基础
node很多对象都会进行事件分发
fs.readStream  触发eventEmitter.on()
提供events类 观察者模式
观察其他对象派过来的事件
主题=被观察者 维护 其他对象派发过来的 观察者
```
const EventEmitter = require('events')
 class DemoEmitter extends EventEmitter()
 const demoEmitter =new DemoEmitter()
 function cb(){
 }
demoEmitter.on('event',cb)
demoEmitter.emit('event')
eventemitter如何实现

 class EventEmitter={
	constuctor(){
		this.event={
		'event1':[cb1,cb2],
		'event2':[cb3]
		}
	}
	emit(type,...args){
		this.event[type].forEach(item=>{
		Reflect.apply(item,this,args)})
	}
	on(type,handler){
		if(!this.events[type]){
			this.events[type]=[]
		}
		this.events[type].push(handler)
	}
	addListener(type,handler){
		this.on(type,handler)
	}
	prependListener(type,handler){
		if(!this.events[type]){
			this.events[type]=[]
		}
		this.events[type].unshift(handler)
		}
	removeListener(type,handler){
		if(!this.events[type]){
			return
		}
		this.events[type]=this.events[type].filter(item=>{item!==handler})
	}
	off(type,handeler){
		this.removeListener(type,handler)
	}

}
```
 

中间件 middleware 衔接涵盖其他部分 能力扩展
http  express koa
本质 回调函数 参数包含请求对象
开始 进入node模块 通过middleware1 (ctx) 《--->middleware2 (ctx) 《--->业务逻辑
通过层层 middleware先进后出 栈
使用
封装一个中间件
koa 通过middleware支持
洋葱模型
（ctx,next）
'ctx' :request response
next 执行下一个中间件的函数
```
app.use

```
每个中间件 足够清晰 职责单一


node事件循环流程每个阶段都会执行对应的队列
process。nexttick 上一阶段和下一阶段的过度 插队逻辑
微任务nexttick
宏任务 
timer queue：settimeout  setinterval  
poll queue 轮询io操作
check queue： setimmediate
close queue: socket.end close

执行顺序
nexttick queue
其他微任务queue
timer的queue
poll的queue
check的queue
close的queue

面试题
```
async function async1(){
	console.log('async 1 start')
    await async2()
	console.log('async 1 end') 
}
async function async2(){
	console.log('async 2 ')   
}
console.log('script start') 
setTimeout(function(){
	console.log('settime0') 
},0)                       
setTimeout(function(){
	console.log('settime2')  
},200)                    
setImmediate(()=>{console.log('setimmediate')}) 
process.next(()=>{console.log('next1')})      
async1()
process.next(()=>{console.log('next2')})    
new Promise((resolve)=>{
	console.log('promise1') 
	resolve()
	console.log('promise1')
}).then(()=>{
	console.log('promise3') 
})
console.log('script end')
```
先找同步任务console.log('script start')
nexttick 微任务队列
其他微任务队列
timer的队列
poll的队列
check的对
close的队列


console.log('script start') 
console.log('async 1 start')
console.log('async 2 ')   
console.log('async 1 end') 
console.log('promise1')
console.log('promise2')
console.log('script end')
console.log('next1')
console.log('next2')
console.log('promise3') 
	//同步任务 微任务
	执行timer
	settime0
	setimmediate
	settime2


timer队列 console.log('settime0')  console.log('settime2')  
check console.log('setimmediate')
等插队console.log('next1'）

node性能
指标
CPU负载 在某个时间段内 占用和等待cpu的进程总数
cpu使用率 1- cpu的空余时间/cpu总时间
cpu占用过高 同步操作很多 容易阻塞异步回调

内存 通过堆栈的使用方式获取
```
const os = require('os')
const{rss,heapUsed.heapTotal}=process.memoryUsage()
// rss 表示node进程占用的内存总量。
// heapUsed  实际堆内存的使用量。
// heapTotal 堆的总大小，包括3个部分
const sysFree=os.freemem()
// 获取系统空闲内存
const sysTotal=os.totalmem()
// 获取系统总内存
heapused/heaptotal node堆内存的占用率
rss/systotal node占用系统内存比例

```
磁盘io 开销昂贵
，硬盘 IO 花费的 CPU 时钟周期是内存的 164000 倍。
内存io比磁盘io快很多
通过redis memcached 缓存实现内存io
easy-monitor 轻量内核性能监控分析工具
```
const easyMonitor = require ('easy-monitor')
easyMonitor('项目名称')
12333端口
```
node 性能优化
使用最新版本的nodejs node升级会针对v8引擎和源码优化
正确使用流 大文件传输 不通过完整文件存取 通过流发送 不存在内存里
```
http.createServer(function(req,res){
	const stream =fs.createReadStream(_dirname+'data.txt'_)
	stream.pipe(res)
})

```
代码优化
查询多次response结果 合并重复操作
```
const user_account_map={}
user.account.find(id in ids).forEach(account=>{
user_account_map[id]=account
})
for id in ids 
account =user_account_map[id
```
内存管理

文件上传
对请求头设置 content-type=multipart/form-data  
```
const fs = require('fs'); const readableStream = fs.createReadStream('path/to/file');
const writableStream = fs.createWriteStream('path/to/destination');
readableStream.pipe(writableStream);

```
koa-multer




