---
title: webpack
tags: webpack
categories: webpack
cover: https://raw.githubusercontent.com/webpack/media/master/logo/logo-on-white-bg.png
---

## webpack5 

初始化项目

``` sh
yarn init -y
```



安装 `webpack`

```sh
yarn add webpack webpack-cli --dev
```

根目录创建 `.webpack.config.js` 并添加以下内容

``` js
module.exports = {
  mode: 'production',
}
```

在 `package.json` 添加

``` json
"scripts": {
  "build": "webpack"
},
```



### 支持部分 IE 浏览器

根目录创建 `.browserslistrc` 并添加以下内容

``` 
[production]	# production 环境
> 1%	# 支持大于 1% 的浏览器
ie 9	# 支持 IE9

[modern]
last 1 chrome version	# chrome 最新的一个个版本
last 1 firefox version	# firefox 最新的一个个版本

[ssr]
node 12

```



### 用 `babel` 打包 `js`

安装  `babel` 相关依赖

```sh
yarn add babel-loader @babel/core @babel/preset-env --dev
```

在 `webpack.config.js` 里添加

``` js
module: {
  rules: [
      {
          test: /\.jsx?$/,	// 匹配以 .js、.jsx 结尾的文件
          exclude: /node_modules/,	// 排除文件夹 node_modules
          use: {	// 使用的 babel-loader
              loader: 'babel-loader',
              options: {
                  presets: [
                      ['@babel/preset-env']
                  ]
              }
          }
      }
  ]
}
```

### 支持 `React`

安装  `@babel/preset-react` 

``` sh
yarn add react react-dom
```



安装  `@babel/preset-react` 

```sh
yarn add @babel/preset-react --dev
```

在 `webpack.config.js` 文件中 `babel-loder` 的 `options` 里修改为

``` js
use: {
    loader: 'babel-loader',
    options: {
        presets: [
            ['@babel/preset-env'],
            ['@babel/preset-react']
        ]
    }
}
```


### 添加 `eslint`

安装 `eslint` 依赖

``` sh
yarn add eslint eslint-webpack-plugin eslint-plugin-import eslint-plugin-flowtype babel-eslint  --dev
```

在 `webpack.config.js` 中添加 `eslint-webpack-plugin`

``` js
const ESLintPlugin = require('eslint-webpack-plugin')

module.exports = {
  mode: 'production',
  plugins: [new ESLintPlugin({
    extensions: ['.js']
  })],
  module: {
    // ...
  }
}
```

### 添加 `react`规则

安装 `eslint` 依赖

``` sh
yarn add eslint-config-react-app eslint-plugin-jsx-a11y eslint-plugin-react eslint-plugin-react-hooks --dev
```

根目录创建 `.eslintrc.js` 并添加以下内容

``` js
module.exports = {
  extends: ['react-app'],	// react 规则
  rules: {	// 自定义规则
    'react/jsx-uses-react': [2],	// 0：不开启、1：警告、2：报错
    'react/react-in-jsx-scope': [2],// 提示要在 JSX 文件里手动引入 React
    'no-console': [0]
  }
}
```

在 `webpack.config.js` 的 `eslint-webpack-plugin` 添加 `.jsx` 检查 `jsx` 文件

``` js
const ESLintPlugin = require('eslint-webpack-plugin')

module.exports = {
  mode: 'production',
  plugins: [new ESLintPlugin({
    extensions: ['.js', '.jsx']
  })],
  module: {
    rules: [
      {
        test: /\.jsx?$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: [
              ['@babel/preset-env'],
              ['@babel/preset-react', { runtime: 'classic' }]
            ]
          }
        }
      }
    ]
  }
}
```

### 打包 `TypeScript`

安装 `typescript` 依赖

``` sh
yarn add typescript @babel/preset-typescript --dev
```

添加 `tsconfig.json`

``` sh
npx tsc --init
```

修改 `webpack.config.js`

``` js
module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.[jt]sx?$/,	// 匹配 ts 文件
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: [
              ['@babel/preset-env'],
              ['@babel/preset-react', { runtime: 'classic' }],
              ['@babel/preset-typescript']	// 添加 ts 规则
            ]
          }
        }
      }
    ]
  }
}
```



为 `ts` 添加 `eslint`

安装 `airbnb`

``` sh
yarn add eslint-config-airbnb-typescript @typescript-eslint/eslint-plugin --dev
```

在 `.eslintrc` 添加一下代码

