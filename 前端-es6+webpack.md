# 

## 01_ES6模块化规则

ES6模块化贵方是浏览器端与服务器端通用的模块化开发规范. 它的出现极大的降低了前端开发者的模块化学习成本. 开发者不需要在额外的学习AMD, CMD, CommonJS等的模块化规范

 **ES6模块化规范中定义:**

+ 每个js文件都是独立的模块
+ 导入其他模块成员使用import关键字
+ 向外共享模块成员使用export关键字

 

## 02_WebPack相关



### 2.1 传统前端VS现在

传统:

+ HTML+CSS+JS
+ 美化样式直接使用Bootstrap
+ 操纵DOM或者Ajax直接使用JQuery
+ 需要渲染模板结构, 直接使用art-template等的模板引擎

现在:

+ 模块化(JS的模块化, css的模块化, 其他资源的模块化)
+ 组件化(复用现有的UI结构, 样式, 行为)
+ 规范化(目录结构的划分, 编码规范化, 接口规范化, 文档规范化, Git分支管理)
+ 自动化(自动化构建, 自动部署, 自动化测试)

 

###  2.2 前端工程化

目前主流的前端工程化的解决方案:

+ webpack: https://www.webpackjs.com
+ parcel: https://zh.parceljs.org



### 2.3 webpack基本使用

 概念:webpack是前端项目工程化的具体解决方案

主要功能: 提供了友好的前端模块化开发支持, 以及代码压缩混淆,处理浏览器端js的兼容性, 性能优化等强大的功能

好处: 让程序员把工作的重心放到具体的功能实现上, 提高了前端开发效率和项目的可维护性

**注意:** 目前企业级的前端项目开发中, 绝大多数的项目都是基于webpack进行打包构建的

**在webpack中默认约定:**

1. 默认的打包入口文件是  src-> index.js
2. 默认的输出文件是路径是  dist->main.js
3. 当然也可以在webpack.config.js中修改默认的打包约定



**代码实例:**

1. 项目文件下输入`npm init `  自动生成`package.json`文件

2. 在项目中安装webpack

   终端中输入: 

   `npm install webpack webpack-cli -D`

3. 项目中配置webpack

   项目根目录中新建`webpack.config.js`

   初始化内容:

   ``` 
   module.exports = {
       mode: 'development'      // mode用来指定构建模式, 可选值有 development 和 production
   }
   ```

4. 在package.json的script节点下 , 新增dev脚本:

   ``` json
   ......
   "scripts": {
     "?dev-diy": "script 节点下的脚本, 可以通过npm run执行 例如: npm run dev-diy   注意json没有注释,此处使用?+key作为注释",
     "dev-diy": "webpack"
   },
   ......
   ```

5. 运行这个脚本`npm run dev-diy`   文件中自动生成一个`dist文件`

6. 实际项目结构

   ```
   .
   ├── dist
   │   └── main.js
   ├── package-lock.json
   ├── package.json
   ├── src
   │   ├── index.html
   │   └── index.js
   └── webpack.config.js
   ```

7. 各个文件内容

   ```
   ---------------------------------------index.html-------------------------------------------------
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta http-equiv="X-UA-Compatible" content="IE=edge">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Document</title>
       <script  src="../dist/main.js"></script>
   </head>
   <body>
   <ul>
       <li >这是第1个li</li>
       <li>这是第2个li</li>
       <li>这是第3个li</li>
       <li>这是第4个li</li>
       <li>这是第5个li</li>
       <li>这是第6个li</li>
       <li>这是第7个li</li>
       <li>这是第8个li</li>
       <li>这是第9个li</li>
   </ul>    
   </body>
   </html>
   ---------------------------------------index.js-------------------------------------------------
   // 1. 使用ES6模块化语法导入Jquery
   import $ from 'jquery'         // 使用$符号接收jquery
   // 2. 实现各行变色
   $(function (){
       $('li:odd').css('background-color', 'red')
       $('li:even').css('background-color', 'cyan')
   })
   ---------------------------------------pack.json-------------------------------------------------
   {
     "name": "vue1",
     "version": "1.0.0",
     "description": "",
     "main": "index.js",
     "scripts": {
       "?dev-diy": "script 节点下的脚本, 可以通过npm run执行 例如: npm run dev-diy   注意json没有注释,此处使用?+key作为注释",
       "dev-diy": "webpack"
     },
     "keywords": [],
     "author": "",
     "license": "ISC",
     "dependencies": {
       "jquery": "^3.6.0"
     },
     "devDependencies": {
       "webpack": "^5.5.1",
       "webpack-cli": "^4.2.0"
     }
   }
   ---------------------------------------webpack.config.js--------------------------------------------- 
   module.exports = {
       mode: 'development'      // mode用来指定构建模式, 可选值有 development 和 production
   }
   ```

   



