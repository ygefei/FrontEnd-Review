# CSS REVIEW

### 移动端适配(需整理)

**分辨率**：1334pt x 750pt 表示屏幕上垂直有1334个物理像素，水平有750个物理像素。

**屏幕尺寸**：4.7英寸指的是屏幕对角线的长度，1英寸等于2.54cm。

**屏幕像素密度**：326ppi 指的是每英寸屏幕所拥有的像素数，在显示器中，dpi=ppi。dpi强调的是每英寸多少点。同时，屏幕像素密度=分辨率/屏幕尺寸

**设备独立像素**：CSS像素

**设备像素比**：设备像素比dpr = 设备像素 / css像素（垂直方向或水平方向）。可以通过JS来获取： `window.devicePixelRatio`

布局视口：在移动端设备远大于浏览器窗口，是为了网页在小屏幕上也能展示得很好。

视觉视口：用户看到网页区域内CSS像素的数量。移动端屏幕缩小时，屏幕窗口覆盖的CSS像素变多，视觉视口变大，反之亦然。

理想视口：最理想的布局视口尺寸。设置布局视口宽度为浏览器（屏幕）的宽度。

```html
<meta name="viewport" content="width=device-width,initial-scale=1"> //解决IE兼容性问题
```

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no;"> //禁止缩放
```

width=device-width` 这句代码可以把布局视口设置成为浏览器（屏幕）的宽度。

`initial-scale=1` 的意思是初始缩放的比例是1，使用它的时候，同时也会将布局视口的尺寸设置为缩放后的尺寸。而缩放的尺寸就是基于屏幕的宽度来的，也就起到了和 `width=device-width`同样的效果。



模糊的产生：位图像素是栅格图像（如：png,jpg,gif等）最小的数据单元。理论上来说，**1个位图像素对应1个物理像素，图片才能达到完美清晰的展示**。

以iphone6为例，1个位图像素对应4个物理像素。由于单个位图像素已经是最小的数据单位了，它不能再被进行切割。于是为了能够显示出来，就只能就近取色，从而导致所谓的图片模糊问题。解决的办法十分简单，就是使用跟dpr同个倍数大小的图片。比如iphone6，一个200x300的 `img`标签，原图就要提供400x600的大小。

反向思考一下，如果普通屏幕，也就是dpr为1的屏幕，也使用了两倍的图片，会发生什么样的情况呢？

很明显，在普通屏幕下，200×300的 `img`标签，所对应的物理像素个数就是200×300个，而两倍图片的位图像素个数则是200x300x4，于是就出现一个物理像素点对应4个位图像素点，所以它的取色也只能通过一定的算法进行缩减，显示结果就是一张只有原图像素总数四分之一，肉眼看上去虽然图片不会模糊，但是会觉得有点色差



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
   
   
   
           var desW = 640,
   
   
   
               winW = document.documentElement.clientWidth(移动端兼容处理)||document.body.clientWidth(pc端兼容处理);
   
   
   
               ratio = winW / desW;
   
   
   
           document.documentElement.style.fontSize = ratio * 100 + 'px';
   
   
   
       }
   
   
   
       refreshRem();
   
   
   
       window.addEventListener('resize', refreshRem);
   ```



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