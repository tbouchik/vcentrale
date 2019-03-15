---
title: Côté client 
nav: true
---

Qu'est-ce que le web ? Autrefois, il s'agissait d'une architecture client/serveur simple (le client
était votre navigateur web, le serveur était une machine sur le réseau qui pouvait livrer
texte et images statiques dans votre navigateur).
- Dans le bon vieux temps, le serveur était beaucoup plus complexe que le côté client : ce dernier ne prenait pas en charge l'interactivité riche, mais le serveur pouvait s'interfacer avec les navigateurs bases de données,autres serveurs, etc.
- Parce que le serveur était beaucoup plus compliqué, la "sécurité web" s'est concentrée sur le côté serveur. Jusqu'à présent, cette classe s'est largement concentrée sur le côté serveur (par exemple, les débordements de tampon sur les serveurs Web, la séparation des privilèges, la mauvaise gestion d'exceptions...).

Le web a changé : maintenant le navigateur est très compliqué:
- JavaScript: permet à une page d'exécuter côté client code.
- Le modèle DOM fournit une interface JavaScript au HTML de la page, permettant à la page d'ajouter/supprimer des balises, changer leur style, etc.
- XMLHttpRequests (AJAX): permet au client de réaliser des requêtes HTTP asynchrones.
- Web Sockets : Communication client/serveur duplex complète sur TCP.
- Géolocalisation : Le navigateur peut déterminer votre position en examinant les unités GPS. Firefox peut également vous localiser en transmettant vos informations WiFi au service de localisation Google.
- Nacl : Permet aux navigateurs d'exécuter du code natif !

# XSS Cross-site scripting
Les attaques XSS (Cross-Site Scripting) sont un type d'injection, dans lequel des scripts malveillants sont injectés dans des sites Web autrement bénins et fiables. Les attaques XSS se produisent lorsqu'un attaquant utilise une application Web pour envoyer un code malveillant, généralement sous la forme d'un script côté navigateur, à un utilisateur final différent. Les failles qui permettent à ces attaques de réussir sont très répandues et se produisent partout où une application Web utilise l'entrée d'un utilisateur dans la sortie qu'elle génère sans la valider ou l'encoder.

{% include figure.html file="xss.png" alt="github octocat" width="95%" %}

Un attaquant peut utiliser XSS pour envoyer un script malveillant à un utilisateur peu méfiant. Le navigateur de l'utilisateur final n'a aucun moyen de savoir que le script n'est pas digne de confiance et exécute le script. Parce qu'il pense que le script provient d'une source fiable, le script malveillant peut accéder à tous les cookies, jetons de session ou autres informations sensibles conservées par le navigateur et utilisées avec ce site.

Les attaques XSS peuvent généralement être classées en deux catégories :"stored" et "reflected":

## Attaques de type stored XSS
Les attaques stockées sont celles dont le script injecté est stocké en permanence sur les serveurs cibles, comme dans une base de données, un forum de messages, un journal des visiteurs, un champ commentaire, etc. La victime récupère ensuite le script malveillant du serveur lorsqu'elle demande les informations stockées. L'XSS stocké est aussi parfois appelé XSS persistant ou XSS de type I.

## Attaques de type reflected XSS
Les attaques réfléchies sont celles où le script injecté est reflété à partir du serveur Web, par exemple dans un message d'erreur, un résultat de recherche ou toute autre réponse qui inclut tout ou partie de l'entrée envoyée au serveur dans le cadre de la requête. Les attaques réfléchies sont transmises aux victimes par un autre moyen, tel qu'un message électronique ou un autre site Web. Lorsqu'un utilisateur est amené à cliquer sur un lien malveillant, à soumettre un formulaire spécialement conçu à cet effet, ou même à naviguer sur un site malveillant, le code injecté se rend sur le site Web vulnérable, ce qui reflète l'attaque sur le navigateur de l'utilisateur. Le navigateur exécute alors le code parce qu'il provient d'un serveur "de confiance". Le XSS réfléchi est aussi parfois appelé XSS non persistant ou XSS de type II.

## Exemple d'attaque
Supposons que nous avons une page d'erreur, qui traite les demandes pour une page non existante, une page d'erreur 404 classique. Nous pouvons utiliser le code ci-dessous à titre d'exemple pour informer l'utilisateur de la page manquante :
```
<html>
<body>

<? php
print "Not found: " . urldecode($_SERVER["REQUEST_URI"]);
?>

</body>
</html>
```

Voyons comment ça marche :

