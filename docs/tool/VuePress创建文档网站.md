# VuePress 创建文档网站

## VuePress 介绍

简单介绍一下，[VuePress](https://vuepress.vuejs.org/zh/guide/) 是尤雨溪 2018 年 04 月 12 日发布的 Vue 静态网站生成器，支持 Vue 语法`，内置 `webpack`，每一个由 `VuePress` 生成的页面都是通过 SSR ` 预渲染的 HTML，也因此具有非常好的加载性能和搜索引擎优化。

个人觉得最大的亮点就是：

1. markdown 文件可以内嵌 Vue 组件
2. 借助 YAML 来作为驱动和配置文档



## 安装

> 在已有项目中安装

如果你想要在一个已有项目中维护文档，就应该将 VuePress 安装为本地依赖。此设置还允许你使用 CI 或 Netlify 服务，在推送时自动部署。

### 安装为本地依赖项

```sh
# 安装为本地依赖项
npm install -D vuepress
```

### 初始化项目

```sh
# 可以使用 npm 来初始化项目,会生成 package.json
npm init
```

然后在项目的根目录下新建一个 `docs` 文件夹，以后我们写的 `markdown` 文件都会放在 `docs` 文件夹下。

```sh
# 创建一个 docs 目录
mkdir docs
# 创建一个 markdown 文件
echo # Hello VuePress > docs/README.md
```

执行本地服务器启动命令：

```sh
vuepress dev docs
```

就可以看到启动了一个页面： 

![image-20200426193547122](https://gitee.com/wugenqiang/PictureBed/raw/master/CS-Notes/20200426193548.png)

为了后续运行方便，我们可以把这些命令写在项目的 `package.json` 文件里面的 `scripts`：

```json
{
  "scripts": {
    "dev": "vuepress dev docs",
    "build": "vuepress build docs",
    "deploy": "npm run build && gh-pages -d docs/.vuepress/dist"
  }
}
```

然后你就可以开始编写文档了：

执行本地服务器启动命令：

```bash
yarn dev # 或者：npm run dev
```



要生成静态资源（HTML 文件），请运行：

```bash
yarn build # 或者：npm run build
```

默认情况下，构建的文件会位于 `.vuepress/dist` 中，该文件可以通过 `.vuepress/config.js` 中的 `dest` 字段进行配置。构建的文件可以部署到任何静态文件服务器。

## 目录结构

VuePress 遵循 **“约定优于配置”** 的原则，推荐的目录结构如下：

```bash
.
├── docs
│   ├── .vuepress (可选的)
│   │   ├── components (可选的)
│   │   ├── theme (可选的)
│   │   │   └── Layout.vue
│   │   ├── public (可选的)
│   │   ├── styles (可选的)
│   │   │   ├── index.styl
│   │   │   └── palette.styl
│   │   ├── templates (可选的, 谨慎配置)
│   │   │   ├── dev.html
│   │   │   └── ssr.html
│   │   ├── config.js (可选的)
│   │   └── enhanceApp.js (可选的)
│   │ 
│   ├── README.md
│   ├── guide
│   │   └── README.md
│   └── config.md
│ 
└── package.json
```

- `docs/.vuepress`: 用于存放全局的配置、组件、静态资源等。
- `docs/.vuepress/components`: 该目录中的 Vue 组件将会被自动注册为全局组件。
- `docs/.vuepress/theme`: 用于存放本地主题。
- `docs/.vuepress/styles`: 用于存放样式相关的文件。
- `docs/.vuepress/styles/index.styl`: 将会被自动应用的全局样式文件，会生成在最终的 CSS 文件结尾，具有比默认样式更高的优先级。
- `docs/.vuepress/styles/palette.styl`: 用于重写默认颜色常量，或者设置新的 stylus 颜色常量。
- `docs/.vuepress/public`: 静态资源目录。
- `docs/.vuepress/templates`: 存储 HTML 模板文件。
- `docs/.vuepress/templates/dev.html`: 用于开发环境的 HTML 模板文件。
- `docs/.vuepress/templates/ssr.html`: 构建时基于 Vue SSR 的 HTML 模板文件。
- `docs/.vuepress/config.js`: 配置文件的入口文件，也可以是 `YML` 或 `toml`。
- `docs/.vuepress/enhanceApp.js`: 客户端应用的增强。

## 基本配置

目前我们只写了一个 `markdown` 文档，所以只有一个页面，后续我们的博客会陆续加入很多内容，肯定需要做目录分级，配置导航栏，可以看[文档里的这部分](https://vuepress.vuejs.org/zh/theme/default-theme-config.html#首页)

### 配置文件

如果没有任何配置，这个网站将会是非常局限的，用户也无法在你的网站上自由导航。为了更好地自定义你的网站，让我们首先在你的文档目录下创建一个 `.vuepress` 目录，所有 VuePress 相关的文件都将会被放在这里。你的项目结构可能是这样：

```bash
.
├─ docs
│  ├─ README.md
│  └─ .vuepress
│     └─ config.js
└─ package.json
```

在  .vuepress 目录下新建一个 config.js, 他导出一个对象

一些配置可以参考官方文档, 这里我配置常用及必须配置的

### 网站信息

```js
module.exports = {
    title: 'VuePress 文档 demo',
    description: 'Document library',
    head: [
        ['link', { rel: 'icon', href: `https://gitee.com/wugenqiang/PictureBed/raw/master/CS-Notes/20200425141925.ico` }],
    ],
}
```

### 导航栏配置

```js
module.exports = {
    themeConfig: {
        nav: [
            {text: '主页', link: '/'},
            {text: '前端规范', link: '/frontEnd/'},
            {text: '开发环境', link: '/development/'},
            {text: '学习文档', link: '/notes/'},
            // 下拉列表的配置
            {
                text: 'Languages',
                items: [
                    {text: 'Chinese', link: '/language/chinese'},
                    {text: 'English', link: '/language/English'}
                ]
            }
        ]
    }
}

```



### 侧边栏配置

可以省略 .md 扩展名,同时以 / 结尾的路径将会被视为 */README.md

```js
module.exports = {
  themeConfig: {
    sidebar: {
      '/frontEnd/': genSidebarConfig('前端开发规范'),
    }
  }
}
```

上面封装的 genSidebarConfig 函数

```js
function genSidebarConfig(title) {
  return [{
    title,
    collapsable: false,
    children: [
      '',
      'html-standard',
      'css-standard',
      'js-standard',
      'git-standard'
    ]
  }]
}
```

支持侧边栏分组(可以用来做博客文章分类) collapsable是当前分组是否展开

```js
module.exports = {
  themeConfig: {
    sidebar: {
      '/note': [
        {
          title:'前端',
          collapsable: true,
          children:[
            '/notes/frontEnd/VueJS组件编码规范',
            '/notes/frontEnd/vue-cli脚手架快速搭建项目',
            '/notes/frontEnd/深入理解vue中的slot与slot-scope',
            '/notes/frontEnd/webpack入门',
            '/notes/frontEnd/PWA介绍及快速上手搭建一个PWA应用',
          ]
        },
        {
          title:'后端',
          collapsable: true,
          children:[
            'notes/backEnd/nginx入门',
            'notes/backEnd/CentOS如何挂载磁盘',
          ]
        },
      ]
    }
  }
}
```





## 部署

下述的指南基于以下条件：

- 文档放置在项目的 `docs` 目录中；
- 使用的是默认的构建输出位置；
- VuePress 以本地依赖的形式被安装到你的项目中，并且配置了如下的 npm scripts:

```json
{
  "scripts": {
    "deploy": "npm run build && gh-pages -d docs/.vuepress/dist"
  }
}
```

1. 在 `docs/.vuepress/config.js` 中设置正确的 `base`。

![image-20200426182800772](https://gitee.com/wugenqiang/PictureBed/raw/master/CS-Notes/20200426182802.png)

```js
module.exports = {
  base: '/vuepress-demo/',
}
```



2. 在你的项目中，创建一个如下的 `deploy.sh` 文件（根据实际情况修改）:

```bash
#!/usr/bin/env sh

# 确保脚本抛出遇到的错误
set -e

# 生成静态文件
npm run build

# 进入生成的文件夹
cd docs/.vuepress/dist

# 如果是发布到自定义域名
# echo 'www.example.com' > CNAME

git init
git add -A
git commit -m 'deploy'

# 如果发布到 https://<USERNAME>.github.io
# git push -f git@github.com:<USERNAME>/<USERNAME>.github.io.git master

# 如果发布到 https://<USERNAME>.github.io/<REPO>
# git push -f git@github.com:<USERNAME>/<REPO>.git master:gh-pages

cd -
```

## GitHub Actions 自动构建/部署

大家有注意到 GitHub 悄悄上线了一个 Actions 功能吗？还不了解的同学可以看[这篇文章](https://zhuanlan.zhihu.com/p/77751445)，写的非常全面。

> GitHub Actions 是什么
>
> GitHub 官方号称 Actions 可以让你的**工作流自动化**：GitHub 监听某个事件（可能是某个分支的提交），然后触发你预定义的工作流，让大家在GitHub服务器上直接测试代码、部署代码。所以，我们可以利用这里特性来做 CI/CD，开发者只要写一下 workflow 脚本就可以了，不用费心思去想要用哪个第三方的 CI/CD 服务, 💯。

actions 其实就是由一些脚本组成，所以它们是可以复用的，GitHub 做了一个[官方市场](https://github.com/marketplace?type=actions)，可以搜索到他人提交的 actions。另外，还有一个 [awesome actions](https://github.com/sdras/awesome-actions) 的仓库，也可以找到不少 action。这样一来，你甚至都不用自己写具体的脚本，直接引用别人的脚本就行啦。

话不多说，赶紧用起来！

### 写 workflow 脚本

首先我们需要到项目仓库的页面上进入 Actions 这个 tab, 选择 Node 环境进入脚本的编辑页面

![image-20200426201144851](https://gitee.com/wugenqiang/PictureBed/raw/master/CS-Notes/20200426201146.png)

这里我直接使用了 peaceiris 的 [`actions-gh-pages`](https://github.com/peaceiris/actions-gh-pages)，这个 `action` 可以帮你把打包好的静态文件部署到 `GitHub Pages` 上去。

最终我的 workflow 脚本如下： 

```bash
# This workflow will do a clean install of node dependencies, build the source code and run tests across different versions of node
# For more information see: https://help.github.com/actions/language-and-framework-guides/using-nodejs-with-github-actions

name: vuepress-demo
on:
  push:
    branches:
    - master

jobs:
  build-deploy:
    runs-on: ubuntu-18.04
    steps:
    - uses: actions/checkout@master

    # - name: Setup Node
    #   uses: actions/setup-node@v1
    #   with:
    #     node-version: '10.x'

    # - name: Cache dependencies
    #   uses: actions/cache@v1
    #   with:
    #     path: ~/.npm
    #     key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
    #     restore-keys: |
    #       ${{ runner.os }}-node-

    - run: npm ci

    - run: npm run build

    - name: Deploy
      uses: peaceiris/actions-gh-pages@v3
      env:
        ACTIONS_DEPLOY_KEY: ${{ secrets.ACCESS_TOKEN }}
        PUBLISH_BRANCH: gh-pages
        PUBLISH_DIR: docs/.vuepress/dist
```

注意

因为我用的 action 是第三方的，所以 action 可能会经常更改，如果你是过了一段时间才看到这篇文章，peaceiris 的 [`actions-gh-pages`](https://github.com/peaceiris/actions-gh-pages) 很可能已经发生了更新，所以脚本的内容建议直接参照它的官方文档来写。

更详细的语法可以去看 [GitHub Actions 的官方文档](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/events-that-trigger-workflows)

注意

### 设置 workflow 的环境变量

因为我们需要 GitHub Actions 把构建成果发到 GitHub 仓库，因此需要 GitHub 密钥，相当于是给 GitHub actions 授权。

首先运行下面的命令生成一对密钥：

```bash
ssh-keygen -t rsa -b 4096 -C "$(git config user.email)" -f gh-pages -N ""
# You will get 2 files:
#   gh-pages.pub (public key)
#   gh-pages     (private key)
```

像这样：

![image-20200426202933017](https://gitee.com/wugenqiang/PictureBed/raw/master/CS-Notes/20200426202935.png)

密钥在这：

![image-20200426203943738](https://gitee.com/wugenqiang/PictureBed/raw/master/CS-Notes/20200426203944.png)

然后：

1. 在博客项目的仓库的 Settings 栏下，找到 `Deploy keys`这一项，把你的公钥加进去，**注意勾选`Allow write access`**
2. 同样在博客项目的仓库的 Settings 栏下，找到 `Secrets`这一项，把你的私钥加进去

![image-20200426203453041](https://gitee.com/wugenqiang/PictureBed/raw/master/CS-Notes/20200426203454.png)

![image-20200426203753813](https://gitee.com/wugenqiang/PictureBed/raw/master/CS-Notes/20200426203755.png)

如图显示即成功：

![image-20200426203837992](https://gitee.com/wugenqiang/PictureBed/raw/master/CS-Notes/20200426203839.png)

![image-20200426203823746](https://gitee.com/wugenqiang/PictureBed/raw/master/CS-Notes/20200426203824.png)

