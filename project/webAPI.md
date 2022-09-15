# 选择器🔥

**document.querySelector(css选择器)与document.quserySelectorAll(css选择器)**

>同: 选取dom元素
>
>异: document.querySelector(css选择器) 是选择满足条件的第一个dom元素,并返回该dom元素
>
>​	 document.quserySelectorAll(css选择器) 是选择所有满足条件的dom元素, 并返回所有元素组成的数组

## code-document.querySelector

```
<body>
  <div>hello world</div>
  <script>
    const div = document.querySelector('div')
  </script>
</body>
```

## code-document.quserySelectorAll

```
<body>
  <ul>
    <li>这是第1个li</li>
    <li>这是第2个li</li>
    <li>这是第3个li</li>
    <li>这是第4个li</li>
    <li>这是第5个li</li>
    <li>这是第6个li</li>
  </ul>
  <script>
    const list = document.querySelectorAll('ul li')
    console.log(list)
  </script>
</body>

```

# 修改内容🔥

**innerHTML与innerText**

>同:都可以用于设置与获取dom元素的内容
>
>异: innerHTML可以设置与获取dom元素里的标签与内容 
>
>​	  innerText只能设置与获取dom元素里的文本内容

**语法:**

1. dom元素.innerHTML
2. dom元素.innerText

## code-innerHTML

```
<body>
  <div>hello<span>world</span></div>
  <script>
    // 获取div标签
    const div = document.querySelector('div')
    // 定义一个html变量接收div里原本的内容
    let html = div.innerHTML
    console.log(html)
    // 重新设置div标签里的内容
    div.innerHTML = 'hello'
    // 重新获取div标签里的内容
    html = div.innerHTML
    console.log(html)
  </script>
</body>
```

## code-innerText

```
<body>
  <div>hello<span>world</span></div>
  <script>
    // 获取div标签
    const div = document.querySelector('div')
    // 定义一个html变量接收div里原本的内容
    let html = div.innerText
    console.log(html)
    // 重新设置div标签里的内容
    div.innerHTML = 'hello'
    // 重新获取div标签里的内容
    html = div.innerText
    console.log(html)
  </script>
</body>
```

# 类名操作🔥

**className与classList**

>同: 都可以添加类名
>
>异: className会覆盖原有的类名, 想要同时让多个类名生效, 必须同时写上
>
>​	  classList不会覆盖原有的类名

**语法:**

1.  dom元素.className = '类名' (可以写多个类名)

2.  dom元素.classList.add('类名') (添加类名)

     dom元素.classList.remove('类名') (删除类名)

     dom元素.classList.toggle('类名') (切换类名, 有就删除, 没有就添加)

## code-className

```
<body>
  <div>hello</div>
  <script>
    const div = document.querySelector('div')
    div.className = 'pink'
  </script>
</body>
```

## code-classList

```
<body>
  <div>hello</div>
  <script>
    const div = document.querySelector('div')
    // 添加
    div.classList.add('pink')
    // 删除
    div.classList.remove('pink')
    // 切换
    div.classList.toggle('pink')
  </script>
</body>
```

# 自定义属性🔥

**什么是标签属性?**

标签属性就是html标签自带的属性,如:title src type等

**什么是自定义属性?**

自定义属性就是我们自己给html标签定义的属性

一般以 data-属性名 开头 如data-name data-id data-index

**如果用js获取自定义属性值?**

在dom对象上一般以dataset的方式获取

**语法:**

1.  dom元素.dataset.自定义名

## code

```javascript
<body>
  <div data-id='111'>我是一段文字</div>
  <script>
    const div = document.querySelector('div')
    console.log(div.dataset.id)
  </script>
</body>
```

# 定时器-间歇函数🔥

> 每隔一段时间自动执行的一段代码

**setInterval与setTimeout**

> 同:都是隔一段时间就执行,都可以在执行之前清除掉,让它们不执行
>
> ​	返回值都是一个number类型的数字
>
> 异:setInterval是无限执行,除非我们手动清除
>
> ​	  setTimeout只执行一次,

**语法:**

1. 开启定时器:

