# JavaScript中new的原理与用法

## 用法

`var object = new myFunction();`

## 解释

以上面的*用法*部分的代码为例，执行new包含以下几个步骤
1. 创建一个空白的简单JavaScript对象`_innerObject`
2. 将`_innerObject`的构造函数设置为`myFunction`
3. 将`_innerObject`绑定为this，执行`myFunction`
4. 如果myFunction执行返回了一个对象，那么将`object`的指向改为这个返回的对象（这种情况相当于`var object = myFunction()`）；如果function执行没有返回对象，那么将object的指向改为`_innerObject`
 
## 模拟new
下面用一个函数模拟new的过程
``` javascript
var simulateNew = function(){
  //1. 创建一个空白的简单JavaScript对象
  var obj = {};
  //取得new的源函数
  var Contrustor = [].shift.apply(arguments);
  //2. 将obj的原型设置为源函数的原型链，即将obj的构造函数设置为源函数
  obj.__proto__ = Contrustor.prototype;
  //3. 将obj绑定为this，并执行源函数
  var ret = Contrustor.apply(obj, arguments);
  //4. 如果源函数执行返回了一个对象，则返回该对象；如果没有，则返回obj
  return typeof ret === 'object'? ret : obj;
}
//定义new的源函数
var myClass = function() {
  this.name = 'myClass'
}
//模拟new的过程
var newInstance = simulateNew(myClass);
console.log(newInstance instanceof myClass);//true
```
* simulateNew函数的参数与new各个参数的对应关系如下
* new: `var a = new myFunction(arg1, arg2, ...)`
* simulateNew: `var a = simulateNew(myFunction, arg1, arg2, ...)`
