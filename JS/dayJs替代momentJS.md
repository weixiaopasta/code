#### 安装
```
npm install dayjs --save
```


```
import * as dayjs from 'dayjs'
// import * as isLeapYear from 'dayjs/plugin/isLeapYear' // import plugin 闰年插件
// import 'dayjs/locale/zh-cn' // import locale
//  import customParseFormat from 'dayjs/plugin/customParseFormat';
// dayjs.extend(isLeapYear) // use plugin
// dayjs.locale('zh-cn') // use locale
// dayjs.extend(customParseFormat)
var day = dayjs()
console.log(day.format('YYYY/MM/DD HH:mm:ss')) //2021/09/24 11:28:11
```

#### 文档

https://dayjs.fenxianglu.cn/category/