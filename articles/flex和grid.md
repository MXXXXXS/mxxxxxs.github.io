# Flex

# Grid

## 响应式自适应的技巧

在设置行列时, 使用`repeat(auto-fill, 20px)`来不限定行列数, 以自适应. 

进一步, 配合`minmax`不限制行列的宽高, 给定一个范围, 使元素可以铺满, `repeat(auto-fill, minmax(200px, 1fr))`

元素不指定区域, 只确定区域大小, 比如`grid-column: span 3;`

使用`grid-auto-flow`设定自适应流方向.

## 放置grid内元素的方式

### 基于"line"来确定放置区域.

通过在container内元素本身设置. 

"line"通过数字索引, 从"1"开始. 

n条track有n+1条线.

三种写法, 原理相同.

1. ```css
   .card0 {
     grid-column-start: 2;
     grid-column-start: 4;
     grid-row-start: 1;
     grid-row-start: 3;
   }
   ```

2. ```css
   .card0 {
     grid-column: 2 / 4;
     grid-row: 1 / 3;
   }
   ```

3. ```css
   .card0 {
     grid-area: 1 / 2 / 3 / 4;
   }
   ```

   *记法: 从12点钟起逆时针绕一圈, 类似的, 设置边框宽度是12点钟起顺时针绕一圈*

还有一种简写, 适用于指定在某一单独"track", 就不用指定两条"line", 只用一条"line"来标识

```css
.card0 {
  grid-row: 1;
  grid-column: 3;
}
```

等同于

```css
.card0 {
  grid-area: 1 / 3 / 2 / 4;
}
```

### 基于"area"来确定放置区域

通过在grid容器上设置, 前提是各个元素设置了放置区域名称.

先给各个grid内元素设置各自的区域名, 名称随意设置.

```css
.card0 {
  grid-area: area0;
}
.card1 {
  grid-area: area1;
}
.card2 {
  grid-area: area2;
}
```

再设置container的结构

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: 200px 200px;
  grid-template-areas: "area0 area0 area2"
                       "area1 . area2";
}
```

在源码中体现出网络的放置情况, 非常直观. 适用少量元素的设置.

*其中"."指代一个格子, 用来占位*

## 给"line"命名以方便指定某条"line"

### 命名的线方便指定区域

```css
.container {
  display: grid;
  grid-template-columns: [line1-start] 1fr [line2-start] 2fr [line1-end] 1fr [line2-end];
  grid-row-columns: [line1-start] 3fr [line2-start] 1fr [line1-end] 1fr [line2-end];
}
```

在`[]`内的就是对应的线的命名. 

*注意后缀"start"和"end"有特殊含义.*

各条线在书写时的位置和实际网格中的位置对应, 非常形象.

*注意, 在行和列中的线名称前缀都使用相同命名有一个好处, 方便放置元素. 因为可以通过单个名称指定一个区域.*

比如

```css
.card0 {
  grid-area: line1;
}
```

就将`.card0`放置在了`line1`交织出的区域了.

### 一条线可以有多个名称

比如` [main-end footer-start row-5] `空格隔开, 多个名称.

### 多条线可以用同一个名称

在指定具体某一条时, 加上数字, 比如`line 3`就是所有名为`line`的线的第三条

### 线的名称可以由区域名字指定

和先命名线, 再确定区域相反; 先确定区域名, 再加上后缀"start"和"end"来指定该区域的边界线.

## 使用"span", 以不用指定起始/终止"line"

用来放置元素.

## 单步跨越

"span"代表跨越"track"的数量, 比如`span 3`就跨越3条"track". 而使用"line"来描述, 就会像`1 / 4`.

但用"line"描述要指定起始和终止, 才确定一个跨越区域, 而"span"设置的却是一个通用的区域跨越量.

比如

```css
.card0 {
  grid-column: 2 / 8;
  grid-row: 3 / 11;
}
```

可以写成

```css
.card0 {
  grid-column: 2 / span 5;
  grid-row: span 8 / 11;
}
```

只设置起始/终止线的位置, 不用关心另一头的线的位置.

## 多步跨越

"span"可以和同名的一组线一起使用

```css
.card0 {
  grid-column: col 2 / col 7;
  grid-row: content 6 / content 10;
}
```

可以写成

```css
.item {
  grid-column: col 2 / span col 5;
  grid-row: content 6 / span content 4;
}
```

由于默认"span n"是一条一条跨越"n"条"track", 但如果同名的线之间间隔为复数条"track", "span"配合同名的一组线可以大步幅跨越区域.