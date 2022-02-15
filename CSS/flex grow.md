#### flex布局概念：

父级元素称为：容器（container）
容器子元素成为：项目（item）

```
<div class="container">
    <div class="box box1">1</div>
    <div class="box box2">2</div>
    <div class="box box3">3</div>
    <div class="box box4">4</div>
    <div class="box box5">5</div>
    <div class="box box6">6</div>
</div>
```

#### flex-grow 项目的放大比例

flex-grow：这是 项目 的一个属性，定义了项目的放大 比例，如果为0，即使有剩余空间也不会放大。

解决的问题：空间有多余时把多余空间分配给各个子元素（项目）。

通俗理解：以上面代码做说明，container有1800px宽度，6个box（item）分别为200px，那么container剩余的宽度则为 600px，flex-grow就是用来设置box怎样瓜分剩余空间（宽度）。
* tips : flex-grow取值为非负数（注：如果取值为负数那么和0的取值效果相同）。


#### 计算公式

概念：将所有正整数相加得到分母sum，每个属性的数值作为分子，然后乘以剩余空间。

### 取整数

公式：分之/分母*剩余空间 

#### 例子1
```
.box1,

.box2,

.box3 {

    flex-grow:1

}
```

这种情况.box1,.box2,.box3 的宽度就是 本身的200 + （1800-200*6）/3 == 400

#### 例子2
```
.box1 {

    flex-grow:1

}

.box2 {

    flex-grow:2

}

.box3 {

    flex-grow:3

}
```

剩余宽度的分母：sum=1+2+3=6

box1分到的宽度：1/6*600px=100px

box2分到的宽度：1/3*600px=200px

box3分到的宽度：1/2*600px=300px

box1-box6的宽度分别为：300px、400px、500px、200px、200px、200px 


### 取小数

概念：小数乘以剩余空间。

公式：小数*剩余空间

```
.box1 {
    flex-grow: 0.1
}
.box2 {
    flex-grow: 0.2
}
```
box1分到的宽度：0.1*600px=60px

box2分到的宽度：0.2*600px=120px

剩余的宽度 600-（60+120）= 420 没有使用

box1-box6的宽度分别为：260px、320px、200px、200px、200px、200px 

#### 小数+正整数
 
 计算方式与正整数一样，如分母取值：sum=0.1+0.2+1=1.3，分子分别为：0.1/1.3、0.2/1.3、1/1.3