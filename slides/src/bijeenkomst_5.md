# Gegevens verwerken<br/><small>in machinetaal</small>

Q-highschool / Computerarchitectuur / Bijeenkomst 5

***

## Vandaag

- Opfrisquiz (over les 3 en 4)
- Uitleg: datasectie en geheugengebruik
- Zelfstandig werken aan hoofdstuk 5
- Afsluiting

Note: Deze bijeenkomst is online. Studenten krijgen eerst een opfrisquiz, daarna uitleg over hoe je het geheugen gebruikt in assembly (datasectie, DAT, LDR, STR). Daarna werken ze zelfstandig aan hoofdstuk 5.

***

## Opfrisquiz

Note: Deze quiz test de kennis van les 3 (architectuur) en les 4 (assembly en machinetaal).

---

Wat is het verschil tussen de von Neumann en Harvard architectuur?

- Von Neumann heeft aparte geheugens voor instructies en data, Harvard niet
- Harvard heeft aparte geheugens voor instructies en data, von Neumann niet
- Von Neumann is sneller dan Harvard
- Ze zijn hetzelfde, alleen de namen verschillen

<!-- .element: class="mc" -->

---

Wat doet de ALU in een processor?

- Het regelt de stroom van instructies
- Het voert berekeningen en logische operaties uit
- Het slaat gegevens op voor later gebruik
- Het communiceert met externe apparaten

<!-- .element: class="mc" -->

---

Wat is de functie van de program counter (PC)?

- Hij telt hoeveel programma's er draaien
- Hij houdt bij welke instructie als volgende uitgevoerd moet worden
- Hij meet hoe snel het programma loopt
- Hij telt hoeveel fouten er in het programma zitten

<!-- .element: class="mc" -->

---

Wat betekent de instructie `ADD R1, #5`?

- Tel 5 op bij de waarde in register R1
- Zet de waarde 5 in register R1
- Trek 5 af van register R1
- Vermenigvuldig R1 met 5

<!-- .element: class="mc" -->

---

Wat is een mnemonic in assembly?

- Een binaire code voor een instructie
- Een geheugensteuntje: een afkorting zoals ADD of MOV
- Een speciaal soort register
- Een foutmelding van de assembler

<!-- .element: class="mc" -->

---

Hoeveel bits is één instructie in de RISC-simulator?

- 8 bits
- 16 bits
- 32 bits
- 64 bits

<!-- .element: class="mc" -->

---

Wat is de opcode in een instructie?

- Het register waar het resultaat in komt
- De vaste waarde (immediate) die gebruikt wordt
- De code die aangeeft welke operatie uitgevoerd moet worden
- Het geheugenadres waar de instructie staat

<!-- .element: class="mc" -->

---

Wat is het grootste getal dat past in een imm8 veld?

- 8
- 127
- 255
- 256

<!-- .element: class="mc" -->

---

Wat doet de instructie `HLT`?

- Hij springt terug naar het begin van het programma
- Hij stopt de uitvoering van het programma
- Hij wist alle registers
- Hij laadt een waarde uit het geheugen

<!-- .element: class="mc" -->

---

In de instructie `ADD R1, R2`, wat gebeurt er?

- R1 en R2 worden opgeteld en het resultaat komt in R3
- De waarde van R2 wordt opgeteld bij R1, resultaat in R1
- De waarde van R1 wordt opgeteld bij R2, resultaat in R2
- R1 en R2 worden vermenigvuldigd

<!-- .element: class="mc" -->

***

## Tekstsectie en datasectie

Note: Timing: 20 minuten uitleg over tekstsectie/datasectie, DAT, LDR en STR

---

### Probleem: waar zet je je data?

Tot nu toe werkten we alleen met registers en immediates:

```
MOV R1, #7
ADD R1, #23
```

Maar wat als je met grotere getallen wilt werken?  
Of als je getallen wilt bewaren voor later?

Notes: Leg uit dat registers beperkt zijn (8 stuks) en immediates maximaal 8 bits (255) kunnen zijn.

---

### De oplossing: het geheugen gebruiken

