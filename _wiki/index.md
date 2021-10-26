---
layout  : wikiindex
title   : Index
toc     : true
public  : true
comment : false
updated : 2021-09-24 21:58:47 +0900
regenerate: true
---

## 학습 일지
<div>
    <ul>
{% for post in site.posts %}
    {% if post.public == true %}
        <li>
            <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">
                {{ post.date | date: '%Y/%m/%d'}} - {{ post.title }}
            </a>
        </li>
    {% endif %}
{% endfor %}
    </ul>
</div>

---

<!--## [[/study/index]]-->

## [[/study/kubernetes/index]]

* [[/study/kubernetes/1_kube]]

* [[/study/kubernetes/2_kube]]

* [[/study/kubernetes/3_kube]]

* [[/study/kubernetes/4_kube]]

* [[/study/kubernetes/5_kube]]

* [[/study/kubernetes/6_kube]]

* [[/study/kubernetes/7_kube]]

* [[/study/kubernetes/8_kube]]

<!--

## [[/algorithm/index]]

* [[/algorithm/dijkstra]]

-->

---

