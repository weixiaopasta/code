在 class component 时代，为了性能优化我们经常会选择使用 PureComponent,每次对 props 进行一次浅比较，当然，除了 PureComponent 外，我们还可以在 shouldComponentUpdate 中进行更深层次的控制。

在 Function Component 的使用中， React 贴心的提供了 React.memo 这个 HOC（高阶组件），与 PureComponent 很相似，但是是专门给 Function Component 提供的，对 Class Component 并不适用。

但是相比于 PureComponent ，React.memo() 可以支持指定一个参数，可以相当于 shouldComponentUpdate 的作用，因此 React.memo() 相对于 PureComponent 来说，用法更加方便。
```
function MyComponent(props) {
  /* render using props */
}
function areEqual(prevProps, nextProps) {
  /*
  return true if passing nextProps to render would return
  the same result as passing prevProps to render,
  otherwise return false
  */
}
export default React.memo(MyComponent, areEqual);
```