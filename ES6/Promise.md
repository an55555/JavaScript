### 基本用法

ES6规定，Promise对象是一个构造函数，用来生成Promise实例

下面代码创造了一个Promise实例。

```javascript
let promise=new Promise(function(resolve,reject){
  //...do some
  if(/*异步操作成功*/){
    resolve(value)
  }else{
    reject(error)
  }
})
```

Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。它们是两个函数，由JavaScript引擎提供，不用自己部署。

resolve函数的作用是，将Promise对象的状态从“未完成”变成“成功”（即Pednding变为Resolved）,在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；

reject函数的作用是，将Promise对象的状态从“未完成”变成“失败”（即Pedding变为Reject）,在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

Promise实例生成之后可以使用then方法分别指定Resolve和Reject状态的回调函数。

```javascript
promise.then(
  function(value){
  //success
  },
  function(value){
  //failure
  }
)
```


then方法接收两个函数作为参数，第一个函数是Promise对象的状态变成Resolve时调用，第二个函数是Promise对象的状态变成Reject时调用。
其中，第二个函数是可选的，不一定要提供。这两个函数都接收Promise对象传出的值作变参数

下面是一个Promise对象的简单例子。

```javascript
function timeout(ms)(
  return new Promise(function(resolve,reject){
   setTimeout(resolve,ms,'done')
  }
))
timeout(100).then(value=>{console.log(value)})
```

上面的代码中，timeout函数返回一个Promise实例，表示一段时间后才会发生的结果。过了指定的时间（参数ms）以后，Promise实例的状态变为Resolved,
就会触发then方法绑定的回调用函数。

Promise新建后就会立即执行。

```javascript
let promise=new Promise(function(resolve,reject){
  console.log('Promise');
  resolve();
})
promise.then(
  function(){
    console.log('Resolved')
  }
)
console.log('Hi!');
//Promise
//Hi!
//Resolved
```

上面的代码中，Promise新建后立即执行，所以首先输出的是“Promise”.然后，then方法指定的回调函数，将在当前脚本所有同步的任务执行完才会执行，
所以“Resolved”最后输出 .

下面是加载图片的例子

```javascript
  function loadImgAsync(url){
        return new Promise(function(resolve,reject){
            var loadImg=new Image();
            loadImg.src=url;
            loadImg.onload=function(){
                resolve(loadImg)
            }
            loadImg.onerror=function(){
                reject(new Error("Could not load image at"+url))
            }
        })
    }
    loadImgAsync('').then(
        function(img){
            console.log("图片加载成功")
            console.log(img)
        },  function(err){
            console.log(err)
        }
    )
```

下面是用Promise实现ajax的例子

```javascript
 var getJSON=function(url){
        var promise=new Promise(function(resolve,reject){
            var client=new XMLHttpRequest();
            client.open("GET",url)
            client.onreadystatechange=handler;
            client.responseType="json";
            client.setRequestHeader("Accept","application/json");
            client.send();
            function handler(){
                if(this.readyState!==4){
                    return;
                }
                if(this.status===200){
                    resolve(this.response);
                }else{
                    reject(new Error(this.statusText));
                }
            };
        })
        return promise;
    }
    getJSON("./posts.json").then(function(json){
            console.log('Contents'+json);
        },
        function(error){
            console.log('出错了',error)
        }
    )
```

上面的代码中，getJSON是对XMLHttpRequest对象的封闭，用于发出一个对JSON数据的HTTP请求，并且返回一个Promise对象。需要注意的是，在getJSON内部，
resolve函数和reject函数调用时，都带有参数。

如果调用reolve函数和reject函数时带有函数，那么它们的参数被传递给回调函数。reject函数的参数通常是Error对象的实例，表示抛出的错误；
resolve函数的参数除了正常的值以外，还可能是另一个Promise实例，表示异步操作的结果有可能是一个值 ，也有可能是另一个异步操作，比如像下面这样。

```javascript
var p1=new Promise(function(resolve,reject){
  //...
})
var p2=new Promise(function(resolve,reject){
  //...
  resolve(p1);
})
```

上面的代码中，p1和p2都是Promise实例，但是p2的resolve方法将p1作为参数，即一个异步操作的结果是返回另一个异步操作。

注意，这时p1的状态就会传递给p2，也就是说，p1的状态决定了p2的状态。如果p1的状态是Pending，那么p2的回调函数就会等待p1的状态改变；
如果p1的状态已经是Resolved或者Rejected,那么p2的回调函数将立刻执行。

```javascript
var p1=new Promise(function(resolve,reject){
    //setTimeout(reject,3000,new Error("fail"))
    setTimeout(()=>reject(new Error("fail")),3000)
})
var p2=new Promise(function(resolve,reject){
    //setTimeout(resolve,1000,p1)
    setTimeout(()=>resolve(p1),1000)
})
p2.then(result=>{console.log(result)})
p2.catch(error=>{console.log(error)})
```

