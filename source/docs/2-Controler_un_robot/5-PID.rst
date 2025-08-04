Le Contrôleur PID
=================

Introduction
------------

Quand on veut que le robot atteigne une position précise, suive une trajectoire, ou maintienne une vitesse constante, 
il ne suffit pas de lui dire "avance" ou "tourne". Il faut contrôler *comment* il s'en approche, et avec quelle précision. 
C’est là qu’intervient le **PID**, un algorithme de régulation très utilisé en robotique, industrie, aéronautique… et bien sûr, 
en robotique.

Qu’est-ce qu’un PID ?
---------------------

Le terme **PID** est l’acronyme de **Proportionnel – Intégral – Dérivé**. C’est un type de contrôleur (régulateur), 
c’est-à-dire un système qui ajuste une commande (ex. vitesse moteur) 
pour atteindre une consigne (ex. position cible). Il compare en permanence la position actuelle du système avec la cible, 
et corrige l’écart intelligemment.

Ce type de régulateur a été formalisé dans les années 1920 par Nicolas Minorsky, 
à l’origine pour contrôler des systèmes industriels (vannes, moteurs, etc.) et la navigation maritime. 
Sa simplicité, sa robustesse et son efficacité en font encore aujourd’hui une solution de référence dans de très nombreux domaines
techniques et industriels.

À quoi sert un PID ?
--------------------

