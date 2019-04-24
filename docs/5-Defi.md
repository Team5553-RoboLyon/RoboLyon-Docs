# 1er Défi : Contrôler un moteur grâce au joystick


Coder la méthode TeleopPeriodic d'un TimedRobot pour répondre à ces objectifs :

- Quand le joystick est proche de zéro (entre -0.2 et 0.2), le moteur ne tourne pas.

- Sinon, le moteur tourne à une vitesse proportionelle à la position du joystick

- Quand la gachette (bouton 1) du joystick est appuyée, le moteur tourne pas


??? note "**Correction**"
    Normalement, votre programme sera séparé en 2 fichiers différents : Robot.h et Robot.cpp. Ici, le programme est dans un seul fichier pour plus de simplicité :

    ```c++
    #include <frc/TimedRobot.h>
    #include <frc/VictorSP.h>
    #include <frc/Joystick.h>

    class Robot : public frc::TimedRobot
    {
    public:
        void TeleopPeriodic() override
        {
            if(Joystick.GetButton(1))
            {
                moteur.Set(0);
            }
            else
            {
                double y = Joystick.GetY();

                if(y < 0.2 && y > -0.2)
                {
                    y = 0;
                }

                moteur.Set(y);
            }
        }

    private:
        frc::Joystick m_joystick(0);
        frc::VictorSP m_moteur(0);
    };
    ```
