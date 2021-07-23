---
title: webpack
date: 2018-03-31 13:46:52
tags: webpack
categories: webpack
cover: https://raw.githubusercontent.com/webpack/media/master/logo/logo-on-white-bg.png
---

``` js
// 简单配置
const HtmlWebpackPlugin = require('html-webpack-plugin'); // 通过 npm 安装
const webpack = require('webpack'); // 用于访问内置插件
const path = require('path');

const config = {
  entry: './path/to/my/entry/file.js',  // 输入：项目主文件（入口文件）
  output: { // 输出
    path: path.resolve(__dirname, 'dist'),  //想要生成(emit)到哪里
    filename: 'my-first-webpack.bundle.js'  // webpack bundle 的名称
  },
  module: { // 配置加载资源
    rules: [  // 规则
      { test: /\.txt$/, use: 'raw-loader' } // webpack 编译器碰到「在 require()/import 语句中被解析为 '.txt' 的路径」时，在对它打包之前，先使用 raw-loader 转换一下。
      // test 属性，用于标识出应该被对应的 loader 进行转换的某个或某些文件。
      // use 属性，表示进行转换时，应该使用哪个 loader。
    ],
  plugins: [   // webpack插件配置
    // new webpack.optimize.UglifyJsPlugin(),  // 现在也不需要使用这个plugin了，只需要使用optimization.minimize为true就行，production mode下面自动为true
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
  }
};

module.exports = config;

```

#### 入口(entry)

可以通过在 webpack 配置中配置 `entry` 属性，来指定一个入口起点（或多个入口起点）。

##### 单个入口（简写）语法

用法：`entry: string|Array<string>`

``` js
const config = {
  entry: './path/to/my/entry/file.js' // 简写
  // entry: {
  //   main: './path/to/my/entry/file.js'
  // }
};
```

##### 对象语法

用法：`entry: {[entryChunkName: string]: string|Array<string>}`

``` js
const config = {
  entry: {
    app: './src/app.js',
    vendors: './src/vendors.js'
  }
};

module.exports = config;
```

#### 输出(output)

output 属性告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件。注意，即使可以存在多个入口起点，但只指定一个输出配置。
在 webpack 中配置 `output` 属性的最低要求是，将它的值设置为一个对象，包括以下两点：

* filename 用于输出文件的文件名。
* 目标输出目录 path 的绝对路径。

``` js
const config = {
  output: {
    filename: 'bundle.js',
    path: '/home/proj/public/assets'
  }
};

module.exports = config;
// 此配置将一个单独的 bundle.js 文件输出到 /home/proj/public/assets 目录中。
```

如果配置创建了多个单独的 "chunk"，则应该使用占位符(substitutions)来确保每个文件具有唯一的名称。

``` js
{
  entry: {
    app: './src/app.js',
    search: './src/search.js'
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/dist'
  }
}

// 写入到硬盘：./dist/app.js, ./dist/search.js
```

#### loader

loader 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader 可以将所有类型的文件转换为 webpack 能够处理的有效模块，然后你就可以利用 webpack 的打包能力，对它们进行处理。
在你的应用程序中，有三种使用 loader 的方式：

* **配置（推荐）：在 webpack.config.js 文件中指定 loader。**

`module.rules` 允许你在 webpack 配置中指定多个 loader。 这是展示 loader 的一种简明方式，并且有助于使代码变得简洁。同时让你对各个 loader 有个全局概览：

``` js
module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          { loader: 'style-loader' },
          {
            loader: 'css-loader',
            options: {
              modules: true
            }
          }
        ]
      }
    ]
  }
```

* 内联：在每个 import 语句中显式指定 loader。

可以在 import 语句或任何等效于 "import" 的方式中指定 loader。使用 ! 将资源中的 loader 分开。分开的每个部分都相对于当前目录解析。

``` js
import Styles from 'style-loader!css-loader?modules!./styles.css';
```

通过前置所有规则及使用 !，可以对应覆盖到配置中的任意 loader。
选项可以传递查询参数，例如 ?key=value&foo=bar，或者一个 JSON 对象，例如 ?{"key":"value","foo":"bar"}。

* CLI：在 shell 命令中指定它们。

你也可以通过 CLI 使用 loader：

