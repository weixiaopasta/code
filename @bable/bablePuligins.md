### bable-plugin-import

### 作用
加载babel, antd, lodash等模块, 自动导入对应样式和JS文件


### 使用
```
npm install babel-plugin-import --save-dev
```
```
// .babelrc 
{
  "plugins":[
    [
      "import",
      option
    ]
  ]
}

// 需要实现多个引入, 可以添加多个import配置数组
// .babelrc
{
  "plugins": [
    [
      "import",
      {
        "libraryName": "antd",
        "libraryDirectory": "lib"
      },
      "antd"
    ],
    [
      "import",
      {
        "libraryName": "antd-mobile",
        "libraryDirectory": "lib"
      },
      "antd-mobile"
    ]
  ]
}

```

### 例子
```
1.
{ "libraryName": "antd" }

import { Button } from 'antd';
ReactDOM.render(<Button>xxxx</Button>);

      ↓ ↓ ↓ ↓ ↓ ↓

var _button = require('antd/lib/button');
ReactDOM.render(<_button>xxxx</_button>);

2.
{ "libraryName": "antd", style: "css" }

import { Button } from 'antd';
ReactDOM.render(<Button>xxxx</Button>);

      ↓ ↓ ↓ ↓ ↓ ↓

var _button = require('antd/lib/button');
require('antd/lib/button/style/css');
ReactDOM.render(<_button>xxxx</_button>);

3.{ "libraryName": "antd", style: true }

import { Button } from 'antd';
ReactDOM.render(<Button>xxxx</Button>);

      ↓ ↓ ↓ ↓ ↓ ↓

var _button = require('antd/lib/button');
require('antd/lib/button/style');
ReactDOM.render(<_button>xxxx</_button>);
```

备注 : 配置 style: true 则在项目编译阶段，可以对引入的 antd 样式文件进行编译，从而可以压缩打包尺寸；而配置style: "css", 则直接引入经过打包后的 antd 样式文件