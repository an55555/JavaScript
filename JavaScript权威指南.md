### 2.5 可选的分号

如果当前语句和下一行语无法合并解析，JavaScript就会在第一行后填补分号，但有两个例外。

第一个例外是在涉及return、break和continue语句的场景中。这三个关键字后紧跟着换行，JavaScript则会在换行处填补填补分号。

```javascript
return
true
```

将解析成

```javascript
return false;
true
```

这段的本意应该是`return true`

也就是说，在return、bread和continue和随后的表达式之间不能有换行。如果添加了换行，程序则只有在极特殊的情况下才会报错，而且程序的高度非常不方便。

第二个例外是涉及“++”和“--”的运算符中。如果将其用做后缀运算符，应当和它的表达式写在同一行。否则将在其填补，同时“++”和“--”将会作为下一行的前缀操作符并与
之一起解析，例如，这段代码：

```javascript
x
++
y
```

将会解析为

```javascript
x;
++y
```

其实本意是`x++;y`

### 3.0 类型、值和变量

**原始类型、对象类型**

原始类型：数字、字符串、boolean、undefine、Null既是原始类型、不可变类型、

对象类型：Object

**可变类型、不可变类型**

 可变类型：数组、对象、字符串（字符串可以看成由字符组成的数组）
 
 不可变类型： 数字、boolean、null和undefine
 
 **有无拥有方法类型**
 
 拥有方法类型： 数字、字符串、Boolean、对象
 
 无方法类型： null、undefine
 
 ### 3.1.4 二进制浮点数和四舍五入错误
 
 JavaScript中的数字具有足够的精度，并可以极其近似于0.1.但事实是，数字不能精确表述的确带来了一个问题。
 
 .2-.1=0.1
 
 .3-.2=0.09999999999999998
 
 .4-.3=0.10000000000000003
 
 除了.2-.1其实的相关都无法百分百精确
 
 ### 3.2.1
 
 字符串直接量可以拆分成数行，每行必须以反斜线（\）结束，反斜线和行结束符都不算是字符串直接量的内容
 
 ```javascipt
 one\
 two\
 there
 ```

输出为

```text
 one
 two
 there
```

如果希望在字符串直接量中另起一行，可以使用转义字符\n

`one\ntwo`

输出为

```text
one
two
```

### 3.3布尔值

布尔值包含toString方法，可以将boolean转换为为`true`或者`false`

```javaScript
Boolean('').toString(); // 'false'
```
### 3.4 null和undefined

```javaScript
var a;
null == a; // true
null === a; // false
```

null和undefined都不包含任何属性和方法。

如果想将它们赋值给变量或者属性，或将它们作为参数传入函数，最佳选择是使用nulll

### 3.6 包装对象

###　字符串、数字、布尔类型，这三个不是对象类型，却可以有自己的属性的方法。例如：

```javascript
var s = "Hello World"
var word = s.substring(s.indexof(" ")+1, s.length)
```

只要引用的s的属性，JavaScript对象就会通过调用new String(s)，将s转换为临时对象，这个对象继承的字符串的方法，所以可以调用其属性方法。一旦调用结束，这个新创建的对象就被销毁。 同字符串一样，数字和布尔类型也有各自的方法

### 基本类型通过 new方法转换为对象是临时的 

```javascript
var s = "str"
s.len = 4
var t = s.len
```

最后输出undefine， 因为s.len 是临时对象，随即就被销毁了

存储和获取字符串、数字、布尔类型的属性时所创建的临时对象叫**包装对象**

### 3.7 不可变的原始值和可变的对象引用

基本类型数字、布尔、字符串、null、undefined，这些是不可变的

```javascript
var s = "hello"
s.toUperCase() // 返回“HELLO”，但并没改变s的值,返回的是一个新 字符串值
s // hello
```

原始值的比较是值的比较，只有它的值相等时它们才相等。

对象类型也称为引用类型，即使两个对象的值一模一样也是永远不相等的

对象类型的比较其实是对两个对象的引用做比较，只比较的双方的引用相同时，它们才能相等

```javascript
var ob1 = { a: 1 }
var ob2 = { a: 1 }
ob1 === ob2 // false, 因为ob1和ob2是两个独立的对象
var ob3 = ob1
ob3 === ob1 // true,ob1和ob3都引用的同一个地址
```
