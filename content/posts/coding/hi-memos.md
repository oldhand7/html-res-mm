---
title: "Hi，Memos"
date: 2022-09-05T23:14:52+0800
tags: [折腾]
feature: https://cdn.edui.fun/images/2022/09/memos.jpg
---

> An open source, self-hosted knowledge base that works with a SQLite db file.

官网：<https://usememos.com/>

可以说是支持 Docker 自部署的 flomo 。

<!--more-->

### 部署及更新代码

推荐使用 `docker-compose.yml` 部署，方便制定数据储存位置及更新版本，其中使用 `${PWD}` 指定路径为当前文件夹。

```
version: "3.0"
services:
  memos:
    image: neosmemo/memos:latest
    container_name: memos
    volumes:
      - ${PWD}/.memos/:/var/opt/memos
    ports:
      - 5230:5230
```

![memos-1](https://cdn.edui.fun/images/2022/10/memos-1.jpg)

宝塔为例：新建网站，新建 yml，开终端，丢代码。版本更新也是 **三行代码** 搞定，一行一行丢哦：

```
docker-compose pull
docker-compose down
docker-compose up -d
```

### 使用心得

- #tag 后面必须有个空格才能创建tag

### 文章推荐：

- 使用 iOS 快捷指令录入笔记：<https://github.com/usememos/memos/discussions/52>
- 开源 Memos 在群晖上部署：<https://life97.top/synology-memos.html>

### 折腾记录

API 调用最新 10条 memos 在博客首页轮播显示。甚至还能把它做后端，直接调用 API 再进行前端渲染 <https://www.life97.top/Dynamics.html> 

具体折腾先看页面源码吧～

```html
<div id="bber-talk"></div>
<style>
#bber-talk{display:-webkit-flex;display:flex;width:100%;line-height:35px;height:45px;max-width:760px;text-align:left;padding:5px 15px;margin:20px 0;position: relative;background-color: var(--light-header);border-radius:8px;font-size:15px;overflow:hidden;}
#bber-talk svg{fill: currentColor;vertical-align: middle;display: inline;margin-right:5px;margin-top: -4px;}
.talk-wrap{width:100%;}
.talk-list{margin: 0;height: 35px;}
.talk-list li {list-style:none;margin-bottom:10px;width: 100%;white-space: nowrap;text-overflow: ellipsis;overflow: hidden;zoom: 1;}
.talk-list li .datetime{margin-right:2px;}
.talk-list li a{text-decoration:none;}
.dark-theme #bber-talk{background-color: var(--dark-header);}
.dark-theme .talk-list{color: var(--dark-color);}
@media only screen and (max-width:683px) {
	#bber-talk{margin:2em 1em 1em;width:94%;}
}
</style>
<script src="https://cdn.jsdelivr.net/npm/dayjs@1.11.5/dayjs.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/dayjs@1.11.5/locale/zh-cn.js"></script>
<script src="https://cdn.jsdelivr.net/npm/dayjs@1.11.5/plugin/relativeTime.js"></script>
<script>dayjs.locale('zh-cn');dayjs.extend(window.dayjs_plugin_relativeTime)</script>
<script>
var bbUrl = "https://me.edui.fun/api/memo?creatorId=101&limit=10"
fetch(bbUrl).then(res => res.json()).then( resdata =>{
    var result = '',resultAll="",data = resdata.data
    console.log(data)
    for(var i=0;i < data.length;i++){
        var bbTime = dayjs.unix(data[i].createdTs).fromNow()
        var bbCont = data[i].content
        var newbbCont = bbCont.replace(/(https?:[^:<>"]*\/)([^:<>"]*)(\.((png!thumbnail)|(png)|(jpg)|(webp)|(jpeg)|(gif))(!blogimg)?)/g,' 🌅 ')
        var newbbCont = newbbCont.replace(/\bhttps?:\/\/(?!\S+(?:jpe?g|png|bmp|gif|webp|jfif|gif))\S+/g,' 🔗 ')
        result += `<li class="item"><span class="datetime">${bbTime}</span>： <a href="https://me.edui.fun/u/101" target="_blank">${newbbCont}</a></li>`;
    }
    var bbDom = document.querySelector('#bber-talk');
    var bbBefore = `<span class="index-talk-icon"><svg viewBox="0 0 1024 1024" width="21" height="21"><path d="M184.32 891.667692c-12.603077 0-25.206154-2.363077-37.809231-7.876923-37.021538-14.966154-59.864615-49.624615-59.864615-89.009231v-275.692307c0-212.676923 173.292308-385.969231 385.969231-385.969231h78.76923c212.676923 0 385.969231 173.292308 385.969231 385.969231 0 169.353846-137.846154 307.2-307.2 307.2H289.083077l-37.021539 37.021538c-18.904615 18.116923-43.323077 28.356923-67.741538 28.356923zM472.615385 195.347692c-178.018462 0-322.953846 144.935385-322.953847 322.953846v275.692308c0 21.267692 15.753846 29.144615 20.48 31.507692 4.726154 2.363077 22.055385 7.876923 37.021539-7.08923l46.473846-46.473846c6.301538-6.301538 14.178462-9.452308 22.055385-9.452308h354.461538c134.695385 0 244.184615-109.489231 244.184616-244.184616 0-178.018462-144.935385-322.953846-322.953847-322.953846H472.615385z"></path><path d="M321.378462 512m-59.076924 0a59.076923 59.076923 0 1 0 118.153847 0 59.076923 59.076923 0 1 0-118.153847 0Z"></path><path d="M518.301538 512m-59.076923 0a59.076923 59.076923 0 1 0 118.153847 0 59.076923 59.076923 0 1 0-118.153847 0Z"></path><path d="M715.224615 512m-59.076923 0a59.076923 59.076923 0 1 0 118.153846 0 59.076923 59.076923 0 1 0-118.153846 0Z"></path></svg></span><div class="talk-wrap"><ul class="talk-list">`
    var bbAfter = `</ul></div>`
    resultAll = bbBefore + result + bbAfter
    bbDom.innerHTML = resultAll;
});
setInterval(function() {
    for (var s, n = document.querySelector(".talk-list"), e = n.querySelectorAll(".item"), t = 0; t < e.length; t++)
    setTimeout(function() {
      n.appendChild(e[0])
    },1000)
},1000)
</script>
```