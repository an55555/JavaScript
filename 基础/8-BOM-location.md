#### location

1. **hash**:#contents --返回URL中的hash

2. **host**:www.wrox.com:80 --返回服务器名称和商品号

3. **hostname**:www.wrox.com --返回服务器名

4. **port**:80 --返回商品号

5. **protocol**-:http -返回页面使用的协议

6. **search** ?q=abc --返回URL的查询字符串

**查询字符串参数**

```javascript
function getURLSearch() {
  var qs=location.search?location.search.substring(1):''
  var searchArr=qs.length?qs.split('&'):[]
  var searchJson={}
  
  for(var i in searchArr){
      var item=searchArr[i].split('&')
      searchJson[item[0]]=searchJson[item[1]]
  }
  return searchArr
}
```

#### window.open()

接收四个参数：

1. 要加载的URL

2. 窗口目标：_self、_parent、_top、_blank

3. 一个特性字符串

4. 新页面是否取代浏览器当前的历史记录的布尔值

#### 位置操作


**在当前页面打开新的URL地址** 并在浏览器的历史记录中生成一条记录

1. location.assign("http://www.an55555.com")

2. location.href("http://www.an55555.com")

3. window.location("http://www.an55555.com")


**在当前页面打开新的URL地址** 不会浏览器的历史记录中生成一条记录

1. location.replace("http://www.an55555.com")

**重新加载当前页面**

```javascript
location.roload() //有可能从缓存中加载
location.roload(true) //从服务器重新加载
```

#### history

history(1)=history.forward()

history(-1)=history.back()