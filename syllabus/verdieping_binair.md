# Meer binair rekenen

## Floating point getallen

Tot nu toe hebben we alleen gewerkt met gehele getallen (integers). Maar computers moeten ook kunnen rekenen met kommagetallen (floating point numbers). Denk aan wetenschappelijke berekeningen, graphics, financiële applicaties, etc.

### Het probleem met kommagetallen

Je zou kunnen denken: waarom gebruiken we niet gewoon een vast aantal bits voor het gehele deel en een vast aantal bits voor het gedeelte achter de komma? Dit heet *fixed point* notatie en wordt ook wel gebruikt, maar heeft beperkingen:

- Als je 16 bits hebt en je gebruikt 8 bits voor het gehele deel en 8 bits voor het fractionele deel, kun je getallen representeren van ongeveer -128.0 tot 127.99609375
- Wil je hele grote getallen kunnen representeren, dan heb je nauwkeurigheid achter de komma te kort
- Wil je heel precieze kleine getallen, dan kun je geen grote getallen representeren

De oplossing: *floating point* notatie, waarbij de komma kan "zweven" (float).

### Wetenschappelijke notatie

Je kent waarschijnlijk de wetenschappelijke notatie uit de natuurkunde of scheikunde:
- $6.022 \times 10^{23}$ (getal van Avogadro)
- $3.0 \times 10^8$ (lichtsnelheid in m/s)
- $1.6 \times 10^{-19}$ (lading van een elektron in Coulomb)

Deze notatie bestaat uit:
- **Mantisse** (of significand): het getal voor de macht (bijv. 6.022)
- **Exponent**: de macht van 10 (bijv. 23)
- **Grondtal**: 10

In binaire floating point gebruiken we hetzelfde principe, maar met grondtal 2 in plaats van 10.

### IEEE 754 standaard

De meest gebruikte standaard voor floating point getallen is *IEEE 754*. Deze definieert verschillende formaten, waarvan de twee belangrijkste zijn:

**Single precision (32 bits):**
- 1 bit voor teken (sign)
- 8 bits voor exponent
- 23 bits voor mantisse (fractie)

**Double precision (64 bits):**
- 1 bit voor teken
- 11 bits voor exponent
- 52 bits voor mantisse

```
Single precision (32 bits):
+---+--------+-----------------------+
| S |  Exp   |      Mantisse        |
+---+--------+-----------------------+
  1     8            23 bits
```

### Hoe werkt het?

Een getal wordt gerepresenteerd als:

$$(-1)^S \times 1.M \times 2^{E - bias}$$

Waarbij:
- $S$ = teken bit (0 = positief, 1 = negatief)
- $M$ = mantisse (de bits na de komma van een genormaliseerd getal)
- $E$ = exponent (zoals opgeslagen in de bits)
- $bias$ = 127 voor single precision, 1023 voor double precision

**Normalisatie**

Binaire getallen worden *genormaliseerd*: ze worden geschreven in de vorm $1.xxxxx \times 2^n$. Omdat het eerste bit voor de komma altijd 1 is bij genormaliseerde getallen (behalve bij speciale gevallen), wordt deze niet opgeslagen. Dit heet de *hidden bit* en geeft ons een extra bit aan nauwkeurigheid.

**Voorbeeld: Het getal 5.75 in single precision**

Stap 1: Converteer naar binair
- $5_{10} = 101_2$
- $0.75_{10} = 0.11_2$ (want $0.5 + 0.25 = 0.75$)
- $5.75_{10} = 101.11_2$

Stap 2: Normaliseer
- $101.11_2 = 1.0111_2 \times 2^2$

Stap 3: Bepaal de onderdelen
- Teken: positief → $S = 0$
- Exponent: $2$, opgeslagen als $2 + 127 = 129_{10} = 10000001_2$
- Mantisse: $0111$ (de bits na de komma van $1.0111$), aangevuld met nullen tot 23 bits: $01110000000000000000000$

Stap 4: Combineer
```
0 10000001 01110000000000000000000
S Exponent Mantisse
```

In hexadecimaal: `0x40B80000`

**Voorbeeld: Decoderen van een float**

Gegeven: `0 10000010 10010000000000000000000`

- Teken: $S = 0$ → positief
- Exponent: $10000010_2 = 130_{10}$ → $130 - 127 = 3$
- Mantisse: $1001000...$ → $1.1001_2$

Getal: $1.1001_2 \times 2^3 = 1100.1_2 = 12.5_{10}$

:::{exercise}

1. Converteer het decimale getal $-0.375$ naar IEEE 754 single precision formaat. Geef je antwoord in binair en hexadecimaal.

2. Decodeer de volgende IEEE 754 single precision getallen:
   - `0 01111111 00000000000000000000000`
   - `1 10000000 10000000000000000000000`

