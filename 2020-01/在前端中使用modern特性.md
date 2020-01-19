---
title: 在前端中使用modern特性
author: '@azurity'
config:
    loop: false
style-config:
    auto-invert: false
highlight-theme: 'https://cdn.bootcss.com/highlight.js/9.15.10/styles/solarized-dark.min.css'
---
<slide/>

# 在前端中使用modern特性

##### **我觉得拖拉机海星:第一次技术分享**{.gray}

[@azurity](https://github.com/azurity)

<slide/>
{data-step-count=3}

# {一|wàn}{切|è}{之|zhī}{始|yuán}

- ### {ECMAScript|JavaScript} 6
    :::div{.animate-step .zoomIn .fast data-step=1}
    > 参见:[ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
    > 
    到2012年为止，所有的浏览器均已支持ES5.1，ES终于消除了历史遗留问题。
    2015年ES2015发布，之后开启了ES每年一个版本的快速发展期。
    实际上，并不存在真正的一个ES6，ES6只是ES5.1之后迭代更新的语言的代名词。
    称之为`ES Next`也不过分。
    :::
- ### HTML 5
    :::div{.animate-step .zoomIn .fast data-step=2}
    > 参见:[HTML](https://html.spec.whatwg.org/multipage/)
    > 
    W3C和WHATWG在漫长的斗争之后（实际上就是标准委员会及各个浏览器厂商之间的斗争），终于联手开始推进HTML标准，
    即HTML5（实际上2019年才签订谅解备忘录）。然后HTML也搞起了实时更新，每个功能模块的更新是独立进行的。
    :::
- ### CSS 3
    :::div{.animate-step .zoomIn .fast data-step=3}
    > 参见:[CSS](https://www.w3.org/Style/CSS/#specs)
    > 
    CSS实际上也是各个模块分别进行推进的，CSS3永远是未完成的标准，每年都有新的特性加入CSS标准。
    :::
{.flexblock .clients}

<slide/>
{data-step-count=4}

#### 考虑到以上事实{.animate-step .zoomIn .fast data-step=1}

#### 每一个技术都是模块化的{.animate-step .zoomIn .fast data-step=2}

#### 意味着每个浏览器都会支持一组截然不同的特性集{.animate-step .zoomIn .fast data-step=3}

#### 你真的可能适配的过来吗？{.animate-step .zoomIn .fast data-step=4}

==这就是在前端使用modern特性的巨大问题=={.animate-step .zoomIn .fast data-step=4}

<slide/>
{data-step-count=9}

因此，大量杂七杂八的`utility`开始出现

- [新的编程语言/方言]{.animate-step .zoomIn .fast data-step=1}
- [用于语言特性回退的`polyfill`]{.animate-step .zoomIn .fast data-step=2}
- [用于处理语言特性回退的编译器]{.animate-step .zoomIn .fast data-step=3}
- [用于其他特性回退的`polyfill`]{.animate-step .zoomIn .fast data-step=4}
- [用于将这些`polyfill`和浏览器支持特性匹配的工具`browserslist`]{.animate-step .zoomIn .fast data-step=5}
- [用于组织这些工作有序进行的脚手架]{.animate-step .zoomIn .fast data-step=6}
- [用于将结果整合的打包工具]{.animate-step .zoomIn .fast data-step=7}
- [用于安装以上工具的包管理器]{.animate-step .zoomIn .fast data-step=8}

**这些均是前端工程化的组成部分，也是在前端使用modern特性的基础**{.animate-step .zoomIn .fast data-step=9}

<slide/>
{data-step-count=1}

#### **不过本次的内容不是介绍前端工程化👻**{.animate-step .zoomOut .fast data-step=1}

# 有趣的modern特性{.animate-step .zoomIn .fast data-step=1}

> 先假设所有特性都已经能用了😆{.animate-step .zoomIn .fast data-step=1 .gray}

> 

<slide/>

# ES部分

<slide/>

**先从不modern的讲起**

<slide/>
{data-step-count=5}

#### 各种异步:`Promise`,`generator-yeild`,`async-await`

::::::div{style="display:flex;height:80vh"}
::::div{.animate-step .zoomOut .fast data-step=2 style="position:absolute;width:100%"}
:::div{.animate-step .zoomIn .fast data-step=1}
```js
// classical
a({/*arguments*/}, function () {
    /*sth.*/
    b(/*arguments*/, function () {
        /*sth.*/
        c(/*arguments*/, function () {
            /*& so on.*/
        })
    })
})
```
**THE CALLBACK HALL**
:::
::::

::::div{{.animate-step .zoomOut .fast data-step=3 style="position:absolute;width:100%"}
:::div{.animate-step .zoomIn .fast data-step=2}
```js
// promise style
new Promise(a(/*argmunents*/))
    .then(function (resolve, reject) {
        /*sth.*/
        resolve(b(/*arguments*/))
    })
    .then(function (resolve, reject) {
        /*sth.*/
        resolve(c(/*arguments*/))
    })
    /*& so on.*/
```
**CHAIN IS OK, BUT**
:::
::::

:::::div{{.animate-step .zoomOut .fast data-step=4 style="position:absolute;width:100%"}
::::div{.animate-step .zoomIn .fast data-step=3}
:::div{style="display:flex;flex-direction:row"}
```js
// generator like
function* gen() {
    yeild a(/*arguments*/)
    /*sth.*/
    yeild b(/*arguments*/)
    /*sth.*/
    yeild c(/*arguments*/)
    /*& so on.*/
    return
}
```
```js
(function run(gen) {
    const item = gen.next()
    if (item.done) {
        return item.value
    }
    const { value, done } = item
    if (value instanceof Promise) {
        value.then((e) => run(gen))
    } else {
        run(gen)
    }
})(gen)
```
:::
**WRITE AS SYNC, AND COMPLEX**
::::
:::::

::::div{{.animate-step .zoomOut .fast data-step=5 style="position:absolute;width:100%"}
:::div{.animate-step .zoomIn .fast data-step=4}
```js
// async like
async function () {
    await a(/*arguments*/)
    /*sth.*/
    await b(/*arguments*/)
    /*sth.*/
    await c(/*arguments*/)
    /*& so on.*/
}
```
**最佳实践**
:::
::::
::::::

<slide/>
{data-step-count=3}

#### 来自标准委员会的模块化方案: ES module

:::::div{style="display:flex;height:80vh"}
::::div{{.animate-step .zoomOut .fast data-step=2 style="position:absolute;width:100%"}
:::div{.animate-step .zoomIn .fast data-step=1}
**各种传统方案**
```js
//CommonJS
const a = require('a')
let b = 0
module.exports = b
```
```js
//AMD(基于RequireJS)
define("b", ["require", "exports", "a"], function (require, exports, a) {
    const a = require('a')
    let b = 0
    exports = b
})
```
```js
//CMD(基于SeaJS)
define('b', ['a'], function(require, exports, module) {
    const a = require('a')
    let b = 0
    module.exports = b
})
```
:::
::::
::::div{{.animate-step .zoomOut .fast data-step=3 style="position:absolute;width:100%"}
:::div{.animate-step .zoomIn .fast data-step=2}
**ES module**
```js
//
import a from 'a'
let b = 0
export default b
// 当然default特性主要用于兼容旧模块，这是后话
// 当然还可以愉快的异步啦
import('c').then((c) => {
    /*do sth.*/
})
```
:::
::::
:::::

<slide/>

#### ES2020的BigInt

**大家都知道，在js里**🤔
```js
9007199254740992 === 9007199254740993 // true
```

**ES2020终于决定决绝这个问题**😀
```js
9007199254740992n === 9007199254740993n // false
```

<slide/>

#### 万众期盼的可选链和Nullish（貌似有人等了5年）
> ES2020收官特性

```js
// classical
let c = a && a.b && a.b.c && a.b.c.d && a.b.c.d != null && a.b.c.d || 'default'
/*😒*/
// now
let c = a?.b?.c?.d ?? 'default'
/*😆*/
```

<slide/>

# HTML和Web API部分

[Web API 列表](https://developer.mozilla.org/en-US/docs/Web/API#Specifications)

[我只讲几个{有趣的|我会的}]{.gray}

<slide/>

# 最简单的Fetch

- 浏览器原生的AJAX方法
- 一个函数直接解决问题
- promise-like
- `fetch(...).then(res=>res.json()).then(result=>...)`

<slide/>

# 必须提到的WebGL

- WebGL就是好在浏览器里使用OpenGL ES，3D渲染喽
- 一般找个封装过的库，裸的3D渲染API没一个好用的
- [**three.js**](https://threejs.org/)

<slide/>

# Web Components

- VUE等框架的参照物
- 原生的组件实现方式
- 在某些地方vue和web components十分相似

<slide/>

# 组成部分

::::grid
:::column
#### Custom Element
- 用户定义元素(<my-element/>)
- 行为和原生元素相似
- web components的基础
:::
:::column
#### Shadow DOM
- 元素的DOM与文档DOM分离
- 自定义组件保证一致效果的必备之选
:::
:::column
#### HTML template
- 将文档作为模板，从模板构建实例
- 可以用作web component的视觉表达方式
:::
::::

**然而，import html特性已经不在支持，因此，为了组件的分离，还是需要通过一些框架来简化工作的**

<slide/>

::::grid
:::column
**一些框架**
- [polymer 3.0](https://polymer-library.polymer-project.org/3.0/docs/about_30): Google推出的框架(不推荐)
- [Lit-Element](https://lit-element.polymer-project.org/): 同一个团队的新框架，轻量级
- [stencil](https://stenciljs.com/)
- [slim.js](https://slimjs.com/#/getting-started)
- [vaadin](https://vaadin.com/)(你甚至又可以用Java写前端了)
:::
:::column
**一些实践方案**
- [webcomponents](https://www.webcomponents.org/) 组件集合站
- [open-wc](https://open-wc.org/) 实践方案
- [vaddin](https://vaadin.com/) vaddin的整套解决方案
:::
::::

<slide/>

# PWA

::::grid
:::column
#### Service Worker
- PWA可以离线运行的根本
- 接管了访问请求
- 没啥复杂功能，Google的默认配置就很好
- [Workbox](https://developers.google.cn/web/tools/workbox/modules?hl=en)
:::
:::column
#### Web manifest
- 配置文件，为PWA提供辅助信息
:::
::::

<slide/>

# Worker

- 你可以理解是多线程，问题不大
- 专用worker: 仅当前上下文可用
- Shared Worker: 当前域可用(比如我这个ppt就用这个做演讲模式)
- 本质就是开个新的上下文，然后两个上下文之间有消息槽

<slide/>

# WebSocket

- 正如其名，就是在Web搞了一个全双工的非HTTP协议
- 做聊天室可能很好用
- 做服务端推出消息，可能也不错
- 最佳实践[socket.io](https://socket.io/)，里面做了重连和fallback

<slide/>

# WebRTC

- 浏览器上的P2P实时通讯技术
- 基于[ICE](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API/Protocols)协议实践(比较复杂了🙁)
- 视频会议的上乘之选
- 传输文件也未尝不可
- [各种各样的实践](https://www.webrtc-experiment.com/)

<slide/>

# Web Audio

- 音频**处理**用的API
- 单纯的播放音频你并不需要它，特效才要
- 其实不是modern特性，不过一直在路上
- 用的比较少
- 一个有趣的框架[Tone.js](https://tonejs.github.io/)

<slide/>

# HTML多媒体元素

> 都已经**2020**了，年底Flash就**停止浏览器支持**了，再也没有什么浏览器flash了，赶紧抛弃**上古**的Flash播放器吧

**正确的抉择**

- 使用<audio>&<video>元素
- 正如其名，一个音频，一个视频
- 简单易用，常用功能都有
- 你不开心，还有框架

<slide/>

**推荐的框架**

- [APlayer](https://aplayer.js.org/): 音频播放器，界面美观，支持歌词
- [DPlayer](http://dplayer.js.org/): 视频播放器，界面美观，支持弹幕
- [我会跟你讲上面两个作者是同一个人？]{.gray}[@DIYgod](https://github.com/DIYgod){.gray}
- [同一个组织开发的一些其他的播放器](https://github.com/MoePlayer)

<slide/>

# WebAssembly

- 二进制形式
- 你可以将将其他语言**编译**到WASM
- 与js是不同的语言、不同的VM
- 正在被广泛使用
- 理论上只要不依赖于特定环境（比如硬件/OS），都可以编译到WASM

<slide/>

**一些案例**
- [go playground](https://play.studygolang.com/)，直接在浏览器运行go
- [Ti-wasm](https://github.com/pingcap/tidb/projects/27)，在浏览器里运行TiDB数据库
- [awesome-wasm](https://github.com/mbasso/awesome-wasm)，更多的相关内容

<slide/>

# 其他的一些API

:::p
> [我不会，我不讲，我就说说😈]{.gray}
:::

- IndexedDB: 真·前端数据库
- Push API: 推送，配合PWA/service wroker使用
- Canvas API: 2D绘图
- Web Animation: 动画
- Pointer Lock: 鼠标限制，FPS极佳搭档
- Payment Request API: 支付请求
- Presentation API: 一个很复杂的演示用API，由第二屏工作组设计

<slide/>

# 更先进的一些API

[**github/browser-2020**](https://github.com/luruke/browser-2020)这里提到了一些

- 画中画
- Web Share API
- Web Locks API

这里许多的特性都是浏览器**限定**，或者系统**限定**

<slide/>

# CSS黑魔法

> 东西太多了，就不讲了
>
> 而且复杂情况下，使用其他语言或方言，要比直接CSS要舒服

**语言与方言**

- [Sass和SCSS](https://sass-lang.com/)
- [Less](http://lesscss.org/)
- [stylus](http://stylus-lang.com/)

<slide/>

## Finish!🎉
