#### FileReader()读取文件

FileReader 对象允许Web应用程序异步读取存储在用户计算机上的文件（或原始数据缓冲区）的内容，使用 File 或 Blob 对象指定要读取的文件或数据。

属性：
FileReader.error 表示在读取文件时发生的错误
FileReader.readyState
FilerReader.result 读取到的结果

#### 下面开始实际例子
index.html如下

```
<!DOCTYPE html>
<html lang="zh">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>FileReader</title>
</head>
<body>
  <input id="input" type="file">
</body>
</html>
```
demo.txt如下
```
this is a demo test
hello world
```

#### 读取txt文件

```
<script>
  const input = document.querySelector('input[type=file]')

  input.addEventListener('change', ()=>{
    const reader = new FileReader()
    reader.readAsText(input.files[0],'utf8') // input.files[0]为第一个文件
    reader.onload = ()=>{
      document.body.innerHTML += reader.result  // reader.result为获取结果
    }
  }, false)
  </script>
```

#### 读取图片文件

```
<script>
  const input = document.querySelector('input[type=file]')

  input.addEventListener('change', ()=>{
    console.log( input.files )
    const reader = new FileReader()
    reader.readAsDataURL(input.files[0]) // input.files[0]为第一个文件
    reader.onload = ()=>{
      const img = new Image()
      img.src = reader.result
      document.body.appendChild(img)  // reader.result为获取结果
    }
  }, false)
  </script>
```

