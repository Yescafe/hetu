### 环境搭建过程记录

1. 安装 mysql-installer-community-5.7.40.0.msi，默认 Developer Default，密码 12345678，不需要建额外的用户，接着一路默认，Connect to Server 测试密码。
2. 安装 node-v15.14.0-x64.msi，全部默认。
3. 安装 yarn-1.22.19.msi，全部默认。
4. MySQL 里建个数据库，这里叫 hetu_dev，然后执行 server/open_hetu.sql。
5. 邮箱是我提前就弄好的，但是又没弄好，不过无所谓。
6. 项目根目录新建一个 system_config.ini：
   ```ini
   [server]
   port = 9536

   [mysql]
   host = 127.0.0.1
   port = 3306
   user = root
   password = 12345678
   database = hetu_dev

   [email]
   host = smtp.163.com
   port = 364
   user = xxxxxxxxx@163.com
   pass = xxxxx

   [client]
   cdn_host = 127.0.0.1
   ```
7. 用 npm 安装 yarn 跳过，因为我自己装了。
8. 开三个 powershell：
   1. 第一个进 /plugin，执行 `yarn`，我这显示有 1501 个依赖要 fetch，最后用了 182s。然后打包 esm，用 `yarn run build:umd`，花了 74s。完成后运行组件库 `yarn run server:dist`，访问 <http://127.0.0.1:8080/manifest.json> 看看端口能否成功返回这个 json。
   2. 第二个进 /client，执行 `yarn`，有 1643 个要 fetch，最后用了 100s。然后先不启动。
   3. 第三个进 /server，执行 `yarn`，有 629 个要 fetch，用了 29s。
9. 按顺序启动 client 和 server。先在 /client 里 `yarn start`，再在 /server 里 `yarn dev`。
10. 然后要等待 client 启动完成，会显示 `Files successfully emitted`。


<p align="center">
<img src="https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a9202d4d9e234092881b110c5bfeee7c~tplv-k3u1fbpfcp-zoom-1.image" width="60" height="60"/>
<h1>河图</h1>
</p>

河图, 是一个 `低代码` 平台, 通过可视化界面, 快速生成各种后台页面, 极大减少开发成本。

河图是贝壳找房内部孵化项目, 目前已在公司大多数业务线落地, 完成200+项目, 1500+页面。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c879472eac3a4e65806f7b18e188d112~tplv-k3u1fbpfcp-zoom-1.image)

## ✨ 特性
- 🚴‍♀️ 操作简单、功能强大的可视化编辑器
- 📦 开箱即用、高质量后台管理系统模版
- ⚙️ 开发流程全部线上化，节省沟通、调试、运维成本
- 🛡 使用 React、TypeScript、nodejs、express 开发

## 🖥 兼容环境  
- 现代浏览器、IE11以上

## 🔗 链接  
- [项目文档](http://139.155.239.172/)
- [项目首页](http://139.155.239.172:9536/)
- [服务器部署](/deploy.md)

## 🍼 准备
### 1. 一个邮箱账号  
用于发送验证码, 需要 [开启SMTP服务](https://www.yuque.com/pengyuanyuan-hqdma/ks1r1a/xs7xmn)  

### 2. MySQL数据库 
> 本项目使用mysql 5.7版本
  - 方式1: [手动部署MySQL数据库](https://www.yuque.com/pengyuanyuan-hqdma/ks1r1a/vsw0o9)  
  - 方式2: 购买MySQL云服务  

### 3. 初始化数据库   
将 [server/open_hetu.sql](/LianjiaTech/hetu/blob/master/server/open_hetu.sql) 文件, 通过mysql Gui工具, 导入到数据库  

### 4. 创建配置文件  
克隆项目, 在项目根目录下创建`system_config.ini`文件, 配置内容如下(将****替换为自己的配置, 去掉注释内容)  
```ini
[server]
port = 9536         // node服务启动端口

[mysql]             // mysql配置
host = ****
port = ****
user = ****
password = ****
database = ****

[email]
host = smtp.163.com // SMTP服务域名
port = 364          // 连接端口
user = ****         // 公共邮箱账号
pass = ****         // 授权密码
```
> 如果项目报`host port`未找到, 可尝试将配置写死在项目中

### 5. 全局安装yarn
```
npm install -g yarn
```

## 🚀 开始
### 第一步 启动组件库服务
```
# 进入plugin目录
cd ./plugin

# 安装依赖
yarn

# 打包输出esm模块
yarn run build:umd

# 启动组件库服务
yarn run server:dist
```
打开 [http://127.0.0.1:8080/manifest.json](http://127.0.0.1:8080/manifest.json) 预览, 可以看到如下内容
```json
{
  "files": {
    "index.js": "/0.0.6/hetu.umd.development.js",
    "index.min.js": "/0.0.6/hetu.umd.production.min.js",
    "index.css": "/0.0.6/index.css"
  },
  "entrypoints": [
    "index.js",
    "index.css"
  ]
}
```
河图主应用, 会自动读取里面的内容, 并动态加载资源

### 第二步 安装依赖
安装client层依赖
```shell
cd ../client && yarn
```

安装server层依赖
```shell
cd ../server && yarn
```

### 第三步 启动服务
启动client层服务
```shell
cd ../client && yarn start
```

启动server层服务
```shell
cd ../server && yarn dev
```

打开 [http://127.0.0.1:1234](http://127.0.0.1:1234) 预览, 可使用任意邮箱注册账号

![](https://file.ljcdn.com/hetu-cdn/hetu-display-index-1598618209.png)

## 🤝 版本记录

[CHANGELOG](/CHANGELOG.md)

## 🙋 问题咨询
- QQ群 【河图开源交流】 782899873

## ❤️ 主要贡献者

| Name                                     | Avatar                                                                                                     | Name                                     | Avatar                                                          | Name                                   | Avatar                                                                                                    | Name                                      | Avatar                                                                                                     | Name                               | Avatar                                                                                                     |
| ---------------------------------------- | ---------------------------------------------------------------------------------------------------------- | ---------------------------------------- | --------------------------------------------------------------- | -------------------------------------- | --------------------------------------------------------------------------------------------------------- | ----------------------------------------- | ---------------------------------------------------------------------------------------------------------- | ---------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| [好爸爸](https://github.com/good-father) | ![](https://avatars0.githubusercontent.com/u/18495604?s=40&u=9c52375b23eb3eb0402922cabb1cb90e910fc943&v=4) | [嘻老师](https://github.com/aa978563552) | ![](https://avatars0.githubusercontent.com/u/61268325?s=40&v=4) | [姚泽源](https://github.com/YaoZeyuan) | ![](https://avatars3.githubusercontent.com/u/7150325?s=40&u=8fb766237479748224c4425c4badd436872fcc12&v=4) | [liaoqixin](https://github.com/liaoqixin) | ![](https://avatars1.githubusercontent.com/u/39083857?s=40&u=bdc30ac0690d258bff8053d91e7ee114891de6fe&v=4) | [般若超](https://github.com/WISZC) | ![](https://avatars2.githubusercontent.com/u/37796897?s=40&u=fe779bfa82b95da9bbc39eb33bfa3ab239969e07&v=4) |


## License

[MIT](http://opensource.org/licenses/MIT)

Copyright(c) 2020 Lianjia, Inc. All Rights Reserved

