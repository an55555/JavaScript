**最大值、最小值**

Math.max()  寻找最大值

Math.min() 寻找最小值 

```javascript
Math.max(1,234,5,6,5,57,56,87) //234
```

```javascript
// 从数组中寻找最大值
Math.max.apply(Math,[1,234,5,6,5,57,56,87]) //234
```

**舍入方法**

Math.ceil():向上取整

Math.floor():向下取整

Math.round():四舍五入

**随机数**

Math.random(): 取大于等于0到小于1之间的随便数

```javascript
值=Math.floor(Math.random()*可能值的总数+第一个可能的值)
```

**其它**

Math.abs(): 绝对值