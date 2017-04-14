

1.引入mongoose包
	var mongoose = require('mongoose');
2.向数据库发起连接
	mongoose.connect('mongodb://192.168.1.34:27017/emp')	//emp是数据库名称，可自己定义
3.创建连接
	connection = mongoose.connection;
4.绑定响应函数
	connection.on('error',function(err){
	
		console,log(err);
	})

	connection.on('open', function(){

		console.log('服务器已连接');
	});
5.创建schema
	var schema = new mongoose.Schema（{
			
		name: String,
		age: Number,
		gender: {type:String, default:'nan'}

	}）；

6.创建model	并存写数据
	var Model = mongoose.model('表名'， '上边创建的schema');
		Model.create({符合schame格式的对象}, function(err, doc){console.error(err)});

7.创建entity
	
	var entity = new Model({符合schema格式的对象});
		entity.save()

8.增删改查
	1.增
		model层：	Model.create('biaoming', function(err, doc){})			//doc此时就是一个entity的实例，相当于new了一个entity
		
		entity层：	var entity = new Model({对象})；		entity.save(function(err, doc){})

	2.删
		model层：	Model.remove({name: ''},function(err, doc){})	
	
		entity层：	model.findById('123548',function(err, doc){})		此时在回掉函数中用doc.remove()删除

	3.改
		model层：	model.update({name: ''}, {$set:{age: 100}}, function(err, doc){})	

		entity层：	model.findById('123548',function(err, doc){

						if(err){
							console.error(err);
						}else{
			
							doc.age = 1;
							doc.save(function(err, doc){});
						}
					})

	4.查
		只能在model层查询
		model.find({查询条件}， {过滤条件}, function(err, docs){});		返回多个，切存储在数组当中

		model.findOne({查询条件}， {过滤条件}， function(err, doc){});	只返回第一个符合条件的，不存在数组中

		model.findById({_id: }, function(err, doc){})