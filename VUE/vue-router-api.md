#### Router构建选项

####  routes 

    类型: Array<RouteConfig>

    RouteConfig 的类型定义：
```
    interface RouteConfig = {
    path: string,
    component?: Component,
    name?: string, // 命名路由
    components?: { [name: string]: Component }, // 命名视图组件
    redirect?: string | Location | Function,
    props?: boolean | Object | Function,
    alias?: string | Array<string>,
    children?: Array<RouteConfig>, // 嵌套路由
    beforeEnter?: (to: Route, from: Route, next: Function) => void,
    meta?: any,

    // 2.6.0+
    caseSensitive?: boolean, // 匹配规则是否大小写敏感？(默认值：false)
    pathToRegexpOptions?: Object // 编译正则的选项
    }
```


####  mode

    类型: string

    默认值: "hash" (浏览器环境) | "abstract" (Node.js 环境)

    可选值: "hash" | "history" | "abstract"
    
    配置路由模式:

        hash: 使用 URL hash 值来作路由。支持所有浏览器，包括不支持 HTML5 History Api 的浏览器。

        history: 依赖 HTML5 History API 和服务器配置。查看 HTML5 History 模式。

        abstract: 支持所有 JavaScript 运行环境，如 Node.js 服务器端。如果发现没有浏览器的 API，路由会自动强制进入这个模式。

### base

    类型: string

    默认值: "/"

    应用的基路径。例如，如果整个单页应用服务在 /app/ 下，然后 base 就应该设为 "/app/"。

### linkActiveClass
    类型: string

    默认值: "router-link-active"

    全局配置 <router-link> 默认的激活的 class。参考 router-link。
### linkExactActiveClass
### scrollBehavior
### parseQuery / stringifyQuery
### fallback
    类型: boolean

    默认值: true

    当浏览器不支持 history.pushState 控制路由是否应该回退到 hash 模式。默认值为 true。

    在 IE9 中，设置为 false 会使得每个 router-link 导航都触发整页刷新。它可用于工作在 IE9 下的服务端渲染应用，因为一个 hash 模式的 URL 并不支持服务端渲染。

