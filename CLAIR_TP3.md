
# TP3 Gestion des paquets - CLAIR Mathias
### Binôme absent pour cause de maladie
### Exercice 1. Commandes de base

**Commencez par mettre à jour votre système avec les commandes vues dans le cours.**

```
ckali@serveur:~$ sudo apt update
Atteint :1 http://fr.archive.ubuntu.com/ubuntu disco InRelease
Atteint :2 http://fr.archive.ubuntu.com/ubuntu disco-updates InRelease
Atteint :3 http://fr.archive.ubuntu.com/ubuntu disco-backports InRelease
Atteint :4 http://fr.archive.ubuntu.com/ubuntu disco-security InRelease
Lecture des listes de paquets... Fait
Construction de l'arbre des dépendances
Lecture des informations d'état... Fait
Tous les paquets sont à jour.
serveur@serveur:~$ sudo apt upgrade
Lecture des listes de paquets... Fait
Construction de l'arbre des dépendances
Lecture des informations d'état... Fait
Calcul de la mise à jour... Fait
0 mis à jour, 0 nouvellement installés, 0 à enlever et 0 non mis à jour.

```
**Donnez les commandes répondant aux questions suivantes :**

**1. Quels sont les 5 derniers paquets installés sur votre machine ?**

```
ckali@serveur:~$ grep install /var/log/dpkg.log | sort -r | head -n 5
2019-09-23 12:32:18 status installed initramfs-tools:all 0.131ubuntu19.1
2019-09-23 12:31:36 status installed linux-image-5.0.0-29-generic:amd64 5.0.0-29.31
2019-09-23 12:30:47 status installed plymouth-theme-ubuntu-text:amd64 0.9.4-1ubuntu1
2019-09-23 12:30:47 status installed man-db:amd64 2.8.5-2
2019-09-23 12:30:47 status installed dbus:amd64 1.12.12-1ubuntu1.1

```

**2. Utiliser dpkg et apt pour compter le nombre de paquets installés (ne pas hésiter à consulter le manuel !). Comment explique-t-on la (petite) différence de comptage ?**

```
ckali@serveur:~$ dpkg -l | wc --lines
548
ckali@serveur:~$ apt list -i | wc -l

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

544

```

**3. Combien de paquets sont disponibles en téléchargement ?**

```
ckali@serveur:~$ apt list | wc -l

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

61599

```

**4. Créer un alias “maj” qui met à jour le système**

J'ai trouvé un fichier .bashrc concernant les alias, voici ce qu'il contient:
```
# Alias definitions.                                                                                 
# You may want to put all your additions into a separate file like
# ~/.bash_aliases, instead of adding them here directly.
# See /usr/share/doc/bash-doc/examples in the bash-doc package.
if [ -f ~/.bash_aliases ]; then 
  . ~/.bash_aliases
    fi 

```

J'ai donc créer un fichier  `.bash_aliases`  pour créer mes alias et j'ai écrit la ligne suivante:

```
alias maj='apt upgrade'

```
Après un redémarrage de la machine, on peut à présent mettre le système à jour avec la commande alias ```maj```  (obligation de passer par le sudo pour exécuter cette commande).
```
ckali@serveur:~$ sudo -s
root@serveur:~# maj
Lecture des listes de paquets... Fait
Construction de l'arbre des dépendances
Lecture des informations d'état... Fait
0 mis à jour, 0 nouvellement installés, 0 à enlever et 0 non mis à jour.
root@serveur:~#

```

**5. A quoi sert le paquet fortunes ? Installez-le.**

```
ckali@serveur:~$ ckali@serveur:~$ apt show fortunes
```
On a une liste de détails concernant le package fortunes comme par exemple la version, la section, la source, la taille du package etc ... 
```

Description: Fichiers de données contenant des biscuits chinois
 Il s’agit d’un programme affichant des épigrammes, connues sous le nom de cookies fortune, choisies aléatoirement à partir de fichiers fortune.
 .
 Il y a plus de 15 000 biscuits chinois différents dans ce paquet.
 .
 Vous aurez besoin du paquet fortune-mod pour afficher les biscuits chinois.

```

Le paquet fortunes et un paquet simulant les biscuits de fortunes traditionnelle chinois.

Il faut installer le paquet fortune-mod pour pouvoir afficher les biscuits chinois et inclure /usr/games dans le PATH. 

**6. Quels paquets proposent de jouer au sudoku ?**

