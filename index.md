---
title: Home
---

<div> 
    <img src="{{ '/images/cyber.jpg' | absolute_url }}" alt="github octocat" style="width:100%;" >
</div>

# Les vulnérabilités des systèmes d'information 

Très souvent les systèmes d'information sont connectés à Internet, lequel abrite des adversaires. C'est pouquoi l'architecture ainsi que le *design* de tels systèmes, doit prendre en question la question de sécurité, i.e est ce que le système est résilient en cas d'attaque.

Cette veille technologique a pour but d'introduire le lecteur aux différentes failles qu'il faut connaître et surtout éviter, lorsqu'on conçoit un système d'information. Il est de plus en plus urgent de se prémunir des outils de défense et d'éduquer les producteurs ainsi que les consommateurs sur les failles de sécurité avec la prolifération des objets IoT dans nos vies quotidiennes ainsi que l'intrusion de l'IA dans tous les domaines.

L'étude de ces failles se décomposera en deux parties: Les failles du *côté serveur* puis les failles *côté client*.



<div class="toc" markdown="1">
## Sommaire:

{% for lesson in site.pages %}
{% if lesson.nav == true %}- [{{ lesson.title }}]({{ lesson.url | absolute_url }}){% endif %}
{% endfor %}
</div>


> built using [Jekyll](https://jekyllrb.com/), [GitHub Pages](https://pages.github.com/), and [workshop-template](https://github.com/evanwill/workshop-template).
>
