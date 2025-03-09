# 42-inception

## Conteneurs

Permettent d'exécuters des applis de manière isolée, sans qu'elles interfèrent entre elles. Ils regroupent tout ce dont une appli à besoin pour fonctionner (code, bibliothèques, dépendances) et tournent sur un même système tout en restant indépendants. 
Contrairement aux VM qui embarquent un système d'exploitation complet, les conteneurs partagent le noyau du système hôte ce qui les rend bcp plus légers et rapides à démarrer. Moins de place, moins de ressources et permettent d'exécuter + d'appli sur une même machine.

## Virtual Machines (VM)

Comme un ordi dans un ordi. Permet de faire tourner plusieurs systèmes indépendants sur une même machien physique. 
Pour que cela fonctionne, un logiciel spécial appelé hyperviseur gère ces machines virtuelles et leur permet de partager les ressources du même ordinateur (processeur, mémoire, disque dur). 
Problème : chaque VM embarque un système d'exploitation complet = ca prend beaucoup de place (plusieurs gigaoctets) et ralentit le démarrage. Plus lourd que les conteneurs, qui sont + rapides et consomment moins de ressources. 

## Dockers

Avant l’arrivée de Docker, imaginons une situation classique : un développeur écrit du code qui fonctionne parfaitement sur son ordinateur. Mais lorsqu’un testeur récupère ce même code pour le tester sur sa machine, cela ne fonctionne pas du tout.

Pourquoi ? Il peut y avoir plusieurs raisons :

- Le testeur n’a pas les bonnes dépendances installées.
- Certaines variables d’environnement nécessaires au bon fonctionnement du programme n’existent pas sur son système.

Ce genre de problème arrive souvent, car les environnements de travail ne sont pas toujours identiques d’une machine à une autre.

Comment résoudre ce problème ?

Docker permet de tout encapsuler dans un conteneur : le code, les dépendances, les variables d’environnement et tout ce qui est nécessaire pour exécuter l’application. Ainsi, peu importe la machine sur laquelle le conteneur tourne, l’application fonctionnera toujours de la même manière.

Avec Docker, plus besoin de dire "ça marche chez moi mais pas chez toi", car tout le monde utilise exactement le même environnement.

## Pourquoi ne pas alors utilisé les VM ?

|   VM    |    Docker   | 
|---    |:-:    |
|  Occupe beaucoup de place en mémoire     |   Occupe beaucoup moins de place en mémoire    | 
|    Démarrage lent   |   Démarrage rapide car il utilise le noyau en cours d'exécution de notre système    |
| Difficile à mettre à l’échelle        |    Facile à mettre à l’échelle    | 
|     Faible efficacité   |  Haute efficacité      | 
|   Le stockage des volumes ne peut pas être partagé entre les machines virtuelles    |   Le stockage des volumes peut être partagé entre l’hôte et les conteneurs    | 


Comment ca marche ?

Moteur Docker = composant central de Docker. C'est un outil léger d'exécution et de packaging qui regroupe notre appli et ses dépendances en un seul package = un conteneur. 

