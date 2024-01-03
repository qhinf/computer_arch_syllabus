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
