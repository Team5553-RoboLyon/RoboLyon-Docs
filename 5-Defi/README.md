# 1er Défi : Contrôler un moteur grâce au joystick

## Le défi
Coder la méthode TeleopPeriodic d'un TimedRobot pour répondre à ces objectifs :

- Quand le joystick est proche de zéro (entre -0.2 et 0.2), le moteur ne tourne pas.

- Sinon, le moteur tourne à une vitesse proportionelle à la position du joystick

- Quand la gachette (bouton 1) du joystick est appuyée, le moteur tourne pas

<details>
  <summary>Correction</summary>
  
  ### Robot.h
  ```c++
  private:
    frc::Joystick m_joystick{0};
    frc::VictorSP m_moteur{0};
  ```

  ### Robot.cpp
  ```c++
  void Robot::TeleopPeriodic()
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
        }d

        moteur.Set(y);
    }
  }
  ```

</details>