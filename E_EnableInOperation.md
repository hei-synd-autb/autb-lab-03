<h1 align="center">
  <br>
  <img src="./img/hei-en.png" alt="HEI-Vs Logo" width="350">
  <br>
  HEI-Vs Engineering School - Industrial Automation Base
  <br>
</h1>

Cours AutB

Author: [Cédric Lenoir](mailto:cedric.lenoir@hevs.ch)

# Modèle Enable InOperation
Utilisation typique, un régulateur qui fonctionne en continu. Une fois que le bloc est opérationnel, ``InOperation``. La régulateur fonctionne normalement.

Le bloc fonctionnel [MC_Power](#un-bloc-fonctionnel-de-type-enable) mentionné ci-dessous fonctionne sur le même principe, simplement la sortie ``InOperation`` est renommée ```Status```.

Tant que la ``Status`` est ``TRUE``, cela signfie que l'axe est sous couple. Si il s'agissait d'un axe pneumatique, on pourrait dire sous pression.

### Enable In Operation Base / State machine
<figure>
    <img src="./puml/EnableInOpBase/EnableInOpBase.svg"
         alt="Lost image : EnableInOpBase">
    <figcaption>Function Block Enable InOperation Base</figcaption>
</figure>

### Définition des états

|State |Id |Description |
|------|---|-------------|
|STATE_IDLE |999 |Starting state of a function block|
|STATE_INIT |1 |Initialization of the function block State runs after the function block starts. All preparations required for the actual operations are made here. A sub-state machine is possible.|
|STATE_INOP| 2 |Working state of the function block In this state, the actual task of the function block is executed.|
|STATE_ERROR| 3 |Error state| State is active after an error occurs. It is exited by resetting “Enable” or “Execute”.|

> La définition des valeurs de l'énumération est facultative. L'habitude de mettre ce genre de valeur, **999** pour ``STATE_IDLE`` résulte de l'idée de se dire qu'une variable à 0 pourrait être une variable que l'on a oublié d'initialiser.

### Déclaration de l'ENUM et commentaire.
```iecst
{attribute 'qualified_only'}
{attribute 'strict'}
TYPE E_EnableInOperation :
(
    // Starting state of a function block
	STATE_IDLE  := 999,
    // Initialization of the function block State
    STATE_INIT := 1,
    //  Working state of the function block
    STATE_INOP := 2,
    // State is active after an error occurs
    STATE_ERROR := 3
)DINT := STATE_IDLE;
END_TYPE

// Declaration
    eEnableInOperation  :   E_EnableInOperation;

// Use, not that ordre is not important
CASE eEnableInOperation OF
    E_EnableInOperation.STATE_ERROR  :
        ;
    E_EnableInOperation.STATE_IDLE  :
        ;
    E_EnableInOperation.STATE_INIT  :
        ;
    E_EnableInOperation.STATE_INOP  :
        ;    
END_CASE
```
