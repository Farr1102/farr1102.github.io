---
layout: post
title: Call, apply, bind区别和源码
author: Farr
categories: 手写js系列
tags: [面试, 手写]
banner: 
  image: https://images.unsplash.com/photo-1574351406668-7585cd5b080c?q=80&w=2070&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D
---

### 区别

- call与apply的区别：参数不同，fn.call(context, arg1, arg2, arg...), fn.apply(context, [...args])
- bind与 call,apply的区别：bind会返回一个新函数，可延时执行；call,apply会直接返回结果

### Call

```js
Function.prototype.myCall = function(context, ...args) {
  if (typeof this !== 'function') {
    throw new TypeError('myCall被调用的必须为函数');
  }
  context = context || window;
  let fn = Symbol('fn');
  context['fn'] = this;
  const res = context['fn'](...args);
  delete context['fn']
  return res
}
```

### Apply

```js
Function.prototype.myApply = function(context, argsArr) {
  if (typeof this !== 'function') {
    throw new TypeError('myApply被调用的必须为函数');
  }
  if (argsArr && Array.isArray(argsArr)) {
    throw new TypeError('myApply参数必须为数组');
  }
  context = context || window;
  let fn = Symbol('fn');
  context['fn'] = this;
  const res = context['fn'](...argsArr);
  delete context['fn'];
  return res;
}
```

### Bind

```js
Function.prototype.myBind = function(fn, ...args) {
  if (typeof this !== 'function') {
    throw new TypeError('myBind被调用的必须为函数');
  }
  context = context || window;
  const _this = this;
  
  return function(...innerArgs) {
    if (this instanceof fn) {
      return new _this(...args, ...innerArgs);
    }
    return _this.apply(context, args.concat(innnerArgs))
  }
}
```
