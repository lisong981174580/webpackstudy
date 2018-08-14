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
-----------------------------------------------------------------------------
### 四、插件
> webpack 有着丰富的插件接口(rich plugin interface)。webpack 自身的多数功能都使用这个插件接口。这个插件接口使 webpack 变得极其灵活。

#### 插件语法
```
module.exports = {
    plugins:[

    ]
}
```
#### 插件分类
##### Webpack官方提供
Webpack官方提供的插件，一般需要我们在本地项目中安装webpack:
```
npm install webpack --save-dev

# webpack.config.js
var webpack = require('webpack');
module.exports = {
    plugins: [
        new webpack.BannerPlugin("Copyright Nico inc.")
    ]
}
```
#### 第三方插件

第三方的插件，需要我们按照插件的要求进行安装与使用：

```
# 安装
npm install --save-dev html-webpack-plugin

# webpack.config.js
module.exports = {
    new HtmlWebpackPlugin({
        template: __dirname + "/app/index.tmpl.html"
    })
}

```

#### 插件列表


 | Name  | Description  |
| --- | --- |
| AggressiveSplittingPlugin  | 将原来的 chunk 分成更小的 chunk |  
| BabelMinifyWebpackPlugin |使用 babel-minify进行压缩 |  
| BannerPlugin	 |在每个生成的 chunk 顶部添加 banner |  
| CommonsChunkPlugin|提取 chunks 之间共享的通用模块 |  
| CompressionWebpackPlugin |预先准备的资源压缩版本，使用 Content-Encoding 提供访问服务 |  
| ContextReplacementPlugin |重写 require 表达式的推断上下文 |  
| CopyWebpackPlugin |将单个文件或整个目录复制到构建目录 |  
| DefinePlugin |允许在编译时(compile time)配置的全局常量 |  
| DllPlugin |为了极大减少构建时间，进行分离打包 |  
| EnvironmentPlugin |DefinePlugin 中 process.env 键的简写方式。|  
| ExtractTextWebpackPlugin |从 bundle 中提取文本（CSS）到单独的文件|  
| HotModuleReplacementPlugin |启用模块热替换(Enable Hot Module Replacement - HMR)|  
| HtmlWebpackPlugin |简单创建 HTML 文件，用于服务器访问|  
| I18nWebpackPlugin |为 bundle 增加国际化支持|  
| IgnorePlugin |从 bundle 中排除某些模块|  
| LimitChunkCountPlugin |设置 chunk 的最小/最大限制，以微调和控制 chunk|  
| LoaderOptionsPlugin |用于从 webpack 1 迁移到 webpack 2|  
| MinChunkSizePlugin |确保 chunk 大小超过指定限制|  
| NoEmitOnErrorsPlugin |在输出阶段时，遇到编译错误跳过|  
| NormalModuleReplacementPlugin |替换与正则表达式匹配的资源|  
| NpmInstallWebpackPlugin |在开发时自动安装缺少的依赖|  
| ProvidePlugin |在开发时自动安装缺少的依赖|  
| UglifyjsWebpackPlugin |可以控制项目中 UglifyJS 的版本|  
| ZopfliWebpackPlugin |通过 node-zopfli 将资源预先压缩的版本|  

----------------------------------------------------------------------------------------------------------------
### 五、使用webpack构建本地服务器
> 想不想让你的浏览器监测你都代码的修改，并自动刷新修改后的结果，其实Webpack提供一个可选的本地开发服务器，这个本地服务器基于node.js构建，可以实现你想要的这些功能，不过它是一个单独的组件，在webpack中进行配置之前需要单独安装它作为项目依赖
#### 安装
```
npm install --save-dev webpack-dev-server
```
#### 调用
```
webpack-dev-server --progress --colors
```
#### 配置
devserver作为webpack配置选项中的一项，具有以下配置选项

 | devserver配置选项  | 功能描述  |
