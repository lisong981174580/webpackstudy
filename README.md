## webpack学习

### 一、什么是webpack
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
### 二、使用
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

---------------------------------------------------------------------------------------------
### 三、Webpack配置文件详解

Webpack 在执行的时候，除了在命令行传入参数，还可以通过指定的配置文件来执行。默认情况下，会搜索当前目录的 webpack.config.js 文件，这个文件是一个 node.js 模块，返回一个 json 格式的配置信息对象，或者通过 --config 选项来指定配置文件。

把一个配置对象给webpack ，它就可以干活儿了。根据你用webpack的用法，有两种途径传入这个对象：

#### CLI( 命令行)

如果你用命令行，命令行会读取一个叫webpack.config.js（或者用 –config 选项传入的一个配置文件）的文件。这个文件应该导出一个配置对象，如下：
```
module.exports = {
    // configuration
};

```

#### node.js API
如果 你用node.js API，你需要把配置文件作为一个参数，传给webpack。如下：

```
webpack({
    // configuration
}, callback);

```
### Webpack四个核心概念

从 webpack v4.0.0 开始，可以不用引入一个配置文件。然而，webpack 仍然还是高度可配置的。在开始前你需要先理解四个核心概念：

* 入口(entry) 告诉webpack 应该使用哪个模块，来作为构建其内部依赖图的开始。进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。
* 输出(output) 告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件，默认值为 ./dist。
* loader Webpack 本身只能处理 JavaScript 模块，如果要处理其他类型的文件，就需要使用 loader 进行转换。
* 插件(plugins) 是用来拓展Webpack功能的，插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。插件接口功能极其强大，可以用来处理各种各样的任务。

### 配置文件格式

webpack.config.js 就是一个普通的 js 文件，符合 commonJS 规范。最后输出一个对象，即:

```
module.exports = {
}

```
### 配置对象的参数

在导出的配置对象中有几个关键的参数：

#### 模式 mode

通过选择 development 或 production 之中的一个，来设置 mode 参数，你可以启用相应模式下的 webpack 内置的优化。

##### 使用

只在配置中提供 mode 选项：
```
module.exports = {
  mode: 'production'
};

```
或者从 CLI 参数中传递：
```
webpack --mode=production

```
##### mode说明

mode: development
```
// webpack.development.config.js
module.exports = {
+ mode: 'development'
- plugins: [
-   new webpack.NamedModulesPlugin(),
-   new webpack.DefinePlugin({ "process.env.NODE_ENV": JSON.stringify("development") }),
- ]
}

```
mode: production

```
// webpack.production.config.js
module.exports = {
+  mode: 'production',
-  plugins: [
-    new UglifyJsPlugin(/* ... */), //css压缩
-    new webpack.DefinePlugin({ "process.env.NODE_ENV": JSON.stringify("production") }), //设置当前环境
-    new webpack.optimize.ModuleConcatenationPlugin(),
-    new webpack.NoEmitOnErrorsPlugin()
-  ]
}

```
### 入口 entry

entry参数定义了打包后的入口文件，有三种写法，每个入口称为一个chunk:

| 类型  | 示例  |  示例|
| --- | --- | --- | 
| 字符串  | entry: "./index/index.js"  |  配置模块会被解析为模块，并在启动时加载。chunk名为默认为main， 具体打包文件名视output配置而定。 默认值是 ./src|
| 数组  | entry: ['./src/mod1.js', [...,] './src/index.js']  | 所有的模块会在启动时 按照配置顺序加载，合并到最后一个模块会被导出。chunk名默认为main|
| 对象  | entry:{index: '...', login : [...]} | 如果传入Object,则会生成多个入口打包文件key是chunk名，value可以是字符串，也可是数组。|

注意：webpack4中它会默认定义./src/index.js为入口文件。

