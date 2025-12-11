(hoofdstuk_meer_logica)=
# Meer computerlogica

In het hoofdstuk {doc}`logica` heb je kennis gemaakt met de basis logische poorten en hoe je waarheidstabellen maakt. Je hebt ook gezien hoe je met schakelingen kunt optellen. In dit verdiepingshoofdstuk gaan we verder kijken naar de theoretische fundamenten van logische schakelingen en hoe we deze kunnen optimaliseren. Ook gaan we kijken naar een compleet ander onderwerp: hoe computers met kommagetallen werken.

## Volledigheid NAND/NOR

In het hoofdstuk over logica heb je verschillende logische poorten gezien: AND, OR, NOT, NAND, NOR, XOR en XNOR. Je zou kunnen denken dat je al deze poorten nodig hebt om complexe schakelingen te maken. Maar dat is niet waar! Er is iets opmerkelijks aan de hand met de NAND-poort en de NOR-poort: met alleen maar NAND-poorten (of alleen maar NOR-poorten) kun je *alle* andere logische poorten namaken. Dit noemen we **functionele volledigheid**.

### Waarom is dit belangrijk?

Voor producenten van processors is dit interessant. Als je maar één type poort hoeft te produceren in plaats van zeven verschillende types, dan:
- Wordt het productieproces eenvoudiger en goedkoper
- Kun je meer standaardisatie toepassen
- Worden ontwerpfouten minder waarschijnlijk

In de praktijk worden vaak NAND-poorten gebruikt, omdat deze relatief eenvoudig te produceren zijn met transistors.

### NAND is functioneel volledig

Laten we bewijzen dat je met alleen NAND-poorten alle andere poorten kunt maken.

**De NOT-poort uit NAND**

De NOT-poort is eigenlijk heel simpel te maken. Als je beide ingangen van een NAND-poort aan elkaar koppelt, krijg je een NOT-poort:

```{mermaid}
graph LR
    A[Input A] --> NAND[NAND]
    A --> NAND
    NAND --> Output[Output]
```

De waarheidstabel van een NAND met gekoppelde ingangen:

| A | B | A NAND B |
|:-:|:-:|:--------:|
| 0 | 0 |    1     |
| 1 | 1 |    0     |

Als beide ingangen hetzelfde zijn, zie je dat dit precies het gedrag van een NOT-poort is: `0` wordt `1` en `1` wordt `0`.

**De AND-poort uit NAND**

Een AND-poort maken is ook niet moeilijk. We weten dat NAND het omgekeerde doet van AND. Dus als we achter een NAND-poort nog een NOT-poort plaatsen (die we net hebben gemaakt met een NAND), dan krijgen we een AND-poort:

```{mermaid}
graph LR
    A[A] --> NAND1[NAND]
    B[B] --> NAND1
    NAND1 --> NAND2[NAND]
    NAND1 --> NAND2
    NAND2 --> Output[Output A AND B]
```

**De OR-poort uit NAND**

Voor de OR-poort gebruiken we een trucje. We maken eerst beide ingangen negatief (met NOT-poorten gemaakt van NAND), en voeren die door een NAND-poort. Dit geeft ons een OR-poort. Dit heeft te maken met de *wet van De Morgan*, die we later in dit hoofdstuk behandelen.

```{mermaid}
graph LR
    A[A] --> NOT1A[NAND]
    A --> NOT1A
    B[B] --> NOT1B[NAND]
    B --> NOT1B
    NOT1A --> NAND3[NAND]
    NOT1B --> NAND3
    NAND3 --> Output[Output A OR B]
```

:::{exercise}

