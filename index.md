---
title: Home
---

<div> 
    <img src="{{ '/images/octocat.jpg' | absolute_url }}" alt="github octocat" style="width:45%;" >
    <img src="{{ '/images/jekyll.png' | absolute_url }}" alt="jekyll icon" style="width:45%;" >
</div>

# Les vulnérabilités des systèmes d'information 

Très souvent les systèmes d'information sont connectés à Internet, lequel abrite des adversaires. C'est pouquoi l'architecture ainsi que le *design* de tels systèmes, doit prendre en question la question de sécurité, i.e est ce que le système est résilient en cas d'attaque.

Cette veille technologique a pour but d'introduire le lecteur aux différentes failles qu'il faut connaître et surtout éviter, lorsqu'on conçoit un système d'information. Il est de plus en plus urgent de se prémunir des outils de défense et d'éduquer les producteurs ainsi que les consommateurs sur les failles de sécurité avec la prolifération des objets IoT dans nos vies quotidiennes ainsi que l'intrusion de l'IA dans tous les domaines.

L'étude de ces failles se décomposera en deux parties: Les failles du *côté serveur* puis les failles *côté client*.



Watch [workshop screen cast](https://youtu.be/SWVjQsvQocA){:target="_blank"} (2017) for full content.

<div class="toc" markdown="1">
## Sommaire:

{% for lesson in site.pages %}
{% if lesson.nav == true %}- [{{ lesson.title }}]({{ lesson.url | absolute_url }}){% endif %}
{% endfor %}
</div>

Hosted at [University of Idaho Library](http://www.lib.uidaho.edu/){:target="_blank"} April 2017, Oct 2018

> built using [Jekyll](https://jekyllrb.com/), [GitHub Pages](https://pages.github.com/), and [workshop-template](https://github.com/evanwill/workshop-template).
>
> licensed cc-by-sa <a href="https://github.com/evanwill">evan will</a> {{ site.pub_year }}. (get [source code]({{ site.repo }}))
> 
> <a href="http://creativecommons.org/licenses/by-sa/4.0/" rel="license"><img style="border-width: 0;" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" alt="Creative Commons License" /></a>
