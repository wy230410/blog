# webpack

基本作用: 将许多零碎的小文件整合到一起, 缩小文件的体积

# webpac配置

## webpack的使用方法

```javascript
// 1. 初始化包环境
npm init
// 2. 安装依赖包
// -D：表示将webpack、webpack-cli这两个包记录在开发环境
npm install webpack webpack-cli -D 
// 3. 运行webpack
webpack
// 注: index.js是webpack打包的入口文件
```

## 修改webpack默认的入口文件与出口文件位置

```javascript
const path = require('path')

module.exports = {
  entry: './src/index.js', // 入口文件位置
  output: {
    path: path.resolve(__dirname, 'dist'), // 出口文件位置
    filename: 'my-first-webpack.bundle.js', // 出口文件名称
  },
};

// 注意: webpack每次打包只会覆盖同位置同名的文件
//      所以每次打包前尽量将上一次打包的文件夹删除
```

## webpack打包流程

npm run build(自定义命令) ===> 执行webpack命令 ===> 检查是否有配置文件

有: 根据配置文件得到配置参数 ===> 

无: 执行默认配置文件 ===>

同: 找到入口 ===> 先构建依赖关系图编译各个模块文件 ===> 输出 ===> 指定文件

## webpack打包后生成html文件并自动引入打包后的js

```javascript
// 1. 下载 html-webpack-plugin插件
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
    plugins: [
    new HtmlWebpackPlugin({
      template: './public/index.html'  // 告诉webpack使用插件时, 以我们自己的html文件作为模板去生成dist/html文件
    })
  ]
}
```

## 加载器 - 处理css文件

```javascript
//  下载 style-loader css-loader 依赖包
module.exports = {
    module: { // 加载器配置
    rules: [ // 规则
      { // 一个具体的规则对象
        test: /\.css$/i, // 匹配.css结尾的文件
        use: ["style-loader", "css-loader"], // 让webpack使用这2个loader处理css文件
        // 执行顺序是从右到左的, 所以不能颠倒顺序
        // css-loader: webpack解析css文件-把css代码一起打包进js中
        // style-loader: css代码插入到DOM上 (style标签)
      },
    ],
  },
}
```

## 加载器 -  处理less文件

```javascript
// 下载 less less-loader 依赖包

module.exports = {
    module: { // 加载器配置
    rules: [ // 规则
      {
        test: /\.less/,
        use: ['style-loader', 'css-loader', 'less-loader']
      }
    ],
  },
}
```

## 加载器 - 处理图片文件

### webpack4 及之前版本

```javascript
// 下载 url-loader file-loader依赖
// url-loader文档：它把文件转换成base64 uri
// file-loader文档：将文件上的import/require()解析为一个url，并将该文件发送到输出目录。
```

### webpack4 之后版本

```javascript
// 不再需要下载依赖

module.exports = {
    module: { // 加载器配置
    rules: [ // 规则
      { // 图片文件的配置(仅适用于webpack5版本)
        test: /\.(gif|png|jpg|jpeg)/,
        type: 'asset' // 匹配上面的文件后, webpack会把他们当做静态资源处理打包
          // 如果你设置的是asset模式
          // 以8KB大小区分图片文件
          // 小于8KB的, 把图片文件转base64, 打包进js中
          // 大于8KB的, 直接把图片文件输出到dist下
      }
    ],
  },
}
```

## webpack加载图片文件的优缺点🔥

>优: 浏览器不用发请求了，可以直接读取
>
>缺: 如果图片太大，再转base64就会让图片的体积增大 30% 左右

## 加载器 - 处理字体图标

```javascript
// webpack5

module.exports = {
    module: { // 加载器配置
    rules: [ // 规则
            { // webpack5默认内部不认识这些文件, 所以当做静态资源直接输出即可
            test: /\.(eot|svg|ttf|woff|woff2)$/,
            type: 'asset/resource', // 所有的字体图标文件, 都输出到dist下
            generator: { // 生成文件名字 - 定义规则
              filename: 'fonts/[name].[hash:6][ext]' // [ext]会替换成.eot/.woff
            }
          }
        ]
    }
}
```

## 加载器 - 处理高版本js语法🔥	

