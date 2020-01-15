# JavaScript中的运算符
本文参考MDN，学习和介绍JavaScript中运算符以及一些用例

## JavaScript运算符的分类
- 成员运算符 member operators
- 赋值运算符 Assignment operators
- 比较运算符 Comparison operators
- 数学运算符 Arithmetic operators
- 位运算符 Bitwise operators
- 逻辑运算符 Logical operators
- 字符串运算符 String operators
- 条件运算符 Conditional operator
- 逗号运算符 Comma operator


## 成员运算符
成员运算符是JS中最常用也是最重要的运算符，包含`.`和`[]`两个，其中`[]`又称为可计算的成员运算符（computed member operator）。
下面的代码展示了使用成员运算符获取变量的例子

``` javascript
var a = {b:1}
console.log(a.b) //打印出1
```
需要注意这里的b必须是一个有效的标识符（identifier），如果需要在程序中动态指定要获取的属性的名称，我们就要用到`[]`。
下面的例子中定义了一个变量c，演示使用`[]`获取a对象的b属性的方法
``` javascript
var a = {b:1}
var c = 'b'
console.log(a[c]) //打印出1
```

## 赋值运算符
> `x=y` 表示将y赋值给x

另外，其他运算符也可以和`=`结合成一个复合运算符，他们的结合性和优先级都是与`=`相同的<br/>
比如: `x+=y` 等价于 `x=x+y`

> JavaScript中进行变量赋值时具体是引用传递还是值传递，取决于变量数据类型。
> 可以用typeof运算符来判断——如果`typeof a`的值为`boolean,string,number`，
> 则将a赋值给另一个变量时，使用引用传递；值为`function,object`，使用值传递


## 比较运算符
> `==`，`===`，`!=`，`!==`，`>`，`>=`，`<`，`<=` 共八个比较运算符
除了`===`和`!==`外，其他运算符均会执行类型转换(type coercion`/koh-ur-shuh n/`)，将两个操作数转换为相同类型后进行比较

## 数学运算符
> `+` `-` `*` `/` 四则双目运算符和其他语言都是相同的，只有一个特例，当除数为0时，JavaScript不会报错，而是会返回`Infinity`

下面的表格列出了所有的运算符

| 运算符               | 解释                                                  |
|------------------|----------------------------------------------------|
| 取余 remainder `%`  | 双目运算符，返回两个数相除的余数                                     |
| 自增 increment `++` | 单目运算符，使被操作数的值+1，如果是`++x`，返回+1之后的值；如果是`x++`，返回+1之前的值 |
| 自减 decrement `--` | 单目运算符，使被操作数的值-1，如果是`--x`，返回-1之后的值；如果是`x--`，返回-1之前的值 |
| 单目取负值 unary negation `-` | 单目运算符，**把操作数强制转换为number类型**，并将操作数的符号位取反 |
| 单目取数值 unary add `+` | 单目运算符，**把操作数强制转换为number类型**，返回操作数的值 |

## 位运算符
位运算符将他们的被操作数视为32位二进制数字，比如十进制数字6会被视为`00000000000000000000000000000110`，多于32位的数值，会保留最后32位

| Operator 操作符 |	Usage 用法 |	Description 描述 |
|--------------|-------------|---------------|
| Bitwise AND 位与 | a & b | 返回值的位是1如果两个操作数的这一位都是1 |
|Bitwise OR 位或 | a \| b | 返回值的位是0如果两个操作数的这一位都是0 |
| Bitwise XOR 位异或 | a ^ b | 返回值的位是0如果这两个操作数的这一位相同 |
| Bitwise NOT 位非 | ~ a | 单目操作符，对操作数的每一位取反 |
| Left shift 左移 | a << b | 将操作数的每一位左移，右侧用0填充 |
| Sign-propagating right shift 有符号右移(符号位被传播了~) | a >> b | 将操作数的每一位右移，左侧用符号位填充 |
|Zero-fill right shift 无符号右移 | a >>> b | 将操作数的每一位右移，左侧用0填充 |

