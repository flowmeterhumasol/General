# Bachelorproject Flowmeter 



## Table of content 
- [Bachelorproject Flowmeter](#bachelorproject-flowmeter)
  * [Introduction](#introduction)
  * [Concept](#concept)
    + [Sensor](#sensor)
    + [Power supply](#power-supply)
    + [Lora Connectie](#lora-connectie)
    + [GPRS modem](#gprs-modem)
    + [Website](#website)
      - [Concept Website](#concept-website)
      - [Uitwerking Website](#uitwerking-website)

## Introduction 
For this project a flow meter system will be designed.  The system will process the water usage forthe village Kunting in The Gambia.  At each private tap a sensor will be placed then the inputs ofthis sensor will be processed, furthermore the data will be collected and sent to an online database.Ute Naessens has designed a website that is connected to the database.  Through this website thelocal residents can access the data.


## Concept 

![flow chart](Flowchart(2).png)

A flow sensor is attached to every tap that is used for private consumption. This flow sensor is attached to a dramco uno arduino with a Lora modem attached to it. Via Lora peer to peer connection, this microcontroller will send the water consumption to the central arduino. This central arduino is a dramco uno arduino that will be able to receive and forward the data to the raspberry pi. The raspberry pi will in turn process the data and update the website via a gprs module, this will be done via QCell network. On the website it will be possible to consult the consumption of each family.


### Sensor

At each private tap a client arduino is placed.  Instead of the traditional arduino uno, a Dramcouno designed by Dramco will be used.  The board is a low power and low cost arduino lora board.For  the  demo  version  a [YF-S401 Water Flow Sensor](https://www.tinytronics.nl/shop/nl/sensoren/vloeistof/yf-s401-water-flow-sensor "YF-S401 Water Flow Sensor")  was  used.   This  sensor  has  3  connections,the power supply, the ground and the signal connector.  The sensor returns 98 pulses per secondfor a flow rate of one liter water per minute.  The arduino will be used to count the pulses and tocalculate the water consumption.  Each minute the used quantity of water will be calculated, thistime variable can be adjusted.

Setup:
A DC power source of 9V will be used to supply the sensor and the arduino (through theVIN-pin).Since the interrupt pin of the Dramco uno is internally used, pin 10 will be used to read the pulses. The maximum allowed voltage on this input pin is 5V. Since pin 10 originally is not an interruptpin, an interrupt service routine has to be written in the code that can react to pin changes.When the sensor is powered with 9V, it will return slightly lower than 9V pulses.  A level shifter will be used to lower the 9V pulses to max 5V pulses so it won’t damage the Dramco uno board. Depending on the type of sensor a lever shifter can be left out. The HV pin of the level shifter will be attached to the 9V power supply, the LV has to be attachedto the 5V pin of the dramco uno.  This pin doesn’t give 5V exactly but 4,5V which is perfect forthe max allowance of pin 10.  The pulse pin will be attached to the level shifter, which convertsthe 9V pulses to 4.5V pulses.As mentioned before the Dramco uno board has an integrated lora module.  There is a thresholdwhich determines when the data will be sent to the receiver.  For example when the threshold isset on 50ml, for each 50ml (or more) the raw data will be sent to the receiver.

###Reveiver 
The receiver is also a Dramco uno board with an integrated Lora module.  This receiver acts asthe server for the Lora connection.  When data is received through lora connection, it is added tothe circular buffer.  One by one the items in the buffer will be sent through serial connection tothe Raspberry Pi.The Receiver can also receive messages through the serial connection from the RPI. This functionwill be used for the acknowledgement.  An item of the buffer will be repeatedly sent through tothe RPI until the receiver reads the acknowledgement.  After an acknowledgement was received,the  index  of  the  buffer  will  be  incremented  and  the  item  in  the  previous  index  will  be  cleared.Normally each message only has to be sent once to the RPI. Only if it is busy processing the data,it can occur that the receiver has to wait for the acknowledgement.

###RPI
The RPI runs 2 python programs, the first program is the serial receiver which runs continuously.The second program is the uploader which will be scheduled with Cron once a day.The Serial receiver will receive the raw data through USB connection.  The date will be addedand then the data will be written to a text file.  This text file functions as a small local database.The uploader program will be executed once a day.  It will read the local database, calculate thetotal amount of water used for each tap for each day and then upload this to the online database.  Itis obvious that this can only be executed when a network connection is present.  Since the networkin The Gambia is not stable it can occur that the RPI won’t be able to upload for a few days.To solve this problem the local database is able to save data of multiple days, and the uploader iscapable of uploading multiple days at once.  The actions of the uploader will be written to a logfile.



### Power supply
The receiver and RPI will be located in the center of the village.  There is a cottage with solarpanels, a UPS shield will be used to ensure steady power supply to the RPI. UPS shield is a modulethat will charge itself during the day with the energy of the solar panels and at night it will powerthe RPI.The client arduinos and sensors will be powered with rechargeable batteries.  This demo versioncan be powered with a 9V battery.

### Lora Connectie 

Lora stands for Long Range, it is also low power and has a low data rate.  These are the perfectterms for sensor communication.  Lora uses license-free radio frequency bands, in this case 868MHzFor this connection only the number of milliliters will be sent to the receiver.  Each client hasa different ID number.  When the arduino acting as the server receives a message, it will be ableto identify from which tap the message came.

### GPRS modem 

The GPRS modem will allow the RPI to connect to 3G or 4G. 

### Website 

#### Concept Website 

![flow chart website](flowchartWebsite.png)

Het is de bedoeling dat de bevolking, het lokaal bestuur en het water departement van Gambia het verbruik van Kunting kunnen raadplegen. We kozen ervoor om per dag het totaal verbruikte aantal liter per privé kraan beschikbaar te stellen. Het is minder interessant om de exacte tijdstippen weer te geven, aangezien de website zo simpel mogelijk moet blijven. Hierdoor zal het bestuur aan de hand van onze data kunnen berekenen wat de maandelijkse kost van water zal zijn. 

#### Uitwerking Website 

Qua front-end is het de bedoeling dat er een kaart wordt weergegeven op de startpagina en dat je op de locatie van bepaalde kraantjes zal kunnen klikken om specifiek het verbruik op die plek te bekijken en grafieken van op te vragen. Voor grafieken ben ik momenteel op zoek naar een graphic plugin voor Wordpress. De plugin die mij momenteel het meeste aanspreekt is WPDatabases. De plugin die zou zorgen voor de kaart is dan Google Maps Placemarks. (Allemaal nog niet zeker).

Aangezien ik met kaarten wil werken, lijkt het me dan ook een goed idee om te werken met een relationele database. Op die manier heb ik de vrijheid om een tabel bij te houden met het kraannummer, datum en aantal verbruikte liters én daarnaast nog een tabel met het kraannummer en de gps coordinaten die hiermee overeenkomen. Van die tweede tabel zal de kaart plugin dan gebruik maken en die eerste tabel wordt voornamelijk gebruikt door WPDatabases. Het is de bedoeling dat deze relationele database volledig dubbel wordt bijgehouden zodat we op elk moment een volledige backup hebben. De database backup zal dus aanwezig zijn op de sd-kaart van de Raspberry Pi zelf. Dit worden mySQL databases die gesynchroniseerd zullen zijn. De database waarvan Wordpress zal gebruik maken via de plugins is de database die we zullen huren bij een externe hosting provider.

Het is ook de bedoeling dat we via DynDNS ervoor zorgen dat we ook vanuit België altijd toegang hebben tot de Raspberry Pi.

