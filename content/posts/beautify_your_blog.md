---
title: 用Next美化 HEXO & GitHub 博客
date: 2019-08-19T15:03:24+08:00
tags: [Web, Development]
categories: [Development]
---

## 回顾

上文我们已经搭建好一个基于 GitHub Pages 的播客啦, 但是 HEXO 自带的主题是真的丑……

所以, 作为外观协会的成员(假的), 我觉得有必要美化一下我们的博客.

那怎么美化咧??? 会不会很麻烦?

你要是个死宅, 那还真挺麻烦的…

## 开始吧

### 安装NexT主题

为了省事, 我们选择了美观而又方便的NexT主题.

安装这么做:

```bash
$ git clone https://github.com/theme-next/hexo-theme-next themes/next
```

注意哦, 这一定是在你的博客目录下面执行的, 在别的文件夹下面是没有效果的哦!

然后你可以用编辑器打开站点配置文件`_config.yml`, 然后找到`theme`, 把`landscape` 改成 `next`就大功告成啦! 但是一定要注意哦, 对于yml文件, 每个属性名称冒号的后面一定是有一个空格的, 否则设置会出错哦!

```yml
theme: next #注意 ‘:’ 后面是有空格的!
```

### 初级配置

现在我们对NexT主题进行一个小小的配置.

打开博客文件夹下的`themes → next → _config.yml`文件, 对,也叫 `_config.yml` …… 不要晕了哦!

NexT提供了四种主题风格, 分别是Muse、Mist、Pisces、Gemini, 可以在刚打开的`_config.yml`里找到`scheme`这一项进行修改,然后 :

```bash
$ hexo s
```