​	    setInterval(回调函数,间歇时间)

​	    setTimeout(回调函数,间歇时间)

2. 关闭定时器:

    clearsetInterval(定时器ID)

    clearTimeout(定时器ID)

## code-setInterval

```
<body>
  <script>
    let timerId
    let i = 0
    timerId = setInterval(function () {
      console.log('hello')
      i++
      if (i > 5) clearInterval(timerId)
    }, 1000)
  </script>
</body>
```

## code-setTimeout

```
<body>
  <script>
    let timerId
    timerId = setTimeout(function () {
      console.log('hello')
    }, 1000)
    clearTimeout(timerId)
  </script>
</body>
```

# 事件监听

- 什么是事件?

    事件是在编程时系统内发生的动作或者发生的事情

    如: 用户在网页单击某个按钮

- 什么是事件监听?

    就是让程序检测是否有事件产生,一旦有事件触发,就立即调用一个函数做出响应

    如: 鼠标经过显示下拉菜单

    **on与addEventListener**

>同: 都用于监听事件
>
>异: on监听的事件,在给同一个元素绑定一个同名事件的时候,会将上一次的事件覆盖,相当于替换
>
>​	  on监听的事件没有事件捕获
>
>​	  addEventListener监听的事件,在给同一个元素绑定一个同名事件的时候,不会覆盖上一次的事件,相当于追加
>
>​	  addEventListener的回调函数的第一个参数默认为事件对象(e event),保存了当前触发事件的信息
>
>​     addEventListener不可以解绑匿名事件
>
>​	 addEventListener的第三个参数为true的时候为事件捕获,默认值或者不写为false,代表事件冒泡

**语法:**

1.  dom元素.on事件类型 = 回调函数
2.  dom元素.addEventListener(事件类型,回调函数[,true or false])

## code-on(添加事件与解绑事件)

```
<body>
  <button>按钮</button>
  <script>
    const btn = document.querySelector('button')
    // 添加点击事件
    btn.onclick = function () {
      console.log('你好呀!')
    }
    // 5秒后解绑事件
    setTimeout(() => btn.onclick = null, 5000)
  </script>
</body>
```

## code-addEventListener(添加事件与解绑事件)

```
<body>
  <button>按钮</button>
  <script>
    const btn = document.querySelector('button')
    function clickBtn(){
      console.log('你好呀!!!')
    }
    // 添加点击事件
    btn.addEventListener('click',clickBtn)
    // 5秒后解绑事件
    setTimeout(() => btn.removeEventListener('click',clickBtn), 5000)
  </script>
</body>
```

# 事件类型

- ### 鼠标事件

    1. click 鼠标单击
    2. mouseenter 鼠标经过
    3. mouseleave 鼠标离开
    4. mouseover 鼠标经过
    5. mouseout 鼠标离开

>mouseenter mouseleave 与 mouseover mouseout的区别?
>
>同: 都是鼠标的经过与离开事件
>
>异: mouseenter和mouseleve无事件冒泡
>
>​	  mouseover和mouseout有事件冒泡

- ## 焦点事件

    1. focus 获得焦点
    2. blur 失去焦点

- ## 键盘事件

    1. kewdown 键盘按下触发
    2. keyup 键盘弹起触发

>可以通过事件对象e获得我们按下或弹起的按键
>
>例: e.key ==='enter'

- ## 文本事件

    1. input 用户输入事件
    2. change 当文本框内容发生改变, 并且输入框失去焦点触发

# this指向🔥

每个函数里都有this, 代表了当前函数运行时所处的环

>1. 在普通函数中,谁调用该函数,this就指向谁
>2. 定时器的this都指向window
>3. 箭头函数没有它自己的this,但是它会沿用它上一级的this,并且无法被call apply bind所修改
>4. 构造函数与他的原型的this都指向它的实例对象
>5. 事件处理函数中的this,指向了绑定事件的dom元素

# 回调函数

什么是回调函数?

简单理解就是回头再调用的函数，将A函数作为B函数的参数的时候,我们就称A函数为B函数的回调函数

## code

