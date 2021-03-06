# projectTemplet
前端项目构建模板。为前端项目搭建提供统一的规范。

__请使用 es6 开发，并使用 sass 编写样式文件。__

# 项目安装

* 下载
    ```
    git clone git@github.com:ChinTeo/projectTemplet.git
    ```
* __进入到项目目录__

    ```
    cd projectTemplet/
    ```
* __安装项目依赖__
    ```
    npm install
    ```
# __项目配置__  

项目所有相关配置放在 project.json 文件中。

project.json 文件格式如下：

```
    {
      "name": "project", //项目名称
      "version": "1.0.0", //项目版本号
      "description": "a simple templet.", //项目描述
      "server": { //webpack-dev-server 服务器相关配置
        "port": "9000" //开启服务器的端口号，默认 9000
      },
      "entry": { //入口文件相关配置
        "app": "./app.js", //入口文件
        "index": "./index.js" //入口文件，若有多个入口文件
      },
      "output": {
        "hash": false,
        "common": true
      },
      "img": {
        "limit": 8000,
        "minify": false
      },
      "scss": {
        "file": [],
        "distname": "style",
        "minify": true,
        "md5": true
      },
      "eslint": true,
      "common": {
        "name": "common",
        "chunks": ["app", "index"]
      },
      "externals": {

      }
    }

```

### __入口文件配置__

入口文件在 entry 中配置。

```
"entry": {
    "[output-file-name]": "[entry-file-path]"
}
```
output-file-name 处填写生成的文件名称，entry-file-path 处填写入口文件路径。

若一个文件有多个入口文件，则采用以下写法：

```
"entry": {
    "[output-file-name]": ["[entry-file1-path]", "[entry-file2-path]"]
}
```
将多个入口文件写在一个数组中。

若有多个生成文件，则采用以下写法：

```
"entry": {
    "[output-file1-name]": "[entry-file1-path]",  
    "[output-file2-name]": "[entry-file2-path]"
}
```

###  __生成文件配置__

生成文件相关配置写在 output 字段中。

* __publicPath__ 配置上线的文件路径。如果缺省此字段默认是和文件打包路径相同的。注意：只有当你确认上线的文件路径时才可以配此字段。
* __hash__  生成的文件是否需要hash命名。 true | false。默认为false。
* __common__ 是否需要把多个生成文件的公共代码提取成单独文件 common.js 。true|false。默认为false。
    如果需要更改提取的go'g公共代码文件名称，在 common 字段的 name 处进行配置。默认 name 为 'common'。


### __提取公共代码__

在 output 中将 common 字段设置为 true，即可启用公共代码提取功能。相关配置在 common 字段里。

```
"common": {
    "name": "common",
    "chunks": ["app", "index"]
}
```
name 字段用来设置公共代码文件名称，默认为common。（注意只写名字不用加后缀）。    

chunks 字段用来指定需要提取公共代码的生成文件，如例子中指定 "app" 和 "index"，则会将打包生成的 app.js 和 index.js 中的相同代码提取出来。 缺失此字段或值设为空数组  [ ] 则会默认为提取所有文件。

### __启用eslint__

eslint 校验默认为关闭。若要启用 eslint ， 将 eslint 字段复制为 true 即可。

### __图片打包配置__

图片文件配置在 img 字段中。

* __limit__ 图片文件转 base64 大小限制。默认 8000， 小于 8000b 的图片文件将转为 base64。
* __minify__ 是否对图片进行压缩处理。

### __scss编译配置__

scss 编译相关配置在 scss 字段中，详见下一段 __编译scss__。

# __编译scss__

使用 scss 文件时只需在 js 文件中 import 引入即可。打包时会将 js 与 scss 打包到同一文件之中。

如果想将 scss 文件单独处理，可以将想要编译的 scss 文件路径填入 project.json 的 scss.name 字段中，然后运行以下命令：
```
        npm run scss
```
会将 scss 文件编译为 css 并产出在 dist/css 路径下。

__scss 编译配置__

```
    "scss": {
        "file": [],//scss 文件入口
        "distname": "style",//打包文件名称
        "minify": true,// 是否压缩
        "md5": true //是否添加MD5命名
    }
```

__全局依赖配置__
externals 字段用来配置全局依赖，会阻止将配置的依赖包打包到代码中。

