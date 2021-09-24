https://blog.csdn.net/wenwen360360/article/details/78339221
mongodb的数据更新命令$set--- 数据更新操作符相关  





#### mongoose 更新修改数据: findOneAndUpdate 的使用

基本语法
Model.findOneAndUpdate([(conditions, doc, [options], [callback])]

#### conditions
第一个参数是一个对象参数，是用于查询与之相匹配的数据用的
####  doc
第二个参数也是一个对象参数，用于修改查询到的数据中的某条信息
####  options
第三个参数也是一个对象参数，主要用于设定匹配数据与更新数据的一些规定，比较复杂，一般用不到
####  callback
第四个参数也就是我们最熟悉的回调函数，函数默认传入两个参数,err、data。当数据库发生错误的时候传回一个err，若数据库正常，err为空；当正常根据第一个参数查询到相关数据并成功修改了我们设定的数据，data返回修改前的数据信息，若根据第一个参数没有查询到相关数据，data为null
#### 　Model.findById(id, [fields], [options], [callback])

这个还是比较常用，要据ID得到数据！　　

#### Model.findByIdAndUpdate()

注意如果不想更新之后的返回值为旧值，请把options中的new参数显示的设置为true

In Mongoose 4.0, the default value for the new option of findByIdAndUpdate (and findOneAndUpdate) has changed to false, which means returning the old doc (see #2262 of the release notes). So you need to explicitly set the option to true to get the new version of the doc, after the update is applied:
```
Model.findByIdAndUpdate(id, updateObj, {new: true}, function(err, model) {...}
```

#### Model.findByIdAndRemove(id, [options], [callback])　　　　　　

#### Model.findOneAndRemove(conditions, [options], [callback])

#### Model.find(conditions, [fields], [options], [callback])

第2个参数可以设置要查询输出的字段
```
var User = require("./user.js");

function getByConditions(){
    var wherestr = {'username' : 'Tracy McGrady'};
    var opt = {"username": 1 ,"_id": 0}; // 1输出，0 不输出
    
    User.find(wherestr, opt, function(err, res){
        if (err) {
            console.log("Error:" + err);
        }
        else {
            console.log("Res:" + res);
        }
    })
}

getByConditions();
```

输出只会有username字段，设置方法如上，1表示查询输出该字段，0表示不输出

比如我要查询年龄范围条件应该怎么写呢？

```
User.find({userage: {$gte: 21, $lte: 65}}, callback);    //这表示查询年龄大于等21而且小于等于65岁
```
```
　其实类似的还有：　

　　$or　　　　或关系

　　$nor　　　 或关系取反

　　$gt　　　　大于

　　$gte　　　 大于等于

　　$lt　　　　 小于

　　$lte　　　  小于等于

　　$ne            不等于

　　$in             在多个值范围内

　　$nin           不在多个值范围内

　　$all            匹配数组中多个值

　　$regex　　正则，用于模糊查询

　　$size　　　匹配数组大小

　　$maxDistance　　范围查询，距离（基于LBS）

　　$mod　　   取模运算

　　$near　　　邻域查询，查询附近的位置（基于LBS）

　　$exists　　  字段是否存在

　　$elemMatch　　匹配内数组内的元素

　　$within　　范围查询（基于LBS）

　　$box　　　 范围查询，矩形范围（基于LBS）

　　$center       范围醒询，圆形范围（基于LBS）

　　$centerSphere　　范围查询，球形范围（基于LBS）

　　$slice　　　　查询字段集合中的元素（比如从第几个之后，第N到第M个元素）
```


####   分页查询

```
var User = require("./user.js");

function getByPager(){
    
    var pageSize = 5;                   //一页多少条
    var currentPage = 1;                //当前第几页
    var sort = {'logindate':-1};        //排序（按登录时间倒序）
    var condition = {};                 //条件
    var skipnum = (currentPage - 1) * pageSize;   //跳过数
    
    User.find(condition).skip(skipnum).limit(pageSize).sort(sort).exec(function (err, res) {
        if (err) {
            console.log("Error:" + err);
        }
        else {
            console.log("Res:" + res);
        }
    })
}

getByPager();
```

#### Model.update(condition,doc,[options],[callback]);
```
参数condition：更新的条件，要求是一个对象。
参数doc：要更新的内容，要求是一个对象。
参数[options]：可选参数，要求也是一个对象。
参数[callback]：可选参数，要求是一个回调函数。

[options]有效值：
safe ：（布尔型）安全模式（默认为架构中设置的值（true））
upsert ：（boolean）如果不匹配，是否创建文档（false）
multi ：（boolean）是否应该更新多个文档（false）
runValidators：如果为true，则在此命令上运行更新验证程序。更新验证器根据模型的模式验证更新操作。
setDefaultsOnInsert：如果这upsert是真的，如果创建了新文档，猫鼬将应用模型模式中指定的默认值。该选项仅适用于MongoDB> = 2.4，因为它依赖于MongoDB的$setOnInsert操作符。
strict：（布尔）覆盖strict此更新的选项
overwrite： （布尔）禁用只更新模式，允许您覆盖文档（false）
```
作用：该方法是根据condition这个条件去更新doc这个对象。
例子：
```
MyModel.update({ age: { $gt: 18 } }, { oldEnough: true }, fn);
```
//匹配年龄大于18岁的那条数据，更新它的oldEnough值为true
```
MyModel.update({ name: 'Tobi' }, { ferret: true }, { multi: true }, function (err, raw) {
  if (err) return handleError(err);
  console.log('The raw response from Mongo was ', raw);
});
```
//把name值为“Tobi”的那个文档中的数据的ferret值更新为true;{multi:true}表明是要更新多个文档，也就是更新所有匹配name值的文档中的ferret值