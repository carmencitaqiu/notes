1.手动创建数据目录
2.启动mongoDB服务器
cd C:\Program Files\MongoDB\Server\3.0\bin
mongod.exe --dbpath d:\data\db
将MongoDB服务器作为Windows服务运行

mongod.exe --bind_ip 10.10.11.173 --logpath "d:\data\dbConf\mongodb.log" --logappend --dbpath    "d:\data\db" --port 8090"
--serviceName carmen --serviceDisplayName carmen --install

修复
mongod --dbpath C:\Program Files\MongoDB\Server\3.0\data

1.文档
文档是mongodb中数据的基本单元，行有唯一标识的字段，比如oracle就有隐藏存在的rowid。标识集合中的一条记录，即集合中的一个对象，文档有唯一的标识"_id",数据库可自动生成，可类比oracle的rowid。
2.集合
集合在mongodb中是一组文档，类似关系型数据库中的数据表，表中的数据行列数、列的类型都是一样，mongodb数据库不是关系型数据库，没有模式的概念。集合中的文档可以是不同形式的。比如：
{"name":"jack","age":19}
{"name":"wangjun","age":22,"sex":"1"}
 集合是由唯一的命名来标识，满足以下条件的任意UTF-8字符串：
    集合名不能使空字符串""
    集合名不能含有\0字符（空字符），这个字符标识集合名的结尾
    集合名不能以"system."开头，该为系统集合保留的前缀
    用户创建的集合名字不能含有保留字符$
3.数据库
mongodb以文档的形式保存在集合中，可以同一数据库存储不同的数据或者集合，即DB2、oracle、teradata等都可以存储在同一个数据库中。最近做的项目就可以将这三者数据库的数据都保存到同一数据库中。在安装数据库的时候数据库实例创建，同时存在系统默认的管理员用户。之后可以创建多个用户并进行赋权，创建的表存在于不同的用户之下，不同的用户存储着不同的数据。





