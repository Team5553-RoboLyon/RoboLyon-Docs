# Contrôler un moteur

## Les contrôleurs moteur

Pour contrôler les moteurs présents sur le robot, nous avons besoin de contrôleurs moteurs. En un mot, ceux-ci recoivent un signal de faible intensité de la part du RoboRio et envoient au moteur un signal de plus forte intensité. Voici quelques exemples de contrôleurs moteur que nous utilisons : VictorSP, Spark et SparkMax.

![Quelques Contrôleurs Moteur](Controllers.jpg)

## PWM et CAN

Il y a 2 manières de contrôler ceux-ci : en PWM ou en CAN. Tous les contrôleurs supportent le PWM mais seulement une partie d'entre eux supporte le CAN.

La principale différence entre les deux modes de transmission est que le PWM ne peut commander que la vitesse du moteur tandis que le CAN permet des contrôles plus avancés et la communication d'informations entre le contrôleur et le RoboRio.

![PWM vs CAN](CanPwm.jpg)

La méthode la plus simple est le PWM. Elle nous permet de contrôler rapidement le moteur voulu en branchant le cable du contrôleur sur le bon port PWM du RoboRio (encadrés en rouge).

![Ports PWM du RoboRio](Roborio.jpg)

## Dans le Code

Du côté du RoboRio, il nous suffit de créer un objet correspondant au contrôleur pour pouvoir asservir le moteur. WpiLib propose un objet pour chaque contrôleur. En fait, avec le PWM, ces objets sont tous identiques mais ils ont été dérivés sous plusieurs noms :
```c++
#include <frc/VictorSP.h>
#include <frc/Spark.h>
#include <frc/PWMVictorSPX.h>
```
Remarquez que `VictorSPX` est précéde de `PWM`, c'est parce qu'il peut être controllé via le CAN. Pour différencier les 2 objets (qui sont totalement différents), celui-ci se nomme donc `PWMVictorSPX`.

Quand on crée un objet qui représente un contrôleur PWM, on doit spécifier dans le constructeur le port sur lequel il est branché. Par exemple, pour un VictorSP branché sur le port n°0 :
```c++
frc::VictorSP mon_moteur{0};
```

On a ensuite accès à tout un tas de méthodes qui nous permettent de contrôler le moteur.

Pour "donner" une puissance voulue à un moteur, on utilise la méthode : `void Set(double value)` qui attend en argument un double entre -1 (pleine puissance vers l'arrière) et 1 (pleine puissance vers l'avant). Si je veux faire tourner mon moteur à la moitié de sa vitesse maximum :
```c++
mon_moteur.Set(0.5);
```

On a aussi `void StopMotor()` qui remplace `Set(0)` et `void SetInverted(bool isInverted)` qui inverse la direction du moteur si on lui donne comme argument `true`.
```c++
mon_moteur.SetInverted(true);

mon_moteur.StopMotor();
```
