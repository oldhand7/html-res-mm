---
title: 'VS Code 初体验'
date: '2020-04-04T20:50:00+08:00'
tags: [折腾]
---

Visual Studio Code 久仰大名，但一直误认为是个臃肿大部头，毕竟是微软出品。 🙇

一试，直接拉取仓库开启 Git 同步，成了，满足本地码字同步而不用碰终端，不用碰终端，碰终端！

二试，使用 gpm 插件更能多仓库管理，也成了，好感爆棚。没遇到什么权限问题，应该是之前终端里配置过的关系。

再试，GitHub 网页上更改，VS 里拉取变更，丝滑，这下舒心到随心咯！

<!--more-->

### 下载： <https://code.visualstudio.com/>

### 插件：

- Chinese ：界面汉化
- Monokai Pro & GitHub Plus Theme：暗黑/明亮主题
- Material Icon Theme ：文件图标
- gpm ：多仓库管理
- Settings Sync :配置同步
- ……

### 配置

- 菜单 → 文件 → 自动保存，打钩


### 启用 Markdown 代码片段

![vscode-1](https://lmm.elizen.me/images/2020/04/vscode-1.png)

`settings.json` 中添加 ：
```
"[markdown]": {
    "editor.quickSuggestions": true
},
```

左下角--用户代码片段，添加：
```html
"new post": {
"prefix": "post",
"body": [
"---",
"title: \"${1:在此处添加标题}\"",
"date: ${CURRENT_YEAR}-${CURRENT_MONTH}-${CURRENT_DATE}T${CURRENT_HOUR}:${CURRENT_MINUTE}:${CURRENT_SECOND}+0800",
"tags: [${2|折腾,日常,育人,日常|}]",
"feature: ",
"---",
"$0"
],
"description": "新文章"
}
```

### 致谢

- [在 Visual Studio Code 中添加自定义的代码片段](https://blog.walterlv.com/post/add-custom-code-snippet-for-vscode.html)