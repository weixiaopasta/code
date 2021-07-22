@supports()

@supports CSS at-rule 您可以指定依赖于浏览器中的一个或多个特定的CSS功能的支持声明。这被称为特性查询。该规则可以放在代码的顶层，也可以嵌套在任何其他条件组规则中

#### 检测是否支持指定的 CSS 属性

```
@supports (animation-name: test) {
    … /* 如果支持不带前缀的animation-name,则下面指定的CSS会生效 */
    @keyframes { /* @supports是一个CSS条件组at-rule,它可以包含其他相关的at-rules */
      …
    }
}
```

#### 检测是否不支持指定的CSS属性

```
@supports ( not ((text-align-last:justify) or (-moz-text-align-last:justify) ){
    … /* 这里的CSS代码用来模拟text-align-last:justify */
}
```


### 应用

```
@supports (bottom: constant(safe-area-inset-bottom)) or (bottom: env(safe-area-inset-bottom)) { 
    .bottom-button {
        margin-bottom: constant(safe-area-inset-bottom);
        margin-bottom: env(safe-area-inset-bottom);  
    }
}
```