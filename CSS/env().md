```
env( <custom-ident> , <declaration-value>? )
```

env()的第二个可选参数，如果环境变量不可用，该参数就是备用值

```
p {
  width: 300px;
  border: 2px solid red;
  padding:
    env(safe-area-inset-top, 50px)
    env(safe-area-inset-right, 50px)
    env(safe-area-inset-bottom, 50px)
    env(SAFE-AREA-INSET-LEFT, 50px);
}
padding: env(safe-area-inset-bottom, 50px); /* zero for all rectangular user agents */
padding: env(Safe-area-inset-bottom, 50px); /* 50px because UA properties are case sensitive */
padding: env(x, 50px 20px); /* as if padding: '50px 20px' were set because x is not a valid environment variable */
padding: env(x, 50px, 20px); /* ignored because '50px, 20px' is not a valid padding value and x is not a valid environment variable */

```