####  Route render methods
```
Route component
Route render
Route children
```

#### Route props

所有的渲染方式都会提供相同的三个路由属性。

```
match
location
history
```


#### browserHistory  hashHistory h5 侧


#### browserHistory
浏览器的路由就不再通过Hash完成了，而显示正常的路径example.com/some/path，背后调用的是浏览器的History API。

如果开发服务器使用的是webpack-dev-server，加上--history-api-fallback参数就可以了。


$ webpack-dev-server --inline --content-base . --history-api-fallback

```
import {
    BrowserRouter as Router,
    Switch,
    Route,
    Link,
    useParams,
    useRouteMatch
} from "react-router-dom";
```

其中引人注意的是

useRouteMatch 用于解析路由对象
path
url 
useParams 用于解析路由参数


#### history
路由历史对象，包含以下属性
```
length - (number) 历史堆栈中的条目数
action - (string) 当前动作类型 (PUSH, REPLACE, or POP)
location - (object) 当前的location对象，可能包含以下属性:

1、pathname - (string) URL的path部分
2、search - (string) URL的query部分
3、hash - (string) URL hash部分
4、state - (object)位置特定的状态，当此位置被推入堆栈时提供的推入状态(路径、状态)。仅在浏览器和内存历史中可用.


    存储：
    this.props.history.push({
            pathname: `/success/1`, 
            state: res.data
        });//成功
    获取：
    this.props.location.state;


push(path, [state]) - (function) 将新条目推入历史堆栈
replace(path, [state]) - (function)替换历史堆栈上的当前条目
go(n) - (function) 在历史堆栈中移动n(可正可负，即向前或者向后)个条目的指针
goBack() - (function) 等价于go(-1), 后退一页
goForward() - (function) 等价于 go(1), 前进一页
历史对象是可变的。因此，建议从渲染道具中访问位置，而不是从history.location中访问

```


#### 常用的hooks
react >= 16.8
React Router v5.1.0
#### useHistory

```
import { useHistory } from "react-router-dom";

function HomeButton() {
  let history = useHistory();

  function handleClick() {
    history.push("/home");
  }

  return (
    <button type="button" onClick={handleClick}>
      Go home
    </button>
  );
}

```

#### useLocation

获取位置对象
```
  let location = useLocation();
```

#### useParams  用于解析路由参数

useParams返回一个包含URL参数的键/值对的对象。使用它来访问match。当前<Route>的参数。

```
import React from "react";
import ReactDOM from "react-dom";
import {
  BrowserRouter as Router,
  Switch,
  Route,
  useParams
} from "react-router-dom";

function BlogPost() {
  let { slug } = useParams();
  return <div>Now showing post {slug}</div>;
}

ReactDOM.render(
  <Router>
    <Switch>
      <Route exact path="/">
        <HomePage />
      </Route>
      <Route path="/blog/:slug">
        <BlogPost />
      </Route>
    </Switch>
  </Router>,
  node
);

```

#### useRouteMatch 用于解析路由对象





#### 滚动至顶部
```
import { useEffect } from "react";
import { useLocation } from "react-router-dom";

export default function ScrollToTop() {
  const { pathname } = useLocation();

  useEffect(() => {
    window.scrollTo(0, 0);
  }, [pathname]);

  return null;
}
```



#### hashHitory

a页面跳转到b页面, 通过state传参

```
 this.props.history.push({
    pathname: `/chunan-my-declare-detail`,
    state: {
      record,
      takenCount: record.takenCount,
      takenTicketTime: record.takenTicketTime,
      policyCode: this.state.policyCode,
      legalPersonName: this.state.farenData.legalPersonName,
      legalPersonPhone: this.state.farenData.legalPersonPhone,
    }
  });
```

b页面接收
```
this.props.location.state

```

#### 缺点
刷新当前页面时 this.props,location.state为undefined

查了一下发现,是使用 HashRouter的原因, 如果是BrowserRouter就不会出现这个问题 

### 解决方案
1.传递和接收的值是对象{}的话使用
sessionStorage.setItem('params', JSON.stringify({a:1, b: 2})); 值转换为 JSON 字符串
JSON.parse(sessionStorage.getItem('params')) JSON 字符串转换为对象


#### HashRouter

#### basename: string
所有路径的基本URL。basename 应以斜杠开头，但不能以斜杠结尾。
```
<HashRouter basename="/calendar"/>
<Link to="/today"/> // renders <a href="#/calendar/today">
```

#### getUserConfirmation: func
跳转路由前的确认函数。默认为使用window.confirm。
```
<HashRouter
  getUserConfirmation={(message, callback) => {
    // this is the default behavior
    const allowTransition = window.confirm(message);
    callback(allowTransition);
  }}
/>

```

#### hashType: string
用于window.location.hash的编码类型。可用值为:
```
"slash"- # 后有一个斜线，例如： #/ 或 #/sunshine/lollipops
"noslash"-# 后面没有斜线，例如：# 或 #sunshine/lollipops
"hashbang"-Google 风格的 ajax crawlable，例如 #!/ 和 #!/sunshine/lollipops

 默认为 "slash"
```


#### children:node
要呈现的单个子元素
####  Link 为你的应用提供声明式的、可访问的导航链接。
```
<Link to="/about">About</Link>
```
#### to: string
链接地址的字符串表示形式，通过 pathname + search + hash 创建
```
<Link to='/courses?sort=name' />
```
#### to: object

to 可以包含以下属性：
```
pathname: 要跳转的路径
search: URL中query参数的字符串形式
hash:URL中的hash部分，例如：#a-hash.
state:存储到 location 中的额外状态数据
```
```
<Link to={{
  pathname: '/courses',
  search: '?sort=name',
  hash: '#the-hash',
  state: {
    fromDashboard: true
  }
}} />
```

### to:function
以当前位置为参数传递给该函数，该函数以字符串或对象的形式返回目标路径。
```
<Link to={location => ({ ...location, pathname: "/courses" })} />


<Link to={location => `${location.pathname}?sort=name`} />
```
#### replace:bool
replace 为 true 时，点击链接将会在历史记录中替换 当前页面，而不是添加一个新的页面。
<Link to="/courses" replace />







