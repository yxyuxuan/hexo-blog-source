---
title: JS中的sort和localeCompare排序
date: 2020-09-16 20:15:43
tags: JavaScript
categories: JavaScript
---

### 一、sort排序

```javascript
const arr = [20, 1, 6, 3, 47, 4]
arr.sort(); //[1, 20, 3, 4, 47, 6]

arr.sort((a,b)=>a-b) // [1, 3, 4, 6, 20, 47]
```

sort()方法默认是按照unicode编码来进行比较的，而不是按照我们想的比较两个数值间的大小来进行排序了。对Number类型的数组元素先调用toString()方法，再按照字符串的Unicode编码进行排序。

对象类型的数组根据对象中的某个属性进行排序：

```javascript
const arr = [
    {name:'zopp',age:1},
    {name:'gpp',age:20},
    {name:'yjj',age:6}
];

function compare(property){
    return function(a,b){
        var value1 = a[property];
        var value2 = b[property];
        return value1 - value2;
    }
}
console.log(arr.sort(compare('age')))
//[
    {name:'zopp',age:1},
    {name:'yjj',age:6},
    {name:'gpp',age:20},
];
```

### 二、使用 localecompare 对中文进行排序

在网页上展示列表时经常需要对列表进行排序：按照修改/访问时间排序、按照地区、按照名称排序。

JavaScript 提供本地化文字排序，比如对中文按照拼音排序，不需要程序显示比较字符串拼音。String.prototype.localeCompare在不考虑多音字的前提下，基本可以完美实现按照拼音排序。

汉语拼音声明表：b p m f d t n l g k h j q x zh ch sh r z s y w

```JavaScript
string.localCompare(target, locals)

'北京'.localeCompare('上海', 'zh')// -1
'佛山'.localeCompare('北京', 'zh')// 1
```

localCompare是根据我们的中文系统，把汉字先转换成了拼音，再进行了比较；对于同拼音的汉字，js再根据声调进行比较。

**数组中按对象的某个属性名按拼音进行排序：**

```javascript
const arr = [
    {name:"北京",age:111},
    {name:"上海",age:122},
    {name:"广州",age:222}
]	
const property ="name"
arr.sort((a,b)=> {
   return a[property].localeCompare(b[property])
})

console.log(arr)
//const arr = [
    {name:"北京",age:111},
    {name:"广州",age:222},
    {name:"上海",age:122}
]

```

