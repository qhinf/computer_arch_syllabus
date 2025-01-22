(eindopdracht)=
# Eindopdracht

De eindopdracht voor deze module bestaat uit enkele onderdelen.

1. Een waarheidstabel
2. Een architectuur opdracht
3. Een assembly programma

Deze drie onderdelen moeten in een Markdown bestand terecht komen. Hoe je dit doet zie je onderaan dit bestand in de alinea {ref}`markdown`.

Je kunt in totaal 45 punten halen voor deze opdrachten. 5 punten krijg je kado. Je totale score wordt door 5 gedeeld en dit wordt je cijfer.

## De waarheidstabel (10p)
### Waarheidstabel bij een schakeling (8p)
Maak een waarheidstabel bij de volgende schakeling. Je mag het met de hand nagaan, je mag de schakeling ook nabouwen in Logic.ly en zelf een waarheidstabel vullen. De schakeling heeft 4 inputs (`in1`, `in2`, `in3` en `in4`) en 4 outputs (`out1`, `out2`, `out3` en `out4`). Je tabel heeft dus 16 rijen en 8 kolommen.

````{figure} assets/Schakeling_eindopdracht.png
---
scale: 50%
align: center
---
De schakeling
````
### Functie van deze schakeling (2p)
De schakeling heeft ook een functie in het binair rekenen. Welke functie is dit? Onderbouw je antwoord.

## Architectuuropdracht (8p)
In hoofdstuk {ref}`hoofdstuk-architectuur` heb je gelezen over de *Fetch-Decode-Execute* cycle. Je weet dat een processor door verschillende fases gaat om een instructie uit te voeren.

Aanschouw de volgende locaties, die voor een Von Neumann-architectuur processor van belang zijn:
1. ALU
2. CU
3. Registers
4. RAM
5. Gegevensbus
6. Adresbus

En de volgende acties:
1. Fetch *Haalt een instructie op uit het geheugen via de adresbus*
2. Decode *De CU vertaal de opgehaalde instrcutie in acties*
3. Execute *De ALU voert de bewerking van de instructie uit*
4. Store *Het resultaat van de bwerking wordt opgeslagen in een register of RAM*
5. Load *Gegevens worden ui het geheugen geladen naar een register*
6. Branch *De CU bepaalt of de uitvoering naar een andere instructie moet springen op basis van een voorwaarde*

Je gaat bij een gegeven instructie een lijst van plaatsen en acties maken, die de processor doorloopt om deze instructie uit te voeren.

### Voorbeeld 
Maak een lijst van plaatsen en acties die nodig zijn om de instructie: *Tel de waarde van register R1 en register R2 op en sla het resultaat op in register R3.* uit te voeren.

- Geheugen
- Fetch
- Adresbus
- CU
- Decode
- Register
- ALU
- Execute
- Register
- Store

### Instructie 1 (4p)

*Laad een getal uit het geheugen (0x002A) in register R1 en vermenigvuldig het met de waarde van R2.*

### Instructie 2 (4p)
*Vergelijk de waarde van register R1 met R2. Als R1 groter is, spring naar instructie op adres 0x0030*

## Assembly programma (27p)
Schrijf een programma dat controleert of een ingevoerd getal een priemgetal is. Het programma kan met herhaalde delingen controleren of een getal deelbaar is door enig ander getal dan 1 en zichzelf. 

Het programma schrijf je zo, dat het werkt in de RISC-simulator van Higginson. Deze simulator heb vaker gebruikt in deze module.

De code voor je assembly-programma zet je ook in je Markdown-bestand. Hoe? Zie {ref}`markdown`.

## Beoordeling
Je assembly programma wordt beoordeeld op de volgende criteria. Je kunt een aantal punten behalen per criterium.

| Criterium | Uitleg | Aantal punten |
|-|-|-|
| Functionaliteit en betrouwbaarheid in de simulator | Je programma moet werken zonder fouten in de simulator en precies doen wat er van je gevraagd wordt. Dat betekent dat het het juiste resultaat moet geven voor verschillende soorten invoer, zelfs als die invoer onverwacht is. Test je programma daarom goed, zodat je zeker weet dat het ook goed werkt bij bijzondere gevallen. | 5 |
| Nauwkeurige besturingslogica | Zorg ervoor dat de beslissingen in je code (zoals `if-then-else` en `loops`) goed zijn opgezet en logisch werken. Gebruik bijvoorbeeld de juiste instructies om je code te laten springen naar het juiste deel, zonder onnodige stappen. Je code moet vloeiend doorlopen zonder fouten, zoals onnodige sprongen of vastlopen. | 5 |
| Correct gebruik van basisbewerkingen | Gebruik de basishandelingen (zoals `MOV`, `ADD` of `SUB`) op de juiste manier. Dit betekent dat je weet hoe je deze instructies moet toepassen in je code om dingen goed te laten werken. Als de basisbewerkingen niet goed zijn gebruikt, werkt je programma waarschijnlijk niet zoals het hoort. | 5|
| Efficiënt gebruik van geheugen en registers | Probeer zo slim mogelijk om te gaan met de opslagruimte in je code. Bewaar alleen dingen, die echt nodig zijn en gebruik registers om waarden tijdelijk op te slaan in plaats van steeds naar het geheugen te schrijven. Je programma wordt hierdoor sneller en je leert efficiént werken met geheugenruimte. | 5| 
| Duidelijkheid en leesbaarheid van de code | Schrijf je code zo dat anderen (en jijzelf later) makkelijk kunnen begrijpen wat je hebt gedaan. Zorg dat er uitleg bij de lastige delen staat, gebruik duidelijke namen voor variabelen en labels, en houd je stijl consistent. Dit helpt iedereen om je code snel te lezen en te begrijpen. | 2|


