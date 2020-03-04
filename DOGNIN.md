# TP3 Gestion des paquets - DOGNIN Alexis
### Exercice 1. Commandes de base

Ayant Linux Mint 19.3 sur mon ordinateur, j'ai fait tourner quelques commandes directement dessus

**Commencez par mettre à jour votre système avec les commandes vues dans le cours.**
On utilise les commandes
```
ADognin@PC:~$ sudo apt update ; sudo apt upgrade
Atteint :1 http://archive.canonical.com/ubuntu bionic InRelease
Atteint :2 http://archive.ubuntu.com/ubuntu bionic InRelease                   
Atteint :3 http://ppa.launchpad.net/cybermax-dexter/sdl2-backport/ubuntu bionic InRelease
Atteint :4 http://security.ubuntu.com/ubuntu bionic-security InRelease         
Atteint :5 http://archive.ubuntu.com/ubuntu bionic-updates InRelease           
Atteint :6 http://archive.ubuntu.com/ubuntu bionic-backports InRelease         
Atteint :7 http://ppa.launchpad.net/rael-gc/ubuntu-xboxdrv/ubuntu bionic InRelease
Ign :8 http://packages.linuxmint.com tricia InRelease                          
Atteint :9 http://packages.linuxmint.com tricia Release                        
Atteint :10 http://repo.steampowered.com/steam precise InRelease               
Lecture des listes de paquets... Fait                          
Construction de l'arbre des dépendances       
Lecture des informations d'état... Fait
Tous les paquets sont à jour.
Lecture des listes de paquets... Fait
Construction de l'arbre des dépendances       
Lecture des informations d'état... Fait
Calcul de la mise à jour... Fait
0 mis à jour, 0 nouvellement installés, 0 à enlever et 0 non mis à jour.

```
**Donnez les commandes répondant aux questions suivantes :**

**1. Quels sont les 5 derniers paquets installés sur votre machine ?**

```
ADognin@PC:~$ grep install /var/log/dpkg.log | sort -r | head -n 5
2020-03-03 19:25:11 status installed libfaudio0:i386 20.03-bionic~1ppa1
2020-03-03 19:25:11 status installed libfaudio0:amd64 20.03-bionic~1ppa1
2020-03-03 19:25:11 status installed libc-bin:amd64 2.27-3ubuntu1
2020-03-03 19:25:11 status half-installed libfaudio0:i386 20.02-bionic~1ppa1
2020-03-03 19:25:11 status half-installed libfaudio0:i386 20.02-bionic~1ppa1
```

**2. Utiliser dpkg et apt pour compter le nombre de paquets installés (ne pas hésiter à consulter le manuel !). Comment explique-t-on la (petite) différence de comptage ?**

```
ADognin@PC:~$ dpkg -l | wc --lines
2226
ADognin@PC:~$ apt list -i | wc -l
2010
```
On note ici un avertissement du terminal à propos de la stabilité de apt.

**3. Combien de paquets sont disponibles en téléchargement ?**

```
ADognin@PC:~$ apt list | wc -l
65672
```

**4. Créer un alias “maj” qui met à jour le système**

Dans le fichier .bashrc, il est mentionné de créer un fichier .bash_aliases afin de ne pas altérer le .bashrc:
```
# Alias definitions.
# You may want to put all your additions into a separate file like
# ~/.bash_aliases, instead of adding them here directly.
# See /usr/share/doc/bash-doc/examples in the bash-doc package.

if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi
```

Je crée donc le fichier `.bash_aliases` et j'y écris cette instruction:

```
alias maj='apt update ; apt upgrade'
```
Après un redémarrage de la machine, on peut mettre le système à jour avec la commande alias ```maj```.
Il faut bien entendu l'executer en temps que root de la machine.
```
ADognin@PC:~$ sudo -s
root@PC:~# maj
Lecture des listes de paquets... Fait
Construction de l'arbre des dépendances
Lecture des informations d'état... Fait
0 mis à jour, 0 nouvellement installés, 0 à enlever et 0 non mis à jour.
```

**5. A quoi sert le paquet fortunes ? Installez-le.**

