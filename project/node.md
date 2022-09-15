# 导入导出

## 导出

1. module.exports = {属性名:属性值}
2. module.exports.变量名=值

## 导入

const {a,a1}=require('./a')

# 全局变量

global

# 使用npm下载插件

1. npm init -y npm初始化               // -y表示后面的选项都同意 可以不写
2. npm install express(插件名)       //  install可以缩写为i

# 开启一个后端服务

```javascript
// 1. 引入express
const express = require('express')
// 2. 创建express实例对象
const obj = express()
// 3. 通过实例对象身上的get() post() put() delete() all()等
//    方法接收http请求(注意: 只要客户端发送http请求, 就会自动触发对应的函数)
obj.all('/API/delbook', (req, res) => {
  console.log('监听到客户端的请求了');
  // 允许跨域
  res.setHeader('Access-Control-Allow-Origin', '*')
  res.setHeader('Access-Control-Allow-Methods', '*')
  res.setHeader('Access-Control-Allow-Headers', '*')
  // 返回客户端数据
  const data = {
    status: 200,
    msg: '删除图书成功'
  }
  res.send(data)
})
// 4. 启动服务
obj.listen(3000)
```

# 网址根目录设置

express实例对象.user(express.static(被作为静态文件的文件位置))

# 获取客户端发送的数据

- url动态参数  req.params
- 查询参数 req.query
- 请求头 req.get('content-type')
- 请求体
    - 原生 
    - 插件 
        1. app.use(express.json)
        2. app.use(express.urlencoded({extended:true}))