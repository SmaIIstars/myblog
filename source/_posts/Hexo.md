---
title: Hexo
tags: Hexo
abbrlink: b132932
date: 2020-12-22 14:31:25
---

# Hexo

**My blog is built using Hexo.**

## Install

```js
// install the hexo
npm install hexo

// create the hexo project
hexo init <folder>
cd <folder>
npm install

/**
 * File Directory Structure
 * ├── _config.yml	:Configuration information for the website
 * ├── package.json	:Dependent files for the project
 * ├── scaffolds		:Template folder
 * ├── source				:A folder to store the resources (Files/folders with names beginning with _ and hidden files
 * |   ├── _posts		 wille be ignored except for '_posts' folder)
 * |   └── _drafts
 * └── themes
 */
```

## beautify

**I am going to modularize the entire page and describe the configurations of each section separately**

![Layout1](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/Hexo-layout1.png)

![Layout2](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/Hexo-layout2.png)

### Abbreviation Agreement

| Abbreviation |                      note                      |
| :----------: | :--------------------------------------------: |
|  _s_config_  |       site's config (/hexo/\_config.yml)       |
|  _n_config_  | next's config (/hexo/themes/next/\_config.yml) |
|   s_source   |         site's source (/myblog/source)         |
|   n_layout   |    next's config (/hexo/themes/next/layout)    |

### The overall style

#### Theme

