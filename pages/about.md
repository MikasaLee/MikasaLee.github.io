---
layout: page
title: About Me
description: 打码改变世界
keywords: Mikasa Lee, 李润睿,Li RunRui
comments: true
menu: 关于
permalink: /about/
---
Li RunRui

A Chinese

A Student

Love Code

Love Anime

## 联系

<ul>
{% for website in site.data.social %}
<li>{{website.sitename }}：<a href="{{ website.url }}" target="_blank">@{{ website.name }}</a></li>
{% endfor %}

</ul>


## Skill Keywords

{% for skill in site.data.skills %}
### {{ skill.name }}
<div class="btn-inline">
{% for keyword in skill.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
