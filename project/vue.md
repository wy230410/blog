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

**注意:** 当动态属性是图片或其他文件路径时, 那么不可以直接将路径的字符串作为动态属性变量的值, 需要使用import或者是require来进行引入

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

**v-on按键修饰符**

**语法:** @事件类型.按键=回调函数       // **按键必须为全小写**

**案例:**

```vue
<template>
  <input type="text" name="" id="" @keyup.enter="keyupFn">
</template>

<script>
export default {
  name:'keyDecoration',
  methods:{
    keyupFn(e){
      console.log('事件对象',e)
    }
  }
}
</script>

<style>

</style>
```

# v-model双向绑定

>把value属性和vue数据变量, 双向绑定到一起, 当其中一个值发生改变, 另一个的值也跟着改变.

**语法:** v-mode='vue数据变量'

**案例:**

```vue
<template>
  <div>
    <!-- 文本框     -->
    <div>
      <label for="uname">用户名</label>
      <input type="text" name="uname" v-model="uname" />
    </div>
    <!-- 下拉列表框 -->
    <div>
      <label for="">地区:</label>
      <select v-model="area" id="">
        <option value="" selected>--请选择地区--</option>
        <option value="成都">成都</option>
        <option value="成都">北京</option>
        <option value="成都">上海</option>
      </select>
    </div>
    <!-- 单选框 -->
    <div>
      <label for="">性别:</label>
      <input type="radio" value="男" v-model="sex">男
      <input type="radio" value="女" v-model="sex">女
    </div>
    <!-- 复选框 -->
    <div>
      <label for="">爱好</label>
      <input type="checkbox" value="吃饭" v-model="hobby">吃饭
      <input type="checkbox" value="睡觉" v-model="hobby">睡觉
      <input type="checkbox" value="打豆豆" v-model="hobby">打豆豆
    </div>
    <!-- 文本域 -->
    <div>
      <textarea v-model="text" id="" cols="30" rows="10"></textarea>
    </div>
  </div>
</template>

<script>
export default {
  name: "dataBind",
  data() {
    return {
      uname: "",
      area: "",
      sex:'',
      hobby:[],
      text:""
    };
  },
};
</script>

<style>
</style>
```

# vue双向绑定的原理🔥

>当一个Vue实例创建时，Vue会遍历data选项的属性，用`Object.defineProperty`将它们转化为`getter/setter`并且在内部追踪相关依赖，在属性被访问拒绝和修改时通知变化。每个组件实例都有相应的watcher程序实例，它会在组件渲染的过程中把属性记录为依赖，之后当依赖项的setter被调用时，会通知watcher重新计算，从而致使它关联的组件得以更新。

# v-model修饰符

**语法:** v-model.修饰符="vue数据变量"

-  number  以parseFloat转成数字类型（会自动清除非数字及以后部分)  // 注: 如果以非数字开头则不生效
- trim 去除首尾空白字符
- lazy 在change时触发而非input时（一切可以触发change事件的情况，如失焦）

**案例:**

```vue
<template>
  <div>
    <label for="">手机号:</label>
    <input type="text" v-model.trim.number.lazy="phone">
  </div>
</template>

<script>
export default {
  name:'bindDecription',
  data(){
    return{
      phone:''
    }
  }
}
</script>

<style>

</style>
```

# v-text与v-html

>更新DOM对象的innerText/innerHTML

**语法:** 

- v-text="vue数据变量"
- v-html="vue数据变量"

**案例:**

```vue
<template>
  <div>
    <div v-text="str"></div>
    <div v-html="str"></div>
  </div>
</template>

<script>
export default {
  name:'insertDemo',
  data(){
    return{
      str:'<span style="color:skyblue">我是一段文本</span>'
    }
  }
}
</script>

<style>

</style>
```

# v-if与v-show

>同: 标签的显示与隐藏
>
>异: v-show是给dom元素添加一个display:none属性,该元素还在dom树上,一般应用于切换比较频繁的地方
>
>​	  v-if是将该dom元素从dom树上彻底删除,                                                      

**语法:**

- v-show="vue变量"
- v-if="vue变量"

**案例:**

```vue
<template>
  <div>
    <p v-show="false">我是v-show</p>
    <p v-if="false">我是v-if</p>
    <div>
      您是:?
      <p v-if="age < 18">未成年</p>
      <p v-else-if="age >= 18 && age < 22">大学生</p>
      <p v-else>社畜</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "isShow",
  data() {
    return {
      age: 17,
    };
  },
};
</script>

<style>
</style> 
```

# v-for

>渲染数据列表

**语法:**

- v-for="值 in 目标结构"
- v-for="(值,索引) in 目标结构"

**目标结构:**

- 数组
- 对象
- 数字
- 字符串

**案例:**

```vue
<template>
  <div>
    <!-- 遍历数组 -->
    <ul>
      <li v-for="(value, index) in arr" :key="index">{{ value }}</li>
    </ul>
    <!-- 遍历对象 -->
    <ul>
      <li v-for="(value, key) in obj" :key="key">{{ value }}---{{ key }}</li>
    </ul>
    <!-- 遍历数组对象 -->
    <ul>
      <li v-for="value in arr2" :key="value.id">
        {{ value.id }}---{{ value.name }}
      </li>
    </ul>
    <!-- 遍历数字 -->
    <ul>
      <li v-for="(value, index) in num" :key="index">
        {{ value }}---{{ index }}
      </li>
    </ul>
    <!-- 遍历字符串 -->
    <ul>
      <li v-for="(value, index) in str" :Key="index">
        {{ value }}---{{ index }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "v-for",
  data() {
    return {
      arr: [1, 2, 3],
      arr2: [
        { id: 1, name: "一号" },
        {
          id: 2,
          name: "二号",
        },
        {
          id: 3,
          name: "三号",
        },
      ],
      obj: {
        name: "张三",
        age: 18,
        work: "法外狂徒",
      },
      num: 12,
      str: "hello world",
    };
  },
};
</script>

<style>
</style>
```

# v-for与v-if组合使用

当 v-if 与 v-for 一起使用时，v-for 具有比 v-if 更高的优先级。这意味着 v-if 将分别重复运行于 每个 v-for 循环中，即先运行 v-for 的循环，然后在每一个 v-for 的循环中，再进行 v-if 的条件对比，会造成性能问题，影响速度

**解决方法：在v-for前对数组进行筛选**

**案例:**

```vue
<template>
  <div>
    <ul>
      <li v-for="(value,index) in arr.filter(item=>item.name!=='二号')" :key="index">{{value}}</li>
    </ul>
  </div>
</template>

<script>
export default {
  name:'togetherDemo',
  data(){
    return{
      arr: [
        { id: 1, name: "一号" },
        {
          id: 2,
          name: "二号",
        },
        {
          id: 3,
          name: "三号",
        },
      ],
    }
  }
}
</script>

<style>

</style>
```

