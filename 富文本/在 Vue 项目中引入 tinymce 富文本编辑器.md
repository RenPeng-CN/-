项目中原本使用的富文本编辑器是 [wangEditor](http://www.wangeditor.com/)，这是一个很轻量、简洁编辑器

但是公司的业务升级，想要一个功能更全面的编辑器，我找了好久，目前常见的编辑器有这些：

**UEditor**：百度前端的开源项目，功能强大，基于 jQuery，但已经没有再维护，而且限定了后端代码，修改起来比较费劲

**bootstrap-wysiwyg**：微型，易用，小而美，只是 Bootstrap + jQuery...

[**kindEditor**](http://kindeditor.net/demo.php)：功能强大，代码简洁，需要配置后台，而且好久没见更新了

[**wangEditor**](http://www.wangeditor.com/)：轻量、简洁、易用，但是升级到 3.x 之后，不便于定制化开发。不过作者很勤奋，广义上和我是一家人，打个call

**quill**：本身功能不多，不过可以自行扩展，api 也很好懂，如果能看懂英文的话...

**summernote**：没深入研究，UI挺漂亮，也是一款小而美的编辑器，可是我需要大的



在有这么参考的情况下，我最终还是选择了 **tinymce** 这个**不搭梯子连官网都打不开**的编辑器（简直是自讨苦吃），主要因为两点：

\1. GitHub 上星星很多，功能也齐全；

**2. 唯一一个从 word 粘贴过来还能保持绝大部分格式的编辑器；**

\3. 不需要找后端人员扫码改接口，前后端分离；

\4. 说好的两点呢！

 

**注意：这篇博客仅适用于 Tinymce 4.x**

**一、资源下载**

tinymce 官方为 vue 项目提供了一个组件 [tinymce-vue](https://github.com/tinymce/tinymce-vue)

```js
npm install @tinymce/tinymce-vue -S
```

**在 vscode、webstorm 的终端运行这段代码可能会报错，最好使用操作系统自带的命令行工具**

如果有购买 tinymce 的服务，可以参考 tinymce-vue 的说明，通过 api-key 直接使用 tinymce

像我这样没购买的，还是要老老实实下载 tinymce

```js
npm install tinymce -S
```

安装之后，在 node_modules 中找到 tinymce/skins 目录，然后将 skins 目录拷贝到 **static** 目录下

// 如果是使用 vue-cli 3.x 构建的 typescript 项目，就放到 public 目录下，文中所有 static 目录相关都这样处理

tinymce 默认是英文界面，所以还需要下载一个中文[语言包](https://www.tinymce.com/download/language-packages/)（记得搭梯子！搭梯子！搭梯子！）

然后将这个语言包放到 **static** 目录下，为了结构清晰，我包了一层 tinymce 目录

![image-20190805173043729](/Users/renpeng/Library/Application Support/typora-user-images/image-20190805173043729.png)





**二、初始化**

在页面中引入以下文件

```js
import tinymce from 'tinymce/tinymce'
import 'tinymce/themes/modern/theme'
import Editor from '@tinymce/tinymce-vue'
```

经评论区提醒，如果找不到  import 'tinymce/themes/modern/theme' 

可以替换成 import 'tinymce/themes/silver/theme' 



tinymce-vue 是一个组件，需要在 components 中注册，然后直接使用

![img](https://images2018.cnblogs.com/blog/1059788/201805/1059788-20180503160548730-1029356392.png)

```js
<Editor id="tinymce" v-model="tinymceHtml" :init="editorInit"></Editor>
```

 

这里的 init 是 tinymce 初始化配置项，后面会讲到一些关键的 api，完整 api 可以参考[官方文档](https://www.tinymce.com/docs/configure/)

编辑器需要一个 skin 才能正常工作，所以要设置一个 skin_url 指向之前复制出来的 skin 文件

```js
editorInit: {
  language_url: '/static/tinymce/zh_CN.js',
  language: 'zh_CN',
  skin_url: '/static/tinymce/skins/lightgray',
  height: 300
}
```

// vue-cli 3.x 创建的 typescript 项目，将 url 中的 static 去掉，即 skin_url: '/tinymce/skins/lightgray'

同时在 mounted 中也需要初始化一次：

![img](https://images2018.cnblogs.com/blog/1059788/201805/1059788-20180503162042555-134241217.png)

如果在这里传入上面的 init 对象，并不能生效，但什么参数都不传也会报错，所以这里传入一个空对象

 

有朋友反映这里有可能出现以下报错

 ![img](https://images2018.cnblogs.com/blog/1059788/201809/1059788-20180911175600160-486485039.png)

这是因为 init 参数地址错误，请核对下 init 参数中的几个路径是否正确

如果参数无误，可以先删除 language_url 和 language 再试

 

 **三、扩展插件**

完成了上面的初始化之后，就已经能正常运行编辑器了，但只有一些基本功能

tinymce 通过添加插件 [plugins ](https://www.tinymce.com/docs/plugins/)的方式来添加功能

比如要添加一个上传图片的功能，就需要用到 image 插件，添加超链接需要用到 link 插件

![img](https://images2018.cnblogs.com/blog/1059788/201805/1059788-20180503164638528-2050046435.png)

同时还需要在页面引入这些插件：

![img](https://images2018.cnblogs.com/blog/1059788/201805/1059788-20180503164806387-1383896359.png)

添加了插件之后，默认会在工具栏 toolbar 上添加对应的功能按钮，toolbar 也可以自定义

 ![img](https://images2018.cnblogs.com/blog/1059788/201805/1059788-20180503165245092-1811860112.png)

 

贴一下完整的组件代码：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```js
<template>
  <div class='tinymce'>
    <h1>tinymce</h1>
    <editor id='tinymce' v-model='tinymceHtml' :init='init'></editor>
    <div v-html='tinymceHtml'></div>
  </div>
</template>

<script>
import tinymce from 'tinymce/tinymce'
import 'tinymce/themes/modern/theme'
import Editor from '@tinymce/tinymce-vue'
import 'tinymce/plugins/image'
import 'tinymce/plugins/link'
import 'tinymce/plugins/code'
import 'tinymce/plugins/table'
import 'tinymce/plugins/lists'
import 'tinymce/plugins/contextmenu'
import 'tinymce/plugins/wordcount'
import 'tinymce/plugins/colorpicker'
import 'tinymce/plugins/textcolor'
export default {
  name: 'tinymce',
  data () {
    return {
      tinymceHtml: '请输入内容',
      init: {
        language_url: '/static/tinymce/zh_CN.js',
        language: 'zh_CN',
        skin_url: '/static/tinymce/skins/lightgray',
        height: 300,
        plugins: 'link lists image code table colorpicker textcolor wordcount contextmenu',
        toolbar:
          'bold italic underline strikethrough | fontsizeselect | forecolor backcolor | alignleft aligncenter alignright alignjustify | bullist numlist | outdent indent blockquote | undo redo | link unlink image code | removeformat',
        branding: false
      }
    }
  },
  mounted () {
    tinymce.init({})
  },
  components: {Editor}
}
</script>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

![img](https://images2018.cnblogs.com/blog/1059788/201805/1059788-20180503165800132-2024660865.png)

 

**四、上传图片**

tinymce 提供了 images_upload_url 等 api 让用户配置上传图片的相关参数

但为了在不麻烦后端的前提下适配自家的项目，还是得用 **images_upload_handler** 来自定义一个上传方法

![img](https://images2018.cnblogs.com/blog/1059788/201805/1059788-20180503173329751-1699470625.png)

这个方法会提供三个参数：**blobInfo, success, failure**

其中 **blobinfo** 是一个对象，包含上传文件的信息：

![img](https://images2018.cnblogs.com/blog/1059788/201805/1059788-20180503173118551-280897318.png)

**success** 和 **failure** 是函数，上传成功的时候向 success 传入一个**图片地址**，失败的时候向 failure 传入报错信息

 

贴一下我自己的上传方法，使用了 axios 发送请求

```js
handleImgUpload (blobInfo, success, failure) {
  let formdata = new FormData()
  formdata.set('upload_file', blobInfo.blob())
  axios.post('/api/upload', formdata).then(res => {
    success(res.data.data.src)
  }).catch(res => {
    failure('error')
  })
}
```


#### 安装tinymce

> npm install tinymce -S

##### 安装tinymce-vue

> npm install @tinymce/tinymce-vue -S

#### 下载中文语言包

tinymce提供了很多的语言包，这里我们下载中文语言包
地址：<https://www.tiny.cloud/get-tiny/language-packages/>

![image](https://upload-images.jianshu.io/upload_images/12200279-b13b3fce890ff13b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 下载完后放到静态文件public目录下

1、在public目录下新建tinymce，将上面下载的语言包解压到该目录
2、在node_modules里面找到tinymce,将skins目录复制到public/tinymce里面

![image](https://upload-images.jianshu.io/upload_images/12200279-b50780638cc1ea1a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### tinymce使用

##### 封装成组件
```js
<template>
    <div class="tinymce-box">
        <editor v-model="myValue"
          :init="init"
          :disabled="disabled"
          @onClick="onClick">
        </editor>
    </div>
</template>

<script>
import tinymce from 'tinymce/tinymce' //tinymce默认hidden，不引入不显示
import Editor from '@tinymce/tinymce-vue'
import 'tinymce/themes/silver'
// 编辑器插件plugins
// 更多插件参考：https://www.tiny.cloud/docs/plugins/
import 'tinymce/plugins/image'// 插入上传图片插件
import 'tinymce/plugins/media'// 插入视频插件
import 'tinymce/plugins/table'// 插入表格插件
import 'tinymce/plugins/lists'// 列表插件
import 'tinymce/plugins/wordcount'// 字数统计插件
export default {
    components:{
        Editor
    },
    name:'tinymce',
    props: {
        value: {
          type: String,
          default: ''
        },
        disabled: {
          type: Boolean,
          default: false
        },
        plugins: {
          type: [String, Array],
          default: 'lists image media table wordcount'
        },
        toolbar: {
          type: [String, Array],
          default: 'undo redo |  formatselect | bold italic forecolor backcolor | alignleft aligncenter alignright alignjustify | bullist numlist outdent indent | lists image media table | removeformat'
        }
    },
    data(){
        return{
            init: {
                language_url: '/tinymce/langs/zh_CN.js',
                language: 'zh_CN',
                skin_url: '/tinymce/skins/ui/oxide',
                // skin_url: 'tinymce/skins/ui/oxide-dark',//暗色系
                height: 300,
                plugins: this.plugins,
                toolbar: this.toolbar,
                branding: false,
                menubar: false,
                // 此处为图片上传处理函数，这个直接用了base64的图片形式上传图片，
                // 如需ajax上传可参考https://www.tiny.cloud/docs/configure/file-image-upload/#images_upload_handler
                images_upload_handler: (blobInfo, success, failure) => {
                  const img = 'data:image/jpeg;base64,' + blobInfo.base64()
                  success(img)
                }
              },
              myValue: this.value
        }
    },
    mounted () {
        tinymce.init({})
    },
    methods: {
        // 添加相关的事件，可用的事件参照文档=> https://github.com/tinymce/tinymce-vue => All available events
        // 需要什么事件可以自己增加
        onClick (e) {
          this.$emit('onClick', e, tinymce)
        },
        // 可以添加一些自己的自定义事件，如清空内容
        clear () {
          this.myValue = ''
        }
    },
    watch: {
        value (newValue) {
          this.myValue = newValue
        },
        myValue (newValue) {
          this.$emit('input', newValue)
        }
    }
}

</script>
<style scoped>

</style>
```

##### 组件引用
```vue
<template>
  <div class="home">
    <tinymce
        ref="editor"
        v-model="msg"
        :disabled="disabled"
        @onClick="onClick"
    />
    <button @click="clear">清空内容</button>
    <button @click="disabled = true">禁用</button>
  </div>
</template>
<script>
import tinymce from '@/components/tinymce.vue'

export default {
    name: 'home',
    components: {
        tinymce
    },
    data(){
        return{
            msg: 'Welcome to Use Tinymce Editor',
                    disabled: false
        }
    },
    methods: {
        // 鼠标单击的事件
        onClick (e, editor) {
            console.log('Element clicked')
            console.log(e)
            console.log(editor)
        },
        // 清空内容
        clear () {
            this.$refs.editor.clear()
        }
    }
}
</script>
```