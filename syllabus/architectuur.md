# Architectuur

## Introductie

### Een 'computer'

````{figure} assets/image-20231127113605443.png
---
scale: 50%
align: left
---
Een vroege computer slechts 

geschikt voor één enkele taak
````

Het woord computer komt van *computare* wat berekenen betekent. Tot zo'n honderd jaar geleden was het woord *computer* een functieomschrijving voor mensen die berekeningen deden. Het apparaat *computer* was in het begin ook niet veel meer dan een apparaat wat berekeningen deed. De eerste computers werden gebouwd met een specifiek doel voor ogen. Een voorbeeld daarvan zijn simpele rekenmachines. Deze computers konden alleen berekeningen uitvoeren waar ze op gebouwd waren. Dat betekende dat je een apart apparaat moest hebben, wanneer je verschillende soorten berekeningen wilde uitvoeren.

Dit veranderde ten tijde van de Tweede Wereldoorlog. De Britten en de Amerikanen waren onafhankelijk van elkaar bezig met het conceptualiseren van een computer die niet voor 1 doel gebouwd was, maar die geherprogrammeerd kon worden. In het Verenigd Koninkrijk was dit Alan Turing in 1936 met zijn ideeën over een *universal machine*. In de Tweede Wereldoorog ging Turing voor de Britse overheid werken om de codes van de Duitsers te kraken. Om te helpen bij het kraken van de Duitse encryptiemethode Enigma, zijn de *bombes* gebouwd. Dit waren computers, geschikt voor één enkele taak. Wat nieuw aan deze, van relais gebouwde, bombes was, was dat er logica en statistiek gebruikt werden als basis van het ontwerp. Bij het kraken van een moeilijkere Duitse encryptiemethode, kon de bombe niet meer gebruikt worden. De Colossus werd ontworpen. Hierbij werd niet meer gebruik gemaakt van relais, maar van electronenbuizen.
De Britten waren er met de Colossus in geslaagd om een programmeerbare computer te bouwen, twee jaar voordat de Amerikanen dat lukte. Dit gebeurde alleen in het diepste geheim, omdat deze computers een belangrijke rol speelden in de oorlog. Kijktip: The Imitation Game. Je zult in deze film de bombes aan het werk zien.

```{figure} assets/image-20231127120234994.png
---
align: center
---
De Colossus in Bletchley Park
```
In de Verenigde Staten waren het Eckert en Mauchly die zich bezighielden met een concept voor het opslaan van programma’s in herprogrammeerbaar geheugen. Von Neumann ging met dit concept aan de haal en publiceerde het werk van de heren onder zijn naam. Daarom kennen we nu het begrip “von Neumann principe”. Uiteindelijk slaagden de Amerikanen erin om de ENIAC te bouwen. De ENIAC was niet alleen programmeerbaar, maar ook herprogrammeerbaar. De Amerikaanse belastingbetaler werd voorgeschoteld dat de ENIAC gebruikt werd voor het berekenen van weersvoorspelling. Dit was ook de dagtaak van de ENIAC. Maar ’s nachts werd de ENIAC in het geheim gebruikt door het Amerikaanse leger om berekeningen te maken voor de ontwikkeling van H-bommen (een atoombom op basis van waterstof).

```{figure} assets/image-20231127120643374.png
---
align: center
---
De ENIAC
```

### De basis  van een computer

Tegenwoordig gebruiken we het woord computer voor een apparaat dat een invoer kan verwerken met behulp van opgeslagen instructies, en het resultaat als uitvoer geeft. Dit noemen we ook wel het *von Neumann principe*: invoer, opslag, verwerking, en uitvoer.

```{figure} assets/image-20231128103708372.png
---
align: center
---
Het schema van de von Neumann architectuur
```

De **invoer** is de manier waarop de gebruiker signalen naar de computer stuurt. Dit kan bijvoorbeeld het indrukken van een toets of het verschuiven van een muis zijn.