浏览器打开 [本地博客](http://localhost:4000) 就能看见你的新主题啦!

想立刻部署到GitHub? 没问题呀, 但是我们先不急......在本地配置满意了在上传也不迟呀.

### 自定义外观

#### 笔者的站点配置文件 ( 可以先跳过 )

我先在这里贴一下我的站点配置,可以先跳过哦:

主题配置文件:

```yml
## ---------------------------------------------------------------
## Theme Core Configuration Settings
## See: https://theme-next.org/docs/theme-settings/
## ---------------------------------------------------------------

## If false, merge configs from `_data/next.yml` into default configuration (rewrite).
## If true, will fully override default configuration by options from `_data/next.yml` (override). Only for NexT settings.
## And if true, all config from default NexT `_config.yml` must be copied into `next.yml`. Use if you know what you are doing.
## Useful if you want to comment some options from NexT `_config.yml` by `next.yml` without editing default config.
override: false

## Console reminder if new version released.
reminder: false

## Allow to cache content generation. Introduced in NexT v6.0.0.
cache:
  enable: true

## Remove unnecessary files after hexo generate.
minify: false

## Define custom file paths.
## Create your custom files in site directory `source/_data` and uncomment needed files below.
custom_file_path:
  #head: source/_data/head.swig
  #header: source/_data/header.swig
  #sidebar: source/_data/sidebar.swig
  #postMeta: source/_data/post-meta.swig
  #postBodyEnd: source/_data/post-body-end.swig
  #footer: source/_data/footer.swig
  #bodyEnd: source/_data/body-end.swig
  #variable: source/_data/variables.styl
  #mixin: source/_data/mixins.styl
  #style: source/_data/styles.styl


## ---------------------------------------------------------------
## Site Information Settings
## See: https://theme-next.org/docs/getting-started/
## ---------------------------------------------------------------

favicon:
  small: /images/favicon-16x16-next.png
  medium: /images/favicon-32x32-next.png
  apple_touch_icon: /images/apple-touch-icon-next.png
  safari_pinned_tab: /images/logo.svg
  #android_manifest: /images/manifest.json
  #ms_browserconfig: /images/browserconfig.xml

## hexo-generator-feed required for rss support. Leave rss as blank to use site's feed link.
## Set rss to false to disable feed link. Set rss to specific value if you have burned your feed already.
rss: /atom.xml

footer:
  # Specify the date when the site was setup. If not defined, current year will be used.
  since: 2019

  # Icon between year and copyright info.
  icon:
    # Icon name in Font Awesome. See: https://fontawesome.com/v4.7.0/icons/
    # `heart` is recommended with animation in red (#ff0000).
    name: heart
    # If you want to animate the icon, set it to true.
    animated: true
    # Change the color of icon, using Hex Code.
    color: "#808080"

  # If not defined, `author` from Hexo `_config.yml` will be used.
  copyright: Reverier

  powered:
    # Hexo link (Powered by Hexo).
    enable: false
    # Version info of Hexo after Hexo link (vX.X.X).
    version: false

  theme:
    # Theme & scheme info link (Theme - NexT.scheme).
    enable: false
    # Version info of NexT after scheme info (vX.X.X).
    version: false

  # Beian ICP information for Chinese users. See: http://www.beian.miit.gov.cn
  beian:
    enable: false
    icp:
  baidushare:
    type: button
    baidushare: true
  jiathis: true

## Creative Commons 4.0 International License.
## See: https://creativecommons.org/share-your-work/licensing-types-examples
## Available values of license: by | by-nc | by-nc-nd | by-nc-sa | by-nd | by-sa | zero
## You can set a language value if you prefer a translated version of CC license, e.g. deed.zh
## CC licenses are available in 39 languages, you can find the specific and correct abbreviation you need on https://creativecommons.org
creative_commons:
  license: zero
  sidebar: false
  post: false
  language:


## ---------------------------------------------------------------
## SEO Settings
## ---------------------------------------------------------------

## Disable Baidu transformation on mobile devices.
disable_baidu_transformation: false

## Set a canonical link tag in your hexo, you could use it for your SEO of blog.
## See: https://support.google.com/webmasters/answer/139066
## Remember to set up your URL in Hexo `_config.yml` (e.g. url: http://yourdomain.com)
canonical: true

## Change headers hierarchy on site-subtitle (will be main site description) and on all post / page titles for better SEO-optimization.
seo: false

## If true, will add site-subtitle to index page.
## Remember to set up your site-subtitle in Hexo `_config.yml` (e.g. subtitle: Subtitle)
index_with_subtitle: false

## Automatically add external URL with Base64 encrypt & decrypt.
exturl: false

## Google Webmaster tools verification.
## See: https://www.google.com/webmasters
google_site_verification: true

## Bing Webmaster tools verification.
## See: https://www.bing.com/webmaster
bing_site_verification:

## Yandex Webmaster tools verification.
## See: https://webmaster.yandex.ru
yandex_site_verification:

## Baidu Webmaster tools verification.
## See: https://ziyuan.baidu.com/site
baidu_site_verification:

## Enable baidu push so that the blog will push the url to baidu automatically which is very helpful for SEO.
baidu_push: true


## ---------------------------------------------------------------
## Menu Settings
## ---------------------------------------------------------------

## Usage: `Key: /link/ || icon`
## Key is the name of menu item. If the translation for this item is available, the translated text will be loaded, otherwise the Key name will be used. Key is case-senstive.
## Value before `||` delimiter is the target link.
## Value after `||` delimiter is the name of Font Awesome icon. If icon (with or without delimiter) is not specified, question icon will be loaded.
## When running the site in a subdirectory (e.g. domain.tld/blog), remove the leading slash from link value (/archives -> archives).
## External url should start with http:// or https://
menu:
  home: / || home
  about: /about/ || user
  tags: /tags/ || tags
  categories: /categories/ || th
  archives: /archives/ || archive
  #schedule: /schedule/ || calendar
  #sitemap: /sitemap.xml || sitemap
  #commonweal: /404/ || heartbeat

## Enable / Disable menu icons / item badges.
menu_settings:
  icons: true
  badges: true


## ---------------------------------------------------------------
## Scheme Settings
## ---------------------------------------------------------------

## Schemes
##scheme: Muse
##scheme: Mist
##scheme: Pisces
scheme: Gemini

## Fireworks
fireworks: true
## ---------------------------------------------------------------
## Sidebar Settings
## See: https://theme-next.org/docs/theme-settings/sidebar
## ---------------------------------------------------------------

## Posts / Categories / Tags in sidebar.
site_state: true

## Social Links
## Usage: `Key: permalink || icon`
## Key is the link label showing to end users.
## Value before `||` delimiter is the target permalink.
## Value after `||` delimiter is the name of Font Awesome icon. If icon (with or without delimiter) is not specified, globe icon will be loaded.
social:
  GitHub: https://github.com/Reverier-Xu || github
  E-Mail: mailto:reverier.xu@gmail.com || envelope
  微博: https://weibo.com/dreamskytech || weibo
  知乎: https://www.zhihu.com/people/reverier-78/activities || stack-overflow
  #Google: https://plus.google.com/yourname || google
  #Twitter: https://twitter.com/yourname || twitter
  #FB Page: https://www.facebook.com/yourname || facebook
  #VK Group: https://vk.com/yourname || vk
  #StackOverflow: https://stackoverflow.com/yourname || stack-overflow
  #YouTube: https://youtube.com/yourname || youtube
  #Instagram: https://instagram.com/yourname || instagram
  #Skype: skype:yourname?call|chat || skype

social_icons:
  enable: true
  icons_only: false
  transition: false

## Blog rolls
links_icon: link
links_title: Links
links_layout: block
##links_layout: inline
links:
    Title: http://example.com

## Sidebar Avatar
avatar:
  # In theme directory (source/images): /images/avatar.gif
  # In site directory (source/uploads): /uploads/avatar.gif
  # You can also use other linking images.
  url: /images/logo.jpg
  # If true, the avatar would be dispalyed in circle.
  rounded: true
  # If true, the avatar would be rotated with the cursor.
  rotated: true

## Table Of Contents in the Sidebar
toc:
  enable: true
  # Automatically add list number to toc.
  number: true
  # If true, all words will placed on next lines if header width longer then sidebar width.
  wrap: false
  # If true, all level of TOC in a post will be displayed, rather than the activated part of it.
  expand_all: false
  # Maximum heading depth of generated toc. You can set it in one post through `toc_max_depth` in Front-matter.
  max_depth: 6

sidebar:
  # Sidebar Position.
  position: left
  #position: right

  # Manual define the sidebar width. If commented, will be default for:
  # Muse | Mist: 320
  # Pisces | Gemini: 240
  #width: 300

  # Sidebar Display (only for Muse | Mist), available values:
  #  - post    expand on posts automatically. Default.
  #  - always  expand for all pages automatically.
  #  - hide    expand only when click on the sidebar toggle icon.
  #  - remove  totally remove sidebar including sidebar toggle.
  display: post

  # Sidebar offset from top menubar in pixels (only for Pisces | Gemini).
  offset: 12
  # Enable sidebar on narrow view (only for Muse | Mist).
  onmobile: false

## A button to open designated chat widget in sidebar.
## Firstly, you need enable the chat service you want to activate its sidebar button.
chat:
  enable: true
  service: chatra
  #service: tidio
  icon: comment # Icon name in Font Awesome, set false to disable icon.
  text: 聊天 # Button text, change it as you wish.

  # Post wordcount display settings
## Dependencies: https://github.com/willin/hexo-wordcount
post_wordcount:
  item_text: true
  wordcount: true
  min2read: true

## ---------------------------------------------------------------
## Post Settings
## See: https://theme-next.org/docs/theme-settings/posts
## ---------------------------------------------------------------

## Automatically scroll page to section which is under <!-- more --> mark.
scroll_to_more: true

## Automatically saving scroll position of each page in the browser.
save_scroll: false
## rss: /atom.xml
## Automatically excerpt description in homepage as preamble text.
excerpt_description: true

## Automatically excerpt (Not recommend).
## Use <!-- more --> in the post to control excerpt accurately.
auto_excerpt:
  enable: true
  length: 150

## Read more button
## If true, the read more button would be displayed in excerpt section.
read_more_btn: true

## Post meta display settings
post_meta:
  item_text: true
  created_at: true
  updated_at:
    enable: true
    another_day: true
  categories: true

## Post wordcount display settings
## Dependencies: https://github.com/theme-next/hexo-symbols-count-time
symbols_count_time:
  separated_meta: false
  item_text_post: false
  item_text_total: false
  awl: 2
  wpm: 275

## Use icon instead of the symbol # to indicate the tag at the bottom of the post
tag_icon: false

## Wechat Subscriber
wechat_subscriber:
  enable: false
  qcode: #/uploads/wechat-qcode.jpg
  #description: Subscribe to my blog by scanning my public wechat account.

## Reward (Donate)
reward_settings:
  # If true, reward would be displayed in every article by default.
  # You can show or hide reward in a specific article throuth `reward: true | false` in Front-matter.
  enable: false
  animation: false
  #comment: Donate comment here.

reward:
  #wechatpay: /images/wechatpay.png
  #alipay: /images/alipay.png
  #bitcoin: /images/bitcoin.png

## Related popular posts
## Dependencies: https://github.com/tea3/hexo-related-popular-posts
related_posts:
  enable: false
  title: # Custom header, leave empty to use the default one
  display_in_home: false
  params:
    maxCount: 5
    #PPMixingRate: 0.0
    #isDate: false
    #isImage: false
    #isExcerpt: false

## Post edit
## Dependencies: https://github.com/hexojs/hexo-deployer-git
post_edit:
  enable: false
  url: https://github.com/reverier-xu/reverier-xu.github.com/tree/master/ # Link for view source
  #url: https://github.com/user-name/repo-name/edit/branch-name/subdirectory-name # Link for fork & edit


## ---------------------------------------------------------------
## Custom Page Settings
## See: https://theme-next.org/docs/theme-settings/custom-pages
## ---------------------------------------------------------------

## Enable "cheers" for archive page.
cheers: true

## TagCloud settings for tags page.
tagcloud:
  # If true, font size, font color and amount of tags can be customized
  enable: false
  # All values below are same as default, change them by yourself
  min: 12 # Minimun font size in px
  max: 30 # Maxium font size in px
  start: "#ccc" # Start color (hex, rgba, hsla or color keywords)
  end: "#111" # End color (hex, rgba, hsla or color keywords)
  amount: 200 # Amount of tags, change it if you have more than 200 tags

## Google Calendar
## Share your recent schedule to others via calendar page.
## API Documentation: https://developers.google.com/google-apps/calendar/v3/reference/events/list
## To get api_key: https://console.developers.google.com
## Create & manage a public Google calendar: https://support.google.com/calendar/answer/37083


## ---------------------------------------------------------------
## Misc Theme Settings
## ---------------------------------------------------------------

## Set the text alignment in posts / pages.
text_align:
  # Available values: start | end | left | right | center | justify | justify-all | match-parent
  desktop: justify
  mobile: justify

## Reduce padding / margin indents on devices with narrow width.
mobile_layout_economy: false

## Android Chrome header panel color ($brand-bg / $headband-bg => $black-deep).
android_chrome_color: "#222"

## Hide sticky headers and color the menu bar on Safari (iOS / macOS).
safari_rainbow: false

## Optimize the display of scrollbars on webkit based browsers.
custom_scrollbar: false

## Custom Logo (Do not support scheme Mist)
custom_logo: #/uploads/custom-logo.jpg

codeblock:
  # Code Highlight theme
  # Available values: normal | night | night eighties | night blue | night bright
  # See: https://github.com/chriskempson/tomorrow-theme
  highlight_theme: normal
  # Add copy button on codeblock
  copy_button:
    enable: true
    # Show text copy result.
    show_result: true
    # Available values: default | flat | mac
    style:

back2top:
  enable: true
  # Back to top in sidebar.
  sidebar: false
  # Scroll percent label in b2t button.
  scrollpercent: false


## Reading progress bar
reading_progress:
  enable: false
  # Available values: top | bottom
  position: top
  color: "#37c6c0"
  height: 2px

## `Follow me on GitHub` banner in the top-right corner.
github_banner:
  enable: true
  permalink: https://github.com/Reverier-Xu
  title: My tour on GitHub


## ---------------------------------------------------------------
## Font Settings
## See: https://theme-next.org/docs/theme-settings/#Fonts-Customization
## ---------------------------------------------------------------
## Find fonts on Google Fonts (https://www.google.com/fonts)
## All fonts set here will have the following styles:
##   light | light italic | normal | normal italic | bold | bold italic
## Be aware that setting too much fonts will cause site running slowly
## ---------------------------------------------------------------
## To avoid space between header and sidebar in scheme Pisces / Gemini, Web Safe fonts are recommended for `global` (and `title`):
## Arial | Tahoma | Helvetica | Times New Roman | Courier New | Verdana | Georgia | Palatino | Garamond | Comic Sans MS | Trebuchet MS
## ---------------------------------------------------------------

font:
  # Use custom fonts families or not.
  # Depended options: `external` and `family`.
  enable: true

  # Uri of fonts host, e.g. //fonts.googleapis.com (Default).
  host: https://fonts.cat.net

  # Font options:
  # `external: true` will load this font family from `host` above.
  # `family: Times New Roman`. Without any quotes.
  # `size: x.x`. Use `em` as unit. Default: 1 (16px)

  # Global font settings used for all elements inside <body>.
  global:
    external: true
    family:  #Fira Code, Helvetica, Tahoma, Arial, "PingFang SC", "WenQuanYi Micro Hei","Hiragino Sans GB", "Heiti SC", "Microsoft YaHei"
    size:

  # Font settings for site title (.site-title).
  title:
    external: true
    family:  #"PingFang SC", "WenQuanYi Micro Hei","Hiragino Sans GB", "Heiti SC", "Microsoft YaHei"
    size:

  # Font settings for headlines (<h1> to <h6>).
  headings:
    external: true
    family:
    size:

  # Font settings for posts (.post-body).
  posts:
    external: true
    family:

  # Font settings for <code> and code blocks.
  codes:
    external: true
    family: 'Fira Code'


## ---------------------------------------------------------------
## Third Party Plugins & Services Settings
## See: https://theme-next.org/docs/third-party-services/
## You may need to install dependencies or set CDN URLs in `vendors`
## There are two different CDN providers by default:
##   - jsDelivr (cdn.jsdelivr.net), works everywhere even in China
##   - CDNJS (cdnjs.cloudflare.com), provided by cloudflare
## ---------------------------------------------------------------

## Math Formulas Render Support
math:
  enable: false

  # Default (true) will load mathjax / katex script on demand.
  # That is it only render those page which has `mathjax: true` in Front-matter.
  # If you set it to false, it will load mathjax / katex srcipt EVERY PAGE.
  per_page: true

  # hexo-renderer-pandoc (or hexo-renderer-kramed) required for full MathJax support.
  mathjax:
    enable: false
    # See: https://mhchem.github.io/MathJax-mhchem/
    mhchem: false

  # hexo-renderer-markdown-it-plus (or hexo-renderer-markdown-it with markdown-it-katex plugin) required for full Katex support.
  katex:
    enable: false
    # See: https://github.com/KaTeX/KaTeX/tree/master/contrib/copy-tex
    copy_tex: false

## Easily enable fast Ajax navigation on your website.
## Dependencies: https://github.com/theme-next/theme-next-pjax
## For moreinformation: https://github.com/MoOx/pjax
pjax: false

## Fancybox. There is support for old version 2 and new version 3.
## Choose only one variant, do not need to install both.
## To install 2.x: https://github.com/theme-next/theme-next-fancybox
## To install 3.x: https://github.com/theme-next/theme-next-fancybox3
## For more information: https://fancyapps.com/fancybox
fancybox: false

## A JavaScript library for zooming images like Medium.
## Do not enable both `fancybox` and `mediumzoom`.
## Dependencies: https://github.com/theme-next/theme-next-mediumzoom
## For more information: https://github.com/francoischalifour/medium-zoom
mediumzoom: false

## Vanilla JavaScript plugin for lazyloading images.
## Dependencies: https://github.com/theme-next/theme-next-lazyload
## For more information: https://github.com/ApoorvSaxena/lozad.js
lazyload: false

## Pangu Support
## Dependencies: https://github.com/theme-next/theme-next-pangu
## For more information: https://github.com/vinta/pangu.js
pangu: false

## Quicklink Support
## Dependencies: https://github.com/theme-next/theme-next-quicklink
## For more information: https://github.com/GoogleChromeLabs/quicklink
quicklink:
  enable: false

  # Quicklink (quicklink.umd.js script) is loaded on demand.
  # Add `quicklink: true` in Front-matter of the page or post you need.
  # Home page and archive page can be controlled through home and archive options below.
  home: true
  archive: true

  # Default (true) will initialize quicklink after the load event fires.
  delay: true
  # Custom a time in milliseconds by which the browser must execute prefetching.
  timeout: 3000
  # Default (true) will enable fetch() or falls back to XHR.
  priority: true

  # For more flexibility you can add some patterns (RegExp, Function, or Array) to ignores.
  # See: https://github.com/GoogleChromeLabs/quicklink#custom-ignore-patterns
  ignores:

## Bookmark Support
## Dependencies: https://github.com/theme-next/theme-next-bookmark
bookmark:
  enable: false
  # If auto, save the reading position when closing the page or clicking the bookmark-icon.
  # If manual, only save it by clicking the bookmark-icon.
  save: auto


## ---------------------------------------------------------------
## Comments and Widgets
## See: https://theme-next.org/docs/third-party-services/comments-and-widgets
## ---------------------------------------------------------------

## Multiple Comment System Support
comments:
  # Available values: tabs | buttons
  style: tabs
  # Choose a comment system to be displayed by default.
  # Available values: changyan | disqus | disqusjs | facebook_comments_plugin | gitalk | livere | valine | vkontakte
  active:
  # Setting `true` means remembering the comment system selected by the visitor.
  storage: true

## Disqus
disqus:
  enable: false
  shortname:
  count: true
  lazyload: false

## DisqusJS
## Alternative Disqus - Render comment component using Disqus API.
## Demo: https://suka.js.org/DisqusJS/
## For more information: https://github.com/SukkaW/DisqusJS
disqusjs:
  enable: false
  # API Endpoint of Disqus API (https://disqus.com/api/).
  # Leave api empty if you are able to connect to Disqus API.
  # Otherwise you need a reverse proxy for Disqus API.
  # For example:
  # api: https://disqus.skk.moe/disqus/
  api:
  apikey: # Register new application from https://disqus.com/api/applications/
  shortname: # See: https://disqus.com/admin/settings/general/

## Changyan
changyan:
  enable: false
  appid:
  appkey:

## Valine
## You can get your appid and appkey from https://leancloud.cn
## For more information: https://valine.js.org, https://github.com/xCss/Valine
valine:
  enable: false # When enable is set to be true, leancloud_visitors is recommended to be closed for the re-initialization problem within different leancloud adk version
  appid: # Your leancloud application appid
  appkey: # Your leancloud application appkey
  notify: false # Mail notifier. See: https://github.com/xCss/Valine/wiki
  verify: false # Verification code
  placeholder: Just go go # Comment box placeholder
  avatar: mm # Gravatar style
  guest_info: nick,mail,link # Custom comment header
  pageSize: 10 # Pagination size
  language: zh-cn # Language, available values: en, zh-cn
  visitor: false # leancloud-counter-security is not supported for now. When visitor is set to be true, appid and appkey are recommended to be the same as leancloud_visitors' for counter compatibility. Article reading statistic https://valine.js.org/visitor.html
  comment_count: true # If false, comment count will only be displayed in post page, not in home page

## LiveRe comments system
## You can get your uid from https://livere.com/insight/myCode (General web site)
livere_uid: MTAyMC80NjE1MC8yMjY2MQ==

## Gitalk
## Demo: https://gitalk.github.io
## For more information: https://github.com/gitalk/gitalk
gitalk:
  enable: false
  github_id: # GitHub repo owner
  repo: # Repository name to store issues
  client_id: # GitHub Application Client ID
  client_secret: # GitHub Application Client Secret
  admin_user: # GitHub repo owner and collaborators, only these guys can initialize gitHub issues
  distraction_free_mode: true # Facebook-like distraction free mode
  # Gitalk's display language depends on user's browser or system environment
  # If you want everyone visiting your site to see a uniform language, you can set a force language value
  # Available values: en | es-ES | fr | ru | zh-CN | zh-TW
  language:


## ---------------------------------------------------------------
## Content Sharing Services
## See: https://theme-next.org/docs/third-party-services/content-sharing-services
## ---------------------------------------------------------------

## AddThis Share. See: https://www.addthis.com
## Go to https://www.addthis.com/dashboard to customize your tools.
add_this_id:

## Likely Share
## For more information: https://ilyabirman.net/projects/likely/, https://github.com/ilyabirman/Likely
## Likely supports four looks, nine social networks, any button text.
## You are free to modify the text value and order of any network.
likely:
  enable: false
  look: normal # Available values: normal | light | small | big
  networks:
    twitter: Tweet
    facebook: Share
    linkedin: Link
    gplus: Plus
    vkontakte: Share
    odnoklassniki: Class
    telegram: Send
    whatsapp: Send
    pinterest: Pin

## NeedMoreShare2
## Dependencies: https://github.com/theme-next/theme-next-needmoreshare2
## For more information: https://github.com/revir/need-more-share2
## iconStyle: default | box
## boxForm: horizontal | vertical
## position: top / middle / bottom + Left / Center / Right
## networks:
## Weibo | Wechat | Douban | QQZone | Twitter | Facebook | Linkedin | Mailto | Reddit | Delicious | StumbleUpon | Pinterest
## GooglePlus | Tumblr | GoogleBookmarks | Newsvine | Evernote | Friendfeed | Vkontakte | Odnoklassniki | Mailru
needmoreshare2:
  enable: false
  postbottom:
    enable: false
    options:
      iconStyle: box
      boxForm: horizontal
      position: bottomCenter
      networks: Weibo,Wechat,Douban,QQZone,Twitter,Facebook
  float:
    enable: false
    options:
      iconStyle: box
      boxForm: horizontal
      position: middleRight
      networks: Weibo,Wechat,Douban,QQZone,Twitter,Facebook


## ---------------------------------------------------------------
## Statistics and Analytics
## See: https://theme-next.org/docs/third-party-services/statistics-and-analytics
## ---------------------------------------------------------------


## CNZZ count
cnzz_siteid:

## Application Insights
## See: https://azure.microsoft.com/en-us/services/application-insights
application_insights:



## Facebook comments plugin
## This plugin depends on Facebook SDK.
## If facebook_sdk.enable is false, Facebook comments plugin is unavailable.
facebook_comments_plugin:
  enable:       false
  num_of_posts: 10    # Minimum posts num is 1
  width:        100%  # Default width is 550px
  scheme:       light # Default scheme is light (light or dark)

## VKontakte API Support


## Another tool to show number of visitors to each article.
## Visit https://console.firebase.google.com/u/0/ to get apiKey and projectId.
## Visit https://firebase.google.com/docs/firestore/ to get more information about firestore.
firestore:
  enable: false
  collection: articles # Required, a string collection name to access firestore database
  apiKey: # Required
  projectId: # Required

## Show Views / Visitors of the website / page with busuanzi.
## Get more information on http://ibruce.info/2015/04/04/busuanzi
busuanzi_count:
  enable: true
  total_visitors: true
  total_visitors_icon: user
  total_views: true
  total_views_icon: eye
  post_views: true
  post_views_icon: eye


## ---------------------------------------------------------------
## Search Services
## See: https://theme-next.org/docs/third-party-services/search-services
## ---------------------------------------------------------------

## Algolia Search
## Dependencies: https://github.com/theme-next/theme-next-algolia-instant-search
## For more information: https://www.algolia.com
algolia_search:
  enable: false
  hits:
    per_page: 10
  labels:
    input_placeholder: Search for Posts
    hits_empty: "We didn't find any results for the search: ${query}"
    hits_stats: "${hits} results found in ${time} ms"

## Local search
## Dependencies: https://github.com/wzpan/hexo-generator-search
local_search:
  enable: false
  # If auto, trigger search by changing input.
  # If manual, trigger search by pressing enter key or search button.
  trigger: auto
  # Show top n results per article, show all results by setting to -1
  top_n_per_article: 1
  # Unescape html strings to the readable one.
  unescape: false
  # Preload the search data when the page loads.
  preload: false

## Swiftype Search API Key
swiftype_key:


## ---------------------------------------------------------------
## Chat Services
## See: https://theme-next.org/docs/third-party-services/chat-services
## ---------------------------------------------------------------

## Chatra Support
## See: https://chatra.io
## Dashboard: https://app.chatra.io/settings/general
chatra:
  enable: false
  async: true
  id: # Visit Dashboard to get your ChatraID
  #embed: # Unfinished experimental feature for developers. See: https://chatra.io/help/api/#injectto

## Tidio Support
## See: https://www.tidiochat.com
## Dashboard: https://www.tidiochat.com/panel/dashboard
tidio:
  enable: false
  key: # Public Key, get it from dashboard. See: https://www.tidiochat.com/panel/settings/developer


## ---------------------------------------------------------------
## Tags Settings
## See: https://theme-next.org/docs/tag-plugins/
## ---------------------------------------------------------------

## Note tag (bs-callout)
note:
  # Note tag style values:
  #  - simple    bs-callout old alert style. Default.
  #  - modern    bs-callout new (v2-v3) alert style.
  #  - flat      flat callout style with background, like on Mozilla or StackOverflow.
  #  - disabled  disable all CSS styles import of note tag.
  style: simple
  icons: false
  border_radius: 3
  # Offset lighter of background in % for modern and flat styles (modern: -12 | 12; flat: -18 | 6).
  # Offset also applied to label tag variables. This option can work with disabled note tag.
  light_bg_offset: 0

## Tabs tag
tabs:
  transition:
    tabs: false
    labels: true
  border_radius: 0

## PDF tag, requires two plugins: pdfObject and pdf.js
## pdfObject will try to load pdf files natively, if failed, pdf.js will be used.
## The following `cdn` setting is only for pdfObject, because cdn for pdf.js might be blocked by CORS policy.
## So, you must install the dependency of pdf.js if you want to use pdf tag and make it available to all browsers.
## See: https://github.com/theme-next/theme-next-pdf
pdf:
  enable: false
  # Default height
  height: 500px

## Mermaid tag
mermaid:
  enable: false
  # Available themes: default | dark | forest | neutral
  theme: forest


## ---------------------------------------------------------------
## Animation Settings
## ---------------------------------------------------------------

## Use velocity to animate everything.
## For more information: http://velocityjs.org
motion:
  enable: true
  async: false
  transition:
    # Transition variants:
    # fadeIn | fadeOut | flipXIn | flipXOut | flipYIn | flipYOut | flipBounceXIn | flipBounceXOut | flipBounceYIn | flipBounceYOut
    # swoopIn | swoopOut | whirlIn | whirlOut | shrinkIn | shrinkOut | expandIn | expandOut
    # bounceIn | bounceOut | bounceUpIn | bounceUpOut | bounceDownIn | bounceDownOut | bounceLeftIn | bounceLeftOut | bounceRightIn | bounceRightOut
    # slideUpIn | slideUpOut | slideDownIn | slideDownOut | slideLeftIn | slideLeftOut | slideRightIn | slideRightOut
    # slideUpBigIn | slideUpBigOut | slideDownBigIn | slideDownBigOut | slideLeftBigIn | slideLeftBigOut | slideRightBigIn | slideRightBigOut
    # perspectiveUpIn | perspectiveUpOut | perspectiveDownIn | perspectiveDownOut | perspectiveLeftIn | perspectiveLeftOut | perspectiveRightIn | perspectiveRightOut
    post_block: fadeIn
    post_header: slideDownIn
    post_body: slideDownIn
    coll_header: slideLeftIn
    # Only for Pisces | Gemini.
    sidebar: slideUpIn

## Progress bar in the top during page loading.
## Dependencies: https://github.com/theme-next/theme-next-pace
## For more information: https://github.com/HubSpot/pace
pace:
  enable: false
  # Themes list:
  # big-counter | bounce | barber-shop | center-atom | center-circle | center-radar | center-simple
  # corner-indicator | fill-left | flat-top | flash | loading-bar | mac-osx | material | minimal
  theme: minimal

## JavaScript 3D library.
## Dependencies: https://github.com/theme-next/theme-next-three
three:
  enable: false
  delay: false # Set true to further delay loading
  three_waves: true
  canvas_lines: true
  canvas_sphere: true

## Canvas-nest
## Dependencies: https://github.com/theme-next/theme-next-canvas-nest
## For more information: https://github.com/hustcc/canvas-nest.js
canvas_nest:
  enable: true
  onmobile: true # Display on mobile or not
  color: "0,0,255" # RGB values, use `,` to separate
  opacity: 0.5 # The opacity of line: 0~1
  zIndex: -1 # z-index property of the background
  count: 99 # The number of lines

## Canvas-ribbon
## Dependencies: https://github.com/theme-next/theme-next-canvas-ribbon
## For more information: https://github.com/zproo/canvas-ribbon
canvas_ribbon:
  enable: false
  size: 300 # The width of the ribbon
  alpha: 0.6 # The transparency of the ribbon
  zIndex: -1 # The display level of the ribbon


##! ---------------------------------------------------------------
##! DO NOT EDIT THE FOLLOWING SETTINGS
##! UNLESS YOU KNOW WHAT YOU ARE DOING
##! See: https://theme-next.org/docs/advanced-settings
##! ---------------------------------------------------------------

## Script Vendors. Set a CDN address for the vendor you want to customize.
## Be aware that you would better use the same version as internal ones to avoid potential problems.
## Please use the https protocol of CDN files when you enable https on your site.
vendors:
  # Internal path prefix. Please do not edit it.
  _internal: lib

  # Internal version: 3.4.1
  # Example:
  # jquery: //cdn.jsdelivr.net/npm/jquery@3/dist/jquery.min.js
  # jquery: //cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js
  jquery:

  # Internal version: 4.7.0
  # Example:
  # fontawesome: //cdn.jsdelivr.net/npm/font-awesome@4/css/font-awesome.min.css
  # fontawesome: //cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css
  fontawesome:

  # MathJax
  # Example:
  # mathjax: //cdn.jsdelivr.net/npm/mathjax@2/MathJax.js?config=TeX-AMS-MML_HTMLorMML
  # mathjax: //cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML
  # mhchem: //cdn.jsdelivr.net/npm/mathjax-mhchem@3
  # mhchem: //cdnjs.cloudflare.com/ajax/libs/mathjax-mhchem/3.3.0
  mathjax:
  mhchem:

  # KaTeX
  # Example:
  # katex: //cdn.jsdelivr.net/npm/katex@0/dist/katex.min.css
  # katex: //cdnjs.cloudflare.com/ajax/libs/KaTeX/0.7.1/katex.min.css
  # copy_tex_js: //cdn.jsdelivr.net/npm/katex@0/dist/contrib/copy-tex.min.js
  # copy_tex_css: //cdn.jsdelivr.net/npm/katex@0/dist/contrib/copy-tex.min.css
  katex:
  copy_tex_js:
  copy_tex_css:

  # Internal version: 0.2.8
  # Example:
  # pjax: //cdn.jsdelivr.net/gh/theme-next/theme-next-pjax@0/pjax.min.js
  pjax:

  # FancyBox
  # Example:
  # fancybox: //cdn.jsdelivr.net/gh/fancyapps/fancybox@3/dist/jquery.fancybox.min.js
  # fancybox: //cdnjs.cloudflare.com/ajax/libs/fancybox/3.5.7/jquery.fancybox.min.js
  # fancybox_css: //cdn.jsdelivr.net/gh/fancyapps/fancybox@3/dist/jquery.fancybox.min.css
  # fancybox_css: //cdnjs.cloudflare.com/ajax/libs/fancybox/3.5.7/jquery.fancybox.min.css
  fancybox:
  fancybox_css:

  # Medium-zoom
  # Example:
  # mediumzoom: //cdn.jsdelivr.net/npm/medium-zoom@1/dist/medium-zoom.min.js
  mediumzoom:

  # Lazyload
  # Example:
  # lazyload: //cdn.jsdelivr.net/npm/lozad@1/dist/lozad.min.js
  # lazyload: //cdnjs.cloudflare.com/ajax/libs/lozad.js/1.9.0/lozad.min.js
  lazyload:

  # Pangu
  # Example:
  # pangu: //cdn.jsdelivr.net/npm/pangu@4/dist/browser/pangu.min.js
  # pangu: //cdnjs.cloudflare.com/ajax/libs/pangu/4.0.7/pangu.min.js
  pangu:

  # Quicklink
  # Example:
  # quicklink: //cdn.jsdelivr.net/npm/quicklink@1/dist/quicklink.umd.js
  quicklink:

  # Internal version: 1.0.0
  # Example:
  # bookmark: //cdn.jsdelivr.net/gh/theme-next/theme-next-bookmark@1/bookmark.min.js
  bookmark:

  # DisqusJS
  # Example:
  # disqusjs_js: //cdn.jsdelivr.net/npm/disqusjs@1/dist/disqus.js
  # disqusjs_css: //cdn.jsdelivr.net/npm/disqusjs@1/dist/disqusjs.css
  disqusjs_js:
  disqusjs_css:

  # Valine
  # Example:
  # valine: //cdn.jsdelivr.net/npm/valine@1/dist/Valine.min.js
  # valine: //cdnjs.cloudflare.com/ajax/libs/valine/1.3.4/Valine.min.js
  valine:

  # Gitalk
  # Example:
  # gitalk_js: //cdn.jsdelivr.net/npm/gitalk@1/dist/gitalk.min.js
  # gitalk_css: //cdn.jsdelivr.net/npm/gitalk@1/dist/gitalk.css
  gitalk_js:
  gitalk_css:

  # likely
  # Example:
  # likely_js: //cdn.jsdelivr.net/npm/ilyabirman-likely@2/release/likely.js
  # likely_css: //cdn.jsdelivr.net/npm/ilyabirman-likely@2/release/likely.css
  likely_js:
  likely_css:

  # Internal version: 1.0.0
  # Example:
  # needmoreshare2_js: //cdn.jsdelivr.net/gh/theme-next/theme-next-needmoreshare2@1/needsharebutton.min.js
  # needmoreshare2_css: //cdn.jsdelivr.net/gh/theme-next/theme-next-needmoreshare2@1/needsharebutton.min.css
  needmoreshare2_js:
  needmoreshare2_css:

  # Algolia Search
  # Example:
  # algolia_instant_js: //cdn.jsdelivr.net/npm/instantsearch.js@2/dist/instantsearch.min.js
  # algolia_instant_css: //cdn.jsdelivr.net/npm/instantsearch.js@2/dist/instantsearch.min.css
  algolia_instant_js:
  algolia_instant_css:

  # PDF
  # Example:
  # pdfobject: //cdn.jsdelivr.net/npm/pdfobject@2/pdfobject.min.js
  # pdfobject: //cdnjs.cloudflare.com/ajax/libs/pdfobject/2.1.1/pdfobject.min.js
  pdfobject:

  # Mermaid
  # Example:
  # mermaid: //cdn.jsdelivr.net/npm/mermaid@8/dist/mermaid.min.js
  # mermaid: //cdnjs.cloudflare.com/ajax/libs/mermaid/8.0.0/mermaid.min.js
  mermaid:

  # Internal version: 1.2.1
  # Example:
  # velocity: //cdn.jsdelivr.net/npm/velocity-animate@1/velocity.min.js
  # velocity: //cdnjs.cloudflare.com/ajax/libs/velocity/1.2.1/velocity.min.js
  # velocity_ui: //cdn.jsdelivr.net/npm/velocity-animate@1/velocity.ui.min.js
  # velocity_ui: //cdnjs.cloudflare.com/ajax/libs/velocity/1.2.1/velocity.ui.min.js
  velocity:
  velocity_ui:

  # Internal version: 1.0.2
  # Example:
  # pace: //cdn.jsdelivr.net/npm/pace-js@1/pace.min.js
  # pace: //cdnjs.cloudflare.com/ajax/libs/pace/1.0.2/pace.min.js
  # pace_css: //cdn.jsdelivr.net/npm/pace-js@1/themes/blue/pace-theme-minimal.css
  # pace_css: //cdnjs.cloudflare.com/ajax/libs/pace/1.0.2/themes/blue/pace-theme-minimal.min.css
  pace:
  pace_css:

  # Internal version: 1.0.0
  # Example:
  # three: //cdn.jsdelivr.net/gh/theme-next/theme-next-three@1/three.min.js
  # three_waves: //cdn.jsdelivr.net/gh/theme-next/theme-next-three@1/three-waves.min.js
  # canvas_lines: //cdn.jsdelivr.net/gh/theme-next/theme-next-three@1/canvas_lines.min.js
  # canvas_sphere: //cdn.jsdelivr.net/gh/theme-next/theme-next-three@1/canvas_sphere.min.js
  three:
  three_waves:
  canvas_lines:
  canvas_sphere:

  # Internal version: 1.0.0
  # Example:
  # canvas_nest: //cdn.jsdelivr.net/gh/theme-next/theme-next-canvas-nest@1/canvas-nest.min.js
  # canvas_nest_nomobile: //cdn.jsdelivr.net/gh/theme-next/theme-next-canvas-nest@1/canvas-nest-nomobile.min.js
  canvas_nest:
  canvas_nest_nomobile:

  # Internal version: 1.0.0
  # Example:
  # canvas_ribbon: //cdn.jsdelivr.net/gh/theme-next/theme-next-canvas-ribbon@1/canvas-ribbon.js
  canvas_ribbon:

## Assets
css: css
js: js
images: images

passage_end_tag:
  enabled: true
```

站点配置文件:

```yml
## Hexo Configuration
### Docs: https://hexo.io/docs/configuration.html
### Source: https://github.com/hexojs/hexo/

## Site
title: Reverier 个人博客
subtitle: 记录, 分享, 实践
description: Hope, Marvel, Youth.
keywords: 算法, 程序开发, 生活, 操作系统, 信息安全
google-site-verification: uSLhm-2JUAoM7eOTok7L19IyTJqp7dgwYpDu8dthN5Y
author: Reverier Xu
language: zh-CN
timezone: Asia/Shanghai

chatra:
  enable: true
  async: true
  id: MwHS8RYqLswW4Gep4
## URL
### If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: https://www.wootec.top
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:

## Directory
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:

## Writing
new_post_name: :title.md # File name of new posts
default_layout: post
titlecase: false # Transform title into titlecase
external_link: true # Open external links in new tab
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
future: true
highlight:
  enable: true
  line_number: true
  auto_detect: true
  tab_replace:

## Home page setting
## path: Root path for your blogs index page. (default = '')
## per_page: Posts displayed per page. (0 = disable pagination)
## order_by: Posts order. (Order by date descending by default)
index_generator:
  path: ''
  per_page: 7
  order_by: -date

## Category & Tag
default_category: uncategorized
category_map:
tag_map:

## Date / Time format
### Hexo uses Moment.js to parse and display date
### You can customize the date format as defined in
### http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD
time_format: HH:mm:ss

## Pagination
### Set per_page to 0 to disable pagination
per_page: 10
pagination_dir: page

## Extensions
### Plugins: https://hexo.io/plugins/
### Themes: https://hexo.io/themes/
theme: next
plugins: hexo-generate-feed



## Deployment
### Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: git@github.com:reverier-xu/reverier-xu.github.io.git
  branch: master
```

可以看见, 文件中对于各项的设置介绍已经十分的清楚了.

#### 开始配置

##### 小红心

首先, 大家也看到了笔者的博客, 鼠标点击会出来小红心 ( 别抬杠说不是红的 ), 这个效果怎么设置呢?

在`/themes/next/source/js/src`下新建文件 `clicklove.js` , 接着把下面的代码拷贝粘贴到 `clicklove.js` 文件中: 

```js
!function (e, t, a) {
    function n() {
        c(".heart{width: 10px;height: 10px;position: fixed;background: #f00;transform: rotate(45deg);-webkit-transform: rotate(45deg);-moz-transform: rotate(45deg);}.heart:after,.heart:before{content: '';width: inherit;height: inherit;background: inherit;border-radius: 50%;-webkit-border-radius: 50%;-moz-border-radius: 50%;position: fixed;}.heart:after{top: -5px;}.heart:before{left: -5px;}"), o(), r()
    } function r() {
        for (var e = 0; e < d.length; e++)d[e].alpha <= 0 ? (t.body.removeChild(d[e].el), d.splice(e, 1)) : (d[e].y-- , d[e].scale += .004, d[e].alpha -= .013, d[e].el.style.cssText = "left:" + d[e].x + "px;top:" + d[e].y + "px;opacity:" + d[e].alpha + ";transform:scale(" + d[e].scale + "," + d[e].scale + ") rotate(45deg);background:" + d[e].color + ";z-index:99999");
        requestAnimationFrame(r)
    } function o() {
        var t = "function" == typeof e.onclick && e.onclick; e.onclick = function (e) { t && t(), i(e) }
    } function i(e) {
        var a = t.createElement("div");
        a.className = "heart", d.push({ el: a, x: e.clientX - 5, y: e.clientY - 5, scale: 1, alpha: 1, color: s() }), t.body.appendChild(a)
    } function c(e) {
        var a = t.createElement("style"); a.type = "text/css"; try { a.appendChild(t.createTextNode(e)) } catch (t) { a.styleSheet.cssText = e } t.getElementsByTagName("head")[0].appendChild(a)
    } function s() {
        return "rgb(" + ~~(255 * Math.random()) + "," + ~~(255 * Math.random()) + "," + ~~(255 * Math.random()) + ")"
    } var d = [];
    e.requestAnimationFrame = function () {
        return e.requestAnimationFrame || e.webkitRequestAnimationFrame || e.mozRequestAnimationFrame || e.oRequestAnimationFrame || e.msRequestAnimationFrame || function (e) { setTimeout(e, 1e3 / 60) }
    }(), n()
}(window, document);
```

在`\themes\next\layout\_layout.swig`文件末尾添加: 

```html
<script type="text/javascript" src="/js/src/clicklove.js"></script>
```

然后跑一下 :

```
$ hexo s
```

就能看见效果啦!

##### 动态背景:变幻线

```yml
canvas_nest:
  enable: true
  onmobile: true # display on mobile or not
  color: '0,0,0' # RGB values, use ',' to separate
  opacity: 0.5 # the opacity of line: 0~1
  zIndex: -1 # z-index property of the background
  count: 150 # the number of lines
```

在主题配置文件找到上面的配置, 然后把`enable`改成`true`就好啦!

其实NexT集成的特效有很多的,别的你可以去搜搜看,这里就只介绍博主的搭建过程啦.

但是你用`hexo s`跑了一下,发现并没有出现想要的效果欸, 骗人!

那当然不是啦,去下载一个资源包就好了:

```
$ git clone https://github.com/theme-next/theme-next-canvas-nest themes/next/source/lib/canvas-nest
```

这就OK啦!

现在打开博客看看, 是不是很有趣呢?

##### 作者头像设置

如何设置头像呢?

你可以把头像文件放在`博客主目录/themes/next/source/images`目录下, 然后在主题配置文件中:

```yml
avatar:
  # in theme directory(source/images): /images/avatar.gif
  # in site  directory(source/uploads): /uploads/avatar.gif
  # You can also use other linking images.
  url: /images/author.jpg  #在这里填写我们刚放的图片~
  # If true, the avatar would be dispalyed in circle.
  rounded: true #开启鼠标进入时的旋转特效~
  # The value of opacity should be choose from 0 to 1 to set the opacity of the avatar.
  opacity: 1 #这个是缩放,,可以不设置
  # If true, the avatar would be rotated with the cursor.
  rotated: true #这个用来设置头像是否为圆形显示,true为圆形哦
```

然后我们`hexo s`一下, 就能看见我们的头像出现啦! 现在把鼠标移上去, 是不是就能看见头像顺溜的转了一圈啦?

##### 文章结束标志

在路径 `\themes\next\layout\_macro` 中新建一个叫 `passage-end-tag.swig` 文件, 注意后缀要正确哦, 然后把下面的代码复制进去:

```html
<div>
    {% if not is_index %}
        <div style="text-align:center;color: #ccc;font-size:14px;">你想在文章末尾对读者说的话</div>
    {% endif %}
</div>
```

接着我们打开`\themes\next\layout\_macro\post.swig`文件, 在`post-body` 之后你会看见一个`END POST BODY`, 然后继续往下翻, 最后在 `post-footer` 之前把这些代码粘贴进去:

```html
<div>
  {% if not is_index %}
    {% include 'passage-end-tag.swig' %}
  {% endif %}
</div>
```

最后我们打开主题配置文件`_config.yml`,在末尾添加下面的代码: 

```yml
passage_end_tag:
  enabled: true
```

大功告成啦!!!

##### 设置侧边栏

OK, 现在我们可以设置一下主题配置文件, 其中`social`表示社交信息, 我们可以填入我们相关的链接, 格式为链接名: 链接地址 || 链接图标, 其中链接图标必须是FontAwesome网站中存在的图标名哦, 否则在页面中是无法显示的!

```yml
## Posts / Categories / Tags in sidebar.
site_state: true

## Social Links
## Usage: `Key: permalink || icon`
## Key is the link label showing to end users.
## Value before `||` delimiter is the target permalink.
## Value after `||` delimiter is the name of Font Awesome icon. If icon (with or without delimiter) is not specified, globe icon will be loaded.
social:
  GitHub: https://github.com/Reverier-Xu || github
  E-Mail: mailto:reverier.xu@gmail.com || envelope
  微博: https://weibo.com/dreamskytech || weibo
  知乎: https://www.zhihu.com/people/reverier-78/activities || stack-overflow
  #Google: https://plus.google.com/yourname || google
  #Twitter: https://twitter.com/yourname || twitter
  #FB Page: https://www.facebook.com/yourname || facebook
  #VK Group: https://vk.com/yourname || vk
  #StackOverflow: https://stackoverflow.com/yourname || stack-overflow
  #YouTube: https://youtube.com/yourname || youtube
  #Instagram: https://instagram.com/yourname || instagram
  #Skype: skype:yourname?call|chat || skype

social_icons:
  enable: true
  icons_only: false
  transition: false
```

就像这样. 注意啊, 不要直接复制到你的博客里去! 否则你的名字下面就会挂上笔者的社交网站信息! 那就糗大咯!

##### 添加文章版权信息

文章当然是要有自己的版权的咯! 我们在这里可以设置一下, 把自己的版权声明贴到博文下面去!

当然还是在主题配置文件里啦:

```yml
creative_commons:
  license: by-nc-sa
  sidebar: true
  post: true
```

找到这一项,然后修改就可以啦!

不过个人建议放`sidebar`就可以了, 因为文章底部的好丑......

## 结束

这么一来我们的博客初始化就结束啦!

后续有问题请留言评论或者在页面上的聊天窗口中直接联系我哦!
