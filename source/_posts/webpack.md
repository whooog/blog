---
title: webpack
date: 2019-11-25 14:15:36
tags:

categories: 
  - webpack
---
**webpack 用途**

**webpack 打包步骤**

- 创建项目文件 mkdir hello-webpack 

- npm init 生成 package.json文件  (注意要转到项目文件路径之后再npm init)

- npm install --save-dev webpack  
  安装一些项目依赖的包到node_modules文件夹内

- 创建一个静态页（index.html）及入口文件（app.js）

- webpack ./src/app.js -o ./dist/index.js   
  以 src/app.js 作为源文件 把转换后的结果放到dist/index.js中

- 创建webpack.config.js 文件

```javscript

module.exports = {
  entry: './src/app.js',       // entry 表示源文件
  output: {
    filename: './dist/app.bundle.js'    //filename 输出的目标文件
  }
};

```
- npm install html-webpack-plugin --save-dev  运行 npm run dev 生成 index.html文件

```javascript

var HtmlWebpackPlugin = require('html-webpack-plugin');
/**
* HtmlwebpackPlugin 插件的作用
* 1:为html文件中引入的外部资源如script,link动态添加每次compile后的hash,
*   防止引用缓存的外部文件问题
* 2:可以生成创建html入口文件，比如单页面可以生成一个html文件入口，
*   配置 N 个html-webpack-plugin可以生成N个页面入口
*/
 

module.exports = {
    entry: './src/app.js',     // entry 表示源文件
    output: {
        path: __dirname + "/dist",     //打包后的文件存放地方  _dirname 是node.js的一个全局变量，指向当前执行脚本所在的目录
        filename: "./dist/index.js"    //filename 输出的目标文件
    },
    plugins: [new HtmlWebpackPlugin({
        title: "hello webpack",     // 改变 生成的页面 index.html 的<title>标签内容
        template: "index.html",     // 根据根目录下的index.html 生成 的 index.html
        filename: 'index.html',     // 生成的页面名，默认为 index.html
        
        minify: { // 压缩HTML文件
            removeComments: true, // 移除HTML中的注释
            collapseWhitespace: true, // 删除空白符与换行符
            minifyCSS: true// 压缩内联css
        },
        hash: true,
    })]
}

```
- npm install --save-dev css-loader style-loader 
    配置css转换成 webpack 可以解析  在webpack.config.js 中添加 module 对象 
    
- npm install sass-loader node-sass --save-dev
   把sass文件转换为css文件
 


```javascript

var HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    entry: './src/app.js',     // entry 表示源文件
    output: {
        path: __dirname + "/dist",      //打包后的文件存放地方 _dirname 是node.js的一个全局变量，指向当前执行脚本所在的目录
        filename: "./dist/index.js"    //filename 输出的目标文件
    },
    resolveLoader: {
        alias: {
            'scss-loader': 'sass-loader',   
            //由于scss是由sass-loader编译的，加入这段让scss-loader指向sass-loader
        },
    },
    plugins: [new HtmlWebpackPlugin({
        title: "hello webpack",     // 改变 生成的页面 index.html 的<title>标签内容
        template: "index.html",     // 根据根目录下的index.html 生成 的 index.html
        filename: 'index.html',     // 生成的页面名，默认为 index.html

        minify: { // 压缩HTML文件
            removeComments: true, // 移除HTML中的注释
            collapseWhitespace: true, // 删除空白符与换行符
            minifyCSS: true// 压缩内联css
        },

        hash: true,
    })],
    module: {
        rules: [
            {
                test: /\.scss$/,
                use: [ 'style-loader', 'css-loader', 'scss-loader' ]
            }

        ]
    }
}


```

-  npm install -g webpack-dev-server
   npm install --save-dev webpack-dev-server 
   用来快速搭建本地运行环境的工具，使用webpack-dev-server 来开启服务
  
```javascript
    //webpack.confing.js 添加一下配置
    devServer: {
        port: 9000,             // 端口号
        open: true              // 运行 webpack-dev-server 时自动打开浏览器
    }
```

- npm install --save-dev extract-text-webpack-plugin 
  抽离css样式,防止将样式打包在js中引起页面样式加载错乱的现象

**使用webpack打包遇到的问题**

问题一： npm install --save-dev webpack

![](/images/webpack1.png)

解决方法：

    删除 node_modules 文件 跟 package-lock.json 文件重新install 再输入命令即可
    
问题二：webpack ./src/app.js -o ./dist/index.js

![](/images/webpack2.png)

解决方法： 
    
    npm install --save-dev webpack
    npm install --save-dev webpack-cli
    因为之前我只全局安装了webpack 和 webpack-cli 
    
问题三：npm install sass-loader node-sass --save-dev 编译scss保存

![](images/webpack3.png)

解决方法
```javascript

resolveLoader: {
    alias: {
        'scss-loader': 'sass-loader',
    },
}
// 由于scss是用sass-loader编译的，加入这段代码让scss-loader指向sass-loader

```

问题四：npm安装模块遇到ENOENT: no such file or directory, rename错误

![](/images/webpack4.png)
   
    
