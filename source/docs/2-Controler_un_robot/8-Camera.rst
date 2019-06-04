Streamer une video
==================

Partager un flux vidéo peut s'avérer utile en match. En effet, voir sur son PC
la vue d'une caméra montée sur le robot peut aider le pilote à se positionner
correctement sur le terrain.


Quelle camera ?
---------------

Le RoboRio possède deux ports USB. C'est pourquoi il est possible d'utiliser
des caméras USB avec celui-ci. Ces cameras sont pratiques car diponibles
partout dans le commerce, bon marché, faciles à remplacer et à brancher.

Une des caméras USB les plus utilisées en FRC est la **Microsoft LifeCam HD-3000** :

.. image:: img/Camera.jpg

Il existe aussi des caméras IP qui permettent directement de streamer un flux
vidéo sur le réseau. Cependant, celles-ci sont de moins en moins utilisées.


Dans le code
------------

Bien évidemen, WPILib a créé une classe afin de streamer le flux vidéo d'une
caméra USB. Cette classe est la classe 