# v-for更新监测

>原因: 当v-for遍历的目标结构改变, `Vue`触发v-for的更新

**案例:**

```vue
<template>
  <div>
    <ul>
      <li v-for="(item,index) in arr" :key="index">{{item}}</li>
    </ul>
    <button @click="reverseArr">颠倒数组</button>
    <button @click="sliceArr">数组截取</button>
    <button @click="updateArr">更新数组第一个元素</button>
  </div>
</template>

<script>
export default {
  name: 'updateDemo',
  data() {
    return {
      arr: [1, 2, 3, 4, 5, 6, 7]
    }
  },
  methods: {
    reverseArr() {
      this.arr.reverse()
    },
    sliceArr() {
      this.arr.splice(3)
    },
    updateArr() {
      // this.arr[0] = 999 // 为什么显示值修改了 页面没更新???
      // 应急方案: $.set()强制更新
      /* 
        this.$set(参数1， 参数2， 参数3)
        参数1： 更新的目标结构 - v-for所遍历的数据
        参数2： 更新数据的位置 - 常常是索引位置
        参数3： 需要更新的值
      */
      // 方案二
      this.arr.splice(0, 1, 999)
    }
  }
}
</script>

<style>

</style>
```

# v-for重要规则🔥

数组对象中，有id就用id，没有id则用索引。如果用id页面报错，则换成索引。

# js数组方法复习

## 不会改变原数组

- map 给数组内的所有元素都执行一次回调函数
- filter 数组过滤
- forEach 数组遍历
- render 累加
- som 数组内只要有一个元素通过回调函数的测试, 就返回true
- every 数组内的所有元素都通过回调函数的测试, 就返回true
- slice 数组截取
- concat 拼接数组
- find 返回满足条件的第一个数组元素
- findIndex 返回满足条件的第一个数组元素的索引

## 会改变原数组

- push 在数组末尾追加一个元素
- pop 删除数组末尾一个元素
- unshift 在数组首位追加一个元素
- shift 删除数组首位元素
- splice 根据参数的不同,可以实现增 删 改功能 (参数一:开始索引,参数二:几个元素,参数三:替换的元素)
- sort 数组排序
- reverse 翻转数组
- flat 扁平化数组 转一维(flat(Infinity))

#  虚拟dom🔥

>  概念：.vue文件中的template里写的标签, 都是模板(并不是浏览器中看到的真实的dom元素), 都要被vue处理成虚拟DOM**对象**, 才会渲染显示到真实DOM页面上

1. 内存中生成一样的虚拟DOM结构 (==本质是个JS对象==)

    因为真实的DOM属性过多, 没办法快速的知道哪个属性改变了

    比如template里标签结构

    ```vue
    <template>
        <div id="box">
            <p class="my_p">1234</p>
        </div>
    </template>
    ```

    对应的虚拟DOM结构

    ```js
    const dom = {
        type: 'div',
        attributes: [{id: 'box'}],
        children: {
            type: 'p',
            attributes: [{class: 'my_p'}],
            text: '1234'
        }
    }
    ```

2. vue数据更新

    * 生成新的虚拟DOM结构
    * 和旧的虚拟DOM结构对比
    * 利用diff算法, 找不不同, 只更新变化的部分(重绘/回流)到页面

**虚拟DOM优势：**虚拟DOM保存在内存中, 只记录dom关键信息, 配合different算法提高DOM更新的性能

# **你对虚拟DOM的理解？**🔥

**答：**`虚拟DOM`本质上是`JavaScript`对象，是对`真实DOM`的抽象表现。状态变更时，记录新树和旧树的差异，最后把差异更新到真正的`dom`中**render函数**

1. 根据`tagName`生成父标签，读取props，设置属性， `如果有content内容`，设置`innerHtml或innerText`；
2. 如果存在子元素，遍历子元素递归调用render方法，将生成的子元素依次添加到父元素中，并返回根目录；

你对虚拟DOM的理解？**

**答：**`虚拟DOM`本质上是`JavaScript`对象，是对`真实DOM`的抽象表现。状态变更时，记录新树和旧树的差异，最后把差异更新到真正的`dom`中**render函数**

1. 根据`tagName`生成父标签，读取props，设置属性， `如果有content内容`，设置`innerHtml或innerText`；
2. 如果存在子元素，遍历子元素递归调用render方法，将生成的子元素依次添加到父元素中，并返回根目录；



# diff算法 - 同级比较🔥

找不同

> vue用diff算法, 新虚拟dom, 和旧的虚拟dom比较

## 场景1: 根元素变了 =》 删除重建 

旧虚拟DOM

```vue
<div id="box">
    <p class="my_p">123</p>
</div>
```

新虚拟DOM

```vue
<ul id="box">
    <li class="my_p">123</li>
</ul>
```

![image-20220602132641608](images/image-20220602132641608.png)

## 场景2: 根元素没变, 属性改变 =》 元素复用, 只更新属性

旧虚拟DOM

```vue
<div id="box">
    <p class="my_p">123</p>
</div>
```

新虚拟DOM

```vue
<div id="myBox" title="标题">
    <p class="my_p">123</p>
</div>
```

![image-20220602132718139](images/image-20220602132718139.png)

**问题：**

1. diff算法如何比较新旧虚拟DOM?
2. 根元素变化?
3. 根元素未变, 属性改变?

> 思考： 如果标签内子标签/内容改变，diff的算法是如何对应改变的？

# diff算法 - 属性key🔥

**场景3：根元素没变, 子元素没变, 元素内容改变**

## 没有key =》 就地更新

​		v-for不会移动DOM, 而是尝试复用, 就地更新，如果需要v-for移动DOM, 你需要用特殊 attribute `key` 来提供一个排序提示

```vue
<template>
  <div>
    <ul>
      <li v-for="obj in arr">
        {{ obj.name }}
        <input type="text">
      </li>
    </ul>
    <button @click="btn">下标1位置插入新来的</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      arr: [
        {name: '老大',id: 50},
        {name: '老二',id: 31},
        {name: '老三',id: 10}
      ],
    };
  },
  methods: {
    btn(){
      this.arr.splice(1, 0, {
        id: 19, 
        name: '新来的'
      })
    }
  }
};
</script>
```

![新_vfor更细_无key_就地更新](images/%E6%96%B0_vfor%E6%9B%B4%E7%BB%86_%E6%97%A0key_%E5%B0%B1%E5%9C%B0%E6%9B%B4%E6%96%B0.gif)

