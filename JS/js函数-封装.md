#### 1.randomNumber()

获取指定区间的随机数。
 ```           
**
 * 在最小值和最大值之间生成随机整数。
 * @param {number} min Min number
 * @param {number} max Max Number
 */
export const randomNumber = (min = 0, max = 1000) =>
  Math.ceil(min + Math.random() * (max - min));

// Example
console.log(randomNumber()); // 97 
```

####  truncate 截断
这对于长字符串很有用，特别是在表内部。
```
/**
 * 截断字符串....
 * @param {string} 要截断的文本字符串
 * @param {} 截断的长度
 */
export const truncate = (text, num = 10) => {
  if (text.length > num) {
    return `${text.substring(0, num - 3)}...`
  }
  return text;
}

// Example
console.log(truncate("this is some long string to be truncated"));  

// this is... 
```

#### softDeepClone 

这个方法是经常被用到的，因为有了它，我们可以深度克隆嵌套数组或对象。

不过，这个函数不能与new Date()、NaN、undefined、function、Number、Infinity等数据类型一起工作。

你想深度克隆上述数据类型，可以使用 lodash 中的 cloneDeep() 函数。
```
/**
 * Deep cloning inputs
 * @param {any} input Input
 */
export const softDeepClone= (input) => JSON.parse(JSON.stringify(input));

```


