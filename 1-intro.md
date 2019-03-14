---
title: Côté serveur
nav: true
---

# Buffer overflow

La vulnérabilité Buffre overflow Un buffer est une section séquentielle de mémoire allouée pour contenir n'importe quoi, d'une chaîne de caractères à un tableau d'entiers. [Un débordement de tampon](https://www.owasp.org/index.php/Buffer_overflow_attack){:target="_blank"}, ou dépassement de tampon, se produit lorsque plus de données sont placées dans un tampon de longueur fixe que le tampon ne peut en traiter. L'information supplémentaire, qui doit aller quelque part, peut déborder dans l'espace mémoire adjacent, corrompant ou écrasant les données contenues dans cet espace. Ce débordement entraîne généralement un plantage du système, mais il crée également la possibilité pour un attaquant d'exécuter du code arbitraire ou de manipuler les erreurs de codage pour provoquer des actions malveillantes.

Un débordement de tampon dans un programme écrit en C, C++, Fortran ou Assembly pourrait permettre à l'attaquant de compromettre complètement le système visé.

## Comment exploiter un buffer overflow
Les cybercriminels exploitent les problèmes de débordement de tampon pour modifier le chemin d'exécution de l'application en écrasant des parties de sa mémoire. Les données supplémentaires malveillantes peuvent contenir du code conçu pour déclencher des actions spécifiques, comme l'envoi de nouvelles instructions à l'application attaquée qui pourrait entraîner un accès non autorisé au système. 

{% include figure.html file="buff_ov.gif" alt="github octocat" width="45%" %}

Dans certains cas, un attaquant injecte un code malveillant dans la mémoire qui a été corrompue par le débordement. Dans d'autres cas, l'attaquant profite simplement du débordement et de la corruption de la mémoire adjacente. 

## Exemple d'attaque

Par exemple, considérons un programme qui demande un mot de passe d'utilisateur afin de permettre à l'utilisateur d'accéder au système. Dans le code ci-dessous, le mot de passe correct accorde à l'utilisateur les privilèges root. Si le mot de passe est incorrect, le programme n'accordera pas de privilèges à l'utilisateur.

```
printf ("\n Mot de passe correct \n");
pass = 1;
}
if(pass)
{
/* Accorder maintenant à l'utilisateur les droits d'admin*/
printf ("\n Droits d'admin accordés à l'utilisateur! \n");
}
return 0;
```
Supposons que la fonction read_req() récupère l'input de l'utilisateur:
```
void read_req() {
    char buf[12];
    int i;
    gets(buf);
    //. . . fais des choses avec le buffer . . .
}
```
il y a une possibilité de débordement de tampon dans ce programme car la fonction gets() ne vérifie pas les limites du tableau. Voici un exemple de ce qu'un attaquant pourrait faire avec cette erreur de codage :
```
$ ./buffer_overflow
Entrez le mot de passe :
aaaaaaaaaaaaaaaaaaaaaaaaaaa
Mot de passe incorrect
Droits d'admin accordés à l'utilisateur!
```

> Dans l'exemple ci-dessus, le programme donne à l'utilisateur les privilèges root, même si l'utilisateur a entré un mot de passe incorrect. Dans ce cas, l'attaquant a fourni une entrée d'une longueur supérieure à celle que le tampon peut contenir, créant un débordement de tampon, qui a écrasé la mémoire des entiers "pass". Par conséquent, malgré le mot de passe incorrect, la valeur de "pass" est devenue non nulle, et l'attaquant reçoit les privilèges root.

{% include figure.html file="heartbleed.png" alt="github octocat" width="85%" %}
## Comment se protéger d'une telle vulnérabilité ?
Pour éviter le débordement de tampon, les développeurs d'applications C/C++ doivent éviter les fonctions de bibliothèque standard qui ne sont pas contrôlées par des bornes, telles que gets, scanf et strcpy.
En outre, les pratiques de développement sécurisées devraient inclure des tests réguliers pour détecter et corriger les débordements de tampon. La façon la plus fiable d'éviter ou de prévenir les débordements de tampon est d'utiliser une protection automatique au niveau de la langue. Une autre correction est la vérification des limites appliquée au moment de l'exécution (runtime), qui empêche le dépassement de tampon en vérifiant automatiquement que les données écrites dans un tampon sont dans des limites acceptables.

# SQL Injection

L'injection SQL (SQLi) est une faille de sécurité de l'application qui permet aux attaquants de contrôler la base de données d'une application - leur permettant d'accéder ou de supprimer des données, de modifier le comportement de l'application en fonction des données et de faire d'autres choses indésirables - en poussant l'application à envoyer des commandes SQL imprévues.

Ces faiblesses surviennent lorsqu'une application utilise des données non fiables, telles que des données saisies dans des champs de formulaire Web, dans le cadre d'une requête de base de données. Lorsqu'une application ne parvient pas à désinfecter correctement ces données non fiables avant de les ajouter à une requête SQL, un attaquant peut inclure ses propres commandes SQL que la base de données va exécuter. De telles vulnérabilités SQLi sont faciles à prévenir, mais SQLi reste un risque majeur pour les applications Web, et de nombreuses organisations restent vulnérables à des brèches de données potentiellement dommageables résultant de l'injection SQL.

{% include figure.html file="sql_inj.png" alt="github octocat" width="95%" %}

