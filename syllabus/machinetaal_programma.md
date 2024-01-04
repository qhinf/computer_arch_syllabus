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
