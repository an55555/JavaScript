**堆、栈方法**

往数组尾部添加项：push()

往数组前添加项：unshift()

移除数组尾部的项：pop()

移除数组头部的项：shift()

**排序方法**

升序：

```javascript
[5,4,2,8,7,].sort(function(sort1,sort2) {
  if(sort1<sort2){
      return -1
  }else if(sort1>sort2){
      return 1
  }else{
      return 0
  }
})
```

1. 返回-1：第一个参数位于第二个参数前

2. 返回1：第一个参数位于第二个参数后

**纯数值排序的简便方法**

升序：

```javascript
[5,4,2,8,7].sort(function(sort1,sort2) {
  return sort1-sort2
})
```

**操作方法**

concat(): 基本当前创建一个新数组，类似复制

concat(arg): 基本当前创建一个新数组并将arg添加至尾部

slice(a,b): 从a位置开始到b位置结束（不取b）

```javascript
var a=[1,2,3]
var b=a.concat()  //[1,2,3]
var c=a.concat([4,5,6])//[1,2,3,4,5,6]
var d=c.slice(2,5) //[3,4,5]
```

splice(a,b,c,d): 从a位置开始删除b项并添加c和d

```javascript
var a=[2,3,4]
var b=a(2,1,7,8,9) //a=[2,3,7,8,9]  b=[4]
```

**位置方法**

indexof(a,b):  从b位置开始查找a,并返回a的位置，没有则返回-1

lastindexof(a,b):  从末尾开始查找,(数组长度-b)位置开始查找a,并返回a的位置，没有则返回-1

**迭代方法**

every():遍历数组，每一项都符合时则返回true

some():遍历数组，有一项符合则返回true

filter():遍历数组，返回符合要求的项

map():遍历数组，返回每一项的结果

forEach();遍历数组，无返回值

```javascript
var numbers=[1,2,3,4,5,6,7]
var everyResult=numbers.every(function(item,index,array) {
  return item>2
})
console.log(everyResult) //false
 ```
 
 **归并方法**
 
 reduce(): 从位置每一项开始遍历至最后一项
 
 reduceRight():从最后一项开始遍历至第一项
 
 ```javascript
 // 数组累加
var values=[1,2,3,4,5]
var sum=values.reduce(function(prev,cur,index,array) {
   return prev+cur;
})
console.log(sum) //15
```