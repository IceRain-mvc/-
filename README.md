# 面试之《css盒模型中的魔法》

- 面试题：谈一谈你对CSS盒模型的认识  
  - 小白 content padding margin 2分 
  - 标准盒模型、IE盒模型 2分 
  - css控制这两种盒模型  2分 
  - js如何设置 获取盒模型对应的宽高 //element.style.width  8分
  - 解决边距重叠问题  2分
  - BFC (解决方案) 



## 1. 盒模型 标准盒模型 IE盒模型(怪异盒模型)

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
  <style>
    #box {
      width: 100px;
      height: 100px;
      background-color: blue;
      border: 20px solid #cccccc;

      /*盒模型就是IE盒模型*/
      box-sizing: border-box;
      /*标准盒模型 (默认)*/
      /*box-sizing: content-box;*/
    }

  </style>
</head>
<body>
  <div id="box" >

  </div>

  <script>
    let oBox = document.getElementById("box");

    /*缺点: 通过这种方式 只能获取行内样式 获取不到内嵌和外链样式*/
    // console.log(oBox.style.width);

    /*方式二 通用*/
    console.log(window.getComputedStyle(oBox).width);

    /*方式三 IE独有的*/
    // console.log(oBox.currentStyle.width);

    /*方式四 : 获取一个元素的绝对位置 (区域)*/
    console.log(oBox.getBoundingClientRect().width);



  </script>
</body>
</html>
```



## 2.边界重叠

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
  <style>
    .top {

      background-color: #3aff27;
      margin-bottom: 50px;
      /*BFC*/
      overflow: hidden;
    }

    .inner {
      width: 100px;
      height: 100px;
      background-color: #ff442a;
      margin-bottom: 20px;
    }
    .bottom {
      width: 200px;
      height: 200px;
      background-color: #e92eff;
    }
  </style>
</head>
<body>
  <!--
    标准文档刘中 竖直方向margin不叠加 只取较大的值作为margin  水平方向不存在这个问题

    父容器和子容器在竖直方向上共用了同一个margin  此时就会出现边界重叠 问题
  -->

  <div class="top">
    <div class="inner"></div>

  </div>

  <div class="bottom">

  </div>
</body>
</html>
```

## 3.padding和margin

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
  <style>
    div {
      border: 1px solid #cccccc;
      width: 200px;
      height: 200px;
      background-color: #e92eff;
    }
    p {
      margin-top: 50px;
      height: 100px;
      width: 100px;
      background-color: blue;
    }

  </style>
</head>
<body>

<!--
  如果父盒子 没有 border 那么儿子的margin 实际上踹的是流
  设置了border之后 踹的就是 父盒子

  建议大家分开子盒子和父盒子的方法就是padding

-->
  <div>
    <p></p>
  </div>
</body>
</html>
```

## 4. BFC

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
  <style>
    #box {
      background-color: pink;
    }

    .left {
      width: 100px;
      height: 100px;
      background-color: blue;
      color: #ffffff;
      float: left;
    }

    .right {
      height: 200px;
      background-color: #e92eff;
      color: #ffffff;
      overflow: hidden;
    }


    .father {
      background-color: pink;
      margin-top: 100px;
      overflow: hidden;
    }

    .son {
      float: left;
      background-color: green;
      color: #ffffff;
    }


  </style>
</head>
<body>
<!--
  什么是BFC: 解决方案
  Block Formatting Context 格式化块级上下文
  你可以把他理解成一个独立的区域

  解决那些问题:
    margin重叠
    float区域重叠问题
    清除浮动

  BFC通过什么手段解决上述问题
    最常用的overflow
    浮动中 float 值不为none
    定位中 position 不是static或者relative
    display inline-block table-cell  ... flex inline-flex



-->


<!--
  float区域重叠
  由于右侧div处在一个标准文档流中 比左侧高的时候 导致右侧有一部分跑到下面
  通过BFC就可以解决当前问题 overflow:hidden
-->
<div id="box">
  <div class="left"></div>
  <div class="right">
    人生就像愤怒的小鸟 失败的时候总有猪在笑 人生就像愤怒的小鸟
    失败的时候总有猪在笑 人生就像愤怒的小鸟 失败的时候总有猪在笑 人生就像愤怒的小鸟 失败的时候总有猪在笑 人生就像愤怒的小鸟 失败的时候总有猪在笑 人生就像愤怒的小鸟 失败的时候总有猪在笑 人生就像愤怒的小鸟 失败的时候总有猪在笑
    人生就像愤怒的小鸟 失败的时候总有猪在笑 人生就像愤怒的小鸟
    失败的时候总有猪在笑 人生就像愤怒的小鸟 失败的时候总有猪在笑 人生就像愤怒的小鸟 失败的时候总有猪在笑 人生就像愤怒的小鸟 失败的时候总有猪在笑 人生就像愤怒的小鸟 失败的时候总有猪在笑 人生就像愤怒的小鸟 失败的时候总有猪在笑
    人生就像愤怒的小鸟 失败的时候总有猪在笑
    失败的时候总有猪在笑 人生就像愤怒的小鸟 失败的时候总有猪在笑 人生就像愤怒的小鸟 失败的时候总有猪在笑 人生就像愤怒的小鸟 失败的时候总有猪在笑 人生就像愤怒的小鸟 失败的时候总有猪在笑 人生就像愤怒的小鸟 失败的时候总有猪在笑
    人生就像愤怒的小鸟 失败的时候总有猪在笑
    失败的时候总有猪在笑 人生就像愤怒的小鸟 失败的时候总有猪在笑 人生就像愤怒的小鸟 失败的时候总有猪在笑 人生就像愤怒的小鸟 失败的时候总有猪在笑 人生就像愤怒的小鸟 失败的时候总有猪在笑 人生就像愤怒的小鸟 失败的时候总有猪在笑
    人生就像愤怒的小鸟 失败的时候总有猪在笑
  </div>


</div>
<!--
  清除浮动

  造成这个问题的原因是: 儿子浮动了 由于父亲没有设置高度 导致父亲看不到背景颜色

  - 给父元素设置一个高度  才能关住浮动
  - 通过BFC的方式解决

  计算BFC高度的时候 浮动元素也会参与计算  在计算BFC高度的时候 子元素 float box 也参与计算

-->

<section class="father">
  <div class="son">
    生命一号
  </div>
</section>

</body>
</html>
```