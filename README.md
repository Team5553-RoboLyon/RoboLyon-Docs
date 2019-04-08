# Cours WpiLib

[![Build Status](https://travis-ci.com/Team5553-RoboLyon/Cours-WpiLib.svg?branch=master)](https://travis-ci.com/Team5553-RoboLyon/Cours-WpiLib)

Bienvenue dans ce tutoriel écrit par l'équipe 5553, Robo'Lyon, de France.

Son but est de permettre aux rookies de s'initier à la programmation d'un robot FRC en C++ avec la librairie WpiLib.


## Structure du projet

Les sources du cours sont écrites en Markdown et sont situées dans le dossier `docs/`. Chaque page du cours correspond ainsi à un fichier `.md`. Les images du cours sont situées dans le dossier `docs/img/`.

Le fichier [mkdocs.yml](mkdocs.yml) sert à configurer la conversion des fichiers .md en pages web.


## Installation

Le site utilise [mkdocs](http://www.mkdocs.org) pour être créé. Pour l'installer suivre [ces instructions](https://www.mkdocs.org/#manual-installation) avec la méthode `pip install mkdocs`. Puis installer les extensions PyMdown avec la commande `pip install pymdown-extensions`.


## Tester et deployer le site

- Pour tester le site en local, executer la commande `mkdocs serve` et aller à l'adresse : http://127.0.0.1:8000, la page sera rafraichie automatiquement à chaque modification des sources.

- Pour build le site en local, entrer la commande `mkdocs build`. Les sources du site seront crées dans le dossier `site/`.

- Pour deployer les modifications, entrer la commande `mkdocs gh-deploy`. Il faut bien sûr posséder les droits d'écriture sur ce repository github.


## Deploiement automatique

Ce projet utilise [travis-ci](https://travis-ci.com/) pour deployer automatiquement le cours en ligne.

A chaque nouveau commit sur la branche master ou à chaque nouvelle pull request, un `build` est lancé par travis. Celui-ci lit alors le fichier [.travis.yml](.travis.yml) qui va installer les dépendances nécessaires (`pip install mkdocs pymdown-extensions`) et deployer le site grâce à la commande `mkdocs gh-deploy`.

Pour deployer le site, travis à besoin d'avoir les droits d'écriture sur ce repository. Une [variable environnement](https://docs.travis-ci.com/user/environment-variables#defining-variables-in-repository-settings) nommée `GITHUB_TOKEN` doit donc être configurée. Elle a pour valeur l'ID d'un [token github](https://help.github.com/en/articles/creating-a-personal-access-token-for-the-command-line) qui permet d'avoir les droits d'écriture sur ce repository. Il faut donc cocher la case `repo` pendant sa création. Cette variable est récupérée dans le build grâce à l'instruction :
```sh
echo -e "machine github.com\n  login $GITHUB_TOKEN" > ~/.netrc
```