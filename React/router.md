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



