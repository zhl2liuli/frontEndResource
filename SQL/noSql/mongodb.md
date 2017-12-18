mongoDB
### shell基本操作
```text
  1.创建数据库
    use [databaseName]
  2.查看所有数据库
    show dbs
  3.给指定数据库添加集合并记录
    db.[documentName].insert({...})
  4.查看数据库中的文档
    show collections
  5.查询制定文档中的数据
    db.[documentName].find()
    db.[documentName].findOne()
  6.更新文档数据
    db.[docementName].update({查询条件}，{更新内容}) ==> 覆盖
    db.[docementName].update({查询条件}，{$set:{更新内容}}) ==> 更新复合条件的
  7.删除文档中的数据
    db.[documentName].remove({...})
  8.删除集合
    db.[documentName].drop()
  9.删除数据库
    db.dropDatebase()
  10.shell的help
    有全局db.help()和数据库相关的db.[documentName].help()
  11.mongoDBAPI
    [API](http://api.mongodb.com/)
  12.命名规范
    不能是空字符串
    不得含有'' 空格 , $ / \ 和\O
    应全部小写
    最多64个字节
    不能与系统保留库相同
  13.shell可以直接运行js代码
    shell可以运行eval
```
### 增删改查

```text
  1.插入
    db.[documentName].insert({...})
  2.批量插入
    js的for循环
  3.Save
    在_id相同时save用来保存，insert会报错
    db.[documentName].save({...})
  4.删除
    db.[documentName].remove()
  5.根据条件删除
    db.[documentName].remove({...})
  6.强硬更新
    db.[documentName].update({查询器},{修改器})
    主键_id冲突时会报错
  7.insert&update
    db.[documentName].update({...},{...},true)
    查询出来就更新，没有的话就插入操作
  8.批量操作
    查询器查询出来默认只修改第一条数据
    批量：
      db.[documentName].update({...},$set:{...}},false,true)
  9.修改器
    $set 
    $inc ==> 只针对数字类型
    $unset ==> 删除指定键
    $push ==> 指定键为数组则添加新值，若没有指定键则创建，若不是数组则报错
    $pushAll ==> {$pushAll:{key:Array}}
    $addToSet ==> 值存在则不添加，不存在则添加
    $pop ==> 值是-1 删除第一个，值是1删除最后一个
    $pull
    $pullAll
    $ ==> 定位器 key.$==> 定位在key数组的内部值
      bd.[documentName].update({"key.key1": "..."},$set:{"key.$.str":...})
    $each ==>
  10.runCommand和findAndModify函数
    runCommand可以执行mongoDB的特殊函数
    db.runCommand({
      findAndModify: processes,
      query: {...},
      sort:{排序}
      remove：
      update：{}
      new
      })
```

- 查

```text
  db.[documentName].find({条件},{指定键}) ==> 指定键值为1表示显示，0表示不查询
    条件： 
      $lt $lte $gt $gte  $ne=>!=
      $in $nin  ==> 包含或者不包含 ==>只能用在集合上
      $or=>或者
      NULL
      正则
      $not
      $all ==> key:{$all:[value1,value2]}
      index ==> key.index: value
      $size ==> key:{$size:4} ==> 数组长度
      $slice ==> key:{$slice:[n,m]}
      $elemMatch ==> 文档查询，内部对象组合必须一致
      $where ==> function
      分页与排序
        limit(n)
        Skip(n) ==> 返回指定的n跨度
        Sort({key:1/-1}) ==> 1=>升序   
  游标：find方法返回值是一个游标。可以通过游标来对最终结果进行控制
    要迭代结果可以使用游标的next方法。也可以使用hasNext来查看结果中是否还有下一个记录。
    游标可以使用foreach循环方法。
    游标销毁：
      客户端发来信息销毁
      游标迭代完毕
      游标超过10分钟没用
  快照：$snapshot
    快照通过在实时数据和指定快照卷中创建指针来工作
    快照仅存储修改的数据。
```
- 索引

