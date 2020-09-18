# CSS

### 移动端适配(需整理)

#### 分辨率和像素

- 分辨率：1334pt x 750pt 表示屏幕上垂直有1334个物理像素，水平有750个物理像素。

- 屏幕尺寸：4.7英寸指的是屏幕对角线的长度，1英寸等于2.54cm。

- 屏幕像素密度：326ppi 指的是每英寸屏幕所拥有的像素数，在显示器中，dpi=ppi。dpi强调的是每英寸多少点。同时，屏幕像素密度=分辨率/屏幕尺寸

- 设备独立像素：CSS像素与设备独立像素之间的关系依赖于当前的缩放等级；

- 设备像素比：设备像素比dpr = 设备像素 / css像素（垂直方向或水平方向）。

  在web中，浏览器为我们提供了window.devicePixelRatio来帮助我们获取dpr。在css中，可以使用媒体查询min-device-pixel-ratio，区分dpr

  CSS的1px等于几个物理像素，除了和屏幕像素密度dpr有关，还和用户缩放有关系。例如，当用户把页面放大一倍，那么CSS中1px所代表的物理像素也会增加一倍；反之把页面缩小一倍，CSS中1px所代表的物理像素也会减少一倍。

#### 视口

- 布局视口：DOM宽度。在移动端设备远大于浏览器窗口，是为了网页在小屏幕上也能展示得很好。

  ```html
  document.documentElement.clientWidth/Height
  ```

- 视觉视口：屏幕宽度。用户看到网页区域内CSS像素的数量。移动端屏幕缩小时，屏幕窗口覆盖的CSS像素变多，视觉视口变大，反之亦然。 

  ```html
  window.innerWidth/Height
  ```

- 理想视口：最理想的布局视口尺寸。设置布局视口宽度为浏览器（屏幕）的宽度。

```html
<meta name="viewport" content="width=device-width,initial-scale=1"> //解决IE兼容性问题
```

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no;"> //禁止缩放

当前缩放值 = ideal viewport宽度  / visual viewport宽度
```

- width: 设置layout viewport 的宽度，为一个正整数，或字符串”width-device”。
- initial-scale: 设置页面的初始缩放值，为一个数字，可以带小数。
- minimum-scale: 允许用户的最小缩放值，为一个数字，可以带小数。
- maximum-scale: 允许用户的最大缩放值，为一个数字，可以带小数。
- height: 设置layout viewport 的高度，这个属性对我们并不重要，很少使用。
- user-scalable: 是否允许用户进行缩放，值为“no”或“yes”。



模糊的产生：位图像素是栅格图像（如：png,jpg,gif等）最小的数据单元。理论上来说，**1个位图像素对应1个物理像素，图片才能达到完美清晰的展示**。

以iphone6为例，1个位图像素对应4个物理像素。由于单个位图像素已经是最小的数据单位了，它不能再被进行切割。于是为了能够显示出来，就只能就近取色，从而导致所谓的图片模糊问题。解决的办法十分简单，就是使用跟dpr同个倍数大小的图片。比如iphone6，一个200x300的 `img`标签，原图就要提供400x600的大小。

反向思考一下，如果普通屏幕，也就是dpr为1的屏幕，也使用了两倍的图片，会发生什么样的情况呢？

很明显，在普通屏幕下，200×300的 `img`标签，所对应的物理像素个数就是200×300个，而两倍图片的位图像素个数则是200x300x4，于是就出现一个物理像素点对应4个位图像素点，所以它的取色也只能通过一定的算法进行缩减，显示结果就是一张只有原图像素总数四分之一，肉眼看上去虽然图片不会模糊，但是会觉得有点色差



#### 适配方案

1. 流式布局（百分比）

2. 响应式布局（css媒体查询）

3. flex布局（rem)

   根据UI设计宽度，还原尺寸，对宽和高用rem设置，然后flexible.js会通过dpr对设备自动缩放

   rem就是根元素（即：html）的字体大小。默认情况下html的1rem = 16px。

   浏览器一般都有最小字体限制，比如谷歌浏览器，最小中文字体就是12px，所以实际上没有办法让1rem=1px。

   阿里团队的高清方案布局代码：所谓高清方案就是利用rem的特性（我们知道默认情况下html的1rem = 16px），根据设备屏幕的DPR（设备像素比，又称DPPX，比如dpr=2时，表示1个CSS像素由4个物理像素点组成）**根据设备DPR动态设置 html 的font-size为（50 \* dpr)，同时调整页面的压缩比率（即：1/dpr），进而达到高清效果**。

   Html font-size = winW/desW*100

   ```javascript
   function refreshRem() {
   
   
   
           var desW = 640,//设计图是css像素还是物理像素
   
   
   
               winW = document.documentElement.clientWidth(移动端兼容处理)||document.body.clientWidth(pc端兼容处理);
   
   
   
               ratio = winW / desW;
   
   
   
           document.documentElement.style.fontSize = ratio * 100 + 'px';
   
   
   
       }
   
   
   
       refreshRem();
   
   
   
       window.addEventListener('resize', refreshRem);
   ```



### 水平垂直居中

1. position: absolute，元素已知宽度：绝对定位+margin反向偏移

```css
.wrap {
    position: relative;
    background-color: orange;
    width: 300px;
    height: 300px;
}
.item {
    background-color: red;
    width: 100px;
    height: 100px;
    position: absolute;
    left: 50%;
    top: 50%;
    margin: -50px 0 0 -50px;
}
```

2. position+transform, 元素未知宽度：将**margin: -50px 0 0 -50px**替换为**transform: translate(-50%,-50%)**

3. flex布局

   ```CSS
   .container {
     background-color: #FF8C00;
     width: 200px;
     height: 200px;
     display: flex;
     justify-content: center; 
     align-items: center; 
   }
   ```

4. 绝对布局

   绝对定位元素：

   ```
   left + margin-left + border-left-width + padding-left + width + padding-right + border-right-width + margin-right + right = width of containing block
   ```

   ```
   top + margin-top + border-top-width + padding-top + height + padding-bottom + border-bottom-width + margin-bottom + bottom = height of containing block
   ```

   ```css
   .warp {
     position: relative;
     background-color: orange;
     width: 200px;
     height: 200px;
   }
   .example3 {
     position: absolute;
     top: 0;
     left: 0;
     right: 0;
     bottom: 0;
     background-color: red;
     width: 100px;
     height: 100px;
     margin: auto;
   }
   //如果未设置宽度，则宽度默认为父元素的宽度
   //如果未设置高度，则宽度默认为父元素的高度；如果宽度和高度都未设置，则充满整个父元素。
   ```



### 三栏布局

1.float布局

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>三栏布局——左右浮动布局</title>
  <style>
    * {
      margin: 0;
      padding: 0;
    }
    body {
      min-width: 550px;
    }
    .column {
      min-height: 100px;
    }
    .left {
      float: left;
      width: 200px;
      background: #ffbbff;
    }
    .center {
      margin: 0 150px 0 200px;
      background: #bfefff;
    }
    .right {
      float: right;
      width: 150px;
      background: #eeee00;
    }
     .container::after{
      content:" ";
      display: block;
      clear: both;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="column left">left</div>
    <div class="column right">right</div>
    <div class="column center">center</div>
  </div>
</body>
</html>
```

