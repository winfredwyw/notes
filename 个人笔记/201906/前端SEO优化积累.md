# 前端 SEO 优化方法

## 1. TKD

```
设置页面title, keyword, description, 示例：

<title>New awsome tab</title>
<meta name="keywords" content="Chrome new tab, custom tab,">
<meta name="description" content="Custom themed new tab. Get the best free chrome extension and theme your browser with your favorite heros.">
```

## 2. Logo优化

- 添加 alt 属性
- 添加 h1 父节点

```
示例：
<h1>
  <img src="logo.png" alt="xxx" />
</h1>
```

## 3. 404页面

未设置404页面，当用户打开一个错误的链接的时候没有可以给用户返回网站的路径，影响用户体验，建议设置用户返回网站的路径增加用户体检，同时也利于搜索引擎的抓取。

## 4. robots文件


未设置robots文件，不利于搜索引擎蜘蛛爬取网站的内容。Robots能够屏蔽网站内的死链接、屏蔽搜索引擎蜘蛛抓取站点内重复内容和页面、阻止搜索引擎索引网站隐私性的内容，能够提高蜘蛛的抓取效率，建议配置robots.txt文件，如下示例。

```
User-agent: *

Disallow: /api/*
Disallow: /bin/*
Disallow: /admin/*
Sitemap: https://xxxx.com/sitemap.xml
```


## 5. sitemap.xml文件

sitemap被搜索引擎用来了解网站内部链接层次和结构，确定网站中网站的页面的重要等级。sitemap能让搜索引擎蜘蛛快速了解你网站内部的所有页面URL，从而利于优化。建议添加sitemap.xml文件。推荐生成 sitemap 的 webpack 插件：

```
const SitemapPlugin = require('sitemap-webpack-plugin').default;

new SitemapPlugin(
  process.env.REACT_APP_ORIGIN,
  require(resolve('../src/sitemap.js')),
  {
    fileName: 'sitemap.xml',
    lastMod: true,
    changeFreq: 'daily',
    priority: '0.4',
    skipGzip: true
  }
)
```

## 6. 添加友链

友链是作为网站布局外链的重要方式，多一条友链就多一条网站的入口，能够提高蜘蛛抓取的几率，建议添加友链板块，并去友链交换平台添加些同行业的友链，以布局网站外链

