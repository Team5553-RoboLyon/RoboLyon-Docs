# Cours WpiLib

Read the docs | Travis
:------------:|:-----:
[![Documentation Status](https://readthedocs.org/projects/cours-wpilib/badge/?version=latest)](https://cours-wpilib.readthedocs.io/fr/latest/?badge=latest) | [![Build Status](https://travis-ci.com/Team5553-RoboLyon/Cours-WpiLib.svg?branch=master)](https://travis-ci.com/Team5553-RoboLyon/Cours-WpiLib)

Bienvenue dans ce tutoriel écrit par l'équipe 5553, Robo'Lyon, de France.

Son but est de permettre aux rookies de s'initier à la programmation d'un robot FRC en C++ avec la librairie WpiLib.


## Structure du projet

Les sources du cours sont écrites en [reStructuredText](https://github.com/ralsina/rst-cheatsheet/blob/master/rst-cheatsheet.rst) et sont situées dans le dossier `source/docs/`. Chaque page du cours correspond ainsi à un fichier `.rst`. Les images du cours sont situées dans le dossier `source/docs/img/`.

Le fichier [source/conf.py](source/conf.py) sert à configurer la conversion des fichiers .rst en pages web.


## Installation

Le site utilise [Sphinx](https://www.sphinx-doc.org/en/1.5/tutorial.html) pour être créé. Pour pouvoir compiler les docs, il est nécessaire d'installer [Python](https://www.python.org/downloads/) puis Sphinx et ses dépendances :
```py
pip install -r source/requirements.txt
```


## Tester et déployer le site

- Pour tester le site en local, exécuter la commande `make livehtml` (`.\make livehtml` sur Windows) et aller à l'adresse : http://127.0.0.1:8000, la page sera rafraîchie automatiquement à chaque modification des sources.

- Pour build le site en local, entrer la commande `make html` (`.\make html` sur Windows). Les sources du site seront générées dans le dossier `build/html`.


## Déploiement automatique et Tests

### Read The Docs

Ce projet utilise [Read the Docs](https://docs.readthedocs.io/en/stable/index.html) pour déployer automatiquement le cours en ligne.

A chaque nouveau commit sur la branche master, un `build` est lancé par Read the Docs. Celui-ci lit alors le fichier [readthedocs.yml](readthedocs.yml) qui va installer les dépendances nécessaires :
```yml
python:
  install:
    - requirements: source/requirements.txt
```

Puis Read the Docs génère le site grâce à Sphinx et le met en ligne à l'adresse : https://cours-wpilib.readthedocs.io/fr/latest/.


### Travis CI

De même que Read the Docs, [travis-ci](https://travis-ci.com/) lance un `build` à chaque nouveau commit sur la branche master **mais aussi à chaque nouvelle Pull Request**. Celui-ci lit alors le fichier [.travis.yml](.travis.yml) qui va installer les dépendances nécessaires :
```yml
install:
  - pip install -r source/requirements.txt
```

Puis le site est généré grâce à la commande `make html` :
```yml
script:
  - make html
```

Ainsi Travis permet de tester chaque Pull Request avant qu'elle ne soit "mergée" dans master.

De plus, Travis est aussi utilisé pour générer et mettre en ligne un fichier pdf avec la totalité du cours. Ainsi, si l'étape `script` est réussie, les étapes `before_deploy` et `deploy` vont être éxecutées.

Premièrement, les docs vont être générées au sein d'une unique page web. Puis le programme [wkhtmltopdf](https://wkhtmltopdf.org/) va installé et appelé pour convertir la page en un fichier pdf, `Tutoriel.pdf` :
```yml
before_deploy:
  - sphinx-build -b singlehtml source build/singlehtml
  - sudo apt-get update && sudo apt-get install -y wkhtmltopdf
  - wkhtmltopdf ./build/singlehtml/index.html Tutoriel.pdf
```

Enfin, Travis va deployer le fichier `Tutoriel.pdf` par l'intermédiaire d'une release Github que l'on pourra retrouver dans l'onglet `releases` du repository :
```yml
deploy:
  provider: releases
  api_key: $GITHUB_TOKEN
  file: "Tutoriel.pdf"
  skip_cleanup: true
```

Pour déployer le pdf, travis à besoin d'avoir les droits d'écriture sur ce repository. Une [variable environnement](https://docs.travis-ci.com/user/environment-variables#defining-variables-in-repository-settings) nommée `GITHUB_TOKEN` doit donc être configurée. Elle a pour valeur l'ID d'un [token github](https://help.github.com/en/articles/creating-a-personal-access-token-for-the-command-line) qui permet d'avoir les droits d'écriture sur ce repository. Il faut donc cocher l'option `repo` pendant la création du token.

[Générer un token github](https://github.com/settings/tokens/new?description=Cours-Wpilib-Autodeploy&scopes=repo)


## Extensions

Deux extensions sont utilisées dans le projet. Elle sont incluses par le fichier [conf.py](source/conf.py) avec l'instruction :
```py
extensions = ['sphinx.ext.imgmath', 'globalindex']
```

### Global Index

L'extension [Global Index](http://fnch.users.sourceforge.net/sphinxindexinsinglehtml.html) permet d'afficher le sommaire dans la page web unique (la page web regroupant toutes les sections) et donc dans le document pdf généré par Travis. En effet, avec la commande `sphinx-build -b singlehtml`, les directives `.. toctree::` (qui permettent de créer le sommaire) placées dans [index.rst](source/index.rst) ne sont pas affichées.

Cette extension explique la présence du fichier [globalindex.py](extensions/globalindex.py) dans le dossier `extensions`.


### Imgmath

L'extension [Imgmath](https://www.sphinx-doc.org/en/1.8/usage/extensions/math.html#module-sphinx.ext.imgmath) permet de convertir les expressions mathématiques en images. Ceci est encore une fois utile pour la création du pdf qui ne peux pas afficher les expressions mathématiques.