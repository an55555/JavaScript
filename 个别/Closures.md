### 闭包

有权访问另一个作用域中变量的函数

#### Functions返回Functions

```javascript
    // More general function.
    function add(a, b) {
      return a + b;
    }
    
    add(1, 2);  // 3
    add(10, 3); // 13

```
```javascript
    // More specific function generator.
    function makeAdder(a) {
      return function(b) {
        return a + b;
      };
    }
    
    // More specific functions.
    var addOne = makeAdder(1);
    addOne(2);  // 3
    addOne(3);  // 4
```

### 闭包使用--Memoization

 Memoization用于优化比较耗时的计算,通过将计算结果缓存到内存中,这样对于同样的输入值,下次只需要中内存中读取结果

```javascript
function Fibonacci(n) {
    return (n===0)||(n===1)?n:arguments.callee(n-1)+arguments.callee(n-2);
}

//在严格撒模式下，callee不可用
//代替

var Fibonacci=(function f(n){
      return (n===0)||(n===1)?n:f(n-1)+f(n-2);
})

function memoizeFn(fn){
    var cache={}
    return function() {
      var key=arguments[0]
      if(cache[key]){
          return cache[key]
      }else{
          var value=fn(key)
          cache[key]=value
          return value
      }
    }
}
var getFibonacci=memoizeFn(Fibonacci)
console.log(getFibonacci(10))
console.log(getFibonacci(10))
```

### 闭包使用--函数重载

```javascript
    function addMethod(object, name, f)
    {
        var old = object[name];  //保存前一个函数
        object[name] = function()
        {
            if (f.length === arguments.length)
            {
                return f.apply(this, arguments);
            }
            else if (typeof old === "function")
            {
                return old.apply(this, arguments);
            }
        };
    }

    // 不传参数时，返回所有name
    function find0()
    {
        return this.names;
    }


    // 传一个参数时，返回firstName匹配的name
    function find1(firstName)
    {
        var result = [];
        for (var i = 0; i < this.names.length; i++)
        {
            if (this.names[i].indexOf(firstName) === 0)
            {
                result.push(this.names[i]);
            }
        }
        return result;
    }


    // 传两个参数时，返回firstName和lastName都匹配的name
    function find2(firstName, lastName)
    {
        var result = [];
        for (var i = 0; i < this.names.length; i++)
        {
            if (this.names[i] === (firstName + " " + lastName))
            {
                result.push(this.names[i]);
            }
        }
        return result;
    }


    var people = {
        names: ["Dean Edwards", "Alex Russell", "Dean Tom"]
    };


    addMethod(people, "find", find0);  //执行完后，old=undefind   people["find"]=find0   find0函数里保存着前一个old
    addMethod(people, "find", find1); //执行完后，old=find0   people["find"]=find1     find1函数里保存着前一个find00
    addMethod(people, "find", find2);//执行完后，old=find1   people["find"]=find2    find2函数里保存着前一个find10


    console.log(people.find()); // 输出["Dean Edwards", "Alex Russell", "Dean Tom"]
    console.log(people.find("Dean")); // 输出["Dean Edwards", "Dean Tom"]
    console.log(people.find("Dean", "Edwards")); // 输出["Dean Edwards"]
    //无论执行哪种参数个数的方法
    //首选执行的是参数为2个的那个函数，因为 addMethod(people, "find", find2);是最后执行的
    //如果当前个数不为2  则执行old的方法，即find1()函数
    //以此类推
```