旧 - 虚拟DOM结构  和  新 - 虚拟DOM结构 对比过程

![image-20220602133357264](images/image-20220602133357264.png)

## 有key - 值为索引 => 就地更新

​		因为新旧虚拟DOM对比, key存在就复用此标签更新内容, 如果不存在就直接建立一个新的

key为索引-图解过程 (又就地往后更新了)

![image-20220602133721850](images/image-20220602133721850.png)

1. v-for先循环产生新的DOM结构, key是连续的, 和数据对应

2. 然后比较新旧DOM结构, 找到区别, 打补丁到页面上

    最后补一个li, 然后从第二个往后, 都要更新内容

## 有key - 值为id(不重复的值) => 移动更新

> <font color='red'>key的值只能是唯一不重复的, 字符串或数值</font>

v-for不会移动DOM, 而是尝试复用, 就地更新，如果需要v-for移动DOM, 你需要用特殊 attribute(属性) `key` 来提供一个排序提示

新DOM里数据的key存在, 去旧的虚拟DOM结构里找到key标记的标签, **复用标签**

新DOM里数据的key存在, 去旧的虚拟DOM结构里没有找到key标签的标签, **创建/插入**

旧DOM结构的key, 在新的DOM结构里没有了, 则 **移除key所在的标签**



```vue
<template>
  <div>
    <ul>
      <li v-for="obj in arr" :key="obj.id">
        {{ obj.name }}
        <input type="text">
      </li>
    </ul>
    <button @click="btn">下标1位置插入新来的</button>
  </div>
</template>
```

![image-20220602134548860](images/image-20220602134548860.png)

> 总结: 不用key也不影响功能(就地更新), 添加key可以提高更新的性能

问题：在实际开发过程中，每次通过v-for进行数据的遍历都得用id吗？

>  答： 数组对象中，有id就用id，没有id则用索引。如果用id页面报错，则换成索引。

# 面试背点02：- 准备找工作前一周拿出来看看理解理解🔥

**问：如何理解Vue中的diff算法？**

**答：**

​		> 找不同！

​		在js中，渲染真是`DOM`的开销是非常大的，比如我们修改了某个数据，如果直接渲染到真实`DOM` ,会引起整个`DOM`树重绘和重排。那么有没有可能实现只更新我们修改的那一小块`DOM`二不要更新整个`DOM`呢?此时我们就需要先根据真实`DOM`生成虚拟`DOM`，当虚拟`DOM`某个节点的数据改变后会生成有一个新的`VNode`，然后新的`VNode`和旧的`VNode`作比较，发现有不一样的地方就直接修改在真实DOM上，然后旧的`VNode`的值为新的`VNode`；

​		**diff**的过程就是调用`patch`函数，比较新旧节点，一边比较一边给真实的`DOM`打补丁，在采用`diff`算法比较新旧节点的时候，比较自会在同层级进行。



**问：什么是patch函数**

**答：**

​		在`patch`方法中，首先进行树级别的比较`new Vnode`不存在就删除`old VNode`，`old VNode`不存在就增加新的`VNode`都存在就执行`diff`更新，当确定需要执行`diff`算法时，比较两个`VNode`，包括三种类型操作：属性更新，文本更新，子节点更新，新老节点均有子节点，则对子节点进行`diff`操作，调用`updatechidren`如果老节点没有子节点，先清空老节点的文本内容，然后为其新增子节点，如果新节点没有子节点，而老节点有子节点的时候，则移除该节点的所有子节点，老节点都没有子节点的时候，进行文本的替换。

# 动态class与style

>动态添加删除类名

**语法:**

- : class="{类名:布尔值}"
-  :  class="[类名]"
-  :  style="{css属性: 值}"
-  :  style="[css属性名: 值]"

**案例:**

```vue
<template>
  <div>
    <!-- 动态class
    语法：
    1. :class="{ 类名: 布尔值(可以是表达式的返回值) }" => 通过布尔值来判断前面的类名是否生效
    => 语法1，是通过一个布尔值来做某个类名的开关

    2. :class="[ 带有类名字符串的变量 ]" => 可以通过改变这个变量的值，来实现切换不同的类名
    => 语法2，通过改变变量中的类名，来切换样式表中的不同的类的样式

    使用的选择： 如果有一个样式在使用场景中只需要它生效或者不生效，此时选择语法1（花括号键值对）
    如果有一个样式在使用场景中需要实时的变化，此时选择语法2（方括号变量） -->
    <p :class="{skyblue:flag}">hello world</p>
    <button @click="flag = !flag">切换skyblue_str</button>
    <p :class="classname">hello world</p>
    <button @click="changeClass">切换className</button>
    <!-- 动态style
    语法：
    动态style的写法特点： 如果出现多个单词短横线链接的样式属性名时，需要转化为驼峰式命名来设置

    1. :style="{ css样式属性名: 携带当前样式的样式值的变量 }" => 通过一个变量来控制当前显示样式的状态

    2. :style="[ 带有属性键值对的对象 ]" => 可以通过这个对象中的值来变化各种样式属性

    使用的选择: 如果仅需要修改某一个 ‘特定’ 样式的属性内容， 选择语法1（花括号键值对）（90%）
    如果需要修改或切换不同的样式种类时， 选择语法2(方括号变量) -->
    <p :style="{color:styleColor}">动态style</p>
    <p :style="[styleObj]">动态style2</p>
    <button @click="changeStyel">改变动态style</button>
  </div>
</template>

<script>
export default {
  name: 'addClassName',
  data() {
    return {
      flag: true,
      classname: 'skyblue',
      styleColor: 'pink',
      styleObj: {
        color: 'red',
        background: '#ccc'
      }
    }
  },
  methods: {
    changeClass() {
      this.classname = 'pink'
    },
    changeStyel() {
      this.styleObj = { fontSize: '50px' }
    }
  }
}
</script>

<style lang="less" scoped>
.skyblue {
  color: skyblue
}

.pink {
  color: pink
}
</style>


```

# 过滤器filter

>在不改变数据的情况下, 使页面的数据改变. 分为**全局**过滤器与**局部**过滤器, 只能使用在**插值表达式**和**v-bind**表达式

**语法:**

