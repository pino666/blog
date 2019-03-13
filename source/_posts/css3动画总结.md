---
title: CSS3 2D/3D总结
date: 2018-07-16 13:10:41
tags: 
    - css
    - css3
    - 动画
cover: /assets/one.jpg
---
{% blockquote %}
本文参考来源于 网络
{% endblockquote %}

## 2D 转换
translate() 方法:根据给定的left(x 坐标)和top(y 坐标)位置参数,元素从其当前位置移动(相对窗口左上角)
<table><tr><td bgcolor=#D1EEEE>  例： translate(50px,100px) :把元素从左侧移动 50 像素，从顶端移动 100 像素</td></tr></table>

rotate() 方法:元素顺时针旋转给定的角度允许负值，元素将逆时针旋转
<table><tr><td bgcolor=#D1EEEE>  例：rotate(30deg) :把元素顺时针旋转 30 度</td></tr></table>

scale() 方法:根据给定的宽度(X 轴)和高度(Y 轴)参数,元素的尺寸会增加或减少
<table><tr><td bgcolor=#D1EEEE>  例；scale(2,4) :把宽度转换为原始尺寸的 2 倍，把高度转换为原始高度的 4 倍</td></tr></table>

skew() 方法:根据给定的水平线(X 轴)和垂直线(Y 轴)参数，元素翻转给定的角度
<table><tr><td bgcolor=#D1EEEE>  例：skew(30deg,20deg) :围绕 X 轴把元素翻转 30 度，围绕 Y 轴翻转 20 度</td></tr></table>

matrix() 方法:把所有 2D 转换方法组合在一起
matrix() 方法需要六个参数，包含数学函数，允许您：旋转、缩放、移动以及倾斜元素

## 3D 转换
rotateX() 方法:元素围绕其 X 轴以给定的度数进行旋转
rotateY() 旋转:元素围绕其 Y 轴以给定的度数进行旋转
transform-style:属性规定如何在 3D 空间中呈现被嵌套的元素
注释：该属性必须与 transform 属性一同使用
<table><tr><td bgcolor=#D1EEEE>  例：transform-style: flat|preserve-3d;  //flat:子元素将不保留其3D位置;preserve-3d:子元素将保留其3D位置</td></tr></table>perspective属性：定义3D元素距视图的距离，以像素计，该属性允许您改变 3D 元素查看 3D 元素的视图
注释：perspective 属性只影响 3D 转换元素
当为元素定义 perspective 属性时，其子元素会获得透视效果，而不是元素本身
提示：请与 perspective-origin 属性一同使用该属性，这样您就能够改变 3D 元素的底部位置
<table><tr><td bgcolor=#D1EEEE>  例：perspective: number|none;   //number:元素距离视图的距离，以像素计;none:默认值与 0 相同,不设置透视
perspective-origin属性：定义3D元素所基于的 X 轴和 Y 轴，该属性允许您改变 3D 元素的底部位置
注释：该属性必须与 perspective 属性一同使用，而且只影响 3D 转换元素
当为元素定义 perspective-origin 属性时，其子元素会获得透视效果，而不是元素本身
例：perspective-origin: x-axis y-axis;
    //x-axis:定义该视图在 x 轴上的位置默认值：50%,可能的值：left,center,right,length,%
    //y-axis:定义该视图在 y 轴上的位置默认值：50%,可能的值：top,center,bottom,length,%
backface-visibility属性:定义当元素不面向屏幕时是否可见
如果在旋转元素不希望看到其背面时，该属性很有用
例：backface-visibility: visible|hidden;    //visible:背面是可见的,hidden:背面是不可见的</td></tr></table>

