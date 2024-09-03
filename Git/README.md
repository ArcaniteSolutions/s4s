## Système de Contrôle de Version (Git)
### Version Control Systems
+ Il s'agit de logiciels permettant de garder une trace des changements dans un projet et de garder plusieurs versions d'un même projet
  + Pourquoi garder une trace des changements?
    + Retrouver quand/pourquoi/par qui quelque chose a été changé
    + Revenir à une version antérieure
  + Pourquoi avoir plusieurs versions ?
    + Plusieurs personnes travaillent sur le même projet
    + Dévelopement incrémental (une version qui fonctionne et ajouter peu à peu des fonctionnalités)
    + Dans l'industrie: Version "en production", "à tester" et "de développement", voir plusieurs versions pour différents utilisat·eur·rice·s
+ [**Git**](https://git-scm.com/docs/gittutorial) est le VCS le plus utilisé aujourd'hui
### Git basics
+ Avec Git, l'historique du projet est organisé en **graphe**. Chaque point du graphe, appelé **commit**, correspond à l'état à un instant T du projet. Chaque commit correspond enfait à un pointeur (= un lien) vers son parent, ainsi que les différences ("**diff**") avec son parent. À chaque commit est aussi associé un identifiant unique ("hash")

![](https://github.com/ArcaniteSolutions/s4s/blob/main/Git/git_graph.png)

+ Git offre un système de **branches**. Un projet a au minimum une branche (souvent appelée "main" ou anciennement "master" par convention)
  + Une branche peut exister en local et/ou en remote
  + Lorsqu'une branche locale est publiée (`push`) en remote, elle peut être liée à une branche remote (voir `--set-upstream`) pour que toutes les prochaines instructions de "push"/"pull" de cette branches locale soient liées à la branche remote correspondante
  + Lorsqu'une branche remote est pulled pour la première fois, une branche locale du même nom est crée
  + Les branches remotes sont affichées en local comme "origin/nom-de-la-branche"
  + Une bonne pratique est de développer sur des branches différentes pour chaque fonctionnalité
    + Quand une étape de dévelopement est atteinte, une **Merge Request** (MR) (appelée Pull Request (PR sur GitHub)) peut être créée
    + Une MR (Merge Request) est souvent l'opportunité de faire relire et valider son code par un pair
    + Des outils d'intégration continue (Continuous Integration - CI) peuvent être mis en place pour vérifier qu'une MR remplit certains critères (e.g. passe certains tests) avant d'autoriser qu'elle soit merged
    + Il existe plusieurs façon de transférer les modifications d'une branches à une autre. On parle de **merge** ou de **rebase** avec différentes options possibles. Quelque soit l'option choisie, le résultat final du contenu du projet devrait être le même (pour autant que les même choix soient faits au moment de la résolution de conflits). La différence entre ces méthodes est dans **l'historique du graphe**
      
![](https://github.com/ArcaniteSolutions/s4s/blob/main/Git/git_graph_merge_or_rebase.png)

Dans l'exemple ci-dessus, une branche a été crée depuis le 2ème commit de la branche principale.
  + La première option, merge, crée un nouveau commit sur la branche main avec les diffs de toute la branche ainsi que les choix de résolution de conflits. 
  + La deuxième option, rebase, réécrit l'historique pour simuler le fait que la branche aurait été basée sur le dernier commit actuel de la branche principale. La résolution des conflits peut-être diffusée dans les différents commits de la branche, qui sont sinon copiés. Il est important de noter que même si le contenu d'un commit ne change pas dans le sens que le diff reste le même, son hash sera différent. En effet son pointeur vers son parent a changé, ce qui veut dire que le commit n'est plus parfaitement identique.

+ Comme Git est un Système de Contrôle de Version Distribué, plusieurs copies du projet existent: locale ("**local**") et distante ("**remote**") (sur un serveur)
+ La version locale peut être divisée en trois "parties": le **workspace**, la **staging area** et le **local repository**
    + Le **local repository** correspond à toutes les branches et l'historique du projet comme sauvegardé sur la machine locale.
    + Le **Workspace** est l'espace de travail dans lequel les modifications sont faites
    + La **Staging area** est l'espace où sont sauvegardés les changements avant d'être copiés sur le local repository
+ Des changements dans un projets sont réunis en un **commit**. Lorsqu'un commit est créé, on ajoute un **message de commit** (voir l'option `-m` dans `git commit -m`)
    + Un bon message de commit peut faire économiser beaucoup de temps et d'énergie plus tard
    + Un bon message de commit fait idéalement < 50 charactères.
    + Un bon message de commit indique ce qui a été changé (fix, nouvelle fonctionnalité) et pourquoi
+ Cheat sheet:
![](https://github.com/ArcaniteSolutions/s4s/blob/main/Git/git_cheatsheet_bg.png)
+ Conseils d'utilisation de git:
    + En CLI (Command Line Interface - dans un terminal), les commandes git donnent souvent des indications supplémentaires sur quelles commandes sont disponibles et ce qu'elles font. C'est souvent intéressant de bien lire ces informations et de chercher plus d'informations sur les commandes inconnues
    + `git status` permet de facilement voir la branche locale courante, la branche remote associée (et si il y a des commits de différence entre les deux), ainsi que les modifications dans la staging area, dans le workspace, ainsi que les modifications non-traquées (i.e. les fichiers ne sont pas connus de git)
    + `git log --oneline` permet de voir l'historique de la branche courante. Les noms de branches qui apparaissent dans cet historique indiquent les branches dont le dernier commit est le commit associé. /!\ les branches remotes (origin/nom-de-branche) indiquent l'état connu du remote, c'est à dire l'état au dernier "fetch"
    + `git add -p` permet de parcourir les modifications du workspace une par une pour décider lesquelles ajouter à la staging area. Passer par un `git add -p` avant de commit (plutôt que d'ajouter fichier par fichier par exemple) permet à la fois de relire et de séparer ce qui doit être commit ou non
    + Conseil plus générique mais qui s'appplique bien à git: ne pas hésiter à créer des alias faciliter l'écriture de longues commandes utilisées régulièrements (par exemple `git log --oneline` -> `glo`), c'est un bon cas d'utilisation pour découvrir comment créer des alias (chercher "alias" + nom de votre shell)

+ Il existe un [Epfl git tutorial](https://wiki.epfl.ch/git-epfl/documents/git.html). Les exemples de création de repo ne sont plus à jour (parce que l'hébergement de serveur git de l'EPFL a changé d'addresse) mais le reste du matériel est encore pertinent
### GitLab & GitHub
+ Gitlab et Github permettent de sauvegarder "dans le cloud", c'est à dire sur des serveurs, la version distante d'un projet utilisant git
+ L'EPFL a son propre serveur gitlab (gitlab.epfl.ch)