>**面试问题： 你对前端浏览器兼容性上有过那些处理方案？**
>
>通过webpack的js降级设置，来让高版本的js语法降级为低版本，适用于ie低版本浏览器；ie已经停止维护了，所以慢慢的我们不会去关注ie的适配问题

```javascript
// 下载 babel-loader @babel/core @babel/preset-env 依赖

module.exports = {
    module: { // 加载器配置
    rules: [ // 规则
     {
        test: /\.m?js$/,
        exclude: /(node_modules|bower_components)/, // 不去匹配这些文件夹下的文件（防止影响其他第三方依赖包）
        use: {
          loader: 'babel-loader', // 使用这个loader处理js文件
          options: { // 加载器选项
            presets: ['@babel/preset-env'] // 预设: @babel/preset-env 降级规则-按照这里的规则降级我们															   的js语法
          }
        }
      }
    ],
  },
}
```

# webpac开发服务器(热更新)

```javascript
//	下载 webpack-dev-server 依赖

"scripts": {
    "build": "webpack --mode production", // 生产环境
    "serve": "webpack serve --mode development" // 开发环境
  },
```

## webpack-dev-server配置(自定义端口号)

```javascript
module.exports = {
    devServer: {
        port: 3000 // 端口号
    }
}
```

# VUE的特点

>vue最大的特点是渐进式框架
>
>渐进式: 逐渐使用, 集成更多的功能

# VUE安装

```javascript
// 1. 全局安装
npm i @vue/cli -g
// 2. 查看版本号
vue -V
// 3. 创建项目
vue create 项目文件夹名称 // 注:项目文件夹名称一般由 小写字母+短横线 组成
// 4. 选择模板 vue2
// 5. 运行
npm run serve
```

# Vue配置

## 端口号

```javascript
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  devServer: { // 自定义服务配置
    open: true,  // 当开启服务器时自动打开浏览器
    port: 3000 // 自定义端口号
    host: 'localhost' // 解决vue域名为0.0.0.0
  }
})
```

## 规范代码检测

>环境: 在定义了变量, 没有使用的情况下

```javascript
// 方法一 (全局屏蔽eslint检测)
给配置文件添加 lintOnSave:false 属性
// 方法二 (局部屏蔽eslint检测 - 单行注释)

// eslint-disable-next-line
let a = 'a'

or

let a = 'a'	// eslint-disable-next-line

// 方法三 (局部屏蔽eslint检测 - 多行注释)
/* eslint-disable */
let a = 123
/* eslint-enable  */
```

## style配合scoped属性, 保证样式只针对当前template内标签生效

# 插值表达式

>可以利用双大括号直接在dom标签中插入内容，不必进行webapi的一系列操作

**语法:** {{ 表达式 }}

```vue
<template>
  <div>
    <div>{{ msg }}</div>
    <div>{{ 1 == 1 }}</div>
    <div>{{ 1 + 1 }}</div>
    <div>
      我的名字是{{ obj.name }},我今年{{ obj.age }}岁,
      {{ obj.age >= 18 ? "我成年了" : "我没有成年" }}.
    </div>
  </div>
</template>

<script>
export default {
  name: "expressionDemo",
  data() {
    return {
      msg: "插值表达式",
      obj: {
        name: "法外狂徒",
        age: 18,
      },
    };
  },
};
</script>

<style>
</style>
```

# MVVM模式🔥

vue使用的mvvm设计模式。**MVVM**是`Model-View-ViewModel`缩写，也就是把`MVC`中的`Controller`演变成`ViewModel`。`Model`层代表数据模型，`View`代表UI组件，`ViewModel`是`View`和`Model`层的桥梁，数据会绑定到`viewModel`层并自动将数据渲染到页面中，视图变化的时候会通知`viewModel`层更新数据。

# v-bind 动态属性

>给标签属性设置vue变量的值

**语法:**

1.  v-bind:属性名="vue变量"
2.  :属性名="vue变量"

**案例:**