2.双飞翼布局

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>三栏布局——双飞翼布局</title>
  <style>
    * {
      margin: 0;
      padding: 0;
    }
    body {
      min-width: 550px;
    }
    .container {
      overflow: hidden;
    }
    .column {
      float: left;
      min-height: 100px;
    }
    .left {
      margin-left: -100%;
      width: 200px;
      background: #ffbbff;
    }
    .center {
      width: 100%;
    }
    .center-inner {
      margin: 0 150px 0 200px;
      min-height: 100px;
      background: #bfefff;
    }
    .right {
      margin-left: -150px;
      width: 150px;
      background: #eeee00;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="column center">
      <div class="center-inner">center</div>
    </div>
    <div class="column left">left</div>
    <div class="column right">right</div>
  </div>
</body>
</html>
```

3.Flex布局

```css
.main {
    display: flex;
  	flex-direction: row;
}
.left{
    width: 400px;
    background-color: red;
}
.center{
    background-color: blue;
    flex: 1;
}
.right{
    background-color: red;
    width: 400px;
}
```

4.grid布局

```css
.div{
    width: 100%;
    display: grid;
    grid-template-rows: 100px;
    grid-template-columns: 300px auto 300px;
}
```

5.table布局

```css
.main{
    width: 100%;
    display: table;
}
.left,.center,.right{
    display: table-cell;
}
.left{
    width: 300px;
    background-color: red;
}
.center{
    background-color: blue;
}
.right{
    width: 300px;
    background-color: red;
}
```





### 回流和重绘

#### 浏览器渲染

![webkit渲染过程](https://segmentfault.com/img/remote/1460000017329983?w=624&h=289)

1. 解析HTML，生成DOM树，解析CSS，生成CSSOM树
2. 将DOM树和CSSOM树结合，生成渲染树(Render Tree)
3. Layout(回流):根据生成的渲染树，进行回流(Layout)，得到节点的几何信息（位置，大小）
4. Painting(重绘):根据渲染树以及回流得到的几何信息，得到节点的绝对像素
5. Display:将像素发送给GPU，展示在页面上。

为了构建渲染树，浏览器主要完成了以下工作：

1. 从DOM树的根节点开始遍历每个可见节点。
2. 对于每个可见的节点，找到CSSOM树中对应的规则，并应用它们。
3. 根据每个可见节点以及其对应的样式，组合生成渲染树。

不可见的节点包括：

- 一些不会渲染输出的节点，比如script、meta、link等。
- 一些通过css进行隐藏的节点。注意，利用visibility和opacity隐藏的节点，还是会显示在渲染树上的。只有display:none的节点才不会显示在渲染树上。

#### 回流

计算可见节点在设备视口(viewport)内的确切位置和大小

- 添加或删除可见的DOM元素
- 元素的位置发生变化
- 元素的尺寸发生变化（包括外边距、内边框、边框大小、高度和宽度等）
- 内容发生变化，比如文本变化或图片被另一个不同尺寸的图片所替代。
- 页面一开始渲染的时候
- 浏览器的窗口尺寸变化（因为回流是根据视口的大小来计算元素的位置和大小的）
- 获取布局信息的操作的时候，会强制浏览器处理队列刷新
  - offsetTop、offsetLeft、offsetWidth、offsetHeight
  - scrollTop、scrollLeft、scrollWidth、scrollHeight
  - clientTop、clientLeft、clientWidth、clientHeight
  - getComputedStyle()
  - getBoundingClientRect()

#### 重绘

将渲染树的每个节点都转换为屏幕上的实际像素

#### 减少回流和重绘

- 为了减少发生次数，我们可以合并多次对DOM和样式的修改，然后统一处理

  ```javascript
  //使用cssText
  const el = document.getElementById('test');
  el.style.cssText += 'border-left: 1px; border-right: 2px; padding: 5px;';
  //修改class
  const el = document.getElementById('test');
  el.className += ' active';
  ```

- 批量修改DOM

  1. 使元素脱离文档流

     - 隐藏元素，应用修改，重新显示

       ```javascript
       function appendDataToElement(appendToElement, data) {
           let li;
           for (let i = 0; i < data.length; i++) {
               li = document.createElement('li');
               li.textContent = 'text';
               appendToElement.appendChild(li);
           }
       }
       const ul = document.getElementById('list');
       ul.style.display = 'none';
       appendDataToElement(ul, data);
       ul.style.display = 'block';
       ```

     - 使用文档片段(document fragment)在当前DOM之外构建一个子树，再把它拷贝回文档。

       ```javascript
       const ul = document.getElementById('list');
       const fragment = document.createDocumentFragment();
       appendDataToElement(fragment, data);
       ul.appendChild(fragment);
       ```

     - 将原始元素拷贝到一个脱离文档的节点中，修改节点后，再替换原始的元素。

       ```javascript
       const ul = document.getElementById('list');
       const clone = ul.cloneNode(true);
       appendDataToElement(clone, data);
       ul.parentNode.replaceChild(clone, ul);
       ```

  2. 对其进行多次修改

  3. 将元素带回到文档中

- 复杂动画效果，使用绝对定位让其脱离文档流

  对于复杂动画效果，由于会经常的引起回流重绘，因此，我们可以使用绝对定位，让它脱离文档流。否则会引起父元素以及后续元素频繁的回流。

- css3硬件加速

  使用css3硬件加速，可以让transform、opacity、filters这些动画不会引起回流重绘 。但是对于动画的其它属性，比如background-color这些，还是会引起回流重绘的，不过它还是可以提升这些动画的性能。

### 动画

#### transition

同时改变宽高

```css
img{
    transition: 1s height, 1s width; 
} 
```

速度：

- ease: 逐渐放慢
- linear：匀速
- ease-in：加速
- ease-out：减速
- cubic-bezier函数：自定义速度模式

```css
img{
    transition: 1s ease;
}
```

局限性：

- transition需要事件触发，所以没法在网页加载时自动发生。
- transition是一次性的，不能重复发生，除非一再触发。
- transition只能定义开始状态和结束状态，不能定义中间状态，也就是说只有两个状态。
- 一条transition规则，只能定义一个属性的变化，不能涉及多个属性

#### Animation

无限循环

```css
div:hover {
  animation: 1s rainbow infinite;
}

@keyframes rainbow {
  0% { background: #c00; }
  50% { background: orange; }
  100% { background: yellowgreen; }
}
```

保持结束状态

> ```css
> div:hover {
>   animation: 1s rainbow forwards;
> }
> ```



> ```css
> 
> div:hover {
>   animation: 1s 1s rainbow linear 3 forwards normal;
> }
> 
> 
> div:hover {
>   animation-name: rainbow;
>   animation-duration: 1s;
>   animation-timing-function: linear;
>   animation-delay: 1s;
>   animation-fill-mode:forwards;
>   animation-direction: normal;
>   animation-iteration-count: 3;
> }
> ```



### transform

rotate

```css
#demo {
   transform:rotate(180deg)  ;/*实现旋转,左上角的东西会在右下角显示*/
}
```

scale

```css
#demo {
  transform:scale(1.2);/*放大1.2倍*/
  transform:scale(.8);/*缩小为正常的0.8倍*/
}
```

skew

```css
#demo {
  transform:skew(45deg);/*文本沿x轴向左倾斜45°*/
  transform:skew(0,45deg);/*文本沿y轴向下倾斜45°*/
}
```

translate

```css
#demo {
  transform:translate(20px,5vh);/*向左移动二十像素,向下移动百分之五的视窗高度*/
}
```



### 1px边框适配解决方案：CSS媒体查询进行缩放

```css
@media only screen and (-webkit-min-device-pixel-ratio:2.0){
  .border-bottom::after{
    -webkit-transform: scaleY(0.5);
		transform: scaleY(0.5);
  }
}
@media only screen and (-webkit-min-device-pixel-ratio:3.0){
		.border-bottom::after{
    -webkit-transform: scaleY(0.33);
		transform: scaleY(0.33);
  }
}
```



### 防抖

```javascript
let debounce = function(fn,delay){
  let timer = null;
  return function(){
    const context = this;
    const args = arguments;
    clearTimeout(timer);
    timer = setTimeout({
     	fn.apply(context,args);
    },delay)
  }
}
```



### 节流

```javascript
const throttle = function(fn,delay){
  let wait = false;
  return function(){
    const context = this;
    const args = arguments;
    if(!wait){
      wait = true;
      setTimeout({
        fn.apply(context,args);
        wait = false;
      },delay)
    }
  }
}
```



### 雪碧图

雪碧图也叫CSS精灵， 是一种CSS图像合成技术；

#### 优点

- 将多张图片合并到一张图片中，可以减小图片的总大小。

- 将多张图片合并成一张图片后，下载全部所需的资源，只需一次请求。可以减小建立连接的消耗。

  #### 缺点

- 在宽屏，高分辨率的屏幕下的自适应页面，雪碧图如果不够宽，容易出现背景断裂
- 在开发的时候，需要通过photoshop或其他工具测量计算每一个背景单元的精确位置
- 在维护的时候比较麻烦，如果页面背景有少许改动，一般就要修改整张合并的图片

```css
.box{width:208px; height:202px; background:url(img/all.png) no-repeat -32px -48px;}
```



### Box Model

box-sizing: content-box 标准盒子模型

box-sizing: border-box 怪异盒子模型



### CSS选择器优先级

```CSS
//两个样式优先级相同，选后者
<style>
    span.inner{
        color:"red";
    }
    .text span{
        color:"black";
    }
