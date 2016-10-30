---
layout: post
title: 实战webpack+vue
---

变懒了，发现原来已经半年没写过一篇东西了，这半年，又换了一个工作的地方，现在觉得，人不能太浮躁。当然，也还是要有跳出舒适区的决心。

### 为什么使用vue
虽然换了工作，但是工作内容还是没有变化多少，目前为止接触的还是一些to B的管理系统项目，作为一个前端，没能负责一个to C的产品，还是有点小伤感的。好的是没有旧项目要维护，一切从零开始，而且不用兼容低版本浏览器。所以项目在技术选型上是很自由的。现在基本上是使用了webpack＋vue的技术方案。之所以选择vue有以下几个原因：

* vue结合了Angular和React的优点，可以方便的使用数据双向绑定，又可以方便的实现组件化
* 很重要的一点，公司有使用vue的项目经验
* vue上手比较快，基本上看一天教程就可以上手干活了（这点对我们来说非常重要。。）
* 社区现在也很活跃，作者尤大貌似要全职做vue开发了

因为ES6是后面的趋势，所以就直接上了vue推荐的方案：使用webpack+ES6+vue-loader。

### 先弄个ES6吧
以下进入正题，首先从package.json文件开始吧，先看一下需要什么npm包能够用ES6把vue写起来：

```
{
  "name": "vue-webpack",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "babel-core": "^6.11.4",
    "babel-loader": "^6.2.4",
    "babel-preset-es2015": "^6.9.0",
    "webpack": "^1.13.1"
  },
  "dependencies": {
    "vue": "^1.0.26"
  }
}
```

我们除了使用babel-core和babel-loader外，还要使用babel-preset-es2015，因为在babel 6之后，默认没有指定转换的js版本，所以这里跑了webpack之后是不会将ES6代码转换的，如果需要转换ES2015，即需要安装多这个包。再写好webpack.config.js后，我们就可以愉快地开始coding了：

```
var path = require('path');
module.exports = {
    entry: "./entry.js",
    output: {
        path: './static',
        publicPath: '/static/',
        filename: 'main.js'
    },
    module: {
        // 特定规则的文件与特定loader的映射
        loaders: [
            {
                test: /\.js$/,
                loader: 'babel',
                query: {
                    // since babel 6 needs preset to determin what to transform
                    presets: ['es2015']
                }
            }
        ]
    },

    resolve: {
        root: [__dirname + '/src']
    },

    devtool: '#source-map'
};
```

### 加个vue-loader
其实这个vue-loader我个人是觉得可有可无的，因为我认为组件的样式和模板是没必要放在一个文件的，至少在我的项目是这样。但是为了尝鲜还是决定用一下。
所需npm包如下：

```
    {
      "vue-loader": "^8.5.3",
      "vue-hot-reload-api": "^1.2.0",
      "vue-html-loader": "^1.2.3",
      "vue-style-loader": "^1.0.0"
  }
```

而webpack.config.js加上对应的loader，还有有一些不用babel转换的就剔除掉（exclude）：

```
loaders: [
    {
        test: /\.vue$/,
        loader: 'vue'
    },
    {
        test: /\.js$/,
        loader: 'babel',
        exclude: /node_modules|vue\/dist|vue-router\/|vue-loader\/|vue-hot-reload-api\//,
        query: {
            // since babel 6 needs preset to determin what to transform
            presets: ['es2015']
        }
    }
]
```

至此已经可以愉快地用es6写vue啦，可以参考这个例子：[点击跳转](https://github.com/plusice/vue-app/tree/v1.0)，下载下来安装完npm包后，执行“npm run dev”，再“npm run test”就可以看到效果啦



### 一个应用骨架
配置好基础的几个包，可以着手开始一个应用骨架的搭建了，我目前主要做是管理系统，就搭建一个后台管理系统的骨架吧。
搭建一个应用，我们还需要路由，通信模块，还有样式。路由不用说就是vue-router,通信使用jquery，考虑到jquery还可能有其他用途，样式就弄一个amaze UI，自己样式用sass编写。最后的webpack.config.js是这样子的：

```
var path = require('path');
module.exports = {
    entry: "./src/entry.js",
    output: {
        path: './static',
        publicPath: '/static/',
        filename: 'main.js'
    },
    module: {
        loaders: [
            {
                test: /\.vue$/,
                loader: 'vue'
            },
            {
                test: /\.scss$/,
                loaders: ["style", "css", "sass"] 
            },
            {
                test: /\.js$/,
                loader: 'babel',
                exclude: /node_modules|vue\/dist|vue-router\/|vue-loader\/|vue-hot-reload-api\//,
                query: {
                    // since babel 6 needs preset to determin what to transform
                    presets: ['es2015']
                }
            }
        ]
    },

    resolve: {
        root: [path.resolve(__dirname, "./src")]
    },

    devtool: '#source-map'
};
```

最终骨架效果如下：

![图片](/img/resource/amazeui.png)

代码在这里：[点击跳转](https://github.com/plusice/vue-app)，持续更新中...

最近添加了些组件：[demo](https://plusice.github.io/vue-app/)