## 2D/3D 方法
transform属性：向元素应用 2D 或 3D 转换，该属性允许我们对元素进行旋转、缩放、移动或倾斜
<table><tr><td bgcolor=#D1EEEE>   例：transform: none|transform-functions;
        //none：定义不进行转换
        //matrix(n,n,n,n,n,n)：定义 2D 转换，使用六个值的矩阵
        //matrix3d(n,n,n,n,n,n,n,n,n,n,n,n,n,n,n,n)：定义 3D 转换，使用 16 个值的 4x4 矩阵	
        //translate(x,y)：定义 2D 转换
        //translate3d(x,y,z)：定义 3D 转换	
        //translateX(x)：定义转换，只是用 X 轴的值
        //translateY(y)：定义转换，只是用 Y 轴的值
        //translateZ(z)：定义 3D 转换，只是用 Z 轴的值	
        //scale(x,y)：定义 2D 缩放转换
        //scale3d(x,y,z)：定义 3D 缩放转换	
        //scaleX(x)：通过设置 X 轴的值来定义缩放转换
        //scaleY(y)：通过设置 Y 轴的值来定义缩放转换
        //scaleZ(z)：通过设置 Z 轴的值来定义 3D 缩放转换	
        //rotate(angle)：定义 2D 旋转，在参数中规定角度
        //rotate3d(x,y,z,angle)：定义 3D 旋转	
        //rotateX(angle)：定义沿着 X 轴的 3D 旋转
        //rotateY(angle)：定义沿着 Y 轴的 3D 旋转
        //rotateZ(angle)：定义沿着 Z 轴的 3D 旋转
        //skew(x-angle,y-angle)：定义沿着 X 和 Y 轴的 2D 倾斜转换
        //skewX(angle)：定义沿着 X 轴的 2D 倾斜转换
        //skewY(angle)：定义沿着 Y 轴的 2D 倾斜转换
        //perspective(n)：为 3D 转换元素定义透视视图
</td></tr></table>

transform-origin属性：允许您改变被转换元素的位置  
2D 转换元素能够改变元素 x 和 y 轴，3D 转换元素还能改变其 Z 轴
<table><tr><td bgcolor=#D1EEEE>   
transform-origin: x-axis y-axis z-axis;
    //x-axis：定义视图被置于 X 轴的何处可能的值：left，center，right，length，%
    //y-axis：定义视图被置于 Y 轴的何处可能的值：top，center，bottom，length，%
    //z-axis：	定义视图被置于 Z 轴的何处可能的值：length
</td></tr></table>

## 过渡
transition属性：(过渡效果通常在用户将鼠标指针浮动到元素上时发生)
<table><tr><td bgcolor=#D1EEEE>   
transition：简写属性，用于在一个属性中设置四个过渡属性
transition-property：规定应用过渡的 CSS 属性的名称
transition-duration：定义过渡效果花费的时间，默认是 0
transition-timing-function：规定过渡效果的时间曲线，默认是 "ease"
transition-delay：规定过渡效果何时开始，默认是 0
</td></tr></table>

## 动画
@keyframes规则：用于创建动画，在 @keyframes 中规定某项CSS样式，就能创建由当前样式逐渐改为新样式的动画效果
<table><tr><td bgcolor=#D1EEEE> 例：@keyframes myfirst (还可以用百分比，例：0%，10%，100%)
{
from {background: red;}
to {background: yellow;}
}
当您在 @keyframes 中创建动画时，请把它捆绑到某个选择器，否则不会产生动画效果
通过规定至少以下两项 CSS3 动画属性，即可将动画绑定到选择器：规定动画的名称，规定动画的时长
例：把 "myfirst" 动画捆绑到 div 元素，时长：5 秒：
div
{
animation: myfirst 5s; } //缺少其一，动画无效果</td></tr></table>

animation 属性:是一个简写属性，用于设置六个动画属性,除了 animation-play-state 属性
<table><tr><td bgcolor=#D1EEEE>   
animation: name duration timing-function delay iteration-count direction;
animation-name:规定需要绑定到选择器的 keyframe 名称
animation-duration:规定完成动画所花费的时间，以秒或毫秒计.默认是 0
animation-timing-function:规定动画的速度曲线,默认是 "ease"
animation-delay:规定在动画开始之前的延迟,默认是 0
animation-iteration-count:规定动画应该播放的次数,默认是 1
animation-direction:规定是否应该轮流反向播放动画,默认是 "normal"

animation-play-state:规定动画是否正在运行或暂停,默认是 "running"
animation-fill-mode:规定对象动画时间之外的状态
</td></tr></table>