```text
  db.[documentName].ensureIndex({key:1/-1},{key:索引名称) ==> key为需要建立索引的key，1=>正序
    索引会提高查询性能，但会影响插入性能
    对于经常查询少插入适合索引
    符合索引要注意索引的先后顺序
    每个键全建立索引不一定就能提高性能呢
    索引不是万能的
    在做排序工作的时候如果是超大数据量也可以考虑加上索引
    用来提高排序的性能
  唯一索引：
    db.[documentName].ensureIndex({key:1/-1},{unique:true})
  剔除重复值：
    db.[documentName].ensureIndex({key:1/-1},{unique:true,dropDups:true})
  查询指定索引：
    db.[documentName].ensureIndex({key:...}).hint({key:1/-1})
  后台执行：
    db.[documentName].ensureIndex({key:1/-1},{background:true})
  删除索引：
    db.runCommand({dropIndex:"documentName",index:"索引名字"})
  二维索引：
    db.[documentName].ensureIndex({key:"2d"},{min:n,max:m})
    db.[documentName].find({key:{$near:[n,m]}})
    db.[documentName].find({key:{"$within":{$box[[n,m],[x,y]]}}}) ==>n,m和x，y对角线的正方形
    db.[documentName].find({key:{$within:{$center:[[n,m],r}}}) ==> 以n,m为圆心r为半径的范围

```
### Count+Distinct+Group

```text
  数量：
    db.[documentName].find({...}).count()
  多少种不同种类：
    db.runCommand({
      distinct: documentName
      key:...
      ...
      }).value
  分组：
    db.runCommand({
      group{
        ns:documentName
        Key: 分组的key
        $keyf: function(doc){}
        Initial: 初始化累加器==>可以是对象
        $reduce:function(组内单个记录，累加器数据) {}
        Condition：条件
        Finalize: 完成器
      }
    })
  
```
- runCommand

```text
  启动restfullAPI：
    mongoDB安装路径 --dbpath mongo数据路径 -httpinterface -rest
    就可以在127.0.0.1:28017/_commands查看runCommand的api
  常用命令：
    buildInfo: 1 ==> 服务器和主机操作系统
    collStats: documentName ==>集合详细信息
    getLastError: documentName
    convertTocapped ==> 需要被转化成固定集合的集合

```
### 固定集合
```text
  特性：
    没有索引
    插入非常快
    查询非常快
    适合日志管理
  创建：
    db.createCollection("documentName",{size:...,capped: ...,max:...})
    db.runCommand({
      convertTocapped:documentName,
      size:...
      })
  反向排序：
    db.documentName.find().sort({$natural:-1})
  尾部游标：是个特殊游标，不会被销毁

```
### GridFS
```text
  是mongoDb的文件系统：二进制形式存储文件
  命令：
    mongofiles 
      --help ==> 查看所有命令
      -d ==> 指定数据库，默认fs
      -l ==> 文件路径
      -u -p ==> 指定用户名和密码 
      -h ==> 指定主机
      -port ==> 主机端口号
      -c ==> 指定集合名
      -t ==> 指定MIME类型 默认忽略
      put
      list
      delete
    mongofiles -d documentName -l "E:/..." put ".."
  工具：
    mongofiles.exe 
```
### 服务器脚本
```text
  eval()
    db.eval(js)
  存储
    db.system.js,insert({_id: key, value:js})
```
### 启动项

```text
  --dbpath
  --port
  --fork
  --logpath
  --config
  --auth ==> 安全认证启动数据库

```
### 导入导出

```text
  导出：
    mongoexport
      -d ==> 库
      -c ==> 表
      -o ==> 文件名
      -csv ==>
      -q ==>
      -type<json|csv|tsv> ==>
    mongoexport --host id --port 端口号 -d databaseName -c documentName -o 路径和文件名
  导入：
    mongoimport --db databaseName --collection documentName --file 路径和文件名
  运行备份：
    mongodump --host id -d databaseName -o 路径和文件名
  运行时恢复：
    mongorestore --host id:port -d databaseName -directoryperdb 路径和文件名
```
### Fsync锁

