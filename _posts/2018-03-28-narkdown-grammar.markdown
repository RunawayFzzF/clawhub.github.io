---
layout:       post
title:        "Markdown 语法大合集"
subtitle:     "markdown study"
date:         2018-03-28 17:36:00
author:       "ClawHub"
header-img:   "img/in-post/2018-03-28-narkdown-grammar/markdown.jpg"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - 测试
    - test
    - tags
---

<!-- Chinese Version -->
<div class="zh post-container">
    {% capture about_zh %}{% include posts/2018-03-28-narkdown-grammar/zh.md %}{% endcapture %}
    {{ about_zh | markdownify }}
</div>

<!-- English Version -->
<div class="en post-container">
    {% capture about_en %}{% include posts/2018-03-28-narkdown-grammar/en.md %}{% endcapture %}
    {{ about_en | markdownify }}
</div>
