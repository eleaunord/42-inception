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

