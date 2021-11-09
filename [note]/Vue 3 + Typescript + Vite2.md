---
title: Vue 3 + Typescript + Vite2
tags: [Vite, Vue, Typescript]
date: 2021-08-10
categories: 
  - ['Vite']
  - ['Vue']
  - ['Typescript']
cover: https://cn.vitejs.dev/logo.svg
---

## 初始化项目

``` sh
yarn create vite <project name> --template vue-ts
```

or

``` sh
yarn create vite
```



## 配置别名

安装 node TS 声明文件

``` sh
yarn add --dev @types/node
```

修改 `vite.config.ts` 

``` ts
// ...
import path from 'path'	// 此语法有可能导致 idea 报错 TS1259: Module '"path"' can only be default-imported using the 'esModuleInterop' flag, 改成 import path = require('path') 可解决报错

// https://vitejs.dev/config/
export default defineConfig({
  resolve: {
    alias: {
      "src": path.resolve(__dirname, "src"),	// 配置根目录别名
      "comps": path.resolve(__dirname, "src/components"),	// 配置组件目录别名
    }
  },
  // ...
})
```



## 路由

安装依赖

``` sh
yarn add --dev vue-router@next
```

添加 `views/home.vue`

``` vue
<template></template>

<script lang="ts">
import { defineComponent } from 'vue'
export default defineComponent({
  name: 'HelloWorld'
})
</script>
```



创建 `src/router/index.ts`

``` ts
import { createRouter, createWebHashHistory, RouteRecordRaw } from 'vue-router'
import Home from 'src/views/home.vue'

const routes: Array<RouteRecordRaw> = [
  {
    path: '/',
    name: 'Home',
    component: Home
  }
]

const router = createRouter({
  history: createWebHashHistory(),
  routes
});

export default router
```

修改 `main.ts` 添加 `router` 配置

``` ts
// ...
import router from "./router"

createApp(App).use(router).mount('#app')
```



修改 `App.vue` 

``` vue
<template>
  <router-view></router-view>
</template>
// ...
```



## 状态管理

安装依赖

``` sh
yarn add vuex@next --save
```

创建 `store/index.ts`

``` ts
import {createStore} from 'vuex';

export const store = createStore({
  state: {}
});
```

修改 `main.ts` 添加 `router` 配置

``` ts
// ...
import { store } from './store'

createApp(App).use(router).use(store).mount('#app')
```



## SASS

安装依赖

``` sh
yarn add --dev sass
```

添加 `styles/index.scss styles/common.scss`

``` scss
// styles/index.scss
@import url(./common.scss);
```

修改 `main.ts` 引入

``` ts
// ...
import './assets/styles/index.scss'
// ...
```



## jsx/tsx

安装插件

``` sh
yarn add --dev @vitejs/plugin-vue-jsx
```

修改 `vite.config.ts`

``` ts
// ...
import vueJsx from '@vitejs/plugin-vue-jsx'

export default defineConfig({
  plugins: [vue(), vueJsx()],
  // ...
})
```

修改 `views/home.vue` 

``` tsx
<script lang="tsx">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'Home',
  render: () => (
      <>
        hello, world
      </>
  )
})
</script>
```



## 代码规范

配置 `.editorconfig`

在项目根目录创建 `.editorconfig`

```yaml
# Editor configuration, see http://editorconfig.org

# 表示是最顶层的 EditorConfig 配置文件
root = true

[*] # 表示所有文件适用
charset = utf-8 # 设置文件字符集为 utf-8
indent_style = space # 缩进风格（tab | space）
indent_size = 2 # 缩进大小
end_of_line = lf # 控制换行类型(lf | cr | crlf)
trim_trailing_whitespace = true # 去除行首的任意空白字符
insert_final_newline = true # 始终在文件末尾插入一个新行

[*.md] # 表示仅 md 文件适用以下规则
max_line_length = off
trim_trailing_whitespace = false
```

配置 `eslint`

安装依赖

``` sh
yarn add --dev prettier eslint
```

### 配置 `prettier`

在项目目录添加 `.prettierrc`

``` json
{
  "useTabs": false,
  "tabWidth": 2,
  "printWidth": 80,
  "singleQuote": true,
  "trailingComma": "none",
  "bracketSpacing": true,
  "semi": false
}
```

修改 `package.json`

``` json
// ...
"scripts": {
  // ...
  "format": "prettier --write ."
},
// ...
```

### 配置 `ESlint`

