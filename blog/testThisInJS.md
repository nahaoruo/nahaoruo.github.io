# 在JavaScript中this的指向
## 结论
使用function声明的函数，函数中的this取决于函数**运行**时的*上下文*；使用lamda表达式声明的函数，函数中的this取决于函数**声明**时的*上下文*。
## 术语解释
* 函数与方法
JavaScript中的函数，如果作为一个对象的成员，那么这个函数也可以称为这个对象的方法。这与JAVA很类似，但是有一个区别——如果一个函数声明为一个变量（比如`var a = function(){}`），这个函数`a`就不是任何对象的方法。
* 上下文
JavaScript
## 代码
以下一段代码用于打印JavaScript中this的指向
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

  var vendors = {
    acceptCallback: function(callback) {
      callback()
    },
    acceptCallbackAndChangeThis: function(callback) {
      var newThis = {name:'newThis'}
      newThis.callbackReference = callback
      newThis.callbackReference()
    }
  }

  //第二组测试
  vendors.acceptCallback(myObject.printThis) //window
  vendors.acceptCallback(myObject.printThisWithLambda) //window

  //第三组测试
  vendors.acceptCallbackAndChangeThis(myObject.printThis) //newThis
  vendors.acceptCallbackAndChangeThis(myObject.printThisWithLambda) //window

  var specifiedThis = { name: 'specifiedThis' }

  //第四组测试
  vendors.acceptCallbackAndChangeThis(myObject.printThis.bind(specifiedThis)) //specifiedThis
  vendors.acceptCallbackAndChangeThis(myObject.printThisWithLambda.bind(specifiedThis)) //window

```
在这次测试中首先定义了一个对象myObject，其中包含两个函数，一个函数`printThis`直接通过传统的funciton方式声明，另一个函数`printThisWithLamda`通过lamda表达式声明
* 第一次测试
通过成员运算符`.`，取得myObject对象的printThis和printThisWithLamda的方法并执行，控制台打印的内容分别是myObject和window。是因为函数的调用者为myObject，而函数声明时没有在任何函数内，因此函数的声明时上下文为window。
* 第二次测试
为了模拟回调函数的情况，定义了一个名为vendors的对象，该对象有一个`acceptCallback`的方法，该方法接收一个函数作为参数，并且执行该函数。在第二次测试中，将myObject对象的printThis方法和
