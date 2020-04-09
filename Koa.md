# Koa

## 安装

```bash
npm i koa -S
```

## 基本使用

```js
const Koa = require('koa')
const app = new Koa()
app.use((ctx,next)=>{
    ctx.body = "Hello World!"
})
app.listen(8080)
```

## 静态文件托管

使用`koa-static`托管静态文件

```bash
npm i koa-static -S
```

使用：

+ `serve(root[,options])`，`root`为静态文件目录，`options`为选项

```js
const path = require('path')
const Koa = require('koa')
const serve = require('koa-static')
const app = new Koa()
app.use((ctx,next)=>{
    ctx.body = "Hello World!"
    await next()
})
app.use(serve(path.resolve(__dirname,'public')))
app.listen(8080)
```

## 路由

通过 `ctx.request.path`可以获取用户请求的路径，由此实现简单的路由。

```js
app.use((ctx,next)=>{
    let path = ctx.request.path
    if(path === '/'){
        //do something
    }else if(path === '/about'){
        //do something
    }else{
        //do something
    }
})
```

使用`koa-router`

```js
//npm i koa-router -S

const Router = require('koa-router')
const router = new Router()
router.get('/',(ctx,next)=>{
    //do something
})
router.get('/about',(ctx,next)=>{
    //do something
})
//...
app.use(router.routes())
app.use(router.allowedMethods())
```

## 中间件

实质上就是一个函数，处在 HTTP Request 和 HTTP Response 中间，用来实现某种中间功能。`app.use()`用来加载中间件。

```js
const one = (ctx,next)=>{
    console.log(">> one")
    next()
    console.log("<< one")
}
const two = (ctx,next)=>{
    console.log(">> two")
    next()
    console.log("<< two")
}
const main = (ctx,next)=>{
    ctx.body = 'Hello World!'
    console.log("done")
}
//one就是一个中间件
app.use(one)
app.use(two)
app.use(main)
```

中间件的第二个参数为一个函数，只要调用了这个函数，就可以把执行权交给下一个中间件。由此形成一个中间件栈，后进栈的中间件先执行完。

上面的代码在控制台输出顺序为：

```
>> one
>> two
done
<< two
<< one
```

## 异步中间件

如果中间件内部包含异步操作，则中间件必须写成 async 函数，例如操作数据库，读取一个文件等。并在异步操作前面使用关键字await。

```js
const fs = require('fs')
const main = async (ctx,next)=>{
  ctx.response.type = 'html';
  ctx.response.body = await fs.readFile('./demos/template.html', 'utf8');
}
```

上面的中间件只有等到文件读取完毕才会赋值成功，并响应数据。

### 组合中间件

```js
// npm i koa-compose -S

const compose = require('koa-compose')
let middleware = compose([one,two,main])
app.use(middleware)
```

经过组合的中间件输出顺序和上面一样。

## 错误处理

使用`ctx.throw`抛出错误。

编写一个错误处理中间件：

```js
const errorHandler = (ctx,next)=>{
    try{
        next()
    }catch(err){
        ctx.response.status = err.statusCode || err.status || 500;
        ctx.response.body = {
          message: err.message
        };
    }
}
const main = (ctx,next)=>{
    //do something
}
app.use(errorHandler)
app.use(main)
```

**注意：**要使错误生效的话，必须全用async函数或者在内部使用return。

## 参考资料

+ [Koa 框架教程](http://www.ruanyifeng.com/blog/2017/08/koa.html)