```
ckali@serveur:~$ apt search sudoku
En train de trier... Fait
Recherche en texte intégral... Fait
fltk1.1-games/disco 1.1.10-26ubuntu1 amd64
  Boîte à outils Fast Light - exemples de jeux : jeux de dames, sudoku

fltk1.3-games/disco 1.3.4-9ubuntu1 amd64
  Boîte à outils Fast Light - exemples de jeux : jeux de dames, sudoku

gnome-sudoku/disco 1:3.32.0-1 amd64
  Casse-tête Sudoku pour GNOME

hitori/disco 3.31.0-1 amd64
  Jeu de puzzle logique similaire au sudoku

ksudoku/disco 4:18.12.3-0ubuntu1 amd64
  Jeu et Solveur de Sudoku

libqqwing-dev/disco 1.3.4-1.1 amd64
  outil pour générer et résoudre des casse-tête Sudoku (développement)

libqqwing2v5/disco 1.3.4-1.1 amd64
  outil pour générer et résoudre des casse-tête Sudoku (bibliothèque)

nudoku/disco 1.0.0-1 amd64
  ncurses based sudoku games

qqwing/disco 1.3.4-1.1 amd64
  tool for generating and solving Sudoku puzzles (application)

sudoku/disco 1.0.5-2build3 amd64
  Sudoku en mode console

texlive-games/disco 2018.20190227-1 all
  TeX Live : Composition de jeux

```

Il y a de nombreux paquets qui permettent de jouer au sudoku ou des paquets qui contiennent le sudoku ainsi que d'autres jeux.


**7. Lister les derniers paquets installés explicitement avec la commande apt install**

```
ckali@serveur:~$ grep "apt install" /var/log/apt/history.log
Commandline: apt install fortunes
```

## Exercice 2.

**A partir de quel paquet est installée la commande ls ? Comment obtenir cette information en une seule commande, pour n’importe quel programme (indice : la réponse est dans le poly de cours 2, dans la liste des commandes utiles) ?**
```
ckali@serveur: $ which -a ls | tail -n 1 | xargs dpkg -S 
coreutils: /bin/ls

```

**Utilisez la réponse à pour écrire un script appelé origine-commande (sans l’extension .sh) prenant en argument le nom d’une commande, et indiquant quel paquet l’a installée.**

```
if [ -z "$1" ]; then
        echo "no argument, provide a command name."
else
        which -a "$1" | tail -n 1 | xargs dpkg -S
fi

```

## Exercice 3.

**Ecrire une commande qui affiche “INSTALLÉ” ou “NON INSTALLÉ” selon le nom et le statut du package spécifié dans cette commande.**
On peut écrire une commande telle que:
```
(dpkg -l "fortunes" | grep "^i") && echo "installé" || echo "non installé"

```
Cela nous permet de savoir si le paquet est installé ou non.
## Exercice 4.

**Lister les programmes livrés avec coreutils. A quoi sert la commande "[" et comment afficher ce qu’elle retourne ?**

```
ckali@serveur:~$ dpkg -L coreutils | grep bin 

```

La commande "[" est la commande de test. Elle permet d'écrire des conditions dont on va pouvoir visionner les résultats à l'aide des opérateur && et ||.

## Exercice 5. Aptitude

**Installez le paquet emacs à l’aide de la version graphique d’aptitude.**

Pour ne pas surcharger le compte rendu je ne vais pas mettre l'interface graphique en entière. 
Voici les différentes étapes que j'ai suivi pour obtenir le résultat voulu:
J'ai utilisé la fonction de recherche  `/`  pour trouver  emacs, puis j'ai utilisé la touche  `+`  pour accepter d'installer le packet, puis deux fois  `g`  pour approuver le téléchargement et lancer l'installation.

## Exercice 6. Installation d’un paquet par PPA

Certains logiciels ne figurent pas dans les dépôts officiels. C’est le cas par exemple de la version ”officielle” de Java depuis qu’elle est développée par Oracle. Dans ces cas, on peut parfois se tourner vers un ”dépôt personnel” ou PPA.

**1. Installer la version Oracle de Java (avec l’ajout des PPA)**

```
sudo add-apt-repository ppa:linuxuprising/java
sudo apt update
sudo apt install oracle-java11-installer

```

**2. Vérifiez qu’un nouveau fichier a été créé dans /etc/apt/sources.list.d. Que contient-il ?**

Le fichier à bien été crée et il contient :

```
deb http://ppa.launchpad.net/linuxuprising/java/ubuntu disco main
# deb-src http://ppa.launchpad.net/linuxuprising/java/ubuntu disco main
# deb-src http://ppa.launchpad.net/linuxuprising/java/ubuntu disco main
```
### Je n'ai pas réussi à aller plus loin que l'exercice 6 dans ce TP
