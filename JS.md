# JavaScript

### Fetch取消加载

```javascript
const controller = new AbortController();

const signal = controller.signal;

fetch(url,signal) 

//3秒后终止请求
setTimeout(()=>{controller.abort();},3000);

//如果请求完成，abort（）不会发生错误
//如果请求完成，fetch会抛出DOMException异常，异常的name属性值为“AbortError”,在promise中的catch捕获
```



### 获取上月最后一天

```javascript
let date = new Date()

let lastMonthLastDate = new Date(date.getYear(), date.getMonth(), 0);
```



### 函数柯里化

柯里化是一种将使用多个参数的一个函数转换成一系列使用一个参数的函数的技术。优点是减少代码冗余，增加可读性。

```javascript
function sum(a){
  return function add(b){
    console.log(a+b);
  }
}


//柯里化实现
function curry(fn,currArgs){
  
  let args = [].slice.call(arguments);
  
  args = args.concat(currArgs);
  
  if(args.length < fn.length){
    return curry(fn,args);
  }
  
  return fn.apply(null,args);
  
}

//test
function sum(a,b,c){
  return a+b+c;
}

let fn = curry(sum);
fn(a,b)(c);
fn(a)(b,c);
```



### call/apply/bind

call()、apply()、bind()是用来改变this的指向的。

三个函数的第一个参数都是this的指向

call的后面传入参数以逗号隔开，apply的后面传入参数则以数组形式，bind与call相同，但是返回函数

call和apply是立即执行函数



#### 用apply实现bind

```javascript
Function.prototype.bind = function(obj){
  let self = this;
  return function(){
    return self.apply(obj,arguments); //arguments作为类数组对象，可作为apply第二个参数
  }
}
```



### arguments转变为数组

```js
var args = Array.prototype.slice.call(arguments);
// Using an array literal is shorter than above but allocates an empty array
var args = [].slice.call(arguments); 
let args = Array.from(arguments);
// or
let args = [...arguments];
```



### null和undefined

null表示"没有对象"，即该处不应该有值。

> （1） 作为函数的参数，表示该函数的参数不是对象。
>
> （2） 作为对象原型链的终点。

```javascript
Object.getPrototypeOf(Object.prototype)
// null
```



undefined表示"缺少值"，就是此处应该有一个值，但是还没有定义

> （1）变量被声明了，但没有赋值时，就等于undefined。
>
> （2) 调用函数时，应该提供的参数没有提供，该参数等于undefined。
>
> （3）对象没有赋值的属性，该属性的值为undefined。
>
> （4）函数没有返回值时，默认返回undefined。
>
> ```javascript
> var i;
> i // undefined
> 
> function f(x){console.log(x)}
> f() // undefined
> 
> var  o = new Object();
> o.p // undefined
> 
> var x = f();
> x // undefined
> ```



### Window对象

window.screenX:  浏览器窗口到操作系统左边的距离

window.innerHeight: 浏览器视口大小，包括滚动条的高度。如用户放大页面，则视口变小

window.outerHeight: 浏览器窗口高度，包括padding(菜单)和border

window.scrollY属性返回页面的垂直滚动距离，单位都为像素

window.open(url, windowName, [windowFeatures])

- `url`：字符串，表示新窗口的网址。如果省略，默认网址就是`about:blank`。
- `windowName`：字符串，表示新窗口的名字。如果该名字的窗口已经存在，则占用该窗口，不再新建窗口。如果省略，就默认使用`_blank`，表示新建一个没有名字的窗口。`_self`表示当前窗口，`_top`表示顶层窗口，`_parent`表示上一层窗口。
- `windowFeatures`：字符串，内容为逗号分隔的键值对。left，top，height，width，outerHeight，outerWidth，menubar，toolbar，location，personalbar，scrollbar等

window.scrollTo(x-coord, y-coord)

window.scrollBy()方法用于将网页滚动指定距离。它接受两个参数：水平向右滚动的像素，垂直向下滚动的像素。

Element.scrollTop()

Element.clientWidth = width+padding



### load和DOMContentLoaded

当初始的 **HTML** 文档被完全加载和解析完成之后，**`DOMContentLoaded`** 事件被触发，而无需等待样式表、图像和子框架的完全加载。另一个不同的事件 `load `应该仅用于检测一个完全加载的页面。



### ES5实现原生类

```javascript
1.判断是否为new调用
function _checkType(obj,constructor){
  if(!(obj instanceof constructor)){
    throw new Error("cannot call a class as a function");
  }
}

2.class内部方法不可枚举
// 修改构造函数描述符
function defineProperties (target, descriptors) {
    for (let descriptor of descriptors) {
        descriptor.enumerable = descriptor.enumerable || false

        descriptor.configurable = true
        if ('value' in descriptor) {
            descriptor.writable = true
        }

        Object.defineProperty(target, descriptor.key, descriptor)
    }
}    

// 构造class
// constructor 表示类对应的constructor对象
// protoDesc 表示class内部定义的方法
// staticDesc 表示class内部定义的静态方法
function _createClass (constructor, protoDesc, staticDesc) {
    protoDesc && defineProperties(constructor.prototype, protoDesc)
    staticDesc && defineProperties(constructor, staticDesc)
    return constructor
}

3.创建class
const Foo = function () {
    function Foo(name) {
        _checkType(this, Foo) // 先检查是不是new调用的

        this.name = name
    }

    _createClass (Foo, [ // 表示在class内部定义的方法
        {
            key: 'say',
            value: function () {
                console.log(this.name)
            }
        }
    ], [ // 表示在class内部定义的静态方法
        {
            key: 'say',
            value: function () {
                console.log('static say')
                console.log(this.name)
            }
        }
    ])

    return Foo
}()



```



