---
title: webpack 入门
date: 2016-10-30 16:07:32
tags: webpack 
---

#### 1. webpack 基本配置

#### 1.1 setup
```
 npm init
 npm install webpack --save-dev # 全局安装依赖
# or
 npm install webpack-dev-server --save-dev # 安装webpack调试工具

```
#### 1.2 config
```
const webpack = require('webpack');
const path = require('path');

// 配置目录
// 因为我们的webpack.config.js文件不在项目根目录下，所以需要一个路径的配置
const path = require('path');
const CURRENT_PATH = path.resolve(__dirname); // 获取到当前目录
const ROOT_PATH = path.join(__dirname, '../'); // 项目根目录
const MODULES_PATH = path.join(ROOT_PATH, './node_modules'); // node包目录
const BUILD_PATH = path.join(ROOT_PATH, './public/assets'); // 最后输出放置公共资源的目录

module.exports = {
  devtool: 'eval-source-map',//这里配置方便调试时使用的source map  如何生成相应格式的source map
  context: CURRENT_PATH, // 设置webpack配置中指向的默认目录为项目根目录
  entry: {
    index: './public/pages/index.js',
    public: './public/pages/public.js'
  },
  output: {
    path: BUILD_PATH, // 设置输出目录
    filename: '[name].bundle.js', // 输出文件名
  },
  resolve: {
    extensions: ['', '.js', '.jsx', '.coffee'] // 配置简写，配置过后，书写该文件路径的时候可以省略文件后缀
//模块别名定义，方便后续直接引用别名，无须多写长长的地址
// alias: {
//     AppStore : 'js/stores/AppStores.js',//后续直接 require('AppStore') 即可
//     ActionType : 'js/actions/ActionType.js',
//     AppAction : 'js/actions/AppAction.js'
// }
  },
  module: {
    loaders: [
      // loader 扔在这里
    ]
  },
  plugins: [
    // 插件扔在这里
  ],
    // 在package中 配置启动服务"dev": "webpack-dev-server --config ./webpack.config --hot --colors --inline --history-api-fallback "
    devServer: {
        // contentBase: "./",//本地服务器所加载的页面所在的目录
        // colors: true,//终端中输出结果为彩色
        // historyApiFallback: true,//不跳转
        // inline: true//实时刷新
    }
}
```
#### 2. webpack中的loaders

>loaders被应用于应用程序的资源文件中，通常用来做转换。它们都是函数（运行在nodejs中），将资源文件的源码作为入参，处理完后，返回新的源码文件。

可以使用`install css/less/style loader .....`处理各种类型的文件

处理样式文件

```
npm install less --save-dev # install less
npm install css-loader style-loader --save-dev # install style-loader, css-loader
npm install less less-loader --save-dev # 基于style-loader,css-loader
```
用来处理图片和字体文件

```
npm install file-loader --save-dev
npm install url-loader --save-dev
```
es6 install babel loader


`npm install babel-loader babel-core babel-preset-es2015 --save-dev`
 

#### 2.1 config loaders

```
module.exports = {
  module: {
    loaders: [
      // style & css & less loader
      { test: /\.css$/, loader: "style-loader!css-loader"},
      { test: /\.less$/, loader: "style-loader!css-loader!less-loader")},
      // babel loader
      {
        test: /\.jsx?$/,
        exclude: /(node_modules|bower_components)/,
        loader: ['babel-loader'],
        query: {
          presets: ['es2015']
          // 如果安装了React的话
          // presets: ['react', 'es2015']
        }
      },
      // image & font
      { test: /\.(woff|woff2|eot|ttf|otf)$/i, loader: 'url-loader?limit=8192&name=[name].[ext]'},
      { test: /\.(jpe?g|png|gif|svg)$/i, loader: 'url-loader?limit=8192&name=[name].[ext]'},
    ]
  }
}
```

#### webpack 中的plugin

#### ExtractTextPlugin分离CSS

```
const ExtractTextPlugin = require("extract-text-webpack-plugin");
plugins: [
  // 分离css
  new ExtractTextPlugin('[name].bundle.css', {
    allChunks: true
  }),
]
```
#### html-webpack-plugin 

这个插件可以帮助生成 HTML 文件，在 body 元素中，使用 script 来包含所有你的 webpack bundles，只需要在你的 webpack 配置文件中如下配置：

```
var HtmlWebpackPlugin = require('html-webpack-plugin')

 plugins: [
    new HtmlWebpackPlugin({
      title: 'My App',
      filename: 'assets/index.html'
    })
 ]
```
这将会自动在 dist 目录中生成一个名为 index.html 的文件，内容如下：

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>My App</title>
  </head>
  <body>
    <script src="index_bundle.js"></script> 
  </body>
</html>
```


