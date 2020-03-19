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

\*\*\*\*

