# Logische schakelingen

Q-highschool / Computerarchitectuur / Bijeenkomst 2

***

## Terugblik

Vorige week heb je geleerd:

- Binair rekenen (omrekenen tussen binair en decimaal)
- Binaire getallen optellen
- Negatieve getallen (twee-complement)
- Hexadecimale notatie

Notes:

Duur: 2-3 minuten. Kort terugblikken op vorige les.

---

## Vandaag

**De vraag:** Hoe rekent een computer eigenlijk met binaire getallen?

**Het antwoord:** Met logische schakelingen!

Vandaag ga je zelf schakelingen bouwen die kunnen rekenen.

Notes:

Duur: 2 minuten. Maak de leerlingen nieuwsgierig.

***

## Van bits naar schakelingen

Een bit is **aan** (1) of **uit** (0)

In een computer zijn dit elektrische signalen:
- **Stroom** = 1
- **Geen stroom** = 0

Met schakelingen kun je deze signalen combineren en transformeren

Notes:

Duur: 3 minuten. Leg uit dat bits fysieke elektrische signalen zijn in een computer.

---

## Logische poorten

**Logische poorten** zijn de bouwstenen van een processor

Elke poort heeft:
- Een of meer **ingangen** (inputs)
- Eén **uitgang** (output)
- Een **regel** die bepaalt wanneer de uitgang 1 is

Voorbeelden: AND, OR, XOR, NOT

Notes:

Duur: 3 minuten. Introduceer het concept van logische poorten.

***

## Tool: Logicly

