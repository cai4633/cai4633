---
title: vue-cli本地代理mock数据
date: 2020-05-25 22:33:10
categories: 前端
tags: ['vue-cli','devServer','proxy']
---

近期在做一个项目需要从qq音乐服务器抓取数据，但是由于浏览器同源策略的限制导致获取数据失败，需要从本地nodejs服务器中代理抓取数据，下面是vue-cli中的常见的配置。
## vue-cli 2.0之前的版本配置
找到**build/dev-server.js**文件，加入以下代码：
```
var axios = require('axios')
var app = express()
var apiRoutes = express.Router()

apiRoutes.get('/getSongs', (req, res) => {
  var url = 'http://www.baidu.com'
  axios.get(url, {
    headers: {
      referer: 'http://www.baidu.com',
      host: 'http://www.baidu.com'
    },
    params: req.query
  }).then((response) => {
    res.json(response.data)
  }).catch((e) => {
    console.log(e)
  })
})

app.use('/api', apiRoutes)
```

---

## vue-cli 3.0+版本配置
3.0+版本的vue已将dev-server.js与webpack.dev.conf.js合并，若要写路由相关配置需要找到webpack.dev.conf.js中的**devServer对象**进行相关配置
```
const axios = require("axios")
const express = require("express")
let app = express()
let apiRoutes = express.Router()
app.use("/api", apiRoutes)

devServer: {
        before(app) {
            app.get("/api/getSongs", function(req, res) {
                var url = "http://www.baidu.com"
                axios.get(url).then((response) => { res.json(response) }) .catch((e) => { console.log(e) }) })
        },
    },
```

---

## vue-cli 4.0+版本配置
4.0+版本的vue-cli配置藏的更深，需要找到`node_modules/@vue/cli-service/options.js`文件中的**devServer**对象进行配置，配置方法同上面3.0版本类似。另外我们还可以在项目根目录自己新建一个vue.config.js配置文件，新建devServer对象利用**before(app)或者proxy**来进行请求代理。
```
const path = require("path")
function resolve(dir) {
    return path.join(__dirname, dir)
}

const axios = require("axios")
const express = require("express")
let app = express()
let apiRoutes = express.Router()
app.use("/api", apiRoutes)

module.exports = {
    chainWebpack: (config) => {
        config.resolve.alias
            .set("@", resolve("src"))
            .set("assets", resolve("src/assets"))
            .set("components", resolve("src/components"))
            .set("base", resolve("baseConfig"))
            .set("public", resolve("public"))
    },
    devServer: {
        before(app) {
            app.get("/api/lyric", function(req, res) {
                var url = "http://www.baidu.com"
                axios
                    .get(url)
                    .then((response) => {
                        res.json(response)
                    })
                    .catch((e) => {
                        console.log(e)
                    })
            })
        },
        // proxy: {
        //     "/api/getLists": {
        //         target:
        //             "http://www.baidu.com", //代理接口
        //         changeOrigin: true,
        //         pathRewrite: {
        //          '^/api': ''    //代理的路径 //是否移除api三个字段,
        //         }
        //         headers: {
        //             Referer: "https://www.baidu.com",
        //         },
        //     },
        // },
    },
}

```