``` js
webpack --module-bind jade-loader --module-bind 'css=style-loader!css-loader'
```

这会对 .jade 文件使用 jade-loader，对 .css 文件使用 style-loader 和 css-loader。

#### 插件(plugins)

想要使用一个插件，你只需要 require() 它，然后把它添加到 plugins 数组中。多数插件可以通过选项(option)自定义。你也可以在一个配置文件中因为不同目的而多次使用同一个插件，这时需要通过使用 new 操作符来创建它的一个实例。
webpack.config.js

``` js
const HtmlWebpackPlugin = require('html-webpack-plugin'); //通过 npm 安装
const webpack = require('webpack'); //访问内置的插件
const path = require('path');

const config = {
  entry: './path/to/my/entry/file.js',
  output: {
    filename: 'my-first-webpack.bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        use: 'babel-loader'
      }
    ]
  },
  plugins: [
    // new webpack.optimize.UglifyJsPlugin(),  // 现在也不需要使用这个plugin了，只需要使用optimization.minimize为true就行，production mode下面自动为true
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
};

module.exports = config;
```

[配置](https://doc.webpack-china.org/configuration/)
[Configuration](https://webpack.js.org/configuration/)

#### mode/–mode参数

4.X 新增了mode/--mode参数来表示是开发还是生产，mode有两个可选值：development和production，production不支持监听，production侧重于打包后的文件大小，development侧重于构建的速度。
`webpack --mode development`

也可以在配置文件中配置：

``` js
// webpack.config.js
module.exports = {
    mode: "production",
    // ...
}
```

#### HtmlWebpackPlugin

HtmlWebpackPlugin 会默认生成 index.html 文件
首先安装插件，并且调整 webpack.config.js 文件：
`npm install --save-dev html-webpack-plugin`

``` js
+ const HtmlWebpackPlugin = require('html-webpack-plugin');

  module.exports = {
+   plugins: [
+     new HtmlWebpackPlugin({
+       title: 'Output Management'
+     })
+   ]
  };
```

#### 清理 /dist 文件夹

clean-webpack-plugin 是一个比较普及的管理插件，让我们安装和配置下。
`npm install clean-webpack-plugin --save-dev`

``` js
+ const CleanWebpackPlugin = require('clean-webpack-plugin');

  module.exports = {
    plugins: [
+     new CleanWebpackPlugin(['dist']), // 在每次构建前清理 /dist 文件夹
      new HtmlWebpackPlugin({
        title: 'Output Management'
      })
    ],
  };
```

#### source map

为了更容易地追踪错误和警告，JavaScript 提供了 source map 功能，将编译后的代码映射回原始源代码。
source map 有很多[不同的选项](https://doc.webpack-china.org/configuration/devtool)可用，请务必仔细阅读它们，以便可以根据需要进行配置。

``` js
 module.exports = {
  devtool: 'inline-source-map',
  };
```

#### webpack-dev-server

`webpack-dev-server` 为你提供了一个简单的 web 服务器，并且能够实时重新加载(live reloading)。让我们设置以下：
`npm install --save-dev webpack-dev-server`
修改配置文件，告诉开发服务器(dev server)，在哪里查找文件：

``` js
 module.exports = {
+   devServer: {
+     port: 8000,
+     contentBase: './dist'
+   }
  };
```

以上配置告知 `webpack-dev-server`，在 `localhost:8000`(默认8080) 下建立服务，将 dist 目录下的文件，作为可访问文件。

让我们添加一个 script 脚本，可以直接运行开发服务器(dev server)：

``` js
"scripts": {
+     "start": "webpack-dev-server --open",
},
```

webpack-dev-server 带有许多可配置的选项。转到[相关文档](https://doc.webpack-china.org/configuration/dev-server)以了解更多。

#### 模块热替换(Hot Module Replacement)

模块热替换(Hot Module Replacement 或 HMR)是 webpack 提供的最有用的功能之一。它允许在运行时更新各种模块，而无需进行完全刷新。

启用此功能实际上相当简单。而我们要做的，就是更新 `webpack-dev-server` 的配置，和使用 webpack 内置的 HMR 插件

``` js
+ const webpack = require('webpack');
 module.exports = {
    devServer: {
      contentBase: './dist',
      port: 8000,
      overlay: {  // webpack编译出现错误，则显示到网页上
        errors: true,
      },
+     hot: true // 不刷新热加载数据
    },
    plugins: [
      new HtmlWebpackPlugin({
        title: 'Hot Module Replacement'
      }),
+     new webpack.NamedModulesPlugin(),
+     new webpack.HotModuleReplacementPlugin()
    ]
  };
```

注意，我们还添加了 `NamedModulesPlugin`，以便更容易查看要修补(patch)的依赖

#### 生产环境构建

开发环境(development)和生产环境(production)的构建目标差异很大。在开发环境中，我们需要具有强大的、具有实时重新加载(live reloading)或热模块替换(hot module replacement)能力的 source map 和 localhost server。而在生产环境中，我们的目标则转向于关注更小的 bundle，更轻量的 source map，以及更优化的资源，以改善加载时间。由于要遵循逻辑分离，我们通常建议为每个环境编写**彼此独立的 webpack 配置**。
虽然，以上我们将生产环境和开发环境做了略微区分，但是，请注意，我们还是会遵循不重复原则(Don't repeat yourself - DRY)，保留一个“通用”配置。为了将这些配置合并在一起，我们将使用一个名为 `webpack-merge` 的工具。通过“通用”配置，我们不必在环境特定(environment-specific)的配置中重复代码。

我们先从安装 webpack-merge 开始，并将之前指南中已经成型的那些代码再次进行分离：
`npm install --save-dev webpack-merge`

``` js
// project
  webpack-demo
  |- package.json
- |- webpack.config.js
+ |- webpack.common.js
+ |- webpack.dev.js
+ |- webpack.prod.js
  |- /dist
  |- /src
    |- index.js
    |- math.js
  |- /node_modules
```

``` js
// webpack.common.js
+ const path = require('path');
+ const CleanWebpackPlugin = require('clean-webpack-plugin');
+ const HtmlWebpackPlugin = require('html-webpack-plugin');
+
+ module.exports = {
+   entry: {
+     app: './src/index.js'
+   },
+   plugins: [
+     new CleanWebpackPlugin(['dist']),
+     new HtmlWebpackPlugin({
+       title: 'Production'
+     })
+   ],
+   output: {
+     filename: '[name].bundle.js',
+     path: path.resolve(__dirname, 'dist')
+   }
+ };
```

``` js
//webpack.dev.js
+ const merge = require('webpack-merge');
+ const common = require('./webpack.common.js');
+
+ module.exports = merge(common, {
+   devtool: 'inline-source-map',
+   devServer: {
+     contentBase: './dist'
+   }
+ });
```

``` js
// webpack.prod.js
+ const merge = require('webpack-merge');
// + const UglifyJSPlugin = require('uglifyjs-webpack-plugin');  // 4.X 现在也不需要使用这个plugin了，只需要使用optimization.minimize为true就行，production mode下面自动为true
+ const common = require('./webpack.common.js');
+
+ module.exports = merge(common, {
+   plugins: [
// +     new UglifyJSPlugin()  // 4.X 现在也不需要使用这个plugin了，只需要使用optimization.minimize为true就行，production mode下面自动为true
+   ]
+ });
```

现在，在 webpack.common.js 中，我们设置了 entry 和 output 配置，并且在其中引入这两个环境公用的全部插件。
在 webpack.dev.js 中，我们为此环境添加了推荐的 `devtool`（强大的 source map）和简单的 devServer 配置。
最后，在 webpack.prod.js 中，我们引入了之前在 tree shaking 指南中介绍过的 ~~UglifyJSPlugin~~。4.X 现在也不需要使用这个plugin了，只需要使用optimization.minimize为true就行，production mode下面自动为true.从webpack 4开始，这也可以通过"mode"配置选项轻松切换，设置为"production"。

``` js
module.exports = {
+ mode: "production"
};
```

现在，我们把 scripts 重新指向到新配置。我们将 npm start 定义为开发环境脚本，并在其中使用 webpack-dev-server，将 npm run build 定义为生产环境脚本：

``` js
// package.json
  {
    "scripts": {
-     "start": "webpack-dev-server --open",
+     "start": "webpack-dev-server --open --config webpack.dev.js",
-     "build": "webpack"
+     "build": "webpack --config webpack.prod.js"
    }
  }
```

##### 指定环境

许多 library 将通过与 process.env.NODE_ENV 环境变量关联，以决定 library 中应该引用哪些内容。例如，当不处于生产环境中时，某些 library 为了使调试变得容易，可能会添加额外的日志记录(log)和测试(test)。其实，当使用 process.env.NODE_ENV === 'production' 时，一些 library 可能针对具体用户的环境进行代码优化，从而删除或添加一些重要代码。我们可以使用 webpack 内置的 [DefinePlugin](https://doc.webpack-china.org/plugins/define-plugin) 为所有的依赖定义这个变量：

``` js
// webpack.prod.js
  module.exports = merge(common, {
+     new webpack.DefinePlugin({
+       'process.env.NODE_ENV': JSON.stringify('production')
+     })
    ]
  })
```

技术上讲，NODE_ENV 是一个由 Node.js 暴露给执行脚本的系统环境变量。通常用于决定在开发环境与生产环境(dev-vs-prod)下，服务器工具、构建脚本和客户端 library 的行为。然而，与预期不同的是，无法在构建脚本 webpack.config.js 中，将 process.env.NODE_ENV 设置为 "production"，请查看 #2537。因此，例如 process.env.NODE_ENV === 'production' ? '[name].[hash].bundle.js' : '[name].bundle.js' 这样的条件语句，在 webpack 配置文件中，无法按照预期运行。

#### [ExtractTextWebpackPlugin](https://doc.webpack-china.org/plugins/extract-text-webpack-plugin)
4.X 版本支持插件 [mini-css-extract-plugin](https://github.com/webpack-contrib/mini-css-extract-plugin)
安装：`npm install --save-dev extract-text-webpack-plugin`
用法：

``` js
const ExtractTextPlugin = require("extract-text-webpack-plugin");

module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ExtractTextPlugin.extract({
          fallback: "style-loader",
          use: "css-loader"
        })
      }
    ]
  },
  plugins: [
    new ExtractTextPlugin("styles.css"),
  ]
}
```

它会将所有的入口 chunk(entry chunks)中引用的 *.css，移动到独立分离的 CSS 文件。因此，你的样式将不再内嵌到 JS bundle 中，而是会放到一个单独的 CSS 文件（即 styles.css）当中。 如果你的样式文件大小较大，这会做更快提前加载，因为 CSS bundle 会跟 JS bundle 并行加载。

#### 代码分离

有三种常用的代码分离方法：

* 入口起点：使用 entry 配置手动地分离代码。
* 防止重复：使用 CommonsChunkPlugin 去重和分离 chunk。
* 动态导入：通过模块的内联函数调用来分离代码。

##### 防止重复(prevent duplication)

webpack 4删除了[CommonsChunkPlugin](https://gist.github.com/sokra/1522d586b8e5c0f5072d7565c2bee693),以支持两个新选项（`optimization.splitChunks`和`optimization.runtimeChunk`）。
默认配置只会对异步请求的模块进行提取拆分，如果要对entry进行拆分，需要设置`optimization.splitChunks.chunks = 'all'`。
`optimization.runtimeChunk`，设置为`true`就会自动拆分runtime文件

``` js
// 默认配置
splitChunks: {
chunks: "async",                      // 可选"initial"，"async"和"all"
  minSize: 30000,                     // 最小尺寸，默认0,
  minChunks: 1,                       // 最小 chunk ，默认1
  maxAsyncRequests: 5,                // 最大异步请求数，默认5
  maxInitialRequests: 3,              // 最大初始化请求数，默认3
  name: true,
  cacheGroups: {
    default: {
      minChunks: 2,
      priority: -20
      reuseExistingChunk: true,
    },
    vendors: {
      test: /[\\/]node_modules[\\/]/, // 正则规则验证，如果符合就提取 chunk
      priority: -10                   // 缓存组优先级
    }
  }
}
```

~~CommonsChunkPlugin 插件可以将公共的依赖模块提取到已有的入口 chunk 中，或者提取到一个新生成的 chunk~~。

``` js
 module.exports = {
    plugins: [
+     new webpack.optimize.CommonsChunkPlugin({
+       name: 'common' // 指定公共 bundle 的名称。
+     })
    ]
  };
```