</style>
</head>
<body>
    <span class="text">
        <span class="inner">Text</span> //黑色
    </span>
</body>
```



### box-shadow

```css
box-shadow: offset-x offset-y blur spread color inset;
```

- `<offset-x>` 设置水平偏移量，如果是负值则阴影位于元素左边。 

- `<offset-y>` 设置垂直偏移量，如果是负值则阴影位于元素上面。如果两者都是0，那么阴影位于元素后面。这时如果设置了`<blur-radius>` `或<spread-radius>` 则有模糊效果。

- `<blur-radius>模糊半径`值越大，模糊面积越大，阴影就越大越淡。 不能为负值。默认为0，此时阴影边缘锐利。

- `<spread-radius>扩展半径`取正值时，阴影扩大；取负值时，阴影收缩。默认为0，此时阴影与元素同样大。

- `<color>`如果没有指定，则由浏览器决定——通常是[`color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/color)的值，不过目前Safari取透明。

- `<inset>`如果没有指定`inset`，默认阴影在边框外，即阴影向外扩散。使用 `inset` 关键字会使得阴影落在盒子内部，这样看起来就像是内容被压低了。 此时阴影会在边框之内 (即使是透明边框）、背景之上、内容之下。

- 若要对同一个元素添加多个阴影效果，请使用逗号将每个阴影规则分隔开。

- 单边阴影（待整理）

  ```css
  .left{
         box-shadow:-5px 0 10px -5px #00ff00;
  }
  .bottom{
         box-shadow:0 5px 10px -5px #00ff00;
  }
  .right{
         box-shadow:5px 0 10px -5px #00ff00;
  }
  .top{
         box-shadow:0px -5px 10px -5px #00ff00;
  }
  ```

  

