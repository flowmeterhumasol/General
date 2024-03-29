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

![flow chart](network.png)

A flow sensor is attached to every tap that is used for private consumption. This flow sensor is attached to a dramco uno arduino with a Lora modem attached to it. Via Lora peer to peer connection, this microcontroller will send the water consumption to the central arduino. This central arduino is a dramco uno arduino that will be able to receive and forward the data to the raspberry pi. The raspberry pi will in turn process the data and update the website via a gprs module, this will be done via QCell network. On the website it will be possible to consult the consumption of each family.


### Sensor

At each private tap a client arduino is placed.  Instead of the traditional arduino uno, a Dramcouno designed by Dramco will be used.  The board is a low power and low cost arduino lora board.For  the  demo  version  a [YF-S401 Water Flow Sensor](https://www.tinytronics.nl/shop/nl/sensoren/vloeistof/yf-s401-water-flow-sensor "YF-S401 Water Flow Sensor")  was  used.   This  sensor  has  3  connections,the power supply, the ground and the signal connector.  The sensor returns 98 pulses per secondfor a flow rate of one liter water per minute.  The arduino will be used to count the pulses and tocalculate the water consumption.  Each minute the used quantity of water will be calculated, thistime variable can be adjusted.

Setup:
A DC power source of 9V will be used to supply the sensor and the arduino (through theVIN-pin).Since the interrupt pin of the Dramco uno is internally used, pin 10 will be used to read the pulses. The maximum allowed voltage on this input pin is 5V. Since pin 10 originally is not an interruptpin, an interrupt service routine has to be written in the code that can react to pin changes.When the sensor is powered with 9V, it will return slightly lower than 9V pulses.  A level shifter will be used to lower the 9V pulses to max 5V pulses so it won’t damage the Dramco uno board. Depending on the type of sensor a lever shifter can be left out. The HV pin of the level shifter will be attached to the 9V power supply, the LV has to be attachedto the 5V pin of the dramco uno.  This pin doesn’t give 5V exactly but 4,5V which is perfect forthe max allowance of pin 10.  The pulse pin will be attached to the level shifter, which convertsthe 9V pulses to 4.5V pulses.As mentioned before the Dramco uno board has an integrated lora module.  There is a thresholdwhich determines when the data will be sent to the receiver.  For example when the threshold isset on 50ml, for each 50ml (or more) the raw data will be sent to the receiver.

### Receiver 

The receiver is also a Dramco uno board with an integrated Lora module.  This receiver acts asthe server for the Lora connection.  When data is received through lora connection, it is added tothe circular buffer.  One by one the items in the buffer will be sent through serial connection tothe Raspberry Pi.The Receiver can also receive messages through the serial connection from the RPI. This functionwill be used for the acknowledgement.  An item of the buffer will be repeatedly sent through tothe RPI until the receiver reads the acknowledgement.  After an acknowledgement was received,the  index  of  the  buffer  will  be  incremented  and  the  item  in  the  previous  index  will  be  cleared.Normally each message only has to be sent once to the RPI. Only if it is busy processing the data,it can occur that the receiver has to wait for the acknowledgement.

### RPI

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


Our goal is that the current population of Kunting, the local board and the Water Department of Gambia can consult the water usage in Kunting. We chose to display that the total numbers of liters consumed per private tap each day. The exact times on which the water is consumed is less interesting to us, considering the fact that we wish to keep the website as simple as possible. Based on our daily data, the board will be able to calculate the amount of money that each compound needs to invest.

#### Actual Website 

When talking about the Front-end of the website, we can say that there is a satellite image map on which you can see the location of each tap. If a local resident can point at their tap, they will thus see what number of tap corresponds to theirs. Then they can use the filtering system of the website in order to determine what the water usage was during which period.

I have chosen a relational database type (MySQL) because this is the most frequently used database type and it allows multiple tables to be linked. In this example I would like to make a table with liters/tapnumber/date and apart from that there would be another table with tapnumber/description. In this second table we have the opportunity to explain whose tap this is. 

We have decided to make sure the DynDNS works before our departure, but not before the ending of this semester. We will thus make sure that DynDNS is used in the actual project because this will be necessary if we want to make sure that there is still access to the Raspberry Pi from Belgium even though the Raspberry Pi is situated in Gambia. For our prototype this is not necessary thus we have not yet developped this method.

The Wordpress plugins that have been used to build the website are: wpDataTables, WP Mapbox and WP-forecast. The first plugin was used for responsive data charts, the second one for the satellite imagery and the markers upon that map. The final plugin made sure it was possible to add a weather forecast to the website. This allowed the people to check whether there would be a lot of sun. Because in such a case there will be a lot of water available for consumption.