```
<body>
  <button>按钮</button>
  <script>
    const btn = document.querySelector('button')
    function say(){
      console.log('hello')
    }
    // 函数say为setTimeout函数的回调函数
    setTimeout(say,1000)
  </script>
</body>
```

# 事件流🔥

**什么是事件流?**

事件流描述的是元素在页面中接收事件的顺序

**事件流分为哪三个阶段?**

事件捕获 ===> 到达目标 ===> 事件冒泡

**注: **on没有事件捕获,只有事件冒泡

**事件捕获与事件冒泡的过程?**

事件捕获: 从DOM根元素开始去执行对应的事件(从外到里)

**注:** addEventListener的第三个参数为true的时候,代表事件捕获阶段触发, 默认值或不写都为false

事件冒泡: 从事件触发的位置向根元素执行对应的事件(从里到外)

**注:** 当事件触发的时候,会从内到外依次调用**同名**事件

# 阻止默认行为与事件冒泡🔥

**语法:**

1. 事件对象.stopPropagation() 阻止事件冒泡
2. 事件对象.preventDefault() 阻止默认行为

# 事件委托🔥

**什么是事件委托?**

利用事件冒泡，将事件监听绑定在父节点上，通过父节点来监听子节点的事件

**有什么优点?**

减少了事件的绑定次数,从而提高性能,常用于动态添加或删除子元素的监听

**注释:**

-  e.target 当前触发事件的元素
- e.target.tagName 当前触发事件元素的标签名

## code

```
<body>
  <ul>
    <li><button>按钮</button></li>
    <li><button>按钮</button></li>
    <li><button>按钮</button></li>
  </ul>
  <script>
    const ul = document.querySelector('ul')
    ul.addEventListener('click', e => {
      const {tagName} = e.target
      if (tagName!=='BUTTON') return
      console.log('hello')
    })
  </script>
</body>
```

# 页面加载事件

老代码喜欢把script写在head中,这时候直接找dom元素找不到

**语法:**

- window.addEventListener('load',function(){})
-  window.addEventListener('DOMContentLoaded',function(){})

**load与DOMContentLoaded**

>同: 等待页面资源加载后执行的函数
>
>异: load是在页面所有资源加载完成之后执行,包括图片等
>
>​	  DOMContentLoaded是在DOM元素加载完成之后执行

## code-load

```
<body>
  <script>
    window.addEventListener('load',function(){})
  </script>
</body>
```

## code-DOMContentLoaded

```
<body>
  <script>
    window.addEventListener('DOMContentLoaded',function(){})
  </script>
</body>
```

# 页面事件

## 页面滚动事件

定义: 页面在滚动的时候触发的事件

**语法:** window.addEventListener('scroll',function(){})

**属性:**

- scrollTop 获取页面被卷曲的头部距离,返回值不带单位 **(可读写)**
- scrollLeft 获取页面被卷曲的左侧距离,返回值不带单位 **(可读写)**
- scrollWidth 返回自身实际的宽度,不包含边框,返回值不带单位
- scrollHeight 返回自身实际高度,不含边框,返回值不带单位

## 页面尺寸变化事件

在窗口尺寸变化时触发

**语法:** window.addEventListener('resize',function(){})

## 元素尺寸与位置

**语法:** dom元素.属性↓

**属性:**

- clientWidth 获取元素的宽度,不包含边框与滚动条,返回值不带单位
- clientHeight 获取元素的高度,不包含边框与滚动条,返回值不带单位
- offsetWidth 获取元素的宽度,包含边框,返回值不带单位
- offsetHeight 获取元素的高度,包含边框,返回值不带单位

>获取宽高的三者的区别?
>
>scroll系列获取的是实际内容的宽高
>
>clinent系列获取的是显示内容的宽高并且不包含滚动条
>
>offset系列获取的是元素的宽高(边框+paddign+内容)	

# 立即执行函数🔥

**什么是立即执行函数?**

不需要调用,立马执行的函数

**为什么需要立即执行函数?**

创建独立的作用域, 防止命名冲突

**注意:** 多个立即执行函数之间用分号分隔,立即函数也可以传参

