# Mongoose

### Mongoose 序列自增长插件



1. 安装插件

```js
npm install --save mongoose-sequence // 安装插件，可以上 github 或 npmjs.com 去了解此插件
```



2. 设置, 在设置 某一集合 的时候引入

   ```js
   const mongoose = require('mongoose')
   const AutoIncrement = require('mongoose-sequence')(mongoose);
   ```



3. 使用

   ```js
   // 设置集合时的举例说明
   UserSchema = mongoose.Schema({
       name: String
   });
   // 你没有必要在 参数里加上  id 字段，插件会自动增加 id 字段，并每次调用时都自动增加一位数
   UserSchema.plugin(AutoIncrement, {inc_field: 'id'});
   mongoose.model('User', UserSchema);
   ```

   