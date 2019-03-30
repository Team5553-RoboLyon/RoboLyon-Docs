# Notre premier programme

## A quoi ressemble un programme de robot ?

Quand on crée un nouveau projet, on a le choix entre plusieurs classes : `SampleRobot`, `IterativeRobot`, `TimedRobot` et `CommandBasedRobot`. Quand nous allons programmer le robot nous allons en effet coder une classe qui représente le robot.

Pour commencer choisissez la classe `TimedRobot Skeleton`. Voici à quoi elle ressemble :

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

Premièrement, on remarque que notre classe s'appelle `Robot` et qu'elle hérite d'une autre classe appelée `TimedRobot`.

Ensuite, on voit que notre classe `Robot` possède plusieurs méthodes : `RobotInit()`, `AutonomousInit()`, `AutonomousPeriodic()`, ect... Ce sont ces méthodes que nous allons coder pour programmer le robot.

Toutes ces déclarations de méthode sont suivies du mot-clé `override`. Petite explication : ces méthodes sont en fait héritées de la classe `TimedRobot`. Cela permet à n'importe quelle programme d'appeler ces méthodes en etant sûr qu'elles existent. Par défaut, ces méthodes sont vides et ne font rien. C'est pourquoi on peut les `override` : ce sera ainsi notre version de la méthode qui sera appelée au lieu de la version vide du `TimedRobot`.


## Les différentes méthodes

Le `TimedRobot` nous propose donc une structure pour coder le robot qui gère les transitions entre les états du robot et les boucles dans les états. Pour chaque état (autonomous, teleop, disabled, test), deux méthodes sont appelées :

- Init : cette méthode est appelée chaque fois que l'on entre dans l'état correspondant (par exemple si l'on passe de Disable à Teleop, la méthode `TeleopInit()` sera appelée une fois)

- Periodic : cette méthode est appelée toutes les 20ms quand on est dans l'état correspondant (par exemple si l'on est en Teleop, la méthode `TeleopPeriodic()` sera appelée un fois toutes les 20ms).

Pour comprendre le fonctionnement du robot, essayez d'afficher un message pour chaque méthode. Les sorties `cout` sont rédirigées vers le réseau et on peut les lire grâce à Riolog ou sur la Driver Station.

Comme ces méthodes sont appelées très fréquement, il ne faut pas y écrire du code trop long à s'executer. Sinon, cela bloque le programme et pose quelques problèmes. Les boucles `while`, `do .. while` et `for` sont donc formellement interdites. On utilisera à la place de celles-ci des `if` qui seront appelés régulièrement.

## Utiliser WpiLib

Pour l'instant, on utilise une seule classe fournie par WpiLib, la classe `TimedRobot`. Elle est incluse grâce à la ligne :
```c++
#include <frc/TimedRobot.h>
```

Mais pour programmer le robot nous allons avoir besoin des autres classes WpiLib pour controller les moteurs, les capteurs, ... Une façon très rapide d'inclure toutes ces classes à la fois est d'écrire cette ligne au début de notre programme :
```c++
#include <frc/WPILib.h>
```
Vous pouvez jeter un coup d'oeil à l'interieur de ce ficher. Il inclu en fait à son tour toutes les classes de WpiLib.
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