#### 语法格式
那么json的语法格式到底是怎样的呢？我们先来看一段json数据：
{"name":"admin","age":18}
这就是一种最简单的json，如果有学过js的开发者是不是发现json的语法与js中object的语法几乎相同。
但是，注意：

json是一种纯字符数据，不属于编程语言
json的语法与js中object的语法几乎一致（下一部分说明不同）
json数据以键值对形式存在，多个键值对之间用逗号,隔开，键值对的键和值之间用冒号:连接
json数据在js对象的基础上做了严格化
json数据中的键值对可以使用编程语言中所谓的关键字（*见注意事项）
json的数据可以用花括号{}或中括号[]包裹，对应js中的object和array


#### 为什么说几乎相同，而不是完全相同呢？接下来我们要说的就是json与js中对象的不同点，也是json严格要求的部分：

json的键值对的键部分，必须用双引号"包裹，单引号都不行（所以如果在键中出现了关键字，也被字符化了），而js中对象没有强制要求（所以在键中不允许出现关键字）
json的键值对的值部分，不允许出现函数function，undefined，NaN，但是可以有null，js中对象的值中可以出现
json数据结束后，不允许出现没有意义的逗号,如：{"name":"admin","age":18,}，注意看数据结尾部分18的后面的逗号，不允许出现




JSON 和 JS Object 的区别
简单来说，两种没有任何关联。

JSON 语法的作者是道格拉斯（Douglas Crockford），JS 语法的作者是布兰登・艾奇（Brendan Eich）。道格拉斯发明 JSON 的时候参考了 JS 的对象语法，仅此而已。

如果硬要说区别：

1. JSON 的字符串必须用双引号。

2. JSON 无法表示 undefined，只能表示 "undefined"

3. JSON 无法表示函数

4. JSON 的对象语法不能有引用

#### 
1. JSON官网其实解释的很清楚, JSON采用完全独立于语言的文本格式, 因为易读, 易写, 易解析的特性成为理想的数据交换语言
2. 要搞清楚个这个问题, 就要明白JSON可以有哪些值, 主要有三种类型的值:
简单值(字符串, 数字, 布尔, null), 对象, 数组
所以, "null"是合法的JSON值, "1"也是合法的JSON值, 要测试也很简单, JSON.parse("null")和JSON.parse("1")都可以正确返回结果
3. JSON和JS对象应该没有什么比较性而言吧, 一个是文本格式, 一个是对象, 问题应该是JSON中的对象和JS对象的区别吧, 我姑且就按JSON中的对象和JS对象的区别回答
主要区别是,
1. JSON中的对象中属性名必须使用双引号
2. 属性值不能除了简单值, 对象, 数组以外的值

