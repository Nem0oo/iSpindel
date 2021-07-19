# Documentation d'iSpindle

***La traduction est en cours, vous pouvez y participer!
Il se peut que cette traduction ne soit pas aussi avancé que la version allemande, pensez à vérifier.***


Documentation d'iSpindle (iSpindel)
===================

**Hydromètre electronique à faire soi même**
***Soutenez ce projet s'il vous plait***  

[![Donner](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://www.paypal.me/universam)


![iSpindle dans de l'eau claire](/pics/floating.jpg)
![Tableau de bord](/pics/Dashboard.jpg)


## Table des matières


- [FAQ](FAQ-en.md)
- [Parts](Parts_en.md)
- [Calibration](Calibration_en.md)
- [Diagramme du circuit](circuit_diagram_en.md)
- [Upload FHEM](upload-FHEM_en.md)
- [Ubidots scripting](ubidotsscripting_en.md)


- [License](#license)
- [Principe](#principe)
  - [Metacentric Height](#metacentric-height)
- [Construction](#construction)
  - [Componsants](#composants)
  - [Circuit Diagram](#circuit-diagram)
  - [Sled](#sled)
- [Configuration](#configuration)
  - [Ubidots](#ubidots)
  - [Portal](#portal)
  - [BierBot Bricks](#bierbot-bricks)
  - [Blynk](#blynk)
- [Graphical User Interface](#graphical-user-interface)
  - [Calibrating the Spindle](#calibration)
  - [Ubidots Graphen](#ubidots-graphen)
  - [CraftBeerPi](#craftbeerpi)
- [Logiciel](#logiciel)


***

## License


> Tous droits réservés, toute utilisation commerciale interdite et enfreindrait les droits applicable.

***

## Principe

Promu par le sujet [Alternative to the Spindle](http://hobbybrauer.de/forum/viewtopic.php?f=7&t=11157&view=unread#p170499), l'idée était née de reproduire le spindler electronique à inclinaison grace à des composant peu onéreux.

Le systeme est basé sur l'utilisation d'un cylindre pouvant s'incliner, nu concepte facile et ingénieux - vous n'avez pas besoin de référence extérieure (sauf la gravité) et le cylindre est extrement simple a nettoyer. L'angle d'inclinaison change avec la flotaison, qui est en relation directe avec le taux de sucre. Il y a un angle formé entre le le centre de masse et le centre de flotaison en relation avec la densité du liquide.

![Kränung](/pics/kraengung.jpg)

Donc l'idée est de placer un outils connecté en wifi disposant d'un accéléromètre et d'un capteur de temperature dans un cylindre flottant. Le système evaluera les capteurs toutes les X minutes, se connectera au wifi et enverra son angle d'inclinaison, sa température et l'état de sa batterie à un service en ligne comme Ubidots.com pour enregistrer les données.
Avec un interval de mise à jour de 30 minutes, il a été possible de faire durer la batterie 3 mois ! 

### *Hauteur Metacentrique*

Il s'agit en fait du "metacentre de gravité", le cylindre s'inclinera au fur et à mesure que la densité du liquide varie proportionnelement à son centre de de gravité et son centre de flotaison. L'angle de cette inclinaison peut alors etre mesurée.

Il est possible de lester le cylindre en ajoutant quelques grammes au fond pour que le cylindre soit plus droit, ou sur le bouchon, pour qu'il soit plus incliné.

Le logiciel calcule l'angle d'Euler pour X et Y à partir des valeurs XYZ de l'acceléromètre et forme l'angle absolu. Nous transformons ces valeurs avec les paramètres de calibratio en ° Plato, SG ou equivalent grace à une table de références.

***

## Construction

### see [Parts](Parts_en.md)

### see [Sourcing](Parts_en.md)

### see [Circuit Diagram](circuit_diagram_en.md)

### see [iSpindel Breadboard Mounting](iSpindelbreadboard_en.md)

***

### Drawer
- A 3D Printed drawer is used to secure the electronics and battery inside the plastic housing as shown below. The 3D model can be found in the repository.
![Sled](/pics/Schlitten_cad.jpg)
![Assembled](/pics/assembled2.jpg)
![Assembled](/pics/assembled.jpg)

<a href="http://www.youtube.com/watch?feature=player_embedded&v=gpVarh8BxhQ" target="_blank"><img src="http://img.youtube.com/vi/gpVarh8BxhQ/0.jpg" 
alt="Druck" width="240" height="180" border="10" /></a>



***

## Configuration

### Ubidots

- To start, you must create a free account at [Ubidots.com](https://ubidots.com)
- Next, you must go to the menu  ```API Credentials``` to get a ```Token``` to be used by the iSpindle to authorize writing data to the Ubidots account.
***Write this down.***  
![Token](/pics/UbiToken.jpg)  

#### Portal

By pressing the  ```Reset Button``` the Wemos creates an access point, which allows you to make the necessary settings to configure the device. **In `operation mode` this portal is not active or accessible because the principle of this design is based on shorted possible acitve time. Basically it will wake up, send its data and deep sleep again. This takes now less than 3s which is directly related to its long life run time.

> The ```iSpindel``` signals the `config mode` by blinking the LED at a 1s interval.  
By saving your settings or waiting timeout of 5min it will end the Portal thus AccessPoint and try to go into `operation mode`.


   ![Setup](/pics/configuration.png)

   ![AccessPoint](/pics/AP.png)![Portal](/pics/Portal.png)

  ![Info](/pics/info.png)

> In Ubidots you can monitor the update of data unders ```Sources``` where the iSpindel will create a new device itself.  
In the ```Dashboard``` now you can create your nice graphs.

### BierBot Bricks

The setup with BierBot Bricks is easy and for free. You will need the iSpindle Firmware  `7.1.0` or later. 

1. Create a free BierBot Bricks account [here](https://bricks.bierbot.com/#/register).
2. After Registration, select "Bricks" in the menu on the left (see 1 in the picture).
3. Hit the blue "Add Brick" button in the top right corner.
4. Select "iSpindel" in the popup and copy the displayed API key into your clipboard.
5. Now open the configuration portal of your iSpindel (by pressing reset multiple times, see [portal](#portal) for more info).
6. Select "Bricks (free & easy)" as service (see 2 in the image).
7. Paste the api key from your clipboard into the "Token/ API key" field and hit the blue save bottom at the bottom.
8. Now go back to [bricks.bierbot.com](https://bricks.bierbot.com/#/) and select "Equipment" in the menu on the left (see 3 in the image).
9. Create a new device (blue button, top right corner), select "**Fermenter**" in the popup.
10. Now assign the gravity sensor from the iSpindle to the respective field of your fermeter by **drag & dropping** - the respective target dropzone on your fermenter will be highlighted green to guide you. You can do the same for your temperature sensor, but this is optional.
11. Hite "Save".
12. To start recording, we will also need a recipe. Go to "Recipes" on the left and create a recipe. You only need to setup one (dummy) fermentation step. Save the recipe and go back to the list of recipes. Start your recipe by clicking the orange play button.

**Done!**

![Bricks tutorial](../pics/ispindle_bricks_tutorial.png)

### Blynk

#### Cloning the Example

* For this example you will need 1800 (4x200 + 100 + 900) energy points, which allows to use the free version;
* Download Blynk App: [Android](http://j.mp/blynk_Android) [iOS](http://j.mp/blynk_iOS)
* Touch the QR-code icon and point the camera to the code below
<p><img src="https://image.ibb.co/gxZFDz/Untitled.png" height="95" /></p>

<img src="/pics/BlynkApp.jpg" width="35%" />  <img src="/pics/BlynkQR.jpg" width="35%" />

* Enjoy the app!

#### Using your own App

* If you want to use your own App or modify the example the Virtual Pins are defined as follows:
1. V20 - Temperature withouth unit;
2. V30 - Battery without unit;
3. V1 - String Tilt in degress;
4. V2 - String Temperature degress `C or F or K`;
5. V3 - String Battery `Volts`;
6. V4 - Gravity;

***
## Graphical User Interface

### Calibration

> In order to convert the measured iSpindle angle to degrees Plato (°Plato), density (SG), or other units, it is necessary to first calibrate the sensor by making several reference measurements of sugar water of known gravities. These reference measurements can then be converted to a mathematical function which is stored for later measurements and display. Since each self-assembled iSpindle will  have different measured values, each device must be individually calibrated after assembly or reassembly to yield accurate measurements. A detailed procedure is below.

[Calibration Procedure](Calibration_en.md)

### Ubidots Graphen

- [Plato Formula](Calibration_en.md#forumla)

### CraftBeerPi

[CraftBeerPi](https://github.com/universam1/iSpindel/issues/3)
***

## Logiciel 

### Firmware flashing

[Firmware flashing](Firmware_en.md)

***Si vous l'aimez, faites le moi savoir***  :beers:

[![Donner](https://www.paypalobjects.com/de_DE/DE/i/btn/btn_donate_LG.gif)](https://www.paypal.me/universam)
