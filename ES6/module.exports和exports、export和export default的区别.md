>Node应用由模块组成，采用CommonJS模块规范

## module.exports和exports、export和export default的区别

### module.exports

module指向当前模块，exports属性是对外的接口。加载某个模块，其实就是加载该模块的module.export属性

```javascript
// 0ipConfig.js
var x = 5;
var addX = function (value) {
  return value + x;
};
module.exports.x = x;
module.exports.addX = addX;
```

引入：

```javascript
import m from './0ipConfig.js';
console.log(m.x)
console.log(m.addX)
```

### exports与module.exports

Node为每一个模块提供一个exports属性，指向module.exports.即：exports=module.exports

**注意，不能直接将exports变量指向一个值，因为这样等于切断了exports与module.exports的联系。**

```javascript
// 0ipConfig.js
exports.ob ={
    dataIp: 'http://192.168.1.116:8080'
}
```

引入：

```javascript
import ip from './0ipConfig.js';
console.log(ip.x)
```

## ES6模块规范(使用export和import来导出、导入模块)

```javascript
// 0ipConfig.js
// 写法一
export var m = 1;

// 写法二
var x = 1;
var y = 2;
var z = 3;

export {x, y, z};

// 写法三
var n = 1;
export {n as m};
```

引入：

```javascript
// 0ipConfig.js
import {m} from './0ipConfig.js';
```

### export default(为模块指定默认输出)

```javascript
// 0ipConfig.js
export default function () {
  console.log('foo');
}
```

引入：

```javascript
import m from './0ipConfig.js';
```