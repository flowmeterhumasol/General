# Bachelorproject Debietmeter 

Dit is de algemene bescrhijving van de werking van de debietmeter. 

## Inhoud

- [Bachelorproject Debietmeter](#bachelorproject-debietmeter)
  * [Inhoud](#inhoud)
  * [Inleiding](#inleiding)
  * [Concept](#concept)
    + [Sensor](#sensor)
    + [Lora Connectie](#lora-connectie)
    + [GPRS modem](#gprs-modem)
    + [Voeding](#voeding)
    + [Website](#website)
      - [Concept Website](#concept-website)
      - [Uitwerking Website](#uitwerking-website)

## Inleiding

Dit debietmeter-project is deel van een ontwikkelingsproject van de organisatie Humasol, in samenwerking met Warme Gloed. De goal van dit project is om in Kunting - een dorpje in Gambia - een zonnepomp te installeren en het leindingetwerk uit te breiden. In een groep van 4 studenten zullen we een jaar lang dit prject technisch uitwerken. In de zomer van 2021 zullen we voor 9 weken naar Kunting trekken om alles te installeren.  

Als uitbreiding op dit project ontwerpen we een debietmeter. Deze zal ervoor zorgen dat de VDC (lokaal bestuur) het verbruik van de bevolking kan opvolgen, en gebruiken voor een betalingssysteem uit te werken. Ook zal 'The water departement' en universiteit van Gambia toegang hebben tot de data voor onderzoek. 

De planning kan op volgende link worden geraadpleegd  [Gantt chart](https://share.clickup.com/g/h/4dy0f-15/a6a5620b579de9b "Gantt chart"). 

## Concept 

![flow chart](Flowchart.png)

Aan elke kraan die voor privé verbruik gebruikt wordt, wordt een flowsensor bevestigd. Deze flowsensor is bevestigd aant een darmco uno arduino met een Lora modem op bevestigd. Via Lora peer to peer connectie zal deze microcontroller het verbruik doorsturen naar de centrale arduino. Deze centrale arduino is een dramco uno arduino die de gegevens zal kunnen ontvangen en doorsturen naar de raspberry pi. De raspberry pi zal op zijn beurt de data verwerken en via een gprs module de website updaten, dit zal gebeuren via QCell network.  Op de website zal men kunnen raadplegen wat het verbruik is van elke famillie.

We hebben het project opgedeeld in verschillende repositories en zullen de definitieve samengevoegde code onder 'General' plaatsen. 


### Sensor

Momenteel zijn we een proefopstelling aan het maken met de goedkope sensor [YF-S401 Water Flow Sensor](https://www.tinytronics.nl/shop/nl/sensoren/vloeistof/yf-s401-water-flow-sensor "YF-S401 Water Flow Sensor").  

In Kunting zullen we gebruik maken van de 800 Series Low Power Flowmeter. Dit is een low power flowmeter die foodsafe is met een diameter van 8mm en op 3V3 werkt. De verdeler van deze flowmeter moeten we nog contacteren. 


### Voeding

De arduino uno heeft een spanning van 5-12V nodig. De 800 Series Low Power Flowmeter heeft maar 3.3V nodig. We zullen de arduino voeden door knoopcel batterijen (CR2032), en de sensor bevestingen aan de 3V3 pin. 

De centrale arduino met Raspberry Pi bevinden zich in een centraal oplaad hutje waar elektriciteit is op zonne energie.

### Lora Connectie 

Lora is een long range wide area netwerk. Via dit netwerk kunnen we aan langeafstandscommunicatie doen met weinig vermogen. Via radio frequentie 868 MHz( die voor IOT is vrijgehouden) zullen we onze data sturen van de nodes ( flowmeters) naar de gateway (de arduino bij de raspberry pi). Op de Dramco uno bordjes zit een lora module geïntegreerd. 

### GPRS modem 

### Backup dingne ute hoe noemt da nu weer?

### Website 

#### Concept Website 

![flow chart website](flowchartWebsite.png)

Het is de bedoeling dat de bevolking, het lokaal bestuur en het water departement van Gambia het verbruik van Kunting kunnen raadplegen. We kozen ervoor om per dag het totaal
verbruikte aantal liter per privé kraan beschikbaar te stellen. Het is minder interessant om de exacte tijdstippen weer te geven, aangezien de website zo simpel mogelijk moet blijven. Hierdoor zal het bestuur aan de hand van onze data kunnen berekenen wat de maandelijkse kost van water zal zijn. 

#### Uitwerking Website 


