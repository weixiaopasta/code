```
https://koa.bootcss.com/

// 处理 post 页面 html 银行跳转回来的加载页面请求有可能是 post 的，此时应该做重定向处理
// app.use(async (ctx, next) => {
//     if (ctx.method.toUpperCase() === 'POST' && ctx.path.endsWith('.html')) {
//         return ctx.redirect(ctx.path + ctx.search);
//     }
//     await next();
// });

```