### this指向

```javascript
var obj = {
    age:18,
    foo: function(func){
        func();
        arguments[0]();//此时this指向Argument对象，所以age没有为undefined
    }
}

var age = 10;
function temp(){
    console.log(this.age);
}

obj.foo(temp); //10,undefined

//func指向temp，但未改变其this的指向
```



### Hoisting

var可以进行变量提升，let和const不可以

暂时性死区：ES6 明确规定，如果区块中存在`let`和`const`命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。

```javascript
var bar = function(){
    console.log(tmp);
    var tmp = 1;
    try{
        console.log(tmp);//referenceError
        let tmp = 2;
    }catch(error){
        console.log(3);
    }
    console.log(tmp);
}
bar();
```



### AMD和CMD

AMD 是 RequireJS 在推广过程中对模块定义的规范化产出。
CMD 是 SeaJS 在推广过程中对模块定义的规范化产出。

AMD、CMD、CommonJs是ES5中提供的模块化编程的方案，import/export是ES6中定义新增的。

这些规范的实现都能达成**浏览器端模块化开发的目的**。

区别：

1. 对于依赖的模块，AMD 是**提前执行**，CMD 是**延迟执行**。不过 RequireJS 从 2.0 开始，也通过require改成可以延迟执行（根据写法不同，处理方式不同）。CMD 推崇 as lazy as possible.

2. CMD 推崇**依赖就近**，AMD 推崇**依赖前置**。

   ```javascript
   // CMD
   define(function(require, exports, module) {
      var a = require('./a')
      a.doSomething()
      // 此处略去 100 行
      var b = require('./b') // 依赖可以就近书写
      b.doSomething()
      // ... 
   })
   
   // AMD 默认推荐的是
   define(['./a', './b'], function(a, b) {  // 依赖必须一开始就写好
       a.doSomething()
       // 此处略去 100 行
       b.doSomething()
       ...
   }) 
   ```

ES6特性模块化

```javascript
export default {
    props:['name'],
    data () {
        return{ }
    },
    methods:{
        increment(){
            this.$emit('incre');
            import('./../until')
        },
        decrement(){
            this.$emit('decre');
        }
    }
}
```



### Require和Import



![img](https://i.stack.imgur.com/5WgFJ.png)



You **can't** selectively load only the pieces you need with `require` but with `imports`, you can selectively load only the pieces you need. *That can save memory.*

Loading is **synchronous**(step by step) for `require` on the other hand `import` can be asynchronous(without waiting for previous import) so it *can perform a little better than* `require`.



### window.requestAnimationFrame(callback)

因为通过setInterval和setTimeout实现的javascript动画无法保证在可靠时间对UI时间进行更新，所以requestAnimationFrame产生

告诉浏览器——你希望执行一个动画，并且要求浏览器在下次重绘之前调用指定的回调函数更新动画。该方法需要传入一个回调函数作为参数，该回调函数会在浏览器下一次重绘之前执行。

```javascript
function animationWidth() { 
  var div = document.getElementById('box');
  var style = getComputedStyle(div);
  var width = style.getPropertyValue('width'); //获得最终所有css样式，但是只可读
  div.style.width = width + 1 + 'px';

  if(parseInt(div.style.width) < 200) {
    requestAnimationFrame(animationWidth)
  }
}

requestAnimationFrame(animationWidth);
```



### Closure

```javascript
function Foo(){
    var x = 0; //global variable; instance variable: this.x
    Foo.prototype.increment = function(){
        ++x;//closure
        console.log(x);
    }
}

var a = new Foo();
a.increment(); //1
a.increment(); //2
var b = new Foo();//re-declare x=0;
a.increment();//1
```



### JSON.stringify

```javascript
JSON.stringify(NaN) //"null"
var foo = function(){}
JSON.stringify(foo) //undefined
JSON.stringify(null) //"null"
var arr = [1]
JSON.stringify(arr) //"[1]"
var obj = {
  "test": 1,
  "sub":{
    "subTest":2
  }
}
JSON.stringify(obj)//"{"test":1}"
JSON.stringify(undefined)//undefined
```



### 懒加载+节流（待整理）

```javascript
var num = document.getElementByTagName('img').length;
var imgs = document.getElementByTagName('img');
var n = 0;
function lazyLoad(){
  var viewport = document.documentElement.clientHeight || document.body.clientHeight;
  var scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
  for(let i=n; i<num; i++){
    if(imgs[i].offsetTop <= viewport + scrollTop) {
      if(imgs[i].src === null){
        imgs[i].src = imgs[i].getAttribute('data-src');
      }
      n = i+1;
    }else{
      break;
    }
  }
}
lazyLoad();

function throttle(fn,delay,time){
  var timeout,
      startTime = new Date();
  return function(){
    	var context = this;
    	var args = arguments;
    	var currentTime = new Date();
    	clearTimeout(timeout);
    	if(currentTime - startTime > time){
        fn.apply(context,args);
        startTime = currentTime;
      }else{
        timeout = setTimeout(function(){
          fn.apply(context,args)}
        ,delay);
      }
  }
}

window.addEventListener('scroll', throttle(lazyLoad(),500,1000);
```



### Attr()

- 返回属性的值：$(*selector*).attr(*attribute*)

- 设置属性和值：$(*selector*).attr(*attribute,value*)

- 使用函数设置属性和值：$(*selector*).attr(*attribute,*function(*index,currentvalue*))

- 设置多个属性和值：$(*selector*).attr({*attribute*:*value*, *attribute*:*value*,...})

- Notice: Attr()可设置属性"name", "value"，"key"，但"isId"不可。

  ```javascript
  document.getElementById("demo").attributes[0].isId; //检查属性是否为元素的ID属性
  ```

  

