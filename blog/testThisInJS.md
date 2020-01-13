# 在JavaScript中this的指向

## 先给出结论
使用function声明的函数，函数中的this指向函数执行时的调用者；使用lamda表达式声明的函数，函数中的this指向函数声明时的调用者。
## 一些术语解释(glossary)
  - 回调函数(callback)：一个函数作为参数传递给另一个函数，那么这个作为参数被传递的函数就被称为回调函数。
  - 供应商（vendor）：在前端软件编写过程中，我们会用到一些依赖（dependency），这些依赖也需要被打包到正式环境中，一般会合并到一个名为vendors的js文件中，表示这是由其他软件公司提供的代码，而不是本公司自己生产的
  - bind方法：在JavaScript中Function的原型链上有一个名为bind的方法，用于绑定函数内的this对象。假设定义了一个函数a，那在同一个上下文内可以通过`var b = a.bind(object)`定义一个变量b，该变量也为一个函数，b与a的区别是，b不管作为谁的成员调用，其内部的this都会指向bind时指定的对象`object`
## 通过一段代码来说明
下面用四组测试，解释函数的四个最常用的使用场景中的this指向问题
```javascript
 var myObject = {
    name:'myObject',
    printThis: function() {
      console.log(this)
    },
    printThisWithLambda: ()=> {
      console.log(this)
    }
  }
  // 第一组测试
  myObject.printThis() //myObject
  myObject.printThisWithLambda() //window

  var vendor = {
    name: 'vendor',
    acceptCallback: function(callback) {
      callback()
    },
    storeCallbackAndCallLater: function(callback) {
      vendor.storedCallback = callback
      vendor.storedCallback()
    }
  }

  //第二组测试
  vendor.acceptCallback(myObject.printThis) //window
  vendor.acceptCallback(myObject.printThisWithLambda) //window

  //第三组测试
  vendor.storeCallbackAndCallLater(myObject.printThis) //vendor
  vendor.storeCallbackAndCallLater(myObject.printThisWithLambda) //window

  var specifiedThis = { name: 'specifiedThis' }

  //第四组测试
  vendor.acceptCallback(myObject.printThis.bind(specifiedThis)) //specifiedThis
  vendor.acceptCallback(myObject.printThisWithLambda.bind(specifiedThis)) //window
```
在这次测试中首先定义了一个对象myObject，其中包含两个函数，一个函数`printThis`直接通过传统的funciton方式声明，另一个函数`printThisWithLamda`通过lamda表达式声明
### 第一次测试
  我们在日常工作中，直接调用myObject对象的方法`printThis`和`printThisWithLambda`，可以看到第一次打印的内容是`myObject`这个对象，因为`printThis`的调用者是myObject；第二次打印的内容是`window`，因为`printThisWithLambda`在声明时没有在任何函数体内，他的声明是由window直接调用的
### 第二次测试
  除了JavaScript原生的方法外，还会用到很多的供应商（vendors），供应商往往都会提供一些方法，他们接收一个回调函数（callback）作为参数，通过在供应商内执行回调函数，来实现供应商强大的可拓展性。为了模拟这种情况，在下面的测试中定义了一个名为`vendors`的对象，该对象有一个`acceptCallback`的方法，该方法接收一个函数作为参数，并且在方法体内时执行该参数。在第二次测试中，将myObject对象的printThis方法和printThisWithLambda，作为参数传递给vendors的一个方法`acceptCallback`，可以看到两次打印的均为window——这是因为在acceptCallback函数内部，参数是直接通过`callback()`直接执行的，并没有作为任何对象的方法，而是直接被window调用的；而通过lambda表达式声明的函数，其内部的this一直为window对象。
### 第三次测试
  有些供应商内，会暂时把这个回调函数作为供应商内通用对象的一个属性存储起来，并且通过成员表达式调用这个函数。在第三次测试中，为`vendor`对象定义了方法`storeCallbackAndCallLater`，可以看到第一次打印的对象为`vendor`，第二次打印的对象为`window`——这是因为在供应商内部调用时，该回调函数是作为供应商的成员方法被引用的，而通过lambda表达式声明的函数，其内部的this一直为window对象。
### 第四次测试
  this的指向的不确定性，会在实际工作中造成很多麻烦，因此在JS标准的不断完善过程中提出了bind方法。通过bind方法可以生成一个绑定函数（bounding function），调用绑定函数时会在函数内部调用原函数。在第四次测试中，通过`acceptCallback`函数调用绑定函数`myObject.printThis.bind(specifiedThis)`和`myObject.printThisWithLambda.bind(specifiedThis)`，可以看到第一次打印的对象为`specifiedThis`，第二次打印的对象为`window`——这验证了bind可以改变函数内this的指向，而并不会改变lambda表达式定义的函数内的this指向。


## 参考
1.  [MDN Scope](https://developer.mozilla.org/en-US/docs/Glossary/Scope)
2.  [Function.prototype.bind()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)
## Words
glossary: 术语表