**语法:**

- ;(function(){console.log(123)}())
- ;(function(){console.log(123)})()

# 什么情况下需要加分号

在以括号开头的情况下,例:	;[]	;{}	;()

# 日期对象

是一个js的内置对象,用来获取时间

**语法:**

-  new Date()
- new Date('2022-08-22 12:00:00')

**时间对象方法:**

- getFullYear() **获得年份** 返回四位年份 例:2022
- getMonth() **获得月份** 取值为0~11 0代表1月
- getDate() **获取月份中的某一天** 不同月份取值不同
- getDay() **获取星期** 取值为0~6 0代表星期日 1代表星期一
- getHours() **获取小时** 取值0~23 
- getMinutes() **获取分钟** 取值0~59
- getSeconds() **获取秒** 取值0~59

## code

```javascript
<body>
  <div></div>
  <script>
    // 1. 先得到一个日期对象
    const date = new Date()
    
    // 2. 使用里面的方法 获取年月日，时分秒
    console.log(date.getFullYear())  // 年
    console.log(date.getMonth() + 1) // 月 ；注意， 月份从0开始 0-11；
    console.log(date.getDate()) // 日
    console.log(date.getDay())  // 星期几   0 - 6  0表示星期天， 1 表示星期一；
    

    // 3. 时分秒
    const h = date.getHours()  // 时
    const m = date.getMinutes() // 分
    const s = date.getSeconds() // 秒
    console.log(h, m, s)
  </script>
</body>
```

## 时间戳的获取方式

时间戳是指1970年01月01日00时00分00秒起至现在的**毫秒数**

```javascript
<body>
  <script>
    // 可以返回指定时间戳
    const date = new Date()
    console.log(date.getTime())
    console.log(+new Date())
    // 只能得到当前时间戳
    console.log(Date.now())
  </script>
</body>
```

# 节点操作🔥

**DOM节点的分类?**

- 元素节点 如: div标签
- 属性节点 如: class属性
- 文本节点 如: 标签中的文字

**父节点:**

子元素.parentNode

**注:** 不是函数

返回最近一级的父节点 找不到返回null

## code-parentNode (父节点)

```
<body>
  <div class="wrip">
    <div class="son"></div>
  </div>
  <script>
   const son = document.querySelector('.son')
   console.log(son)
   console.log(son.parentNode)
   console.log(son.parentNode.parentNode)
  </script>
</body>
```

**子节点:**

**父元素.childNodes**

获得所有子节点 包括文本节点(空格 换行) 注释节点等

**父元素.children**

仅获得所有元素节点 返回的是一个数组

## **code-children** (子节点)

```javascript
<body>
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
  </ul>
  <script>
    const ul = document.querySelector('ul')
    console.log(ul)
	// ul的所有子节点
    console.log(ul.children)
  </script>
</body>
```

**兄弟节点:**

- **下一个兄弟节点:** nextElementSibling 属性
- **上一个兄弟节点:** previousElementSibling 属性

### **code-ElementSibling (兄弟节点)**

```javascript
<body>
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
  </ul>
  <script>
    const li2 = document.querySelector('ul li:nth-child(2)')
    console.log(li2)
	// 下一个兄弟节点
    console.log(li2.nextElementSibling)
	// 上一个兄弟节点
    console.log(li2.previousElementSibling)
  </script>
</body>
```

**创建节点:**

document.createElment('标签名')

### code-createElement (创建节点)

```javascript
<body>
  <script>
    const li = document.createElement('li')
    console.log(li)
  </script>
</body>
```

**追加节点:**

- **在父元素最后追加:** 父元素.appendChild(要追加的元素)
- **在某个子元素之前追加:** 父元素.insertBefore(要插入的元素, 在哪个元素前面)

**注意:** 把一个节点元素同时进行两次追加, 会把第一次追加到dom树的该节点移动到新的位置,不会重新创建

### code-appendChildren (追加节点)

