#express框架


##1.路由

![](http://i.imgur.com/Pf06qIO.png)

##示例代码：

![](http://i.imgur.com/Tooecb3.png)




##2.应用

	app.get('/', function(req, res){res.end()})
		匹配根目录优先去public中寻找



##3.路由分解介绍：

![](http://i.imgur.com/gCQR7E0.png)


##4.路由方法

```
	app.get('路径'， function(req, res, next){});
	app.post('路径'， function(req, res, next){});
	app.all('路径'， function(req, res, next){})		post和get都能处理
```

##5.路由路径；

	字符串，对应的文件路径
	最后兜底可用
```
		app.all('*', function(req, res){res.end('404,没找到')})
```

next()函数的使用：每个路由可绑定多个回调函数，调用next()后会调用下一个回调函数，当前请求端点最后一个函数还继续调用的话，会出去继续匹配符合路径的路由，直到app.all。如果调用next(‘route’)，则直接跳出当前的路由去匹配下边符合条件的路由。


##6.响应方法：

```
	res.write()
	res.end()			只能接受字符串和buffer
	以上两个是node js原生的API

	res.send(obj)		可直接发送obj
		res.send()是node js包装的，发送obj时，其内部自动检查，并调用了json.stringify(),将obj转化为了字符串返回信息

	res.render()		模板渲染，动态生成html代码并发送给浏览器
		此函数接受两个函数。一个模板文件，一个对象，是需要更改的信息


	res.redirect()		接受静态资源的路径，重定向到该资源
```

##7.重定向：

```
	res.redirect ('静态资源路径')		重定向到静态资源下的页面
```


##8.app下的use()和all()

  示例代码：
```
		app.all('/', function(req, res){res.end('all***')}, next())
		app.use('/', function(req, res){res.end('use***')}, next())

	1)二者表示的都是匹配所有，不同的是use()第一个参数不写路径是表示默认匹配'/'
	2)all()是完全匹配，也就是路径要完全一样，use()是前导匹配，匹配的是根目录开始的路径！！！
		即（匹配顺序从  '/'---->'/aaa'---->'/aaa/bbb'--->'/aaa/bbb/ccc'.........）
```


##9.路由模块化

	1）在routes下建立相关的路由文件

	2)暴露ruotes下新建的文件接口
		示例代码：
			var express = require('express');		引入express模块
			var router = express.Router();			调用一个方法创建一个对象

			{中间写相关路由}

			但是在app中调用的app.get/post('/aaa/bbb', function(req, res, next){})不可用
				此文件中应该用新生成的router替换app
					

			module.exports = router					暴露一个对象

	
	3）在app.js中引入路由文件
		示例代码： var xiyou = require(./route/xiyou.js)
					 
	4）在app调用use()函数使用文件
		示例代码：	app.use('/', xiyou);			此时第二个参数不是回掉函数，换成引入的文件


注意：use()函数在匹配路径时会吃掉除根目录以外的字符串，所以在routes内的路由文件路径中不应该再有use中的路径。即：use中的路径 + routes中路由文件路径 = 浏览器发送的请求路径 


##10.express模板引擎（ejs/嵌入式的js）

	动态生成html的工具，原理是模板 + 数据 = html

	1）	安装：npm install ejs --save

	2)	在views文件下下制作ejs文件（把需要使用的html文件复制并改后缀为ejs）

	3）	路由中返回数据用res.render('aa.ejs'. {username: 'aaaaa'})
		参数：第一个参数为要使用的模板的名字，因为是放在views下，直接写文件名字
			 第二个参数是所要更改的信息，以对象形式上传

	4)	模板语言：
		<%=  js代码  %>	 替换html中指定位置的内容，用对象中的名替换后显示对象的值
		<%  js代码   %>	 在html指定位置插入要执行的js代码

	5）	在ejs文件中相应的位置修改内容（即用<% %>或<%= %>修改）

	6）include使用
		把相同的代码抽取，单独放在一个文件内引入，当内容发生改变时只需要改某个文件
		1.在views文件夹下创建ejs模板

		2.在要使用的用的模板内添加<%- include('要引用的ejs文件名')%>


##11.express中的download

	app.get('/////', function(req, res, next){
		res.down('路径'， 'filename',[function(err){if(err){}else{}}])
	
	})

		

	