```js
module.exports = {
  // ...
  overrides: [{
    files: ['*.ts', '*.tsx'],
    parserOptions: {
      project: './tsconfig.json',
    },
    extends: ['airbnb-typescript'],	使用 airbnb 代码检查
    rules: {	// 配置规则
      '@typescript-eslint/object-curly-spacing': [0],
      'import/prefer-default-export': [0],
      'no-console': [0],	// console
      'import/extensions': [0]	// 引入拓展名
    }
  }]
}
```

在 `webpack.config.js` 的 `ESLintPlugin` 添加 `.ts` 和 `.tsx` 

``` js
module.exports = {
  // ...
  plugins: [new ESLintPlugin({
    extensions: ['.js', '.jsx', '.ts', '.tsx']
  })],
  // ...
}
```

### 支持 `tsx`

安装 `react` 声明文件

``` sh
yarn add @types/react @types/react-dom --dev
```



修改 `tsconfig.json`

``` json	
{
    "jsx": "react",
    "strict": false,
    "noImplicitAny": true,   
}
```

### 支持 `@`

修改 `tsconfig.json`

``` json	
{
    "baseUrl": "./",
    "paths": {
      "@/*": ["src/*"]
    },     
}
```

在 `webpack.config.js` 中添加

``` js
const path = require('path');

module.exports = {
  // ...
    resolve: {
    alias: {
      '@': path.resolve(__dirname, './src/')
    }
  },
  // ...
}
```

### 支持 `SCSS`

安装依赖

``` sh
yarn add sass style-loader css-loader sass-loader --dev
```

在 `webpack.config.js` 中添加

``` js
module.exports = {
  // ...
  module: {
    rules: [
	// ...
      {
        test: /\.s[ac]ss$/i,
        use: [
          'style-loader',
          'css-loader',
          'sass-loader'
        ]
      }
    ]
  }
}
```

自动引入全局 `SCSS` 文件

在 `webpack.config.js` 中修改 `sass-loader`

``` js
module.exports = {
  // ...
  module: {
    rules: [
	// ...
      {
        test: /\.s[ac]ss$/i,
        use: [
          'style-loader',
          'css-loader',
          {
            loader: 'sass-loader',
            options: {
              additionalData: `@import '~[文件路径]';`,	// 在每一个文件前附加数据
              sassOptions: {
                includePaths: [__dirname]	// 基于当前目录
              }
            }
          }
        ]
      }
    ]
  }
}
```

`JS` 读取`SCSS` 变量

在 `webpack.config.js` 中修改 `css-loader`

``` js
module.exports = {
  // ...
  module: {
    rules: [
	// ...
      {
        test: /\.s[ac]ss$/i,
        use: [
          'style-loader',
          {
            loader: 'css-loader',
            options: {
              modules: {
                compileType: 'icss'
              }
            }
          },
          {
            loader: 'sass-loader',
            // ...
            }
          }
        ]
      }
    ]
  }
}
```

### 支持 `LESS`

安装依赖

``` sh
yarn add less less-loader --dev
```

在 `webpack.config.js` 中添加

``` js
module.exports = {
  // ...
  module: {
    rules: [
	// ...
      {
        test: /\.less$/i,
        use: [
          'style-loader',
          {
            loader: 'css-loader',
            options: {
              modules: {
                compileType: 'icss'
              }
            }
          },
          { loader: 'less-loader' }
        ]
      }
     // ...
    ]
  }
}
```

自动引入全局 `LESS` 文件

在 `webpack.config.js` 中修改 `less-loader`

``` js
module.exports = {
  // ...
  module: {
    rules: [
	// ...
      {
        test: /\.less$/i,
        use: [
          // ...
          {
            loader: 'less-loader',
            options: {
              additionalData: `@import '~[文件路径]';`	// 在每一个文件前附加数据
            }
          }
        ]
      }
    ]
  }
}
```



### 支持 `stylus`

安装依赖

``` sh
yarn add stylus stylus-loader --dev
```

在 `webpack.config.js` 中添加

``` js
module.exports = {
  // ...
  module: {
    rules: [
	// ...
      {
        test: /\.styl(us)?$/i,
        use: [
          'style-loader',
          {
            loader: 'css-loader',
            options: {
              modules: {
                compileType: 'icss'
              }
            }
          },
          {
            loader: 'stylus-loader',
            options: {
              stylusOptions: {
                import: [path.resolve(__dirname, '[文件路径]')]
              }
            }
          }
        ]
      }
     // ...
    ]
  }
}
```



### 把 `CSS`打包为单独文件

安装依赖

``` sh
yarn add mini-css-extract-plugin --dev
```