```javascript
<body>
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
  </ul>
  <script>
    const ul = document.querySelector('ul')
    const li2 = document.querySelector('ul li:nth-child(2)')
    // 创建li节点
    const li4 = document.createElement('li')
    li4.innerHTML = '4'
    // 在ul末尾追加li
    ul.appendChild(li4)

    const li0 = document.createElement('li')
    li0.innerHTML = '0'
    // 在首位追加li
    ul.insertBefore(li0, ul.children[0])
  </script>
</body>
```

**克隆节点:**

元素.cloneNode(布尔值)   默认为false,若为true,则代表克隆时会包含后代节点一起克隆

### code-cloneNode (克隆节点)

```javascript
<body>
  <div>hello</div>
  <script>
    const div = document.querySelector('div')
    // 克隆一个div节点
    const cloneDiv = div.cloneNode(true)
    console.log(cloneDiv)
  </script>
</body>
```

**删除节点:**

- **删除子节点:** 父元素.removeChild(要删除的元素)
- **删除自身及自节点:** 元素.remove() 删除自身所在的节点 包括子节点

### code-removechild (删除节点)

```javascript
<body>
  <ul>
    <li>1</li>
  </ul>
  <script>
    const ul = document.querySelector('ul')
    // 删除li节点
    ul.removeChild(ul.children[0])
	// 删除ul以及它的子节点
	ul.remove()
  </script>
</body>
```

# js执行机制🔥and 事件循环(缺事件循环笔记) 

javaScript是一门单线程的语言,也就是同一时间只能做一件事情,所有的工作都是顺序执行

>首先判断JS任务是同步任务还是异步任务,如果是同步任务,就放入主执行栈依次执行
>
>如果是异步任务就放入相应的异步进程里处理,当满足执行条件,异步进程就会将异步任务放入任务队列中
>
>当主线程执行完所有的同步任务后,会去任务队列里查看是否有可以执行的异步任务,
>
>如果有,就拿到主线程中执行,执行完之后再去任务队列中查找,依次循环

**以下理解  ↓**

**为什么JS是一门单线程的呢?**

JS设计之初,是为了做网页交互,那么就会操作DOM元素,那么就会有添加或者删除DOM节点的一些操作,这些操作不能同时执行

**线程 和 进程**

进程是资源分配的最小单位, 线程是CPU调度的最小单位

进程(process) : 一个进程就是一个正在运行的程序 (任务管理器)

线程(thread) :  一个进程内执行着的每个任务即为一个线程. 线程是允许应用程序并发执行多个任务的一种机制.

**注:** 一个进程里面可以包含多个线程,	进程必须依附于线程存在

**同步 和 异步**

**同步:** 同一时间只能执行一个任务,上一个任务执行完成,才可以执行下一个任务

**异步:** 可以同时执行多个任务,提高了程序执行效率

**同步任务**:

所有的同步任务都在主线程上执行,形成了一个执行栈

**异步任务:**

JS的异步任务是通过回调函数来实现的,在做某个任务的同时可以处理其他任务

常见的三种类型

事件: click , resize 监听

资源加载: load , DOMContentLoaded

定时器: setTieout , setInterval

# location对象

location对象保存了浏览器URL地址的各个组成部分

- location.href 得到当前文件**URL地址** 也可以用于跳转到某个地址
- location.search 获取URL地址中携带的参数 **符号?以及它后面的部分**
- location.hash 获取URL地址中的哈希值 **符号#以及它后面的部分**
- location.reload(Boolean值)  用来刷新当前页面, 传入参数为true时表示强制刷新 

# navigator对象

navigator的数据类型是对象，该对象下记录了浏览器自身的相关信息

**userAgent**

## code-navigator

```javascript
// 检测 userAgent（浏览器信息）
;(function () {
      const userAgent = navigator.userAgent
      // 验证是否为Android或iPhone
      const android = userAgent.match(/(Android);?[\s\/]+([\d.]+)?/)
      const iphone = userAgent.match(/(iPhone\sOS)\s([\d_]+)/)
      // 如果是Android或iPhone，则跳转至移动站点
      if (android || iphone) {
        location.href = '地址'
      }
})()
```

# history对象