例如在使用了 jquery 后想要将 jquery 文件与打包文件分离，配置如下：
```
externals: {
  jquery: 'jQuery'
}
```
配置后可以照常 import jquery文件：
```
import $ from 'jquery';

$('.my-element').animate(...);
```
但此时不会将 jquery 模块打包。  
所以需要在页面另行引入 jquery。

# __使用 React__

默认情况下是不支持 react 环境的，如果想要使用 react，需要初始化 react 环境。  

执行 ```npm run init-react``` 来安装配置 react。 命令执行完毕后即可在项目中使用 react 了。

虽然也许用不到，但是如果发现项目不适合 react 的话需要卸载 react 环境，可以执行 ```npm run uninstall-react``` 来清理 react 相关配置。

__react 依赖库分离__

如下配置 externals 字段：
```
externals: {
  'react': 'React',
  'react-dom': 'ReactDOM'
}
```
此配置会阻止将react及react-dom依赖打包到代码中。  
注意：因为打包后的文件不包含react，因此需要在页面内另引入 react 及 react-dom 文件。

# __项目调试__  

开启本地服务器运行项目
```
npm run start
```
# __打包资源文件__

打包项目文件

```
   npm run build
```

打包线上文件（混淆压缩）

```
npm run dist
```

*打包后的项目文件在dist目录下

*若要修改打包后的文件名称，在 project.json 中将 name 改成想要的打包文件名即可。

example：
```
    {
      "name": "myproject"
      ...
    }
```
打包后的文件名即为 myproject。
# 目录结构
```
.  
├── LICENSE  
├── README.md  
├── TREE.md        目录结构  
├── dist           文件打包路径  
├── index.html     模板文件
├── .eslintrc.json eslint配置文件
├── gulpfile.js    gulp文件
├── package.json  
├── project.json   项目配置
├── src            资源文件  
│   ├── assets     公共资源（库文件等）  
│   ├── components 模板文件（react、vue等的模板）
│   ├── index.js   入口文件  
│   └── static     项目静态资源文件  
│        ├── js     js资源文件
│        ├── style  样式资源文件
│        └── img    图片资源
├── views           模板文件目录
├── webpack.config.dev.js  
├── webpack.config.js  
└── webpack.config.prod.js  
```

# 依赖包

* __webpack__  模块打包工具
* __webpack-dev-server__ webpack 本地服务器，用于本地调试
* __glob__ 允许你使用 *等符号, 来写一个glob规则,像在shell里一样,获取匹配对应规则的文件.
* __chalk__ 允许使用特定颜色打印输出
* __babel-loader__ 加载es6文件
* __cross-env__ 设置node环境变量工具
* __css-loader__ 加载css文件
* __file-loader__ url-loader的依赖
* __node-sass__ 编译sass
* __open-browser-webpack-plugin__ 可以直接打开浏览器
* __sass-loader__ 加载sass文件
* __style-loader__ 加载样式文件
* __url-loader__ 加载图片文件
* __babel-core babel__ 编译核心代码
* __babel-preset-latest__
* __babel-runtime__

* __eslint-config-airbnb__ eslint 相关依赖
* __eslint-plugin-import__
* __eslint-plugin-jsx-a11y__
* __eslint-plugin-react__

* __gulp__  gulp 项目构建工具
* __gulp-concat__ gulp 文件合并
* __gulp-minify-css__ gulp 压缩css
* __gulp-rev__ gulp 文件添加md5后缀

# 状态分析

webpack 会记录打包状态，可以使用 ```npm run stat``` 来生成记录文件 stat.json。

将 stat.json 上传至以下网站即可获取可视化数据。  
[官方分析平台](http://webpack.github.io/analyse/#home)  
[Webpack Visualizer ](http://chrisbateman.github.io/webpack-visualizer/)

# 常见问题

* 问题描述：浏览器提示 ```webpackJsonp is not defined```
  问题原因：因为启用了 common，在模板中把 common 文件放在了其他文件后面。  
  解决方法：把common 文件的引用位置放在其他文件前面即可。

* 出现错误提示：
```events.js:72
        throw er; // Unhandled 'error' event
              ^
```
一般就是已经运行的另一个服务器使用了相同的端口，请检查是否已开启服务。
