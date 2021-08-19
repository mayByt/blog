# 这里是 May.Byt's Blog 的源码 git

## Blog 网址 http://maybyt.cn

# Hexo-个人BLog日志

## 2.1（2021/09/19)

## 添加aplayer音乐播放器

参考[aplayer官方文档](https://github.com/MoePlayer/hexo-tag-aplayer)

安装 aplayer 的 hexo 插件：

```bash
npm install --save hexo-tag-aplayer
```

安装完成后就可以在页面用以下语句插入歌曲：

```markdown
{% aplayer title author url [picture_url, narrow, autoplay, width:xxx, lrc:xxx] %}
```

利用MeingJS 支持引入自己的网易云歌单！在站点配置文件 `_config.yml` 中设置：

```yaml
aplayer:
  meting: true
```

接着就可以通过 `{% meting ...%}` 在文章中使用 MetingJS 播放器了：

```markdown
<!-- 简单示例 (id, server, type)  -->
{% meting "60198" "netease" "playlist" %}

<!-- 进阶示例 -->
{% meting "60198" "netease" "playlist" "autoplay" "mutex:false" "listmaxheight:340px" "preload:none" "theme:#ad7a86"%}
```

这样只能在个别文章页添加播放器，以下操作为添加全盘吸底的阿player播放器：

在站点配置文件 `_config.yml` 中：

```yaml
aplayer:
  meting: true
  asset_inject: false
```

在主题配置文件 `_config.butterfly.yml` 中：

```yaml
# Inject the css and script (aplayer/meting)
aplayerInject:
  enable: true
  per_page: true
```

接着在主题配置文件 `_config.butterfly.yml` 的 `inject.bottom`中插入 Aplayer html：

```yaml
inject:
	bottom:
      - <div class="aplayer no-destroy" data-id="6926241253" data-server="netease" data-type="playlist" data-fixed="true" data-mini="true" data-listFolded="true" data-volume="0.5" data-order="random" data-theme="#44cef6" data-preload="none" data-autoplay="true" muted></div>
```

切换页面时，要使音乐不会中断，把主题配置文件的 pjax 设为 true。

---

## 修改鼠标指针

找到 `themes\Butterfly\source\css` 下创建 `mousediy.css` 文件、输入以下内容：

```css
/*鼠标样式*/
body,
html {
  cursor: url(https://cdn.jsdelivr.net/gh/mayByt/mouse/ComixCursors-White/Comix_White_Pointer.cur),
	auto !important;
}

a,
img {
  cursor: url(https://cdn.jsdelivr.net/gh/mayByt/mouse/ComixCursors-White/Comix_White_Link.cur),
	auto !important;
}

/*a标签*/
a:hover {
  cursor: url(https://cdn.jsdelivr.net/gh/mayByt/mouse/ComixCursors-White/Comix_White_Link.cur),
	auto !important;
}

/*按钮*/
button:hover {
  cursor: url(https://cdn.jsdelivr.net/gh/mayByt/mouse/ComixCursors-White/Comix_White_Link.cur),
	auto !important;
}

/*i标签*/
i:hover {
  cursor: url(https://cdn.jsdelivr.net/gh/mayByt/mouse/ComixCursors-White/Comix_White_Link.cur),
	auto !important;
}

/*页脚a标签*/
#footer-wrap a:hover {
  cursor: url(https://cdn.jsdelivr.net/gh/mayByt/mouse/ComixCursors-White/Comix_White_Link.cur),
	auto !important;
}

/*分页器*/
#pagination .page-number:hover {
  cursor: url(https://cdn.jsdelivr.net/gh/mayByt/mouse/ComixCursors-White/Comix_White_Link.cur),
	auto !important;
}

/*头部的导航栏*/
#nav .site-page:hover {
  cursor: url(https://cdn.jsdelivr.net/gh/mayByt/mouse/ComixCursors-White/Comix_White_Link.cur),
	auto !important;
}
/*鼠标样式END*/
```

编辑 `_config.butterfly.yml` 文件、在 `inject->head` 下面添加如下内容：

```yaml
- <link rel="stylesheet" href="/css/mousediy.css">
```

---

## 增加文章打赏

```yaml
reward:
  enable: true
  QR_code:
    - img: /img/wechat.jpg
      link:
      text: 微信
    - img: /img/alipay.jpg
      link:
      text: 支付宝
```



## 2.0（2021/08/17）

## 添加友情链接

首先在博客根目录 ***Blog*** 执行以下命令新建友情链接页面：

```bash
hexo new page link
```

接着修改 `source/link/index.md` 添加Front-Matter

```yaml
---
title: 友情链接
date: 2018-06-07 22:17:49
type: "link"
---
```

在博客目录中的`source/_data`创建一个文件 `link.yml`

```yaml
- class_name: 友情链接  class_desc: 需要我这个小白好好学习的大佬们  link_list:    - name: 波波哥      link: https://jegret.cn      avatar: https://jegret.cn/img/avatar.jpg      descr: 一个兼具身材和无敌编程能力的大佬    - name: 菲菲姐      link: https://greyorange.xyz/images/avatar.png      avatar: https://greyorange.xyz/images/avatar.png      descr: 理论学习天花板，波波哥的良配- class_name: 网站小盒子  class_desc: 值得推荐的网站  link_list:    - name: GitHub      link: https://github.com/      avatar: https://i.loli.net/2021/08/18/8GNXQJsF6gwKr3P.jpg      descr: 世界上最大的代码托管平台，开发者之家    - name: Bilibili      link: https://www.bilibili.com/      avatar: https://i.loli.net/2021/08/18/t1mK9vfYSr7gx3w.jpg      descr: Bilibili大学，学习鬼畜兼备    - name: 站酷      link: https://www.zcool.com.cn/home      avatar: https://i.loli.net/2021/08/18/S3j2DlpANB5waXP.jpg      descr: 很酷的网站，我喜欢经常逛逛，收录了许多很好的创意
```

---

## 增加看板娘

1. 在 ***Blog*** 目录下输入指令 live2D 插件

   ```bash
   npm install --save hexo-helper-live2d
   ```

2. 在站点配置文件 `_config.yml` 里按照如下操作：

   ```yaml
   # Live2D## https://github.com/EYHN/hexo-helper-live2dlive2d:  enable: true #开关插件版看板娘  scriptFrom: local # 默认  pluginRootPath: live2dw/ # 插件在站点上的根目录(相对路径)  pluginJsPath: lib/ # 脚本文件相对与插件根目录路径  pluginModelPath: assets/ # 模型文件相对与插件根目录路径  # scriptFrom: jsdelivr # jsdelivr CDN  # scriptFrom: unpkg # unpkg CDN  # scriptFrom: https://cdn.jsdelivr.net/npm/live2d-widget@3.x/lib/L2Dwidget.min.js # 你的自定义 url  tagMode: false # 标签模式, 是否仅替换 live2d tag标签而非插入到所有页面中  debug: false # 调试, 是否在控制台输出日志  model:    use: live2d-widget-model-wanko # npm-module package name    # use: wanko # 博客根目录/live2d_models/ 下的目录名    # use: ./wives/wanko # 相对于博客根目录的路径    # use: https://cdn.jsdelivr.net/npm/live2d-widget-model-wanko@1.0.5/assets/wanko.model.json # 你的自定义 url  display:    position: right #控制看板娘位置    width: 150 #控制看板娘大小    height: 300 #控制看板娘大小  mobile:    show: true # 手机中是否展示
   ```

3. 下载新的看板娘模型

   ```bash
   npm install --save live2d-widget-model-koharu
   ```

4. 然后在站点配置文件 `_config.yml` 里更换看板娘model：

   ```yaml
   model:  use: live2d-widget-model-koharu  # 默认为live2d-widget-model-wanko
   ```

## 美化社交图标样式

首先在[阿里巴巴矢量图标库](https://www.iconfont.cn/)找到自己需要的图标添加到购物车，之后将购物车内的图标添加至项目，然后在我的项目里选择 `Font class`，点击查看在线链接并在浏览器中打开此链接，最后另存为本地

![](C:\Users\79485\Pictures\个人博客美化\社交媒体图标美化.jpg)

将刚刚另存为的文件复制到主题文件下的 `source \ css` 里面，之后主题的配置文件（`_config.butterfly.yml`）里的 `inject` 引入刚刚的图标 css 文件

```yaml
inject:  head:    - <link rel="stylesheet" href="css/socialicons.css">
```

接着修改社交图标样式的引用

```yaml
social:  iconfont icon-GitHub: https://github.com/mayByt || Github  iconfont icon-youjian: mailto:may.tian@foxmail.com || Email  iconfont icon-weixin: https://i.loli.net/2021/08/17/frW1mqN3SAzaJ2V.jpg || WeChat  iconfont icon-QQ: https://i.loli.net/2021/08/17/TGHxE3ZC52cqvAa.jpg || QQ
```

然后打开刚才保存的 icon 的 测试时文件 `socialicons` 修改大小、颜色样式

```css
.icon-weixin:before {  content: "\e637";  font-size: 22px;  color: #00bc12;}.icon-QQ:before {  content: "\e876";  font-size: 22px;  color: #44cef6;}.icon-GitHub:before {  content: "\e8c6";  font-size: 22px;  color: #2e4e7e;}.icon-youjian:before {  content: "\e607";  font-size: 22px;  color: #ffa400;}
```



## 1.0（2021/08/16）

## 更改主题——Butterfly

Hexo的原始主题虽然也简约大气，但是还是不能满足我一颗追求美好的心灵:smirk:，所以我在[Hexo官方主题网站](https://hexo.io/themes/)的200余款主题当中，挑选了较为心仪 ***[Butterfly](https://github.com/jerryc127/hexo-theme-butterfly)*** 主题下载使用，其他我认为比较好的主题还有 ***[Hueman](https://github.com/ppoffice/hexo-theme-hueman)*** 、***[LiveMyLife](https://github.com/V-Vincen/hexo-theme-livemylife)***、***[NexT](https://github.com/next-theme/hexo-theme-next)***，可以根据自己的喜好选择最喜欢的主题来魔改~

首先下载稳定版本的 Butterfly 主题，在 Hexo 根目录下运行 Git Bash ，输入命令

```bash
git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
```

![](C:\Users\79485\Pictures\个人博客美化\下载主题.jpg)

安装完成后，应用主题，修改 Hexo 根目录下的 _config.yml，把主题改为butterfly

```yaml
theme: butterfly
```

接着安装插件 pug 以及 stylus 的渲染器，命令行输入：

```bash
npm install hexo-renderer-pug hexo-renderer-stylus --save
```

按照官方文档建议将主题配置也放入根目录下，在 hexo 的根目录创建一个文件 `_config.butterfly.yml`，并把主题目录的 `_config.yml` 内容复製到 `_config.butterfly.yml` 。这样以后对主题配置就可以直接在文件`_config.butterfly.yml`里修改，非常方便。

为了方便调试可以将这三条命令：

```powershell
hexo cleanhexo ghexo s
```

合并成一条命令，可以在根目录下找到文件 ***package.json*** ，在其中找到 “scripts” 部分，将 “sever”修改为以下语句：

```json
"scripts": {    "build": "hexo generate",    "clean": "hexo clean",    "deploy": "hexo deploy",    "server": "hexo clean && hexo generate && hexo server"  }
```

这样之后只需要执行一句

```bash
npm run server
```

就可以实现三步操作了。

---

## 修改博客主页内容（2021.8.16）

### 修改网站资料

在`站点配置文件` ***_config.yml*** 文件下修改网站资料

```yaml
# Sitetitle: May.BYT's Blogsubtitle: '天的小空间'description: '我与我周旋久，宁作我'keywords:author: May.BYTlanguage: zh-CNtimezone: ''email: may.tian@foxmain.com# URL## Set your site url here. For example, if you use GitHub Page, set url as 'https://username.github.io/project'url: http://mayByt.github.iopermalink: :year/:month/:day/:title/permalink_defaults:pretty_urls:  trailing_index: true # Set to false to remove trailing 'index.html' from permalinks  trailing_html: true # Set to false to remove trailing '.html' from permalinks
```

### 创建导航菜单

在`主题配置文件` ***_config.butterfly.yml*** 文件下创建导航菜单

```yaml
menu:  首页: / || fas fa-home  时间轴: /archives/ || fas fa-archive  标签: /tags/ || fas fa-tags  分类: /categories/ || fas fa-folder-open  # List||fas fa-list:  #   Music: /music/ || fas fa-music  #   Movie: /movies/ || fas fa-video  友链: /link/ || fas fa-link  关于: /about/ || fas fa-heart
```

### 更新 Footer 设置

```yaml
footer:  owner:    enable: true    since: 2020  custom_text: 欢迎来到天的小空间</br>Welcome to my Blog  copyright: true # Copyright of theme and framework
```

---

## 美化博客代码部分

#### 修改代码高亮主题

```yaml
highlight_theme: mac light
```

#### 开启代码复制

```yaml
highlight_copy: true
```

#### 显示代码的语言

```yaml
highlight_lang: true
```

#### 增加代码块高度限制

便于博客阅读，超出高度限制部分将被隐藏，可点击展开

```yaml
highlight_height_limit: 200 # unit: px
```

---

## 增加社交图标

```yaml
social:  fab fa-github: https://github.com/mayByt || Github  fas fa-envelope: mailto:may.tian@foxmail.com || Email  fab fa-weixin: https://z3.ax1x.com/2021/08/17/f49Ifg.jpg || WeChat  fab fa-qq: https://z3.ax1x.com/2021/08/17/f4PF2Q.jpg || QQ
```

---

## 添加文章节选展示

```yaml
# Display the article introduction on homepage# 1: description# 2: both (if the description exists, it will show description, or show the auto_excerpt)# 3: auto_excerpt (default)# false: do not show the article introductionindex_post_content:  method: 2  length: 500 # if you set method to 2 or 3, the length need to config
```

---

## 增加简繁体字转换

```yaml
# Conversion between Traditional and Simplified Chinese (簡繁轉換)translate:  enable: true  # The text of a button  default: 繁  # the language of website (1 - Traditional Chinese/ 2 - Simplified Chinese）  defaultEncoding: 2  # Time delay  translateDelay: 0  # The text of the button when the language is Simplified Chinese  msgToTraditionalChinese: '繁'  # The text of the button when the language is Traditional Chinese  msgToSimplifiedChinese: '簡'
```

## 网站副标题

在网站副标题增加打字效果，调用一言网API

```yaml
subtitle:  enable: true  # Typewriter Effect (打字效果)  effect: true  # loop (循環打字)  loop: true  # source調用第三方服務  # source: false 關閉調用  # source: 1  調用搏天api的隨機語錄（簡體）  # source: 2  調用一言網的一句話（簡體）  # source: 3  調用一句網（簡體）  # source: 4  調用今日詩詞（簡體）  # subtitle 會先顯示 source , 再顯示 sub 的內容  source: 2  # 如果有英文逗號' , ',請使用轉義字元 &#44;  # 如果有英文雙引號' " ',請使用轉義字元 &quot;  # 開頭不允許轉義字元，如需要，請把整個句子用雙引號包住  # 如果關閉打字效果，subtitle只會顯示sub的第一行文字  sub: 今天的你依旧很可爱
```

---

## 添加文章字数统计

首先执行命令，安装依赖：

```bash
npm install hexo-wordcount --save
```

接着修改`主题配置文件`

```yaml
wordcount:  enable: true  post_wordcount: true  min2read: true  total_wordcount: true
```

---

## 添加本地搜索

首先安装依赖：

```bash
npm install hexo-generator-search --save
```

然后修改 `主题配置文件`

```yaml
local_search:  enable: false
```

---

## 更新公告板

```yaml
card_announcement:    enable: true    content: 嘿嘿！你终于来啦！<img src="https://pica.zhimg.com/v2-7b58f2cac7e509ae1d355483cf13f2cc_b.webp">
```

---

## 更换顶部图及背景

```yaml
index_img: "https://i.loli.net/2021/08/17/Lnpmr9tR4Vezv5u.jpg"default_top_img: "https://i.loli.net/2021/08/17/YTW9Rva5n6GP1sF.jpg"background: url(https://i.loli.net/2021/08/17/2gZuAW8CGzlvxVR.jpg)footer_bg: true
```