```
ADognin@PC:~$ apt show fortunes
```
On obtient alors des informations détaillées à propos du paquet.
```
Package: fortunes
Version: 1:1.99.1-7build1
Priority: optional
Section: universe/games
Source: fortune-mod
Origin: Ubuntu
Maintainer: Andrea Colangelo <warp10@ubuntu.com>
Bugs: https://bugs.launchpad.net/ubuntu/+filebug
Installed-Size: 2 670 kB
Provides: fortune-cookie-db
Depends: fortunes-min
Recommends: fortune-mod (>= 9708-12)
Download-Size: 833 kB
APT-Sources: http://archive.ubuntu.com/ubuntu bionic/universe amd64 Packages
Description: Fichiers de données contenant des biscuits chinois
 Il s’agit d’un programme affichant des épigrammes, connues sous le nom de
 cookies fortune, choisies aléatoirement à partir de fichiers fortune.
 .
 Il y a plus de 15 000 biscuits chinois différents dans ce paquet.
 .
 Vous aurez besoin du paquet fortune-mod pour afficher les biscuits
 chinois.
```

Le paquet fortunes affiche des épigrammes nommées cookies fortunes.

Pour pouvoir afficher les biscuits chinois, nous avons besoin d'installer le paquet fortune-mod et d'inclure /usr/games dans le chemin. 

**6. Quels paquets proposent de jouer au sudoku ?**

```
ADognin@PC:~$ apt search sudoku
p   gnome-sudoku                    - Casse-tête Sudoku pour GNOME             
p   gnome-sudoku:i386               - Casse-tête Sudoku pour GNOME             
p   ksudoku                         - Jeu et Solveur de Sudoku                 
p   ksudoku:i386                    - Jeu et Solveur de Sudoku                 
p   sudoku                          - Sudoku en mode console                   
p   sudoku:i386                     - Sudoku en mode console  
```

Plusieurs paquets permettent de jouer au Sudoku, sachant que chacun à 2 version, une pour chaque architecture (32bits/64bits)

**7. Lister les derniers paquets installés explicitement avec la commande apt install**

```
ADognin@PC:~$ grep "apt install" /var/log/apt/history.log
Commandline: apt install fortunes
```

## Exercice 2.

**A partir de quel paquet est installée la commande ls ? Comment obtenir cette information en une seule commande, pour n’importe quel programme (indice : la réponse est dans le poly de cours 2, dans la liste des commandes utiles) ?**
```
ADognin@PC: $ which -a ls | tail -n 1 | xargs dpkg -S 
coreutils: /bin/ls

```

**Utilisez la réponse à pour écrire un script appelé origine-commande (sans l’extension .sh) prenant en argument le nom d’une commande, et indiquant quel paquet l’a installée.**

```
if [ -z "$1" ]; then
        echo "Erreur"
else
        which -a "$1" | tail -n 1 | xargs dpkg -S
fi

```

## Exercice 3.

**Ecrire une commande qui affiche “INSTALLÉ” ou “NON INSTALLÉ” selon le nom et le statut du package spécifié dans cette commande.**
On utilise cette commande:
```
(dpkg -l "nom_du_paquet" | grep "^i") && echo "installé" || echo "non installé"

ADognin@PC:~$ (dpkg -l "python" | grep "^i") && echo "installé" || echo "non installé"
ii  python         2.7.15~rc1-1 amd64        interactive high-level object-oriented language (default version)
installé

ADognin@PC:~$ (dpkg -l "pip" | grep "^i") && echo "installé" || echo "non installé"
dpkg-query: aucun paquet ne correspond à pip
non installé
```
On peut alors savoir si un paquet a été installé ou non.

## Exercice 4.

**Lister les programmes livrés avec coreutils. A quoi sert la commande "[" et comment afficher ce qu’elle retourne ?**

```
ADognin@PC:~$ dpkg -L coreutils | grep bin 
```
La liste étant très longue, j'ai décidé de ne pas l'inclure pour éviter de nuire à la clarté de ce rapport. On peut cependant noter quelques commandes comme mkdir, rm, chmod etc.
"[" est une commande de test. Elle permet d'écrire des conditions dont on va pouvoir visionner les résultats à l'aide des opérateur && et ||.

## Exercice 5. Aptitude

**Installez le paquet emacs à l’aide de la version graphique d’aptitude.**

Pour ne pas surcharger le compte rendu je ne vais pas mettre l'interface graphique en entière. 
Voici les différentes étapes que j'ai suivi pour installer emacs:
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

## Exercice 7. Création de dépôt personnalisé
J'ai réussi à obtenir mon paquet, mais cependant je n'arrive pas à signer mon dépôt.
Après plusieurs tentatives infructueuses, j'ai décider de m'arrêter à la question 7 de la deuxième partie de cet exercice.

## Exercice 8. Installation d’un logiciel à partir du code source
J'ai eu des problèmes avec l'installation de gettext.
En effet, pour l'installation de nudoku, la version de gettext doit être supérieure à 0.20. Cependant, la version la plus récente d'après apt est la 0.19.8.1
J'ai du télécharger le paquet sur le site de GNU, et j'ai ensuite pu installer nudoku sans problème.