| --- | --- |
| contentBase  | 默认webpack-dev-server会为根文件夹提供本地服务器，如果想为另外一个目录下的文件提供本地服务器，
应该在这里设置其所在目录（本例设置到“public"目录） |  
| port |设置默认监听端口，如果省略，默认为”8080“ |  
| inline	 |设置为true，当源文件改变时会自动刷新页面 |  
| colors|设置为true，使终端输出的文件为彩色的 ,只能用于CLI |  
| historyApiFallback |	在开发单页应用时非常有用，它依赖于HTML5 history API，
如果设置为true，所有的跳转将指向index.html |  
| proxy |如果你有单独的后端开发服务器 API，并且希望在同域名下发送 API 请求 ，那么代理某些 URL 会很有用。 |  

 
 如下配置：
 ```
 module.exports = {
    ...
    devServer: {
        contentBase: "./public",//本地服务器所加载的页面所在的目录
        colors: true,//终端中输出结果为彩色
        historyApiFallback: true,//不跳转
        inline: true//实时刷新
    }
}

```
#### proxy
如果你有单独的后端开发服务器 API，并且希望在同域名下发送 API 请求 ，那么代理某些 URL 会很有用。
##### 简单使用

在 localhost:3000 上有后端服务的话，你可以这样启用代理：

```
proxy: {
  "/api": "http://localhost:3000"
}
```
请求到 /api/users 现在会被代理到请求 http://localhost:3000/api/users。

##### 重写路径
如果你不想始终传递 /api ，则需要重写路径：
```
proxy: {
  "/api": {
    target: "http://localhost:3000",
    pathRewrite: {"^/api" : ""}
  }
}
```
----------------------------------------------------------------------------------------------------------
### 六、Webpack插件
#### webpack.BannerPlugin
为每个 chunk 文件头部添加 banner。

##### 参考
> https://www.webpackjs.com/plugins/banner-plugin/
##### 使用
```
const webpack = require("webpack");

plugins:[
    new webpack.BannerPlugin(banner)
    // or
    new webpack.BannerPlugin(options)
]
```
##### 参数(options)
```
{
  banner: string, // 其值为字符串，将作为注释存在
  raw: boolean, // 如果值为 true，将直出，不会被作为注释
  entryOnly: boolean, // 如果值为 true，将只在入口 chunks 文件中添加
  test: string | RegExp | Array,
  include: string | RegExp | Array,
  exclude: string | RegExp | Array,
}
```
##### 占位符(Placeholders)
从 webpack 2.5.0 开始，会对 banner 字符串中的占位符取值：
```
new webpack.BannerPlugin({
  banner: "hash:[hash], chunkhash:[chunkhash], name:[name], filebase:[filebase], query:[query], file:[file]"
})

```
##### 示例
```
var webpack = require('webpack');
module.exports = {
  plugins: [
    new webpack.BannerPlugin("Copyright Nico inc.")
  ]
}

```
-----------------------------------------------------------------------------------
#### webpack.ProvidePlugin
自动加载模块，而不必到处 import 或 require 。

当我们经常使用React、jQuery等外部第三方库的时候，通常在每个业务逻辑JS中都会遇到这些库。

如我们需要在各个文件中都是有jQuery的$对象，因此我们需要在每个用到jQuery的JS文件的头部通过require('jquery')来依赖jQuery。 这样做非常繁琐且重复，因此webpack提供了我们一种比较高效的方法，我们可以通过在配置文件中配置使用到的变量名，那么webpack会自动分析，并且在编译时帮我们完成这些依赖的引入。

##### 参考
https://www.webpackjs.com/plugins/provide-plugin/

##### 使用
```
new webpack.ProvidePlugin({
  identifier: 'module1',
  // ...
})

new webpack.ProvidePlugin({
  identifier: ['module1', 'property1'],
  // ...
})
```
任何时候，当 identifier 被当作未赋值的变量时，module 就会自动被加载，并且 identifier 会被这个 module 输出的内容所赋值。（模块的 property 用于支持命名导出(named export)）

> 对于 ES2015 模块的 default export，你必须指定模块的 default 属性。

##### 示例
```
var webpack = require('webpack');

plugins: [
   new webpack.ProvidePlugin({
       'Moment': 'moment',
       "$": "jquery",
       "jQuery": "jquery",
       "React": "react",
       "ReactDOM":"react-dom"
   })
]

```

---------------------------------------------------------------------------------------------
#### optimization.splitChunks
项目中，对于一些常用的组件，站点公用模块经常需要与其他逻辑分开，然后合并到同一个文件，以便于长时间的缓存。