```vue
<template>
  <div>
    <a v-bind:href="url">百度</a>
    <a :href="url">百度</a>
    <!-- 
      关于动态属性的特例： 在使用动态属性渲染图片时，不可以直接将变量的值设置为路径
      原因： v-bind会把这个路径认为是字符串
      解决方案： 
        1. 通过 import 的方式引入图片并且赋值给data中的变量
        2. 通过 require 的方式引入图片并赋值给data中的变量

      以上两种解决方案，你觉得哪一个更好呢？？
      答： require 的引入方式更为友好。 当一个vue文件被使用时，那么既然是js代码，那么遵循从上往下依次执行，import中引入的图片就会在页面渲染初期被加载，而require引入的图片，只有在使用的时候才会被加载，初次开启页面会显得更加的高效
    -->
    <hr />
    <img src="../assets/123.png" alt="">
    <img :src="url2" alt="">
    <hr>
    <img :src="url3" alt="">
    <hr>
    <img :src="url4" alt="">
  </div>
</template>

<script>
import image from '../assets/123.png'
export default {
  name: "bindDemo",
  data() {
    return {
      url:'http://baidu.com',
      url2:'../assets/123.png',
      url3:image,
      url4:require('../assets/123.png')
    };
  },
};
</script>

<style>
</style>
```

# vue项目优化🔥

可以用require的方式引入图片，当使用到它的时候才会按需加载，而不会像import引入那样在页面创建时就会加载，加快页面初次加载效率

# v-on 事件绑定

>给标签绑定事件

**语法:**

1.  v-on:事件名=回调函数
2. @事件名=回调函数

**案例:**

```vue
<template>
  <div>
    <div>当前数量:{{count}}</div>
    <button v-on:click='count+=1'>点击+1</button>
    <button v-on:click='addFn'>点击+2</button>
    <button v-on:click='addCountFn(5)'>点击+5</button>
    <button @click="reduceFn()">点击-1</button>
  </div>
</template>

<script>
export default {
  name:'eventBindDemo',
  data(){
    return{
      count:0
    }
  },
  methods:{
    // 这里的this代表的是 export default 这个对象
    addFn(){
      this.count+=2
    },
    addCountFn(num){
      this.count+=num
    },
    reduceFn(){
      this.count--
    }
  }
}
</script>

<style>

</style>
```

# v-on 获取事件对象

**方法:**

1. 无传参, 通过形参直接接收
2. 传参, 通过`$event`指代事件对象传给事件处理函数

**案例:**

```vue
<template>
  <div>
    <!-- 事件对象 -->
    <!--  
    v-on获取事件对象 - event 1. 无参数获取事件对象 -
    我们的时间对象event默认在第一个形式参数上 2.
    有参数(特指带小括号的方法)，这个小括号会覆盖event默认参数，我们可以同$event的实际参数来代替这个事件对象
    注意事项：一般情况下，我们都将事件对象event 放到整个形式参数的最后一位
     -->
    <button @click="eventFn">事件一</button>
    <button @click="eventFn2(5, $event)">事件二</button>
    <a @click="go" href="http://www.baidu.com">我想去看诗和远方</a>
  </div>
</template>

<script>
export default {
  name: "eventBindDemo",
  methods: {
    eventFn(e) {
      console.log(e);
    },
    eventFn2(num, e) {
      console.log(num, e);
    },
    go(e){
      e.preventDefault()
    }
  },
};
</script>

<style>
</style>
```

# v-on修饰符

**语法:** @事件名.修饰符=回调函数

- .stop 阻止事件冒泡
- .prevent 阻止默认行为
- .once 程序运行期间, 只触发一次事件处理函数

**案例:**

```vue
<template>
  <div @click='fatherFn'>
    <p @click.stop='stopFn'>点击后希望阻止冒泡</p>
    <a href="http://www.baidu.com" @click.prevent.stop>跳转百度</a>
    <p @click.once="clickCount">观察这个点击事件到底执行几次</p>
  </div>
</template>

<script>
export default {
  name:'modifierDemo',
  data(){
    return{

    }
  },
  methods:{
    // 父标签
    fatherFn(){
      console.log('冒泡事件触发了')
    },
    // 阻止冒泡
    stopFn(){
      console.log('子元素事件触发')
    },
    // 执行次数
    clickCount(){
      console.log('once触发')
    }
  }
}
</script>

<style>

</style>
```