#### entry参数为字符串
```
var path = require('path');
module.exports = {
    entry: './src/index.js',
    output: {
        path:path.resolve(__dirname,"dist/js/page") ,
        publicPath: "/output/",
        filename: "[name].bundle.js"
    }
}

```
#### entry参数为数组
```
var path = require('path');
module.exports = {
    entry: [
        'index1.js',
        'index2.js',
        .....
    ],
    output: {
        path:path.resolve(__dirname,"dist/js/page") ,
        publicPath: "/output/",
        filename: "[name].bundle.js"
    }
}

```
#### entry参数为对象
```
var path = require('path');
module.exports = {
    entry: {
        //支持字符串形式
        page1: "./page1",

        //支持数组形式，将加载数组中的所有模块，但以最后一个模块作为输出
        page2: ["./entry1", "./entry2"]
    },
    output: {
        path:path.resolve(__dirname,"dist/js/page") ,
        publicPath: "/output/",
        filename: "[name].bundle.js"
    }
}

```

该段代码最终会生成一个 page1.bundle.js 和 page2.bundle.js，并存放到 ./dist/js/page 文件夹下

### 输出 output

output参数是个对象，定义了输出文件的位置及名字：
```
var path = require('path');
output: {
        path: path.resolve(__dirname,"dist/js/page"),
        publicPath: "/output/",
        filename: "[name].bundle.js"
    }
    
 ```
 | 参数  | 描述  |
| --- | --- | --- | 
| path  | 打包文件存放的绝对路径,默认值 ./dist  |  
| publicPath  |用于在生产模式下更新内嵌到css、html文件里的url值  |
| filename  |打包后的文件名|

> 注意：“path”仅仅告诉Webpack结果存储在哪里，然而“publicPath”项则被许多Webpack的插件用于在生产模式下更新内嵌到css、html文件里的url值。
例如： publicPath:'http://hnz.com',图片在css中引用路径./logo.png 生成后为 http://hnz.com/logo.png

> 注意：当我们在entry中定义构建多个文件时，filename可以对应的更改为[name].js用于定义不同文件构建后的名字。

> 注意：webpack4中它会默认定义./dist/main.js为出口文件。

### 模块 module
#### Loader介绍
> Webpack 本身只能处理 JavaScript 模块，如果要处理其他类型的文件，就需要使用 loader 进行转换。

 | type  | url  |
| --- | --- | --- | 
| Loaders  | https://doc.webpack-china.org/loaders/ |  

Loader 可以理解为是模块和资源的转换器，它本身是一个函数，接受源文件作为参数，返回转换的结果。这样，我们就可以通过 require 来加载任何类型的模块或文件，比如 CoffeeScript、 JSX、 LESS 或图片。

loader 一般以 xxx-loader 的方式命名，xxx 代表了这个 loader 要做的转换功能，比如 json-loader。

Loaders需要单独安装并且需要在webpack.config.js下的modules关键字下进行配置，Loaders的配置选项包括以下几方面：

 | 参数  | 描述  |
| --- | --- |
| test  | 一个匹配loaders所处理的文件的拓展名的正则表达式（必须） |  
| loader |loader的名称（必须） |  
| include/exclude	 |手动添加必须处理的文件（文件夹）或屏蔽不需要处理的文件（文件夹）（可选） |  
| query |为loaders提供额外的设置选项（可选） |  

#### 常用loader

 | 名称  | 描述  |
| --- | --- |
| css-loader  | 处理css中路径引用等问题 |  
| style-loader | 动态把样式写入css |  
| sass-loader |scss编译器 |  
| less-loader |less编译器 |  
| postcss-loader |为CSS代码自动添加适应不同浏览器的CSS前缀 |  
| babel-loader |编译下一代JavaScript标准、编译JavaScript扩展(JSX) |  
| url-loader | 主要用来处理图片 |  
| file-loader | 文件处理 |  

