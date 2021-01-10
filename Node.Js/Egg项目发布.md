### Egg 项目 发布流程


#### 部署准备

一、 购买服务器
    去 腾讯云 或 阿里云 等地方 购买适合的服务器

二、安全组配置
    开启 80 、443、 22、等端口 7001(egg)端口 

三、安装环境
    用 宝塔面板 (bt.cn) 来安装，根据宝塔面板的指示，在服务器安装好 宝塔面板，在 服务器端的 终端例 输入 bt default  就可以看 宝塔面板的地址了

四、域名购买

五、域名解析

六、创建网站



#### 部署后端
一、解析域名
二、上传解压
三、安装 pm2(node环境)，切换版本到最新版本
四、在服务器端，打开 服务器端的终端 命令行，切换到网站代码上传的地址的根目录下
    www/wwwroot/gpdpzs.cn 
五、如果是国内服务器，先切换镜像: 国内切换淘宝镜像，下载更快
   npm config set registry https://registry.npm.taobao.org
   如果是国外服务器就不需要了，香港也不需要
六、执行 npm install  安装全部所需插件
七、安装数据库迁移工具 npm install --save-dev sequelize-cli
八、修改配置信息
   在自己写的 egg 后台项目里配置
   config/config.default.js
   sequelize 配置
   oss配置
   
   在 database/config.json 配置数据库相关信息
九、执行迁移命令 npx sequelize db:migrate
十、npm start
十一、添加反向代理
十二、修改前端项目的 /common/lib/config.js 和 manifest.json 里面的域名即可


#### 部署前端

