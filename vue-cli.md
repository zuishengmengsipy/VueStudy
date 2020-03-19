# vue-cli

vue-cli是官方提供的快速构建这个单页面应用的脚手架。

## 安装

老版本（本文档版本）：npm instal vue-cli （-g）加上-g安装到全局，推荐安装到全局

新版本（官方文档版本）： Vue CLI 的包名称由 `vue-cli` 改成了 `@vue/cli`。 如果你已经全局安装了旧版本的 `vue-cli` \(1.x 或 2.x\)，你需要先通过 `npm uninstall vue-cli -g` 或 `yarn global remove vue-cli` 卸载它。

{% tabs %}
{% tab title="新版本安装语句" %}
```text
npm install -g @vue/cli
# OR
yarn global add @vue/cli
```
{% endtab %}
{% endtabs %}

用这个命令来检查其版本是否正确：

```text
vue --version
```

**依赖：**

**vue-cli需要依赖webpack，所以vue-cli里面自带了webpack**

## [ubuntu下安装vue-cli框架](https://www.cnblogs.com/teersky/p/7245119.html)

首先安装好node.js,安装方式见[http://www.cnblogs.com/teersky/p/7255334.html](http://www.cnblogs.com/teersky/p/7255334.html)

之后正式开始vue-cli之旅吧，输入以下代码安装vue-cli模块

```text
sudo npm i vue-cli -g
```

这一步完成之后可以查看vue的版本，用以下代码查看：

```text
vue -V     //此处V必须时大写
```

## **初始化项目**

### ubuntu系统

新建一个基于webpack模板的新项目project，安装过程中需要你输入一些信息\(名称，描述和作者\)

```text
sudo vue init webpack project
```

之后进入项目

```text
cd project
npm install (sudo npm install)
npm run dev
```

到这儿vue-cli框架搭建也就完成了

注：国内因为墙的原因，npm服务器会让人很抓狂，建议使用国内镜像

```text
sudo npm install -g cnpm --registry=https://registry.npm.taobao.org
```

此处因为没带sudo我死了好多次

### windows系统

安装完成后在自己的工作空间里

```text
vue init webpack vue-demo  
```

输入命令后进入安装阶段，需要用户输入一些信息 这里省略了.....

切换到我们的项目目录下 

```text
cd vue-demo
npm run dev
```

目录结构：

　　-- build 里面是一些操作文件，使用npm run \* 时其实执行的就是这里的文件

　　-- config 配置文件，执行文件需要的配置信息

　　-- src 资源文件 所有的组件以及所有的图片 都在这个文件夹下

　　-- node\_modules  项目依赖包

　　-- static 静态资源

　　-- package.json   依赖包的json文件