history 对象表示当前窗口首次使用以来用户的导航历史记录。因为 history 是 window 的属性，所以每个 window 都有自己的 history 对象。出于安全考虑，这个对象不会暴露用户访问过的 URL，但可以通过它在不知道实际 URL 的情况下前进和后退。

- back() **后退功能**
- forward() **前进功能**
- go(参数) **前进后退功能** 参数如果是1 前进一个页面 如果是-1 后退一个页面

## code-history

```javascript
const back = document.querySelector('button:first-child')
const forward = back.nextElementSibling
back.addEventListener('click', function () {
  // 后退一步
  // history.back()
  history.go(-1)
})
forward.addEventListener('click', function () {
  // 前进一步
  // history.forward()
  history.go(1)
})
```

# 本地存储🔥

localstorage 与 sessionStorage

>同: 都用于存储浏览器的数据 **容量为5MB**左右 以键值对的方式存储 只能存储**string类型**的数据
>
>异: 
>
>localstorage存储的数据永久生效,除非手动删除,否则页面关闭也存在
>
>可以多个窗口共享
>
>sessionStorage在关闭当前页面的时候清除,只能在同一页面共享数据

**方法:**  (sessionStorage与之相同)

- **存储数据:** localStorage.setItem(key, value)
- **获取数据:** localStorage.getItem(key)
- **删除数据:** localStorage.removeItem(key)
- **清空数据:** localStorage.clear()

##  code

```javascript
// 1. 要存储一个名字  'uname'， 'pink老师'
// localStorage.setItem('键'，'值')
localStorage.setItem('uname', 'pink老师')
// 2. 获取方式  都加引号
console.log(localStorage.getItem('uname'))
// 3. 删除本地存储  只删除名字
// localStorage.removeItem('uname')
// 4. 改  如果原来有这个键，则是改，如果没有这个键是增
localStorage.setItem('uname', 'red老师')

// 我要存一个年龄
// 2. 本地存储只能存储字符串数据类型
localStorage.setItem('age', 18)
console.log(localStorage.getItem('age'))
```

# 数组方法🔥

- arr.unshift(元素) 在数组首位添加元素,返回数组长度
- arr.shift() 删除数组首位元素,返回数组长度
- arr.push(元素) 在数组末尾添加元素,返回数组长度
- arr.pop() 删除数组末尾元素,返回数组长度
- arr.splice(数组索引,删除几个元素[,添加的元素]) 当传两个参数代表删除元素,三个参数代表删除后添加元素,返回值为被删除的元素
- arr.slice([start,[end]]) 不传递参数为**浅拷贝**数组,
    - 传一个参数表示截取star到数组结尾并返回新数组,
    - 传两个参数表示截取[start,end)
- arr.forEach 遍历数组,不改变原数组
- arr.filter 过滤数组,给每一个元素执行一次回调函数,回调函数的返回一个布尔值,不改变原数组
- arr.map 给每一个数组执行一次回调函数,返回执行完回调函数后的值,改变原数组
- arr.indexof 从前向后查找是否有满足条件的元素,如果有就返回该元素的索引,没有就返回-1
- arr.lastIndexof 从后向前查找是否有满足条件的元素,如果有就返回该元素的索引,没有就返回-1
- arr.concat(arr1,arr2...) 合并数组 **浅拷贝**
- arr.find(回调函数)  返回数组中满足回调函数条件的数组元素的**值**
- arr.findIndex(回调函数) 返回数组中满足回调函数条件的数组元素的**索引**
- Array.from(arr) 将伪数组转换为真数组
- arr.includes(元素) 查找数组中是否有该元素,有就返回true,没有就返回false
- Array.isArray(arr)  判断传递的值是否为一个数组
- arr.join('') 将数组以参数里的符号分隔为一个字符串
- arr.reduce(回调函数,默认值) 累加器
- arr.reverse() 反转数组
- arr.sort 数组排序
- arr.toStirng() 数组转字符串
- arr.every(fun) 一个数组内的所有元素是否都通过fun函数测试
- arr.some(fun) 一个数组内至少有一个元素通过fun函数测试

# 正则

https://juejin.cn/post/7016871226899431431

 
