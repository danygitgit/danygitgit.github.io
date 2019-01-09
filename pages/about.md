---
layout: page
title: About
description: Wendy's Personal Blog
keywords: 毛毛虫, Wendy
comments: true
menu: 关于
permalink: /about/
subtitle:   <h3>Download My CV</h3>
            <a role="button" class="btn btn-primary hvr-grow-shadow" href="/assets/files/CV_Wendy_e.pdf" target="_blanks">
                <span class="flag-icon flag-icon-gb"></span> English
            </a>
            <a role="button" class="btn btn-primary hvr-grow-shadow" href="/assets/files/CV_Wendy_e.pdf" target="_blanks">
                <span class="flag-icon flag-icon-cn"></span> 中文
            </a>
                            
---

我是毛毛虫_Wendy，一直努力，不断进步。

等待破茧成蝶。

记录知识，坚信努力改变人生。

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
