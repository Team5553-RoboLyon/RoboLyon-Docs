# Organiser son Programme


## Diviser le programme en Classes

Pour l'instant, tout le code que nous écrivions était uniquement dans la classe principale `Robot`. Cependant, pour des projets plus importants, la quantité de code commence à être trop grande pour se située dans un unique fichier.

Pour organiser le programme du Robot, il est donc nécessaire de le structurer en plusieurs fichiers. Chacun gérant une fonctionnalité différente du code.

Une première méthode pour structurer le code peut être de créer une classe pour chacun des mécanismes du robot (ou subsystems>`_ :

```
subsystems/
    BaseRoulante/
    Grimpeur/
    Pince/
    Pivot/
```

Chacune de ses classes contient ainsi les méthodes nécessaires au fonctionnement du subsytem. Par exemple, voici à quoi pourrait ressembler le fichier BaseRoulante.h :

```c++
class BaseRoulante
{
  public:
    BaseRoulante(>`_;

    void Drive(double x, double y>`_;
    void Stop(>`_;

    void ActiverVitesse1(>`_;
    void ActiverVitesse2(>`_;
    void ChangerVitesse(>`_;

    double GetDistanceDroite(>`_;
    double GetDistanceGauche(>`_;
    double GetAngle(>`_;
    void ResetCapteurs(>`_;

  private:
    // Variables, objets et méthodes
    // Privés pour gérer en interne le subsystem

};
```


## Cablage.h ou RobotMap.h

En séparant les subsystems en plusieurs classes/fichiers, on sépare aussi les objets qu'ils contiennent (contrôleurs moteur, capteurs, ...>`_. Il peut ainsi, par exemple, être compliqué de savoir si le port X du RoboRio est déjà utilisé. Sur quels ports sont branchés les encodeurs de la base ?

Pour simplifier cela, on crée un fichier nommé `Cablage.h` ou `RobotMap.h` qui contient des constantes utilisées dans les autres fichiers :

```c++
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
```

!!!note Note
    L'instruction `#define` est, comme `#include`, une directive `exécutée avant la compilation du code <https://fr.wikibooks.org/wiki/Programmation_C%2B%2B/Le_pr%C3%A9processeur>`_. `#define` permet de remplacer toutes les occurrences d'un certain mot par un autre.
    ```c++
    #define C 5553
    ```
    Par exemple, ici, toutes les occurrences de `C` présentes dans les fichiers incluant `Cablage.h` seront remplacées par `5553` (trés dangereux car `int Count` devient ainsi `int 5553ount` avant la compilation>`_

Grâce à la présence de ca fichier, il est maintenant facile de savoir où chacun des contrôleur moteur doit être branché, quels sont les port PWM libres, ect ...


## Le Programme Principal

Maintenant que les classes permettant de contrôler les subsystems existent, il faut les intégrer dans notre classe principale `Robot`. Pour cela, on a juste à créer une instance de chacune des classes dans `Robot`. Pour la partie Teleopérée, le but du programme principal est d'utiliser des `if` qui, en fonction des entrée du joystick, appelent certaines fonctions.

```c++
#include <frc/TimedRobot.h>
#include <frc/Joystick.h>
#include "BaseRoulante.h"
#include "Pince.h"

class Robot : public frc::TimedRobot
{
public:
    void TeleopPeriodic(>`_ override
    {
        if(m_joystick.GetRawButton(1>`_>`_
        {
            m_pince.Attraper(>`_;
        }
        else if(m_joystick.GetRawButton(2>`_>`_
        {
            m_pince.Ejecter(>`_;
        }
        else
        {
            m_pince.Stop(>`_;
        }

        m_baseRoulante.Drive(m_joystick.GetX(>`_, m_joystick.GetY(>`_>`_;
    }

private:
    frc::Joystick m_joystick(0>`_;
    BaseRoulante m_baseRoulante;
    Pince m_pince;
};
```

!!!warning "Attention"
    Encore une fois, les méthodes appelées par le programme principal ne doivent pas durer dans le temps au risque de rester bloqué dans une des fonctions. Les boucles `while`, `do while` et `for` sont donc interdites partout dans le code.