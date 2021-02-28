# Bachelorproject Debietmeter 

Dit is de algemene bescrhijving van de werking van de debietmeter. 

## Inhoud
<details>
  <summary>Klik om uit te vouwen</summary>
- [Bachelorproject Debietmeter](#Bachelorproject Debietmeter)

</details>

## Situering

Dit debietmeter-project is deel van een ontwikkelingsproject van de organisatie Humasol, in samenwerking met Warme Gloed. De goal van dit project is om in Kunting - een dorpje in Gambia - een zonnepomp te installeren en het leindingetwerk uit te breiden. In een groep van 4 studenten zullen we een jaar lang dit prject technisch uitwerken. In de zomer van 2021 zullen we voor 9 weken naar Kunting trekken om alles te installeren.  

Als uitbreiding op dit project ontwerpen we een debietmeter. Deze zal ervoor zorgen dat de VDC (lokaal bestuur) het verbruik van de bevolking kan opvolgen, en gebruiken voor een betalingssysteem uit te werken. Ook zal 'The water departement' en universiteit van Gambia toegang hebben tot de data voor onderzoek. 

## Concept 

![flow chart](Flowchart.png)

Aan elke kraan die voor privé verbruik gebruikt wordt, wordt een flowsensor bevestigd. Deze flowsensor is bevestigd aant een darmco uno arduino met een Lora modem op bevestigd. Via Lora peer to peer connectie zal deze microcontroller het verbruik doorsturen naar de centrale arduino. Deze centrale arduino is een dramco uno arduino die de gegevens zal kunnen ontvangen en doorsturen naar de raspberry pi. De raspberry pi zal op zijn beurt de data verwerken en via een gprs module de website updaten, dit zal gebeuren via QCell network.  Op de website zal men kunnen raadplegen wat het verbruik is van elke famillie.

We hebben het project opgedeeld in verschillende repositories en zullen de definitieve samengevoegde code onder 'General' plaatsen. 

### Lora Connectie 


