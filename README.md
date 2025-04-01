# Dribbler

## Carte NAMeC

Dans le dépôt vous trouverez le pdf Main Board_V2.0.1_Schematic.PDF qui correspond au schéma électronique de la main board des robots SSL. La page 4 (ci-dessous) donne les éléments concernant plus particulièrement le dribbler.

<img width="821" alt="image" src="https://github.com/user-attachments/assets/f8aa2cb5-94fa-464e-87f9-b6fb41876f88" />

## Microcontrolleur du dribbler

![alt text](image.png)

STM32F401RCT6

ARM STM32F4 03 RCTE ON 50205 CHN GQ 135
- `STM32` = famille de microcontrôleur basé sur les processeurs ARM Cortex-M 32-bit
- `F` = Mainstream ou High performance
- Modèle du processeur : `3/4` = M4
- Performance : Cette caractéristique représente la vitesse d'horloge (MHz), la RAM et les entrées et sorties. Elle est codée sur 2 chiffres.
- Nombre de pins : `R` = 64-66 
- Taille mémoire flash (en KByte) : `C` = 256
- Package : `T` = LQFP
- Gamme de température : `6` = -40°C à 85°C 

## Notre modèle % code

STM32F401RCTx Device 
STM32F4 01 RCTx
**                      256Kbytes FLASH
**                      64Kbytes RAM