上面代码中，p1是一个Promise，3秒之后变为rejected.p2的状态由p1决定，1秒之后，p2调用resolve方法,但是此时p1的状态还没有改变，
因此p2的状态也不会变。又过了2秒，p1变为rejected,p2也跟着变为rejected.

### Promise.prototype.then()

Promise实例具有then方法，也就是说，then方法是定义在原型对象Promise.prototype上的。它的作用是为Promise实例添加状态改变时的回调函数。
前面说过，then方法的第一个参数是Resolved状态的回调函数，第二个参数是Rejected状态的回调函数 。

then方法返回的是一个新的Promise实例（注意，不是原来那个Promise实例）。因此可以采用链式写法，即then方法后再调用另一个then方法。

```javascript
getJSON("./posts.json").then(function(json){
  return json.post
}).then(function(post){
  //...
})

上面的代码使用then方法，依次指定了两个回调函数。第一个回调函数完成以后，会将结果作为参数，传入第二个回调函数。

采用链式的then，可以指定一组按照次序调用的回调函数。这时，前一个回调函数，有可能返回的还是一个Promise对象（即有异步操作），
这时后一个回调函数，就会等待该Promise对象的状态发生变化，才会被调用。

```javascript
getJSON('./posts.json').then(function(post){
  return getJSON(post.comentURL);
}).then(function funcA(comments){
  console.log("resolved:",comments);
},function funcB(err){
  console.log("Rejected:",err)
})
```

上面的代码中，第一个then方法指定的回调函数，返回的是另一个Promise对象。这时，第二个then方法指定的回调函数，就会等待这个新的Promise对象状态发生变化 。
如果变为Resoved，就调用funcA,如果状态变为Rejected,就调用funcB.

如果采用箭头函数，上面的代码可以写得更简洁

```javascript
getJSON("./posts.josn").then(
  post=>{ getJSON(post.commentURL)}
).then(
  comments=>console.log('Resolved:',comments),
  err=>console.log("Rejected:",err)
)
```

### Promise.prototype.catch()

Promise.prototype.catch方法是.then(null,rejection)方法的别名，用于指定发生错误的回调函数。

```javascript
getJSON("./post.json").then(function(){
  //...
}).catch(function(error){
  console.log('发生错误！',error)
})
```
上面代码中，getJSON方法返回一个Promise对象，如果该对象状态变为Resolved，则会调用then方法指定的回调函数；如果异步操抛出错误，状态就会变为Rejected,
就会调用catch方法指定的回调函数 ，处理这个错误。另外，then方法指定的回调函数，如果运行中抛出错误，也会被catch捕获。

```javascript
p.then((val)=>console.log("fulilled:",val))
.catch((err)=>console.log("rejected:",err))
//等同于
p.then((val)=>console.log("fulilled:",val))
.then(null,(err)=>console.log("rejected:",err))
```

下面是一个例子。

```javascript
var prommise=new Promise(function(reosolve,reject){
  throw new Error('test');
});
promise.catch(function(error){
  console.log(error)
})
//Error:test
```

上面代码中，promise抛出一个错误，就被catch方法指定的回调函数捕获。注意，上面的写法与下面两种写法是先从的。

```javascript
//写法一
var prommise=new Promise(function(reosolve,reject){
  try{
     throw new Error('test');
  }catch(e){
    reject(e)
  }
});
promise.catch(function(error){
  console.log(error)
})
//写法二
var prommise=new Promise(function(reosolve,reject){
  reject(new Error('test'));
});
promise.catch(function(error){
  console.log(error)
})
//Error:test
```

比较上面两种写法，可以发现reject方法的作用，先同于抛出错误。

如果Promise状态已经变成Resolved,再抛出错误是无效的。

```javascript
var promise=new Promise(function(resolve,reject){
  resolve('ok');
  throw new  Error('test');
});
promise.then(value=>console.log(value)).then(error=>console.log(error))
```

上面代码中，Promise在resolve语句后面，再抛出错误，不会被捕获，等于没有抛出。

Promise对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止，也就是说，错误总是会被下一个catch语句捕获。

```javascript
getJSON("./post/1.json")
.then(post=>getJSON(post.commentURL))
.then(function(comments){
  //some code
}).catch(function(error){
  //处理前面三个Promise产生的错误
})
```

上面代码中，一共有三个Promise对象：一个由getJSON产生，两个由then产生。它们之中任何一个抛出的错误，都会被最后一个catch捕获。一般来说，
不要在then方法里面定义Reject状态的回调函数（即then的第二个参数），总是使用catch方法。

```javascript
//bad
promise
.then(function(data){
  //success
},function(err){
  //error
})
//good
promise
.then(function(data){
  //success
})
.catch(function(data){
  //error
})
```