Een programma bestaat uit twee delen:

1. **Tekstsectie**: de instructies die uitgevoerd worden
2. **Datasectie**: de data (getallen) die het programma gebruikt

De datasectie komt altijd **na** de tekstsectie!

Note: De processor begint bij adres 0 en voert instructies uit. Daarom moeten instructies eerst komen. Na HLT komen de data.

---

### Tekstsectie: instructies

De tekstsectie bevat alle instructies die uitgevoerd worden:

```
MOV R1, #7
ADD R1, #23
HLT
```

De tekstsectie eindigt altijd met `HLT` (halt).

Note: HLT zorgt ervoor dat de processor stopt met instructies uitvoeren. Daarna komt de datasectie.

---

### Datasectie: getallen

Na `HLT` komt de datasectie met je data:

```
MOV R1, #7
ADD R1, #23
HLT
DAT 100
DAT 200
DAT 0
```

Hier staan drie getallen: 100, 200 en 0.

Note: DAT betekent "data". Het is géén instructie, maar een geheugenplaats met een waarde.

***

## DAT: data in het geheugen

Note: Timing: eerste 5 minuten van de uitleg

---

### DAT: waarden neerzetten

Met `DAT` zet je een waarde (16 bits) in het geheugen:

```
DAT 42
DAT 100
DAT 0xFFFF
```

Je kan decimaal (42) of hexadecimaal (0xFFFF) schrijven.

Note: DAT staat voor "data". Het is het enige mnemonic dat niet vertaald wordt naar een opcode.

---

### Voorbeeld

```
MOV R1, #5
HLT
DAT 7
DAT 23
DAT 100
```

De getallen 7, 23 en 100 staan op adressen 2, 3 en 4.

Note: Adres 0 is MOV, adres 1 is HLT, adressen 2-4 zijn de data.

---

<!-- .slide: class="voordoen" -->

### Voordoen: DAT gebruiken