## 逻辑运算符
与强类型语言不同，JavaScript中的逻辑运算符的操作数不一定是布尔值，其他类型的变量也可以参与逻辑运算。
null,undefined,空字符串，NaN，0，这五个变量在逻辑运算中相当于false，其他的变量都相当于true。<br/>

在计算机逻辑的学习中，经常使用真值表(truth table)来表示各个变量的布尔值与整个表达式的真假之间的关系。
在下面我列出了与，或，非三则运算在JavaScript中的真值表，可供日常使用参考

> 与(`&&`)运算

| operand1 | operand2 | output |
| ------ | ------ | ------ |
| true | true | operand2 |
| true | false | operand2 |
| false | true | operand1 |
| false | false | operand1 |

> 或(`||`)运算

| operand1 | operand2 | output |
| ------ | ------ | ------ |
| true | true | operand1 |
| true | false | operand1 |
| false | true | operand2 |
| false | false | operand2 |

> 非(`!`)运算

| operand | output |
| ------- | ------ |
| true | false |
| false | true |

从上面的表格中可以看出，与(`&&`)运算和或(`||`)运算他们的执行结果并不是true或false，而是第一个或者第二个操作数！
不仅如此，当返回值为operand1时，JavaScript还会进行短路运算（short-circuit evaluation），即，第二个操作数的表达式不会被执行！
<br/>在代码压缩中，经常会用到这一特性，下面举两个例子

> 使用非(`!`)运算符压缩true
``` javascript
console.log(true) //会打印出true
//true可以被简化为!!0（节省一个字符）
console.log(!!0) //会打印出true
```

> 使用短路运算压缩if语句
``` javascript
function a(){
  console.log('a is executed')
}
var flag = true
if(flag) {
  a()//打印出 a is executed
}
//上面的if语句就可以被简化为下面的表达式（节省两个字符）
flag&&a() //打印出 a is executed
```

## 字符串运算符
当`+`作为一个双目运算符出现，且其中一个操作数不是number类型时，两个操作数都会被强制转换为String类型，并且返回这两个字符串拼接之后的字符串。
<br/>另外，加法赋值表达式中的操作数有至少一个为String类型，也会将左侧操作数赋值为两个操作数进行字符串拼接之后的结果
。
```javascript
var operand1 = 'opr1';
var operand2 = 2;
operand2 += operand1; //operand2的值被修改为字符串 2oper1
operand1 += operand2; //operand1的值被修改为字符串 oper12oper1
```

> JavaScript中，将对象转换为字符串的方法是调用该对象的toString方法，这与Java是相似的

## 条件运算符（三目运算符）
> `condition ? expr1 : expr2`

三目运算符可以看做是if-else表达式的一种简写形式，并且可以在一些不能使用if-else表达式的地方使用（比如插值表达式内部{{}}），但是大多数框架并不推荐这么用
<br/>以上的条件运算表达式，可以写为类似下面的if-else语句

```  javascript
if(condition){
  expr1
} else {
  expr2
}
```

> 三目运算符也是短路运算符
```javascript
var a = true ? 0 : false ? console.log(222) : 11&&console.log(111) //不执行console.log语句
```
上面这段代码，不会在控制台打印任何内容，这与if-else语句的表现是一致的

## 逗号运算符
逗号运算符`,`，只是一个简单的连接符，会执行逗号两侧的表达式，后一个表达式的返回值会作为整个表达式的返回值

# 运算符优先级（precedence）和结合性（associativity）
参考： [Operator precedence](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)<br/>
在一个表达式中，可能包含多个运算符，优先级和结合性会对表达式的结果产生影响：
优先级高的运算符会优先执行，优先级相同的运算符，会根据结合性，从左到右（left associativity）或者从右到左（right associativity）执行
<br/>比如下面的表达式
``` javascript
var a = 5 - 2 * 2 
console.log(a) //打印出1
```
首先根据优先级，乘法`*`优先级 > 减法`-`优先级 > 赋值`=`优先级，因此乘法先执行，右侧表达式变为计算5-4的值，计算出的结果为1，最后将1赋值给a