webpack 4 移除 CommonsChunkPlugin，取而代之的是两个新的配置项（optimization.splitChunks 和 optimization.runtimeChunk），下面介绍一下用法和机制。
##### 参考
https://www.webpackjs.com/plugins/split-chunks-plugin/

##### 介绍
> webpack模式模式现在已经做了一些通用性优化，适用于多数使用者。

##### 默认用法

需要注意的是：默认模式只影响按需(on-demand)加载的代码块(chunk)，因为改变初始代码块会影响声明在HTML的script标签。如果可以处理好这些（比如，从打包状态里面读取并动态生成script标签到HTML），你可以通过设置optimization.splitChunks.chunks: "all"，应用这些优化模式到初始代码块(initial chunk)。

webpack根据下述条件自动进行代码块分割：

*　新代码块可以被共享引用，OR这些模块都是来自node_modules文件夹里面
×　新代码块大于30kb（min+gziped之前的体积）
×　按需加载的代码块，最大数量应该小于或者等于5
×　初始加载的代码块，最大数量应该小于或等于3

为了满足后面两个条件，webpack有可能受限于包的最大数量值，生成的代码体积往上增加。
```
module.exports = {
    entry:'',
    output:{},
    module:{},
    optimization:{
        splitChunks:{
            chunks:"all"
        }
    }        
}

```

##### 选项
```
optimization:{
    splitChunks: {
        chunks: "initial",         // 必须三选一： "initial" | "all"(默认就是all) | "async"
        minSize: 0,                // 最小尺寸，默认0
        minChunks: 1,              // 最小 chunk ，默认1
        maxAsyncRequests: 1,       // 最大异步请求数， 默认1
        maxInitialRequests: 1,     // 最大初始化请求书，默认1
        name: () => {},            // 名称，此选项课接收 function
        cacheGroups: {             // 这里开始设置缓存的 chunks
            priority: "0",             // 缓存组优先级 false | object |
            vendor: {                  // key 为entry中定义的 入口名称
                chunks: "initial",         // 必须三选一： "initial" | "all" | "async"(默认就是异步)
                test: /react|lodash/,      // 正则规则验证，如果符合就提取 chunk
                name: "vendor",            // 要缓存的 分隔出来的 chunk 名称
                minSize: 0,
                minChunks: 1,
                enforce: true,
                maxAsyncRequests: 1,       // 最大异步请求数， 默认1
                maxInitialRequests: 1,     // 最大初始化请求书，默认1
                reuseExistingChunk: true   // 可设置是否重用该chunk（查看源码没有发现默认值）
            }
        }
    }
}
```
-------------------------------------------------------------------------------------

### html-webpack-plugin
html-webpack-plugin可以根据你设置的html模板，在每次运行后生成对应的模板文件，同时所依赖的CSS/JS也都会被引入，如果CSS/JS中含有hash值，则html-webpack-plugin生成的模板文件也会引入正确版本的CSS/JS文件。

 | 插件名称  | 地址  |
| --- | --- |
| html-webpack-plugin  | https://www.npmjs.com/package/html-webpack-plugin |  

#### 介绍
https://www.webpackjs.com/plugins/split-chunks-plugin/

#####  安装（Install）
```
npm install --save-dev html-webpack-plugin
```
##### 引入
在webpack.config.js中引入：
```
const HtmlWebpackPlugin = require('html-webpack-plugin');
```
##### 完整配置
你可以传一个配置选项到 HtmlWebpackPlugin，允许的值如下：

 | 参数	  | 描述  |
