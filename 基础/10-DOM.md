#### Node类型

`element.childNodes[0]`:获得第一个子节点元素

**nodeList并不是一个真正的数组**

`var arrayOfNodes=Array.prototype.slice.call(element.childNodes,0):`将nodeList转换为真正的数组

**操作元素**

```javascript
element.appendChild(newNode)//向element元素里的末尾添加新元素nodeNode

element.insertBefore(newNode,null)//向元素的末尾前添加新元素

element.insertBefore(newNode,element.firstChild)//在元素的第一个元素前添加新元素

element.insertBefore(newNode,element.lastChild)//在元素的第一个元素前添加新元素

element.replaceChild(newNode,element.firstChild)//将元素里的第一个元素替换成新元素

element.removeChild(element.firstChild)//移到元素里的第一个元素

element.cloneChild(true)//深复制元素，包括子元素、事件都会复制

element.cloneChild(false)//浅复制元素，只复制当前元素，不包括子元素的事件
```

**Document**

```javascript
var getTile=document.title //取得文档信息

document.title="page title" //设置文档信息

var url=document.URL //获得完成的URL

var domain=document.domain //获得域名

var referrer=document.referrer// 获得源页面的URl
```

**查找元素**

```javascript
document.getElementById('id') 
document.getElementsByTagName('tagName') 
document.getElementsByName('name')
 
 var myImage=images.nameItem("flag")  //在images的元素集合中找到name="falg"的元素
 
 images["flag"]==images.nameItem("flag") 
```

**文档写入**

```javascript
document.write("<strong>"+(new Date())).roString()+"</strong>");

document.write("<script src='\file.js'"+"<\/script>");  //注意结束标签的 ‘/’前要加入转义字符,要不然会写层的<script>配对

```

#### element元素

```html
<div id="myDiv" class="bg" title="Body text" lang="en" dir="ltr"></div> 
```

**特性**

```javascript
var div=document.getElementById("myDiv");
div.id//myDiv
div.className//bg
div.title//Body text
div.lang//en
div.dir//ltr
```

**属性**

```javascript
getAttribute('name') //获取某个属性
setAttribute('name','newName') //设置某个属性
removeAttribute('name')//移除某个属性
```

`getAttribute` 也可以操作元素的特性：div.getAttribute('class')等于div.className 、div.setAttribute('class','bg')等于div.className='bg'
 
 