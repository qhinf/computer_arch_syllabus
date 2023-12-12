# Machinetaal

We weten nu hoe we met complexe schakelingen bijvoorbeeld de binaire optelling kunnen maken. Dit is slechts één voorbeeld van wat complexe schakelingen kunnen maken. Een processor is zelf een complexe schakeling, die vele malen ingewikkelder is dan de schakelingen waar wij mee bezig zijn geweest. We weten nu ook wat de functie van de processor is in de von Neumann architectuur en de Harvard architectuur. In dit hoofdstuk ga je leren hoe je de code schrijft, die ervoor zorgt dat de processor ook echt iets gaat doen voor je. We beginnen met assembly en gaat later door op machinetaal. Machinetaal is op het laagste niveau. Dichter bij de processor kun je eigenlijk niet komen. 

## Assembly

Voordat we verder gaan kijken naar assembly, moet je goed begrijpen dat de instructies die we gaan schrijven niet bedoeld zijn voor de ALU, maar voor de besturingseenheid/control unit (CU) van de processor. De CU ontvangt de instructies en geeft die vervolgens door aan de ALU. Daarbij moet de besturingseenheid aangeven welke operator en operand(en) de ALU moet gebruiken. De operator is gebaseerd op de *mnemonic*, en de operand(en) op de argument(en). Een mnemonic is letterlijk vertaald een geheugensteuntje. In assembly beschouwen we deze geheugensteuntjes als super simpele instructies, die de CU kan begrijpen. In dit hoofdstuk zul je gaan leren welke instructies dit zijn en hoe je ze kan gebruiken om zelf een programma in assembly te schrijven.

```{figure} assets/image-20231212111728905.png
---
align: center
scale: 80%
---
```
### Mnemonics

Een mnemonic voor assemby is meestal een twee of drieletterige combinatie van letters, die een afkorting is van de taak die de betreffende mnemonic uitvoert weergeeft. Voorbeelden van mnemonics zijn `ADD`, `BE`, `HLT`, `MUL`,`JMP` en `MOV`. De mnemonic `ADD` staat voor een optel-instructie (addition), de mnemonic `BE` voor Branch-if-equal (maak een sprong wanneer iets gelijk aan elkaar is), de mnemonic `HLT` voor het stoppen van de processor. Deze instructies worden door de CU uit het geheugen opgehaald. De CU haalt vervolgens de argumenten/operanden voor de instructie op en geeft deze door aan de ALU. Bij een `ADD`-operatie moet je natuurlijk weten *wat* je bij elkaar op wil tellen. Instructies waar geen operanden voor nodig zijn, gaan ook niet langs de ALU. Een voorbeeld van zo'n argumentloze instructie is  `HLT`: halt. Als de processor die instructie krijgt, stopt die met het uitvoeren van opdrachten. Daarvoor hoeft de ALU niets te doen. Daarom heeft die instructie ook helemaal geen argumenten nodig. 

### Argumenten

De besturingseenheid bepaalt de operanden voor de ALU op basis van de argumenten die in een instructie staan. Er zijn verschillende manieren waarop dat gebeurt. Een argument kan namelijk van alles zijn, bijvoorbeeld: 

- een vaste waarde,
- een register,
- een geheugenadres.

We zullen nu eerst focussen op de eerste twee opties: vaste waarden en registers. Dat doen we aan de hand van de onderstaande rekensom. De opdracht is om het getal 22 op te tellen bij de waarde van register 4. Het antwoord wordt dus ook weer opgeslagen in register 4.

> R4 = R4 + 22

De opdracht is dat er opgeteld moet worden. Het mnemonic dat daarbij hoort, is `ADD`. Bij die instructie kan je twee argumenten opgeven, die de volgende vragen beantwoorden:
1. Van welk register moet de waarde verhoogd worden? → register `R4`
2. Welk getal moet erbij opgeteld worden? → getal `22`

Dit schrijf je als volgt op in assembly:
```{assembly}
ADD R4, #22
```

Deze code geeft antwoord op vier vragen:

1. Wat moet er gebeuren (operator)? → optellen (`ADD`)
2. Wat is de eerste operand? → de waarde uit `R4`
3. Wat is de tweede operand? → de waarde `22`
4. Waar moet het antwoord opgeslagen worden? → in `R4`