### Duidelijkheid en leesbaarheid van je code
- Zorg dat je regelmatig uitleg toevoegt bij je code. Denk aan commentaarregels als korte notities die anderen (en jijzelf) helpen begrijpen wat er gebeurt. Voor elk blok code (zoals een herhaling of een keuze) voeg je tenminste één regel commentaar toe.
- Gebruik beschrijvende namen voor je labels en variabelen, zodat iemand die je code leest meteen snapt wat een bepaalde waarde betekent. Bijvoorbeeld, kies voor een naam als `totaalSom` in plaats van `temp1`. Dit helpt om snel te zien waar alles voor dient.
- Houd de stukken code die geen uitleg hebben, kort. Je code moet niet langer dan ongeveer 10 regels zijn zonder uitleg, zodat de lezer gemakkelijk kan volgen wat er gebeurt.
- Zorg dat de uitleg bij je code zinvol is. De uitleg moet niet gewoon herhalen wat de code doet, maar uitleggen waarom dat nodig is. Bijvoorbeeld: in plaats van `ADD R1, R2, R3 // telt R1 en R2 op` kun je schrijven `// Bereken de totale punten voor de speler`.

Zorg ervoor dat je assembly-code kopieerbaar en plakbaar is voor de RISC-simulator. Test dit zelf even :) Je zal de eerste niet zijn, die ongeteste code inlevert en vervolgens hierdoor een laag eindresultaat voor een programmeermodule heeft.

Een generatieve AI als ChatGPT, Google Gemini of Claude AI is wel in staat om assembly code te genereren. Mijn ervaring tot nu toe, is dat tot nu toe geen enkele van die door AI gegenereerde assembly programma's het goed doen en de eindstreep van deze module kunnen halen.

De **deadlines** voor deze module zijn:

- Inleveren van de eindopdracht: {{ deadline }}
- Aanvragen van uitstel: {{ deadline_uitstel_aanvragen }}
  - Uitgestelde deadline: {{ deadline_uitstel }}

(markdown)=
## Markdown
Maak je uitwerkingen in een tekst-editor zoals Notepad of Visual Studio Code. **Niet in Word**. Gbruik [Markdown](https://www.markdownguide.org/cheat-sheet/) om structuur te geven aan je document. Er is een voorbeeld van hoe je uitwerking eruit moet zien. Je kunt dit bestand [hier](assets/referentie_uitwerking.txt) vinden. Open het in Visual Studio Code en je kunt er meteen mee aan de slag. Het kan zijn dat Visual Studio Code je allerlei vragen stelt over plug-ins voor Markdown. Advies: installeer deze. Voor meer info over het bewerken van Markdown in Visual Studio Code kun je [hier](https://code.visualstudio.com/docs/languages/markdown) vinden.

### Waarheidstabel in Markdown

Een waarheidstabel in Markdown kun je als volgt maken:

Zoals het uiteindelijk eruit ziet:
| A | B | A and B |
|---|---|---------|
| 0 | 0 |    0    |
| 0 | 1 |    0    |
| 1 | 0 |    0    |
| 1 | 1 |    1    |

En zo ziet de Markdown broncode eruit:

```markdown
| A | B | A and B |
|---|---|---------|
| 0 | 0 |    0    |
| 0 | 1 |    0    |
| 1 | 0 |    0    |
| 1 | 1 |    1    |
```

### Assembly code in Markdown
Om ervoor te zorgen dat je assembly code er zo mooi uitziet als deze

```
LDR R1, score
CMP R1, #100
BEQ if_score100    // if score equals 100
BRA skip_score100  // if not: skip conditional code
```

gebruik je de volgende code

````
```
LDR R1, score
CMP R1, #100
BEQ if_score100    // if score equals 100
BRA skip_score100  // if not: skip conditional code
```
````

Let op: wanneer je je toetsenbord instellingen in Windows op NL hebt staan, wil het typen van die backticks ` wel eens vervelend zijn. Dit kun je oplossen door je toetsenbord op INT of US te zetten.