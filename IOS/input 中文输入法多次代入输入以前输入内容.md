```
const handleInputChange =(val)=>{
        const value = getNoneSpace(val) // 不能过滤输入内容  这个是导致问题出现的真实原因
      setInputVal(value);
      if(value.length>0){ 
          setSearchText('取消')
          handleGetSearchListFn(value)
      }
  }
```