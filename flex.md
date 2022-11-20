# flex

这个说得非常好，形象


![image_Hxs08nCMsc.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1663901203368-d1761989-630b-4e39-b18f-e05ade33b5c7.png#clientId=ue9075889-ce0f-4&crop=0&crop=0&crop=1&crop=1&from=ui&id=u7e6c42cf&name=image_Hxs08nCMsc.png&originHeight=423&originWidth=744&originalType=binary&ratio=1&rotation=0&showTitle=false&size=50191&status=done&style=none&taskId=u8e48893f-abdc-4662-ad1e-795d8ca2f50&title=)

flex 容器的所有子元素自动成为容器成员。

```css
/* Webkit 内核的浏览器必须加上 -webkit 前缀 */
/* .Webkit { */
    /* display: -webkit-flex; */
    /* display: flex; */
/* }  */
```

flex 窗口默认存在两根轴：水平的轴和垂直的交叉轴

- flex-direction 决定主轴的方向


```css
/* 决定主轴的方向（就是排列方向） 
row 默认：主轴为水平方向，起点左端
row-reverse 主轴为水平方向，起点在后端 
column  主轴为垂直方向，起点在上沿
column 主轴为垂直方向，起点在下沿
*/
  flex-direction: row;
```

- flex-wrap 属性

```css
  /* 决定容器内项目是否可换行
  nowrap 默认，不换行，当空间不足时，项目尺寸会随着调整而并不会被挤到下一行
  wrap 允许换行，第一行在上方
  wrap-reverse 换行，第一行在下方

  */
    /* flex-wrap: nowrap; */
```


- flex-flow

```css
/*这个属性是 flex-direction 和 flex-wrap 的简写形式
    默认值为 row nowrap 
    */
    /* flex-flow: column; */
```

- justify-context 属性

```css
/* 决定了项目在主轴的对齐方式
默认值 flex-start 左对齐
center 居中
space-between 两端对齐，项目之间的间隔相等，就是剩余空间分成间隙
space-around 两个项目两侧的间隔相等。
*/
    /* justify-content: baseline; */
```


- align-items

```css
 /* 决定项目在交叉轴上的对齐方式
    默认值 stretch 如果项目没有设置高度或者为 auto ，将会占满整个窗口的高度
    flex-start 交叉轴的起点对齐
    flex-end 交叉轴的终点对齐
    center 交叉轴的中点对齐
    baseline 项目的第一行文字的基线对齐

    */
    /* align-items: baseline; */
```


- align-content

定义了多根轴线的对齐方式。项目只有一根轴线，这个属性不起作用


### 项目属性

有 6 个属性可以设置在项目上：

1.  order 
   1. 决定了项目的排列顺序。数值越小排列越靠前，默认为0；

2.  flex-grow 
   1. 决定了项目的放大比例。默认为0；就是各个组件的占比。

3.  flex-shrink 
   1. 决定了项目的缩小比例，默认为1，不能为负值。

4.  flex-basis 
   1. 决定了在分配多余空间之前，项目占据的主轴空间（main size)。默认值为 auto；可以设置 px 值，即项目占据固定空间
5.  flex
它是 flex-grow ， flex-shrink ，flex-basis 的简写，默认值为 0 1 auto 后面两个属性可选；它有两个快捷值 auto （1 1 auto)  none (0 0  auto) 
6.  align-self 
   1. 决定允许单个项目有与其他项目不一样的对齐方式，可覆盖 align-items 属性，默认值为 auto ，表示继承父元素的 align-items 属性，没有父元素等同于 stretch；可取 6 个值，除了 auto 其他和  align-items 属性一致


