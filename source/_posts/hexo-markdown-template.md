---
title: Hexo is not support markdown template, so I write one!
date: 2023-10-08 16:46:56
tags: [hexo, update, plugin, markdown, template, '模版']
categories:
- [knowledge, hexo]
- [code]
---

I update Hexo last stable version (6.3.0). And I change node/npm version to v18.18.0/9.8.1.
I found the changes in Hexo is quite big. I found images can not be displayed properly.

Old version image is like this:
```html
<div align=center>
![](https://cdn.jsdelivr.net/gh/chilly/blog_cdn@master/images/4096/1.jpg)
**figure 1.1**
</div>
```

It can not be rendered properly.Then I changed the format of html like this:
```html
<div align=center>

![1.jpg](https://cdn.jsdelivr.net/gh/chilly/blog_cdn@master/images/4096/1.jpg)

**figure 1.1**

</div>

```

Then it worked. I think it is hexo **incompatibility** issue.

Every time I add a centered picture and picture title, I write like the above code. It's too complicated. If hexo support markdown template,m, I will write like this:

```html
{% image_mid '/image/a.png'  'Figure1.1'  '1.jpg' %}
```

But there is no plugin in Hexo can implement this functionality. And theme `_partials` and `_macro` is njk can not used in markdown.

So I write my own plugin `image-mid`.

In `node_modules/hexo/lib/plugins/tag`, add new file called `image_mid.js`
```javascript
'use strict';
const { resolve } = require('url');
const img = require('./img');
const { encodeURL, escapeHTML } = require('hexo-util');
/**
 * Asset image tag
 *
 * Syntax:
 *   {% image_mid path [figure] [text] %}
 * 
 *   @param {string} path
 *   1. support post_asset
 *   1. support relative path like /images/a.jpg, will find in "source" folder => sources/images/a.jpg
 *   1. support http:// and https:// 
 * 
 *   @param {string} figure
 *   Optional parameter, is image title which is below the image.
 * 
 *   @param {string} text
 *   Optional parameter, will be displayed when the image fails to load
 */

function getImage(id, path, text, ctx) {
    var image = getImageFromAsset(id, path, text, ctx);
    if (!image) {
        image = getImageFromRelativePath(path, text, ctx);
    }

    if (!image) {
        image = getImageFromInternalPath(path, text, ctx);
    }
    
    if (!image) {
        throw new Error('can not render image tag. By path:', path);
    }
    return image;
}

function getImageFromAsset(id, path, text, ctx) {
    const PostAsset = ctx.model('PostAsset');
    const asset = PostAsset.findOne({post: id, slug: path});
    if (asset) {
        return img(ctx)([encodeURL(resolve('/', asset.path)), "'' '"+text+"'"]);
    }
    return null;
}

function urlConcat(base, path) {
    if (base.endsWith('/')) {
        if (path.startsWith('/')) {
            return base.substring(0, base.length-1) + path;
        }
        return base + path;
    }
    if (path.startsWith('/')) {
        return base + path;
    }
    return base + '/' + path;
}

function removeLastSlash(path) {
    if (path.endsWith('/')) {
        return path.substring(0, path.length-1);
    }
    return path;
}

function getImageFromRelativePath(path, text, ctx) {
    var base_path = '/';
    var image_path = null;
    if (ctx.config.jsdelivr_cdn) {
        if (ctx.config.jsdelivr_cdn.cdn_url_prefix) {
            base_path = removeLastSlash(ctx.config.jsdelivr_cdn.cdn_url_prefix) +'@master/';
            image_path = urlConcat(base_path, path);
        }
    }

    if (!image_path) {
        image_path = resolve(base_path, path);
    }

    return img(ctx)([encodeURL(image_path), "'' '"+text+"'"]);
}

function getImageFromInternalPath(path, text, ctx) {
    if (path.indexOf('http://') > 0 || path.indexOf('https://') > 0 ) {
        return img(ctx)([encodeURL(path), "'' '"+text+"'"]);
    }
    return null;
}

module.exports =  ctx => {

    return function assetImgMidTag(args) {
      const len = args.length;
      if (args.length <= 0) {
        console.warn('image path is not set. args:', args, 'file:', this.full_source);
        return;
      }

      var path = args[0];
      var figure = '';
      var text = '';
      if (args.length >= 2) {
        figure = escapeHTML(args[1]);
      }

      if (args.length === 3) {
        text = escapeHTML(args[2]);
      }
  
      // Find image html tag
      var image = getImage(this._id, path, text, ctx);
      return `<div align=center><br/>${image}<br/><strong>${figure}</strong><br/></div>`
    };
  };
```

In file `node_modules/hexo/lib/plugins/tag/index.js`, add the following code.
```javascript
  tag.register('image_mid', require('./image_mid')(ctx));
  tag.register('img_mid', require('./image_mid')(ctx));
```

Then in your markdown file. Write like this:
```
{% image_mid '/image/a.png'  'Figure1.1'  '1.jpg' %}

```

It will generate the html like this:
```
<div align="center"><br><img src="/image/a.png" class=""><br><strong></strong><br></div>
```

If you install [jsdelivr_cdn plugin](https://github.com/renzibei/hexo-cdn-jsdelivr/blob/master/readme-cn.md). And you write your _config.yml in Hexo root like this:
```yaml
jsdelivr_cdn:
  use_cdn: true 
  cdn_url_prefix: write your_cdn_root_path like 'https://cdn.jsdelivr.net/gh/<username for github>/<assets repo name>/'
```

The html will be generated like this:
```html

<div align="center"><br><img src="https://cdn.jsdelivr.net/gh/<username for github>/<assets repo name>@master/image/a.png" class=""><br><strong></strong><br></div>
```

The effect is as follows:

{% image_mid '/images/4096/2.png'  '在4旁边生成4' '2.png' %}


The image tag is generated as CDN path.

This `image_mid` is support:
* post asset path 
* relative path
* http/https path
* And post asset path and relative path can be generated as CDN path.


Use it and write your comments.