### CSS与CSS预处理处理
#### CSS处理
> css-loader 处理css中路径引用等问题 style-loader 动态把样式写入css
```
npm install css-loader style-loader --save-dev

```
配置示例：
```
module: {
        //加载器配置
        rules:[
            //.css 文件使用 style-loader 、 css-loader和postcss-loader来处理
            { test: /\.css$/, loader: 'style-loader!css-loader!postcss-loader' },
        ]
}

```
#### sass预处理
sass-loader scss编译器
```
npm install sass sass-loader  --save-dev
```
配置示例：
```
module: {
        //加载器配置
        rules:[
            //.scss 文件使用 style-loader、css-loader、postcss-loader和 sass-loader 来编译处理
            { test: /\.scss$/, loader: 'style-loader!css-loader!sass-loader!postcss-loader'}
        ]
}
```
#### less预处理
less-loader less编译器
```
npm install less less-loader  --save-dev
```
配置示例：

```
module: {
        //加载器配置
        rules:[
            //.less 文件使用 less-loader、cssloader、postcss-loader和style-loader来编译
            { test:/\.less$/, loader: 'style-loader!css-loader!less-loader!postcss-loader'}
        ]
}

```
#### 为CSS3代码自动添加适应不同浏览器的CSS前缀

postcss-loader 为CSS3代码自动添加适应不同浏览器的CSS前缀
```
npm install postcss-loader autoprefixer  --save-dev
```
使用postcss-loader需要在项目根目录创建postcss.config.js的配置文件：
```
# postcss.config.js
module.exports = {
    plugins: [
        require('autoprefixer')
    ]
}
```

#### JavaScript处理
babel-loader babel官网

> 安装
```
// npm一次性安装多个依赖模块，模块之间用空格隔开
npm install babel-core babel-loader babel-preset-es2015 babel-preset-react --save-dev
```
> 使用
```
module: {
  rules: [
    {
        test:/\.jsx?$/,
        exclude:/node_modules/,
        loader:'babel-loader'
    }
  ]
}

```
#### 图片处理
url-loader
> 安装
```
npm install --save-dev url-loadr file-loader
```
> 使用
```
module: {
  rules: [
    {
      test: /\.(png|jpg|gif|jpeg|bmp)$/,
      loader: 'url-loader?limit=10000&name=images/[name].[ext]'
    }
  ]
}

```
对于上面的配置，如果图片资源小于10kb就会转化成 base64 格式的 dataUrl，其他的图片会存放在(相对于output参数的path路径)build/images文件夹下。

#### 压缩图片
image-webpack-loader是用来压缩图片的一个插件。
> 安装
```
npm install image-webpack-loader --save-dev
```
> 使用
```
module: {
  rules: [
    {
      test: /\.(png|jpg|gif|jpeg|bmp)$/,
      loaders: [
        'url-loader?limit=10000&name=build/images/[name].[ext]',
        'image-webpack-loader'
        ]
    }
  ]
}
```
> 使用参数
```
module: {
  rules: [
    {
      test: /\.(png|jpg|gif|jpeg|bmp)$/,
      loaders: [
        'url-loader?limit=10000&name=build/images/[name].[ext]',
        'image-webpack-loader?{progressive:true, optimizationLevel: 7, interlaced: false, pngquant:{quality: "65-90", speed: 4}}'
        ]
    }
  ]
}
```

> 错误
> 在Mac os版本中，可能会导致缺少libpng依赖性的错误:

> Module build failed: Error: dyld: Library not loaded: /usr/local/opt/libpng/lib/libpng16.16.dylib
可以通过 homebrew 安装最新版本的libpng来解决:

> brew install libpng

#### 文件处理
file-loader
> 安装
```
npm install --save-dev file-loader
```
> 使用

```
module: {
    rules: [
        {
            test:/\.(woff|woff2|svg|ttf|eot)($|\?)/i,
            loader:'url-loader?limit=500&name=fonts/[name].[ext]'
        }
    ]
}
```





























