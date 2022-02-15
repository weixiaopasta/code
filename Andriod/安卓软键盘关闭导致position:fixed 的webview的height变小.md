https://ishare.58corp.com/articleDetail?id=79698

```
useEffect(() => {
    const isAndroid = flib.env.os == 'android';
    let originHeight = document.documentElement.clientHeight || document.body.clientHeight;
    const handelResize = throttle(() => {
        const resizeHeight =
          document.documentElement.clientHeight || document.body.clientHeight;
        if (originHeight < resizeHeight) {
            // 键盘收起后操作
        } else {
            // 键盘弹起后操作
        }
        originHeight = resizeHeight;
    }, 300);

    if (isAndroid) {
        window.addEventListener('resize',
        handelResize, false);
    };
    return () => {
        if (isAndroid) {
            window.removeEventListener('resize',                handelResize, false);
        };
    };
}, [])

```