(hoofdstuk-instructies)=
# Machinetaal - Instructies voor de processor

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
```{code-block} asm
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

### Assembly uitvoeren in de RISC-simulator
Wanneer je je assembly-code uitvoert in de RISC-simulator (door op `RUN` te klikken), zie je een hoop beweging op je scherm. Je zult zien dat de Fetch-Decode-Execute cyclus uitgevoerd wordt.
1. Het register PC wordt uitgelezen. Hier bevindt zich de locatie van de instructie, die opgehaald moet worden.
2. Met de informatie uit dit register wordt de instructie opgehaald *en* met de incrementer het PC register met 1 opgehoogd, want daar staat vast wel de volgende instructie.
3. De opgehaalde instructie wordt aan de control unit gegeven en die besluit welke deel van de processor de instructie gaat uitvoeren. Wanneer het bijvoorbeeld een `ADD`-instructie is, dan wordt de ALU aan de slag gezet om het resultaat te berekenen en wordt het resultaat weggeschreven in een register. Wanneer het nodig is om voor de instructie nog meer data uit het geheugen op te halen, wordt dat in deze fase ook uitgevoerd.

Mocht de animatie te snel gaan, dan kun je met de knop '<<' de snelheid lager instellen.

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

## Machinetaal
In een computer moet alles in binaire getallen (digitaal) worden genoteerd. Ook instructies zijn dus reeksen van enen en nullen. Dat noemen we **machinetaal**. Hoe je op die manier een instructie opschrijft, is vastgelegd in de **instructieset** van een processor. Een onderdeel daarvan beschrijft bijvoorbeeld hoe je rekensommen kan noteren voor een processor. In deze paragraaf ontdek je hoe dat werkt met behulp van de [RISC simulator](https://peterhigginson.co.uk/RISC).

Een programma dat geschreven is in assembly (afgekort ASM), moet dus eerst vertaald worden naar machinetaal voordat het uitgevoerd kan worden. Daarvoor wordt een **assembler** gebruikt. Dat is een programma dat ASM vertaalt naar machinetaal.

In een RISC-architectuur heeft iedere instructie in machinetaal een vaste lengte. Bij de RISC-simulator is dat even groot als een woord. Die werkt met woorden van 16 bits, waardoor de instructies ook allemaal zo lang zijn. Een aantal van die bits is nodig om aan te geven wat er moet gebeuren, bijvoorbeeld optellen. Daardoor kan je in geen enkele instructie een getal van 16 bits gebruiken. Als je toch 16-bits getallen wilt gebruiken als operanden, dan moet je die getallen eerst in het geheugen of in de registers plaatsen.

### Instructieformaten

We gaan nu verder kijken hoe je een berekening kan opschrijven in machinetaal, dus in enen en nullen. Om alle benodigde gegevens in een instructie op te schrijven, wordt de instructie in velden opgedeeld. Een **veld** is een groepje van bits die samen een betekenis hebben. Een **instructieformaat** geeft aan hoe groot elk veld is. Een voorbeeld daarvan zie je in de onderstaande afbeelding, namelijk instructieformaat A. Dat is maar één van de instructieformaten uit de instructieset van de RISC-simulator. Verderop zullen we ook andere formaten ontdekken.

```{figure} assets/image-20231212154452317.png
---
align: center
---
Een instructieformat van 16 bits
```

### Operation codes

In de afbeelding hierboven zie je dat de drie velden allemaal een naam hebben. We zullen ze van links naar rechts doornemen, zodat je begrijpt wat de namen betekenen. Het eerste veld (op) is het **opcode** veld. Dat is een afkorting van **operation code**. Die geeft aan wat er moet gebeuren. Een mnemonic wordt door de assembler dus vertaald in een opcode. De RISC simulator heeft bijvoorbeeld de volgende binaire codes voor `ADD` en `SUB`: 00010 is `ADD` 00011 is `SUB`. In de tabel hieronder zie je alle 8 de opcodes die bij formaat A horen.

| op-veld | betekenis                       |
| ------- | ------------------------------- |
| 00000   | halt                            |
| 00001   | modulo nemen (Rsd = Rsd % imm8) |
| 00010   | optellen (Rsd = Rsd + imm8)     |
| 00011   | aftrekken (Rsd = Rsd - imm8)    |
| 00100   | vergelijken (Rsd == imm8)       |
| 00101   | waarde instellen (Rsd = imm8)   |
| 00110   | logische EN (Rsd = Rsd & imm8)  |
| 00111   | logische OF (Rsd = Rsd          |

### Argumenten

Na de eerste 5 bits voor de opcode, zijn er in formaat A nog 11 bits beschikbaar voor de argumenten. Die worden verdeeld in twee velden: een veld van 3 bits met de naam Rsd en een veld van 8 bits met de naam imm8. De namen van de velden geven aan wat de betekenis van het argument is, zoals uitgelegd bij assembly.

Laten we nu een voorbeeld bekijken. Bij het optellen volgens formaat A, wordt er een getal van 8 bits (imm8) opgeteld bij de waarde van een bepaald register (Rsd). Het resultaat van de berekening word vervolgens weer opgeslagen in hetzelfde register. Als je bijvoorbeeld het getal 22 bij register 4 op wilt tellen, dan kan dat dus met de volgende instructie: 0001010000010110. In de afbeelding hieronder zie je welke waarde ieder veld dan heeft.

```{figure} assets/image-20231212155513333.png
---
align: center
---
Een instructie
```
In deze instructie zijn de decimale waarden van de velden:
- op = 2,
- Rsd = 4,
- imm8 = 22

Dus deze instructie staat voor: `ADD R4, #22` oftewel R4 = R4 + 22.

