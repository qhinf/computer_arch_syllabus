(hoofdstuk-besturing)=
# Machinetaal - Programmabesturing

## Springen door de code

In een imperatief computerprogramma maak je gebruik van drie structuren:

1. Opeenvolging
2. Keuze
3. Herhaling

In Python is keuze en herhaling te herkennen respectievelijk aan `if-else` en aan `for`/`while`. De opeenvolging is te herkennen aan het onder elkaar plaatsen van commando's. Bijvoorbeeld:

```python
getal = 42
print(42)
```

Tot nu toe kunnen we alleen de eerstgenoemde structuur toepassen in ASM. Voor beide andere structuren is het nodig om in de code naar een andere regel te kunnen springen. Dat springen noemen we **branching**.

Het enige wat je hoeft te doen om te springen in een programma, is de program counter (PC-register) aanpassen. De meest eenvoudige manier om dat te doen is met het `BRA`-mnemonic, wat staat voor *branch always*. Daarbij is één argument nodig, namelijk het geheugenadres dat weggeschreven moet worden in het PC-register. De vorm van de instructie is dus `BRA address`.

Als je  instructie in je programma zet, en je springt vooruit in de code, dan laat je een of meer instructies overslaan.  Als je ermee terug springt, dan creëer je een **infinite loop** (eindeloze herhaling). Om hier een beeld bij te krijgen, voer je de volgende oefening uit.

:::{exercise}

Beantwoord de vragen hieronder, die gaan over deze code:

```
MOV R1, #1
MUL R1, #2
BRA 1
```

Voer het programma nog niet uit!

a. Naar welke instructie springt de processor terug? Naar `MOV` of `MUL`?

b. Voorspel wat de waarde is van `R1` na 10 herhalingen?

c. Voorspel wat de waarde is van `R1` na 15herhalingen?

d. Voorspel wat de waarde is van `R1` na 20 herhalingen?

