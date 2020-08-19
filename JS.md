# JavaScript Review

### Fetch取消加载

```javascript
const controller = new AbortController()

const signal = controller.signal

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