Vandaag werken we met [logic.ly/demo](https://logic.ly/demo/)

Dit is een online simulator voor logische schakelingen

Je kunt:
- Poorten slepen naar het canvas
- Schakelaars en lampjes toevoegen
- Verbindingen maken met draden
- De schakeling testen

Notes:

Duur: 5 minuten. 

Laat kort op het scherm zien:
1. Hoe je een poort toevoegt
2. Hoe je een schakelaar en lampje toevoegt
3. Hoe je verbindingen maakt
4. Hoe je de simulatie start

Leerlingen gaan daarna zelf aan de slag.

***

## Werkblok 1: Basispoorten
<!-- .slide: style="font-size: 0.8em" -->

**Opdracht** (werk in tweetallen):

Bouw en test deze vier poorten:
1. AND-poort
2. OR-poort  
3. XOR-poort
4. NOT-poort

Voor elke poort:
- Sluit schakelaars aan op de ingangen
- Sluit een lampje aan op de uitgang
- Test alle mogelijke combinaties
- Maak een waarheidstabel

Notes:

Duur: 20 minuten werk + 5 minuten reflectie = 25 minuten totaal.

Loop rond en help waar nodig. Stel vragen zoals:
- "Wat verwacht je dat er gebeurt?"
- "Heb je alle combinaties geprobeerd?"
- "Zie je een patroon?"


---

## Reflectie werkblok 1

Bespreek met je groep:

- Welke poort vond je het makkelijkst te begrijpen?
- Zie je patronen in de waarheidstabellen?
- Waar zou je elke poort voor kunnen gebruiken?

Notes:

Duur: 5 minuten.

Laat een paar groepen hun bevindingen delen. Vraag naar concrete voorbeelden van wanneer ze elke poort zouden gebruiken.

***

## Werkblok 2: Samengestelde poorten
<!-- .slide: style="font-size: 0.8em" -->
**Opdracht:**

Nu ga je poorten combineren. Bouw deze drie poorten:

1. **NAND** = AND + NOT
2. **NOR** = OR + NOT  
3. **XNOR** = XOR + NOT

Voor elke poort:
- Bouw hem uit de basispoorten
- Test alle combinaties
- Maak een waarheidstabel

Notes:

Duur: 25 minuten werk + 5 minuten reflectie = 30 minuten totaal.

Dit werkblok gaat vaak sneller dan verwacht omdat leerlingen het principe nu begrijpen.

---

## Voorbeeld: NAND-poort

NAND = NOT-AND = "niet beide"

```
[A] ──┐
      ├─[AND]──[NOT]──[Output]
[B] ──┘
```

Geeft 1, **behalve** als beide ingangen 1 zijn

Notes:

Laat dit voorbeeld zien als leerlingen vastlopen. NAND-poorten zijn interessant omdat je met alleen NAND-poorten alle andere poorten kunt bouwen!

---

## Reflectie werkblok 2

Bespreek:

- Wat doet het toevoegen van een NOT-poort met het gedrag?
- Kun je patronen herkennen in de waarheidstabellen?
- Welke samengestelde poort vind je het meest logisch?

Notes:

Duur: 5 minuten.

Patroon om te bespreken: NOT-poort keert het resultaat om. NAND doet het tegenovergestelde van AND, etc.

***

## Pauze

Notes:

Duur: 10 minuten.

***

## Computers kunnen rekenen

Je hebt nu de bouwstenen gezien

Maar hoe tel je hier twee binaire getallen mee op?

**De uitdaging:** Bouw een schakeling die 1 + 1 kan rekenen

Notes:

Duur: 5 minuten intro, dan ~30 minuten werk.

---

## Probleem: 1 + 1 in binair

In binair: $1 + 1 = 10$

Dat betekent:
- De som is **0** (de rechtse bit)
- De carry (onthouden) is **1** (de linkse bit)

We hebben dus **twee outputs** nodig!

Notes:

Leg uit dat dit het kernprobleem is: bij binair optellen heb je soms een "onthoud"-bit nodig.

---

## Half-adder

Een **half-adder** telt twee bits op

Heeft:
- 2 inputs: A en B (de bits om op te tellen)
- 2 outputs: Sum (de som) en Carry (onthouden)

| A | B | Sum | Carry |
|---|---|-----|-------|
| 0 | 0 | 0   | 0     |
| 0 | 1 | 1   | 0     |
| 1 | 0 | 1   | 0     |
| 1 | 1 | 0   | 1     |

Notes:

Bekijk de tabel goed met de leerlingen. 

Sum = XOR van A en B (1 als ze verschillend zijn)
Carry = AND van A en B (1 als beide 1 zijn)

***

## Werkblok 3: Bouw een half-adder
<!-- .slide: class="voordoen" -->

**Opdracht:**

1. Bouw een half-adder met een XOR en een AND poort
2. Test alle vier de combinaties
3. Vul de waarheidstabel in
4. Controleer: komt het overeen met de tabel op de vorige slide?

**Extra uitdaging:**
- Kun je bedenken waarom dit een "**half**-adder" heet?
- Wat zou een "**full**-adder" dan doen?

Notes:

Duur: 30 minuten.

De half-adder heet zo omdat hij geen carry-in heeft. Een full-adder heeft wel een carry-in, zodat je meerdere bits kunt optellen.

Loop rond en help. Dit is vaak een aha-moment voor leerlingen!

---

## Half-adder oplossing

```
[A] ──┬─[XOR]──[Sum]
      │
[B] ──┼─[XOR]
      │
      └─[AND]──[Carry]
        [AND]
```

De XOR geeft de som, de AND geeft de carry

Notes:

Laat deze slide alleen zien als hulp of aan het einde van het werkblok.

---

## Full-adder

Een **full-adder** heeft ook een **carry-in**

Hiermee kun je meerdere bits achter elkaar optellen:

```
Carry-in ──┐
           │
    A ─────┼──[Full adder]── Sum
           │
    B ─────┘               └─ Carry-out
```

De carry-out van bit 0 wordt de carry-in van bit 1, etc.

Notes:

Als er tijd over is, kunnen leerlingen een full-adder proberen te bouwen. Dit is best lastig!

Een full-adder bestaat uit twee half-adders en een OR-poort.

***

## Vrije werktijd

Kies wat je wilt doen:

1. **Full-adder bouwen** - probeer een full-adder te maken
2. **4-bits adder** - koppel vier full-adders aan elkaar
3. **NAND game** - ga naar [nandgame.com](https://nandgame.com/)
4. **Andere schakelingen** - probeer opdrachten uit de syllabus

Notes:

Duur: rest van de les (±40-50 minuten, afhankelijk van hoe snel de eerdere blokken gingen).

Loop rond, help waar nodig, moedig aan om uitdagende dingen te proberen.

NAND game is leuk omdat je stap voor stap een hele computer bouwt!

---

## De NAND game

[nandgame.com](https://nandgame.com/)

Een spel waarin je van scratch een computer bouwt!

Je begint met alleen een NAND-poort en bouwt steeds complexere onderdelen:
- Eerst andere poorten
- Dan rekenschakelingen
- Dan geheugen
- Uiteindelijk een hele processor!

Notes:

Moedig leerlingen aan om dit te proberen. Het is verslavend en leerzaam!

Ze hoeven niet alles af te krijgen, maar het geeft goed inzicht in hoe een computer opgebouwd is.

***

## Reflectie

Wat heb je vandaag gebouwd?

- Welke poorten ken je nu?
- Wat was het moeilijkste onderdeel?
- Wat vond je verrassend?
- Hoe ver ben je gekomen met de half-adder of NAND game?

Notes:

Duur: 10 minuten.

Laat leerlingen delen wat ze hebben gemaakt. Vier successen!

---

## Het grotere plaatje

Wat je vandaag hebt gebouwd, zit **echt** in een processor

Een moderne processor (zoals in je telefoon) heeft miljarden van deze poorten

Allemaal gecombineerd om te kunnen:
- Rekenen
- Geheugen opslaan
- Signalen doorsturen
- Programma's uitvoeren

Notes:

Maak de koppeling naar het grotere geheel. Ze hebben vandaag de fundamenten van een computer gebouwd!

***

## Huiswerk

**Oefeningen uit de syllabus:**
- Hoofdstuk: Logica voor computers
- Alle "Oefenen" secties
- Maak waarheidstabellen voor verschillende schakelingen

**Extra uitdaging:**
- Speel verder met de NAND game thuis
- Maak screenshots van levels die je oplost

Notes:

Volgende week: processorarchitectuur en machinetaal

***

## Vragen?


Notes:

Laat ruimte voor vragen. Complimenteer de leerlingen met hun werk.
