---
title: Object.assign is not a function
date: 2019-08-09 14:51:03
tags: JavaScript
categories: JavaScript
---

> 今天安卓说发现低版本的手机打不开我写的h5页面，看了他电脑上的运行日志发现了问题，Uncaught TypeError: Object.assign is not a function

```
08-09 10:28:50.011 8628-8628/com.cheezgroup.tosharing.intl I/chromium: [INFO:CONSOLE(41480)] "Uncaught TypeError: Object.assign is not a function", source: http://192.168.1.16:3000/_next/static/development/pages/_app.js?ts=1565317716331 (41480)
```

网上搜了一波，发现了问题是因为[Babel](https://babeljs.io/)是不能转换[Object.assign](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)方法的。

如果想让这Object.assign运行，必须使用[babel-polyfill](https://babeljs.io/docs/en/6.26.3/babel-polyfill)，为当前环境提供一个polyfill，但是考虑到babel-polyfill太大，不建议安装。建议手动写一个polyfill文件，需要哪些polyfill就手动加上，然后在app.js中导入即可。

然后找了一个polyfill如下：

```
if (typeof Object.assign !== "function") {
  Object.assign = function(target) {
    "use strict";
    if (target === null) {
      // TypeError if undefined or null
      throw new TypeError("Cannot convert undefined or null to object");
    }

    var to = Object(target);

    for (var index = 1; index < arguments.length; index++) {
      var nextSource = arguments[index];

      if (nextSource !== null) {
        // Skip over if undefined or null
        for (var nextKey in nextSource) {
          // Avoid bugs when hasOwnProperty is shadowed
          if (Object.prototype.hasOwnProperty.call(nextSource, nextKey)) {
            to[nextKey] = nextSource[nextKey];
          }
        }
      }
    }
    return to;
  };
}
```

编译运行还是报错，发现网上大多数都是这样写的，没什么差别，老大说人家都能执行，你的不行肯定是在函数初始化的时候就错了，后来发现根本就没运行上面的代码，那就是判断条件可能出错了，做了如下修改，运行成功：

```
if (!Object.prototype.assign || typeof Object.assign !== "function") {
  Object.assign = function(target) {
    "use strict";
    if (target === null) {
      // TypeError if undefined or null
      throw new TypeError("Cannot convert undefined or null to object");
    }

    var to = Object(target);

    for (var index = 1; index < arguments.length; index++) {
      var nextSource = arguments[index];

      if (nextSource !== null) {
        // Skip over if undefined or null
        for (var nextKey in nextSource) {
          // Avoid bugs when hasOwnProperty is shadowed
          if (Object.prototype.hasOwnProperty.call(nextSource, nextKey)) {
            to[nextKey] = nextSource[nextKey];
          }
        }
      }
    }
    return to;
  };
}
```

总结：多实践，多尝试，比较差异。