在 `webpack.config.js` 中添加

``` js
// ...
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const mode = 'production';
module.exports = {
  mode,
  plugins: [
    new ESLintPlugin({ extensions: ['.js', '.jsx', '.ts', '.tsx'] }),
    mode === 'production' && new MiniCssExtractPlugin()	// 添加 MiniCssExtractPlugin 插件
  ].filter(Boolean),
  // ...
  module: {
    rules: [
      // ...
      {
        test: /\.less$/i,
        use: [
          mode === 'production' ? MiniCssExtractPlugin.loader : 'style-loader', // 只在生产环境单独打包
          // ...
        ]
      },
      {
        test: /\.styl(us)?$/i,
        use: [
          mode === 'production' ? MiniCssExtractPlugin.loader : 'style-loader',
          // ...
        ]
      },
      {
        test: /\.s[ac]ss$/i,
        use: [
          mode === 'production' ? MiniCssExtractPlugin.loader : 'style-loader',
          // ...
        ]
      }
    ]
  }
}

```

为 `CSS、JS`文件名添加 `hash`

在 `webpack.config.js` 中添加

``` js
// ...
module.exports = {
  mode,
  output: { filename: '[name].[contenthash].css' },	// 为 JS 添加 hash
  plugins: [
    new ESLintPlugin({ extensions: ['.js', '.jsx', '.ts', '.tsx'] }),
    new MiniCssExtractPlugin({ filename: '[name].[contenthash].css' })	// 为 CSS 添加 hash
  ],
  // ...
}

```

### 把 `CSS`打包为单独文件

安装依赖

``` sh
yarn add html-webpack-plugin --dev
```

在 `webpack.config.js` 中添加

``` js
// ...
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
module.exports = {
  mode,
  plugins: [
    // ...
    new HtmlWebpackPlugin()
  ].filter(Boolean),
  // ...
}

```

### 自动清理 `dist`

安装依赖

``` sh
yarn add clean-webpack-plugin --dev
```

在 `webpack.config.js` 中添加

``` js
// ...
const HtmlWebpackPlugin = require('clean-webpack-plugin');
module.exports = {
  mode,
  output: { 
    // ... 
    path: path.resolve(__dirname, 'dist') 
  },
  plugins: [
    // ...
    new HtmlWebpackPlugin()
  ].filter(Boolean),
  // ...
}

```

### 优化

#### `runtime` 打包优化

在 `webpack.config.js` 中添加

``` js
module.exports = {
  // ...
  optimization: {
    runtimeChunk: 'single'
  },
  // ...
}

```

#### node 依赖单独打包

在 `webpack.config.js` 中添加

``` js
module.exports = {
  // ...
  optimization: {
    // ...
    splitChunks: {
      cacheGroups: {
        vendor: {
          minSize: 0,	// 如果不写 0，由于 React 文件尺寸太小，会直接跳过
          test: /[\\/]node_modules[\\/]/, // 为了匹配 /node_modules/ 或 \node_modules\
          name: 'vendors', // 文件名
          chunks: 'all',  // all 表示同步加载和异步加载，async 表示异步加载，initial 表示同步加载
          // 这三行的整体意思就是把两种加载方式的来自 node_modules 目录的文件打包为 vendors.xxx.js
          // 其中 vendors 是第三方的意思
        }
      },
    }
  },
  // ...
}

```

#### 固定 `moduleIds`

在 `webpack.config.js` 中添加

``` js
module.exports = {
  // ...
  optimization: {
    // ...
    moduleIds: 'deterministic'
  },
  // ...
}

```

### 解决 `TS` 默认不加文件后缀报错（TS 2691）

在 `TS` 中引入 `.ts` 文件不加后缀， `webpack` 会把没有后缀的文件默认解析为 `.js` ，导致打包找不到对应文件。

``` ts	
import {c} from './fileName';		// Module not found: Error: Can't resolve './fileName'
```

加了后缀， `TS`会报错

``` ts
import {c} from './fileName.ts';	// 导入路径不能以“.ts”扩展名结束。考虑改为导入“./fileName”。ts(2691)
```

在 `webpack.config.js` 中添加 `resolve.extensions` 可 `webpack` 优先解析

``` js
module.exports = {
  // ...
  resolve: {
    extensions: [".json", ".ts", ".js", "..."],  // 按顺序解析后缀名。如果有多个文件有相同的名字，但后缀名不同，webpack 会解析列在数组首位的后缀的文件 并跳过其余的后缀。 "..." 为默认拓展名
  },
  // ...
}

```







