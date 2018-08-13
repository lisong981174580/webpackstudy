## webpack学习

### 什么是webpack
> Webpack 是当下最热门的前端资源模块化管理和打包工具。它可以将许多松散的模块按照依赖和规则打包成符合生产环境部署的前端资源。还可以将按需加载的模块进行代码分隔，等到实际需要的时候再异步加载。通过 loader 的转换，任何形式的资源都可以视作模块，比如 CommonJs 模块、 AMD 模块、 ES6 模块、CSS、图片、 JSON、Coffeescript、 LESS 等。
![交互图](http://webpack.hnz.kim/amWiki/img/what-is-webpack2.png)
#### Webpack官网
> Webpack官网:https://doc.webpack-china.org/

> Webpack中文文档:https://doc.webpack-china.org/configuration/

#### Webpack 的特点
* 兼容 CommonJS 、 AMD 、ES6的语法
* 对js、css、图片等资源文件都支持打包
* 串联式模块加载器以及插件机制，让其具有更好的灵活性和扩展性，例如提供对CoffeeScript、ES6的支持
* 有独立的配置文件webpack.config.js
* 可以将代码切割成不同的chunk，实现按需加载，降低了初始化时间
* 支持 SourceUrls 和 SourceMaps，易于调试
* 具有强大的Plugin接口，大多是内部插件，使用起来比较灵活
* webpack 使用异步 IO 并具有多级缓存。这使得 webpack 很快且在增量编译上更加快
------------------------------------------------------------------------------------
### 使用
#### 安装
> Webpack基于Nodejs，要确保安装 Node.js，Node.js 自带了软件包管理器 npm，Webpack命令行工具可以通过npm去安装。最新的webpack版本是：4.16.5;
####　安装到全局
> 用 npm 安装 Webpack(v4)命令行工具与核心包：
```
# WebpackV4.x 安装
$ npm install webpack -g        # webpack核心包
$ npm install webpack-cli -g    # webpack命令行工具

```
此时 Webpack命令行工具已经安装到了全局环境下，可以通过命令行测试是否安装成功。

```
# WebpackV4.x
$ webpack-cli -h

```

#### 安装到本地
> 通常我们会将 Webpack 安装到项目的依赖中，这样就可以使用项目本地版本的 Webpack。
```
# 进入项目目录
# 确定已经有 package.json，没有就通过 npm init 创建
# 安装 webpack 依赖
$ npm install webpack --save-dev

```
参数作用： --save-dev开发时依赖 --save运行时依赖

#### 其他webpack
```
# 查看 webpack 版本信息
$ npm info webpack命令行工具

```
安装指定版本的 webpack

```
$ npm install webpack@4.0.x --save-dev

```

--save 和 --save-dev 的区别 npm install时使用 --save 和 --save-dev，可分别将依赖（插件）记录到package.json中的dependencies和devDependencies下面。

| 参数  | 描述  |  
| --- | --- | 
| dependencies  | 记录的是项目在运行时必须依赖的插件，常见的例如react jquery等，即及时项目打包好了、上线了，这些也是需要用的，否则程序无法正常执行。    |  
| devDependencies  |记录的是项目在开发过程中使用的插件，例如这里我们开发过程中需要使用webpack打包，但是一旦项目打包发布、上线了之后，webpack就都没有用了，可卸磨杀驴。   |

#### 常用命令
webpack的使用和browserify有些类似，下面列举几个常用命令：

| 命令  | 描述  |  
| --- | --- | 
| webpack-cli | 最基本的启动webpack命令    |  
| webpack-cli -w  |提供watch方法，实时进行打包更新   |
| webpack-cli -o  |输出指定位置的文件   |
| webpack-cli -d |	提供SourceMaps，方便调试   |
| webpack-cli --mode |生产模式和开发模式 "development", "production"  |
| webpack-cli --colors|输出结果带彩色，比如：会用红色显示耗时较长的步骤 |
| webpack-cli --profile|输出性能数据，可以看到每一步的耗时 |
| webpack-cli --profile|输出性能数据，可以看到每一步的耗时 |
| webpack-cli --config webpack.dev.config.js|使用另一份配置文件 |

> 注意：以上命令均为WebpackV4.x命令，V3.x 去掉-cli,mode为V4.x 新增参数。

#### Webpack使用方式
介绍两种Webpack使用方式：
* Webpack命令行使用
* 通过配置文件使用Webpack
##### Webpack命令行使用
> 首先创建一个静态页面 index.html 和一个 JS 入口文件 entry.js：

文件：index.html
```
<html>
<head>
  <meta charset="utf-8">
</head>
<body>
  <script src="dist/main.js"></script>
</body>
</html>
```
文件：entry.js
```
document.write('It works.')
```
然后在命令行输出如下命令，编译 entry.js 并打包,会自动生成一个dist文件夹，并输出编译后文件 main.js 
```
$ webpack-cli entry.js

```
打包过程会显示日志：
```
Hash: 259cde458cb62853c828
Version: webpack 4.0.1
Time: 498ms
Built at: 2018-3-2 14:17:47
  Asset       Size  Chunks             Chunk Names
main.js  577 bytes       0  [emitted]  main
Entrypoint main = main.js
   [0] ./entry.js 35 bytes {0} [built]

WARNING in configuration
The 'mode' option has not been set. Set 'mode' option to 'development' or 'production' to enable defaults for this environment.
```
用浏览器打开 index.html 将会看到 It works. 。

警告：这里警告是让我们设置一下 mode参数 可以修改为开发环境或生产环境。

```
# 设置当前环境为 开发环境
webpack entry.js --mode "development"  

# 设置当前环境为 生产环境
webpack entry.js --mode "production"

```

接下来添加一个模块 module.js 并修改入口 entry.js：

文件：module.js
```
module.exports = 'It works from module.js.'
```
文件：entry.js
```
document.write('It works.')
document.write(require('./module.js')) // 添加模块
```
重新打包
```
webpack-cli entry.js  --mode "development"

```

```
Hash: 168c21a0618a5c753bad
Version: webpack 4.0.1
Time: 118ms
Built at: 2018-3-2 14:21:48
  Asset      Size  Chunks             Chunk Names
main.js  3.16 KiB    main  [emitted]  main
Entrypoint main = main.js
[./entry.js] 73 bytes {main} [built]
[./module.js] 25 bytes {main} [built]

```
Webpack 会分析入口文件，解析包含依赖关系的各个文件。这些文件（模块）都打包到 main.js 。Webpack 会给每个模块分配一个唯一的 id 并通过这个 id 索引和访问模块。在页面启动时，会先执行 entry.js 中的代码，其它模块会在运行 require 的时候再执行。
### 通过配置文件使用Webpack
> Webpack 在执行的时候，除了在命令行传入参数，还可以通过指定的配置文件来执行。默认情况下，会搜索当前目录的 webpack.config.js 文件，这个文件是一个 node.js 模块，返回一个 json 格式的配置信息对象，或者通过 --config 选项来指定配置文件。
###### 示例
> 创建一个配置文件 webpack.config.js：
```
var webpack = require('webpack')

module.exports = {
  entry: './entry.js',
  output: {
    path: __dirname,
    filename: 'bundle.js'
  },
  module: {
    loaders: [
      {test: /\.css$/, loader: 'style!css'}
    ]
  }
}

```
> 注：__dirname是node.js中的一个全局变量，它指向当前执行脚本所在的目录。

在项目目录下使用命令行执行如下命令即可打包：
```
$ webpack-cli
```