Un PID sert à **réduire une erreur**, c’est-à-dire l’écart entre une mesure (position réelle, vitesse, etc.) et une consigne 
(autrement dit un objectif tel qu'une position désirée, une vitesse cible, etc.) comme ceci :

.. math::

   \text{erreur} = \text{consigne} - \text{mesure}

Le PID agit sur une commande (par exemple, la puissance envoyée à un moteur) pour diminuer cet écart de façon 
stable, performante et réactive.

**Exemple courant :**

Tu veux que ton élévateur atteigne la position 1.2 m. Sans PID, tu pourrais simplement "mettre de la puissance", mais :

- S’il y a trop de puissance, il dépasse la cible.
- Pas assez ? Il n’y arrive jamais.
- Un changement de poids ou de friction ? Il se dérègle.

Un PID ajuste la commande à chaque instant pour que l'élévateur atteigne exactement 1.2 m, sans oscillations ni erreur finale.

Le terme Proportionnel (P)
--------------------------

Introduction
^^^^^^^^^^^^

Le **terme proportionnel** est la partie la plus simple du PID. Il applique une correction proportionnelle à l’erreur actuelle : 
plus l’erreur est grande, plus la correction est forte et rapide. C’est comme si tu disais "plus je suis loin de la cible, plus j’accélère".
Et si tu es proche, la correction est faible.

Formule
^^^^^^^

.. math::

   \text{Commande}_P = k_P \times \text{erreur}

- ``kP`` est un **coefficient fixe** que tu choisis.
- L’**erreur** est la différence entre la consigne et la mesure.

Exemple
^^^^^^^

Si ton élévateur est à 0.8 m, et que tu veux aller à 1.2 m :

- erreur = 1.2 - 0.8 = 0.4 m  
- si kP = 5.0, la correction sera : 5.0 × 0.4 = 2.0 (par exemple, 2.0 V envoyés au moteur)

Code minimaliste
^^^^^^^^^^^^^^^^^^^^
.. tab-set::

   .. tab-item:: C++

      .. code-block:: cpp

        double setpoint = 1.2;     // Position cible en mètres
        double current = getPosition(); // Position mesurée
        double error = setpoint - current; 

        double kP = 5.0;
        double output = kP * error;

        setMotorVoltage(output);

   .. tab-item:: Java

      .. code-block:: java

         double setpoint = 1.2;     // Position cible en mètres
         double current = getPosition(); // Position mesurée
         double error = setpoint - current;

        double kP = 5.0;
        double output = kP * error;

        setMotorVoltage(output);

.. note::

   Ce code ne gère que le P. Il est simple, mais peut suffire pour des systèmes lents ou très stables.

Le terme Intégral (I)
---------------------

Introduction
^^^^^^^^^^^^

Le **terme intégral** prend en compte **l’historique de l’erreur**. Contrairement au terme proportionnel (P) qui réagit à l’instantané, 
le terme I observe le **cumul des erreurs dans le temps**.  

Autrement dit, il "se souvient" si ton robot est resté longtemps en retard ou en avance par rapport à la consigne. 
Cela permet de **corriger les erreurs persistantes**, qu’on appelle aussi *erreurs statiques*. 

Pour cela on utilise une **intégrale** de l’erreur sur le temps. C'est un `concept mathématique <https://youtu.be/i7kCaE7Yvfc?si=0DU_aCes_m0ukEEF>`__ qui signifie tout simplement 
**additionner** l’erreur à chaque appel à la valeur précédente.

Formule
^^^^^^^

.. math::

   \text{Commande}_I = k_I \cdot \int_0^t \text{erreur}(t) \, dt

- Le symbole :math:`\int` représente une **intégrale**, c’est-à-dire une **somme continue**.
- En robotique, on **approxime** cela en additionnant l’erreur à chaque cycle :

.. math::

   \text{intégrale} \approx \sum \text{erreur} \times \Delta t

Exemple
^^^^^^^

Imagine que ton élévateur s’arrête à 1.15 m au lieu de 1.20 m. Le terme proportionnel devient petit (car l’erreur est petite), donc la correction s’arrête trop tôt.  
Le terme intégral va **accumuler** cette erreur de 5 cm à chaque cycle, et ajouter petit à petit une correction supplémentaire. Cela permet au bras de rattraper lentement ce dernier écart.

Code minimaliste
^^^^^^^^^^^^^^^^^^^^
.. tab-set::

    .. tab-item:: C++

      .. code-block:: cpp

        double error = setpoint - current;
        errorSum += error * dt; // dt = période du loop, typiquement 0.02s en FTC/FRC

        double kI = 0.05;
        double output_I = kI * errorSum;

   .. tab-item:: Java

      .. code-block:: java

        double error = setpoint - current;
        errorSum += error * dt; // dt = période du loop, typiquement 0.02s en FTC/FRC

        double kI = 0.05;
        double output_I = kI * errorSum;

.. warning::

   Le I peut devenir **trop fort** si l’erreur s'accumule trop longtemps (ex : robot bloqué). 
   C’est ce qu’on appelle un *effet intégral excessif*. Pour cela il faut éviter que le ``kI`` ne soit trop grand.
   Aussi il est préférable de mettre un **limiteur** sur l’erreur cumulée ou d'utiliser un **anti-windup**.


Le terme Dérivé (D)
-------------------

Introduction
^^^^^^^^^^^^

Le **terme dérivé** mesure la **vitesse de variation de l’erreur**. Autrement dit, il regarde **à quelle vitesse l’erreur change**, 
pour anticiper ce qui va se passer.

On dit qu’il fait un **effet prédictif** : si l’erreur diminue rapidement, le D freine la commande pour **éviter un dépassement**. 
C’est un peu comme des amortisseurs sur une voiture : ils absorbent les variations brusques pour stabiliser le mouvement.

Formule
^^^^^^^

.. math::

   \text{Commande}_D = k_D \cdot \frac{d(\text{erreur})}{dt}

- Le symbole :math:`\frac{d}{dt}` est une `dérivée <https://youtu.be/RLEE-iSBimc?si=v7_-RtmowAl8uN2h>`__, c’est-à-dire une mesure du **taux de changement**.
- En pratique, on approxime la dérivée par :

.. math::

   \frac{\Delta \text{erreur}}{\Delta t} = \frac{\text{erreur}_{\text{actuelle}} - \text{erreur}_{\text{précédente}}}{dt}

Exemple
^^^^^^^

Si ton élévateur va vite vers la cible, le D verra que l’erreur diminue très vite. Il va donc **freiner la commande**
pour éviter qu’il ne dépasse la position voulue.

Inversement, si ton élévateur est presque immobile, le D ne fait rien (car le taux de variation est faible).

Code minimaliste
^^^^^^^^^^^^^^^^^^^^
.. tab-set::

    .. tab-item:: C++

    .. code-block:: cpp

        double dError = (error - lastError) / dt;
        lastError = error;

        double kD = 0.4;
        double output_D = kD * dError;

    .. tab-item:: Java

      .. code-block:: java

        double dError = (error - lastError) / dt;
        lastError = error;

        double kD = 0.4;
        double output_D = kD * dError;


.. tip::

   Le D est très utile pour éviter les oscillations (ex : bras qui rebondit autour de la cible).  
   Mais il est **sensible au bruit** des capteurs. Si la position mesurée varie brutalement (bruit), la dérivée devient instable.  
   On peut parfois lisser la mesure ou filtrer le D pour améliorer la stabilité.

Combiner les trois termes
--------------------------

Formule complète
^^^^^^^^^^^^^^^^

.. math::

   \text{Commande} = k_P \cdot e + k_I \cdot \int e \, dt + k_D \cdot \frac{de}{dt}

Chaque terme a un rôle distinct :

.. list-table::
   :header-rows: 1

   * - Terme
     - Rôle principal
   * - P
     - Corriger l’erreur actuelle
   * - I
     - Corriger les erreurs passées
   * - D
     - Anticiper les erreurs futures

Code C++ complet minimal
^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: cpp

   double error = setpoint - current;
   errorSum += error * dt;
   double dError = (error - lastError) / dt;
   lastError = error;

   double output = kP * error + kI * errorSum + kD * dError;

   setMotorVoltage(output);

.. note::

   Ce contrôleur est générique et réutilisable mais pas optimisé. Il est préférable d'utiliser un contrôleur PID déjà implémenté comme ``PidRBL``.