创建 `.eslintrc.js` 文件

``` sh
npx eslint --init
```

### 使用 husky

``` sh
npx husky-init && yarn
```

在 `package.json` 添加

``` json
"scripts": {
  // ...
  "lint:code": "eslint --fix ./src --ext .vue,.js,.jsx,.ts,.tsx"
},
```

修改 `.husky/pre-commit`

``` sh
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

yarn lint:code
```

### lint-staged

``` sh
yarn add --dev lint-staged
```

在 `package.json` 添加

``` json
"lint-staged": {
  "*.{js,jsx,ts,tsx}": [
    "prettier --write",
    "eslint --fix"
  ],
  "*.vue": [
    "prettier --parser=vue --write",
    "eslint --fix"
  ]
},
```



## jest

安装依赖

``` sh
yarn add --dev jest  @types/jest vue-jest@next @vue/test-utils@next
yarn add --dev ts-jest @babel/preset-env @babel/preset-typescript
```

添加 `jest.config.ts`

```sh
npx jest --init
√ Would you like to use Typescript for the configuration file? ... yes
√ Choose the test environment that will be used for testing » jsdom (browser-like)
√ Do you want Jest to add coverage reports? ... yes
√ Which provider should be used to instrument code for coverage? » babel
√ Automatically clear mock calls and instances between every test? ... yes
```

修改 `jest.config.ts`

``` ts
module.exports = {
  testEnvironment: "jsdom",
  transform: {
    "^.+\\.vue$": "vue-jest",
    '^.+\\.(j|t)sx?$': [
      'babel-jest',
      {
        presets: [
          ['@babel/preset-env', { targets: { node: 'current' } }],
          ['@babel/preset-typescript']
        ],
        plugins: ['@vue/babel-plugin-jsx']
      }
    ]
  },
  moduleFileExtensions: ['vue', 'js', 'jsx', 'ts', 'tsx', 'json', 'node'],
  testMatch: ["**/tests/**/*.spec.(js|ts)", "**/__tests__/**/*.spec.(js|ts)"],
  testPathIgnorePatterns: ['/node_modules/']
}
```



### 解决报错

####  `TypeError: Cannot destructure property 'config' of 'undefined' as it is undefined.`

将 jest 和 ts-jest(babel-jest)版本从 27 改为 26

``` sh
yarn add --dev ts-jest@26.5.6 jest@26.6.3
```

#### `TS7016: Could not find a declaration file for module '@vue/test-utils'.`

添加 `<file name>.d.ts`

``` ts
declare module '@vue/test-utils';
```



## 提交规范

### 使用 `commitizen`
安装 `commitizen`

``` sh
yarn global add commitizen
```

初始化 `commitizen`

安装并初始化 `commitizen`

``` sh
npx commitizen init cz-conventional-changelog --yarn --dev --exact
```

自定义配置

``` sh
npx commitizen init cz-customizable --yarn --dev --exact --force
```

在项目根目录添加 `.cz-config.js`