Le moteur Docker comprend le Docker deamon (= un processus en arrière-plan qui gère les conteneurs Docker) + le Docker client (= outil en ligne de commande permettant d'interagir avec le Docker deamon). 

Docker image = modèle / blueprint qui contient tout ce qu'il faut pour exécuter une appli.
=> permet de créer des conteneurs (= environnements isolés ou l'appli pt fonctionner de manière fiable, quel que soit l'ordi ou le serveur sur lequel elle est exécutée.
Elle comprend : le code de l'appli, bibliothèques & dépendances nécessaires, système d'exploitation minimal (ubuntu, alpine...), instructions pour exécuter l'appli.
image = modèle (figé) = recette de cuisine
conteneur = instance en cours d'exécution d'une image = plat

Comment fonctionne le moteur Docker ?
1. Ecriture des instructions : on crée un ficher Dockerfile qui décrit comment construire une image Docker. Le fichier liste tout ce dont notre appli a besoin pour fonctionner (code, dépendances, bibliothèques,...)
2. Création de l'image Docker : on utilise une commande (docker build) pour transformer le Dockerfile en une image Docker = sorte de modèle prêt à l'emploi contenant notre appli + son environnement
3. Lancement du conteneur : une fois l'image créée, on peut l'exécuter avec la commande docker run. Docker va alors créer un conteneur (= instance fonctionnelle de l'image) et y faire tourner notre appli. 
4. Gestion des ressources : Docker s'assure que chaque conteneur fonctionne de manière isolée et sécurisée, tout en optimisant l'utilisation des ressources de notre machine (CPU, mémoire, stockage). 
5. Adminisatration et partage : on peut gérer nos conteneurs en les listant, en les arrêtant ou en les supprimant avec des commandes Docker. Si on veut partager notre image avec d'autre on peut l'envoyer sur un regsitre comme Docker Hub.

Docker permet d'emballer une appli avec tout ce dont elle a besoin et de l'executer facilement, sans se soucier des différences entre les environnements (PC, serveurs...)


## Dockerfile

Dockerfile = fichier principale des Docker images

From = indique à Docker sous quel OS notre VM doit tourner eg : debian:buster (pour Debian) ou alpine:x:xx pour Linux

Run = permet de lancer une commande sur notre VM ; maj des ressources de notre VM comme apk ou ajout d'utilitaire basiques comme vim, curl, sudo

Copy = copier un fichier
NB : une image docker = dossier. Contient Dockerfile à la racine.

Expose = informe Docker que le conteneur écoute sur les ports réseaux spécifiés au moment de l'execution. Expose le port spécifié et le rend dispo QUE pour la communication entre les conteneurs. 

Entrypoint = commande au lancement de notre container. 

## Docker compose 

Docker compose = outil pour aider à définir et à partager des appli multi conteneurs
=> création d'un fichier YAML (.yml) pour définir les services et avec une seule commande tout mettre en route ou tout démonter

Objecti : relier plusieurs image Docjer et les lancer ensemble sans qu'elles perdent leur indépendance. 

### Explication claire de l’architecture des conteneurs pour un site WordPress avec Nginx et MariaDB  

Un site WordPress fonctionne avec plusieurs éléments qui doivent communiquer entre eux. En utilisant Docker, on va organiser ces éléments sous forme de conteneurs pour mieux gérer l’ensemble.  

---

### 1️⃣ Les conteneurs et leurs rôles  

🔹 MariaDB (Base de données) → Stocke les informations  
- Ce conteneur contient toutes les données du site WordPress (articles, pages, utilisateurs, etc.).  
- Il fonctionne comme un cahier de notes dans lequel WordPress enregistre et retrouve ses informations.  
- Il utilise un volume Docker pour que les données restent enregistrées même si le conteneur est redémarré.  
- Port utilisé en interne : 3306 (port standard de MySQL/MariaDB).  

🔹 WordPress (Application web) → Gère le site  
- Ce conteneur exécute le code WordPress.  
- Il récupère les informations depuis MariaDB (exemple : un article à afficher).  
- Il génère les pages du site web à partir de ces données.  
- Il utilise aussi un volume pour stocker certains fichiers (images, thèmes, plugins).  
- Port utilisé en interne : 9000 (généralement, car WordPress avec PHP-FPM écoute sur ce port).  

🔹 Nginx (Serveur web) → Affiche le site aux visiteurs  
- Ce conteneur est chargé de servir le site WordPress aux visiteurs du site.  
- Il reçoit les requêtes des utilisateurs (exemple : un internaute visite la page d’accueil).  
- Il demande à WordPress quoi afficher, puis envoie la réponse au navigateur.  
- Port utilisé : 80 (HTTP) et 443 (HTTPS pour un accès sécurisé).  

---

### 2️⃣ Comment les conteneurs communiquent entre eux ?  
- WordPress ↔️ MariaDB → WordPress envoie des requêtes SQL à MariaDB pour enregistrer ou récupérer des infos (via le port 3306).  
- Nginx ↔️ WordPress → Nginx demande à WordPress (PHP-FPM) de générer une page et affiche ensuite le résultat (via le port 9000).  
- Utilisateurs ↔️ Nginx → Les visiteurs du site accèdent à WordPress via Nginx (via le port 80 ou 443).  

---

### 3️⃣ Pourquoi utiliser des conteneurs ?  
✅ Isolation → Chaque service (WordPress, MariaDB, Nginx) fonctionne séparément, évitant les conflits.  
✅ Facilité de gestion → On peut redémarrer ou mettre à jour un service sans affecter les autres.  
✅ Portabilité → Le site peut être déplacé d’un serveur à un autre sans problème.  
✅ Persistance des données → Les volumes Docker permettent de conserver les fichiers et bases de données même après un redémarrage.  

---

💡 Résumé :  
- MariaDB stocke les données.  
- WordPress génère le site en fonction des données.  
- Nginx affiche le site aux visiteurs.  
- Les ports permettent à ces services de communiquer entre eux. 🚀

## Le sujet 

### 3 containers à mettre en place 
- NGINX (avec TLS v1.2)
- WordPress (avec php-fpm configuré)
- MARIADB (sans NGINX)

### 2 volumes à mettre en place
- Volume contenant votre base de données WordPress
- Volume contenant les fichiers de votre site WordPress

=> dispo à partir du dossier /home/<login>/data de la machine hôte utilisant Docker

### Un docker-network = lien entre les containers

### Utilisateur à créer dans la base de données Wordpress
- un utilisateur admin (qui ne doit pas s'appeler admin)
- un utilisateur standard

Seul point d'entrée de notre Compose doit être par le containeur de NGINX passant par le port 443

Ports qui permettront à WordPress de communiquer en interne avec la base de données (stockage de nouvelles pages ou autre) et NGINX qui communique avec WordPress (qui lui dira quoi afficher sur le server web). Le container MariaDB stocke ses info (bases de données) dans le volume correspondant et Wordpress (site wordpress) dans le sien. 

# Protocole TLSv1.2 ou TLSv1.3

TLS = Transport Layer Security = "sécurité de la couche de transport" = protocole de sécurisation des échanges par réseau informatique, notammaent par Internet

TLS permet :
- authentification serveur
- confidentialité données échangées
- optionnellement : authentification client

# Structure projet
 
 1/ Dossier principal srcs : contient docker-compose.yml, fichier .env, dossier requirements contenant nos 3 container chacun rpz par un dossier portant son nom
 
 2/ Dans chaque dossier de container doit se trouver : 
-son Dockerfile
et en + différents fichier/dossiers de config qu'il serait interessant de copier dans notre container grâce au Dockerfile et son mot-clef COPY aka 
- un fichier conf : contient fichier de configuration du container (eg : config de NGINX pour son container associé)
- fichier .dockerignore : comme .gitignore, précise à Docker les fichiers à ne pas regarder car pas utilisé par Docker
- dossier tools : stock d'autres outils dont on pourrait avoir besoin


