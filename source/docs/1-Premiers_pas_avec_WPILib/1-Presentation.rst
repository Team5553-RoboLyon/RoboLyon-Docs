WpiLib, c'est quoi ?
====================

Comment contrôle-t-on un robot ?
--------------------------------

Le robot est contrôlé par le RoboRio, le cerveau du robot. C'est lui que
nous allons programmer. Il communique avec de nombreux composants du robot
pour, par exemple, envoyer des ordres aux moteurs et aux vérins. Il reçoit
aussi les informations venant des capteurs du robot (encodeurs, ultrasons,
limit switch).

Le RoboRio est connecté par ethernet à la borne wifi du robot. Il peut ainsi
communiquer par wifi avec l'ordinateur du pilote.

Voici un schéma des branchements du robot pour mieux comprendre :

.. image:: img/Schema.jpg

Sur l'ordinateur du pilote, c'est le programme appelé ``FRC Driver Station``
qui permet de communiquer avec le robot. Celui-ci permet au pilote de
contrôler le robot avec son joystick.


Et WpiLib dans tout ça ?
------------------------

Présentation
~~~~~~~~~~~~

WpiLib (Worcester Polytechnic Institute Robotics Library) est une
librairie qui permet la programmation du RoboRio. C'est un ensemble
de classes qui permettent l'interface entre le logiciel et le hardware.

.. image:: img/Wpilib.jpg

Cette librairie nous fournit des classes pour gérer les moteurs, la
pneumatique, les capteurs et à peu près tout ce dont nous avons besoin pour
contrôler un robot. C'est une librairie de haut-niveau, c'est à dire qu'elle
nous permet de manipuler facilement des objets complexes.

WpiLib est disponible en C++ et en Java. Il existe aussi des versions
non-officiellement supportées en Python ou en Kotlin.


Installation
~~~~~~~~~~~~

Pour programmer le robot en C++ et en Java, WpiLib propose d'utiliser Visual
Studio Code qui est l'IDE officiellement supporté. Pour installer
l'environnement de programmation, un installer Windows est disponible. Les
instructions sont à retrouver
`ici <https://docs.wpilib.org/en/latest/docs/software/getting-started>`_.

VS Code permet l'utilisation des classes de WpiLib mais aussi de déployer le
programme sur le robot.

Pour contrôler le robot, il faut aussi avoir la Driver Station. Voici
`les instructions <https://docs.wpilib.org/en/latest/docs/software/driverstation/frc-update-suite.html>`_
pour l'installer l'installer.



Documentation
-------------

Pour obtenir des informations sur la librairie, savoir à quoi sert quelle
classe, quelles fonctions sont disponibles, ect...,
`une documentation <http://first.wpi.edu/FRC/roborio/release/docs/cpp/>`_
est disponible. Chaque classe y est répertoriée avec la description de chacune
des méthodes qu'elle propose.
