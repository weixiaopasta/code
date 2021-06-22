### loader的本质

loader从本质上来说其实就是一个node模块。

### loader编写原则

* 单一原则: 每个 Loader 只做一件事；
* 链式调用: Webpack 会按顺序链式调用每个 Loader；
* 统一原则: 遵循 Webpack 制定的设计规则和结构，输入与输出均为字符串，各个 Loader 完全独立，即插即用；
```
npm i -D @babel/parser @babel/traverse @babel/generator @babel/types
```

@babel/parser 将源代码解析成 AST
@babel/traverse 对AST节点进行递归遍历，生成一个便于操作、转换的path对象
@babel/generator 将AST解码生成js代码
@babel/types通过该模块对具体的AST节点进行进行增、删、改、查

新建drop-console.js
```
const parser = require('@babel/parser')
const traverse = require('@babel/traverse').default
const generator = require('@babel/generator').default
const t = require('@babel/types')
module.exports=function(source){
  const ast = parser.parse(source,{ sourceType: 'module'})
  traverse(ast,{
    CallExpression(path){ 
      if(t.isMemberExpression(path.node.callee) && t.isIdentifier(path.node.callee.object, {name: "console"})){
        path.remove()
      }
    }
  })
  const output = generator(ast, {}, source);
  return output.code
} 
```

如何使用
```
const path = require('path')
module.exports = {
  mode:'development',
  entry:path.resolve(__dirname,'index.js'),
  output:{
    filename:'[name].[contenthash].js',
    path:path.resolve(__dirname,'dist')
  },
  module:{
    rules:[{
      test:/\.js$/,
      use:path.resolve(__dirname,'drop-console.js')
      }
    ]
  }
}
```