| --- | --- |
| title  | 用于生成的HTML文件的标题 |  
| filename  | 用于生成的HTML文件的名称，默认是index.html。你可以在这里指定子目录（例如:assets/admin.html） |  
| template  | 模板的路径。支持加载器，例如 html!./index.html |  
| inject  | rue \ ‘head’ \ ‘body’ \ false 。把所有产出文件注入到给定的 template 或templateContent。当传入 true或者 ‘body’时所有javascript资源将被放置在body元素的底部，“head”则会放在head元素内 |  
| favicon  | 给定的图标路径，可将其添加到输出html中 |  
| minify  | {…} \ false 。传一个html-minifier 配置object来压缩输出 |  
| hash  | true \ false。如果是true，会给所有包含的script和css添加一个唯一的webpack编译hash值。这对于缓存清除非常有用 |  
| cache  | rue \ false 。如果传入true（默认），只有在文件变化时才 发送（emit）文件 |  
| showErrors  | true \ false 。如果传入true（默认），错误信息将写入html页面 |  
| chunks  | 引入的模块，这里指定的是entry中设置多个js时，在这里指定引入的js，如果不设置则默认全部引入 |  
| chunksSortMode	  | 在chunk被插入到html之前，你可以控制它们的排序。允许的值 ‘none’ \ ‘auto’ \ ‘dependency’ \ {function} 默认为‘auto’. |  
| excludeChunks	  | 排除的模块 |  
| xhtml	  | 生成的模板文档中标签是否自动关闭，针对xhtml的语法，会要求标签都关闭，默认false |  

###### title
title: 生成的html文档的标题。配置该项，它并不会替换指定模板文件中的title元素的内容，除非html模板文件中使用了模板引擎语法来获取该配置项值，如下ejs模板语法形式：
```
<title>{%= o.htmlWebpackPlugin.options.title %}</title>
```
###### filename
filename：输出文件的文件名称，默认为index.html，不配置就是该文件名；此外，还可以为输出文件指定目录位置（例如'html/index.html'）

> 1.filename配置的html文件目录是相对于webpackConfig.output.path路径而言的，不是相对于当前项目目录结构的。
指定生成的html文件内容中的link和script路径是相对于生成目录下的，写路径的时候请写生成目录下的相对路径。

> 2.指定生成的html文件内容中的link和script路径是相对于生成目录下的，写路径的时候请写生成目录下的相对路径。

###### inject
inject：向template或者templateContent中注入所有静态资源，不同的配置值注入的位置不经相同。
> 1.true或者body：所有JavaScript资源插入到body元素的底部

> 2.head: 所有JavaScript资源插入到head元素中

> 3.false： 所有静态资源css和JavaScript都不会注入到模板文件中

###### minify
minify: 是html-webpack-plugin中集成的 html-minifier ，生成模板文件压缩配置，有很多配置项，
```
{
    caseSensitive: false, //是否大小写敏感
    collapseBooleanAttributes: true, //是否简写boolean格式的属性如：disabled="disabled" 简写为disabled
    collapseWhitespace: true //是否去除空格
}

```
#### 使用
##### 基本使用
在webpack.config.js中引入：
```
const HtmlWebpackPlugin = require('html-webpack-plugin');

```
在src目录下，创建一个Html文件模板，这个模板包含title等其它你需要的元素，在编译过程中，本插件会依据此模板生成最终的html页面，会自动添加所依赖的 css, js，favicon等文件，在本例中我们命名模板文件名称为index.tmpl.html，模板源代码如下

index.tmpl.html
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Webpack</title>
  </head>
  <body>
    <div id='box'>
    </div>
  </body>
</html>
```
webpack.config.js
```
var webpack = require('webpack');
var HtmlWebpackPlugin = require('html-webpack-plugin');
module.exports = {
    plugins: [
    new HtmlWebpackPlugin({
      template: __dirname + "/src/index.tmpl.html"//new 一个这个插件的实例，并传入相关的参数
    })
  ]
}

```
##### 配置多个html页面
html-webpack-plugin的一个实例生成一个html文件，如果单页应用中需要多个页面入口，或者多页应用时配置多个html时，那么就需要实例化该插件多次；

即有几个页面就需要在webpack的plugins数组中配置几个该插件实例：

```
plugins: [
    new HtmlWebpackPlugin({
        filename: 'index.html',
        template: 'src/html/index.html',
        excludeChunks: ['list', 'detail']
    }),
    new HtmlWebpackPlugin({
        filename: 'list.html',
        template: 'src/html/list.html',
        thunks: ['common', 'list']
    }),
    new HtmlWebpackPlugin({
        filename: 'detail.html',
        template: 'src/html/detail.html',
        thunks: ['common', 'detail']
    })
]

