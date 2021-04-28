### useCallback
### useCallback 的作用在于利用 memoize 减少无效的 re-render，

useCallback(fn, inputs) === useMemo(() => fn, inputs))。

React.memo 和 React.useCallback 一定记得需要配对使用，缺了一个都可能导致性能不升反“降”，毕竟无意义的浅比较也是要消耗那么一点点点的性能。