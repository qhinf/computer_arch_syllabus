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

## In- en uitvoer