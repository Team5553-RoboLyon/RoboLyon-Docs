# Le contrôleur PID

## Contrôler un mécanisme

Maintenant que nous savons contrôler les moteurs et lire les informations des capteurs, il faut les utiliser ensemble pour contrôler intelligement et efficacement vos mécanismes. Voici quelques termes qui seront utiles pour la suite :

### Setpoint
Le setpoint est l'objectif que le mécanisme doit atteindre. Pour un élévateur, le setpoint est la hauteur désirée. Pour un pivot, c'est un angle. On peut aussi imaginer un shooter dont le setpoint serait la vitesse de rotation adéquate pour lancer l'objet à la bonne distance.

### Erreur
L'erreur est la différence entre le setpoint et l'état (position, vitesse) du mécanisme à un instant donné. Pour un élévateur, c'est la distance (positive ou négative) qu'il reste à parcourir pour atteindre le setpoint.

### Output
L'output est la correction exercé sur le mécanisme pour le rapprocher du setpoint. Il peut être exprimé sur une échelle de -1 à 1 représentant la puissance donnée au moteur ou bien en volts.


## PID, ça veut dire quoi ?

Le PID est une méthode pour contrôler les mécanismes efficacement. C'est la boucle de contrôle la plus utilisée dans l'industrie car elle peut s'appliquer à de nombreuses situations (thermostat, regulateur de position, de vitesse). C'est un acronyme signifiant : **Proportonnel**, **Intégral**, **Dérivé**, les 3 termes qui composent le PID.

L'équation d'un contrôleur PID est la somme de ces 3 termes :
\begin{align}
output = P \times erreur + I \times \sum erreur + D \times \frac{\Delta erreur}{\Delta t}
\end{align}

### Proportionel
\(P \times erreur\)

![](https://upload.wikimedia.org/wikipedia/commons/a/a3/PID_varyingP.jpg){ width=400px }

Le terme proportionel est égal au produit d'un coefficient constant (**kP** ou **P gain**) et de l'erreur. Ce terme est ainsi élevé quand l'erreur est élevé (au début) et diminue lorsque le mécanisme se rapproche du setpoint. Plus le coefficient est élevé, plus la réponse du système sera rapide mais plus le mécanisme risquera d'osciller.

### Intégral
\(I \times \sum erreur\)

![](https://upload.wikimedia.org/wikipedia/commons/c/c0/Change_with_Ki.png){ width=400px }

En utilisant seulement le terme proportionel, le mécanisme peut osciller (kP trop élevé) ou bien rester en dessous du setpoint (kP trop faible). Pour cela, on peut utiliser le terme [intégral](https://couleur-science.eu/?d=211a43--les-integrales-en-math). Celui-ci est égal à la somme de toutes les erreurs depuis le début. Ce terme va ainsi augmenter de plus en plus si le mécanisme reste en dessous du setpoint trop longtemps.

### Dérivé
\(D \times \frac{\Delta erreur}{\Delta t}\)

![](https://upload.wikimedia.org/wikipedia/commons/c/c7/Change_with_Kd.png){ width=400px }

Le terme [dérivé](https://couleur-science.eu/?d=94f1c0--les-fonctions-derivees-en-math) est égal à la variation de l'erreur sur la variation du temps. C'est la "pente" de l'erreur.  Dans le code du robot, le delta temps sera toujours le même entre 2 itérations. On peut donc résumer le terme dérivé en la variation de l'erreur entre 2 itérations soit la différence entre l'erreur actuelle et l'erreur précedente.

\(D \times (erreur - erreurPrecedente)\)

Le coeefficient kD est souvent négatif afin de réguler "l'accélération" du mécanisme. Si l'accélération est trop élevée, le terme dérivé sera alors d'autant plus important et ralentira le mécanisme.

### Feed-Forward

Au PID on peut ajouter un 4ème terme, le terme F pour feed forward. Il peut être calculé en connaissant les caractéristiques du mécanisme :

**Elévateur** : Pour contrer la gravité exercée sur un élévateur, le voltage nécéssaire peut être calculé en fonction de la masse de l'élévateur, du torque du moteur et du ratio de la gearbox.

**Pivot** : Pour contrer la gravité exercée sur le bras du pivot, le terme F peut être calculé en fonction de l'angle \(\theta\) du bras : \(k \cos \theta\)

Il existe d'autres cas comme les bases roulantes où le terme F peut être utile pour contrer les forces de frottement ou d'accélération.


### Coder un PID

Maintenant que nous avons apris la théorie du PID, utilisons le pour déplacer notre élévateur de façon autonome. Pour l'exemple, un dira que l'unique moteur de l'élévateur sera contrôlé par un `VictorSP` et que la position de l'élévateur nous sera donnée par un `Encoder`. A vous de jouer.

??? note "Correction"
    Normalement, votre programme sera séparé en 2 fichiers différents : Robot.h et Robot.cpp. Ici, le programme est dans un seul fichier pour plus de simplicité :

    ```c++
    #include <frc/TimedRobot.h>
    #include <frc/VictorSP.h>
    #include <frc/Encoder.h>

    class Robot : public frc::TimedRobot
    {
    public:
        void RobotInit() override
        {
            // Le sens de rotation du moteur
            m_moteur.SetInverted(false);

            // Le sens dans lequel compte l'encodeur
            m_encodeur.SetReverseDirection(false);

            // Convertion ticks -> mètres
            m_encodeur.SetDistancePerPulse(m_distanceParTick);

            m_setpoint = 0.0;
            m_erreur = 0.0;
            m_erreurPrecedente = 0.0;
            m_sommeErreurs = 0.0;
            m_derivee = 0.0;
        }

        void RobotPeriodic () override
        {
            position = m_encodeur.GetDistance();

            m_erreur = m_setpoint - position;
            m_sommeErreurs += m_erreur;
            m_derivee = m_erreur - m_erreurPrecedente;

            double output = m_P * m_erreur + m_I * m_sommeErreurs + m_D * derivee + m_F;

            m_moteur.Set(output);

            m_erreurPrecedente = m_erreur;
        }

        void TeleopPeriodic() override
        {
            // En fonction des actions du pilote :
            // Utiliser la fonction SetSetpoint pour déplacer l'élévateur
        }

        void SetSetpoint(double setpoint)
        {
            if(setpoint < m_minSetpoint)
            {
                m_setpoint = m_minSetpoint;
            }
            else if(setpoint > m_maxSetpoint)
            {
                m_setpoint = m_maxSetpoint:
            }
            else
            {
                m_setpoint = setpoint;
            }
        }

    private:
        // Moteurs et Capteurs
        frc::VictorSP m_moteur(0);
        frc::Encoder m_encodeur(0, 1);

        // Facteur de convertion des ticks vers une distance en mètre
        const double m_distanceParTick = 0.05;

        // Variables du PID
        double m_setpoint;
        double m_erreur;
        double m_erreurPrecedente;
        double m_sommeErreurs;
        double m_derivee;

        // Valeurs déterminées scientifiquement
        const double m_P = 0.8;
        const double m_I = 0.01;
        const double m_D = - 0.2;
        const double m_F = 0.15;

        // L'élévateur peut aller de 0 m jusqu'à 1.5 m de hauteur
        const double m_minSetpoint = 0.0;
        const double m_maxSetpoint = 1.5;
    };
    ```