### 2.4 mode的可选值

mode节点的可选值有两个:

+ development

  + 开发环境
  + 不会对打包生成的文件进行代码塔索和性能优化
  + 打包速度快, 适合在开发阶段使用

+ product

  + 生产环境
  + 会对打包生成的文件进行代码压缩和性能优化
  + 打包速度很慢, 仅适合在项目发布阶段使用

  



### 2.5 webpack中的插件

通过安装和配置webpack插件, 可以拓展webpack的能力, 从而让webpack用起来更方便, 最常用的有两个:

1. webpack-dev-server
   + 类似于node.js阶段用到的nodemon工具
   + 每当修改了源代码, webpack会自动进行项目的打包和构建
2. Html-webpack-plugin
   + webpack中的HTML插件(类似于 一个模板引擎插件)
   + 可以通过这个插件自定制index.html页面的内容



**webpack-dev-server使用:   (打包后的项目在内存中  目录是不可见的 所以需要指定首页)**

1. 运行命令, 即可以在项目中安装此插件

   `npm install webpack-dev-server -D`

2. 修改package.json中的script节点

   ```
     "scripts": {
       "?dev-diy": "script 节点下的脚本, 可以通过npm run执行 例如: npm run dev-diy   注意json没有注释,此处使用?+key作为注释",
       "dev-diy": "webpack server",
     },
   ```

   

3. 修改静态页面权限位置

   ``` js
   module.exports = {
       mode: 'development',      // mode用来指定构建模式, 可选值有 development 和 production
       devServer: {
           static: './src',
       }
   }
   ```

4. html中修改main的位置  因为允许和变异环境不一样

   ``` html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta http-equiv="X-UA-Compatible" content="IE=edge">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Document</title>
       <script  src="/main.js"></script>
   </head>
     ........
   ```

   



**Html-webpack-plugin使用:   (需要配合webpack-dev-server)**

1. 运行命令

   `npm install html-webpack-plugin`

2. 修改pack.json的内容

   ```
   const path = require('path');
   const HtmlPlugin = require('html-webpack-plugin')
   let htmlPlugin = new HtmlPlugin({        // 将项目的文件拷贝到内存中  即项目运行时的目录
       template: './src/index.html',
       filename: './index.html'
   })
   module.exports = {
       mode: 'development',      // mode用来指定构建模式, 可选值有 development 和 production
       entry: path.join(__dirname,'./src/index.js'),
       output: {
           path: path.join(__dirname, "./dist"),
           filename: "gjTest.js"
       },
       plugins: [htmlPlugin]
   }
   ```

3. 运行  `npm run dev-diy`



**devServer节点:**

基于上述Html-webpack-plugin可以对webpack-dev-server进行更多的配置

