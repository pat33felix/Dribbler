# Dribbler

## Carte NAMeC

Dans le dépôt vous trouverez le pdf Main Board_V2.0.1_Schematic.PDF qui correspond au schéma électronique de la main board des robots SSL. La page 4 (ci-dessous) donne les éléments concernant plus particulièrement le dribbler.

<img width="821" alt="image" src="https://github.com/user-attachments/assets/f8aa2cb5-94fa-464e-87f9-b6fb41876f88" />

## Microcontrolleur du dribbler

![image](https://github.com/user-attachments/assets/23014840-5b39-4a6e-8232-77ea5e358f85)

Sur la carte il apparaît que le microcontrolleur soit un STM32F401RCT6...

## Une première utilisation basique des outils STM32CubeMX et STM32CubeIde

STM32CubeMX est un outil graphique qui permet de configurer facilement les microcontrôleurs STM32 et de générer du code d'initialisation pour diverses bibliothèques logicielles. STM32CubeIde contient toutes les fonctionnalités d'une chaine de développement classique : édition, compilation, téléversement, debogage, etc.

## Étapes pour utiliser STM32CubeMX

Cette vidéo pourra vous être utile : https://www.youtube.com/watch?v=Yw5lS55bL5I.

1. **Télécharger et installer STM32CubeMX et STM32CubeIde**
    - Rendez-vous sur le site officiel de STMicroelectronics pour télécharger STM32CubeMX et STM32CubeIde.
    - Suivez les instructions d'installation fournies.

2. **Créer un nouveau projet**
    - Ouvrez STM32CubeMX et cliquez sur "ACCESS TO BOARD SELECTOR".
    - Sélectionnez votre microcontrôleur ou carte de développement STM32 : par exemple `STM32F3DISCOVERY`.
    - Cliquez sur `Start Project`

3. **Configuration du projet**
    - Dans l'onglet `Project Manager` préciser le nom du projet, et que vous utiliserez l'IDE STM32CubeIDE (`Toolchain / IDE`).
(Nous verrons plus tard d'autres éléments de configuration)

4. **Générer le code**
    - Une fois la configuration terminée, cliquez sur `Generate Code`.

5. **Importer le projet dans l'IDE**
    - Une fois le code généré avec succès, vous pouvez ouvrir votre projet (Cliquez sur `Open Project`).
    - Votre IDE s'ouvrira sur votre projet (d'autres IDEs comme par exemple, STM32CubeIDE, Keil, IAR auraient pu être choisis...).
    - Compilez.
    - Exécutez : cela téléchargera le code sur votre microcontrôleur.
Dans la configuration, vous aurez préciser comment télécharger. Ici, c'est avec la sonde ST-LINK intégrer dans le kit de développement.
![alt text](image-3.png)
6. **Modifier votre projet**

Par exemple, pour notre carte de développement `STM32F3DISCOVERY`, on pourra rajouter ce code pour un clignotement des 8 LEDs du kit :

```C
  /* -1- Enable GPIOE Clock (to be able to program the configuration registers) */
  __HAL_RCC_GPIOE_CLK_ENABLE();

  /* -2- Configure PE.8 to PE.15 IOs in output push-pull mode to drive external LEDs */
  GPIO_InitStruct.Pin = (GPIO_PIN_15 |GPIO_PIN_14 |GPIO_PIN_13 |GPIO_PIN_12 |GPIO_PIN_11 | GPIO_PIN_10 | GPIO_PIN_9 | GPIO_PIN_8);
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_PULLUP;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_HIGH;

  HAL_GPIO_Init(GPIOE, &GPIO_InitStruct);
  while (1)
  {
	    HAL_GPIO_TogglePin(GPIOE, GPIO_PIN_15);
	    HAL_GPIO_TogglePin(GPIOE, GPIO_PIN_14);
	    HAL_Delay(1000);
	    HAL_GPIO_TogglePin(GPIOE, GPIO_PIN_13);
	    HAL_GPIO_TogglePin(GPIOE, GPIO_PIN_12);
	    HAL_Delay(1000);
	    HAL_GPIO_TogglePin(GPIOE, GPIO_PIN_11);
	    HAL_GPIO_TogglePin(GPIOE, GPIO_PIN_10);
	    HAL_Delay(1000);
	    HAL_GPIO_TogglePin(GPIOE, GPIO_PIN_9);
	    HAL_GPIO_TogglePin(GPIOE, GPIO_PIN_8);
	    HAL_Delay(1000);
  }
```

- Compilez et exécutez.

**Attention** : Pour le téléchargement et deboguage, un driver doit être installé. Voici le lien avec les installations pour Windows (st-stlink-server.xxxx.msi), MACOS (st-stlink-server.xxxx.pkg) et Linux (en fonction de votre système).
https://www.st.com/en/development-tools/st-link-server.html#get-software

6. **Déboguer et tester**
Nous avons utilisé les outils de débogage de notre IDE pour tester et déboguer notre application. Par exemple :
- Mettre des points d'arrêt
- Regarder l'évolution d'une variable
- Etc.

## Étapes pour utiliser STM32CubeMX avec le code du dribbler de NAMeC

### Problèmes dans le code

Dans le dépôt vous trouverez le code du dribbler dans le zip dribbler-2023-master.zip. Après avoir déployé le projet, il se trouve que des éléments sont manquants. On peut citer :
- dossiers vides : nanopb et sensor-data-protocol
- des fichiers manquants...

### Problèmes pour flasher le code

Procédure réalisée :
- vérification version Jlink 
- vérification probe Jlink 
- Run configuration :
	- main : project et C/C++ Application verifier que c'est le bon projet
 	- debugger : GDB port number régler à 3333; SWD : 8000 kHz speed, Device : STM32F40RCT6; paramettre identique au .cfg
  	- Serial Wire Viewer : enable et t
 
Voici le résultat obtenu lors du flashage :

```csh
SEGGER J-Link GDB Server V8.12c Command Line Version
JLinkARM.dll V8.12c (DLL compiled Jan 22 2025 12:55:59)
Command line: -port 3333 -s -device STM32F401RCT6 -endian little -speed 8000 -vd
-----GDB Server start settings-----
GDBInit file: none
GDB Server Listening port: 3333
SWO raw output listening port: 2332
Terminal I/O port: 2333
Accept remote connection: localhost only
Generate logfile: off
Verify download: on
Init regs on start: off
Silent mode: off
Single run mode: on
Target connection timeout: 0 ms
------J-Link related settings------
J-Link Host interface: USB
J-Link script: none
J-Link settings file: none
------Target related settings------
Target device: STM32F401RCT6
Target device parameters: none
Target interface: JTAG
Target interface speed: 8000kHz
Target endian: little

Connecting to J-Link...

J-Link is connected.

Failed to get index for device name 'STM32F401RCT6'.GDBServer will be closed...
Shutting down...
Could not connect to target.
Please check power, connection and settings.
```
  
 





