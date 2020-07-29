---
layout: page
title: About
permalink: /about/
weight: 3
---

# **About Me**

Hi I am **{{ site.author.name }}** :wave:,<br>
I am a Machine Learning Engineer

<div class="row">
{% include about/skills.html title="Programming Skills" source=site.data.programming-skills %}
{% include about/skills.html title=" " source=site.data.programming-skills-2 %}
</div>

<div class="row">
{% include about/timeline.html %}
</div>