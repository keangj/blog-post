---
title: VuePress
date: 2018-12-17 18:03:50
tags:
---
# VuePress

## 使用
### 安装

在现有项目中
```sh
# 将 VuePress 作为一个本地依赖安装
yarn add -D vuepress # 或者：npm install -D vuepress

# 新建一个 docs 文件夹
mkdir docs

# 新建一个 markdown 文件
echo '# Hello VuePress!' > docs/README.md
```

添加 package.json 配置
```json
{
  "scripts": {
    "docs:dev": "vuepress dev docs",
    "docs:build": "vuepress build docs"
  }
}
```

命令行运行以下命令开启 dev server
```sh
yarn docs:dev # 或者：npm run docs:dev
```
### 创建目录
- 在 `docs` 中创建相应的文件目录并在其中创建 `README.md` 文件，如：
```text
.
├─ docs
│  ├─ get-start
│  │  └─ README.md
│  └─ install
│     └─ README.md
└─ package.json
```
- 在新创建的 `README.md` 文件中添加对应内容，如：

`get-start/README.md`

```markdown

---
title: 快速上手
---

# 快速上手
```


`install/README.md`
```markdown

---
title: 安装
---

# 安装
```


## 配置

### 基本配置

在 `docs` 目录中创建 `.vuepress` 目录，并在 `.vuepress` 目录中创建 `config.js` 文件
```text
.
├─ docs
│  ├─ README.md
│  ├─ .vuepress
│  │   └─ config.js
│  ├─ get-start
│  │  └─ README.md
│  └─ install
│     └─ README.md
└─ package.json
```

之后在 `.vuepress/config.js` 中添加以下配置
```js
module.exports = {
  title: 'VuePress',
  description: '这是一个使用 VuePress 创建的文档'
}
```

- 配置 nav
想要添加 nav，需要在 `.vuepress/config.js` 中配置 `themeConfig.nav`
```js
// .vuepress/config.js
module.exports = {
  themeConfig: {
    nav: [
      { text: 'Home', link: '/' },
      { text: '开始', link: '/get-started/' }
    ]
  }
}
```

- 配置 sidebar
想要添加 sidebar，需要在 `.vuepress/config.js` 中配置 `themeConfig.sidebar`
```js
// .vuepress/config.js
module.exports = {
  themeConfig: {
    sidebar: [
      '/get-start/',
      '/install'
    ]
  }
}
```
  - sidebar 的分组
  
  新建 `components/button.md` 并添加以下内容
  ```markdown
    ---
    title: button
    sidebarDepth: 2 // 设置嵌套深度
    ---
    # button
  ```
  修改配置
  ```js
    // .vuepress/config.js
    module.exports = {
      themeConfig: {
        sidebar: [
              {
                title: '入门',
                collapsable: false, // 设置默认展开
                children: [
                  '/get-started/',
                  '/install/'
                ]
              },
              {
                title: '组件',
                children: [
                  '/components/button'
                ]
              }
            ]
      }
    }
  ```
到这里基本配置就完成了
![](demo.png)
  
  