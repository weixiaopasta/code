<img src="imgs/pointer-events.webp" />
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Document</title>
	<style>
		  .login-form {
        position: absolute;
        width: 400px;
        padding: 40px;
        background: #fff;
        box-sizing: border-box;
        box-shadow: 0 15px 25px rgba(160, 149, 149, 0.432);
        border-radius: 10px;
      }

      .login-form h2 {
        margin: 0 0 30px;
        padding: 0;
        color:#000;
        text-align: center;
      }

      .login-form .login-info {
        position: relative;
      }

      .login-form .login-info input {
        width: 100%;
        padding: 10px 10px;
        font-size: 16px;
        color: #000;
        margin-bottom: 30px;
        border: none;
        border: 1px solid #000;
        outline: none;
        background: transparent;
      }
      .login-form .login-info label {
      	pointer-events:none; //重点，label可穿透
        position: absolute;
        top: 0;
        left: 0;
        padding: 10px 10px;
        font-size: 16px;
        color: rgb(15, 20, 25);;
        transition: 0.5s;
      }

 .login-form .login-info input:focus  ~ label {
    top: -20px;
    left: 0;
    color: rgb(29, 155, 240);
    font-size: 12px;
    z-index: 1;
  }
  .login-form .login-info input:focus{
    border: 1px solid rgb(29, 155, 240);
  }

	</style>
</head>
<body>
	
	<div class="login-form">
      <h2>登录</h2>
      <form>
        <div class="login-info">
          <input type="text" name="" required=""/>
          <label>请输入用户名</label>
        </div>
        <div class="login-info">
          <input type="password" name="" required="" />
          <label>请输入密码</label>
        </div>
      </form>
    </div>

</body>
</html>
```