```text
  上锁：
    db.Command({fsync:1,lock:1})
  解锁：
    db.currentOp()
  数据修复：
    db.repairDatabase()
  添加用户：
    db.createUser(user,writeConcern)
      user ==> {
        user:
        pwd:
        customData:
        roles:[
          {role:<role>,db:...} | <role>
          {role:<role>,db:...} | <role>
          {role:<role>,db:...} | <role>
          ...
        ]
      }
      其中<role>:
          read,readWrite
          dbAdmin,dbOwner,userAdmin
          clusterAdmin,clusterManager,clusterMonitor,hostManager
          backup,restore
          readAnyDatabase,readWriteAnyDatabase,userAdminAnyDatabase,dbAdminAnyDatabase
          root
          _system
      writeConcern:
        w: 
        j：true==> mongod意外关闭不会丢失数据
        wtimeout：限定时间
  切换用户：
    db.auth(username,pwd)
  查看用户：
    db.system.user.find({...})
  删除用户：
    db.dropUser(username)
```
### 主从复制(replica set)

```text
  主从复制：一个简单的数据库同步备份的集群技术
    主服务器只有一台
    --master
    --slave
    --source
  主库启动配置：
    dbpath = 主库服务器数据地址
    port = 
    bind_ip = 
    master = true
  主库启动配置：
    dbpath = 从服务器数据地址
    port =
    bind_ip = 
    source = 主服务器地址和端口（也可以在shell中动态设置）
    slave = true
  注意：从服务器默认不可读写 db.getMongo().setSlaveOk()设置可读
  其他配置项：
    only  从节点指定复制某个数据库,默认是复制全部数据库
    slavedelay  从节点设置主数据库同步数据的延迟(单位是秒)
    fastsync 从节点以主数据库的节点快照为节点启动从数据库
    autoresync 从节点如果不同步则从新同步数据库
    oplogSize  主节点设置oplog的大小(主节点操作记录存储到local的oplog中)
  从服务器的local数据中的sources集中保存着主主服务器的信息
     可以
       db.sources.insert({"host":主服务器id和port}) ==>设置主服务器
       db.sources.remove({"host":主服务器id和port}) ==>删除主服务器
  replset:rs1，
  rs.initiate(cfg) ==> 初始化
    cfg = { 
      _id:"rs1", 
      members:[
        {_id:0,host:'id:port',priority:2}, 
        {_id:1,host:'id:port',priority:1},
        {_id:2,host:'id:port',arbiterOnly:true}
      ]}
  rs.status() ==> 配置结果信息

  [主从服务器例子](http://blog.csdn.net/zx13525079024/article/details/52911704)
  
```

### 分片

```text
  何时分片：
    复制所有的写入操作到主节点
    延迟的敏感数据会在主节点查询
    单个副本集限制在12个节点
    当请求量巨大时会出现内存不足。
    本地磁盘不足
    垂直扩展价格昂贵
  结构：
    Router ==> 前段路由
    Config Server ==> 配置服务器存储整个ClusterMetadata
    Shard01 Shard02 ... ==> 片块
  步骤：
    创建一个配置服务器
    创建路由服务器
      mongos --port 1000 --configdb 127.0.0.1:2000
      configdb是连接的配置服务器
    创建分片服务器
    在路由服务器中设置片区
      db.runCommand({addshard:ip:port,allowLocal:true})
      db.runCommand({addshard:ip:port,allowLocal:true})
      ...
    为某数据库打开分片功能：
      db.runCommand({"enablesharding":databaseName})
    为某集合分片：
      db.runCommand({"shardcollection":"databaseName.documentName","key":{"_id":1}}) 
      key==>也可以time分片
  查看配置库对于分片服务器的配置存储
    db.printShardingStatus()
  查看集群对bar的自动分片机制配置信息
    mongos> db.shards.find()
  [详细例子](http://www.runoob.com/mongodb/mongodb-sharding.html)
```