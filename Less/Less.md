[TOC]



# { Less }

一种 动态 样式 语言.

LESS 将 CSS 赋予了动态语言的特性，如 <font color="red">变量</font>，  <font color="red">继承</font>， <font color="red">运算</font>，  <font color="red">函数</font>. LESS 既可以在  <font color="red">客户端</font> 上运行 (支持IE 6+, Webkit, Firefox)，也可以借助 <font color="red">Node.js</font>或者 <font color="red">Rhino</font>在服务端运行。



 [**点击这里 学习使用Less**]( http://www.bootcss.com/p/lesscss/ )



# 在服务器端使用

## 安装

在服务器端安装 LESS 的最简单方式就是通过 [npm](http://github.com/isaacs/npm)(node 的包管理器), 像这样:

```js
$ npm install less
```

如果你想下载最新稳定版本的 LESS，可以尝试像下面这样操作:

```js
$ npm install less@latest
```



## 使用

只要安装了 LESS，就可以在Node中像这样调用编译器:

```js
var less = require('less');

less.render('.class { width: 1 + 1 }', function (e, css) {
    console.log(css);
});
```

上面会输出

```css
.class {
  width: 2;
}
```

你也可以手动调用解析器和编译器:

```js
var parser = new(less.Parser);

parser.parse('.class { width: 1 + 1 }', function (err, tree) {
    if (err) { return console.error(err) }
    console.log(tree.toCSS());
});
```

## 配置

你可以向解析器传递参数:

```js
var parser = new(less.Parser)({
    paths: ['.', './lib'], // Specify search paths for @import directives
    filename: 'style.less' // Specify a filename, for better error messages
});

parser.parse('.class { width: 1 + 1 }', function (e, tree) {
    tree.toCSS({ compress: true }); // Minify CSS output
});
```

## 在命令行下使用

你可以在终端调用 LESS 解析器:

```js
$ lessc styles.less
```

上面的命令会将编译后的 CSS 传递给 `stdout`, 你可以将它保存到一个文件中:

```js
$ lessc styles.less > styles.css
```

如何你想将编译后的 CSS 压缩掉，那么加一个 `-x` 参数就可以了.