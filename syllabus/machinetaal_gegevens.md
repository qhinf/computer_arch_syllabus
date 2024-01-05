(hoofdstuk-gegevens)=
# Machinetaal - Gegevens verwerken

Tot nu toe zagen we alleen hoe je instructies kan uitvoeren die werken met een register en een vaste waarde. Dat zijn namelijk de twee velden die formaat A heeft. Daarbij hebben we alleen gelet op berekeningen. Een processor kan veel meer dan dat. Daarom ga je in deze paragraaf nieuwe instructieformaten ontdekken waarmee je gegevens kan verwerken. Daarnaast leer je ook hoe je gebruik kan maken van het geheugen en van in- en uitvoer zodat je de gegevens ook kan verplaatsen naar andere plekken dan alleen registers.

## Geheugen gebruiken

In assembly-code kan je niet alleen instructies zetten, maar ook gewone getallen. Zoals eerder gezegd, zijn instructies in de [RISC-simulator](http://peterhigginson.co.uk/RISC/) één woord lang. Bij de RISC-simulator is dat 16 bits. Omdat een instructie altijd uit meerdere velden bestaat, kan een argument nooit 16 bits lang zijn. Er moet namelijk minstens een opcode-veld voor de argumenten staan. De processor kan wel rekenen met getallen van 16 bits. Daarom is er een manier om, naast de instructies, ook getallen van 16 bits in een programma op te nemen.

Om een waarde in te voeren gebruik je de mnemonic `DAT`. Dat is het enige mnemonic die niet vertaald wordt in een opcode. Het geeft juist aan dat er alleen een waarde neergezet moet worden op die plaats in het geheugen. Een waarde kan je decimaal of hexadecimaal intypen. Om aan te geven dat een getal hexadecimaal is, zet je er `0x` voor. `DAT 10` doet dus precies hetzelfde als `DAT 0xA`.

Het is goed om te weten dat de processor niet zelf kan bedenken wat jij hebt bedoeld met de bits van een programma. De processor ziet niet of er `DAT` voor stond in assembly, of niet. Sommige woorden (rijen van bits) zal de processor interpreteren als een instructie, en andere zal hij gebruiken als een gewoon getal. Hoe zorg je ervoor dat een getal niet per ongeluk wordt uitgevoerd alsof het een instructie is? In de volgende oefening denk je daarover na.

:::{exercise} Betekenis van bits

Zet het onderstaande programma in de [RISC-simulator](http://peterhigginson.co.uk/RISC/) (nog niet uitvoeren!).

```
DAT 0x294d
DAT 4365
DAT 0
DAT 0x5104
```

Maak nu de onderstaande opdrachten.

a. Wat denk je dat er gebeurt als je dit programma uitvoert?

b. Voer het programma nu uit. Klopte je voorspelling? Wat is er anders?

c. Schrijf `0x294D` eens op als binair getal. Wat is de betekenis hiervan als je het leest als machinecode (zie formaat A in 4.1)?

d. Wat doet de processor met`DAT 0x294D`?

   *Tip: gebruik de STEP-knop in de simulator om de instructies één voor één uit te voeren.*

e. Wat doet de processor met `DAT 4365`?

f. Wat doet de processor met`DAT 0`?

g. Wat doet de processor met `DAT 0x5104`?

h. Schrijf het programma nu op in assembly.
:::

Zoals je nu gezien hebt, hoeft de computer helemaal niet te weten wat de betekenis van een rij bits is. Als de program counter naar een geheugencel wijst, dan wordt wat daar staat geïnterpreteerd als instructie. Als het een gewoon getal is, dan moet je een instructie gebruiken die de waarde uit het geheugen haalt, om er vervolgens bijvoorbeeld een berekening mee uit te voeren. Uiteindelijk bepaal jij dus wat er gebeuren moet met een rij bits in het geheugen.

```{figure} assets/image-20231224000454723.png
---
scale: 40%
align: right
---
```

Een programma moet altijd beginnen met instructies. Dat komt omdat de program counter begint bij 0. Na iedere instructie wordt daar 1 bij opgeteld. Daarom moet een programma altijd beginnen met de instructies die uitgevoerd moeten worden. Daartussen kan je niet zomaar een waarde neerzetten die je niet als instructie bedoelt, want de control unit van de processor kan niet bepalen of iets een getal of een instructie is.

Het eerste gedeelte van een programma bestaat dus uit alleen instructies. Dat noemen we de **tekstsectie**. Die eindigt met de instructie `HLT` (halt). Daarachter kan je alle waarden neerzetten die in het programma gebruikt worden. Dat noemen we de **datasectie**.

Bij de meeste instructies kan je als argumenten alleen immediates of registers gebruiken. Waarden uit het geheugen moeten daarom eerst in een register worden gezet. Daarvoor gebruik je de instructie LDR. Dat staat voor **L**oa**D R**egister. Een waarde uit een register kan ook weer opgeslagen worden in de datasectie. Daarvoor is de instructie STR. Dat staat voor **ST**ore **R**egister.

```{list-table} Instructies voor geheugengebruik
:header-rows: 1
:width: 80%
:align: center
* - Instructie
  - Betekenis
* - `DAT` word
  - Voeg een 16-bits waarde (word) in op deze plek in het programma
* - `LDR` Rd, direct
  - Laad de waarde voor een register uit het geheugen
* - `STR` Rd, direct
  - Schrijf een waarde van een register in het geheugen
```

Beide instructies hebben twee argumenten: een register en een geheugenadres. Dat is ook te zien in de tabel hiernaast. Rd en Rs zijn het doel- en bronregister (d: destination en s: source). Het argument direct is een **direct geheugenadres** waar de waarde vandaan komt of naartoe moet. Dit wordt “direct” genoemd, omdat je op deze manier iedere mogelijke geheugenplaats kan adresseren, in tegenstelling tot indirecte adressen. Bij een **indirect adres** geef je alleen aan hoeveel geheugenplaatsen je naar voren of naar achteren moet ten opzichte van een gegeven direct adres. Hier gaan we nu niet verder op in, omdat je indirecte adressen pas nodig hebt bij het verdiepingsonderdeel “Strings printen” dat later aan de orde komt.

:::{exercise} Gegevens opslaan in het geheugen
In deze kleine opdracht ga je een stuk code aanpassen. Bekijk de onderstaande code

```
DAT 7
LDR R1, 0
ADD R1, #23
STR R1, 0
HLT
```

a. Wat denk je dat er gebeurt als je dit programma uitvoert? Geef per instructie een omschrijving.

b. Voer het programma nu uit. Klopte je voorspelling? Wat is er anders?

c. Pas het programma zodanig aan dat de regel met `DAT 7` helemaal onderaan komt te staan en het resultaat hetzelfde blijft, wanneer je het programma uitvoert. *Hint* je zult iets aan de directe adressering moeten veranderen.
:::

Zoals gezegd gebruiken de meeste instructies alleen immediates of registers als argumenten. Je hebt tot nu toe alleen instructies gezien die een register en een immediate gebruiken. Dat zijn de instructies van formaat A. Er zijn ook andere formaten, waarbij je bijvoorbeeld **twee registers als argumenten** moet opgeven, bijvoorbeeld `ADD R1, R2`. Bij die instructie wordt de waarde die in register `R2` staat opgeteld bij de waarde die in register `R1` staat. Hieronder zie je een lijstje met instructies die je ook kan gebruiken.

```{list-table} Instructies met registers als argumenten
:header-rows: 1
:width: 80%
:align: center
* - Instructie
  - Betekenis
* - `ADD` Rsd, Rb
  - Rsd = Rsd + Rb
* - `SUB` Rsd, Rb
  - Rsd = Rsd - Rb
* - `AND` Rsd, Rb
  - Rsd = Rsd && Rb
* - `ORR` Rsd, Rb
  - Rsd = Rsd || Rb
* - `MOD` Rsd, Rb
  - Rsd = Rsd % Rb
* - `CMP` Rd, Rb
  - Rd == Rb
* - `MOV` Rd, Rb
  - Rd = Rb (*Toewijzing*)
* - `MUL` Rsd, Rb
  - Rsd = Rsd * Rb
* - `DIV` Rsd, Rb
  - Rsd = Rsd / Rb
```

Om een beeld te krijgen van hoe een programma eruit ziet met een datasectie, maak je de onderstaande oefening.

`````{exercise} Datasectie gebruiken.
In deze opdracht schrijf je sommen op in assembly. Gebruik daarvoor de getallen die al klaar staan in de datasectie in de onderstaande code. Je kan je antwoord testen in de [RISC-simulator](http://peterhigginson.co.uk/RISC/).

```
// zet hieronder de instructies neer die de berekeningen uitvoeren
// hier komt opdracht a
// hier komt opdracht b
// hier komt opdracht c
// hier komt opdracht d
// hier komt opdracht e
// hier komt opdracht f
STR R1, // voer hier het adres voor het resultaat in
HLT // start van de datasectie
DAT 7  // getal P
DAT 2  // getal Q
DAT 11 // getal R
DAT 45 // getal S
DAT 0 // de plek voor antwoord A
DAT 0 // de plek voor antwoord B
DAT 0 // de plek voor antwoord C
```
In de tekstsectie laad je beide waarden uit de datasectie, voert de berekening daarmee uit, en schrijft het daarna weg op de juiste plek in de datasectie. De eerste is al voorgedaan. Volg dit voorbeeld bij de andere opgaven.

a. P * Q

````{admonition} Uitwerking
:class: dropdown

```
LDR R1, 5 // laad de eerste operand (getal P) in R1
LDR R2, 6 // laad de tweede operand (getal Q) in R2
MUL R1, R2 // opdracht a
STR R1, 9 // opslaan van antwoord A in het geheugen
// hier komt opdracht b
// hier komt opdracht c
HLT // start van de datasectie
DAT 7  // getal P (regel 5)
DAT 2  // getal Q (regel 6)
DAT 11 // getal R
DAT 45 // getal S
DAT 0 // de plek voor antwoord A (regel 9)
DAT 0 // de plek voor antwoord B
DAT 0 // de plek voor antwoord C
```
Test of het werkt: komt er inderdaad 14 te staan op de plek voor antwoord a?
````

b. P * Q + R

  **Let op**: vergeet niet te testen nadat je deze opdracht hebt toegevoegd. Gaat opdracht a dan ook nog goed?

````{admonition} Uitleg
:class: dropdown
De getallen die nu in R1 en R2 geladen worden, zijn niet 7 en 2 maar heel andere getallen. Hoe komt dat?

Omdat je nieuwe regels in de tekst-sectie hebt toegevoegd, is de datasectie opgeschoven. Daarom moet je de adressen 5 en 6 veranderen naar de adressen waar de getallen 7 en 2 nu in het geheugen staan.

Waarschijnlijk heb je drie instructies toegevoegd, dus kan je het oplossen door 5 te veranderen in 8 en 6 te veranderen in 9.

Let op dat je ook het adres van antwoord A aanpast, want anders overschrijf je een getal dat daar al staat. Adres 9 is nu namelijk de plek waar getal Q staat.
````

````{admonition} Uitwerking (als je er echt niet uitkomt)
:class: dropdown
```
LDR R1, 8 // laad de eerste operand (getal P) in R1
LDR R2, 9 // laad de tweede operand (getal Q) in R2
MUL R1, R2 // opdracht a
STR R1, 12 // opslaan van antwoord A in het geheugen
LDR R2, 10 // laad de tweede operand (getal R) in R2 
ADD R1, R2  // opdracht b
STR R1, 13 // opslaan van antwoord B in het geheugen
// hier komt opdracht b
// hier komt opdracht c
HLT // start van de datasectie
DAT 7  // getal P (regel 8)
DAT 2  // getal Q (regel 9)
DAT 11 // getal R (regel 10)
DAT 45 // getal S
DAT 0 // de plek voor antwoord A (regel 12)
DAT 0 // de plek voor antwoord B (regel 13)
DAT 0 // de plek voor antwoord C
```
Opmerking: het antwoord van a staat nog in R1, dus dat hoef je niet opnieuw uit het geheugen te laden.
````

c. S - (P * Q + R)
`````

Zoals je hebt gemerkt, is het lastig dat je van tevoren niet weet wat het adres van een waard in de datasectie is. Dat komt omdat je nog niet kan voorspellen hoe lang het programma wordt. Bovendien moet je op deze manier ook alle adressen aanpassen als je ook maar één instructie toevoegt of verwijdert. Gelukkig is daar een oplossing voor: **labels**

## Geheugenplaatsen labelen

In plaats van een nummer te gebruiken als adres, kan je ook een label gebruiken in ASM. De assembler zal dat label dan voor je vertalen naar het juiste geheugenadres. De naam van een label kan je overal invullen in een programma waar je anders een adres zou neerzetten. Daarvoor moet een label natuurlijk wel bekend zijn bij de assembler.

Het definiëren van een label doe je door een identifier met een dubbele punt voor een DAT-instructie te zetten, bijvoorbeeld zo: `item1: DAT 8`. Een **identifier** is een tekenreeks die alleen bestaat uit letters, cijfers of lage streepjes. Een identifier mag niet beginnen met een getal. Voor labels gelden dus dezelfde regels als voor namen van functies en variabelen in hogere programmeertalen.

### Gebruik van variabelen

Als een compiler variabelen tegenkomt in een hogere programmeertaal, dan vertaalt die ze met behulp van labels naar Assembly. Een declaratie wordt vertaald naar een `DAT`-instructie in de datasectie met daarbij de naam van de variabele (een identifier) als label. Soms wordt het label nog ingekort, omdat sommige assemblers een maximaal toegestane lengte hanteren voor labels.

Een toewijzing naar de waarde van een variabele wordt vertaald naar één of meerdere instructies waarin het gebruikte label terugkomt. Een (stukje van een) eenvoudig programma staat hieronder weergegeven. Let alleen wel op dat de declaraties bij een hogere programmeertaal vaak *bovenaan* staan, maar in assembly juist *onderaan*. Bekijk de code eens goed, en lees daarna de uitleg die eronder staat.

Opmerking: Een // staat voor een commentaar. De karakters *achter* de // worden door de compiler, interpreter of assembler genegeerd. Zo kun je handig toelichting geven op je code, zodat deze leesbaarder wordt. Er staat soms “`// ...`”. Dat betekent dat  er meer regels code nodig zijn om een werkend programma te maken, maar dat die  weggelaten zijn omdat ze het voorbeeld anders te lang zouden maken. Bekijk onderstaand programma in een 'hogere' programmeertaal, in dit geval C.

**Hogere programmeertaal**

```c
// declarations
int speed;
int x = 100;

// program code
// ...
speed = 5;

// ...
// increase speed with 2
speed = speed + 2; 

// ...
// increase x with the speed
x = x + speed;

// ...

```

En bekijk nu hetzelfde programma in assembly.

**Assembly**

```
// textsection
// ...
MOV R1, #5
STR R1, speed
// ...
LDR R1, speed
ADD R1, #2
STR R1, speed
// ...
LDR R1, x
LDR R2, speed
ADD R1, R2
STR R1, x
// ...
HLT

// datasection
speed: DAT 0
x:     DAT 100
```

Je ziet in het bovenstaande voorbeeld dat een declaratie vertaald wordt naar ASM door in de datasectie een DAT-instructie met een label neer te zetten. De toewijzingen hebben twee of drie instructies nodig in de vertaling naar ASM. Als de waarde van een variabele gewijzigd wordt ten opzichte van zijn vorige waarde, moet eerst de huidige (oude) waarde opgehaald worden (`LDR`), dan moet de berekening worden uitgevoerd (in dit voorbeeld beide keren `ADD`) en ten slotte moet de waarde weer teruggeschreven worden naar het geheugen (`STR`).



:::{exercise} Variabelen vertalen naar assembly

Vertaal de onderstaande stukjes code van een hogere programmeertaal naar assembly. Probeer zo min mogelijk coderegels te gebruiken. Test je antwoord in de [RISC-simulator](http://peterhigginson.co.uk/RISC/).

a. Helderheid van een scherm verhogen:

```C
int brightness = 50;
// ...
brightness = brightness + 10;	
```

b. Geld pinnen:

```C
int balance = 100;
int amount = 15;
// ...
balance = balance - amount;
```

c. Zwaartekracht

```C
int y = 34;
int speed_y = 0;
int gravity = 2;
// ...
speed_y = speed_y - gravity; // maak de verticale snelheid lager
y = y + speed_y; // verplaats de speler naar zijn nieuwe y-positie
```
:::


## In- en uitvoer

Het is van belang dat een gebruiker ook informatie kan invoeren in een programma. Dat gebeurt vaak met een muis of toetsenbord. Ook moet het mogelijk zijn om informatie weer te geven, want een gebruiker heeft er niets aan als het antwoord van een som in een register of in het geheugen staat: dat is voor hem niet zichtbaar namelijk. Er zijn dus manieren nodig waarop de processor invoer en uitvoer kan gebruiken.

In de [RISC-simulator](http://peterhigginson.co.uk/RISC/) kan je alleen getallen invoeren. Dat kan in het invoer-veld waar INPUT bij staat. Ook de uitvoer is beperkt. Daarvoor is een klein schermpje beschikbaar waarop je getallen of andere tekens uit kan voeren. Deze plekken hebben ook een adres gekregen, omdat er in theorie meerdere apparaten aangesloten zouden kunnen zijn. Het adres van het invoerveld is 2, het adres van het uitvoerveld is 4. Het uitvoerveld heeft nog twee andere adressen omdat je zo de mogelijkheid hebt om een getal niet als decimaal getal te laten printen, maar op een andere manier weer te geven. Daarvoor gebruik je dus een ander adres. Om de opgegeven waarde als hexadecimaal getal weer te geven, gebruik je adres 6. Om de waarde weer te geven als ASCII-karakter, gebruik je adres 7. In de tabel hieronder zie je de ASM-codes die je kan gebruiken. Hieronder zie je de beschikbare instructies nog eens samengevat staan.



| mnemonic          | betekenis                                                    |
| ----------------- | ------------------------------------------------------------ |
| `INP` Rd, address | Vraag de gebruiker een getal in te voeren in het Input-veld van de simulator. De waarde wordt vervolgens opgeslagen in register Rd. Address is altijd 2, dus: `INP` Rd `, 2` |
| `OUT` Rd, address | Laat een waarde aan de gebruiker zien. Address kan de volgende waarden hebben:<br />4: een gewoon getal<br />6: hexadecimaal getal<br />7: ASCII-karakter |

De ASCII-karakters die je kan printen naar adres 7, staan in de [ASCII-tabel](https://stackoverflow.com/questions/7856930/mips-assembly-string-ascii-instructions). Die staat hieronder. Het eerste hexadecimale teken geeft de rij aan, het tweede de kolom. Zo kom je bijvoorbeeld met 0x48 (rij 4, kolom 8) uit bij de hoofdletter H.

```{figure} assets/image-20231226011026874.png
---
align: center
---
```

In de volgende oefeningen ga je zelf gebruikmaken van de input en output.

`````{exercise} output
In deze opdracht ga je oefenen met waarden naar de output schrijven. Daarvoor moet je ervoor zorgen dat het juiste getal in een register staat. Vervolgens schrijf je het weg naar de output op adres 4, 6 of 7 (zie de tabel hierboven). De eerste opdracht is weer voorgedaan, volg dat voorbeeld. Gebruik de [RISC-simulator](http://peterhigginson.co.uk/RISC/) om je antwoord te testen.

a. Print de letter C.
````{admonition} Uitwerking
:class: dropdown
```
MOV R1, #0x43
OUT R1, 7
HLT
```
````    
b. Print 135 als decimaal getal.
    
   *Hint:* gebruik adres 4 in plaats van 7 bij de OUT-instructie.
    
c. Print 16 als hexadecimaal getal.

d. Print 0x19 als decimaal getal.

e. Print de letters “elf”.

f. Print de som 4+8=, met op de volgende regel het antwoord (als decimaal getal).
    
*Opmerking:* laat de computer de som uitrekenen met de ADD-instructie.
`````

:::{exercise} input verwerken

Maak een programma dat de ASCII-letter print die hoort bij het getal dat ingevoerd wordt bij de input. Als je bijvoorbeeld 65 geeft als input, print het programma de kleine letter a.

Test de antwoord in de [RISC-simulator](http://peterhigginson.co.uk/RISC/).

*Hint:* je hebt maar twee instructies nodig (als je HLT niet meerekent).

:::

:::{exercise} getal binair printen

Maak een programma in de [RISC-simulator](http://peterhigginson.co.uk/RISC/) dat een getal van de invoer print als een binaire reeks. Voldoe aan de volgende eisen:

1. Het programma werkt voor getallen van 4 bits.
2. Door een 0 of 1 te printen als ASCII-karakter, staat de reeks op 1 regel.

Als je dus 12 invoert, is de uitvoer 1100.

Volg deze stappen:

1. Laad een getal van de input in register `R1`.

2. Bepaal de waarde van de vierde bit vanaf rechts en print dit getal naar de output als decimaal getal. Zet hiervoor de waarde van de huidige bit in register `R2` (de vierde bit heeft waarde $2^3=8$).

   *Tip:* om te kijken of een bit aan moet staan, kan je het getal delen door de getalswaarde van die bit (8 voor de eerste bit). Merk op dat de RISC-processor niet met kommagetallen werkt, dus dat alles achter de komma wegvalt. 12 / 8 is dus 1.

   *Let op:* De `DIV`-instructie accepteert geen immediates, maar alleen twee registers. `DIV R2, #8` **werkt dus niet**. Je kan `DIV` alleen gebruiken in de volgende vorm: `DIV Rsd, Rb`. Je moet dus het tweede argument ook in een register zetten. Dat kan met de `MOV`-instructie. Zet daarmee het getal 8 in `R2`.

3. Wat gebeurt er als je een getal van meer dan 4 bits invoert? Bijvoorbeeld: 28 (dat is 11100). Test het in de [RISC-simulator](http://peterhigginson.co.uk/RISC/).

4. Zorg ervoor dat het getal dat geprint wordt maximaal 1 kan zijn (dan is het dus 1 of 0).

   *Tip:* met de instructie `AND` kan je bits uitzetten. Dat werkt als een AND-poort voor getallen van 16 bits. In het antwoord wordt een bit 1 als de bit in beide getallen op die plek 1 is, en anders komt er een 0 te staan.

   Test onderstaand programmaatje in de [RISC-simulator](http://peterhigginson.co.uk/RISC/). Test het daarna nog eens, maar dan met `#6` in `R1`.

   ```nasm
   MOV R1, #3 //  3 = 0011
   AND R1, #1 // 0011 and 0001 = 0001
   OUT R1, 4  // print als decimaal getal
   ```

5. Zorg ervoor dat de bit als ASCII-karakter ‘0’ of ‘1’ wordt geprint. Daarvoor moet je het getal 0 of 1 omrekenen naar de ASCII-code voor ‘0’ of ‘1’. Welk getal moet je er dan bij optellen? (oftewel: wat is het verschil tussen het getal 0 of 1 en het getal dat in de ASCII-tabel hoort bij ‘0’ of ‘1’?)

6. Als we nu de volgende bit (bit 3 vanaf rechts) willen bepalen, hebben we het originele getal weer nodig. Zet daarom dat getal (uit `R1`) eerst in register `R3` voordat je er allerlei berekeningen mee doet.

7. Zet de waarde van register `R1` weer terug op de beginwaarde (die nu in `R3` staat).

8. Voordat je de waarde van de derde bit kan bepalen, moet je eerst berekenen wat zijn getalswaarde is. Bereken dus de getalswaarde van bit 3 door `R2` te delen door 4.

9. Bepaal de waarde van de derde bit vanaf rechts en print dit getal ook als ASCII-karakter. Kopieer en plak hiervoor de regels code die je kan hergebruiken van de vierde bit (vanaf rechts).

   *Tip:* voor deze stap hoef je geen nieuwe regels code te schrijven.

10. Print op dezelfde manier ook de bits met waarde 2 en 1.

11. Zijn er nog regels code die je kan weglaten? Kijk vooral goed naar de berekening van de laatste bit.

:::

:::{exercise} snelheidsboete berekenen

Maak een programma in de [RISC-simulator](http://peterhigginson.co.uk/RISC/) dat berekent hoe hoog de boete bij een snelheidsoverschrijding. Voldoe aan de volgende eisen:

1. De enige invoer is de gemeten snelheid in km/h.
2. De maximale snelheid is 70 km/h.
3. De snelheid wordt gemeten op een traject waar werkzaamheden zijn.
4. De gemeten snelheid wordt verminderd met 3 km/h. Dit is de gecorrigeerde snelheid.
5. De formule voor de boete is: km-te-hard * 7 + 18 (dit een benadering voor boetes tot 10 km/h te hard. Zoek de huidige boetetarieven maar eens op en kijk hoe vaak het antwoord klopt)
6. De administratiekosten worden er ook bij opgeteld.
7. Het totale bedrag wordt geprint (dit kan een negatief bedrag zijn, bijvoorbeeld als de snelheid 67 was).

:::

## Meer instructieformaten in machinetaal

We gaan nu weer kijken naar machinetaal. Tot nu toe hebben we alleen gelet op “formaat A”. Daarmee kon je bewerkingen uitvoeren op één register (Rsd) met een vast getal (imm8: immediate van 8 bits lang). Hieronder zie je het schema van formaat A nog eens staan.

```{figure} assets/image-20231229162951996.png
---
align: center
---
Instructieformaat A
```

Misschien is het je toen al opgevallen: alle instructies van formaat A beginnen met 00. De eerste twee bits geven bij de RISC-simulator aan welk formaat er gebruikt wordt. De instructieformaten zijn namelijk opgedeeld in vier **instructieformaatgroepen** (00, 01, 10 en 11): groep A, B, C en D. Formaatgroep A bestaat uit maar 1 formaat, maar de andere groepen bestaan uit meerdere formaten (groep B zelfs uit 12 formaten: B1 t/m B12). Nu gaan we op een paar van die formaten wat dieper in.

Eerst kijken we nog even naar formaat A. Alle instructies van dat formaat beginnen dus met 00. De drie bits achter die groepscode geven bij formaat A aan welke operatie er precies uitgevoerd moest worden. Het op-veld moet dus eigenlijk nog in tweeën gesplitst worden qua betekenis:

```{figure} assets/image-20231229163037996.png
---
align: center
---
Instructieformaat A
```

Het eerste veld geeft aan in welke groep het instructieformaat valt. Daarom wordt dat veld **op_g** genoemd: **opcode group**. Het tweede veld, **func**, geeft de functie aan die we gebruiken. Dat veld geeft aan wat de ALU te doen staat. Bij een enkele func-waarde is dat helemaal niets, bijvoorbeeld bij MOV (func is dan 101): dan hoeft er alleen een waarde in een register geladen te worden, zonder dat daar iets mee berekend wordt.

We zullen nu twee andere formaten bekijken uit groep B (op_g == 01). Die groep bestaat uit twaalf sub-formaten. Dat betekent dat er verschillende velden nodig zijn voor de operations die met formaatgroep B kunnen worden genoteerd. We zullen nu alleen naar instructieformaten B1, B3 en B10 kijken. Alle overige formaten zijn te vinden in de [lijst van instructieformaten](assets/RISC-Simulator-Instruction-Set.pdf).

```{figure} assets/image-20231229163137878.png
---
align: center
---
Instructieformaat B1
```

```{figure} assets/image-20231229163145491.png
---
align: center
---
Instructieformaat B3
```

```{figure} assets/image-20231229163153668.png
---
align: center
---
Instructieformaat B3
```

In deze drie formaten zie je nog en nieuwe veldnaam: **op_f**. Dat veld vormt samen met op_g de opcode. Zoals eerder gezegd, geeft op_g de groep aan. op_f geeft het instructieformaat binnen die groep aan. Als je de opcode weet (op_g en op_f), dan weet je ook de verdeling van de overige bits van de instructie in velden.

Met deze nieuwe formaten is het mogelijk om berekeningen te doen waarbij beide waarden uit registers komen. Het register voor het eerste argument wordt Rs genoemd (source), net als in formaat A. Het register voor het tweede argument wordt Rb genoemd (base). In assembly noteer je dat zoals in de tabel hieronder is weergegeven.

|Mnemonic|A(00)/B1(010) | B3 (0110) | B10 (011101) | opmerking |
|-|-|-|-|-|
|`ADD` |  Fun: 010(A)<br />`ADD` Rsd, #imm8| func:000<br />`ADD` Rd, Rs, Rb | | `ADD` Rsd, Rb, wordt door de assembler vertaald als `ADD` Rsd, Rsd, Rb |
|`SUB` | func: 011(A)<br />`SUB` Rsd, #imm8 | func: 001<br />`SUB` Rd, Rs, Rb | | `SUB` Rsd, Rb wordt door de assembler vertaald als `SUB` Rsd, Rsd, Rb |
|`MUL` | func: 011(A)<br />`MUL` Rsd, #imm8 |  | func: 1111<br />`MUL` Rsd, Rb |  |
|`DIV` |  |  | func: 0101<br />`DIV` Rsd, Rb |  |
|`MOD` | func: 001(A)<br />`MOD` Rsd, #imm8 |  | func: 0001<br />`MOD` Rsd, Rb |  |
|`CMP` | func: 100(A)<br />`CMP` Rsd, #imm8 |  | func: 1010<br />`CMP` Rs, Rb |  |
|`MOV` | func: 101(A)<br />`MOV` Rd, #imm8 |  | func: 1100<br />`MOV` Rd, Rs |  |
|`NEG` |  |  | func: 0111<br />`NEG` Rd, Rs | Negativate: maakt het getal negatief, alsof je het vermenigvuldigd met -1. 2's complement-notatie |
|`AND` | func: 110(A)<br />`AND` Rsd, #imm8 | func: 010<br />`AND` Rd, Rs, Rb |  | `AND` Rsd, Rb wordt door de assembler vertaald als `AND` Rsd, Rsd, Rb |
|`OR` | func: 111(A)<br />`OR` Rsd, #imm8 | func: 010<br />`OR` Rd, Rs, Rb |  | `OR` Rsd, Rb wordt door de assembler vertaald als `OR` Rsd, Rsd, Rb |

:::{exercise} Handmatig assembleren
In deze oefening doe je het werk dat een assembler normaalgesproken voor je doet: assembleren. Vertaal de onderstaande opdrachten naar machinetaal. Bepaal daarvoor eerst welk formaat je hiervoor gebruikt (A/B1/B3/B10). De eerste opdracht is voorgedaan. Volg dat voorbeeld in je eigen uitwerkingen.

a. `ADD` R3, R5
    
```{admonition} Uitwerking
:class: dropdown
    
Formaat: B3, op = 0110

*Opmerking: ADD R3, R5 wordt door de assembler vertaald naar ADD R3, R3, R5 in machinetaal, omdat er geen B10-variant van de ADD-instructie is*

Functie: `ADD`, func = 000
    
Argumenten: Rd = 011 (3), Rs = 011 (3), Rb = 101 (5)
    
Instructie: `0110 000 011 011 101`
```
b. `MUL` R7, R2

c. `MOD` R4, #5

d. `SUB` R3, R2, R1

e. `MUL` R7, #2

f. `AND` R3, R7
:::