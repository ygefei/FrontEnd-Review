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