```
> 如上例应用中配置了三个入口页面：index.html、list.html、detail.html；并且每个页面注入的thunk不尽相同；类似如果多页面应用，就需要为每个页面配置一个

------------------------------------------------------------------------------------------------
### mini-css-extract-plugin
> 该插件建立在webpack v4功能（模块类型）之上，需要webpack 4才能正常工作.

此插件将CSS提取到单独的文件中。它为每个包含CSS的JS文件创建一个CSS文件。它支持CSS和SourceMaps的按需加载。


 | 插件名称  | 地址  |
| --- | --- |
| mini-css-extract-plugin  | https://github.com/webpack-contrib/mini-css-extract-plugin |  

#### 介绍
##### 安装
```
npm install --save-dev mini-css-extract-plugin

```
##### 引入
在webpack.config.js中引入：
```
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
```
##### 使用
基本使用
```
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
    plugins: [
        new MiniCssExtractPlugin({
            filename: "[name].css",
            chunkFilename: "[id].css"
        })
    ],
    module:{
        rules:[
        {
    test: /\.css$/,
    use: [
            {
                loader: MiniCssExtractPlugin.loader,
                "css-loader"
            }
        ]
    }
}
```
less与css
```
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
    plugins: [
        new MiniCssExtractPlugin({
            filename: "[name]-[hash].css",
            chunkFilename: "[id]-[hash].css"
        })
    ],
    module:{
        rules:[
            {
                test: /\.css$/,
                use: [
                    MiniCssExtractPlugin.loader,
                    'css-loader',
                    'postcss-loader',
                ]
            },
            {
                test: /\.less$/,
                use: [
                    MiniCssExtractPlugin.loader,
                    'css-loader',
                    'less-loader',
                    'postcss-loader',
                ]
            }
        ]
    }
}
```
生产环境压缩

```
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const OptimizeCSSAssetsPlugin = require("optimize-css-assets-webpack-plugin");

