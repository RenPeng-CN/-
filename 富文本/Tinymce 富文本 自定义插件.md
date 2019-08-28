# TinyMCE 富文本编辑器 ━━ 自定义插件 [原创]

2019年8月7日，好不容易在富文本自定义插件成功，记录已被后用



### 第一步、创建文件夹

1. 在 vue 中引入 TinyMCE 富文本，并安装好后，在 node_modules 文件夹下，找到 tinymce 文件夹。

2. 打开后，再打开 plugins 文件夹，在 plugins 文件夹下 建立要 自定义 的插件名称一样的文件夹 imgselector



###第二步、建立插件文件，注册工具栏按钮

1. 在 imgselector 文件夹下 建立 index.js 文件，输入内容



index.js

```js
require('./plugin.js'); // 引入同目录下的 plugin.js 文件
```





2. 在 imgselector 文件夹下，再建立 plugin.js 文件，输入内容



plugin.js

```js
tinymce.PluginManager.add('imgselector', function(editor, url) {
    
    // 注册一个工具栏按钮名称
    editor.ui.registry.addButton('imgselector', {
    //   text: '工具栏按钮名',
      icon: 'image',
      tooltip:"从图片库选择图片",
      onAction: function () { // 当点击按钮时，执行方法内程序
        
        // 注意: imageSelectorCallback 方法为自定方法，需要在 TinyMCE 初始化时注册，否则报错       
        editor.settings.imageSelectorCallback(function(URL){   
          
          // 插入图片到富文本编辑器
            editor.insertContent( '<br /><img width="680" src="' + URL + '"/><br />'); 
        });
      }
    });
});
```



###第三步、在调用 TinyMCE 富文本的页面



调用的页面.vue

```js
 // 1. 导入自定义的模块
import 'tinymce/plugins/imgselector'  

// 2. // TinyMCE 初始化
      editorInit: {
        language_url: '/tinymce/zh_CN.js', // 不能加 public 位置信息，系统会自动找到
        language: 'zh_CN',
        statusbar: false,
        image_caption: true,
         
// 5. ***这里是 重点、重点、重点 ****  
        // 将 plugin.js 里 自定义的方法 imageSelectorCallback 在这里注册，
        // 并绑定一个方法 this.showImageSelector 
// 神奇的是 this.showImageSelector(example) 中的 example 形式参数返回的
// 是 plugin.js 里自定义的方法 imageSelectorCallback 里的内容
// 也就是：editor.insertContent( '<br /><img width="680" src="' + r + '"/><br />');
        imageSelectorCallback: this.showImageSelector, // 点击编辑器上的图片按钮后的回调方法
// *********上面这行是重点***********
          
        skin_url: '/tinymce/skins/ui/oxide', // 不能加 public 位置信息，系统会自动找到
        height: 600, // 设定 富文本 高度
      
 // 3. 在插件 plugins 里 加入 imgselector 插件
        plugins: 'link lists advlist image code table wordcount emoticons emoticons' +
                 'autosave charmap codesample fullscreen hr insertdatetime media nonbreaking noneditable pagebreak searchreplace ' +
                 'wordcount imgselector',

// 4. 在 工具条里 加入 imgselector 插件 ,应该可以看到 工具条上出现了自定义的插件按钮
        toolbar: 'bold italic underline strikethrough | fontsizeselect | forecolor backcolor ' +
                 ' | alignleft aligncenter alignright alignjustify | bullist numlist | outdent indent blockquote searchreplace' +
                 '| undo redo | link unlink image media nonbreaking code | pagebreak hr removeformat |charmap emoticons  codesample ' +
                 'insertdatetime | wordcount fullscreen imgselector'
      }



// 6. 建立 方法 showImageSelector，在括号里 引入形参 function，点击 工具条上自定义的按钮后，就会执行此方法
// 建立一个 变量 imageSelectedCallback ，用来接收 plugin.js 传来的方法  
showImageSelector(function) {
      this.imgDialogShow.visible = true // 打开自己开发的图片选择器窗口，进行选择多个图片
      this.imageSelectedCallback = function // 接收来自 plugin.js 传来的 方法
},
  
  
// 7. 建立函数 insertImage，自定义的图片选择器选好图片后，点击确定，将值传给形参 selectedItems, 是选好的图片url地址数组
  insertImage(selectedItems) {
      if (selectedItems.length === 0) { return }

      for (let i = 0; i < selectedItems.length; i++) { // 循环数组,将所有图片插入富文本
        this.imageSelectedCallback(selectedItems[i]) // 用传来的方法，把图片地址输入进去，插入富文本中
      }
    }
```

完成





### 以下是别人写的内容，Tinymce 的版本比较老，仅供参考

```js
var imageSelector;
    var imageSelectedCallback = null;
    function showImageSelector(cb){
        imageSelectedCallback = cb;
        imageSelector = new ImageSelector('#imageSelectorDiv', {}, function(type, data){  // 初始化图片选择弹窗
        });
        $('#imageSelectorPop').modal({keyboard: true, backdrop: 'static'});
    }

    function insertImage(){
        if(imageSelector.selectedItems.length == 0)
            return ;

        imageSelectedCallback(imageSelector.selectedItems[0].url);   // 调用插件内部回调把图片插入到编辑器中
        $('#imageSelectorPop').modal('hide');
    }

tinymce.init({
            selector: '.richEditor',
            width: 820,
            height: 200,
            plugins: [
                'advlist autolink lists link imageSelector hr',
                'visualblocks visualchars code',
                'textcolor colorpicker textpattern'
            ],
            toolbar: 'styleselect | forecolor backcolor | bold italic | alignleft aligncenter alignright alignjustify | bullist numlist outdent indent | link imageSelector',
            imageSelectorCallback: showImageSelector,  // 点击编辑器上的图片按钮后的回调方法
            setup: function(editor) {                  
                editor.on('init', function(e) {        // tinyMCE初始化完成事件
                    tinyMCE.editors[0].setContent('<%-topic.content%>');  // 设置之前编辑的内容
                });
            }
        });
```
































