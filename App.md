# App种类

### Native App

​		专为特定手机平台开发的应用程序。由“云服务器数据+APP应用客户端”两部分构成，APP应用所有的UI元素、数据内容、逻辑框架均安装在手机终端上。访问的时候，不需要重新下载加载应用页面框架，只需要加载数据即可。所以加载速度更快，页面响应更快。

- iOS: Object-C / Swift
- Android: Java / Kotlin
- 需要开发者自己把 WebView 控件放到页面上
- 优点：性能和体验好；可以调用系统的所有软件和硬件的API，如GPS，摄像头，麦克风
- 缺点：开发成本高；使用底层操作系统的语言，开发和调试周期长；必须下载安装，升级版本必须重新下载



### Web App

​		使用网页开发的应用程序， 必须在浏览器中使用。WebAPP打开一个页面，都需重新加载页面的所有元素，访问速度受手机终端性能和网络环境的限制，导致加载速度慢，而且操作频繁容易卡死。

- 优点：无需下载安装，打开浏览器即可使用；开发和调试都比较快，不需经过应用商店的批准即可发布
- 缺点：浏览器提供的API有限，大部分系统硬件和硬盘文件都不能通过网页访问；网页通过浏览器渲染，性能不如原生App。



### Hybrid App

​		Native App和Web App的结合。混合 App 的原生外壳称为"容器"，内部隐藏的浏览器，通常使用系统提供的WebView  控件。混合 App 从上到下分成三层：HTML5  网页层、网页引擎层、容器层。

​		原生外壳提供API Bridge（JSbridge）让内部网页调用底层系统的API，再返回结果给网页。

- 优点：跨平台，开发成本低；灵活性大，容易加载外部的H5页面，另一方面可方便调用外部web服务；web页面的调试和构建省时，更新只用在服务器上发布新版本再出发容器内更新即可。
- 缺点：WebView不是全功能浏览器，可能性能欠缺；无法使用特定平台提供的功能。
- 页面本身就是网页，默认在 WebView 中显示。



### React Native App

​		使用JSX编写UI页面，然后编写成各平台的原生App。

​		网页加载：提供一个 WebView 的语法，编译的时候将其换成原生的 WebView。可使用<WebView />组件引入外部的HTML页面。

​		在 React Native 中，一个“原生模块”就是一个实现了“RCTBridgeModule”协议的 Objective-C 类。

​		使用`RCTRootView`将 React Natvie 视图封装到原生组件中。`RCTRootView`是一个`UIView`容器，承载着 React Native 应用。同时它也提供了一个联通原生端和被托管端的接口。

​		在 RN 中，根组件(root components)需要通过`AppRegistry`的`registerComponent`方法进行注册。所谓根组件，就是 Native to JS 的入口，Native 在加载 RN bundle 之后可通过`AppRegistry`的`runApplication`方法运行指定的根组件，从而进入 RN 的世界。



### 小程序

​		针对微信这个特定容器的H5开发 ，微信开发接口（JSbridge)，外部开发这使用规定语法编写页面，可调用微信的各种功能。