```
http://site_vulnerable.com/fichier_non_existant

```
En réponse nous obtenons :
```
Not found: /fichier_non_existant
```
Maintenant nous allons essayer de forcer la page d'erreur à inclure notre code :
```
http://site_vulnerable.com/<script>alert("TEST");</script>
```
Nous obtenons alors: 
```
Not found: / (mais avec le code JS <script>alert("TEST");</script>)
```
Nous avons réussi à injecter le code, notre XSS ! Qu'est-ce que cela signifie ? Par exemple, que nous pouvons utiliser cette faille pour essayer de voler le cookie de session d'un utilisateur.

# Cross-Site Request Forgery (CSRF)
Cross-Site Request Forgery (CSRF) est une attaque qui force un utilisateur final à exécuter des actions indésirables sur une application Web dans laquelle il est actuellement authentifié. Les attaques CSRF ciblent spécifiquement les demandes de changement d'état, et non le vol de données, puisque l'attaquant n'a aucun moyen de voir la réponse à la demande falsifiée. Avec un peu d'aide de l'ingénierie sociale (comme l'envoi d'un lien par e-mail ou par chat), un attaquant peut amener les utilisateurs d'une application web à exécuter des actions de son choix. Si la victime est un utilisateur normal, une attaque CSRF réussie peut forcer l'utilisateur à effectuer des demandes de changement d'état comme transférer des fonds, changer son adresse e-mail, etc. Si la victime est un compte administratif, le CSRF peut compromettre l'ensemble de l'application Web.

La CSRF est une attaque qui amène la victime à soumettre une requête malveillante. Il hérite de l'identité et des privilèges de la victime pour exercer une fonction non désirée au nom de la victime. Pour la plupart des sites, les demandes de navigateur incluent automatiquement toutes les informations d'identification associées au site, telles que le cookie de session de l'utilisateur, l'adresse IP, les identifiants de domaine Windows, etc. Par conséquent, si l'utilisateur est actuellement authentifié sur le site, le site n'aura aucun moyen de distinguer entre la fausse demande envoyée par la victime et une demande légitime envoyée par la victime.

{% include figure.html file="csrf.png" alt="github octocat" width="95%" %}

CSRF attaque la fonctionnalité cible qui provoque un changement d'état sur le serveur, comme le changement de l'adresse e-mail ou du mot de passe de la victime, ou l'achat de quelque chose. Forcer la victime à récupérer des données ne profite pas à un attaquant parce que l'attaquant ne reçoit pas la réponse, la victime si. Ainsi, les attaques du CSRF ciblent les demandes de changement d'état.

Il est parfois possible de stocker l'attaque CSRF sur le site vulnérable lui-même. De telles vulnérabilités sont appelées " défauts CSRF stockés ". Pour ce faire, il suffit de stocker une balise IMG ou IFRAME dans un champ acceptant le HTML, ou de lancer une attaque de script intersite plus complexe. Si l'attaque peut stocker une attaque CSRF sur le site, la gravité de l'attaque est amplifiée. En particulier, la probabilité est accrue parce que la victime est plus susceptible de voir la page contenant l'attaque que certaines pages aléatoires sur Internet. La probabilité est également accrue parce que la victime est sûre d'être déjà authentifiée sur le site.

## Exemple: Scénario avec un GET
Si l'application a été conçue pour utiliser principalement les requêtes GET pour transférer des paramètres et exécuter des actions, l'opération de transfert d'argent peut être réduite à une requête comme :

```
GET http://bank.com/transfer.do?acct=BOB&amount=100 HTTP/1.1
```

Maria décide maintenant d'exploiter cette vulnérabilité de l'application web en utilisant Alice comme victime. Maria construit d'abord l'URL d'exploitation suivante qui transférera 100 000 $ du compte d'Alice à son compte. Elle prend l'URL de la commande originale et remplace le nom du bénéficiaire par le sien, ce qui augmente considérablement le montant du transfert :

```
http://bank.com/transfer.do?acct=MARIA&amount=100000
```

L'aspect d'ingénierie sociale de l'attaque incite Alice à charger cette URL lorsqu'elle est connectée à l'application bancaire. Cela se fait généralement avec l'une des techniques suivantes :

- l'envoi d'un e-mail non sollicité avec un contenu HTML
- la mise en place d'une URL ou d'un script d'exploitation sur les pages qui sont susceptibles d'être visitées par la victime alors qu'elle effectue également des opérations bancaires en ligne

L'URL d'exploitation peut être déguisée en lien ordinaire, encourageant la victime à cliquer dessus :

```
<a href="http://bank.com/transfer.do?acct=MARIA&amount=100000">View my Pictures!</a>
```
Ou comme une fausse image 0x0 :
```
<img src="http://bank.com/transfer.do?acct=MARIA&amount=100000" width="0" height="0" border="0">
```
Si cette balise d'image était incluse dans le courriel, Alice ne verrait rien. Toutefois, le navigateur soumettra toujours la demande à bank.com sans aucune indication visuelle que le transfert a eu lieu.