```
const path = require('path');
const HtmlPlugin = require('html-webpack-plugin')
let htmlPlugin = new HtmlPlugin({        // 将项目的文件拷贝到内存中  即项目运行时的目录
    template: './src/index.html',
    filename: './index.html'
})
module.exports = {
    mode: 'development',      // mode用来指定构建模式, 可选值有 development 和 production
    entry: path.join(__dirname,'./src/index.js'),
    output: {
        path: path.join(__dirname, "./dist"),
        filename: "gjTest.js"
    },
    plugins: [htmlPlugin],
    devServer: {
    	open: true, 									// 初次打包后自动打开浏览器
    	host: '127.0.0.1',             // 实施打包的主机地址
    	port: 80												//  端口
    }
}
```



### 2.6 webpack的loder

**概述:**

在webpack中, webpack默认只能打包处理以`.js`结尾的模块, 其他的非`.js`结尾的模块例如,css,html,jpg等, webpack则无法处理, 需要调用loader加载器才可以正常进行打包处理, 否则会报错

+ Css-loader:  
+ Less-loader:
+ Babel-loader:  打包处理webpack无法处理的高级js语法

**loader的调用过程:**

![image-20220103194053722](https://tva1.sinaimg.cn/large/008i3skNly1gy0pxcgfwbj31nk0rq0x9.jpg)



**打包处理css文件:**

步骤:

1. 运行`npm install style-loader css-loader -D` 安装css文件的loader

2. 在webpack.config.js的module->rules数组中, 添加loader规则

   ```
   module: {
   rules: [
   {test: /\.css$/, use: ['style-loader', 'css-loader']},
   ]
   }
   ```

3. 在idnex.js中导入css样式文件模块

   ```
   import './css/index.css'
   ```

4. 启动项目



**打包处理less文件:**

步骤:

1. 运行`npm install less-loader less -D` 安装lss文件的loader(需要先执行上面的css)

2. 在webpack.config.js的module->rules数组中, 添加loader规则

   ```
   module: {
   rules: [
   {test: /\.css$/, use: ['style-loader', 'css-loader']},
   {test: /\.less$/, use: ['style-loader', 'css-loader','less-loader']}
   ]
   }
   ```

3. 在idnex.js中导入css样式文件模块

   ```
   import './css/index.less'
   ```

4. 启动项目





**打包处理url相关文件(图片…):**

步骤:

1. 运行`npm install url-loader file-loader less -D` 安装lss文件的loader(需要先执行上面的css)

2. 在webpack.config.js的module->rules数组中, 添加loader规则

   ```
   module: {
   rules: [
   {test: /\.jpg|png|gif$/, use: 'url-loader?limit=2229'},          // 只有图片小于2229时候会自动转为base64
   ]
   }
   -----------------------------planB-----------------------
   module: {
   rules: [
   {   test: /\.jpg|png|gif$/, 
   		use: {
   			loader: 'url-loader',
   			options: {
   				limit: 2229            // 只有图片小于2229时候会自动转为base64
   			}
   		} 
   },         
   ]
   }
   ```

3. 启动项目





**打包处理js中的高级语法:**

实例:  例如`static`关键字等等

步骤:

1. 运行`npm install babel-loader @babel/core @babel/plugin-proposal-class-properties -D` 

2. 在webpack.config.js的module->rules数组中, 添加loader规则

   ```
   module: {
   rules: [
   {   test: /\.js$/,
   exclude: /node_modules/,        // exclude表示排除, babel只需要处理开发者的js不需要处理node_modules下的
   use: {
   loader: 'babel-loader',
   options: {          // 参数项
   //  指定处理babel的包
   plugins: ['@babel-/plugin-proposal-class-properties'],
   }
   }
   },
   ]
   }
   ```

3. 启动项目



### 2.7 打包发布

 为什么需要打包:

1. 开发过程中打包生成的文件是存放在内存中的,  无法获得打包生成的最终文件
2. 开发过程中打包生成的文件不会对代码进行压缩和性能优化



步骤:

1. 在package.json的script节点下:

   ```
     "scripts": {
       "build-package": " webpackage --mode production"
     },
   ```

2. 运行`npm run buile-package`, 会生成一次`dist` 目录



**出现的问题:**   `dist`目录下的文件比较杂乱  

修正:

1. 把js文件统一放置到js目录中:

   在webpackage.config.js配置文件的output节点中, 进行配置:

   ```js
   module.exports = {
       mode: 'development',      // mode用来指定构建模式, 可选值有 development 和 production
       entry: path.join(__dirname,'./src/index.js'),
       output: {
           path: path.join(__dirname, "./dist"),
           filename: "js/gjTest.js"         // 修改出口文件目录
       },
   ```

2. 其他文件 css, 图片等等  同理 在自己的module下新增outputPath

   ``` js
   module: {
     rules: [
       {
         test: /\.js$/,
         exclude: /node_modules/,        // exclude表示排除, babel只需要处理开发者的js不需要处理node_modules下的
         use: {
           loader: 'babel-loader',
           options: {          // 参数项
             //  指定处理babel的包
             plugins: ['@babel-/plugin-proposal-class-properties'],
           }
         }
       },
       {
         test: /\.css$/, use: {
           loader: ['style-loader', 'css-loader'],
           options: {
             outputPath: "css",
           }
         }
       },
       {
         test: /\.less$/, use: {
           loader: ['style-loader', 'css-loader', 'less-loader'],
           options: {
             outputPath: "less",
           }
         }
       },
     ]
   }
   ```

   



**出现的问题:**  每次打包不会自动清理之前打包的旧文件

修正:

1. 安装插件

   `npm install clean-webpack-plugin -D`

2. 按需导入插件, 得到插件的构造函数, 创建插件的实例对象

   ```
   const {CleanWebpackPlugin} = require('clean-webpack-plugin')
   const cleanPlugin = new CleanWeboackPlugin()
   ```

3. 把创建的cleanPlugin插件实例对象, 挂载到plugins节点中

   ```
   plugins: {htmlPlugin, cleanPluginx},    // 挂载插件
   ```

   



企业级项目的打包发布:

企业级项目在进行打包发布时, 远远比刚才的方式复杂的多. 主要的流程有:

1. 生成打包报告, 根据报告分析具体的优化方案
2. Tree-Shaking
3. 为第三方组件库启用CDN加载
4. 配置组件的按需加载
5. 开启路由的懒加载
6. 自定制首页内容





### 2.8 Source Map

前端项目在投入生产环境之前, 都需要对Js代码进行压缩混淆, 从而减小文件体积, 提高文件的加载效率, 此时就不可避免的产生了一个问题:

对压缩混淆之后的代码除错(Debug) 是一件极其困难的事情.

+ 变量被替换成没有任何语义的名称
+ 空行和注释被剔除

SourceMap就是一个信息文件, 里面储存这位置信息. 也就是说 SourceMap文件中存储着代码压缩混淆前后的对应关系

有了它, 出错的时候, 除错工具直接显示原始代码, 而不是转换后的代码, 方便后期排错



#### 开发环境的SourceMap

**开发环境中其实默认了一个SourceMap但是这个默认的工具会出现显示行数不一致的问题**

![image-20220105160104093](https://tva1.sinaimg.cn/large/008i3skNly1gy2ut8016xj31m00u0djh.jpg)



解决:

开发环境下, 建议在webpack.config.js下添加如下的配置, 就可以保证运行时报错的行数和源代码的行数保持一致

```js
module.exports = {
  mode: 'development' ,
  // eval-sourcepmap  仅限在"开发者模式"下使用, 不建议在"生产模式"下使用
  // 此选项生成的 SourceMap 能够保证"运行时报错的行数" 与 "源代码中的行数" 保持一致
  devtool: 'eval-source-map',
  ......
}
```



#### 生产环境的SourceMap

在生产环境下, 如果省略了dectool选项,  则最终生成的文件中不包含SourceMap. 这能够防止原始代码通过SourceMap的形式暴露给外人

虽然可以不暴露源码 但是也不利于我们调试

解决:

将devtool的值设置为`nosources-source-map`就可以只提示错误行数,且不暴露源码







