- Vue.filter("过滤器名", (值) => {return "返回处理后的值"})
- filters: {过滤器名字: (值) => {return "返回处理后的值"}

**案例:**

```vue
<template>
  <div>
    <!--
      过滤器的作用： 在不改变data中数据的基础上，在页面上展示不同于数据的内容效果
      过滤器在实际工作中，其实只有一些非常简单且通用的展示方法会去使用过滤器，其他时候一般很少遇到

      过滤器有两种不同的写法
      1. 全局过滤器（定义在全局，所有的组件都可以使用） - filter是Vue构造函数的静态方法
      Vue.filter(
        参数1 - "自定义过滤器名称",
        参数2 - (需要过滤的值-必填, 过滤时传过来的自定义参数-选填) => {
          return '返回处理后的数据'
        })

      2. 局部过滤器(定义在组件中，只有当前的组件才可以使用)
      和data同级的地方有一个属性，叫做filters
      filters: {
        '自定义过滤器名称' (需要过滤的值-必填, 过滤时传过来的自定义参数-选填) {
          return '返回处理后的数据'
        }
      }

      过滤器的使用 - 在插值表达式中，通过 ‘|’ 来链接变量和过滤器
      {{ 变量 | 过滤器(传入自定义的参数) }}
    -->
    <!-- 全局过滤器 -->
    <div>
      {{str | reverseG}}
    </div>
    <div>
      {{str | reverseG('-')}}
    </div>
    <!-- 局部过滤器 -->
    <div>
      {{str | toUp | reverseG}}
    </div>
    <!-- 直接写 -->
    {{str.toUpperCase()}}
  </div>
</template>

<script>
export default {
  name: 'filtersDemo',
  data() {
    return {
      str: 'hello world'
    }
  },
  filters: {
    toUp(str) {
      return str.toUpperCase()
    },
  }
}
</script>

<style>

</style>
```

# 计算属性computed

>当一个或多个变量发生变化时, 依赖它的变量也同时变化
>
>优: 因为缓存机制，所以不会重复的进行计算，页面效率高
>
>缺: 因为缓存机制，计算属性会消耗内存
>
>总:  空间换时间

**语法:**

```vue
computed: {
        '自定义的计算属性名称' () {
          return '计算后返回的值'
        }
      }
```

**注意: **data中的变量与计算属性的名称不可以相同

**案例:**

```vue
<template>
  <!-- 
      computed计算属性语法
      在data同级，创建computed
      computed: {
        '自定义的计算属性名称' () {
          return '计算后返回的值'
        }
      }

      计算属性的使用： 与data中的变量的使用方式是一样的
      注意事项：data中的变量与计算属性的名称不可以相同

      计算属性的特点： 计算属性具有缓存结果的特点，计算属性基于它的依赖项（计算属性中所使用的其他变量）进行计算的结果会缓存起来，只要依赖项的值不变，那么下次使用计算属性时不必进行计算，只会返回缓存中的结果，对于代码效率来说是比较高的

      计算属性的优缺点：
      优点：因为缓存机制，所以不会重复的进行计算，页面效率高
      缺点：因为缓存机制，计算属性会消耗内存
      ---- 空间换时间 ----
     -->
  <div>
    <div>a:{{a}}</div>
    <div>b:{{b}}</div>
    <div>c=a+b:{{c}}</div>
    <div>c=a+b:{{c}}</div>
    <div>c=a+b:{{c}}</div>
    <div>c=a+b(function):{{sum()}}</div>
    <div>c=a+b(function):{{sum()}}</div>
    <div>c=a+b(function):{{sum()}}</div>
  </div>
</template>

<script>
export default {
  name: 'computedDemo',
  data() {
    return {
      a: 10,
      b: 20
    }
  },
  computed: {
    'c'() {
      console.log('计算属性')
      return this.a + this.b
    }
  },
  methods: {
    sum() {
      console.log('方法')
      return this.a + this.b
    }
  }
}
</script>

<style>

</style>
```

## 计算属性的完整写法

>计算属性也是变量, 如果想要直接赋值, 需要使用完整写法
>
>有缺陷, 基本用不着, 常用于全选反选

**语法:**

```vue
computed: {
    "属性名": {
        set(值){
            
        },
        get() {
            return "值"
        }
    }
}
```

# 全选反选案例

```vue
<template>
  <div>
    <span>全选:</span>
    <input type="checkbox" v-model="isAll" />
    <button @click="reverseBtn">反选</button>
    <ul>
      <li v-for="(obj,index) in arr" :key="index">
        <input type="checkbox" v-model="obj.c" />
        <span>{{obj.name}}</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      arr: [
        {
          name: "猪八戒",
          c: false,
        },
        {
          name: "孙悟空",
          c: false,
        },
        {
          name: "唐僧",
          c: false,
        },
        {
          name: "白龙马",
          c: false,
        },
      ],
    };
  },
  computed: {
    'isAll': {
      set(value) {
        // value表示当前isAll的计算结果
        this.arr.forEach(obj => obj.c = value)
      },
      get() {
        return this.arr.every(obj => obj.c)
      }
    }
  },
  methods: {
    reverseBtn() {
      this.arr.forEach(obj => obj.c = !obj.c)
    }
  }

};
</script>
```

# computend和watch的区别🔥

>**computed计算属性**该属性的结果会被缓存，当`computed`中的函数所依赖的属性没有发生改变的时候，那么调用当前函数的时候结果会从缓存中读取。除非依赖的响应式属性变化时才会重新计算，主要当做属性来使用`computed`中的函数必须用`return`返回最终的结果，`computed`更高效，优先使用
>
>优: 因为缓存机制，所以不会重复的进行计算，页面效率高
>
>缺: 因为缓存机制，计算属性会消耗内存
>
>总:  空间换时间
>
> **watch属性监听**是一个对象，键是需要观察的属性，值是对应回调函数，主要用来监听某些特定数据的变化，从而进行某些具体的业务逻辑操作，监听属性的变化，需要在数据变化时执行异步或开销较大的操作使用。
>
> **使用场景：**`computed`当一个属性受多个属性影响的时候使用，例：购物车结算功能； `watch`当一条数据影响多条数据的时候使用，例：搜索数据。

# 监听器watch

>监听变量(data或计算属性),当监听的变量或值发生变化时,就会触发监听器中设置的逻辑 - 一个数据的改变能够影响多个数据的变化

**语法:**

```vue
// 简单数据类型
watch:{
	'被监听的变量(属性)名'(newValue, oldValue){
		// newValue: 监听这个变量更改后的值
		// oldValue: 监听这个变量更改前的值
	}
}
// 复杂数据类型(完整写法)
watch:{
	'被监听的变量(属性)名':{
		handler(newValue, oldValue){
		// newValue: 监听这个变量更改后的值
		// oldValue: 监听这个变量更改前的值
		},
		deep: true // 启动深度监听复杂类型
		immediate: true // 立即执行 - 当监听器生成时, 就会执行一次handler里面的逻辑
	}
}
```

**案例:**

```vue
<template>
  <div>
    <input type="text" v-model="str" />
    <form action="">
      <input type="password" v-model="form.pwd" autocomplete='off'>
    </form>
  </div>
</template>

<script>
export default {
  name: "watch-demo",
  data() {
    return {
      str: "",
      form:{
        pwd:''
      }
    };
  },
  watch: {
    str(newValue, oldValue) {
      console.log(newValue, oldValue);
    },
    'form':{
      handler(newValue,oldValue){
        console.log(newValue,oldValue)
      },
      deep:true
    }
  },
};
</script>

<style>
</style>
```

# 组件

>作用: 将一个页面的代码拆分成多个组件, 这样便于阅读; 可以反复使用同一个组件, 而且反复使用的组件中的数据也是独立的	

**语法:**

```vue
<template>
	<!-- 3. 挂载 -->
	<组件名></组件名>
</template>
<script>
// 1. 引入
import 自定义组件名 from '组件所在路径'
export default {
    name: "foldPanel",
    components: {
        // 2.注册
        自定义组件名:使用的组件名
        // 一般情况只写一个 名字与值相同
    }
}
</script>
```

**案例:**

```vue
<template>
  <div id="app">
    <h3>案例：折叠面板</h3>
    <!-- 3. 挂载 -->
    <pannel></pannel>
    <pannel></pannel>
    <pannel></pannel>
    <pannel></pannel>
  </div>
</template>


<script>
// 1. 引入
import Pannel from "./pages/pannel.vue";
export default {
  name: "foldPanel",
  components: {
    // 2. 注册
    Pannel,
  },
};
</script>

<style lang="less" scoped>
body {
  background-color: #ccc;
  #app {
    width: 400px;
    margin: 20px auto;
    background-color: #fff;
    border: 4px solid blueviolet;
    border-radius: 1em;
    box-shadow: 3px 3px 3px rgba(0, 0, 0, 0.5);
    padding: 1em 2em 2em;
    h3 {
      text-align: center;
    }
    
  }
}
</style>
```

# 父子组件的通信

**语法:**

```vue
// 1. 在父组件的挂载标签内以键值对的方式将需要传递的数据写入  注: 可以配合 v-for 大量传递数据
<Product title="口水鸡" price="99.99" content="开业大酬宾,全场八折"></Product>
// 2. 在子组件通过props将父组件传递过来的数据接收
props:['title','price','content']
```

**案例:**

父组件

```Vue
<template>
  <div>
    <Product
      title="口水鸡"
      price="99.99"
      content="开业大酬宾,全场八折"
    ></Product>
    <Product :title="title" :price="price" :content="content"></Product>
    <Product
      v-for="(obj, index) in arr"
      :key="index"
      :title="obj.title"
      :price="obj.price"
      :content="obj.content"
    ></Product>
  </div>
</template>

<script>
import Product from "./pages/MyProduct.vue";
export default {
  name: "product-App",
  components: {
    Product,
  },
  data() {
    return {
      title: "无骨鸡爪",
      price: 50,
      content: "超级加倍",
      arr: [
        {
          title: "a1",
          price: "a2",
          content: "a3",
        },
        {
          title: "b1",
          price: "b2",
          content: "b3",
        },
        {
          title: "c1",
          price: "c2",
          content: "c3",
        },
      ],
    };
  },
};
</script>

<style>
</style>
```

子组件

```vue
<template>
  <div class="my-product">
    <h3>标题: {{title}}</h3>
    <p>价格: {{price}} 元</p>
    <p>描述: {{content}}</p>
  </div>
</template>

<script>
export default {
  name:'productPannel',
  props:['title','price','content']
};
</script>

<style>
.my-product {
  width: 400px;
  padding: 20px;
  border: 2px solid #000;
  border-radius: 5px;
  margin: 10px;
}
</style>
```

# 单向数据流

>从父到子的数据流向，叫做单向数据流
>

**在vue中需要遵循单向数据流原则**

  1. 父组件的数据发生了改变，子组件会自动跟着变

  2. 子组件不能直接修改父组件传递过来的props  props是只读的（这里的只读，是栈内存中的数据只读，可以改堆）**父组件传给子组件的是一个对象，子组件修改对象的属性，是不会报错的，对象是引用类型, 互相更新**

     ```vue
     <template>
       <div class="my-product">
         <h3>标题: {{ title }}</h3>
         <p>价格: {{ price }}元</p>
         <p>{{ intro }}</p>
         <button @click="() => {price--}">宝刀-砍1元</button>
       </div>
     </template>
     ```

# 在子组件修改父组件传递过来对象属性的方式

在子组件的data中, 拷贝一个父组件传递过来的对象, 通过大家都有相同的堆地址, 来间接的修改父组件中的对象参数

**基本语法:**

父组件

```vue
<template>
	<Product :obj='obj'></Product>
</template>
<script>
data(){
	return{
        obj{
         title: "a1",
         price: 20,
         content: "a3",
    }
    }
}
</script>
```

子组件

```vue
<template>
	<button @click="objCloon.price--">砍一刀,价格减一</button>
</template>
<script>
props:['obj'],
data(){
    return{
        objCloon:this.obj
    }
}
</script>
```

# 子父组件通信

> 目标： 从子组件把值传出来给外面使用

**语法:**

子组件

```vue
<template>
	<button @click="btn">砍一刀,价格减一,子向父</button>
</template>
<script>
methods:{
    props:[title],
	btn(){
		this.$emit('subPrice',this.title)
	}
}
</script>
```

父组件

```vue
<template>
	 <Product @subPrice='subPriceFn'></Product>
</template>
<script>
data(){
    return{
        arr[
        	{
        		title:a,
    			price:20
    		},
        	{
        		title:b,
        		price:30
			}
        ]
	}
}
methods: {
    subPriceFn(title) {
      this.arr.forEach((obj) => {
        if (obj.title === title) obj.price--;
      });       
    },
  },
</script>
```

# props进阶用法(work)

**说明：**props也可以作为一个对象，并且可以指定数据类型，必要性，默认值

**语法:**

```vue
export default {
	props: {
        '接收父组件传递过来的属性名': {
            type: String, // 必填 - 传参类型
            required: true, // 是否必传（一般也不写，了解）如果没传,只会控制台报错,不影响正常运行
            default: 'string' // 默认值
        },
        b: {
            type: Array,
            default: () => []	
        },
        c: {
            type: Object,
            default: () => ({})
        }
    }
};
```

# 跨组件传参 - EventBus

为何不使用?

1. 无法精确的确认当前创建的自定义事件是否在之前被使用了，或者有其他用途
2. 接收方无法确保拿到的数据，是你目标组件发送来的

# 生命周期🔥

## 初始化之前 - beforeCreate

>创建vue实例, 初始化事件和生命周期函数, 什么事情都不能做

## 初始化之后 -  created

>在created中是最先获取data数据，使用methods方法等的时机
>
>**使用场景：** 当处理数据（静态数据，通过网络请求后端获取的数据），注册一些全局事件时，应该设置在created中

## 挂载之前 - beforeMount

>基本与created相同 只是多了虚拟dom

## 挂载之后 - mounted

> 此时真实dom已挂载到页面上，可以执行dom操作, 同时也可以堆数据进行处理
>
> **注意事项：**如果你比较懒，同时也不确定是否需要左dom操作，那么直接把逻辑放到mounted里面绝不会错

## 更新之前 - beforeUpdate

>获取不了更新之后的DOM节点

## 更新之后 - updated

>可以获取更新之后的DOM节点
>
>**注意:** update阶段会产生无限的更新循环, 所以以后如果需要在数据变化时做业务操作, 用watch监听器来实现

## 销毁之前 - beforeDestory

>**使用场景:** 清除定时器 取消订阅消息 解绑自定义事件等**异步**操作

## 销毁之后 - destoryed

>啥也干不了

# res and refs

##  获取DOM元素

**案例:**

```vue
<template>
	<div>
        <p ref='demo'>
            hello wrold
    	</p>
    </div>
</template>
mounted(){
	console.log(this.$refs.demo)
}
```

## 使用子组件的方法与属性

子组件

```vue
<template>
	<div>
        {{msg}}
    </div>
</template>
data(){
	return{
		msg:'children'
	}
}
methods:{
	changMsg(txt){
		this.msg = txt
	}
}
```

父组件

```vue
<template>
	<div>
        <child ref='child'></child>
    </div>
</template>
import child from 子组件路径
components:{
	child
}
mounted(){
    // 虽然可以通过refs将data中的数据读取到父组件中，而且可以通过父组件直接修改当前的数据，但是这种形式非常难于对数据进行规范的	      管理，所以，父组件中读取到的子组件数据只可使用不可修改
	console.log(this.$refs.child.msg)  
	//  如果父组件一定要修改子组件的数据，那么子组件中需要暴露出一个专门用于修改某一特定属性的方法， 父组件通过$refs来进行调用		  和修改
	this.$refs.child.changeMsg('father say')
}
```

# nextTick

>Vue更新DOM-异步的!（数据与页面有延时）

**案例：**点击count++, 马上通过"原生DOM"拿标签内容, 无法拿到新值

```vue
<template>
  <div>
    <p ref="myP">{{ count }}</p>
    <button @click="btn">count+1</button>
  </div>
</template>

<script>
export default {
  name: "nextTickDemo",
  data() {
    return {
      count: 0,
    };
  },
  methods: {
    btn() {
      this.count++;
      console.log("数据中的值", this.count);
      console.log("页面中的值", this.$refs.myP.innerText);

      // 方案一: 使用setTimeout让后面代码延时执行
      setTimeout(() => {
        console.log("页面中的值", this.$refs.myP.innerText);
      }, 0);

      // 方案二: 使用vue内置方法$nextTick(()=>{ })
      // 作用: 该方法仅在dom更新后进行触发执行
      this.$nextTick(() => {
        console.log("页面中的值", this.$refs.myP.innerText);
      });
    },
  },
};
</script>

<style>
</style>
```

# 动态组件

>多个组件使用同一个挂载点，并动态切换

**语法:**

```vue
<template>
	<component :is="组件名"></component>
</template>
```

**案例:**

```vue
<template>
  <div>
    <button
      @click="
        flag = true
        componentName = 'UserName'
      "
    >
      账号密码填写
    </button>
    <button
      @click="
        flag = false
        componentName = 'UserInfo'
      "
    >
      个人信息填写
    </button>
    <UserName v-if="flag"></UserName>
    <UserInfo v-else></UserInfo>
    <hr />
    <!-- keep-alive组件缓存 -->
    <keep-alive>
      <component :is="componentName"></component>
    </keep-alive>
  </div>
</template>
<script>
import UserName from '../components/04-userName.vue'
import UserInfo from '../components/03-userInfo.vue'
export default {
  name: 'userDynamic',
  components: {
    UserName,
    UserInfo,
  },
  data() {
    return {
      flag: true,
      componentName: 'UserName',
    }
  },
}
</script>
<style lang="scss" scoped>
</style>

```

# 组件缓存

>将动态组件<component></component>放在keep-alive缓存组件中时，在组件切换的过程中，将不会进行销毁

**语法:**

```vue
<template>
	<keep-alive>
		<component :is="组件名"></component>
    </keep-alive>
</template>
```

# keep-alive钩子函数

>被缓存的组件不再创建和销毁, 而是激活和非激活
>
>**作用:** 在缓存组件中有些需要进入组件时就需要执行的代码从created和mounted中转换到activated里
>          在缓存组件中，如果有计时器定时器需要在页面关闭时主动关闭，需要将关闭的代码从beforeDestroy中转换到deactivated里

**生命周期:**

- activated - 激活
- deactivated - 失去激活状态

**案例:**

```vue
<script>
created() {
    console.log('组件创建了')
  },
  mounted() {
    console.log('页面挂载')
  },
  activated() {
    console.log('组件激活')
  },
  deactivated() {
    console.log('组件失活')
  },
  beforeDestroy() {
    console.log('组件销毁')
  },
</script>
```

# 自定义指令(了解)

## 全局注册 - main.js

**语法:**

```vue
Vue.directive('自定义指令名称(不带v-)',{
	inserted(element,binding){ // 当绑定元素插入到父元素中时 - 当前绑定的标签渲染到页面时
		// element: 当前绑定的元素
		对该元素进行业务处理
		// binding 传递过来的参数 
	},	
	update(element){ //当标签发生更新时, 会自动触发一下内容

	}
})
```

**案例:**

```vue
// main.js
Vue.directive('gFocus', {
  inserted(element,binding) {
    console.log(binding)
    element.focus()
    element.style.color=binding.value
  }
})
// 组件
<input type="text" v-gFocus="'red'">
```

## 局部注册

**语法:**

```vue
directives: {
        '自定义指令名称（不带v)': {
          inserted (elemen,bindingt) { // 当绑定元素插入到父元素中时 - 当前绑定的标签渲染到页面时
            // element: 当前绑定的元素
            对该元素进行业务处理
			// binding 传递过来的参数 
          },

          update (element) { // 当前标签发生更新时，会自定触发一下内容
          }
        }
```

# slot插槽

>用于实现组件的内容分发, 通过 slot 标签, 可以接收到写在组件标签内的内容

## **基本用法:**

子组件

```vue
<template>
  <div>
    <!-- 按钮标题 -->
    <div class="title">
      <h4>芙蓉楼送辛渐</h4>
      <span class="btn" @click="isShow = !isShow">
        {{ isShow ? '收起' : '展开' }}
      </span>
    </div>
    <!-- 下拉内容 -->
    <div class="container" v-show="isShow">
      <slot>
        <p>默认内容</p>
      </slot>
    </div>
  </div>
</template>
```

父组件

```vue
<template>
  <div>
    <Panle>
        <p>寒雨连江夜入吴,</p>
        <p>平明送客楚山孤。</p>
        <p>洛阳亲友如相问，</p>
        <p>一片冰心在玉壶。</p>
    </Panle>
    <panle></panle>
  </div>
</template>
```

## 具名插槽

>当一个组件内有2处以上需要外部传入标签的地方，传入的标签可以分别派发给不同的slot位置
>
>**注意事项：** 如果父组件中子组件标签内的某些元素没有正确的设置对应插槽名称时，就不会在子组件插槽中生效

子组件

```vue
<template>
  <div>
    <!-- 按钮标题 -->
    <div class="title">/
      <h4><slot name="title">默认标题</slot></h4>
      <span class="btn" @click="isShow = !isShow">
        {{ isShow ? '收起' : '展开' }}
      </span>
    </div>
    <!-- 下拉内容 -->
    <div class="container" v-show="isShow">
      <slot name='content'>
        <p>默认内容</p>
      </slot>
    </div>
  </div>
</template>
```

父组件

```vue
<template>
  <div>
    <Panle>
      <template #title> 芙蓉楼送辛渐 </template>
      <template v-slot:content>
        <p>寒雨连江夜入吴,</p>
        <p>平明送客楚山孤。</p>
        <p>洛阳亲友如相问，</p>
        <p>一片冰心在玉壶。</p>
      </template>
    </Panle>
    <panle></panle>
  </div>
</template>
```

## 作用域插槽

>让子组件中的某些变量可以提供给父组件使用

子组件

```vue
<template>
  <div>
    <!-- 按钮标题 -->
    <div class="title">
      <h4><slot name="title">默认标题</slot></h4>
      <span class="btn" @click="isShow = !isShow">
        {{ isShow ? '收起' : '展开' }}
      </span>
    </div>
    <!-- 下拉内容 -->
    <div class="container" v-show="isShow">
      <slot name="content" :dataObj="obj" :dataArr="arr">
        <p>默认内容</p>
      </slot>
    </div>
  </div>
</template>
data() {
    return {
      isShow: false,
      obj: {
        name: '张三',
        age: 18,
      },
      arr: [1, 23, 4, 5],
    }
  },
```

父组件

```vue
<template>
  <div>
    <Panle>
      <template #title> 芙蓉楼送辛渐 </template>
      <template v-slot:content="scope">
        <div>dataObj:{{scope.dataObj}}</div>
        <div>dataArr:{{scope.dataArr}}</div>
        <p>寒雨连江夜入吴,</p>
        <p>平明送客楚山孤。</p>
        <p>洛阳亲友如相问，</p>
        <p>一片冰心在玉壶。</p>
      </template>
    </Panle>
    <panle></panle>
  </div>
</template>
```

# veu-router 路由🔥

>在一个同一个页面,切换业务场景,不会刷新页面

**用法:**

```vue
// 1. 下载vue-router
npm i vue-router@3.5.1   // 注: vue2
// 2. 全局引入
import VueRouter from 'vue-router'
// 3. 全局注册
Vue.use(VueRouter)
// 4. 创建路由规则
const routes = [
  {path:路由的唯一路径,component:组件名}
]
// 5. 生成路由对象
const router = new VueRouter({
  routes
})
// 6. 关联到Vue实例
new Vue({
  router
})
// 7. 设置挂载点 - 当url的hash值路径切换, 显示规则里对应的组件到这
<router-view></router-view>
```

## 声明式导航

>特点： 
>        1. 本质上就是声明一个实际的标签。通过该标签进行路由跳转(<router-link>)，这个标签本质上<a>标签
>        2. 当路由在变化时， 声明式导航标签会为当前路由绑定的按钮添加动态class类名，我们就可以通过这个动态的类名来设置高亮样式
>
>与a标签的区别
>    1. a标签的路径跳转基于 href属性，而router-link标签基于 to
>    2. a标签跳转的路径是 # + 规则列表的path, 而router-link的路径 直接时 规则列表的path

**案例:**

路由规则

```vue
import Father from './views/father.vue'
const routes = [
	{
		path:'/father', // 路由路径
		component:Father // 路由组件
	}
]
```

路由组件

```vue
<template>
	<router-link to="/father">toFather</router-link>
</template>
```

## 声明式导航传参

>传参方式
>    1. 传递： to="/path?参数名1=参数值1&参数名2=参数值2"
>       接收： $route.query.参数名
>
>2. 传递:  to="/path/参数值"
>          在路由规则数组中path部分定义 path: '/path/:参数名'
>     接收:  $route.params.参数名
>
>声明式传参的选择： 首选 方案1
>1. 方案二可读性差， 参数名与参数值定义在不同的文件中，在后续的代码阅读上，会带来困难
>2. 方案二在设置参数值的时候，必须保留原路由规则，如果不保留，会出现如果不传参时导致找不到目标组件的情况
>3. 如果说某个组件要求用户必须传递某个参数时，方案二反而会给各位带来更好的效果

**案例(方案一):**

父组件

```vue
<template>
	<router-link to='/father01?name=我是father01'>toFather01</router-link>
</template>
```

子组件

```vue
<template>
	<div>接收到的参数:{{$route.query.name}}</div>
</template>
```

**案例(方案二):**

规则数组

```vue
import Father from './views/father.vue'
const routes = [
	{
		path:'/father/:name', // 路由路径
		component:Father // 路由组件
	}
]
```

父组件

```vue
<template>
	<router-link to='/father/我是father'>toFather</router-link>
</template>
```

子组件

```vue
<template>
	<div>接收到的参数:{{ $route.params.uname }}</div>
</template>
```

## 路由重定向

>场景：首次进入页面时，没有任何路由的hash值，页面元素会显示空白，如何解决？

**原因:**

- 网页打开url默认hash值是/路径
- redirect是设置要重定向到哪个路由路径

**解决方案:**

```vue
const routes = [
  {
    path: "/", // 默认hash值路径
    redirect: "/find" // 重定向到/find
    // 浏览器url中#后的路径被改变成/find-重新匹配数组规则
  }
]
```

## 路由404页面

>路由hash值, 没有和数组里规则匹配

在路由规则里添加 (一般在末尾,由于版本原因,**老版本**的如果在**开头**添加,会直接使它之后的**所有规则无效**)

```vue
import NotFound from './views/notFound.vue'
const routes = [
  {
    path: "*", 
    component:NotFound 
  }
]
```

## 路由模式

>让url地址是否显示#号
>
>模式分为:
>
>hash路由例如: http://localhost:8080/#/home
>
>history路由例如: http://localhost:8080/home (以后上线需要服务器端支持, 否则找的是文件夹)

**使用方式:**

```vue
const router = new VueRouter({
  routes,
  mode: "history" // 打包上线后需要后台支持, 模式是hash
})
```

## **hash模式和history模式的原理和区别**

**区别：**

1. hash 能兼容到IE8， history 只能兼容到 IE10；
2. 由于 hash 值变化不会导致浏览器向服务器发出请求，而且 hash 改变会触发 hashchange 事件（hashchange只能改变 # 后面的url片段）；虽然hash路径出现在URL中，但是不会出现在HTTP请求中，对后端完全没有影响，因此改变hash值不会重新加载页面，基本都是使用 hash 来实现前端路由的。

**原理：**

1. hash通过监听浏览器的onhashchange()事件变化，查找对应的路由规则

2. history原理： 利用H5的 history中新增的两个API pushState() 和 replaceState() 和一个事件onpopstate监听URL变化

## 编程式导航

>编程式导航的跳转方式
>        1. $router.push({ path: '路由路径' })
>           例如： $router.push({ path: '/find' })
>
> 2. $router.push({ name: '路由名称' })
>       例如：$router.push({ name: 'find' })
>
>    注意事项： 
>    
>    1. 原则上，让name与path的名称保持一致增加代码可读性
>    2. 关于根路由'/'，一般情况下是不用设置name的，如果要设置的话 建议设置为'main'或者是'home'
>    3. 一般情况下重定向的路由规则就用path来进行跳转，不需要设置name属性
>    4. 声明式导航配置的路由规则可以不设置name属性，因为编程式导航不会用到该规则

**案例(方案一):**

```vue
<template>
	<span @click="toFn">toFather</span>
</template>
<script>
methods:{
    toFn(){
      this.$router.push({
        path:'/father'
      })
    }
}
</script>
```

**案例(方案二):**

规则数组

```vue
import Father from './views/father.vue'
const routes = [
	{
		path:'/father/:name', // 路由路径
		component:Father // 路由组件
		name:'father' // 自定义路由名
	}
]
```

组件

```vue
<template>
	<span @click="toFn">toFather</span>
</template>
<script>
methods:{
    toFn(){
      this.$router.push({
        name:'father'
      })
    }
}
</script>
```

## 编程式导航传参

> 编程式导航的数据传参
>    1. query对象 传递 => $route.query.参数名
>        现象：
>      1. 通过query对象的传参方式，会以?参数名=参数值的形式自动的添加到url地址上
>      2. 无论是path跳转还是name跳转，都可以在路由组件中通过$route.quer来获取参数
>
> 结论：
>  query传参适用于path、name路由跳转
>
>2. params对象 传递 => $route.params.参数名
>  现象：
>  
>  1. params对象传参，不会将参数显示到url上，而且当刷新页面时数据会丢失
>  2. 通过path跳转，params传参的形式，无法在路由组件中获取到数据
>  
>  结论：
>  params传参必须通过name来进行跳转
>
>=> 开发要求：
>如果通过path跳转的路由 =》 query进行参数传递
>如果通过name跳转的路由 =》 params进行参数传递
>如果跳转的路由地址可能涉及到**刷新页面** =》 选择path的跳转方式

**案例:**

```vue
this.$router.push({
    path: "路由路径"
    name: "路由名",
    query: {
        "参数名": 值
    }
    params: {
        "参数名": 值
    }
})

// 方式1:
// query => $route.query.参数名

// 方式2:
// params => $route.params.参数名
```

**格外注意:** 使用path的话，只能和query一起使用，不可以使用params(会自动忽略)

## 路由嵌套

>路由嵌套 - 设置二级路由
>
>1. 在children属性里面设置的路由地址，不必在前面加/
>2. 在组件中进行跳转路由时，如果路由有嵌套层级，那么跳转时需要把这个层级拼接完整
>    例如： '/father/son'
>
>3. 如何让一级路由展示时默认开启某个二级路由
>    3.1 让某个二级路由作为默认值 =》 将一个path设置为 '' 空字符串，但是这种方式是存在问题的
>    3.2 通过重定向来实现默认展示（工作中最为标准的展示默认路由的方式）
>    当路由跳转到目标一级路由时，在一级路由中设置重定向属性，定向到二级路由

**案例:**

路由规则

```vue
import Father from './views/father.vue'
import Son01 from './views/son01.vue'
import Son02 from './views/son02.vue'
import Son03 from './views/son03.vue'
const routes = [
	{
    path: '/father',
    component: Father,
    name:'father',
    children: [
      {
        path: 'son01',
        component: Son01,
        name:'son01'
      },
      {
        path: 'son02',
        component: Son02,
        name:'son02'
      },
      {
        path: 'son03',
        component: Son03,
        name:'son03'
      },
    ]
  }
]
```

父组件

```vue
<template>
	<div>
        <span @click="toSon01">toSon01 | </span>
        <span @click="toSon02">toSon02 | </span>
        <span @click="toSon03">toSon03 | </span>
      </div>
	 <div>
        <router-view></router-view>
     </div>
<template>
<script>
    methods:{
    toSon01(){
      this.$router.push({
        name:'son01'
      })
    },
    toSon02(){
      this.$router.push({
        name:'son02'
      })
    },
    toSon03(){
      this.$router.push({
        name:'son03'
      })
    },
  }
</script>
```

## 声明导航 - 类名的区别

>关于声明式导航类名的高亮样式选择
> 如果当前router-link中to所引申出来的路由地址，它的规则数组中还有更深层次的路由组件
>
>1. 如果没有children 那么使用精准匹配的类名  router-link-exact-active
>2. 如果有children 并且在使用， 那么选择模糊匹配的类名 router-link-active

## 全局前置守卫

>目的: 路由跳转之前, 先执行一次前置守卫函数, 判断是否可以正常跳转

### beforeEach

**语法:**

```vue
// 创建vue-router实例
const router = new Router({
  routes,
  mode: "history"
})
router.beforeEach((to,from,next)=>{})
// to表示要跳转到的位置
// from表示当前所在的位置
// next(true) 表示允许跳转
// next(false) 表示不允许跳转
// next(path路径) 表示强制换成path路径跳转
```

### afterEach

**语法:**

```vue
// 创建vue-router实例
const router = new Router({
  routes,
  mode: "history"
})
router.afterEach((to,from)=>{})
```