3. Leg uit waarom het getal $0.1_{10}$ niet exact representeerbaar is in binaire floating point. Wat voor problemen kan dit geven in programma's?

4. Hoeveel verschillende getallen kunnen gerepresenteerd worden met 32-bit floating point? Is dit meer of minder dan met 32-bit signed integers?

:::

### Speciale waarden

IEEE 754 definieert ook een aantal speciale waarden:

**Zero (nul)**
- Exponent: allemaal 0-en
- Mantisse: allemaal 0-en
- Er zijn twee nullen: +0 en -0 (afhankelijk van het tekenbit)

**Infinity (oneindig)**
- Exponent: allemaal 1-en
- Mantisse: allemaal 0-en
- Er zijn twee oneindigheden: $+\infty$ en $-\infty$

**NaN (Not a Number)**
- Exponent: allemaal 1-en
- Mantisse: niet allemaal 0-en
- Wordt gebruikt voor ongeldige operaties zoals $0/0$ of $\sqrt{-1}$

**Denormalized numbers (gedenormaliseerde getallen)**
- Exponent: allemaal 0-en
- Mantisse: niet allemaal 0-en
- Worden gebruikt om getallen heel dicht bij nul te representeren
- Formule: $(-1)^S \times 0.M \times 2^{-126}$ (single precision)

### Problemen met floating point

Floating point getallen zijn ontzettend nuttig, maar hebben ook hun beperkingen:

**Beperkte precisie**
```python
>>> 0.1 + 0.2
0.30000000000000004
```

Dit komt omdat $0.1_{10}$ een oneindig herhalend getal is in binair, net zoals $1/3$ oneindig herhalend is in decimaal.

**Verlies van precisie bij optellen**

Als je een heel groot getal bij een heel klein getal optelt, kan het kleine getal "verdwijnen":
```python
>>> 1e20 + 1.0 == 1e20
True
```

**Vergelijkingen**

Nooit floating point getallen vergelijken met `==`! Gebruik altijd een tolerantie:
```python
# Fout:
if (a == b):
    ...

# Goed:
epsilon = 1e-9
if (abs(a - b) < epsilon):
    ...
```

**Operatie volgorde**

De volgorde van operaties kan het resultaat beïnvloeden:
```python
>>> (1e20 + 1.0) - 1e20
0.0
>>> 1e20 + (1.0 - 1e20)
-1e20
```

:::{exercise}

1. Probeer in Python (of een andere programmeertaal) het volgende uit:
   ```python
   a = 0.1 + 0.1 + 0.1
   b = 0.3
   print(a == b)
   print(a)
   print(b)
   ```
   Leg uit wat je ziet.

2. Schrijf een klein programma dat demonstreert dat `(a + b) + c` niet altijd hetzelfde is als `a + (b + c)` voor floating point getallen.

3. Zoek uit wat *catastrophic cancellation* is bij floating point operaties. Geef een voorbeeld.

4. In veel programmeertalen zijn er aparte types voor `float` (32-bit) en `double` (64-bit). Wanneer zou je voor welke kiezen?

:::

### Floating point in de praktijk

Bij het werken met floating point getallen zijn er een aantal best practices:

1. **Gebruik double precision als default** tenzij geheugen/snelheid echt een probleem is.
2. **Nooit vergelijken met ==** maar altijd met een tolerantie.
3. **Let op bij aftrekken** van twee bijna gelijke getallen (catastrophic cancellation).
4. **Optellen in juiste volgorde**: bij het optellen van vele getallen, sorteer eerst op grootte.
5. **Wees voorzichtig met herhaalde operaties**: fouten kunnen zich opstapelen.
6. **Gebruik speciale bibliotheken** voor kritische toepassingen (bijv. multiprecision libraries).

In financiële toepassingen wordt vaak *helemaal geen* floating point gebruikt, maar worden bedragen opgeslagen als gehele getallen in centen. Dit voorkomt afrondingsfouten bij geldbedragen.

:::{exercise}

**Verdiepende opdracht:**

1. Onderzoek wat *ULP* (Unit in the Last Place) betekent bij floating point getallen.

2. Kijk naar de documentatie van de `math` module in Python (of een vergelijkbare bibliotheek in een andere taal). Welke functies zijn er om met floating point precisie om te gaan?

3. Sommige processoren hebben speciale instructies voor floating point operaties (zoals de FPU - Floating Point Unit). Zoek uit waarom dit sneller is dan floating point operaties implementeren met normale integer operaties.

4. **Project:** Schrijf een programma dat het verschil laat zien tussen het berekenen van $\sum_{i=1}^{1000000} \frac{1}{i}$ door van 1 tot 1000000 te tellen vs van 1000000 tot 1 terug te tellen. Verklaar het verschil.

:::

