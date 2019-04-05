# Cours WpiLib

Bienvenue dans ce tutoriel écrit par l'équipe 5553, Robo'Lyon, de France.

Son but est de permettre aux rookies de s'initier à la programmation d'un robot FRC en C++ avec la librairie WpiLib.

## Structure du projet

Les sources du cours sont écrites en Markdown et sont situées dans le dossier `docs/`. Chaque page du cours correspond ainsi à un fichier `.md`. Les images du cours sont situées dans le dossier `docs/img/`.

Le fichier `mkdocs.yml` sert à configurer la converssion des fichiers .md en pages web.

## Fonctionnement du projet

Le site utilise [mkdocs](http://www.mkdocs.org) pour être créé. Pour l'installer suivre la doc officielle disponible [ici](https://www.mkdocs.org/#manual-installation).

- Pour tester le site en local, il faut executer la commande `mkdocs serve` et aller à l'adresse : http://127.0.0.1:8000, la page sera rafraichie automatiquement à chaque modification des sources.

- Pour build le site en local, entrer la commande `mkdocs build`. Les sources du site seront crées dans le dossier `site/`.

- Pour deployer les modifications, entrer la commande `mkdocs gh-deploy`. Il faut bien sûr posséder les droits d'écriture sur ce repository github.