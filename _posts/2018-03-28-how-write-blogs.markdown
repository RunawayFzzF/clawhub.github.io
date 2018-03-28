---
layout:       post
title:        "怎样用github写博客"
subtitle:     "write"
date:         2018-03-28 19:46:00
author:       "ClawHub"
header-img:   "img/in-post/2018-03-28-how-write-blogs/how-write.jpg"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - markdown
    - blogs
---

<!-- Chinese Version -->
<div class="zh post-container">
    {% capture about_zh %}{% include posts/2018-03-28-how-write-blogs/zh.md %}{% endcapture %}
    {{ about_zh | markdownify }}
</div>

<!-- English Version -->
<div class="en post-container">
    {% capture about_en %}{% include posts/2018-03-28-how-write-blogs/en.md %}{% endcapture %}
    {{ about_en | markdownify }}
</div>