Open de [RISC-simulator](http://peterhigginson.co.uk/RISC/) en typ:

```
MOV R1, #5
HLT
DAT 10
DAT 20
DAT 30
```

Kijk in het geheugen: zie je de waarden staan?

Note: Laat studenten dit zelf typen en uitvoeren. Laat ze zien dat de waarden in het geheugen staan op de juiste adressen. Timing: 3 minuten.

***

## LDR: laden uit het geheugen

Note: Timing: volgende 7 minuten van de uitleg

---

### LDR: Load Register

Met `LDR` (Load Register) laad je een waarde uit het geheugen in een register:

```
LDR Rd, adres
```

- `Rd`: het register waar de waarde in komt
- `adres`: het geheugenadres waar de waarde staat

Note: LDR haalt een waarde op uit het geheugen en zet hem in een register.

---

### Voorbeeld

```
LDR R1, 2
HLT
DAT 42
```

Wat gebeurt er?

1. De waarde op adres 2 (dat is 42) wordt geladen
2. Die waarde wordt in register R1 gezet

Note: Adres 0 is LDR, adres 1 is HLT, adres 2 is DAT 42. Dus R1 krijgt de waarde 42.

---

### Geheugenadres berekenen

Hoe weet je welk adres je moet gebruiken?

```
     MOV R1, #5    // adres 0
     HLT           // adres 1
     DAT 7         // adres 2
     DAT 23        // adres 3
```

Om het getal 23 te laden: `LDR R1, 3`

Note: Tel vanaf 0. Elke regel is één adres. Je moet zelf uitrekenen op welk adres je data staat.

---

<!-- .slide: class="voordoen" -->

### Voordoen: LDR gebruiken

Open de [RISC-simulator](http://peterhigginson.co.uk/RISC/) en typ:

```
LDR R1, 3
HLT
DAT 100
DAT 200
```

Wat staat er in R1 na uitvoering?

Note: Laat studenten dit uittypen en uitvoeren. R1 moet 200 bevatten. Leg uit waarom: adres 0=LDR, 1=HLT, 2=100, 3=200. Timing: 4 minuten.

***

## STR: opslaan in het geheugen

Note: Timing: laatste 8 minuten van de uitleg

---

### STR: Store Register

Met `STR` (Store Register) sla je een waarde uit een register op in het geheugen:

```
STR Rd, adres
```

- `Rd`: het register waar de waarde vandaan komt
- `adres`: het geheugenadres waar de waarde naartoe moet

Note: STR doet het omgekeerde van LDR: hij schrijft een waarde van een register naar het geheugen.

---

### Voorbeeld

```
MOV R1, #42
STR R1, 3
HLT
DAT 0
```

Wat gebeurt er?

1. R1 krijgt de waarde 42
2. Die waarde wordt weggeschreven naar adres 3
3. Het getal op adres 3 verandert van 0 naar 42

Note: STR schrijft de waarde van R1 (42) naar het geheugen op adres 3.

---

### Volledig voorbeeld

```
LDR R1, 5      // laad eerste getal (7)
LDR R2, 6      // laad tweede getal (2)
MUL R1, R2     // vermenigvuldig: 7 * 2 = 14
STR R1, 7      // sla resultaat op
HLT
DAT 7          // eerste getal (adres 5)
DAT 2          // tweede getal (adres 6)
DAT 0          // plek voor antwoord (adres 7)
```

Note: Dit programma rekent 7 * 2 uit en slaat het resultaat (14) op in het geheugen op adres 7.

---

<!-- .slide: class="voordoen" -->

### Voordoen: volledig programma

Open de [RISC-simulator](http://peterhigginson.co.uk/RISC/) en typ het vorige programma over.

Voer het stap voor stap uit (STEP) en kijk wat er gebeurt:
- Wat gebeurt er met R1 en R2?
- Waar komt het resultaat te staan?

Note: Laat studenten dit zelf typen en uitvoeren met de STEP knop. Bespreek elke stap. Timing: 5 minuten.

***

## Aan de slag

Note: Timing: 5 minuten uitleg over wat ze gaan doen

---

### Hoofdstuk 5: Gegevens verwerken

Nu ga je zelf oefenen met:

- `DAT`: waarden in het geheugen zetten
- `LDR`: waarden laden uit het geheugen
- `STR`: waarden opslaan in het geheugen  
- Tekstsectie en datasectie gebruiken
- Berekeningen met gegevens uit het geheugen

Note: Leg uit dat ze nu zelfstandig aan hoofdstuk 5 gaan werken in de syllabus.

---

### Werktijd

- Werk zelfstandig door hoofdstuk 5 in de syllabus
- Test je code in de [RISC-simulator](http://peterhigginson.co.uk/RISC/)
- Stel vragen in de chat als je vastloopt
- Doel: zoveel mogelijk van hoofdstuk 5 afmaken

Note: Timing: 50 minuten werktijd. Loop rond in breakout rooms of beantwoord vragen in de chat. Studenten kunnen in eigen tempo werken.

***

## Afsluiting

Note: Timing: 5 minuten

---

### Wat heb je geleerd?

- Het verschil tussen tekstsectie en datasectie
- Waarden in het geheugen zetten met `DAT`
- Waarden laden met `LDR`
- Waarden opslaan met `STR`
- Berekeningen uitvoeren met gegevens uit het geheugen

Note: Vraag wat studenten lastig vonden. Geef vooruitblik naar volgende les.

---

### Volgende keer

Les 6: Programmabesturing

- Springen in code met `BRA`
- Labels gebruiken
- Condities met `CMP`
- Loops en if-else structuren programmeren

Note: Volgende les gaan we kijken naar hoe je de flow van je programma kan besturen met sprongen en condities.

---

### Huiswerk

- Maak hoofdstuk 5 helemaal af
- Test alle voorbeelden in de RISC-simulator
- Oefen met de opdrachten uit de syllabus
- Zorg dat je begrijpt hoe LDR en STR werken

Note: Zorg dat studenten weten wat ze af moeten hebben voor volgende keer.

---

### Veel succes! 

Vragen? Stel ze in de chat!

Note: Einde van de bijeenkomst. Studenten kunnen nu verder werken of afsluiten.
