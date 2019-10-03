Organiser son Programme
=======================


Diviser le programme en Classes
-------------------------------

Pour l'instant, tout le code que nous écrivions était uniquement dans la
classe principale ``Robot``. Cependant, pour des projets plus importants,
la quantité de code commence à être trop grande pour se située dans un
unique fichier.

Pour organiser le programme du Robot, il est donc nécessaire de le structurer
en plusieurs fichiers. Chacun gérant une fonctionnalité différente du code.

Une première méthode pour structurer le code peut être de créer une classe
pour chacun des mécanismes du robot (ou subsystems) :

.. code-block:: text

  subsystems/
  ├── BaseRoulante/
  ├── Grimpeur/
  ├── Pince/
  └── Pivot/

Chacune de ses classes contient ainsi les méthodes nécessaires au
fonctionnement du subsytem. Par exemple, voici à quoi pourrait ressembler
le fichier BaseRoulante.h :

.. code-block:: c++

  class BaseRoulante
  {
  public:
    BaseRoulante();

    void Drive(double x, double y);
    void Stop();

    void ActiverVitesse1();
    void ActiverVitesse2();
    void ChangerVitesse();

    double GetDistanceDroite();
    double GetDistanceGauche();
    double GetAngle();
    void ResetCapteurs();

  private:
    // Variables, objets et méthodes
    // Privés pour gérer en interne le subsystem

  };


Cablage.h ou Constants.h
------------------------

En séparant les subsystems en plusieurs classes/fichiers, on sépare aussi les
objets qu'ils contiennent (contrôleurs moteur, capteurs, ...). Il peut ainsi,
par exemple, être compliqué de savoir si le port X du RoboRio est déjà utilisé.
Sur quels ports sont branchés les encodeurs de la base ?

Pour simplifier cela, on crée un fichier nommé ``Cablage.h`` ou
``Constants.h`` qui contient des constantes utilisées dans les autres fichiers :

.. code-block:: c++

  // PWM MOTORS
  #define PWM_ROUES_PINCE 0
  #define PWM_BASE_DROITE_1 1
  #define PWM_BASE_DROITE_2 2
  #define PWM_BASE_GAUCHE_1 3
  #define PWM_BASE_GAUCHE_2 4

  // CAN MOTORS
  #define CAN_PIVOT 1

  // DIO ENCODEURS
  #define DIO_ENCODEUR_PIVOT_A 0
  #define DIO_ENCODEUR_PIVOT_B 1

  // PCM PNEUMATICS
  #define PCM_PINCE_A 0
  #define PCM_PINCE_B 1

.. note::
  L'instruction ``#define`` est, comme ``#include``, une directive
  `exécutée avant la compilation du code <https://fr.wikibooks.org/wiki/Programmation_C%2B%2B/Le_pr%C3%A9processeur>`__.
  ``#define`` permet de remplacer toutes les occurrences d'un certain mot
  par un autre.

  .. code-block:: c++

    #define C 5553

  Par exemple, ici, toutes les occurrences de ``C`` présentes dans les
  fichiers incluant ``Cablage.h`` seront remplacées par ``5553``
  (très dangereux car ``int Count`` devient ainsi ``int 5553ount`` avant
  la compilation)

Grâce à la présence de ca fichier, il est maintenant facile de savoir où
chacun des contrôleur moteur doit être branché, quels sont les port PWM
libres, ect ...


Le Programme Principal
----------------------

Maintenant que les classes permettant de contrôler les subsystems existent,
il faut les intégrer dans notre classe principale ``Robot``. Pour cela, on
a juste à créer une instance de chacune des classes dans ``Robot``. Pour la
partie Téléopérée, le but du programme principal est d'utiliser des ``if``
qui, en fonction des entrée du joystick, appellent certaines fonctions.

.. code-block:: c++

  #include <frc/TimedRobot.h>
  #include <frc/Joystick.h>
  #include "BaseRoulante.h"
  #include "Pince.h"

  class Robot : public frc::TimedRobot
  {
  public:
    void TeleopPeriodic() override
    {
      if(m_joystick.GetRawButton(1))
      {
        m_pince.Attraper();
      }
      else if(m_joystick.GetRawButton(2))
      {
        m_pince.Ejecter();
      }
      else
      {
        m_pince.Stop();
      }

      m_baseRoulante.Drive(m_joystick.GetX(), m_joystick.GetY());
    }

  private:
    frc::Joystick m_joystick(0);
    BaseRoulante m_baseRoulante;
    Pince m_pince;
  };

.. attention::
  Encore une fois, les méthodes appelées par le programme
  principal ne doivent pas durer dans le temps au risque de rester bloqué dans
  une des fonctions. Les boucles ``while``, ``do while`` et ``for`` sont donc
  interdites partout dans le code.