## L'attaque:
Une attaque SQLi se déroule en deux étapes :
1. **Recherche** : L'attaquant essaie de soumettre diverses valeurs inattendues pour l'argument, observe comment l'application répond, et détermine une attaque à tenter.
2. **Attaque** : L' attaquant fournit une valeur d'entrée soigneusement élaborée qui, lorsqu'elle est utilisée comme argument à une requête SQL, sera interprétée comme faisant partie d'une commande SQL plutôt que comme de simples données ; la base de données exécute alors la commande SQL telle que modifiée par l'attaquant.
Les étapes de recherche et d'attaque peuvent être facilement automatisées grâce à des outils [facilement disponibles](https://kalilinuxtutorials.com/sql-injection/){:target="_blank"}.

## La défense:
Il existe des moyens simples d'éviter d'introduire des vulnérabilités SQLi dans une application, et de limiter les dommages qu'elles peuvent causer.
- On peut éviter et réparer les vulnérabilités SQLi en utilisant des requêtes paramétrées. Ces types de requêtes spécifient des espaces réservés pour les paramètres afin que la base de données les traite toujours comme des données plutôt que comme faisant partie d'une commande SQL. Les instructions préparées et les "mappeurs" relationnels objet (ORM) facilitent la tâche des développeurs.
- On peut atténuer l'impact des vulnérabilités SQLi en appliquant le moins de privilèges possible sur la base de données. En s'assurant que chaque application possède ses propres informations d'identification de base de données et que ces informations d'identification ont les droits minimaux dont l'application a besoin.

## Exemple:
Supposer qu'une possible requête de l'application, exposable à une injection SQL soit la suivante:
```
SELECT numCompte, solde FROM comptes WHERE proprietaire_compte_id = 391
```
>Ceci est transmis à la base de données, et les comptes et soldes de l'utilisateur 391 sont retournés, et des lignes sont ajoutées à la page pour les afficher.

L'attaquant pourrait changer le paramètre "id" pour être interprété comme :
```
0 OR 1=1
```
Et cela a pour résultat que la requête devient :
```
SELECT numCompte, solde FROM comptes WHERE proprietaire_compte_id = 0 OR 1=1
```
Lorsque cette requête est transmise à la base de données, elle renvoie tous les numéros de compte et les soldes qu'elle a stockés, et des lignes sont ajoutées à la page pour les afficher. L'attaquant connaît maintenant les numéros de compte et les soldes de chaque utilisateur.

{% include figure.html file="sql_inj_xkcd.png" alt="github octocat" width="85%" %}

# Mauvaise gestion d'exceptions
Il n'est pas rare que des applications Web ou des bases de données génèrent des messages d'erreur. En fait, ils font partie intégrante des opérations et fournissent des renseignements précieux sur les enjeux et les problèmes. Toutefois, une mauvaise gestion des erreurs présente des risques importants pour la sécurité.

Les vulnérabilités les plus courantes surviennent lorsqu'un système révèle des messages d'erreur ou des codes d'erreur détaillés générés à partir de traces de pile, de vidanges de base de données et d'une grande variété d'autres problèmes, y compris des problèmes de mémoire insuffisante, des exceptions de pointeur nul et des erreurs de temporisation réseau.
## Comment l'attaquant peut en profiter ?
Les attaquants peuvent utiliser ces informations pour exploiter les failles et pénétrer dans les systèmes. De plus, les incohérences dans les messages peuvent fournir des indices sur le fonctionnement d'un site et la façon de l'exploiter. Une mauvaise gestion des erreurs peut entraîner des pannes du système, des débordements de tampon et des attaques par déni de service. Ils peuvent également exposer des données et des informations sensibles, y compris des mots de passe.

## L'attaque
Voici un exemple OWASP d'une erreur HTTP 404 Not Found qui révèle des informations sensibles :
```
Not Found
The requested URL /page.html was not found on this server.
Apache/2.2.3 (Unix) mod_ssl/2.2.3 OpenSSL/0.9.7g  DAV/2 PHP/5.1.2 Server at localhost Port 80
```
>Ce message d'erreur est généré lorsque l'utilisateur demande une URL inexistante. En plus d'informer l'utilisateur qu'une erreur s'est produite et que le fichier est introuvable, le code fournit des informations précieuses sur la version du serveur Web, l'OS, les modules et le code utilisé. Un attaquant peut utiliser ces informations pour concevoir une attaque.

## La défense
Il ne suffit pas de s'appuyer sur les paramètres par défaut pour générer des messages d'erreur à partir des serveurs, des systèmes d'exploitation, des bases de données et des applications Web. Il est important de fournir des messages d'erreur qui fournissent des informations utiles sans révéler de détails inutiles sur le système ou l'application. Pour ce faire, les équipes de sécurité doivent tester les sites et les autres ressources pour déceler divers types d'erreurs et comprendre comment elles y réagissent. L'étape suivante est un examen détaillé du code qui examine la logique de traitement des erreurs.

Une fois qu'une organisation a identifié les vulnérabilités, il est essentiel de combler toute lacune et de s'efforcer d'assurer l'uniformité entre tous les sites, bases de données et applications Web. L'OWASP recommande d'élaborer des politiques précises sur la façon de corriger les erreurs et de déterminer quels renseignements sont fournis aux utilisateurs qui reçoivent des messages d'erreur. Enfin, il est essentiel de consigner les erreurs et d'analyser les données pour détecter les failles ainsi que les tentatives de piratage. Par exemple, si un fichier journal montre que des erreurs génèrent de nombreux messages à partir d'un gestionnaire d'exceptions par défaut, une mise à jour du code est probablement nécessaire.

{% include figure.html file="error_xkcd.jpg" alt="github octocat" width="85%" %}
