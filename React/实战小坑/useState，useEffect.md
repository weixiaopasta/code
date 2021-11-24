需求:
根据后端给到的trustNum值（1|0）来控制按钮的颜色
1为改变颜色

```
const [gray, setGray] = useState(!!trustNum)
```

需要额外增加代码

```
useEffect(()=>{
    setGray(gray)
},[trustNum])
```

原因： 刚进来的时候 未拿到接口中的trustNum的时候  我们会给个默认值，有可能是和接口中给到的默认值不相等，所以需要在拿到接口的值之后  重新设置state的状态。