1. Maak in [Logicly](https://logic.ly/demo/) een NOT-poort met behulp van alleen een NAND-poort. Test alle mogelijke invoeren en controleer of het zich gedraagt als een NOT-poort.

2. Maak in Logicly een AND-poort met behulp van alleen NAND-poorten. Controleer met een waarheidstabel of deze zich correct gedraagt.

3. Maak in Logicly een OR-poort met behulp van alleen NAND-poorten. Controleer met een waarheidstabel of deze zich correct gedraagt.

4. **Uitdaging:** Maak een XOR-poort met behulp van alleen NAND-poorten. Tip: je hebt minimaal 4 NAND-poorten nodig.

5. Hoeveel NAND-poorten heb je minimaal nodig om een NOR-poort te maken?

:::

### NOR is ook functioneel volledig

Hetzelfde geldt voor de NOR-poort. Ook met alleen NOR-poorten kun je alle andere logische poorten namaken. Het principe is hetzelfde als bij NAND, maar dan andersom.

:::{exercise}

1. Leg uit hoe je met een NOR-poort een NOT-poort kunt maken.

2. Maak in Logicly een OR-poort met behulp van alleen NOR-poorten.

3. Maak in Logicly een AND-poort met behulp van alleen NOR-poorten.

:::

### Praktische toepassing

In echte processors worden niet altijd pure NAND- of NOR-poorten gebruikt, maar wel vaak combinaties die gebaseerd zijn op dit principe. Door te weten dat je met één type poort alle functionaliteit kunt maken, kunnen ontwerpers van processors slimme keuzes maken in het ontwerp.

## De Morgan wetten

Augustus De Morgan was een Britse wiskundige die in de 19e eeuw leefde. Hij ontdekte twee belangrijke wetten die een relatie leggen tussen AND, OR en NOT operaties. Deze wetten zijn fundamenteel voor het begrijpen en optimaliseren van logische schakelingen.

### De eerste wet van De Morgan

De eerste wet zegt:

$$\overline{A \cdot B} = \overline{A} + \overline{B}$$

In woorden: *het complement (NOT) van een AND is gelijk aan de OR van de complementen*.

Of anders gezegd: een NAND-operatie is hetzelfde als eerst beide ingangen omkeren (NOT) en dan een OR-operatie uitvoeren.

Laten we dit controleren met een waarheidstabel:

| A | B | A·B | $\overline{A \cdot B}$ | $\overline{A}$ | $\overline{B}$ | $\overline{A} + \overline{B}$ |
|:-:|:-:|:---:|:----------------------:|:--------------:|:--------------:|:-----------------------------:|
| 0 | 0 |  0  |           1            |       1        |       1        |               1               |
| 0 | 1 |  0  |           1            |       1        |       0        |               1               |
| 1 | 0 |  0  |           1            |       0        |       1        |               1               |
| 1 | 1 |  1  |           0            |       0        |       0        |               0               |

Je ziet dat de kolom $\overline{A \cdot B}$ precies gelijk is aan de kolom $\overline{A} + \overline{B}$. De wet klopt!

### De tweede wet van De Morgan

De tweede wet zegt:

$$\overline{A + B} = \overline{A} \cdot \overline{B}$$

In woorden: *het complement (NOT) van een OR is gelijk aan de AND van de complementen*.

Of anders gezegd: een NOR-operatie is hetzelfde als eerst beide ingangen omkeren (NOT) en dan een AND-operatie uitvoeren.

:::{exercise}

1. Maak een waarheidstabel om te bewijzen dat de tweede wet van De Morgan klopt.

2. Gegeven is de expressie $\overline{A + B + C}$. Herschrijf deze met behulp van de wet van De Morgan.

3. Gegeven is de expressie $\overline{A \cdot B} + \overline{C \cdot D}$. Pas de wet van De Morgan toe om de expressie om te schrijven.

4. Vereenvoudig de volgende expressie met behulp van de wetten van De Morgan: $\overline{\overline{A} \cdot \overline{B}}$

:::

### Waarom zijn de wetten van De Morgan belangrijk?

De wetten van De Morgan zijn belangrijk omdat:

1. **Optimalisatie**: Je kunt hiermee schakelingen vereenvoudigen en efficiënter maken.
2. **Flexibiliteit**: Als je bijvoorbeeld alleen NAND-poorten hebt, kun je met De Morgan uitrekenen hoe je een OR-functie maakt.
3. **Begrip**: Ze helpen je begrijpen hoe logische operaties met elkaar samenhangen.

In de praktijk gebruik je de wetten van De Morgan vaak om een schakeling om te schrijven naar een vorm die efficiënter te implementeren is met de beschikbare componenten.

## Boolse algebra

George Boole was een Engelse wiskundige die in de 19e eeuw een algebraïsch systeem ontwikkelde voor logica. Dit systeem, *Boolse algebra* genoemd, gebruikt variabelen die alleen de waarden `0` (onwaar) of `1` (waar) kunnen aannemen. Het is de wiskundige basis voor alle digitale elektronica en computerlogica.

### Basisbewerkingen

In Boolse algebra zijn er drie fundamentele bewerkingen:

1. **AND** (vermenigvuldiging): $A \cdot B$ of gewoon $AB$
2. **OR** (optelling): $A + B$
3. **NOT** (complement): $\overline{A}$ of $A'$

Let op: het `+`-teken betekent hier OR, niet optelling! En het `·`-teken betekent AND, niet vermenigvuldiging! De notatie is gekozen omdat er overeenkomsten zijn, maar het zijn andere bewerkingen.

### Rekenregels in Boolse algebra

Net zoals in de gewone algebra zijn er rekenregels. Hier zijn de belangrijkste:

**Identiteitswetten:**
- $A + 0 = A$
- $A \cdot 1 = A$

**Nulelement wetten:**
- $A + 1 = 1$
- $A \cdot 0 = 0$

**Idempotente wetten:**
- $A + A = A$
- $A \cdot A = A$

**Complementwetten:**
- $A + \overline{A} = 1$
- $A \cdot \overline{A} = 0$

**Dubbele negatie:**
- $\overline{\overline{A}} = A$

**Commutatieve wetten:**
- $A + B = B + A$
- $A \cdot B = B \cdot A$

**Associatieve wetten:**
- $(A + B) + C = A + (B + C)$
- $(A \cdot B) \cdot C = A \cdot (B \cdot C)$

**Distributieve wetten:**
- $A \cdot (B + C) = (A \cdot B) + (A \cdot C)$
- $A + (B \cdot C) = (A + B) \cdot (A + C)$ ← Let op! Deze is anders dan in gewone algebra!

**Absorptiewetten:**
- $A + (A \cdot B) = A$
- $A \cdot (A + B) = A$

**De Morgan's wetten** (die we al kennen):
- $\overline{A \cdot B} = \overline{A} + \overline{B}$
- $\overline{A + B} = \overline{A} \cdot \overline{B}$

### Voorbeelden van vereenvoudiging

**Voorbeeld 1:** Vereenvoudig $A \cdot (A + B)$

Met de absorptiewet weten we direct: $A \cdot (A + B) = A$

Maar we kunnen het ook stap voor stap doen:
- $A \cdot (A + B)$
- $= (A \cdot A) + (A \cdot B)$ (distributieve wet)
- $= A + (A \cdot B)$ (idempotente wet: $A \cdot A = A$)
- $= A$ (absorptiewet)

**Voorbeeld 2:** Vereenvoudig $A \cdot B + A \cdot \overline{B}$

- $A \cdot B + A \cdot \overline{B}$
- $= A \cdot (B + \overline{B})$ (distributieve wet)
- $= A \cdot 1$ (complementwet: $B + \overline{B} = 1$)
- $= A$ (identiteitswet)

**Voorbeeld 3:** Vereenvoudig $\overline{A} \cdot B + A \cdot B$

- $\overline{A} \cdot B + A \cdot B$
- $= (\overline{A} + A) \cdot B$ (distributieve wet)
- $= 1 \cdot B$ (complementwet)
- $= B$ (identiteitswet)

:::{exercise}

Vereenvoudig de volgende Boolse expressies met behulp van de rekenregels:

1. $A + A \cdot B$
2. $A \cdot (\overline{A} + B)$
3. $(A + B) \cdot (A + \overline{B})$
4. $A \cdot B + \overline{A} \cdot B + A \cdot \overline{B}$
5. $\overline{A \cdot B} \cdot \overline{A + B}$
6. $(A + B) \cdot (\overline{A} + B)$

**Uitdaging:** Bewijs dat $A + \overline{A} \cdot B = A + B$ met behulp van de Boolse rekenregels.

:::

### Van waarheidstabel naar Boolse expressie

Soms heb je een waarheidstabel en wil je daar een Boolse expressie van maken. Dit doe je met de **som-van-producten** methode:

1. Kijk naar alle rijen waar de uitvoer `1` is
2. Voor elke zo'n rij maak je een product (AND) van alle invoeren:
   - Als de invoer `1` is, neem je de variabele
   - Als de invoer `0` is, neem je het complement van de variabele
3. Maak de som (OR) van al deze producten

**Voorbeeld:**

| A | B | C | Uitvoer |
|:-:|:-:|:-:|:-------:|
| 0 | 0 | 0 |    0    |
| 0 | 0 | 1 |    1    |
| 0 | 1 | 0 |    1    |
| 0 | 1 | 1 |    0    |
| 1 | 0 | 0 |    1    |
| 1 | 0 | 1 |    0    |
| 1 | 1 | 0 |    0    |
| 1 | 1 | 1 |    1    |

De rijen met uitvoer `1` zijn:
- Rij 2: A=0, B=0, C=1 → $\overline{A} \cdot \overline{B} \cdot C$
- Rij 3: A=0, B=1, C=0 → $\overline{A} \cdot B \cdot \overline{C}$
- Rij 5: A=1, B=0, C=0 → $A \cdot \overline{B} \cdot \overline{C}$
- Rij 8: A=1, B=1, C=1 → $A \cdot B \cdot C$

De Boolse expressie is:
$$F = \overline{A} \cdot \overline{B} \cdot C + \overline{A} \cdot B \cdot \overline{C} + A \cdot \overline{B} \cdot \overline{C} + A \cdot B \cdot C$$

Deze expressie kun je vervolgens vereenvoudigen met de Boolse rekenregels.

:::{exercise}

1. Maak een Boolse expressie voor de volgende waarheidstabel:

| A | B | Uitvoer |
|:-:|:-:|:-------:|
| 0 | 0 |    0    |
| 0 | 1 |    1    |
| 1 | 0 |    1    |
| 1 | 1 |    0    |

2. Herken je deze functie? Welke logische poort is dit?

3. Maak een Boolse expressie voor een 2-bit vergelijkingsschakeling: de uitvoer is `1` alleen als A en B gelijk zijn.

:::

## Minimalisatie van schakelingen

Als je een Boolse expressie hebt gemaakt uit een waarheidstabel, is deze vaak niet de meest efficiënte vorm. Door de expressie te vereenvoudigen, kun je schakelingen maken die:
- Minder poorten gebruiken
- Sneller zijn (minder vertragingen)
- Minder energie verbruiken
- Goedkoper zijn om te produceren

### Algebraïsche minimalisatie

Dit is de methode waarbij je de Boolse rekenregels gebruikt om een expressie te vereenvoudigen. We hebben hier al voorbeelden van gezien.

**Voorbeeld:**

Gegeven: $F = A \cdot B \cdot C + A \cdot B \cdot \overline{C} + A \cdot \overline{B} \cdot C$

Vereenvoudiging:
- $F = A \cdot B \cdot C + A \cdot B \cdot \overline{C} + A \cdot \overline{B} \cdot C$
- $= A \cdot B \cdot (C + \overline{C}) + A \cdot \overline{B} \cdot C$ (distributief op eerste twee termen)
- $= A \cdot B \cdot 1 + A \cdot \overline{B} \cdot C$ (complementwet)
- $= A \cdot B + A \cdot \overline{B} \cdot C$ (identiteitswet)
- $= A \cdot (B + \overline{B} \cdot C)$ (distributief)
- $= A \cdot (B + C)$ (absorptiewet: $B + \overline{B} \cdot C = B + C$)

Van 3 AND-poorten met 3 ingangen plus 2 OR-poorten zijn we nu naar 1 AND-poort en 1 OR-poort gegaan!

### Karnaugh-diagram (K-map)

Voor kleine functies (2-4 variabelen) is er een visuele methode die vaak sneller werkt: het *Karnaugh-diagram* of *K-map*.

Een Karnaugh-diagram is een rooster waarbij elke cel een rij uit de waarheidstabel voorstelt. De cellen zijn zo gerangschikt dat aangrenzende cellen maar in één bit verschillen (dit heet Gray-codering).

**Voorbeeld voor 2 variabelen (A, B):**

```
      B=0  B=1
    +----+----+
A=0 |  0 |  1 |
    +----+----+
A=1 |  2 |  3 |
    +----+----+
```

Elk vakje komt overeen met een combinatie:
- Vakje 0: A=0, B=0
- Vakje 1: A=0, B=1
- Vakje 2: A=1, B=0
- Vakje 3: A=1, B=1

**Voorbeeld voor 3 variabelen (A, B, C):**

```
        BC=00  BC=01  BC=11  BC=10
      +------+------+------+------+
A=0   |   0  |   1  |   3  |   2  |
      +------+------+------+------+
A=1   |   4  |   5  |   7  |   6  |
      +------+------+------+------+
```

Let op de volgorde van BC: 00, 01, 11, 10. Dit is Gray-codering: elke stap verschilt maar in 1 bit.

**Hoe gebruik je een K-map?**

1. Vul de K-map in: schrijf een `1` in elke cel waar de functie `1` oplevert, en een `0` (of laat leeg) waar de functie `0` oplevert.
2. Zoek groepen van 1-en. Groepen moeten:
   - Rechthoekig zijn
   - Een grootte hebben die een macht van 2 is (1, 2, 4, 8, ...)
   - Zo groot mogelijk zijn
3. Voor elke groep bepaal je welke variabelen constant zijn:
   - Als een variabele in de hele groep `0` is, neem je $\overline{X}$
   - Als een variabele in de hele groep `1` is, neem je $X$
   - Als een variabele zowel `0` als `1` is in de groep, laat je die variabele weg
4. De minimale expressie is de som (OR) van alle groepen.

**Voorbeeld:**

Stel we hebben deze waarheidstabel:

| A | B | C | F |
|:-:|:-:|:-:|:-:|
| 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 1 |
| 0 | 1 | 0 | 0 |
| 0 | 1 | 1 | 1 |
| 1 | 0 | 0 | 1 |
| 1 | 0 | 1 | 1 |
| 1 | 1 | 0 | 1 |
| 1 | 1 | 1 | 1 |

K-map:
```
        BC=00  BC=01  BC=11  BC=10
      +------+------+------+------+
A=0   |   0  |   1  |   1  |   0  |
      +------+------+------+------+
A=1   |   1  |   1  |   1  |   1  |
      +------+------+------+------+
```

We kunnen twee groepen maken:
- Groep 1: De hele onderste rij (4 cellen) → A=1, BC varieert → dit is gewoon $A$
- Groep 2: De twee middelste cellen van de bovenste rij → A=0, C=1, B varieert → dit is $\overline{A} \cdot C$

De minimale expressie: $F = A + \overline{A} \cdot C$

Dit kunnen we nog verder vereenvoudigen met absorptie: $F = A + C$

**Don't care condities**

Soms is de uitvoer voor bepaalde invoercombinaties niet belangrijk (bijvoorbeeld omdat die combinaties nooit voorkomen). Deze noemen we *don't care* condities en we geven ze aan met een `X` of `-`.

Bij het minimaliseren mag je deze cellen behandelen als `0` of `1`, wat het meest handig is om grotere groepen te maken.

:::{exercise}

1. Maak een K-map voor de XOR-functie (2 variabelen) en bepaal de minimale expressie.

2. Gegeven is de volgende functie: $F = \overline{A} \cdot B \cdot C + A \cdot \overline{B} \cdot C + A \cdot B \cdot \overline{C} + A \cdot B \cdot C$
   - Maak een K-map
   - Bepaal de minimale expressie

3. Maak een K-map voor een 3-input majority functie: de uitvoer is `1` als ten minste 2 van de 3 ingangen `1` zijn.

4. **Uitdaging:** Maak een K-map voor 4 variabelen (A, B, C, D) waarbij de functie `1` is voor alle even getallen (waarbij ABCD het binaire getal voorstelt). Bepaal de minimale expressie.

:::

### Quine-McCluskey methode

Voor functies met veel variabelen (meer dan 4) wordt de K-map onoverzichtelijk. Dan kun je de *Quine-McCluskey methode* gebruiken. Dit is een algoritmische methode die altijd werkt, maar die laten we voor een eventuele vervolgstudie.

### Praktische overwegingen

Bij het minimaliseren van schakelingen spelen ook praktische overwegingen:

- **Aantal poorten vs aantal ingangen**: Soms is een oplossing met meer poorten maar met minder ingangen per poort voordeliger.
- **Propagation delay**: Het aantal lagen poorten bepaalt hoe lang een signaal er over doet om door de schakeling te gaan.
- **Fan-out**: Hoeveel andere poorten kunnen worden aangestuurd door één uitgang.
- **Beschikbare componenten**: Als je alleen bepaalde IC's (integrated circuits) hebt, moet je daar rekening mee houden.

In de praktijk gebruiken computerontwerpers geautomatiseerde tools die al deze factoren meenemen.

