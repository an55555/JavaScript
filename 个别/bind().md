> 引用及参考 ：https://segmentfault.com/a/1190000002662251

# bind()

这个方法会创建一个函数的实例，其this值会被绑定到传给bind()函数第一个参数的值

```javascript
window.color="red";
var o={color:"blue"};
function sayColor() {
  console.log(this.color)
}
console.log(sayColor())  //"red"
var bindColor=sayColor.bind(o)
console.log(bindColor()) //"blue"
```

如果第一个参数为`null` ，则不改变this （严格模式下会出错）

### bind()的第二个开始的参数会传给函数使用（偏函数应用）

```javascript
window.color="red";
var o={color:"blue"};
function sayColor(a,b) {
  console.log(arguments)
}
console.log(sayColor("A","B"))  //["A","B"]
var bindColor=sayColor.bind(o,"C","D")
console.log(bindColor("A","B")) //["C","d","A","B"]
```

###  偏函数 

```javascript
function list() {
  return Array.prototype.slice.call(arguments)
}
var list1 = list(1, 2, 3); // [1, 2, 3]

// 预定义参数37
var leadingThirtysevenList = list.bind(undefined, 37);

var list2 = leadingThirtysevenList(); // [37]
var list3 = leadingThirtysevenList(1, 2, 3); // [37, 1, 2, 3]
```

### 和setTimeout一起使用

一般情况下`setTimeout()` 的this会指向window.

```javascript
var color="black";
var o={
    color:"red",
    showColor:function() {
      setTimeout(function() {
        console.log(this.color)
      },1000)
    }
}
o.showColor()  //"black"
```

结合bind()

```javascript
var color="black";
var o={
    color:"red",
    showColor:function() {
      setTimeout(function() {
        console.log(this.color)
      }.bind(o),1000)
    }
}
o.showColor()//red
```

### 绑定函数作为构造函数

```javascript
function Point(x,y){
	this.x=x
	this.y=y
}
Point.prototype.showPoint=function(){
	console.log(this.x+","+this.y)
}
var p=new Point(1,2)
p.showPoint()  //1,2

var newPoint=Point.bind(null,10)
var np=new newPoint(20)
np.showPoint()  //10,20

np instanceof newPoint //true
np instanceof  Point//true
new Point(100,100) instanceof newPoint//true
```

### 将一个类数组对象转换为真正的数组

call方法：

```javascript
Array.prototype.slice.call(arguments)
```

bind方法

```javascript
var unboundSlice=Array.prototype.slice;
var slice=Function.prototype.call.bind(unboundSlice)
slice(arguments)
```

### 自己实现bind()方法

简单版：

```javascript
Function.prototype.bind=function(context) {
  var self=this;
  return function() {
    return self.apply(context,arguments)
  }
}
```

考虑到函数柯里化的情况，我们可以构建一个更加健壮的bind()：

```javascript
Function.prototype.bind=function(context) {
  var self=this;
  var arg=Array.prototype.slice.call(arguments,1)
  return function() {
        var concatArg=arg.concat(Array.prototype.slice.call(arguments))
         return self.apply(context,concatArg)
  }
}
```

Javascript的函数还可以作为构造函数，那么绑定后的函数用这种方式调用时，情况就比较微妙了，需要涉及到原型链的传递：

```javascript
Function.prototype.bind=function(context) {
  var self=this;
  var arg=Array.prototype.slice.call(arguments,1)
   var F=function() { }
  var bound= function() {
        var concatArg=arg.concat(Array.prototype.slice.call(arguments))
         return self.apply((this instanceof F ? this : context), concatArg);
  }

  F.prototype=sele.prototype
  bound.prototype=new F()
  return bound
}
```

对于为了在浏览器中能支持bind()函数，只需要对上述函数稍微修改即可：

```javascript
Function.prototype.bind = function (oThis) {
    if (typeof this !== "function") {
      throw new TypeError("Function.prototype.bind - what is trying to be bound is not callable");
    }

    var aArgs = Array.prototype.slice.call(arguments, 1), 
        fToBind = this, 
        fNOP = function () {},
        fBound = function () {
          return fToBind.apply(
              this instanceof fNOP && oThis ? this : oThis || window,
              aArgs.concat(Array.prototype.slice.call(arguments))
          );
        };

    fNOP.prototype = this.prototype;
    fBound.prototype = new fNOP();

    return fBound;
  };
```