e. Voer de code nu uit in de [RISC-simulator](http://peterhigginson.co.uk/RISC/). Zet de speed op 6 en houdt `R1` goed in de gaten. Wat zijn de goede antwoorden op a, b, c en d hierboven?

f. Selecteer nu bij OPTIONS: hex, en voer het programma nog eens uit. Wat is de waarde die `R1` na 15 herhalingen als je het hexadecimaal noteert?
:::

Wat je in deze oefening hebt ontdekt, noemen we een **overflow**: het getal wordt na 16 herhalingen groter dan wat er opgeslagen kan worden in de 16 bits die een register groot is. De waarde is dan eigenlijk een 1 met 16 nullen. Dan worden alleen de 16 nullen nog opgeslagen, en de 1 gaat verloren. Daarom was de waarde van R1 daarna steeds 0.

Verder heb je ook gezien dat -32768 hetzelfde is als 0x8000. Dat is de manier waarop de RISC-simulator negatieve getallen opschrijft: de voorste bit heeft niet de waarde van $2^{15}$, maar van $-2^{15}$. Deze manier van negatieve getallen noteren heet *two’s complement* (zie {ref}`twee-complement`).

Om aan te geven bij welke instructie het programma verder moet gaan, kan je ook gebruikmaken van labels. Dat werkt hetzelfde als met variabelen (`DAT`). Als je labels gebruikt, hoef je niet alle adressen achter branch-instructies te veranderen zodra je een instructie toevoegt aan  het programma, of er juist uit weghaalt. Daarom gebruik je altijd de naam van een label als argument bij een branch-instructie. De code uit oefening 1 kan je dus beter opschrijven zoals je hieronder ziet staan.

```
      MOV R1, #1
loop: MUL R1, #2
      BRA loop
```
## Condities

Meestal wil je helemaal geen infinite loop, maar een herhaling die door blijft gaan totdat er aan een voorwaarde wordt voldaan. Daarom gebruik je een branch-opdracht meestal samen met een conditie (voorwaarde). Daarin vergelijk je twee getallen met elkaar. Dat doe je met de instructie CMP. In de tabel hieronder zie je welke mogelijke argumenten je daarbij kan gebruiken. Hoe de ALU een vergelijking precies uitvoert, lees je verderop onder het kopje “waarden vergelijken”.

(branching-table)=
```{list-table} Condities voor branching
:header-rows: 1
* - instructie
  - betekenis
* - `CMP` Rb, #imm8
  - Vergelijk de waarde in Rb met een vaste waarde #imm8
* - `CMP` Rb, Rs
  - Vergelijk de waarde in Rb met de waarde in Rs
```
Nadat de ALU de vergelijking heeft uitgevoerd, kan je de program counter laten veranderen. Of dat moet gebeuren, is afhankelijk van het resultaat van de vergelijking. Om te kiezen op welk resultaat de program counter moet reageren, zijn er meerdere varianten beschikbaar van de branch-instructie. Allemaal beginnen ze wel met een B. Daarachter staan twee letters die aangeven wanneer de program counter aangepast moet worden. rechts hiervan zie je een deel van de mogelijkheden weergegeven. Er zijn nog negen andere, maar die zal jij voorlopig niet nodig hebben.
```{list-table} Branch mnemonics
:header-rows: 1
* - conditie
  - mnemonic
  - betekenis
* - geen
  - `BRA` address
  - **BR**anch **A**lways to address
* - $A = B$
  - `BEQ` address
  - **B**anch if **EQ*ual to address
* - $A\neq B$
  - `BNE` address
  - **B**ranch if **N**ot **E**qual to address
* - $A<B$
  - `BLT` address
  - **B**ranch if **L**ess **T**han to address
* - $A>B$
  - `BGT` address
  - **B**ranch if **G**reater **T**han to address
* - $A\leq B$
  - `BLE` address
  - **B**ranch if **L**ess or **E**qual to address
* - $A\geq B$
  - `BGE` address
  - **B**ranch if **G**reater or **E**equal to address
```
Je kan branchen met een conditie op twee manieren gebruiken: door de Program Counter (PC) achteruit te laten springen, of juist vooruit. Beide opties worden uitgelegd in de volgende drie deelparagrafen.

## Eindige herhaling
Als je met een conditionele branch-instructie de program counter terug laat springen, dan krijg je een *eindige herhaling*. In talen als Python, C of Javascript heet dat een `for`- loop. Om dat goed in assembly te coderen, voer je het onderstaande stappenplan uit.

```{admonition} Stappenplan eindige herhaling vertalen
1. **Label**<br />
Begin met het label op de regel waar de program counter naar teruggezet moet worden.
2. **Instructies**<br />
Voer instructies in die uitgevoerd moet worden.
3. **Registers laden**<br />
Laad alle waarden in registers die nodig zijn voor de vergelijking.
4. **Voorwaarde berekenen**<br />
Voer berekeningen uit die nodig zijn voor de vergelijking. (niet altijd nodig)
5. **Waarden vergelijken**<br />
Zet een `CMP`-instructie neer met het juiste register en een waarde (of twee registers).
6. **Branch-instructie**
Vertaal de conditie naar de juiste branch-instructie (`B..`)
```
```{figure} assets/image-20240104150615568.png
---
align: right
scale: 50%
---
```

Als voorbeeld kijken we naar een robothondje dat door moet lopen totdat het iets dichtbij ziet, binnen 10 centimeter. Aan de rechterkant zie je dat programma weergegeven als activity diagram. We zullen alle zes de stappen van het stappenplan nu uitvoeren.

**1. Label**

Het hondje moet lopen, dus noemen we het label “walk”. Het is namelijk altijd een goed idee om namen die je zelf mag kiezen zo duidelijk mogelijk te laten zijn. De code begint dus zo:

```

  walk:
  
```

**2. Instructies**

Zo lang de voorwaarde geldt (er is niets binnen 10 cm), moet het hondje een stap zetten. In dit programma gaan we ervan uit dat de pootjes van het hondje reageren op adres 4.

Om een waarde weg te schrijven, hebben we de `OUT`-instructie nodig. Die instructie heeft twee argumenten: een register en een adres. De waarde die we naar adres 4 gaan schrijven, moet dus in een register staan. Daarom voegen we bovenaan het programma een regel toe waarin we de waarde 1 in register `R1` zetten.

Dan wordt de code als volgt:

```
      MOV R1, #1  // speed
walk: OUT R1, 4   // make a step

```

*Toelichting: We zouden het label `walk:` ook voor de `MOV`-instructie kunnen zetten, maar die instructie hoeft niet steeds herhaald te worden. Als `R1` namelijk eenmaal de waarde `1` heeft, blijft dat zo (totdat de waarde veranderd wordt met een andere instructie, maar dat gaat we nu niet doen in de herhaling).*

**3. Registers laden**
Nu moeten we zorgen dat de gemeten afstand in een register komt te staan. De waarde gaan we opvragen met de `INP`-instructie. De afstandssensor is luistert op adres 2. De waarde zetten we in register `R2`(dat is het eerste niet-gebruikte register).

De code ziet er nu zo uit:
```
      MOV R1, #1  // speed
walk: OUT R1, 4   // make a step
      INP R2, 2   // measure distance
```

**4. Voorwaarde berekenen**

Er hoeft niets berekend te worden in dit voorbeeld.

**5. Waarden vergelijken**

De gemeten afstand moet vergeleken worden met 10. Daarvoor gebruiken we de `CMP`-instructie. De code wordt nu:

```
      MOV R1, #1  // speed
walk: OUT R1, 4   // make a step
      INP R2, 2   // measure distance
      CMP R2, #10 // compare distance to 10
```

**6. Branch-instructie**

Als uit deze vergelijking komt dat de gemeten afstand groter is, dan moeten de instructies van stap 2 herhaald worden. Daarom gebruiken we de `BGT`-instructie (zie {ref}`branching-table` voor meer opties). De code is nu:

```
      MOV R1, #1  // speed
walk: OUT R1, 4   // make a step
      INP R2, 2   // measure distance
      CMP R2, #10 // compare distance to 10
      BGT walk    // branch if greater than
```

Als laatste moet het programma ook stoppen als er niet herhaald wordt. Daarom sluiten we af met de `HLT`-instructie.

De uiteindelijke code is dus:
```
      MOV R1, #1  // speed
walk: OUT R1, 4   // make a step
      INP R2, 2   // measure distance
      CMP R2, #10 // compare distance to 10
      BGT walk    // branch if greater than
      HLT         // stop
```

Hieronder zie je een tabel met uitleg per regel, zodat je de code nog eens kunt doornemen om te 
begrijpen wat er gebeurt. Je kan de code ook kopiëren en plakken in de RISC-simulator om te zien wat er gebeurt.

```{list-table}
:header-rows: 1
* - regel
  - uitleg
* - 0: `MOV`
  - Het programma begint met het instellen van een register, omdat die straks nodig is om een waarde naar de pootjes te sturen (`OUT` kan alleen een waarde uit een register gebruiken).
* - 1: `OUT`
  - Je ziet dat de eerste ruit vertaald is met een label, namelijk “loop”.<br />
De opdracht “make a step” wordt uitgevoerd door het getal 1 (uit register `R1`) te schrijven naar output 6 (we doen even alsof dat de pootjes van het hondje zijn).
* - 2: `INP`
  - Het meten van de afstand wordt gedaan door invoer te lezen op adres 2 (bij het testen kan je zelf een getal invoeren). Deze afstand wordt opgeslagen in register `R2`.
* - 3: `CMP`
  - De gemeten afstand uit register `R2` wordt vergeleken met de vaste waarde `#10`.
* - 4: `BGT`
  - Als het resultaat van de vergelijking “groter dan” is, dan wordt de program counter aangepast naar 1 (de regel waar “loop” staat). Anders wordt de program counter gewoon 1 hoger dan dat die was, dus 5.
* - 5: `HLT`
  - Het programma stopt.
```

In de onderstaande oefening ga je zelf aan de slag met een eindige herhaling.

:::{exercise} Getal raden 1
Maak een raad-spel in de [RISC-simulator](http://peterhigginson.co.uk/RISC/) dat als volg werkt:

1. Iemand kan een getal invoeren onder de 30.
2. Iemand anders moet raden welk getal het was door een getal in te voeren.
3. Als het getal goed is, print het programma 0, en begint het opnieuw.
:::

## Keuzes

We gaan nu kijken hoe je een keuze-structuur (`if`-statement) vertaalt naar assembly. Hierbij laat je de program counter juist vooruit springen in een programma. Het stappenplan dat je hierbij gebruikt, lijkt aardig op dat van een eindige herhaling. Er zijn een paar stappen extra nodig, omdat je bij keuzes ook wilt kunnen aangeven wat er anders moet gebeuren (else), en omdat je de voorwaardelijke instructies anders moet overslaan.

```{admonition} Stappenplan keuze structuur vertalen
1. **Registers laden**<br />
Laad alle waarden van variabelen in registers
2. **Voorwaarde berekenen**<br />
Voer berekeningen uit. (niet altijd nodig)
3. **Waarden vergelijken**<br />
Zet een `CMP`-instructie neer met het juiste register en een waarde (of twee registers)
4. **Branch-instructie indien waar**<br />
Vertaal de conditie naar de juiste branch-instructie.
5. **Branch-instructie indien onwaar**<br />
Sluit af met een `BRA`-instructie voor het geval dat de voorwaarde niet waar is, zodat instruties overgeslagen worden die alleen uitgevoerd moeten worden als de voorwaarde wel waar is.
6. **Label indien waar**
Zet een label neer op de plek waar de instructies staan die alleen uitgevoerd moeten worden als aan de voorwaarde is voldaan.
7. **Instructies indien waar**<br />
Achter het label zet je de instructies neer die uitgevoerd moeten worden als aan de voorwaarde is voldaan. 
8. **Label aan het einde**<br />
Tenslotte zet je een label neer op de plek waar het programma verder gaat met instructies, die niet met de voorwaarde te maken hebben.
```

```{figure} assets/image-20240105002639698.png
---
align: right
scale: 50%
---
```

Laten we dit stappenplan nu toepassen op een concreet voorbeeld. Om in een game te bepalen of de speler een level al gewonnenheeft, zou je de volgende code kunnen gebruiken:

```C
if (score == 100) {
	level = 2;
}
```

Rechts zie je deze code weergegeven als een activity diagram. We zullen dit programma nu stap voor stap vertalen naar assembly.

Het programma heeft twee variabelen nodig: `score` en `level`. Daarom begint het programma als volgt:

```
// text

// data
score: DAT 0
level: DAT 1
```
In de uitwerking van de stappen wordt alleen de code voor in de tekstsectie weergegeven. Aan het einde, als alle stappen zijn uitgevoerd, staat de complete, uitvoerbare code.

**1. Registers laden**

In de voorwaarde is de waarde van score nodig. Die laden we dus in een register:

```
               LDR R1, score
```
**2. Voorwaarde berekenen**

Deze stap is in dit geval niet nodig. Later zullen we nog een voorbeeld zien waarbij het wel nodig is.
    
**3. Waarden vergelijken**

We vergelijken die waarde nu met 100:

```
               LDR R1, score
               CMP R1, #100
```
** 4. Branch-instructie indien waar**

Er moet gesprongen worden naar het juiste label als de waarden gelijk zijn, dus `BEQ`. We bedenken nu wat de naam van het label is dat we in stap 6 gaan neerzetten: `if_score100`.
```
               LDR R1, score
               CMP R1, #100
               BEQ if_score100    // if score equals 100
```
**5. Branch-instructie indien onwaar**

We voegen nu een `BRA`-instructie toe zodat het level niet verandert wordt naar 2 als de score nog geen 100 is. Hiermee springen we dus over de code heen die alleen uitgevoerd moet worden als de voorwaarde waar is. Ook hier bedenken we een label, nu alvast voor stap 8: `skip_score100`.

```
               LDR R1, score
               CMP R1, #100
               BEQ if_score100    // if score equals 100
               BRA skip_score100  // if not: skip conditional code
```

**6. Label indien waar**
    
Nu zetten we het label `if_score100` neer. De rest van de code laten we inspringen tot de maximale lengte van een label, zodat je de labels makkelijk kan terugvinden.

```
               LDR R1, score
               CMP R1, #100
               BEQ if_score100    // if score equals 100
               BRA skip_score100  // if not: skip conditional code
if_score100:   
```

**7. Instructies indien waar**

Achter dat label zetten we de instructies om het level te veranderen naar 2:

```
               LDR R1, score
               CMP R1, #100
               BEQ if_score100    // if score equals 100
               BRA skip_score100  // if not: skip conditional code
if_score100:   MOV R1, #2
               STR R1, level
```

**8. Label aan het einde**
    
Ten slotte zetten we het label `skip_score100` neer op de plek waar het programma verder gaat met instructies die niets met de voorwaarde te maken hebben. In dit geval zetten we daar `HLT` achter, zodat het programma stopt. Merk op dat deze instructie ook uitgevoerd zal worden als de voorwaarde wel waar was!

```
// text
               LDR R1, score
               CMP R1, #100
               BEQ if_score100    // if score equals 100
               BRA skip_score100  // if not: skip conditional code
if_score100:   MOV R1, #2
               STR R1, level
skip_score100: HLT
```

De volledige code is nu als volgt:
```
// text
               LDR R1, score
               CMP R1, #100
               BEQ if_score100    // if score equals 100
               BRA skip_score100  // if not: skip conditional code
if_score100:   MOV R1, #2
               STR R1, level
skip_score100: HLT

// data
score: DAT 0
level: DAT 1
```

Probeer dit programma eens uit te voeren in de [RISC-simulator](http://peterhigginson.co.uk/RISC/). Test eens of het werkt door score op 100 te zetten (`score: DAT 100`). Kijk naar de waarde van geheugencel 8 (dat is de plek waar de waarde van level opgeslagen staat).

In de volgende oefening ga je zelf ook een stukje conditionele code vertalen.

:::{exercise} Uitroepteken bij een lege accu
Schrijf een programma in de RISC-simulator dat een uitroepteken print als de accu minder dan 10 procent vol is. Gebruik de onderstaande code als basis. Het uitroepteken staat al klaar in de datasectie.

```
// text
INP R1, 2 // batterij-niveau opvragen via input

LDR R2, warning // uitroepteken inladen
OUT R2, 7       // uitroepteken weergeven

// data
warning: DAT 0x21
```
Volg de acht stappen van het “stappenplan keuze-structuur vertalen”.
:::

:::{exercise} Getal raden 2
Deze oefening is een uitbreiding op **getal raden (1)**. Gebruik het programma dat je daarvoor geschreven hebt als basis!

Maak een raad-spel in de [RISC-simulator](http://peterhigginson.co.uk/RISC/) dat als volg werkt:

1. Iemand kan een getal invoeren onder de 30.
2. Iemand anders moet raden welk getal het was door een getal in te voeren.
3. Het programma reageert met < (kleiner) of > (groter).
4. Het programma telt het aantal pogingen.
5. Als het getal goed is, print het programma het aantal pogingen, en begint het opnieuw.
:::

## Uitgebreide keuzes

Bij een keuze-structuur (`if`) kan je natuurlijk ook aangeven wat er moet gebeuren als de voorwaarde niet geldt (`else`). Die code kan je in assembly neerzetten tussen de twee branch-instructies (dus vlak voor de `BRA`-instructie). Hieronder zie je het uitgebreide stappenplan, met een extra stap op plek 5.

```{admonition} Stappenplan keuze structuur vertalen
1. **Registers laden**<br />
Laad alle waarden van variabelen in registers
2. **Voorwaarde berekenen**<br />
Voer berekeningen uit. (niet altijd nodig)
3. **Waarden vergelijken**<br />
Zet een `CMP`-instructie neer met het juiste register en een waarde (of twee registers)
4. **Branch-instructie indien waar**<br />
Vertaal de conditie naar de juiste branch-instructie.
5. **Instructies indien onwaar**<br />
Zet de instructies neer die moeten worden uitgevoerd als er niet aan de voorwaarde is voldaan.
6. **Branch-instructie indien onwaar**<br />
Sluit af met een `BRA`-instructie voor het geval dat de voorwaarde niet waar is, zodat instruties overgeslagen worden die alleen uitgevoerd moeten worden als de voorwaarde wel waar is.
7. **Label indien waar**
Zet een label neer op de plek waar de instructies staan die alleen uitgevoerd moeten worden als aan de voorwaarde is voldaan.
8. **Instructies indien waar**<br />
Achter het label zet je de instructies neer die uitgevoerd moeten worden als aan de voorwaarde is voldaan. 
9. **Label aan het einde**<br />
Tenslotte zet je een label neer op de plek waar het programma verder gaat met instructies, die niet met de voorwaarde te maken hebben.
```
Dit stappenplan zullen we uitvoeren met een voorbeeld: bepalen of een cijfer voldoende is, of onvoldoende. De gebruiker moet eerst twee dingen invoeren: het maximale aantal punten en het gehaalde aantal punten. Dan berekent het programma of het resultaat voldoende (V) of onvoldoende (O) is. Hieronder zie je hoe de code er in een hogere programmeertaal uit zou kunnen zien.

```Javascript
if (score*10/max >= 6) {
	print("V")
}
else {
	print("O")
}
```

**1. Registers laden**
    
Het programma vraagt de gebruiker eerst om twee getallen. Daarom begint het programma als volgt:
```
            INP R1, 2   // max. score
            INP R2, 2   // score
```

**2. Voorwaarde berekenen**
    
In de voorwaarde staat nu ook een berekening die uitgevoerd moet worden voordat er twee getallen vergeleken kunnen worden: `score*10/max`. De code ziet er dan zo uit:

```
            INP R1, 2   // max. score
            INP R2, 2   // score
            MUL R2, #10 // score*10
            DIV R2, R1  // score*10/max
```

**3. Waarden vergelijken**
    
De uitkomst van de berekening moeten we vergelijken met `6`:

```
            INP R1, 2   // max. score
            INP R2, 2   // score
            MUL R2, #10 // score*10
            DIV R2, R1  // score*10/max
            CMP R2, #6  // compare to 6
```

**4. Branch-instructie indien waar**
    
De vraag is of het groter of gelijk is, dus `BGE`:

```
            INP R1, 2   // max. score
            INP R2, 2   // score
            MUL R2, #10 // score*10
            DIV R2, R1  // score*10/max
            CMP R2, #6  // compare to 6
            BGE sufficient
```

**5. Instructies indien onwaar**
    
Als de voorwaarde geldt, wordt de volgende code overgeslagen die staat tussen `BGE` en `BRA` (met daarna het label `sufficiënt`, zie bij stap 7). Hier laten we dus de letter `O` printen (ASCII: `0x4F` oftewel `79`):

```
            INP R1, 2   // max. score
            INP R2, 2   // score
            MUL R2, #10 // score*10
            DIV R2, R1  // score*10/max
            CMP R2, #6  // compare to 6
            BGE sufficient
            MOV R3, #79
            OUT R3, 7
```

**6. Branch-instructie indien onwaar**
    
Nu zetten we een `BRA`-instructie neer om het gedeelte over te slaan dat alleen uitgevoerd moet worden als de voorwaarde geldt:

```
            INP R1, 2   // max. score
            INP R2, 2   // score
            MUL R2, #10 // score*10
            DIV R2, R1  // score*10/max
            CMP R2, #6  // compare to 6
            BGE sufficient
            MOV R3, #79
            OUT R3, 7
            BRA end
```

**7. Label indien waar**
    
Hierna komt het label waar de program counter naartoe moet springen als het cijfer wel minimaal een 6 was:

```
            INP R1, 2   // max. score
            INP R2, 2   // score
            MUL R2, #10 // score*10
            DIV R2, R1  // score*10/max
            CMP R2, #6  // compare to 6
            BGE sufficient
            MOV R3, #79
            OUT R3, 7
            BRA end
sufficient: 
```

**8. Instructies indien waar**
    
Als het cijfer minimaal 6 was, printen we een V (ASCII: `0x56` oftewel 86):

```
            INP R1, 2   // max. score
            INP R2, 2   // score
            MUL R2, #10 // score*10
            DIV R2, R1  // score*10/max
            CMP R2, #6  // compare to 6
            BGE sufficient
            MOV R3, #79
            OUT R3, 7
            BRA end
sufficient: MOV R3, #86
            OUT R3, 7
```

**9. Label aan het einde**
    
Ten slotte zetten we het label `end` neer, zodat de program counter daarheen springt aan het einde van het else-gedeelte:

```
            INP R1, 2   // max. score
            INP R2, 2   // score
            MUL R2, #10 // score*10
            DIV R2, R1  // score*10/max
            CMP R2, #6  // compare to 6
            BGE sufficient
            MOV R3, #79
            OUT R3, 7
            BRA end
sufficient: MOV R3, #86
            OUT R3, 7
end:        HLT
```

Dan is het nu weer tijd om er zelf mee te oefenen! Bij deze oefeningen heb je de [ASCII-tabel](https://stackoverflow.com/questions/7856930/mips-assembly-string-ascii-instructions) weer nodig, daarom staat deze hieronder. 

```{figure} assets/ASCII_table.png
---
align: center
---
```

:::{exercise} Kluis met pincode
Schrijf een programma in de [RISC-simulator](http://peterhigginson.co.uk/RISC/) dat controleert of de gebruiker de juiste pincode invoert.

1. Maak een variabele pincode aan in de datasectie.
2. Zorg ervoor dat er om een pincode gevraagd wordt in de tekstsectie (je hoeft hiervoor geen tekst te printen, maar als je het leuk vindt mag dat natuurlijk wel).
3. Laat de pincode controleren door een keuzestructuur. Als de pincode goed is, schrijf je “OK” naar de output. Als het fout is, schrijf je “WRONG PIN” naar de output.
4. Laat de tekstsectie eindeloos herhalen.
:::

:::{exercise} Cijfer berekenen
Als je een cijfer hebt gehaald, is het wel interessant om te weten of het een 6,2 is of een 6,8. In deze oefening schrijf je een programma in de [RISC-simulator](http://peterhigginson.co.uk/RISC/) dat een cijfer berekent met één cijfer achter de komma. Dat doe je aan de hand van het maximum aantal punten en het aantal  behaalde punten.

1. Begin je programma met twee `INP`-instructies, net zoals in het uitgewerkte voorbeeld hierboven.
2. Bereken het cijfer eerst op schaal van 0 tot 100 (kijk ook weer naar het uitgewerkte voorbeeld, daar wordt het op schaal van 0 tot 10 berekend).
3.  Print nu drie ASCII-karakters: het cijfer (0-100) gedeeld door 10, een komma, en het cijfer modulo 10 (`MOD`-instructie).
    
    *Tip: de ASCII-karakters kan je berekenen door een getal op te tellen bij het cijfer dat je wilt printen. Kijk hiervoor in de ASCII-tabel!*
    
4. Wat nu als het cijfer een 10 is? Daar heb je een keuzestructuur voor nodig, omdat je dan een extra ASCII-karakter nodig hebt. Zorg ervoor dat er dan 10,0 geprint wordt.
5. Extra: de cijfers worden niet goed afgerond, reken maar eens na: 23 punten van max. 40 moet een 6,2 opleveren (6,175), maar dat wordt nu een 6,1! Los dit probleem op door het cijfer eerst op schaal van 0 tot 1000 te bepalen, daar 5 bij-op te tellen, en er dan een cijfer van 0-100 van te maken.
6. Extra: pas het programma aan, zodat het cijfer op schaal van 1 tot 10 wordt berekend (score / max * 9 + 1).
7. Extra: laat de maximale score maar één keer invoeren, en herhaal de rest van het programma totdat de score 0 wordt ingevoerd. Zo kunnen docenten makkelijk meerdere cijfers laten uitrekenen achter elkaar! (en dat score 0 het cijfer 1 oplevert, daar hebben ze hopelijk geen programma bij nodig…)
:::

:::{exercise} Boete berekenen
In 4.2 heb je ook al de boete berekent bij een snelheidsovertreding waar je 70 km/h mocht rijden. De formule daar werkte alleen bij snelheden tot 80 km/h. Boven die snelheid wordt er namelijk een andere formule gebruikt. Laad je code van die opdracht weer in de [RISC-simulator](http://peterhigginson.co.uk/RISC/).

Pas het programma nu aan zodat het ook voldoet aan de volgende eisen:

1. Als de snelheid niet wordt overtreden met meer dan 3 km/h, is de boete €0.
2. Als de snelheid (v) met maximaal 10 km/h wordt overtreden, wordt de oude formule gebruikt: $(v - 3) * 7 + 18$.
3. Als de snelheid met meer dan 10 km/h wordt overtreden, wordt de volgende formule gebruikt: $\frac{10}{51} v^2 + \frac{387}{100} v + 26$.
4. Als de snelheid met meer dan 30 km/h wordt overtreden, print het programma: “OM!”, omdat het Openbaar Ministerie de hoogte van de boete dan moet bepalen.
:::

:::{exercise} Verdiepingsopdracht 1: cijfer berekenen als S/M/V/G/U

Een cijfer kan natuurlijk meer zijn dan alleen voldoende of onvoldoende. Pas daarom het uitgewerkte voorbeeld aan, zodat de volgende letters weergegeven worden:

S: 1-3 (slecht)

M: 4-5 (matig)

V: 6 (voldoende)

G: 7-8 (goed)

U: 9-10 (uitmuntend)

Zorg ervoor dat het cijfer wordt berekend op schaal 1-10 en dat het goed afgerond wordt!
:::

## De ALU vergelijkt waarden

Om twee getallen de vergelijken, trekt de ALU ze van elkaar af. De `CMP`-instructie doet dus eigenlijk hetzelfde als de `SUB`-instructie, behalve dat de processor het resultaat niet opslaat. Het resultaat maakt namelijk niet uit; alleen de uitkomst van de vergelijking is interessant. Die uitkomst wordt door de ALU altijd in het **condition-code-register (CC-register)** opgeslagen. Dat is een speciaal register waarvan vier bits iets aangeven over het resultaat van een berekening. Zulke bits met een bepaalde betekenis worden **flags** genoemd. Die vier bits in het CC-register worden ook wel de **condition flags** genoemd. In de RISC-simulator wordt dit register weergegeven boven het IR-register, zoals je op de afbeelding hieronder ook ziet.

```{figure} assets/image-20240105011522080.png
---
align: right
---
```
De vier condition flags hebben allemaal een letter: NZCV. De betekenis van deze flags lees je hieronder. De eerste twee zijn nodig bij vergelijkingen. Daarbij worden de twee getallen die vergeleken zijn A en B genoemd. De andere twee hebben te maken met getallen die te groot of te klein zijn om op te slaan in een register. 

- N: negative. Het antwoord op de min-som is negatief. Dat betekent dat A kleiner is dan B.
- Z: zero. Het antwoord op de min-som is 0. Dat betekent dat A en B gelijk zijn.
- C: carry. Er is een extra bit nodig om het antwoord op te kunnen slaan. Bij de RISC-simulator betekent het dat het antwoord groter of gelijk is aan $2^{16}$, omdat een word daarin 16 bits is. 
*Let op:* De carry-bit wordt ook 1 als er een min-som is uitgerekend! Dit komt omdat de carry-bit er niet vanuit gaat dat er gewerkt wordt met negatieve getallen.
- V: overflow. Dit heeft ongeveer dezelfde betekenis als C, alleen wordt hierbij ook rekening gehouden met negatieve getallen.

```{list-table} Condition flags
:header-rows: 1
:width: 50%
* - flag
  - naam
  - betekenis
* - N
  - negative
  - A < B
* - Z
  - zero
  - A==B
* - C
  - carry
  - antwoord is groter dan $2^{16}-1$
* - V
  - overflow
  - antwoord is kleiner dan $-2^{15}$ of groter dan $2^{15}-1$
```

:::{exercise} Flags
Voer de code van oefening 1 nog een keer uit, en let dan op de flags.

1. Welke flag verandert er het eerste?
2. Na hoeveel herhalingen veranderen de flags?
:::

:::{exercise} Logische poorten voor flags
De flags worden bepaald door middel van logische poorten. De logische schakelingen die je nodig hebt, ga je in deze oefening maken. Gebruik als startpunt de schakeling die je eerder maakte voor een 4-bits full adder.

1. Voor welke flag denk je dat je meer logische poorten nodig hebt? De N- of de Z-flag?
2. Voeg een lampje toe aan je 4-bits adder voor de N-flag en zorg dat het lampje brandt als de uitkomst van een berekening negatief is.
3. Voeg een lampje toe aan je 4-bits adder voor de Z-flag en zorg dat het lampje brandt als de uitkomst van een berekening nul is.
4. Wat is het verschil tussen de N-flag en de C-flag?
5. Voeg een lampje toe aan je 4-bits adder voor de C-flag en zorg dat het lampje brandt als de uitkomst van een berekening groter dan vier bits is.

**Verdiepingsopdrachten:**

6. Zou je voor de Z-flag minder transistoren kunnen gebruiken als je niet alleen logische poorten hoeft te gebruiken? Indien ja, hoe kan je dat dan doen?

7. Maak de logische schakeling voor de V-flag.

8. Wat is het verschil tussen de C-flag en de V-flag?
:::

:::{exercise} Flags (verdieping)
Voorspel de waarde van de flags bij alle onderstaande berekeningen. Test je antwoord met de RISC-simulator. 

```{admonition} Hint
:class: drop-down
Gebruik voor het testen van 1 t/m 4 alleen
- een `MOV`-instructie; en
- een `MUL`/`SUB`-instructie.
```
1. 0 * 3
2. 5 - 4
3. 5 - 5
4. 255 * 255
5. 256 * 256

```{admonition} Hint
:class: drop-down
Let op: omdat dit 9-bits getallen zijn kan het niet als waarde in een imm8-veld meegegeven worden! Daarom heb je hiervoor de DAT-instructie nodig.
```
:::

:::{exercise} Carry en Overflow (verdieping)
Voer onderstaande berekeningen uit in de RISC-simulator. Doe dat in twee modussen:

1. Signed (standaard)
2. Unsigned (te selecteren bij “OPTIONS” in de simulator)

Verklaar voor iedere som wat de waarde is van de C-flag en de V-flag.

```{admonition} Hint
:class: drop-down
Gebruik voor het testen van a t/m c alleen:

- een `LDR`-instructie;
- een `ADD`- of `SUB`-instructie; en
- een `DAT`-instructie.
```

1. 65535 + 1
2. -32768 - 1
3. 32767 + 1
:::

In een programma wil je natuurlijk meer opties hebben dan alleen `A<B` en `A==B`. Gelukkig is dat mogelijk door de flags N en Z te combineren. A<=B betekent namelijk dat één van beide flags 1 moet zijn: $N\vee Z$. Op dezelfde manier kan je ook bepalen of A>=B geldt: dan moet N 0 zijn ($\neg N$) óf Z 1. In logische formules staan alle mogelijkheden in de tabel hieronder weergegeven.

```{list-table}
:header-rows: 1
:width: 50%
* - conditie
  - flags
* - geen
  - 
* - $A=B$
  - $Z$
* - $A\neq B$
  - $\neg Z$
* - $A < B$
  - $N$
* - $A > B$
  - $\neg N \wedge \neg Z$
* - $A\leq B$
  - $N \vee Z$
* - $A\geq B$
  - $\neg N \vee Z$
```
