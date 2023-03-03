https://www.cnblogs.com/niuyaomin/p/11574773.html

<img src="https://img2018.cnblogs.com/blog/1748308/201909/1748308-20190923205625573-1184857145.jpg">

### JavaScript fromCharCode() 方法

将 Unicode 编码转为一个字符:
fromCharCode() 可接受一个指定的 Unicode 值，然后返回一个字符串。

注意：该方法是 String 的静态方法，字符串中的每个字符都由单独的 Unicode 数字编码指定

语法
String.fromCharCode(n1, n2, ..., nX)
参数值
参数	描述
n1, n2, ..., nX	必需。一个或多个 Unicode 值，即要创建的字符串中的字符的 Unicode 编码。

返回值
类型	描述
String	返回代表 Unicode 编码的字符。


### parseInt

parseInt(string, radix)   解析一个字符串并返回指定基数的十进制整数， radix 是2-36之间的整数，表示被解析字符串的基数。

#### 语法
parseInt(string, radix);
#### 参数
string
要被解析的值。如果参数不是一个字符串，则将其转换为字符串(使用  ToString 抽象操作)。字符串开头的空白符将会被忽略。
radix 可选
从 2 到 36，表示字符串的基数。例如指定 16 表示被解析值是十六进制数。请注意，10不是默认值！文章后面的描述解释了当参数 radix 不传时该函数的具体行为。

#### 返回值

从给定的字符串中解析出的一个整数。 或者NaN（第一个非空字符不能转换为数字。）


#### 描述
parseInt函数将其第一个参数转换为一个字符串，对该字符串进行解析，然后返回一个整数或 NaN。

如果 radix 是 undefined、0或未指定的，JavaScript会假定以下情况：
```
如果输入的 string以 "0x"或 "0x"（一个0，后面是小写或大写的X）开头，那么radix被假定为16，
字符串的其余部分被当做十六进制数去解析。
如果输入的 string以 "0"（0）开头， radix被假定为8（八进制）或10（十进制）。
具体选择哪一个radix取决于实现。
ECMAScript 5 澄清了应该使用 10 (十进制)，但不是所有的浏览器都支持。
因此，在使用 parseInt 时，一定要指定一个 radix。
如果输入的 string 以任何其他值开头， radix 是 10 (十进制)。
```

如果第一个字符不能转换为数字，parseInt会返回 NaN。

#### 注意
为了算术的目的，NaN 值不能作为任何 radix 的数字。你可以调用isNaN函数来确定parseInt的结果是否为 NaN。如果将NaN传递给算术运算，则运算结果也将是 NaN。


#### 知识点

要将一个数字转换为特定的 radix 中的字符串字段，请使用 thatNumber.toString(radix)函数。


####  JavaScript charCodeAt() 方法

charCodeAt() 方法可返回指定位置的字符的 Unicode 编码。这个返回值是 0 - 65535 之间的整数。

方法 charCodeAt() 与 charAt() 方法执行的操作相似，只不过前者返回的是位于指定位置的字符的编码，而后者返回的是字符子串。


#### 语法
stringObject.charCodeAt(index)
参数	描述
index	必需。表示字符串中某个位置的数字，即字符在字符串中的下标。

**注释：字符串中第一个字符的下标是 0。如果 index 是负数，或大于等于字符串的长度，则 charCodeAt() 返回 NaN。** 

#### JavaScript toString() 方法

数字的字符串表示。例如，当 radix 为 2 时，NumberObject 会被转换为二进制值表示的字符串。

语法
number.toString(radix)
参数值
```
参数	描述
radix	可选。规定表示数字的基数，是 2 ~ 36 之间的整数。若省略该参数，则使用基数 10。
但是要注意，如果该参数是 10 以外的其他值，则 ECMAScript 标准允许实现返回任意值。
2 - 数字以二进制值显示
8 - 数字以八进制值显示
16 - 数字以十六进制值显示
```

#### 字符串(汉字)转换为十六进制
ps: 主要使用字符串.charCodeAt()方法,此方法返回一个字符的Unicode值,再用toString(16)方法,该方法是先将数字对象转换为二进制,再把二进制转化为16进制.

```
var str = "牛耀民";
var val = "";
for(var i = 0; i < str.length; i++){
    if (val == "")
        val = str.charCodeAt(i).toString(16);
    else
        val += "," + str.charCodeAt(i).toString(16);
}
console.log(val);
VM131:9 725b,8000,6c11
```


#### 十六进制转化为字符串(汉字)

　主要使用Object.fromCharCode()方法,此方法将Unicode码转换为与之对应的字符.先将字符转化为数字,parseInt(string,radix)实现,由于这些字符都是十六进制对应的字符,所以radix也应为16

```
var str = "725b,8000,6c11";
var val="";
var arr = str.split(",");
for(var i = 0; i < arr.length; i++){
　　val += String.fromCharCode(parseInt(arr[i],16));
}
console.log(val); 
VM165:7 牛耀民
```