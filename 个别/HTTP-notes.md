# HTTP-NODES

## POST-Form Data/Request Payload

### Form Data('Content-Type','application/x-www-form-urlencoded')

前：

```javascript
xmlhttp.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
xmlhttp.send("arg1=data1&arg2=data2");
```
参数形式：

```javascript
arg1:data1
arg2:data2
```
后台获取方式：

```javascript
var bodyParser = require('body-parser');
app.use(bodyParser.urlencoded({ extended: false}));
app.post('/user/query', function (req, res) {
    console.log(req.body)
})
```


### Request Payload(Content-Type:text/plain;charset=UTF-8)

Request Payload方式是以“流“”的方式出入到后台，需要监听data事件来获取完整的数据

前：

```javascript
 xmlhttp.send(JSON.stringify({'name':'ljz'}));
```

参数格式：

```javascript
{name:"ljz"}>name:"ljz"
```

后台获取方式：
```javascript
var str="";
req.on("data",function (dt) {
    str+=dt
})
req.on("end",function(){
    console.log(str);//{'name':'ljz'}
})
```
*Request Payload2（Content-Type:multipart/form-data;）*:

前：

```javascript
var data=new FormData();
data.append('name','ljz')
xmlhttp.send(data);
```

后台获取方式：

```javascript
var http = require('http');
var multiparty = require('multiparty');
app.post('/user/query', function (req, res) {
    var form = new multiparty.Form();
    form.parse(req, function (err, fields, files) {
        console.log(fields);
    });
})
```

## GET

获取GET请求的参数：req.query