``` js
module.exports = {
  // type 类型（定义之后，可通过上下键选择）
  types: [
    { value: 'feat', name: 'feat:     新增功能' },
    { value: 'fix', name: 'fix:      Bug修复' },
    {
      value: 'docs',
      name: 'docs:     文档类的更新，包括修改用户文档或者开发文档等'
    },
    {
      value: 'style',
      name: 'style:    代码格式（不影响功能，例如空格、分号等格式修正）'
    },
    {
      value: 'refactor',
      name: 'refactor: 代码重构（不包括 feat fix perf style）'
    },
    { value: 'perf', name: 'perf:     提高代码性能的变更' },
    { value: 'test', name: 'test:     添加、修改测试用例' },
    {
      value: 'build',
      name: 'build:    构建流程、外部依赖变更（如升级 npm 包、修改 webpack 配置等）'
    },
    { value: 'ci', name: 'ci:       修改 CI 配置、脚本' },
    {
      value: 'chore',
      name: 'chore:    对构建过程或辅助工具和库的更改（不影响源文件、测试用例）'
    },
    { value: 'revert', name: 'revert:   回滚 commit' }
  ],

  // scope 类型（定义之后，可通过上下键选择）
  scopes: [
    ['components', '组件相关'],
    ['hooks', 'hook 相关'],
    ['utils', 'utils 相关'],
    // ['element-ui', '对 element-ui 的调整'],
    ['styles', '样式相关'],
    ['deps', '项目依赖'],
    ['auth', '对 auth 修改'],
    ['other', '其他修改'],
    // 如果选择 custom，后面会让你再输入一个自定义的 scope。也可以不设置此项，把后面的 allowCustomScopes 设置为 true
    ['custom', '以上都不是？我要自定义']
  ].map(([value, description]) => {
    return {
      value,
      name: `${value.padEnd(30)} (${description})`
    }
  }),

  // 是否允许自定义填写 scope，在 scope 选择的时候，会有 empty 和 custom 可以选择。
  // allowCustomScopes: true,

  // allowTicketNumber: false,
  // isTicketNumberRequired: false,
  // ticketNumberPrefix: 'TICKET-',
  // ticketNumberRegExp: '\\d{1,5}',

  // 针对每一个 type 去定义对应的 scopes，例如 fix
  /*
  scopeOverrides: {
    fix: [
      { name: 'merge' },
      { name: 'style' },
      { name: 'e2eTest' },
      { name: 'unitTest' }
    ]
  },
  */

  // 交互提示信息
  messages: {
    type: '确保本次提交遵循 Angular 规范！\n选择你要提交的类型：',
    scope: '\n选择一个 scope（可选）：',
    // 选择 scope: custom 时会出下面的提示
    customScope: '请输入自定义的 scope：',
    subject: '填写简短精炼的变更描述：\n',
    body: '填写更加详细的变更描述（可选）。使用 "|"  换行：\n',
    breaking: '列举非兼容性重大的变更（可选）：\n',
    footer: '列举出所有变更的 ISSUES CLOSED（可选）。 例如: #31, #34：\n',
    confirmCommit: '确认提交？'
  },

  // 设置只有 type 选择了 feat 或 fix，才询问 breaking message
  allowBreakingChanges: ['feat', 'fix'],

  // 跳过要询问的步骤
  // skipQuestions: ['body', 'footer'],

  // subject 限制长度
  subjectLimit: 100,
  breaklineChar: '|' // 支持 body 和 footer
  // footerPrefix : 'ISSUES CLOSED:'
  // askForBreakingChangeFirst : true,
}
```

### 使用 commitlint 验证 commit

``` sh
yarn add --dev @commitlint/config-conventional @commitlint/cli
```

创建 `commitlint.config.js`

``` ts
module.exports = { extends: ['@commitlint/config-conventional'] }
```

使用以下命令生成 commit-msg

``` sh
npx husky add .husky/commit-msg "npx --no-install commitlint --edit $1"
```

使用 `commitizen` 提交代码

``` sh
git cz
```

## Axios

安装 axios

``` sh
yarn add axios
```

在根目录添加配置文件 `.env.development`

``` 
VITE_BASE_API=/api
```

添加 `env.d.ts` 声明文件

``` ts
// eslint-disable-next-line no-unused-vars
interface ImportMetaEnv {
  VITE_BASE_API: string
}
```

创建 `src/utils/request.ts`

``` ts
import Axios from 'axios'

const baseURL: string = import.meta.env.VITE_BASE_API

const axios = Axios.create({
  baseURL,
  timeout: 5000
})

// 前置拦截器（发起请求之前的拦截）
axios.interceptors.request.use(
  (config) => {
    /**
     * 根据你的项目实际情况来对 config 做处理
     * 这里对 config 不做任何处理，直接返回
     */
    // config.headers['X-Token'] = 'my token'
    return config
  },
  (error) => {
    return Promise.reject(error)
  }
)

// 后置拦截器（获取到响应时的拦截）
axios.interceptors.response.use(
  (response) => {
    /**
     * 根据你的项目实际情况来对 response 和 error 做处理
     * 这里对 response 和 error 不做任何处理，直接返回
     */
    return response
  },
  (error) => {
    if (error.response && error.response.data) {
      const code = error.response.status
      const msg = error.response.data.message
      console.error(`Code: ${code}, Message: ${msg}`)
      console.error('[Axios Error]', error.response)
    } else {
      console.error(`${error}`)
    }
    return Promise.reject(error)
  }
)

export default axios
```

## 完整项目

[项目地址](https://github.com/keangj/vue3-vite-demo)


## 参考

- [1] [备战2021：vite工程化实践，建议收藏](https://juejin.cn/post/6910014283707318279#heading-9)
- [2] [从 0 开始手把手带你搭建一套规范的 Vue3.x 项目工程环境](https://juejin.cn/post/6951649464637636622#heading-8)