### Machinetaal invoeren in de RISC-simulator

```{figure} assets/image-20231219115603223.png
---
align: right
scale: 40%
---
```

Je kan de instructies die je in de vorige oefening hebt geschreven ook uit laten voeren in de [RISC-simulator](https://peterhigginson.co.uk/RISC/). Ga daarvoor naar https://peterhigginson.co.uk/RISC. Onderaan kan je bij OPTIONS op binary klikken (zie rechts), zodat je de binaire instructies in het geheugen kan zetten.

Kopieer vervolgens je binaire codes in de eerste twee words in het geheugen (Main Memory) zoals je hieronder ziet. In dit voorbeeld zijn dat 0010100100000111 en 0000100100000011.

**Let op:** het bewerken van het geheugen kan niet als je in de linker kolom de code aan het bewerken bent (Assembly Language). Klik dan eerst op `Submit` of `Cancel`.

```{figure} assets/image-20231222233537270.png
---
align: center
scale: 50%
---
```
Klik vervolgens op `RUN`.

```{figure} assets/image-20231222233720396.png
---
align: center
scale: 50%
```
Nu zie je hoe de simulator de instructie ophaalt (rode getallen) en vervolgens de juiste waarden (operanden) naar de ALU stuurt (blauwe getallen).

Om meer inzicht te krijgen in de betekenis van machinetaal, ga je in de volgende opdracht de betekenis bepalen van binaire instructies.

`````{exercise} Machinetaal decoderen
Bepaal van alle onderstaande instructies wat de betekenis is en wat het resultaat is. De eerste opdracht is voorgedaan, volg dat voorbeeld. De waarden van registers zijn:

```{list-table}
:header-rows: 1
:width: 80%
:align: left
* - Register
  - Waarde (decimaal)
  - Waarde (binaire byte)
* - R1
  - 16
  - `00010000`
* - R2
  - 11
  - `00001011`
* - R3
  - 20
  - ...
* - R4
  - 7
  - ...
* - R5
  - 29
  - ...
* - R6
  - 49
  - ...
```
a. `0011010100101010`
````{admonition} Uitwerking
:class: dropdown

```{list-table}
:header-rows: 1
:width: 80%
:align: center
* - 
  - instructiedeel
  - 
* - opcode
  - `00110`
  - `&`
* - Rsd
  - `101`
  - 5
* - imm8
  - `00101010`
  - 170
* - betekenis
  - 
  - R5 = R5 & 170
* - resultaat
  - 
  - $10_d$, want `00011011 & 1010101010 == 00001010` 
```
````

b. `0001111000101010`

c. `0000101000000100`

d. `0001010000011000`

e. `0011101100001110`

f. `0000100100000101`
`````

Om een waarde van een register in te stellen, zonder een berekening uit te voeren, gebruik je de opcode 00101 (zie de tabel met opcodes voor formaat A). Om de waarde 16 in register 1 te laden, gebruik je dus de volgende instructie:

```{figure} assets/image-20231223002355026.png
---
scale: 80%
align: center
---
```

Nu volgt een opdracht waarin je zelf de binaire instructie moet bepalen voor een gegeven som. Zo ontdek je nog meer hoe instructies voor de computer werken.

````{exercise} Machinetaal schrijven
Schrijf alle onderstaande sommen op in machinetaal. Bepaal hiervoor wat de operator en de operanden zijn. Kies voor één van beide operanden een register. Stel de waarde van het register in, en voer daarna de berekening uit. Je hebt dus voor iedere som 2 instructies nodig.

a. 7 % 3
```{admonition} Uitwerking
:class: dropdown

Register instellen: R1 = 7 // Eerste instructie

Opcode: *waarde instellen* --> `00101`

Argument 1: R1 --> `001`

Argument 2: 7 --> `0000 0111`

Resulterende instructie: **`00101 001 0000 0111`**

Berekening: R1 = R1 % 3 // Tweede instructie

Opcode: *modulo nemen* --> `00001`

Argument 1: R1 --> `001`

Argument 2: 3 --> `0000 0011`

Resulterende Instructie: **`00001 001 0000 0011`**

De code voor 7 % 3 is dus

`0010100100000111`

`0000100100000011`

of `0x2907` en `0x0903` in hexadecimale notatie.
```

b. 11 + 14

c. 49 - 25

d. 20 % 6

e. 16 + 16

f. 8 & 29
````


## Compileren of interpreteren

Je hebt nu gezien dat het schrijven van programma's in machinetaal best bewerkelijk is. Assembly lost deze bewerkelijkheid maar deels op. Machinetaal en assembly noemen we daarom ook **lagere programmeertalen**. Een lagere programmeertaal kenmerkt zich doordat je veel kleine instructies achter elkaar moet zetten om iets fatsoenlijks voor elkaar te krijgen. En hoe zit dat dan met talen als C, Java, Python, Rust en SQL? Dit zijn hogere programmeertalen. Programma's geschreven in deze hogere programmeertalen worden ook uitgevoerd door de computer, dus die zullen op een of andere manier bij de processor en machinetaal niveau uit moeten komen.

Dat kan op twee manieren gebeuren. Je kan het programma laten vertalen naar assembly. Dat is het werk van een **compiler** Die vertaalt een hogere programmeertaal naar een lagere programmeertaal, zoals assembly. Vaak is naast een compiler dus ook een assembler nodig om een programma om te zetten in machinetaal. Voorbeelden van gecompileerde talen zijn C, C++, Pascal, Rust en Golang.

Je kan het programma ook laten uitvoeren door een ander programma. Dat leest de opdrachten dan, en voert ze achter elkaar uit. Zo’n programma noemen we een **interpreter**. Voorbeelden van geïnterpreteerde talen zijn Python, Javascript en Ruby.

Het voordeel van compileren is dat het programma snel uit te voeren is door een processor. Dat is ongeveer tien keer sneller dan wanneer het programma geïnterpreteerd moet worden. Bovendien is er steeds een interpreter nodig als het programma geïnterpreteerd moet worden, terwijl na het compileren het programma zelf genoeg is om te hebben. Het voordeel van interpreteren is dat er ook een klein stukje code geïnterpreteerd kan worden, zonder dat het hele programma eerst vertaald moet worden naar machinetaal.

Tegenwoordig is er ook een tussenvorm van compileren en interpreteren, die de voordelen van beide technieken combineert. Daarvoor wordt de hogere programmeertaal in twee stappen gecompileerd: eerst naar een hogere vorm van assembly, die **bytecode** wordt genoemd, en daarna naar assembly of machinetaal. Die laatste stap wordt pas gedaan als het programma uitgevoerd wordt. Daarom wordt de compiler die daarvoor gebruikt wordt een **just-in-time (JIT) compiler** genoemd. Die vertaalt de bytecode naar gewone assembly of direct naar machinetaal. Die bytecode wordt gemaakt door de compiler van talen die werken met een JIT compiler, zoals Java en C#.

```{figure} assets/image-20231223004711386.png
---
align: center
---
Bron: GeeksforGeeks
```

Voor meer informatie kan je terecht op [Just In Time Compiler - GeeksforGeeks](https://www.geeksforgeeks.org/just-in-time-compiler/).

Om een beeld te krijgen van de verhouding van machinetaal en assembly, maak je de onderstaande oefening. Daarin vertaal je machinetaal naar assembly. Je doet dus precies het tegenovergestelde van wat een assembler doet. Dat is **reverse engineering**, iets wat bijvoorbeeld hackers doen om te ontdekken wat een programma precies doet en waar de zwakke plekken zitten.

````{exercise} Machinetaal schrijven
Schrijf de onderstaande machinetaal-instructies op in assembly. Om de opdracht te testen, kan je je antwoord invoeren in de RISC-simulator. Samen vormen de opdrachten een programma van 6 regels, dus het beste test je het in één keer samen.

De eerste opdracht is voorgedaan, volg dat voorbeeld bij je eigen uitwerking.

a. `0010101100000111`
```{admonition} Uitwerking
:class: dropdown
Opcode: `00101` -> `MOV`

Rsd: `011` -> `R3`

imm8: `00000111` -> 7

Antwoord: `MOV R3, #7`

*Let op*: zet een hekje voor het getal van imm8!
```

b. `0001001100001111`

c. `0000101100000100`

d. `0010110100001011`

e. `0001110100000110`

f. `0000000000000000`
````

