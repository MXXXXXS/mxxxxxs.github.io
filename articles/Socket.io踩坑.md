2019.12.9

*遇到和我一样踩坑的[老哥]( https://jasminexie.github.io/using-webpack-for-node-backend/ )*

## 服务器端socket.io报错

### bug1

```
ERROR in ./node_modules/engine.io/lib/server.js
    Module not found: Error: Can't resolve 'uws' in 'd:\coding\webcam\node_modules\engine.io\lib'
```

去'node_modules'里找'uws'没找到, 网上查是作者清空了仓库😂

一个方法是安装uws的以前的版本`npm i uws@10.148.1`, 但是已经废弃了

另一个方法是改源码, './node_modules/engine.io/lib/server.js'的107行, 这么改

```javascript
switch (this.wsEngine) {
  // case 'uws': wsModule = require('uws'); break;
  case 'uws': wsModule = require('ws'); break;
  case 'ws': wsModule = require('ws'); break;
  default: throw new Error('unknown wsEngine');
}
```

不使用'uws', 直接使用'ws'模块

我选择了第二种方法

### bug2

```
Error: Cannot find module 'socket.io-client/dist/socket.io.js'
```

>  You don’t even f*ing need socket.io client on your server. 

正如那位老哥所言, server要什么客户端, 而且还报错, 不用了

```typescript
const io = socketIO(server, {
  serveClient: false
})
```

传入选项, 禁用掉