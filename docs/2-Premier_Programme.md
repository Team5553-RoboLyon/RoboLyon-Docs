# Notre premier programme

## A quoi ressemble un programme de robot ?

Quand on crée un nouveau projet, on a plusieurs choix : `SampleRobot`, `IterativeRobot`, `TimedRobot` et `CommandBasedRobot`. Pour commencer choisissez `TimedRobot Skeleton`. Voici à quoi cela ressemble :

```c++
#include <frc/TimedRobot.h>

class Robot : public frc::TimedRobot
{
 public:
  void RobotInit() override;

  void AutonomousInit() override;
  void AutonomousPeriodic() override;

  void TeleopInit() override;
  void TeleopPeriodic() override;

  void TestInit() override;
  void TestPeriodic() override;
};
```

**Mais où est le main() ?**


En effet, le programme du robot est différent de ce que l'on a l'habitude de voir. Au lieu d'avoir un `int main()` et beaucoup de fonctions, le programme est une classe. Bien sûr, il existe quelque par un main; mais il ne change jamais et il est déjà écrit pour nous.

Le programme du robot va donc lire la classe que nous allons coder.


**Lisons le programme ligne par ligne**

`#include <frc/TimedRobot.h>` : Cette ligne inclut le fichier "frc/TimedRobot.h"

`class Robot : public frc::TimedRobot` : Notre classe s'appelle `Robot` et qu'elle hérite d'une autre classe appelée `TimedRobot`. TimedRobot a été inclus précédement, cette classe fait partie de la librairie WpiLib.

Ensuite, on voit que notre classe `Robot` possède plusieurs méthodes : `RobotInit()`, `AutonomousInit()`, `AutonomousPeriodic()`, ect... Ce sont ces méthodes que nous allons coder pour programmer le robot.

Toutes ces déclarations de méthode sont suivies du mot-clé `override`. Petite explication : ces méthodes sont en fait héritées de la classe mère `TimedRobot`. Cela permet à n'importe quelle programme d'appeler ces méthodes en etant sûr qu'elles existent. Par défaut, ces méthodes sont vides et ne font rien. C'est pourquoi on peut les `override` : ce sera ainsi notre version de la méthode qui sera appelée au lieu de la version vide du `TimedRobot`.


## Les différentes méthodes

Le `TimedRobot` nous propose donc une structure pour coder le robot qui gère les transitions entre les états du robot et les boucles dans ces états. Pour chaque état (autonomous, teleop, disabled, test), deux méthodes sont appelées :

- Init : cette méthode est appelée chaque fois que l'on entre dans l'état correspondant (par exemple si l'on passe de Disable à Teleop, la méthode `TeleopInit()` sera appelée une fois)

- Periodic : cette méthode est appelée toutes les 20ms quand on est dans l'état correspondant (par exemple si l'on est en Teleop, la méthode `TeleopPeriodic()` sera appelée un fois toutes les 20ms).

Pour comprendre le fonctionnement du robot, essayez d'afficher un message pour chaque méthode. Les sorties `cout` sont rédirigées vers le réseau et on peut les lire grâce à Riolog ou sur la Driver Station.

!!! warning "Attention"
    Comme ces méthodes sont appelées très fréquement, il ne faut pas y écrire du code trop long à s'executer. Sinon, cela bloque le programme et pose des problèmes. Les boucles `while`, `do .. while` et `for` sont donc formellement interdites. On utilisera à la place de celles-ci des `if` qui seront appelés régulièrement.

## Utiliser WpiLib

Pour l'instant, on utilise une seule classe fournie par WpiLib, la classe `TimedRobot`. Elle est incluse grâce à la ligne :
```c++
#include <frc/TimedRobot.h>
```

Mais pour programmer le robot nous allons avoir besoin des autres classes WpiLib pour controller les moteurs, les capteurs, ... Une façon très rapide d'inclure toutes ces classes à la fois est d'écrire cette ligne au début de notre programme :
```c++
#include <frc/WPILib.h>
```
Vous pouvez jeter un coup d'oeil à l'interieur de ce ficher. Il inclut en fait à son tour toutes les classes de WpiLib (dont TimedRobot).
```c++
#include "frc/ADXL345_I2C.h"
#include "frc/ADXL345_SPI.h"
#include "frc/ADXL362.h"
#include "frc/ADXRS450_Gyro.h"
#include "frc/AnalogAccelerometer.h"
#include "frc/AnalogGyro.h"
#include "frc/AnalogInput.h"
#include "frc/AnalogOutput.h"
#include "frc/AnalogPotentiometer.h"
#include "frc/AnalogTrigger.h"
#include "frc/AnalogTriggerOutput.h"
#include "frc/BuiltInAccelerometer.h"
#include "frc/Compressor.h"
#include "frc/ControllerPower.h"
#include "frc/Counter.h"
#include "frc/DMC60.h"
#include "frc/DigitalInput.h"
#include "frc/DigitalOutput.h"
........
......
```