module.exports = {
    plugins: [
        new MiniCssExtractPlugin({
            filename: "css/[name].css",
            chunkFilename: "css/[id].css"
        }),
        //通过该插件完成CSS压缩
        new OptimizeCSSPlugin()
    ],
    module:{
        rules:[
        {
    test: /\.css$/,
    use: [
            {
                loader: MiniCssExtractPlugin.loader,
                "css-loader"
            }
        ]
    }
}
```
---------------------------------------------------------------------------------------------------

### optimize-css-assets-webpack-plugin
由于Webpack分离出css文件未压缩，因此需要该插件，主要是为了优化、压缩css文件。

 | 插件名称  | 地址  |
| --- | --- |
| optimize-css-assets-webpack-plugin  | https://github.com/NMFR/optimize-css-assets-webpack-plugin |  

#### 介绍
##### 安装
```
npm install --save-dev optimize-css-assets-webpack-plugin

```
##### 引入
在webpack.config.js中引入：
```
const OptimizeCssAssetsPlugin = require('optimize-css-assets-webpack-plugin');

```
##### 选项

 | 参数  | 	描述  |
| --- | --- |
| assetNameRegExp  | 一个正则表达式，指定要优化\最小化的资源的名称。提供的正则表达式针对配置中MiniCssExtractPlugin实例导出的文件的文件名运行，而不是源CSS文件的文件名。默认为/\.css$/g |  
| cssProcessor  | 用于优化\最小化CSS的CSS处理器，默认为cssnano。|  
| cssProcessorOptions  | 传递给cssProcessor的选项，默认为 {}|  
| canPrint  |一个布尔值，指示插件是否可以将消息打印到控制台，默认为 true|  

```
new  OptimizeCssAssetsPlugin（{
    assetNameRegExp: /\.optimize\.css$/g,
    cssProcessor: require('cssnano'),
    cssProcessorOptions: { discardComments: { removeAll: true } },
    canPrint: true
}）
```
#### 使用
基本使用
```
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const OptimizeCSSAssetsPlugin = require("optimize-css-assets-webpack-plugin");

module.exports = {
    plugins:[
        new MiniCssExtractPlugin({
          filename: "css/[name].css"
        }),
        new OptimizeCSSPlugin()
    ]
}
```
-----------------------------------------------------

### copy-webpack-plugin
将单个文件或整个目录复制到构建目录

 | 插件名称  | 	地址  |
| --- | --- |
| copy-webpack-plugin | https://github.com/webpack-contrib/copy-webpack-plugin |  

#### 介绍
安装
```
npm install copy-webpack-plugin --save-dev
```
引入

```
const  CopyWebpackPlugin  =  require('copy-webpack-plugin')

module.exports = {
    plugins: [
        new CopyWebpackPlugin([ ...patterns ], options)
    ]
}
```
Patterns
```
{ from: 'source', to: 'dest' }

```
示例
```
module.exports = {
    plugins: [
        new CopyWebpackPlugin([{
            from: __dirname + '/src/public'
        }]);
        //作用：把public 里面的内容全部拷贝到编译目录
    ]
}

```
-------------------------------------------------------------------------------------
# 七、WebpackReact
## package.json配置
每个项目的根目录下面，一般都有一个package.json文件，定义了这个项目所需要的各种模块，以及项目的配置信息（比如名称、版本、许可证等元数据）。npm install命令根据这个配置文件，自动下载所需的模块，也就是配置项目所需的运行和开发环境。
### scripts字段
scripts指定了运行脚本命令的npm命令行缩写，比如start指定了运行npm start时，所要执行的命令。查看以下代码。npm start和npm run build分别是运行代码和打包项目。另外，"start"、"test"可以不用run。
#### Windows用户
```
"scripts": {
    "start": "webpack-dev-server --progress --colors",
    "build": "rmdir /s/q build && webpack-cli --config ./webpack.production.config.js --progress --colors"
},

```
#### Mac用户
```
"scripts": {
    "start": "webpack-dev-server --progress --colors",
    "build": "rm -rf ./build && webpack-cli --config webpack.product.config.js --progress --colors"
},

```
这两个命令主要有以下区别：

* start命令：默认使用 webpack.config.js 作为配置文件，
* build命令：强制使用 webpack.production.config.js 作为配置文件

#### 运行开发环境打包配置
在CLI输入npm start就会执行start对应的命令。
```
"scripts": {
    "start": "webpack-dev-server --progress --colors"
},
```
#### 运行生产环境打包配置
在CLI输入npm run build就会执行start对应的命令。
```
"scripts": {
    "build": "rm -rf ./build && webpack-cli --config ./webpack.production.config.js --progress --colors"
},

```
### dependencies字段与devDependencies字段
npm install时使用--save和--save-dev，可分别将依赖（插件）记录到package.json中的dependencies和devDependencies下面。
```
npm install <包名> --save      #将包添加到dependencies 生产依赖列表
npm install <包名> --save-dev  #将包添加到devDependencies 开发依赖列表

```
* dependencies 下记录的是项目在运行时必须依赖的插件，常见的例如react、jquery等，即及时项目打包好了、上线了，这些也是需要用的，否则程序无法正常执行。
* devDependencies 下记录的是项目在开发过程中使用的插件，例如这里我们开发过程中需要使用webpack打包，但是一旦项目打包发布、上线了之后，webpack就都没有用了，即可不用安装了。

它们都指向一个对象。该对象的各个成员，分别由模块名和对应的版本要求组成，表示依赖的模块及其版本范围。

### 示例
```
package.json

"dependencies": {
    "normalize.css": "^8.0.0",
    "react": "^16.2.0",
    "react-dom": "^16.2.0",
    "react-router-dom": "^4.2.2",
    "react-swipe": "^5.1.1",
    "swipe-js-iso": "^2.0.4",
    "whatwg-fetch": "^2.0.3"
},
"devDependencies": {
    "autoprefixer": "^8.1.0",
    "babel-core": "^6.26.0",
    "babel-loader": "^7.1.3",
    "babel-preset-env": "^1.6.1",
    "babel-preset-react": "^6.24.1",
    "css-loader": "^0.28.10",
    "extract-text-webpack-plugin": "^4.0.0-beta.0",
    "file-loader": "^1.1.11",
    "html-webpack-plugin": "^3.0.4",
    "image-webpack-loader": "^4.1.0",
    "less": "^3.0.1",
    "less-loader": "^4.0.6",
    "open-browser-webpack-plugin": "0.0.5",
    "optimize-css-assets-webpack-plugin": "^4.0.0",
    "postcss-loader": "^2.1.1",
    "style-loader": "^0.20.2",
    "url-loader": "^1.0.1",
    "webpack": "^4.1.0"
}
```
--------------------------------------------------------------------
## 开发环境配置
### 目录结构
```
/
 ├── dist/ 自动生成
 │
 ├── src/ 开发目录
 │      ├── components/ 组件
 │      ├── pages/ 页面
 │      ├── index.jsx
 │
 ├── conf/ 工程配置
 │
 ├── test/ 测试
 │
 ├── docs/ 项目文档
 │
 ├── static/ 库文件等，不会被webpack的loader处理,手动管理
 │
 ├── node_modules/ 自动生成，包含.Node 依赖以及开发依赖
 │
 ├── package.json 项目配置文件
 │
 ├── webpack.config.js webpack开发环境配置文件
 │
 ├── webpack.production.config.js webpack生产环境配置文件
 │
 └── README.md 项目说明
 
 ```
### webpack.config.js
React项目基于webpack开发环境配置示例：
```
var webpack = require('webpack');
var path = require('path');
var HtmlWebpackPlugin = require('html-webpack-plugin');
var OpenBrowserPlugin = require('open-browser-webpack-plugin');
module.exports = {
    entry:'./src/index.jsx',
    output: {
        path: path.resolve(__dirname,'/dist'),
        filename: "[name].js"
    },
    module:{
        rules:[
            //.css 文件  style-loader css-loader postcss-loader
            {
                test:/\.css$/,
                loader:"style-loader!css-loader!postcss-loader"
            },
            //.less文件  less less-loader postcss-loader
            {
                test:/\.less$/,
                loader:"style-loader!css-loader!postcss-loader!less-loader"
            },
            //.jsx文件 babel-core babel-loader babel-preset-env babel-preset-react
            {
                test:/\.jsx?$/,
                exclude:/node_modules/,
                loader:"babel-loader"
            },
            {
                test: /\.(png|jpg|gif|jpeg|bmp)$/,
                loader: 'url-loader?limit=10000'
            },
            {
                test:/\.(woff|woff2|svg|ttf|eot)($|\?)/i,
                loader:'url-loader?limit=500000'
            }
        ]
    },
    plugins:[
        new webpack.ProvidePlugin({
            "React": "react",
            "ReactDOM":'react-dom'
        }),
        new webpack.BannerPlugin("Copyright Nico inc."),
        new HtmlWebpackPlugin({
            filename:"./index.html",
            template:"src/index.tmpl.html",
            hash:true
        }),
        new OpenBrowserPlugin({
            url: 'http://localhost:8080'
        }),
        new webpack.HotModuleReplacementPlugin()
    ],
    devServer:{
        contentBase: "./",
        inline: true,//实时刷新
        port:8080,
        compress:true,
        hot: true,
        proxy: {
          "/api.php": "http://localhost/mh"
        }
    }
}
```
### postcss.config.js
自动为CSS3属性添加前缀：
```
module.exports = {
    plugins: [
        require('autoprefixer')
    ]
}

```
.babelrc
```
{
    "presets":["env","react"]
}

```
---------------------------------------------------------------------------------------------------

## 生产环境配置
### 目录结构

```
/
 ├── dist/ 自动生成
 │
 ├── src/ 开发目录
 │      ├── components/ 组件
 │      │
 │      ├── pages/ 页面
 │      │
 │      ├── index.jsx
 │      │
 │      └── index.tmpl.html
 │
 ├── conf/ 工程配置
 │
 ├── test/ 测试
 │
 ├── docs/ 项目文档
 │
 ├── static/ 库文件等，不会被webpack的loader处理,手动管理
 │
 ├── node_modules/ 自动生成，包含.Node 依赖以及开发依赖
 │
 ├── package.json 项目配置文件
 │
 ├── webpack.config.js Webpack 开发环境配置文件
 │
 ├── webpack.production.config.js Webpack生产环境配置文件
 │
 └── README.md 项目说明
 
 ```
### webpack.config.js
React项目基于webpack生产环境配置示例：
```
var webpack = require('webpack');
var path = require('path');
var HtmlWebpackPlugin = require('html-webpack-plugin');
var ExtractTextPlugin = require('extract-text-webpack-plugin');
var OptimizeCSSPlugin = require('optimize-css-assets-webpack-plugin')
module.exports = {
    entry:'./src/index.jsx',
    output: {
        path:path.resolve(__dirname,'dist'),
        filename: "js/[name].js"
    },
    resolve: {
        extensions: ['.js','.jsx']
    },
    module:{
        rules:[
            {
                test:/\.css$/,
                loader:ExtractTextPlugin.extract({ fallback: 'style-loader', use: ['css-loader','postcss-loader'] })
            },
            {
                test:/\.less$/,
                loader:ExtractTextPlugin.extract({ fallback: 'style-loader', use: ['css-loader','less-loader','postcss-loader'] })
            },
            {
                test:/\.jsx?$/,
                exclude:/node_modules/,
                loader:"babel-loader"
            },
            {
                test: /\.(png|jpg|gif|jpeg|bmp)$/,
                loader: ['url-loader?limit=10&name=images/[name].[ext]','image-webpack-loader?{progressive:true, optimizationLevel: 7, interlaced: false, pngquant:{quality: "65-90", speed: 4}}']
            },
            {
                test:/\.(woff|woff2|svg|ttf|eot)($|\?)/i,
                loader:'url-loader?limit=50&name=fonts/[name].[ext]'
            }
        ]
    },
    plugins:[
        new webpack.ProvidePlugin({
            "React": "react",
            "ReactDOM":'react-dom'
        }),
        new HtmlWebpackPlugin({
            template:"./src/index.tmpl.html",
            hash:true,
            minify:{
                caseSensitive: false,
                collapseBooleanAttributes: true,
                collapseWhitespace: true
            }
        }),
        new ExtractTextPlugin('css/[name].css'),
        new OptimizeCSSPlugin({
            cssProcessorOptions: {
                safe: true
            }
        }),
        new webpack.optimize.OccurrenceOrderPlugin()
    ],
    optimization:{
        splitChunks:{
            chunks:"all"
        }
    }
}

```
### postcss.config.js
```
module.exports = {
    plugins: [
        require('autoprefixer')
    ]
}

```
### .babelrc
```
{
    "presets":["env","react"]
}

```
----------------------------------------------------------------------------------
## 项目目录结构推荐
### 目录结构
```
/
 ├── shell/ 脚本
 │
 ├── conf/ 工程配置
 │
 ├── src/ 开发目录
 │      ├── components/ 组件
 │      ├── pages/ 页面
 │      └──
 ├── dist/ 自动生成
 │
 ├── test/ 测试
 │
 ├── node_modules/ 自动生成，包含.Node 依赖以及开发依赖
 │
 ├── static/ 库文件等，不会被webpack的loader处理,手动管理
 │
 └── etc
 ```
 
 完整的目录结构：
 
 ```
 project/  
    ├── shell/    node脚本    
    │    ├──
    │    ├── dev-server.js  本地开发服务器
    │    ├── build.js  打包脚本
    │    └── utils.js  工具函数
    │          
    ├── conf/     工程配置  
    │    ├──
    │    ├── index.js  基础配置文件，在此可简单的修改webpack相关配置
    │    ├── webpack.base.js  webpack的基础配置，主要是loader、resolve的配置
    │    ├── webpack.dev.js  webpack开发配置，主要是eslint、livereload、hot module replacement及相关的插件
    │    ├── webpack.prod.js  webpack生产配置，主要是代码的压缩混淆，图片压缩，加hash
    │    └── karma.conf.js  测试配置
    │   
    ├── src/   开发目录
    │     ├── components/   组件
    │     └── pages/        页面（页面下的项目目录需要遵循一定的规范以便创建webpack的入口文件，不过这些规范是可以调整的；以下只是推荐）
    │           └── index/   首页
    │                ├── images/  图片资源
    │                ├── page.css 样式文件，文件名称可以按照自己意愿命名
    │                ├── page.js  脚本文件及webpack的入口文件，文件名称可以在/conf/index.js配置
    │                └── template.html 模板文件及要撰写的html文件，文件名称可以在/conf/index.js配置
    │                
    │              
    ├── dist/      自动生成
    │      
    ├── test/      测试（目录可以意愿来创建，但是测试文件名称必须遵循*_test.js的命名规范，可在/conf/karma.conf.js修改配置）
    │  
    ├── node_modules/     自动生成，包含node依赖以及开发依赖
    │  
    ├── static/           库文件等，不会被webpack的loader处理,手动管理
    │     
    └── etc

