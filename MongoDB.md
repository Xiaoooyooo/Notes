# MongoDB



## 数据库

+ 数据库的服务器

  ​	保存数据

+ 数据库的客户端

  ​	操作服务器，对数据进行增删改查



## 配置过程

##### 1.启动mongdodb服务器	命令行输入mongod

​	mongoDB默认会检查目录 c:\data\db 是否存在，若不存在则会报错

​	手动指定数据库目录与端口：在命令后面加上参数 --dbpath 绝对路径 --port 端口

##### 2.启动mongodb客户端	另开一个命令行，输入mongo链接数据库



## 将MongoDB设置为系统服务 ---待完善

1.创建log文件夹

2.在安装目录下创建一个配置文件 **mongod.cfg** ,在其中输入以下内容：

> systemLog:
> 	destination: file
> 	path: D:\MongoDB\log\mongod.log
> storage:
> 	dbPath: D:\MongoDB\data\db

3.以管理员身份打开命令行，执行如下命令：

> sc.exe create MongoDB binPath= "\\"D:\MongoDB\bin\mongod.exe\\" --service --config=\"D:\MongoDB\mongod.cfg\\"" DisplayName= "MongoDB" start= "auto"



## 三个概念

**1.数据库**

​	用于存放集合

**2.集合**

​	类似于数组，其中可以存放文档

**3.文档**

​	数据库的最小单位，我们储存和操作的内容都是文档
​	在MongoDB中，数据库和集合都不需要手动创建
​		当创建文档时，若文档所在的集合和数据库不存在则会自动创建数据库和集合



### 基本指令

+ show dbs/databases
  + 显示当前的数据库
+ use [数据库名]
  + 进入到指定数据库
+ db
  + 显示当前所处的数据库
+ show collections
  + 显示当前数据库中的集合
+ db.[集合名].drop()    删除当前集合
+ db.dropDatabase()    删除当前数据库



### CRUD操作

+ 插入

  + db.[集合名].insert([文档])	向当前集合中插入**一个或多个**文档
    db.[集合名].insertOne([文档])	向当前集合中插入**一个**文档
    db.[集合名].insertMany([文档])	向当前集合中插入**多个**文档
    向集合中插入文档时如果没有指定_id属性，则数据库会自动添加该属性
    该属性用来作为文档的唯一标识

    ```js
    //向当前数据库的stus集合中插入一条数据
    db.stus.insert({name:"name",age:20,gender:"male"})
    
    //向当前数据库的stus集合中插入多条数据
    db.stus.inert([
        {name:"Tom",age:20,gender:"male"},
        {name:"Mike",age:21,gender:"male"}
    ])
    ```

    

    

+ 查询

  + db.[集合名].find({})	查询集合中所有符合条件的文档
+ db.[集合名].find({name:"Mike"})	查找name为Mike的所有文档
  + db.[集合名].findOne({})	查询符合条件的第一个文档
  **注：**find() 返回的数据是数组，findOn() 返回的是对象
    		db.[集合名].find().count()	计算查询结果的个数

+ 修改

  + db.[集合名].update(查询条件, 新对象[, 配置项])	默认只会修改一个

  + db.[集合名].updateMany(查询条件, 新对象)	修改所有符合条件的文档

    ```js
    //将name为Mike的文档更新为新对象，**注：**该方法默认情况下旧的文档会被完全替换
    db.stus.update({name:"Mike"},{age:25})
    
    //仅更新指定属性
    db.stus.update({name:"Mike"},
    	{	//将搜索结果的指定属性进行更新
        	$set:{ name:"Jack", age:27 },
        	//删除搜索结果的指定属性
    		$unset:{ name }
    	})
    ```



+ 删除

  + db.[集合名].remove(查询条件[, justOne?])	默认删除符合条件的所有文档，**注：**如果传入的查询条件是一个空对象，则会删除集合中所有文档

  + db.[集合名].deleteOne()    删除一条文档

  + db.[集合名].deleteMany()    删除多个文档

    ```js
    //删除集合中所有文档
    db.[集合名].drop()
    //删除符合条件的所有文档
    db.stus.remove({name:"Tom"})
    //删除符合条件的第一条文档
    db.stus.remove({name:"Tom", true})
    ```

    