De eerste drie vragen zijn van belang voor de ALU. De laatste vraag is iets waar de ALU zich niet mee bezig houdt, maar wat wel moet gebeuren: waar gaat het antwoord heen? De ALU stuurt het antwoord naar de besturingseenheid. Die slaat het op in een register.

De assembly code, die we nu schrijven, kun je ook online uitproberen in de [RISC-Simulator](https://peterhigginson.co.uk/RISC/) van Peter Higginson. In de [documentatie](http://www.peterhigginson.co.uk/RISC/instruction_set.pdf)[ van de RISC-simulator](http://www.peterhigginson.co.uk/RISC/instruction_set.pdf) kan je vinden hoe je instructies op kan schrijven. Rechts hiervan staat een aantal basis-instructies waar je eerst mee gaat oefenen. Later zal je nog meer instructies leren kennen. De namen van de argumenten geven aan wat de betekenis ervan is. *Rsd* staat hierin bijvoorbeeld voor een register, en *imm8* voor een getal. Verderop wordt hier meer over uitgelegd. We nemen nu eerst de mnemonics door.

| mnemonic            | betekenis                            |
| ------------------- | ------------------------------------ |
| `HLT`               | halt                                 |
| `MOD` *Rsd*, *imm8* | modulo nemen ( *Rsd* = Rsd % *imm8*) |
| `ADD` *Rsd*, *imm8* | optellen ( *Rsd* = *Rsd* + *imm8*)   |
| `SUB` *Rsd*, *imm8* | aftrekken ( *Rsd* = *Rsd* - *imm8*)    |
| `CMP` *Rs*, *imm8*      | vergelijken (*Rs* == *imm8*)             |
| `MOV` *Rd*, *imm8*      | waarde instellen (*Rd* = *imm8*)         |
| `AND` *Rsd*, *imm8*     | logische EN (*Rsd* = *Rsd* & *imm8*)       |
| `ORR` *Rsd*, *imm8*     | logische OF (*Rsd* = *Rsd*               |
| `MUL` *Rsd*, *imm8*     | vermenigvuldigen (*Rsd* = *Rsd* * *imm8*)  |

- De eerste mnemonic, `HLT`, is een bijzondere instructie: ***halt***. Als de processor deze instructie krijgt, stopt die met het uitvoeren van instructies.

- Het berekenen van *modulo* is: bepalen wat de rest is na een deelsom. Je haalt de tweede operand zo vaak mogelijk van de eerste operand af, totdat je een getal over houdt dat kleiner is dan de tweede operand.

  Dit doe je bijvoorbeeld zelf ook als je op de wekker kijkt: als daar 21:00 staat, bedenk je dat het 21 - 12, dus 9 uur is. Dit kan je ook opschrijven als `21 % 12 == 9`. Op dezelfde manier geldt ook`33 % 12 == 9`, want `33 / 12` is `2 rest 9`.

- *Optellen* en *aftrekken* heeft geen verduidelijking nodig.

- *Vergelijken* wordt verderop pas behandeld, omdat dat pas nodig is in combinatie met instructies voor programma-besturing (control). Hierbij wordt het resultaat niet opgeslagen in een register.

- Het *instellen van een waarde* houdt in dat het opgegeven getal (imm8) in een register wordt geladen.

- De *logische EN* en *OF* worden ook wel bitwise AND en OR genoemd: hierbij worden de bits van de twee getallen onder elkaar gezet, en vervolgens wordt ieder paar van bits, dat onder elkaar staat, door een EN- of OF-poort verwerkt. `6 & 5` (`0110 & 0101`) wordt dan `4` (`0100`) en `6 | 5` is `7` (`0110 | 0101 == 0111`).

- *Vermenigvuldigen* spreekt voor zich.

We zullen nu kijken naar de twee argumenten die achter de mnemonics staan: een register en een getal.

### Registers als argument

Registers kunnen op verschillende manieren gebruikt worden: lezen en schrijven. Als een register gelezen moet worden, wordt er nog preciezer aangegeven wat het doel is om een register te lezen. De letters achter de `R` betekenen het volgende:

| letter | betekenis   | uitleg                                                       |
| ------ | ----------- | ------------------------------------------------------------ |
| s      | source      | De waarde uit het register wordt gelezen als eerste operand. |
| d      | destination | Het antwoord op de berekening wordt in het register opgeslagen. |
| b      | base        | De waarde uit het register wordt gelezen als tweede operand. |
| a      | address     | De waarde uit het register wordt gelezen als geheugenadres.  |

Een argument kan ook aangeven dat een register voor meerdere doelen gebruikt worden. Dat zie je in de tabel met basis-instructies ook: daar staat als eerste argument vaak `Rsd`. Het register dat je op die plaats invult, wordt dus zowel gebruikt om de waarde voor de eerste operand uit te lezen, alsook om het antwoord op de berekening in op te slaan.

### Getallen als argument

Het tweede argument in de basis-instructies is een getal van 8 bits. De naam imm8 betekent: 8-bit immediate value. De term **immediate** geeft aan dat hier een directe waarde van een operand in de instructie staat, en geen geheugenadres of register-nummer. Het is goed om je ervan bewust te zijn dat de maximale waarde van een 8-bits getal 255 is. Grotere getallen kan je dus niet opgeven als argument: dat levert een foutmelding op in de simulator.

### Code uitleggen

Om coderegels uit te leggen, kan je **commentaar** toevoegen. Dat doe je door een dubbele schuine streep (slash) neer te zetten: `//`. Alles wat je daarachter typt, wordt niet uitgevoerd door de computer. Nog meer dan bij een programmeertaal als Python of Java(script) is het belangrijk om commentaar bij je code te zetten. Soms heb je echt geen idee *waarom* je een bepaalde instructie hebt geschreven. De cryptische vorm van assembly helpt je ook niet echt in het snel begrijpen waarom iets geschreven is.

### Assembly invoeren in de RISC-simulator

```{figure} assets/image-20231212120442724.png
---
align: right
scale: 40%
---
```

Je kan je antwoorden van de vorige oefening laten uitvoeren door de [RISC-simulator](https://peterhigginson.co.uk/RISC/) op https://peterhigginson.co.uk/RISC. Daar zet je de code in het veld links boven in je scherm. Klik vervolgens op *submit*.

```{figure} assets/image-20231212120539885.png
---
align: right
scale: 40%
---
```
Je ziet nu dat de regels genummerd worden. Het nummer voor de regel geeft aan op welk geheugenadres de instructie in machinetaal is gezet.

```{figure} assets/image-20231212120706784.png
---
align: right
scale: 40%
---
```

Vervolgens kan je op `RUN` klikken om het programma uit te voeren. Dan zie je dat er een knop `STOP` voor in de plek komt, en de knoppen `<<` en `>>` om de snelheid aan te passen waarmee de code wordt uitgevoerd.

Als je niet op `RUN`, maar op `STEP` klikt, kan je stap voor stap door het programma gaan.

Als je een programma opnieuw wilt uitvoeren, moet je eerst op `RESET` klikken, en dan weer op `RUN`.

Nu komen we terug bij de oefening waar dit hoofdstuk mee begon: berekeningen ontleden. Nu heb je genoeg kennis om deze berekeningen te vertalen naar assembly!

Om rekensommen op te schrijven in assembly, volg je drie stappen:

1. Berekening ontleden
2. Registers kiezen
3. Instructies opschrijven

Volg dat stappenplan in de volgende oefening.

:::{exercise} Assembly schrijven
Schrijf van alle onderstaande sommen op welke operaties je moet uitvoeren om de som uit te rekenen. Pak je uitwerking van oefening 1 erbij, daar heb je de eerste stap al gedaan: je hebt daar de operator en de operanden bepaald.

De eerste opdracht is voorgedaan, volg dat voorbeeld.

1. 4 + 2 * 10

```{admonition} Uitwerking
:class: dropdown

*Berekening ontleden (zie oefening 1)*

operator 1: \*, operanden: 2 en 10, antwoord: 20

operator 2: +, operanden: 4 en 20, antwoord: 24

*Registers kiezen*

register instellen: R1 = 2

instructie: `MOV R1, #2`

*Instructies opschrijven*

instructie 1: `MUL R1, #10 // 2 * 10`

instructie 2: `ADD R1, #4 // 20 + 4`
```
2. 16 + 12 * 2
3. (16 + 12) % 3
4. 2 * 3 * 5
5. 14 * 2 * 3 - 15
6. 4 ^ 2 * 6 % 2
:::{note} Let op: 
^ betekent “tot de macht”, maar daarvoor is geen mnemonic beschikbaar. Welke operator kan je nu gebruiken in plaats van ^?
:::

:::

## Machinetaal

## Compileren of interpreteren

