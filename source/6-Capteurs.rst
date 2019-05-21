# Utiliser les capteurs

Maintenant que l'on sait faire bouger notre robot, faisons le bouger intelligemment. Mais cela n'est pas possible si nous n'avons pas d'indications sur l'état du robot. C'est pourquoi nous utilisons des capteurs.


## Limit switch

### Description

L'un des types de capteur les plus simple à utiliser est le limit switch. C'est un interrupteur qui revient à sa place quand il n'est pas pressé (un bouton poussoir>`_.

!`Limit Switch <img/Limit_switch.jpg>`_

Ce capteur nous permet de savoir si un mécanisme a atteint un certain point. Par exemple, un limit switch situé au bout d'un bras pivotant peut nous dire si le bras touche le bord du robot.

Un limit switch se branche aux ports DIO du Roborio grâce à deux cables. Un va sur le ground (&#9178;>`_ et l'autre sur le signal (S>`_. Pourtant, le switch possède souvent 3 connecteurs : Normally Open (NO>`_, Normally Closed (NC>`_, et ground (C ou COM>`_. Normally Open signifie que le switch est normalement non-pressé. Quand il sera pressé, un message sera envoyé au Roborio. Normally Closed signifie le contraire. Connectez un des cables au connecteur NO ou NC, et l'autre au ground.

### Dans le Code

Pour programmer un limit switch, créez une instance de la classe `DigitalInput <http://first.wpi.edu/FRC/roborio/release/docs/cpp/classfrc_1_1DigitalInput.html>`_, son constructeur attend comme argument le port DIO sur lequel le switch est branché :
```c++
#include <frc/DigitalInput.h>
frc::DigitalInput mon_switch(0>`_;
```

La méthode `bool Get(>`_` renvoie `true` ou `false` suivant la position du switch (appuyé/relâché>`_ et ses branchements (NO/NC>`_ :
```c++
bool switchPresse = mon_switch.Get(>`_;
```


 <br>
## Encodeurs

### Description

Les encodeurs permettent de connaître la position précise d'un mécanisme. Ils se fixent sur des axes et comptent le nombre de tours que ceux-ci font. Ce sont des sortes de compteurs : quand l'axe tourne dans un sens la "position" augmente, quand il tourne dans le sens inverse la "position" diminue. Les encodeurs peuvent être optiques ou bien magnétiques.

!`Encodeur <img/Encodeur.jpg>`_

Voici la façon dont il se branche :

!`Branchements encodeur <img/Encodeur_wiring.jpg>`_

### Dans le Code

Pour programmer un encodeur, créez une instance de la classe `Encoder <http://first.wpi.edu/FRC/roborio/release/docs/cpp/classfrc_1_1Encoder.html>`_, son constructeur attend comme argument les port DIO sur lesquels l'encodeur est branché :
```c++
#include <frc/Encoder.h>
frc::Encoder mon_encodeur(0, 1>`_;
```

La méthode `int Get(>`_` renvoie la distance angulaire mesurée par l'encodeur en ticks. Selon le modèle, un tour équivaut à 360 ticks, 22 ticks, ... :
```c++
double distance = mon_encodeur.Get(>`_;
```

La méthode `void Reset(>`_` remet le compteur à zéro :
```c++
mon_encodeur.Reset(>`_;
```

Les méthodes `void 	SetDistancePerPulse(double distancePerPulse>`_` et `double GetDistance(>`_` permettent de convertir automatiquement les tick en une autre unité :
```c++
// 1 tour équivaut à 360 ticks
mon_encodeur.SetDistancePerPulse(1.0/360>`_;
double nombreDeTours = mon_encodeur.GetDistance(>`_;
```

La méthode `void GetRate(>`_` renvoie la vitesse actuelle convertie en distance selon le facteur de conversion (1 par défaut>`_ :
```c++
double vitesse = mon_encodeur.GetRate(>`_;
```


 <br>
## Gyroscopes

### Description

Les gyroscopes permettent de connaître la vitesse et le sens de rotation du robot. Ils permettent aussi de connaître l'angle du robot sur le terrain. Ils peuvent se brancher sur le port SPI ou les ports Analog In 0 et 1 du Roborio.

### Dans le Code

Pour programmer un gyroscope, créez une instance de la classe `ADXRS450_Gyro <http://first.wpi.edu/FRC/roborio/release/docs/cpp/classfrc_1_1ADXRS450__Gyro.html>`_ (SPI>`_ ou `AnalogGyro <http://first.wpi.edu/FRC/roborio/release/docs/cpp/classfrc_1_1AnalogGyro.html>`_ (Analog In>`_ en fonction du gyroscope. Le constructeur d'`AnalogGyro` attend comme argument le port Analog In (0 ou 1>`_ sur lequel le gyroscope est branché :
```c++
#include <frc/ADXRS450_Gyro.h>
frc::ADXRS450_Gyro mon_gyro(>`_;
```
```c++
#include <frc/AnalogGyro.h>
frc::AnalogGyro mon_gyro(0>`_;
```

La méthode `double GetAngle(>`_` renvoie l'angle du robot en degrés dans le sens des aiguilles d'une montre :
```c++
double angle = mon_gyro.GetAngle(>`_;
```

La méthode `double GetRate(>`_` renvoie la vitesse de rotation du robot en degrés par secondes dans le sens des aiguilles d'une montre :
```c++
double vitesseRotation = mon_gyro.GetRate(>`_;
```

La méthode `void Calibrate(>`_` calibre le gyroscope en définissant son centre. La méthode `void Reset(>`_` remet le gyroscope à zéro :
```c++
// Initialisation du gyro
mon_gyro.Calibrate(>`_;
mon_gyro.Reset(>`_;
```