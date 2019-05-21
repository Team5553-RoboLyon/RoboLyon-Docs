Lire les entrées d'un joystick
==============================

Le Joystick
-----------

Pour permettre au pilote de contrôler le robot, celui-ci utilise un joystick. Nous pouvons aussi utiliser une manette de Xbox.

Le joystick que nous utilisons est le Logitech 3D Pro. Il possède 3 axes (avant/arrière, gauche/droite et twist), 12 boutons, un POV (bouton avec 9 positions possibles) et un throttle (manette type avion).

.. image:: img/Joystick.jpg

On peut connaître l'état de chaque bouton/axe du joystick en temps réel sur la Driver Station :

.. image:: img/Ds.jpg


Dans le Code
------------

WpiLib fournit une classe `Joystick <http://first.wpi.edu/FRC/roborio/release/docs/cpp/classfrc_1_1Joystick.html>`_ qui nous permet de récupérer les infos de celui-ci. Son constructeur attend en argument le numéro du joystick. Les numéros sont attribués selon l'ordre de branchement : le 1er joystick sera le 0, le 2ème le 1, ect ... Si on a un seul joystick, il aura donc pour numéro 0 :

.. code-block:: c++

    #include <frc/Joystick.h>

    frc::Joystick mon_joystick(0);


Pour récupérer l'état d'un bouton (appuyé/relâché, on peut utiliser la méthode `bool GetRawButton(int button)` qui attend en argument le numéro du bouton et qui renvoi `true` si le bouton est appuyé et `false` si il est relâché.

.. code-block:: c++

    // Récupère l'état de la gâchette (trigger)
    bool gachetteAppuyee = mon_joystick.GetRawButton(1);


Pour récupérer la position d'un axe entre -1 et 1, on peut utiliser les méthodes `double GetX()`, `double GetY()`, `double GetZ()` (ou `GetTwist()`) et `double GetThrottle()`.

.. code-block:: c++

    // Récupère l'état de chaque axe
    double x = mon_joystick.GetX();
    double y = mon_joystick.GetY();
    double twist = mon_joystick.GetTwist();
    double throttle = mon_joystick.GetThrottle();


On a aussi la méthode `int GetPOV()` qui renvoi l'angle formé par le POV ou -1 si il est situé au centre.

.. code-block:: c++

    int pov = mon_joystick.GetPOV();

    switch (pov)
    {
        case -1:
            // Centre
            break;

        case 0:
            // Haut
            break;

        case 180:
            // Bas
            break;

        case 90
            // Droite
            break;

        case 270
            // Gauche
            break;
    }
