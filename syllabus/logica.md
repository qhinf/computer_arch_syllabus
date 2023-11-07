# Logica voor computers

## En zo begint het

### Tekens & betekenis

Cijfers, letters, gebaren, signalen, afbeeldingen, pictogrammen, iconen. Deze gebruik je om een boodschap over te brengen of gegevens vast te leggen. Kortom: communicatie. Het is nodig om hierover afspraken te maken. De  betekenis van een afbeelding of een combinatie van een aantal tekens moet worden vastgelegd.  Zo’n afspraak noem je een code.

[co·de *(de; m;* meervoud: *codes)*

: **1** verzameling van overeengekomen woorden en tekens met een afgesproken betekenis

: **2** regels waaraan beoefenaars van allerlei beroepen zich vrijwillig onderwerpen: *beroepscode, gedragscode, reclamecode*

Zowel betekenis 1 als 2 gaan over een afspraak.

 *[www.vandale.nl](http://www.vandale.nl/) (27 juni 2022)*

### Syntax & semantiek

Voor het Nederlandse schrift worden 26 letters gebruikt in twee vormen: kleine letters en hoofdletters. Verder zijn er een aantal interpunctietekens. De ontelbare combinaties hiervan worden gemaakt met behulp van syntaxregels. Je hebt bij de vakken waar je een taal leert (dus ook Nederlands) hier al uitgebreid mee te maken gehad. Syntax of in normaal Nederlands *zinsbouw* is een onderdeel van de *grammatica* van een taal. Met de syntax kun je beschrijven welke zinnen wel en welke zinnen geen onderdeel van een taal vormen. 

Wanneer je het over de *semantiek* van een taal hebt, dan gaat het over de betekenis van een zin. Wat wordt er bedoeld met een zin. Met een mensentaal komt het regelmatig voor dat een syntactisch correcte zin meerdere betekenissen heeft. Kijk maar even naar de volgende zin:

> De man slaat de hond met de stok

De zin is grammaticaal helemaal correct, maar we kunnen nu niet met zekerheid zeggen of het nu de man een hond slaat, die een stok een zijn bek heeft *óf* dat de man gebruikt maakt van een stok om een hond te slaan. Hoe dan ook, het is een best zielig voorbeeld en het laat wel goed zien dat de betekenis van een correcte gevormde zin niet altijd duidelijk is.

Een computer is over het algemeen erg goed in het bepalen of iets syntactisch correct is. Je herkent het vast wel van de meldingen, die je van Python of een andere programmeertaal krijgt: `Syntax error: ...`. Dan ben je vast ergens iets als een haakje vergeten. De 'betekenis' van een programma kun je meestal alleen achterhalen door het programma uit te voeren (al dan niet in je hoofd). Dit maakt het vinden van niet-syntactische fouten in je programma ook zo moeilijk.

Om ervoor te zorgen dat een computer met tekens kan werken, zullen we hier dus afspraken over moeten maken. *Welke tekens en wat betekenen ze*.

### Geschiedenis 

Bij moderne computers wordt elektriciteit gebruikt. Hoe simpel zou je de daarin gebruikte code kunnen maken? Als volgt: er is géén of wél stroom (eventueel spanning) aanwezig, waarbij we ‘geen’ aangeven met ‘0’ en ‘wel’ met ‘1’. Een code van twee mogelijkheden, de binaire code. Dit basiselement van de code wordt ‘bit’ genoemd. Een serie van 8 bits (een byte, by-eight) zorgt voor 256 verschillende combinaties. Je hebt hier in hoofdstuk {doc}`binair_rekenen` al mee leren rekenen. Nu de betekenis nog! De afspraken over de betekenis ervan moeten worden vastgelegd. Zo kun je met een binaire code rekenen, maar je kunt ook de computer via een instructie laten weten: nu volgt er een code, waarvan de betekenis is vastgelegd is in de ASCII-tabel.

![ASCII tabel](assets/ASCII_table.png)

::: {exercise}

Schrijf met behulp van de bovenstaande ASCII-tabel de binaire tekenreeks van je voornaam. Tip: ga via de hexadecimale notatie naar binair.

Wat is de beperking van de ASCII-tabel?

Wat is de opvolger van ASCII?

Uit hoeveel bits bestaat een teken van deze code?

:::

## Inleiding logische schakelingen

In de processor van een computer wordt door de ALU, de Arithmetic Logic Unit (rekenkundig-logische eenheid, rekencentrum) rekenkundige en logische bewerkingen uitgevoerd. Dit is een complex proces van elektronica op microniveau. Wat ooit begon met onderdelen zoals forse schakelaars, relais, elektronenbuizen en transistors is nu zo geminiaturiseerd, dat een moderne processor miljarden schakelingen bevat op een oppervlakte niet veel groter dan je duimnagel. Dit in tegenstelling tot de eerste computers, gemaakt met elektronenbuizen. Deze coomputers waren zo groot dat je een flinke zaal nodig had om ze te kunnen 'hebben'. En daar bleef het niet bij: ze vraten zoveel stroom en genereerde zoveel warmte dat ze een aparte airconditioning nodig hadden. Door deze schakelingen loopt een elektronenstroom. Door de miniaturisatie zijn de afstanden die de elektronen af moeten leggen kleiner geworden.  Een kleinere afstand betekent sneller. Een duizend keer kleinere afstand wordt immers duizend keer vlugger afgelegd. Of het nu een primitieve kamerbrede computer is met airconditioning of een hypermoderne processor ter grootte van je duimnagel, het principe van de schakelingen, waar stroom doorheen gaat blijft hetzelfde.

Slimme constructies van schakelingen maken het mogelijke dat een computer kan rekenen. Aan de invoerkant van zo’n constructie wordt een binair signaal aangeboden en aan de uitvoerkant komt het resultaat, ook weer een binair signaal.

Een andere toepassing is die van opslag. Het is vaak noodzakelijk dat resultaten (of tussenresultaten) in het werkgeheugen van een computer worden opgeslagen om later weer te worden gebruikt.

Om resultaten (uitvoer) verder te verwerken is het nodig dat de signalen, die deze resultaten bevatten, niet alleen kunnen worden opgeslagen, maar als het nodig is ook naar de juiste plek kunnen worden gestuurd. Dit doorsturen wordt mogelijk gemaakt met, je raadt het al, schakelingen.

Verderop in dit hoofdstuk ga je zelf aan de slag met het ‘bouwen’ van constructies voor rekenen, opslag en doorschakelen (doorsturen).

### Waarom logische schakelingen?

Waarom zou je logische schakelingen gebruiken in een computer? Logische schakelingen kun je sturen met elektrische signalen. Ze zijn handig, omdat ze maar twee verschillende uitvoermogelijkheden hebben, die je kunt uitleggen als één en nul of waar en niet waar. Hetzelfde geldt voor de invoersignalen van deze schakelingen. De schakelingen worden daarmee bouwstenen waarmee je kunt ‘sturen’.

Dus, als je aan het open of dicht zijn van schakelaars een betekenis geeft, heb je de basis gelegd voor het werken met code. Dicht of open, wel of geen stroom, wel of geen signaal, waar of niet waar, één of nul. De bit, als basiseenheid voor de code.

Altijd énen of nullen? Dat is nog maar de vraag. De computers van nu werken hier wel mee. De Von Neuman-architectuur hangt hiermee samen. Deze wordt in een volgend hoofdstuk behandeld. Er zijn in het verleden ook experimentele computers geweest, die met een driewaardige logica werkten. Dit was slechts een academische oefening, [ternaire computers](https://en.wikipedia.org/wiki/Ternary_computer) zijn nooit uit het laboratorium gekomen. Een ontwikkeling, die mogelijk in de toekomst bruikbaar wordt en ongekende mogelijkheden beloofd, is, die van de quantumcomputer.

Lees hiervoor meer op https://www.universiteitleiden.nl/wetenschapsdossiers/de-quantumcomputer

::: {exercise}

Wat is booleaanse logica?

Maakt de quantumcomputer gebruik van bits? Leg uit!

Wat is de belofte die de quantumcomputer hoopt waar te maken?

Wat zou je, behalve stroom, nog meer kunnen gebruiken om 1-en en 0-en fysiek te representeren?

:::

Slimme constructies van schakelingen maken het mogelijke dat een computer kan rekenen. Aan de invoerkant van zo’n constructie wordt een binair signaal aangeboden en aan de uitvoerkant komt het resultaat.

Een andere toepassing is die van opslag. Het is vaak noodzakelijk dat resultaten (of tussenresultaten) in het werkgeheugen van een computer worden opgeslagen om later weer te worden gebruikt.

Om resultaten (uitvoer) verder te verwerken is het nodig dat de signalen, die deze resultaten bevatten, niet alleen kunnen worden opgeslagen, maar als het nodig is ook naar de juiste plek kunnen worden gestuurd. Dit doorsturen wordt mogelijk gemaakt met, je raadt het al, schakelingen.

Verderop in dit hoofdstuk ga je zelf aan de slag met het ‘bouwen’ van constructies voor rekenen, opslag en doorschakelen (doorsturen).