[reference](https://hexo.io/themes/)

```git
git clone https://github.com/theme-next/hexo-theme-next themes/next
```

```yaml
# s_config

modify s_config:
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next
```

##### Schemes

```yaml
# n_config

# Schemes
#scheme: Muse
#scheme: Mist
scheme: Pisces
#scheme: Gemini
```

- Muse

  ![Muse](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/Next-Schemes-Muse.png)

- Mist

  ![Mist](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/Next-Schemes-Mist.png)

- Pisces

  ![Pisces](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/Next-Schemes-Pisces.png)

- Gemini

  ![Gemini](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/Next-Schemes-Gemini.png)

##### Dark Mode

```yaml
# n_config

# Dark Mode
darkmode: true
```

#### Basic Website Infomation

```yml
# s_config

# Site
title: SmallStars's blog
subtitle: ""
description: ""
keywords:
author: SmallStars
language: en
timezone: "Asia/Shanghai"
```

#### Custom Style

```yaml
# n_config

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
  style: source/_data/styles.styl
```

##### Background Image and Cursor

```stylus
// s_source/_data/styles.styl

// background image
body {
  background-image: url(xxx.jpg);
  background-repeat: no-repeat;
  background-attachment:fixed;
  background-position:50% 50%;
  background-origin: content-box;
  opacity: 0.9;
  +mobile(){
    background-image: url(xxx.jpg);
    background-size: cover;
  }

  // cursor
  cursor: url(xxx.cur), default;
}

// hover cursor
a:hover{
  cursor: url(xxx.cur), default;
}
```

##### ScrollBar

```stylus
// s_source/_data/styles.styl

::-webkit-scrollbar{
  width: 5px;
  height: 1px;
  margin: 0 auto;
}

::-webkit-scrollbar-thumb {
  border-radius: 10px;
  -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
  background: #535353;
}

::-webkit-scrollbar-track {
  box-shadow   : inset 0 0 5px rgba(0, 0, 0, 0.2);
  background   : #000;
}
```

#### GitHub banner

```yaml
# n_config

github_banner:
  enable: true
  permalink: https://github.com/xxx
  title: Follow me on GitHub
```

### Head

### Sidebar

![Sidebar](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/Sidebar.png)

#### Website Title

Reference the Basic Website Infomation

#### Menu Item

- Item

  ```yaml
  # n_config

  menu:
    home: / || fa fa-home
    #about: /about/ || fa fa-user
    tags: /tags/ || fa fa-tags
    #categories: /categories/ || fa fa-th
    archives: /archives/ || fa fa-archive
    #schedule: /schedule/ || fa fa-calendar
    #sitemap: /sitemap.xml || fa fa-sitemap
    #commonweal: /404/ || fa fa-heartbeat
  ```

- Icon and badges

  ```yaml
  # n_config

  menu_settings:
    icons: true
    badges: true
  ```

##### User

- avatar

  ```yaml
  # n_config

  # Sidebar Avatar
  avatar:
    url: https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/img1.jpg
    rounded: true
    rotated: true
  ```

- author: Reference the Basic Website Infomation

##### site_state

```yaml
# n_config

site_state: true
```

##### social

```yaml
# n_config

social:
  GitHub: https://github.com/SmaIIstars || fab fa-github
  E-Mail: mailto:smallstars.he@qq.com || fa fa-envelope
  #Weibo: https://weibo.com/yourname || fab fa-weibo
  #Google: https://plus.google.com/yourname || fab fa-google
  #Twitter: https://twitter.com/yourname || fab fa-twitter
  #FB Page: https://www.facebook.com/yourname || fab fa-facebook
  #StackOverflow: https://stackoverflow.com/yourname || fab fa-stack-overflow
  #YouTube: https://youtube.com/yourname || fab fa-youtube
  #Instagram: https://instagram.com/yourname || fab fa-instagram
  #Skype: skype:yourname?call|chat || fab fa-skype
  RSS: /atom.xml || fa fa-rss

social_icons:
  enable: true
  icons_only: true
  transition: true
```

#### Back to top button

```yaml
# n_config

back2top:
  enable: true
  sidebar: true
  scrollpercent: true
```

### Post

![PostCodeblock](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/PostCodeblock.png)

#### Code Block

```yaml
# n_config

codeblock:
  highlight_theme: night eighties
  copy_button:
    enable: true
    show_result: true
    style: default
```

#### Password

![Password](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/Password.png)

```swig
// n_layout
// password.swig
<script src="https://cdn.staticfile.org/jquery/3.1.1/jquery.min.js"></script>
<script>
    var div = $('.post-body');
    //var toc=$('.post-toc-wrap')
    var toc=$('.nav')
    function password() {
        if('{{ page.password }}'){
            div.remove();
            toc.remove();
            $('.post-header').after(
            '<span class="description" style="font-weight: bold;border: none;display: block;'+
            'width: 60%;margin: 0 auto;text-align: center;outline: none;margin-bottom: 40px;resize:none ">'+
            'Please Enter your password and press Enter to read' +
            '</span>' +
            '<div class="qiang" style="height: 100px;width: 60%;margin:0 auto">' +
            '<input class="password"  type="password" autocomplete="new-password" autofocus="autofocus" value="" style="border-radius: 5px; height: 30px; display: block; margin: 0 auto; width: 95%; box-shadow: none; background-color: transparent; border: 1px solid rgba(255,255,255,0.5); color: rgba(255,255,255,0.5); outline: none; text-align: center;"/>' +
            '</div>')

            document.onclick = function (event) {
                var e = event || window.event;
                var elem = e.srcElement || e.target;

                while (elem) {
                    if (elem != document) {
                        if (elem.className == "password") {
                            return;
                        }
                        elem = elem.parentNode;
                    } else {
                        return;
                    }
                }
            }

            $(document).keyup(function(event){
                if(event.keyCode ==13&&$('.password').length>0){
                    if ($('.password').val() == '{{ page.password }}') {

                        (div).appendTo($(".post-header"))

                        toc.appendTo($(".post-toc"))
                        var doms =  document.getElementsByClassName("nav-child");
                        for(var i = 0; i < doms.length; i++){
                            doms[i].style.display="block";　　
                        }

                        $(".description").remove();
                        $(".qiang").remove();
                        $(".password").remove();

                        $(".post-block").css({opacity:1});
                        $(".post-header").css({opacity:1});
                        $(".post-body").css({opacity:1});
                        $(".pagination").css({opacity:1});
                    }else {
                        alert("对不起，密码输入错误。")
                    }
                }
            });
        }
    }
    password();
</script>
```

```swig
// n_layout
<main class="main">
  ......
  {% include 'password.swig' %}
</main>
```

Add the password in the beginning of article

```markdown
password: xxx
```

#### PostEnd

![PostEnd](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/PostEnd.png)

##### The end of the post

```swig
// themes/next/layout/_macro/passage-end-tag.swig

<div>
  {% if not is_index %}
  	<div style="text-align:center;color: #ccc;font-size:14px;">
  		-------------The end of this article, thanks for reading-------------
  	</div>
  {% endif %}
</div>
```

```swig
// themes/next/layout/_macro/post.swig

<div>
  {% if not is_index %}
  {% include 'passage-end-tag.swig' %}
  {% endif %}
</div>
```

##### Donate

```yaml
# n_config

reward_settings:
  enable: true
  animation: false

reward:
  wechatpay: xxx.png
  alipay: xxx.png
```

##### End tag's icons

```yaml
# n_config

tag_icon: true
```

### Footer

![Footer](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/Footer.png)

#### Icon between year and copyright info

```yaml
# n_config

icon:
	# The heart icon in the middle
  name: fa fa-heart
  animated: true
  # Change the color of icon, using Hex Code.
  color: "#ff0000"

  # If not defined, `author` from Hexo `_config.yml` will be used.
  copyright:
```

#### Set the elapsed time of the website

```html
<!-- themes/next/layout/_partials/footer.swig -->

<span id="timeDate">Loading the days...</span>
<span id="times">Loading the time...</span>
<script>
  var now = new Date();
  function createtime() {
    var grt = new Date("12/16/2020 00:00:00"); // Change the time of your website or site launch
    now.setTime(now.getTime() + 250);
    days = (now - grt) / 1000 / 60 / 60 / 24;
    dnum = Math.floor(days);
    hours = (now - grt) / 1000 / 60 / 60 - 24 * dnum;
    hnum = Math.floor(hours);
    if (String(hnum).length == 1) {
      hnum = "0" + hnum;
    }
    minutes = (now - grt) / 1000 / 60 - 24 * 60 * dnum - 60 * hnum;
    mnum = Math.floor(minutes);
    if (String(mnum).length == 1) {
      mnum = "0" + mnum;
    }
    seconds =
      (now - grt) / 1000 - 24 * 60 * 60 * dnum - 60 * 60 * hnum - 60 * mnum;
    snum = Math.round(seconds);
    if (String(snum).length == 1) {
      snum = "0" + snum;
    }
    document.getElementById("timeDate").innerHTML =
      "The site has been running safely for " + dnum + " days ";
    document.getElementById("times").innerHTML =
      hnum + " hours " + mnum + " minutes " + snum + " seconds";
  }
  setInterval("createtime()", 250);
</script>
```

#### Remove the 'Powered by [Hexo](https://hexo.io/) & [NexT.Pisces](https://pisces.theme-next.org/)'

```yaml
# n_config

# Powered by Hexo & NexT
powered: false
```

#### busuanzi_count

```yaml
# n_config

busuanzi_count:
  enable: true
  total_visitors: true
  total_visitors_icon: fa fa-user
  total_views: true
  total_views_icon: fa fa-eye
  post_views: true
  post_views_icon: fa fa-eye
```

### Thrid Party

#### Small features

```yaml
# n_config

# FancyBox is a tool that offers a nice and elegant way to add zooming functionality for images.
fancybox: true
# Vanilla JavaScript plugin for lazyloading images.
lazyload: true
```

#### Local Search

```npm
npm install hexo-generator-searchdb
```

```yaml
# n_config

local_search:
  enable: true
  trigger: auto
  top_n_per_article: 1
  unescape: true
  preload: true
```

```yaml
# s_config

# Local Search
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```

#### Mermaid

```npm
npm install hexo-filter-mermaid-diagrams
```

```yaml
# n_config

mermaid:
  enable: true
  theme: dark
```

#### Pace

```yaml
# n_config

pace:
  enable: true
  theme: minimal
```

#### Comment

1. [Register](https://www.leancloud.cn/)

2. Storage ===> Objects

   Create class Comment (no restrictions)

3. Modify the files

   ```yaml
   # n_config

   valine:
     enable: true
     appid: # Your leancloud application appid
     appkey: # Your leancloud application appkey
     notify: false # Mail notifier
     verify: false # Verification code
     placeholder: Just go go # Comment box placeholder
     avatar: mm # Gravatar style
     guest_info: nick,mail,link # Custom comment header
     pageSize: 10 # Pagination size
     language: en
   ```

#### Top

```npm
// Change the default plugins
npm uninstall hexo-generator-index --save
npm install hexo-generator-index-pin-top --save
```

```yaml
// xxx.md
---
title: Hexo
tags:
  - Hexo
  - Markdown
abbrlink: b132932
date: 2020-12-22 14:31:25
top: 11
---
```

```swig
// themes/next/layout/_macro/post.swig

{%- if post.sticky > 0 %} ===> {%- if post.top > 0 %}
```

#### Abbrlink

```npm
npm install hexo-abbrlink --save
```

```yaml
# s_config

# hexo-abbrlink
permalink: posts/:abbrlink/
abbrlink:
  alg: crc32 #算法： crc16(default) and crc32
  rep: hex #进制： dec(default) and hex
```

#### Read More

```npm
npm install hexo-excerpt --save
```

```yaml
# s_config

# hexo-excerpt
excerpt:
  depth: 5
  excerpt_excludes: []
  more_excludes: []
  hideWholePostExcerpts: true
```

#### Chat

1. [Register](http://dashboard.daovoice.io/get-started?invite_code=3a4f5f60)

2. Settings => App Settings => Install to site

3. Modify the files

   ```swig
   // themes/next/layout/_partials/head/head.swig

   {% if theme.daovoice.enable %}
     {% if theme.daovoice.daovoice_app_id %}
       <script>
         (function (i, s, o, g, r, a, m) {
           i["DaoVoiceObject"] = r;
           (i[r] =
             i[r] ||
             function () {
               (i[r].q = i[r].q || []).push(arguments);
             }),
             (i[r].l = 1 * new Date());
           (a = s.createElement(o)), (m = s.getElementsByTagName(o)[0]);
           a.async = 1;
           a.src = g;
           a.charset = "utf-8";
           m.parentNode.insertBefore(a, m);
         })(
           window,
           document,
           "script",
           ("https:" == document.location.protocol ? "https:" : "http:") +
             "//widget.daovoice.io/widget/{{theme.daovoice.daovoice_app_id}}.js",
           "daovoice"
         );

         daovoice('init', {
           app_id: "{{theme.daovoice.daovoice_app_id}}"
         });
         daovoice('update');
       </script>
     {% endif %}
   {% endif %}
   ```

   ```yaml
   # n_config

   # Online contact
   daovoice:
     enable: true
     daovoice_app_id: appId
   ```

#### Music

## Reference

[Hexo Official website](https://hexo.io/zh-cn/)

[Next Official Website](https://theme-next.js.org/)

[Hexo 框架下用 NexT(v7.0+)主题美化博客](https://blog.csdn.net/weixin_39345384/article/details/80785373)

[Hexo-Next 主题博客个性化配置超详细，超全面(两万字)](https://blog.csdn.net/as480133937/article/details/100138838/)
