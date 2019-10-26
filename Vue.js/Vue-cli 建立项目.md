
# Vue-cli 建立项目步骤

<font color='red'>Vue-cli</font>  #(<font color="blue"> command-line interface 命令行界面 </font>)

[点击在 NPM 官网查看 Vue-cli 详细使用说明](https://www.npmjs.com/package/vue-cli)

### 安装 Installation

Prerequisites: [Node.js](https://nodejs.org/en/) (>=6.x, 8.x preferred), npm version 3+ and [Git](https://git-scm.com/).

```js
//全局安装
$ npm install -g vue-cli

// MAC 电脑终端 升级 vue-cli
sudo npm install -g @vue/cli  

// windows 电脑升级 vue-cli
npm install -g @vue/cli

// 查看 vue-cli 版本
vue -V  或者 vue --version
```

### 使用 Usage

```js
// vue 初始化后，写上 模板的名字，根据不同的模板名，系统会配套相应的依赖套餐
//使用 vue-cli 时，只需要写 vue init 初始化 模板名 就可以了。 
$ vue init <template-name> <project-name>
```

###示例 Example:

```js
// cli2.XXX 的用法
$ vue init webpack my-project // 这是 cli2.XXX 的创建方式
run dev

// cli3.XXX 的用法（推荐） 直接连 git 都自动创建好了
$ vue create my-app
cd my-app // 进入创建好的项目
npm run server  // 运行此项目
vue add element // 安装 element

// 配置 vue.config.js (关闭保存代码错误提示)
module.exports = {
	lintOnSave: false
}

// 在终端，先停止服务，然后输入
vue ui // 打开项目管理的图形界面，关闭 ESlint 每次保存不在检查格式
```

The above command pulls the template from [vuejs-templates/webpack](https://github.com/vuejs-templates/webpack), prompts for some information, and generates the project at `./my-project/`.



### Vue 项目打包

```js
npm run build
```



### vue build

Use vue-cli as a zero-configuration development tool for your Vue apps and component, check out the [docs](https://github.com/vuejs/vue-cli/blob/HEAD/docs/build.md).

### 官方模板 Official Templates

The purpose of official Vue project templates are to provide opinionated, battery-included development tooling setups so that users can get started with actual app code as fast as possible. However, these templates are un-opinionated in terms of how you structure your app code and what libraries you use in addition to Vue.js.

All official project templates are repos in the [vuejs-templates organization](https://github.com/vuejs-templates). When a new template is added to the organization, you will be able to run `vue init <template-name> <project-name>` to use that template. You can also run `vue list` to see all available official templates.

Current available templates include:

- [webpack](https://github.com/vuejs-templates/webpack) - （全动能webpack模板）A full-featured Webpack + vue-loader setup with hot reload, linting, testing & css extraction.
- [webpack-simple](https://github.com/vuejs-templates/webpack-simple) -（没有热拔插 等） A simple Webpack + vue-loader setup for quick prototyping.
- [browserify](https://github.com/vuejs-templates/browserify) - (基于浏览器的)A full-featured Browserify + vueify setup with hot-reload, linting & unit testing.
- [browserify-simple](https://github.com/vuejs-templates/browserify-simple) - A simple Browserify + vueify setup for quick prototyping.
- [pwa](https://github.com/vuejs-templates/pwa) - PWA template for vue-cli based on the webpack template
- [simple](https://github.com/vuejs-templates/simple) -( 最简单的模板，用的比较少 ) The simplest possible Vue setup in a single HTML file



###Vue-cli 建立后的文件夹及文件

![屏幕快照 2019-01-31 上午11.29.13](/Users/renpeng/Library/Mobile Documents/com~apple~CloudDocs/编程笔记/img/sss.png)

![img](/Users/renpeng/Library/Mobile Documents/com~apple~CloudDocs/编程笔记/img/443.png)



![image-20190328201405907](/Users/renpeng/Library/Mobile Documents/com~apple~CloudDocs/编程笔记/img/image-20190328201405907.png)





3.启动项目

```js
npm run dev
```

如果浏览器打开之后，没有加载出页面，有可能是本地的 8080 端口被占用，需要修改一下配置文件 config里的index.js

> > ![img](/Users/renpeng/Library/Mobile Documents/com~apple~CloudDocs/编程笔记/img/596.png)
>
> 还有，如果本地调试项目时，建议将build 里的`assetsPublicPath`的路径前缀修改为 ' ./ '（开始是 ' / '），因为打包之后，外部引入 js 和 css 文件时，如果路径以 ' / ' 开头，在本地是无法找到对应文件的（服务器上没问题）。所以**如果需要在本地打开打包后的文件**，就得修改文件路径。
> 我的端口没有被占用，直接成功（服务启动成功后浏览器会默认打开一个“欢迎页面”）：
>
> > > ![img](/Users/renpeng/Library/Mobile Documents/com~apple~CloudDocs/编程笔记/img/700.png)



4.vue-cli的webpack配置分析

- 从

  ```
  package.json
  ```

  可以看到开发和生产环境的入口。

  ![img](https://upload-images.jianshu.io/upload_images/10868449-255932a94e033291.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

- 可以看到dev中的设置，**build/webpack.dev.conf.js**，该文件是开发环境中webpack的配置入口。

- 在webpack.dev.conf.js中出现**webpack.base.conf.js**，这个文件是开发环境和生产环境，甚至测试环境，这些环境的公共webpack配置。可以说，这个文件相当重要。

- 还有**config/index.js 、build/utils.js 、build/build.js**等，具体请看这篇介绍：
  [https://segmentfault.com/a/1190000008644830](https://link.jianshu.com/?t=https%3A%2F%2Fsegmentfault.com%2Fa%2F1190000008644830)

5.打包上线

注意，自己的项目文件都需要放到 src 文件夹下。
在项目开发完成之后，可以输入 `npm run build` 来进行打包工作。

```
npm run build
```

另：

```css
1.npm 开启了npm run dev以后怎么退出或关闭？
ctrl+c

2.--save-dev
自动把模块和版本号添加到模块配置文件package.json中的依赖里devdependencies部分

3. --save-dev 与 --save 的区别
--save     安装包信息将加入到dependencies（生产阶段的依赖）
--save-dev 安装包信息将加入到devDependencies（开发阶段的依赖），所以开发阶段一般使用它
```

打包完成后，会生成 dist 文件夹，如果已经修改了文件路径，可以直接打开本地文件查看。
项目上线时，只需要将 dist 文件夹放到服务器就行了。

* <font color="blue">客户端向服务器端发请求</font>，需要通过 axios 模块来实现，需要单独通过 npm 下载
* <font color="blue">首页顶头栏和底部栏</font>，需要使用 eleme 移动端组件库实现



![屏幕快照 2019-01-31 下午12.02.35](/Users/renpeng/Library/Mobile Documents/com~apple~CloudDocs/编程笔记/Vue.js/屏幕快照 2019-01-31 下午12.02.35.png)



###下载安装 MintUI 插件

下载 axios（vue里的功能组件，用来发送请求）  和  mint-ui 两个插件

```js
npm i -S axios mint-ui
```

## 引入 Mint UI

你可以引入整个 Mint UI，或是根据需要仅引入部分组件。我们先介绍如何引入完整的 Mint UI。

### 完整引入

在 main.js 中写入以下内容：

```javascript
import Vue from 'vue'
import MintUI from 'mint-ui'
import 'mint-ui/lib/style.css'
import App from './App.vue'

Vue.use(MintUI)

new Vue({
  el: '#app',
  components: { App }
})
```

## 开始使用

至此，一个基于 Vue 和 Mint UI 的开发环境已经搭建完毕，现在就可以编写代码了。启动开发模式：

```bash
npm run dev
```

编译：

```bash
npm run build
```

```js
npm run build:prod --report
```



各个组件的使用方法请参阅它们各自的文档。