De **verwerking** neemt deze invoer en maakt daar berekeningen mee. Welke berekeningen gedaan worden is afhankelijk van het opgeslagen programma. Dit programma zijn de instructies die in de **opslag** bewaard staan. Dit kan bijvoorbeeld een berekening zijn die de ingetoetste letter verwerkt en de bijbehorende pixels berekent om die letter weer te geven, of bijvoorbeeld een berekening die de nieuwe positie van de cursor uitrekent. De opslag noemen we ook wel het geheugen. In dit geheugen worden zowel instructies als data opgeslagen. 

De **uitvoer** is de manier waarop de computer de resultaten van de berekeningen teruggeeft aan de gebruiker. Bijvoorbeeld het beeldscherm, waar letters op verschijnen of een cursor beweegt door andere pixels te laten branden. Dit kan de gebruiker dan weer zien.

:::{Tip}
Bekijk ook de [video "How computers work: CPU, Memory, Input & Output"](https://youtu.be/DKGZlaPlVLY)  van (http://code.org). Nederlandse ondertiteling aanwezig.
:::

### Een stukje technischer

De uitwerking van het von Neumann principe leidt tot een computerarchitectuur die verschillende onderdelen bevat. Die onderdelen werken samen om een programma uit te voeren. Vooral de verwerkingseenheid heeft een hoop verschillende onderdelen met ieder zijn eigen taakje.

Het *geheugen* (*memory*) is een opslagplek met heel veel vakjes. Ieder vakje kan een beperkte hoeveelheid data bevatten. De data in de vakjes zijn instructies voor een *processor*. De processor verwerkt de data en maakt op basis daarvan berekeningen en keuzes. Meerdere instructies samen vormen een *computerprogramma*.

```{figure} assets/image-20231128105716129.png
---
scale: 50%
align: right
---
```

*Control Unit*: De CU zorgt ervoor dat instructies één voor één opgehaald worden uit het geheugen, uitgevoerd worden, en dat eventueel data terug naar het geheugen verplaatst wordt om op te slaan.

*Arithmetic and Logic Unit*: De ALU kan twee waardes bij elkaar optellen of van elkaar af halen en kan simpele logische instructies uitvoeren.

*Accumulator*: kan tijdelijk een waarde onthouden. Hier wordt de uitkomst van de ALU bewaard totdat deze in het geheugen gezet kan worden. Invoer (input) komt ook eerst in de accumulator te staan zodat deze verwerkt kan worden, en ook resultaten van een berekening worden vanuit de accumulator als uitvoer (output) gegeven.

### Even opfrissen

De onderdelen die in een computer zitten hebben jullie al bij [Basis van Computer Science](https://qhinf.github.io/syllabi/basis_cs/) eens eerder gezien. We gaan ons in de komende paragrafen verdiepen in deze onderdelen en kijken hoe ze nou precies samenwerken. Dit doen we met de von Neumann architectuur in ons achterhoofd.

```{figure} assets/image-20231128110209429.png
---
align: center
---
```

:::{exercise}
Benoem de onderdelen in bovenstaand figuur
:::

## De processor

### Berekenaar

Elke aanpassing of verandering die met een computer gedaan wordt is in feite een berekening. Deze berekeningen worden gedaan door de processor. Die processor is dus eigenlijk het brein van de computer. De processor is non-stop aan het rekenen. Dat vraagt een hoop energie. Wanneer we energie omzetten dan komt daar ook warmte bij vrij. Hoe harder de processor moet werken, hoe meer energie-omzetting, hoe meer warmte er gegenereerd wordt. De processor kan zelfs oververhit raken, daarom moet hij goed gekoeld worden.

De processor is dus de verwerkingseenheid in de von Neumann architectuur. Die zorgt ervoor dat de invoer die de gebruiker geeft wordt verwerkt tot een resulterende uitvoer. De Engels term voor processor is Central Processing Unit (CPU).

### Transistoren en instructies

Een processor moet continu berekeningen doen. Die berekeningen gebeuren door stroompjes te laten lopen door schakelaars en logische schakelingen. Die schakelaars noemen we transistoren. Dat zijn elektrische componenten die stroom wel (1) of niet (0) door laten. Hoe die logische schakelingen en schakelaars samenwerken om berekeningen te maken is behandeld in het vorige hoofdstuk. Wanneer we de logische schakelingen van de *NOT*, *AND*, en *OR* poort uittekenen dan worden de transistoren meestal weergegeven met een cirkel waar 3 lijnen in samenkomen.

```{figure} assets/image-20231128111315775.png
---
align: left
scale: 15%
---
Logische poorten gemaakt met transistoren
```
De voorloper van de transistor was de elektronenbuis. Een grote glazen vacuümbuis die wel iets weg had van een gloeilamp. Deze vacuümbuizen werden gebruikt om elektrische signalen te versterken. Dit was vooral handig voor het oppikken van zwakke radiosignalen in de lucht. Door deze signalen met een vacuümbuis te versterken konden de radiosignalen in de lucht hoorbaar gemaakt worden. De huidige transistoren kunnen nog steeds elektrische signalen versterken maar in de processor is hun doel om elektrische signalen aan en uit te zetten om zo eentjes en nulletjes te maken.

```{figure} assets/image-20231128111453360.png
---
align: right
scale: 50%
---
Vlnr: vacuümbuis, transistor, IC en SMD IC
```
De eerste vacuümbuizen waren nog groter dan je hand en ze werden heel warm. Niet erg praktisch dus. Met de komst van de transistor was zo’n schakelaar ineens kleiner dan een vinger, en ze werden ook niet meer zo warm. De technologie ging snel vooruit, en de transistoren werden zo klein dat er chips kwamen waar steeds meer transistoren op pasten. Die vooruitgang staat bekent als de Wet van Moore. Iedere 2 jaar verdubbelt het aantal transistoren op dezelfde microchip-oppervlakte. Op deze manier verdubbelt de rekenkracht dus iedere twee jaar, een exponentiële toename. Tegenwoordig (2023) maken we al processoren op een schaalgrootte van 5$nm$. Dat wil zeggen dat de transistoren slechts enkele atomen groot zijn. Voor de mensen, die al wat meer natuurkunde en quantumtheorie hebben gehad: op deze schaal moet je als ontwerper rekening houden met bizarre quantumverschijnselen, zoals het [tunneleffect](https://nl.wikipedia.org/wiki/Tunneleffect).

:::{Tip}
Bekijk ook de video [“De transistor: een schakelaar voor de moderne tijd”](https://schooltv.nl/video/de-transistor-een-schakelaar-voor-de-moderne-tijd/) van Schooltv
:::

### ALU, CU, Registers

In de vorige paragraaf heb je gezien dat wanneer we het Von Neumann-principe uitwerken, daar verschillende benodigdheden uit ontstaan. Het geheugen (memory) was hier een onderdeel van. Daar gaan we komende paragraaf 3.3 verder op in. De andere benodigdheden bevinden zich in de processor (Central Processing Unit). De Control Unit en de Arithmetic/Logic Unit zijn al besproken. We herhalen ze nog even.

*Control Unit*: De CU zorgt ervoor dat instructies één voor één opgehaald worden uit het geheugen, uitgevoerd worden, en dat eventueel data terug naar het geheugen verplaatst wordt om op te slaan.

*Arithmetic and Logic Unit*: De ALU kan twee waardes bij elkaar optellen of van elkaar af halen en kan simpele logische instructies uitvoeren.

*Accumulator*: kan tijdelijk een waarde onthouden. Hier wordt de uitkomst van de ALU bewaard totdat deze in het geheugen gezet kan worden. Invoer (input) komt ook eerst in de accumulator te staan zodat deze verwerkt kan worden, en ook resultaten van een berekening worden vanuit de accumulator als uitvoer (output) gegeven.

```{figure} assets/image-20231128112541527.png
---
align: center
---
Schema van een CPU
```
Nu iets nieuws: Dit tijdelijk onthouden van data gebeurt met *registers*. Een register die je al kent is de accumulator, maar er zijn er nog meer.  Al deze registers bewaren tijdelijk een waarde of instructie. Bijvoorbeeld wanneer deze uit het geheugen opgehaald is of wanneer er iets terug in het geheugen geplaatst moet worden. 

*Program Counter*: houdt bij wat het volgende vakje in het geheugen is die uitgevoerd moet worden. Ieder vakje in het geheugen kan bepaalde waardes op slaan. Wat die waardes betekenen is verschillend. Zo kan een vakje een *instructie* bevatten (bijvoorbeeld optellen of aftrekken), een *doeladres* van een ander geheugenvakje bevatten, of een *getal* bevatten (bijvoorbeeld 17 of 88). 

*Current Instruction Register (CIR):* Wanneer een geheugenvakje een instructie bevat dan wordt deze instructie even tijdelijk naar de CIR gestuurd zodat de instructie vanaf daar verwerkt en uitgevoerd kan worden. Dit kan bijvoorbeeld een instructie zijn dat de waarde van de accumulator in een geheugenvakje geschreven moet worden. Of dat het getal in een geheugenvakje bij de accumulator opgeteld moet worden.

*Memory Address Register (MAR):*  In de voorbeelden die hier voor de CIR gegeven worden is er dus nog een ander geheugenvakje nodig. Dit is het doelvakje voor de instructie. Wanneer de processor een instructie gaat uitvoeren zoals het lezen of schrijven van data, dan staat er in de MAR wat het adres van het doelvakje is en welk geheugenvakje er dan dus gelezen of beschreven gaat worden. 

*Memory Data Register (MDR):* Ook de data die gelezen of geschreven wordt, gaat naar een register. Dit register bewaart dus geen instructies of adressen, maar alleen data in de vorm van getallen. Daar is weer een apart plekje voor (de CIR of de MAR).

```{warning} Let op!
Met het Engelse woord ‘data’ in de naamgeving van MDR wordt hier dus specifiek data-getallen bedoeld. In de rest van de tekst gebruiken we het woord data om aan te geven dat het nog geïnterpreteerd moet worden. 
```
### Kloksnelheid

Zoals hierboven beschreven, haalt de processor steeds informatie op uit het werkgeheugen (*fetch*), de Control Unit leest deze informatie (*decode*) en de ALU voert de informatie uit (*execute*). Soms moet er ook nog informatie terug in het werkgeheugen geschreven worden (*store*). Dit riedeltje herhaalt zich keer op keer en wordt ook wel de fetch-execute cycle genoemd.
```{figure} assets/image-20231128112817461.png
---
scale: 50%
align: center
---
```
Die cyclus gaat met een vast patroon, maar ook met een vaste frequentie, als het ritme van een drum. Elke drumslag geeft het moment aan dat een nieuwe ronde van de cyclus gestart wordt. Dit noemen we de kloksnelheid en deze wordt uitgedrukt in Hertz. Hertz ken je waarschijnlijk ook van Natuurkunde als de frequentie van een golf. 1 Hertz is gelijk aan 1 instructie per seconde. De kloksnelheid is dus het aantal instructies dat de processor per seconde uit kan voeren uitgedrukt in Hz. 
```{figure} assets/image-20231128112912882.png
---
scale: 20%
align: center
---
```

Die ene instructie per seconde is alleen best wel inefficiënt. Zoals je hierboven in de afbeelding ziet gebeurt elk onderdeeltje van een instructie namelijk ergens anders, werkgeheugen, control unit, of ALU. Nu kunnen alle onderdelen op elkaar blijven wachten, maar dan zitten ze een groot deel van de tijd nutteloos te zijn. Het is veel efficiënter als de instructies zich sneller achter elkaar opvolgen. Terwijl de control unit de ene instructie decodeert, is het werkgeheugen alvast bezig de volgende instructie op te halen en staat deze meteen klaar zodra de control unit klaar is. Dit noemen we pipelining. Op deze manier passen de instructies beter opvolgend aan elkaar, zoals ook in de afbeelding te zien is.

```{figure} assets/image-20231128114345254.png
---
align: center
---
```

:::{note} Let op!
Ook bij pipeling wordt er bij 1 Hz nog steeds maar één instructie per seconde voltooit, ook al zijn er wel meerdere instructies tegelijk in uitvoering. 
:::

### Cores

Toch kan er nog steeds maar een enkele instructie per cyclus voltooid worden, ook met pipelining. Om processors nog sneller te maken worden ze tegenwoordig opgedeeld in cores. Iedere core kan afzonderlijk een instructie uitvoeren. Op die manier is het mogelijk om meerdere instructies tegelijk te voltooien. Een enkele core met een snelheid van 1Hz kan dus 1 instructie per seconde voltooien, maar vier cores met ieder een snelheid van 1Hz kunnen dan vier instructies per seconde voltooien.

```{figure} assets/image-20231128114546091.png
---
align: center
scale: 55%
---
```
Je computer wordt een stuk beter in multitasken met meerdere cores, maar wordt daar niet perse altijd sneller van. Er kunnen meerdere taken parallel aan elkaar uitgevoerd worden, zoals het runnen van verschillende programma’s, maar een specifieke enkele taak gaat dan nog niet vlotter. Denk maar aan de uitspraak: “Een vrouw kan een baby maken in negen maanden, maar negen vrouwen kunnen niet samen in één maand een baby maken.”

### CISC & RISC

De bovenstaande uitleg is alleen niet toepasbaar op iedere soort processor. Er zijn namelijk twee categorieën processoren: CISC en RISC. De uitleg hieronder is zwart of wit.

CISC staat voor Complex Instruction Set Computer. Deze categorie ken je van van laptops en PC’s met een Intel of AMD processor. Kenmerkend van CISC is dat er een hoop verschillende instructies zijn. Die instructies zijn behoorlijk complex (vandaar ook de naam). Complexe instructiesets zorgen ervoor dat programma’s zo compact mogelijk gemaakt kunnen worden. Omdat er veel verschillende instructies mogelijk zijn kan je met weinig achtereenvolgende instructies een heel programma schrijven. Je kan bijvoorbeeld met een enkele instructie twee getallen die in het werkgeheugen staan bij elkaar optellen. Op deze manier kunnen er compactere computerprogramma’s geschreven worden die weinig opslagruimte nodig hebben en dat scheelde tijd en geld. 



Het idee van deze uitgebreide instructiesets klinkt onnodig ingewikkeld, maar deze is ontstaan omdat werkgeheugen vroeger erg duur en traag was. Computerbouwers streefden ernaar zo min mogelijk duur werkgeheugen nodig te hebben. Ook is het ophalen van data uit het werkgeheugen een van de traagste stappen in het proces dus hoe minder vaak er losse instructies uit het werkgeheugen opgehaald moeten worden hoe beter.

Een nadeel hiervan is dat de complexe instructies ingewikkelder zijn om uit te voeren. Zo ingewikkeld zelfs, dat ze soms niet in één klokcyclus uit te voeren zijn. Hoeveel tijd er dan wel nodig is om een instructie uit te voeren varieert. Dit maakt dat pipelining voor een CISC processor niet tot nauwelijks mogelijk is. Als de tijd die nodig is voor een instructie niet altijd in één klokcyclus past weet je niet of de onderdelen op tijd klaar zijn voor hun volgende taakje totdat de gehele instructie afgerond is. Geduldig afwachten tot het einde van de instructie dus.

De complexe instructies van een CISC-processor vereisen ook meer logische poorten. Dus meer transistoren, dus een hoger energieverbruik, dus meer warmte bij het gebruik van zo'n processor. Een voorbeeld van CISC-achtige processoren is de Intel i3/i5/i7/i9-serie. Waarschijnlijk zit een processor van deze 'familie' in je laptop. Deze processoren staan erom bekend niet echt energiezuinig te zijn en veel warmte te produceren. Vandaar alle aandacht voor de koeling.

Bij een CISC architectuur is er dus meer aandacht nodig voor de hardware om compactere software mogelijk te maken.

RISC processoren werken echter met Reduced Instruction Set Computer. Een voorbeeld is de ARM processor in je smartphone, tablet, en slimme apparaten. Sinds de opkomst van deze draagbare slimme apparaten is energiebeheer een stuk belangrijker geworden, en CISC is nou niet bepaald energiezuinig. Door een beperktere instructieset met minder mogelijkheden aan te bieden kunnen programma’s uitgevoerd worden op simpelere processoren die minder energie verbruiken. Maar dit maakt wel dat er meer moeite nodig is om programma’s te schrijven.

Waar bij CISC voor het optellen misschien maar een instructie nodig is, zijn daar bij RISC meer instructies voor nodig. Eerst twee keer een instructie om getallen uit het werkgeheugen op te halen en in een register te zetten, dan een instructie om de twee getallen vanuit het register bij elkaar in de accumulator op te tellen, en dan nog eentje om deze weer terug te schrijven naar het werkgeheugen. Programma's geschreven voor een RISC instructieset zijn daarom ook groter dan hetzelfde programma geschreven in een CISC instructieset.

De compactere hardware vraagt om meer aandacht voor de software. Al die losse instructies maken wel dat deze categorie processoren over het algemeen een stukje trager is. Gelukkig passen bij RISC de instructies wél allemaal binnen een vaste klokcyclus. Daardoor kunnen RISC processoren wel gebruik maken van pipelining. Dat scheelt weer iets om het verschil in snelheid wat te compenseren.

```{figure} assets/image-20231128120121332.png
---
align: center
---
```

### Welke processor
Nu weet je hoe processoren werken, maar hoe kom je er nou achter welke processor er in jouw computer zit? De drie grootste spelers die processoren produceren zijn Intel, AMD, en ARM. Intel en AMD zijn meestal CISC en worden gebruikt voor laptops en PC’s. Een apparaat waar geen Intel of AMD processor in zit heeft waarschijnlijk een ARM processor (RISC). 

Er zijn verschillende manieren om op te zoeken wat voor processor er in een laptop of PC zit. Zo is het meestal wel mogelijk om met het modelnummer van de computer op het internet de specificaties op te zoeken. (Dat modelnummer staat bij laptops trouwens meestal op de onderkant.) Het is ook mogelijk dit in de computer zelf op te zoeken door middel van het programma Systeeminformatie of Taakbeheer. Taakbeheer is daarnaast extra interessant omdat deze ook de informatie over de belasting van de processor geeft. Ook zijn er speciale programma’s te downloaden die de specificaties van je computer kunnen uitlezen zoals CPU-Z of Speccy.

```{figure} assets/image-20231128120950851.png
---
align: center
scale: 50%
---
Windows Taakbeheer
```

:::{exercise}
1a. Zoek met behulp van je taakbeheer op welke processor er in jouw computer zit en schrijf dat op.

1b. Denk je dat dit een krachtige processor is? Waarom wel/niet? Leg je antwoord uit.

1c. Benoem een voordeel en een nadeel van het hebben een krachtigere processor in je computer.

2. Soms kan je je computer sneller maken met overklokken. Wat gebeurt er dan met je processor? Benoem ook een nadeel van overklokken. 

3. Wat is het effect op de processor wanneer je meerdere programma’s tegelijk aan het runnen bent?

4. Processoren worden snel warm. Benoem 3 verschillende technieken die toegepast worden om processoren te koelen. Geeft van iedere techniek aan hoe deze werkt en wat een voordeel en een nadeel is.

5. Er is veel discussie over het einde van de wet van Moore. Zal de wet van Moore voor altijd gelden? Leg je antwoord uit. 

6. Beredeneer waarom een processor aparte opslagplaatsen heeft voor  CIR, MAR en MDR
:::
