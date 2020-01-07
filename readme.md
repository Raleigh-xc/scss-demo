>version 1.24.0

- 安装使用
安装Sass
`npm install -g sass`
编译单个文件
`sass ./scss/001.scss ./css/001.css`
如果需要自动检测文件变化
`sass --watch ./scss/001.scss ./css/001.css`
或者自动检测整个文件夹的变化
`sass --watch ./scss:./css`

- Sass还是Scss
Sass使用缩进语法
Scss使用花括号和分号语法，完全箭筒css

- 注释
```
// 当行注释 slient comment 不会生成到css中
/* 多行注释 loud comment */
html{
  font-size: 14px;
  color: #ccc;
}
```
```
@charset "UTF-8";
/* 多行注释 loud comment */
html {
  font-size: 14px;
  color: #ccc;
}
```

- 变量
使用`$`开头
```
$font-size: 14px;
body{
  font-size: $font-size;
}
```
```
body {
  font-size: 14px;
}
```

- 局部变量
```
body{
  $bg-color: #ddd;
  background-color: $bg-color;
}
```
```
body {
  background-color: #ddd;
}
```

- 插值
使用`${}`进行插值操作
```
$font-size: 14px;
body{
  font-size: $font-size;
  line-height: #{$font-size};
}
```
```
body {
  font-size: 14px;
  line-height: 14px;
}
```

- 嵌套
```
$color-info: #909399;
.nav {
  ul {
    padding: 0;
    margin: 0;
    > li {
      list-style: none; 
      border-bottom: 1px solid #ddd; 
      &:last-child {
        border: none;
      }
      &:hover{
        background-color: $color-info;
      }
    }
  }
}
```
```
.nav ul {
  padding: 0;
  margin: 0;
}
.nav ul > li {
  list-style: none;
  border-bottom: 1px solid #ddd;
}
.nav ul > li:last-child {
  border: none;
}
.nav ul > li:hover {
  background-color: #909399;
}
```

- 父级选择器
```
.nav {
  ul {
    padding: 0;
    margin: 0;
    > li {
      list-style: none; 
      border-bottom: 1px solid #ddd; 
    }
  }
  body & {
    padding: 0;
    margin: 0;
  }
  &__list{
    position: relative;
  }
}
```
```
.nav ul {
  padding: 0;
  margin: 0;
}
.nav ul > li {
  list-style: none;
  border-bottom: 1px solid #ddd;
}
body .nav {
  padding: 0;
  margin: 0;
}
.nav__list {
  position: relative;
}
```

- 继承
```
$font-size: 14px;
$color-success: #67C23A;
$color-warning: #E6A23C;
.tips {
  font-size: $font-size;
}
.tips-success {
  @extend .tips;
  color: $color-success;
}
.tips-warning {
  @extend .tips;
  color: $color-warning;
}
```
```
.tips, .tips-warning, .tips-success {
  font-size: 14px;
}

.tips-success {
  color: #67C23A;
}

.tips-warning {
  color: #E6A23C;
}
```

- 占位选择器
使用`%`开头的选择器，该选择器中的样式只在调用的地方生成
```
$color-primary: #409EFF;
$color-danger: #F56C6C;
%btn{
  height: 50px;
  width: 200px;
  border-radius: 5px;
}
.btn-primary{
  @extend %btn;
  background-color: $color-primary;
  color: #fff;
}
.btn-danger{
  @extend %btn;
  background-color: $color-danger;
  color: #fff;
}
```
```
.btn-danger, .btn-primary {
  height: 50px;
  width: 200px;
  border-radius: 5px;
}

.btn-primary {
  background-color: #409EFF;
  color: #fff;
}

.btn-danger {
  background-color: #F56C6C;
  color: #fff;
}
```

- 混入
```
@mixin square($size, $radius: 0) {
  width: $size;
  height: $size;
  @if $radius != 0 {
    border-radius: $radius;
  }
}
.avatar {
  @include square(100px, $radius: 4px)
}
```
```
.avatar {
  width: 100px;
  height: 100px;
  border-radius: 4px;
}
```

- 函数
```
@function transNums($num1, $num2){
  @return ($num1 + $num2)/2;
}
.container{
  font-size: transNums(12px, 24px);
}
```
```
.container {
  font-size: 18px;
}
```

- 模块化
以`_`开头的文件，不会被编译成css文件，使用`@use`引入的时候也不需要写`_`
```
// _common.scss
$bg-color: #ffffff;
@mixin full {
  height: 100%;
  width: 100%;
}
// mian.scss
@use 'common';
.wrap{
  @include common.full;
  background-color: common.$bg-color;
  font-size: 14px;
}
```
```
.wrap {
  height: 100%;
  width: 100%;
  background-color: #ffffff;
  font-size: 14px;
}
```

- 条件语句
```
$color-primary: #409EFF;
$color-success: #67C23A;
$color-warning: #E6A23C;
$color-danger: #F56C6C;
$color-info: #909399;
@mixin theme ($color: primary){
  @if $color == success {
    color: $color-success;
  } @else if $color == warning {
    color: $color-warning;
  } @else if $color == warning {
    color: $color-warning;
  } @else if $color == danger {
    color: $color-danger;
  } @else if $color == info {
    color: $color-info;
  } @else {
    color: $color-primary;
  }
}
.tips {
  @include theme(danger)
}
```
```
.tips {
  color: #F56C6C;
}
```

- each循环
```
$sizes: 40px, 50px, 80px;
@each $size in $sizes {
  .icon-#{$size} {
    font-size: $size;
    height: $size;
    width: $size;
  }
}
```
```
.icon-40px {
  font-size: 40px;
  height: 40px;
  width: 40px;
}
.icon-50px {
  font-size: 50px;
  height: 50px;
  width: 50px;
}
.icon-80px {
  font-size: 80px;
  height: 80px;
  width: 80px;
}
```

- for循环
`@for from to`和`@for from through`的区别就是to是小于，而through是小于等于
```
@for $i from 1 through 3 {
  .mgr#{$i*10}{
    margin-right: $i * 10px;
  }
}
```
```
.mgr10 {
  margin-right: 10px;
}
.mgr20 {
  margin-right: 20px;
}
.mgr30 {
  margin-right: 30px;
}
```

- 操作符
```
.box1{
  width: 100 / 200 * 100%;
}
.box2{
  width: 100px / 200px * 90%;
}
```
```
.box1 {
  width: 50%;
}
.box2 {
  width: 45%;
}

```