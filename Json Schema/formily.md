Formily对标准schema进行了扩展，扩展之后的schema节点分为三类： 1. 表单控件节点 2. 数据容器节点 3. UI容器节点

### 表单控件节点
1.Formily内置的基础表单控件
2.你自己通过x-component注册的控件

### 数据容器节点
就像javascript的数据类型分为基础和引用类型，Formily的schema节点对应这几种类型。

1. type="string|number|radio|date|datetime|daterange|datetimerange"的时候，对应的有默认表单控件来处理

 2. type="object|array"的时候，如果没有声明x-component，那么它就只是一个数据容器节点，不会有任何UI结构出现。Formily最终组装数据的时候会按照这个声明将数据组装成对应的object|array。


### 理解Form Schema
http://formilyjs.org/#/0yTeT0/jAU8UVSYI8

1. triggerType 
 * 字段校验时机
 * "onChange" | "onBlur"
2. editable
    * 字段是否可编辑
    * boolean
3. visible
    * 字段是否可见（数据+样式）
    * boolean
4. display
    * 字段样式是否可见
    * boolean
5. x-props
    * 字段扩展属性
    * { [name: string]: any }
6. x-index
    * 字段顺序
    * number
7. x-rules
    * 字段校验规则，详细描述可以往后看
    * ValidatePatternRules
    | 'url'
    | 'email'
    | 'ipv6'
    | 'ipv4'
    | 'idcard'
    | 'taodomain'
    | 'qq'
    | 'phone'
    | 'money'
    | 'zh'
    | 'date'
    | 'zip'
    | string
8. x-component
    * 字段 UI 组件名称，大小写不敏感
    * string
9. x-component-props	
    * 字段 UI 组件属性
    * {}
10. x-linkages
    * 字段间联动协议，详细描述可以往后看
    * Array<{ target: FormPathPattern, type: string, [key: string]: any }>
11. x-mega-props
    * 字段布局属性
    * { [name: string]: any }


### x-props 扩展属性

```
{
    addonAfter: FormItem 的尾随内容 - 类型ReactNode,
    itemStyle: style属性 - 类型Object，
    itemClassName:  className 属性 - 类型string,
    triggerType: 配置校验触发类型 "onChange" | "onBlur" | "none" - 类型： string

}
针对组件库的 FormItem 属性 - 比如 labelCol/wrapperCol 等
```



### Form Schema 表达式

Formily 针对 Form Schema 支持了表达式的能力，可以帮助我们在 JSON 字符串中注入一些逻辑能力

type 有三种
value:visible，由值变化控制指定字段显示隐藏
value:schema，由值变化控制指定字段的 schema
value:state，由值变化控制指定字段的状态


```
<SchemaForm
  schema={{
    type: 'object',
    aa: {
      title: '{{customTitle}}',
      description: '{{customDescription}}',
      'x-component-props': {
        placeholder: '{{customPlaceholder}}'
      }
    }
  }}
  expressionScope={{
    customTitle: 'this is custom title',
    customDescription: 'this is custom description',
    customPlaceholder: 'this is custom placeholder'
  }}
/>
```

### Form Schema 联动协议

```
{
  type: "object",
  properties: {
    aaa: {
      type: "string",
      "x-linkages": [
        {
          type: "value:schema",
          target: "bbb",
          condition: '{{ $self.value == "123"}}',//当值为123时发生联动
          schema: {//控制bbb字段的标题，如果不指定condition，默认会走到该处
            title: "这是标题"
          },
          otherwise: {//条件不满足时控制bbb字段标题
            title: ""
          }
        }
      ]
    },
    bbb: {
      type: "string",
      "x-linkages": [
        {
          type: "value:state",
          target: "ccc",
          condition: '{{ $self.value == "123"}}',//当值为123时发生联动
          state:{//控制bbb字段的可编辑状态，如果不指定condition，默认会走到该处
             editable:true
          },
          otherwise:{//条件不满足时控制bbb字段的编辑状态
            editable:false
          }
        }
      ]
    },
    ccc: {
      type: "string"
    }
  }
}
```

