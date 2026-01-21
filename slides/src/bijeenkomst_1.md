# Binair Rekenen

Q-highschool / Computerarchitectuur / Bijeenkomst 1

***

## Welkom!

In deze bijeenkomst leer je:

1. Binaire getallen (heen-en-weer omrekenen)
2. Binaire getallen optellen
3. Negatieve getallen
4. Hexadecimale getallen

***

## Over deze module

**Computerarchitectuur**

In deze module leer je hoe computers "onder de motorkap" werken:

- Hoe computers rekenen met bits en bytes
- Hoe logische schakelingen werken
- Hoe processors instructies uitvoeren
- Hoe geheugen is georganiseerd

---

## Waarom is dit belangrijk?

Als je begrijpt hoe computers werken:

- Begrijp je waarom sommige code sneller is
- Kun je beter debuggen en problemen oplossen
- Kun je betere technische beslissingen maken

---

## Opbouw van de module

**Bijeenkomsten:**

1. **Vandaag:** Binair rekenen
2. **Volgende keer:** Logische poorten en boolean algebra
3. **Daarna:** Machinetaal en instructies
4. **Later:** Processorarchitectuur en geheugen

---

## Werkvormen

**Wat kun je verwachten?**

- **Uitleg:** Ik leg nieuwe concepten uit
- **Voordoen:** Ik werk voorbeelden uit
- **Oefenen:** Jij werkt aan opgaven
- **Reflectie:** We bespreken wat je hebt geleerd

Je mag en moet vragen stellen!

---

## Huiswerk en toetsing

**Tussen de bijeenkomsten:**
- Oefenopgaven uit de syllabus
- Online materiaal bekijken

**Beoordeling:**
- Eindopdracht waarin je alles combineert

***

## Vandaag: Binair Rekenen

Computers rekenen niet met 0-9, maar met 0-1

Vandaag leer je:
- Waarom computers binair rekenen
- Hoe je binair omrekent
- Hoe je binair kunt optellen
- Hoe computers negatieve getallen opslaan
- Hexadecimale notatie (programmeurs gebruiken dit veel!)

***

# 1. Kennismaking binaire getallen

***

## Wat zijn binaire getallen?

Computers werken met twee toestanden: </br> **Uit** of **Aan**, **0** of **1**

In het **decimale stelsel** gebruiken we 10 cijfers: </br> 0, 1, 2, 3, 4, 5, 6, 7, 8, 9

In het **binaire stelsel** gebruiken we 2 cijfers: </br>**0** en **1**

---

## Binair tellen

Decimaal: $0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, \ldots$

Binair: `0, 1, 10, 11, 100, 101, 110, 111, 1000, ...`

De cijfers zijn veel sneller "op"!

---

## Van binair naar decimaal

Net als bij decimale getallen gebruiken we **machten**

Decimaal voorbeeld: </br>$743 = 7\times 10^2 + 4\times 10^1 + 3\times 10^0$

Binair voorbeeld: </br>$1101_b = 1\times 2^3 + 1\times 2^2 + 0\times 2^1 + 1\times 2^0$

Dus: </br> $1101_b = 8 + 4 + 0 + 1 = 13_d$

---

## Machten van 2

Leer deze uit je hoofd! 

| Macht | Waarde | Binair | | Macht | Waarde | Binair |
|-------|--------|--------|-|-------|--------|--------|
| $2^0$ | 1 | `1` | | $2^5$ | 32 | `100000` |
| $2^1$ | 2 | `10` | | $2^6$ | 64 | `1000000` |
| $2^2$ | 4 | `100` | | $2^7$ | 128 | `10000000` |
| $2^3$ | 8 | `1000` | | $2^8$ | 256 | `100000000` |
| $2^4$ | 16 | `10000` | | $2^9$ | 512 | `1000000000` |
|  |  |  | | $2^{10}$ | 1024 | `10000000000` |

<!-- .element: style="font-size: 0.70em" -->

***

## Voordoen: Binair naar decimaal
<!-- .slide: class="voordoen" -->

Zet $10110_b$ om naar decimaal

Notes:

**Uitwerking:**

$10110_b = 1\times 2^4 + 0\times 2^3 + 1\times 2^2 + 1\times 2^1 + 0\times 2^0$

$= 16 + 0 + 4 + 2 + 0$

$= 22_d$

***

## Oefenen: Binair naar decimaal

Zet de volgende binaire getallen om naar decimaal:

1. $1011_b$
2. $101101_b$
3. $1100111_b$
4. $10010110_b$
5. $1111001_b$
6. $100001010_b$

---

## Reflectie

- Wat vond je moeilijk?
- Welke trucs heb je ontdekt?
- Deel je aanpak met je groep

***

## Van decimaal naar binair

We delen steeds door 2 en schrijven de **rest** op:

Voorbeeld: $22_d$ naar binair

- $22 \div 2 = 11$ rest **0**
- $11 \div 2 = 5$ rest **1**
- $5 \div 2 = 2$ rest **1**
- $2 \div 2 = 1$ rest **0**
- $1 \div 2 = 0$ rest **1**

Lees de resten **van onder naar boven**: $10110_b$

***

## Voordoen: Decimaal naar binair
<!-- .slide: class="voordoen" -->

Zet $46_d$ om naar binair

Notes:

**Uitwerking:**

- $46 \div 2 = 23$ rest **0**
- $23 \div 2 = 11$ rest **1**
- $11 \div 2 = 5$ rest **1**
- $5 \div 2 = 2$ rest **1**
- $2 \div 2 = 1$ rest **0**
- $1 \div 2 = 0$ rest **1**

Van onder naar boven: $46_d = 101110_b$

***

## Oefenen: Decimaal naar binair

Zet de volgende decimale getallen om naar binair:

1. 25
2. 63
3. 99
4. 200
5. 327

---

## Reflectie

- Hoe controleer je of je antwoord klopt?
- Wat is de efficiëntste manier om dit te doen?
- Deel je bevindingen

***

# 2. Binaire getallen optellen

***

## Binair optellen: De basis

Voor getallen met één cijfer:

- $0_b + 0_b = 0_b$
- $0_b + 1_b = 1_b$
- $1_b + 0_b = 1_b$
- $1_b + 1_b = 10_b$ ← Let op: we gaan over de grens!

---

## Dezelfde methode als decimaal

Net als bij decimaal optellen:

1. Zet getallen onder elkaar (rechts uitgelijnd)
2. Begin rechts, tel op
3. Bij "overschrijden grens": onthoud 1
4. Ga verder naar links

---

## Voorbeeld decimaal

```
         1     11     11 
 498    498    498    498
 334    334    334    334
 --- +  --- +  --- +  --- +
          2     32    832
```

---

## Voorbeeld binair

```
                  1       1       1
 1010    1010    1010    1010    1010
 1011    1011    1011    1011    1011
 ---- +  ---- +  ---- +  ---- +  ---- +
            1      01     101   10101 
```

***

## Voordoen: Binair optellen
<!-- .slide: class="voordoen" -->

Tel op: $1101_b + 1011_b$

Notes:

**Uitwerking:**

```
           1      11     11 
 1101    1101    1101    1101
 1011    1011    1011    1011
 ---- +  ---- +  ---- +  ---- +
            0      00    1000 
```

Antwoord: $11000_b$

Controle: $13_d + 11_d = 24_d$ en $11000_b = 16 + 8 = 24_d$ ✓

***

## Oefenen: Binair optellen

Tel de volgende binaire getallen op:

1. $101_b + 110_b$
2. $11011_b + 1010_b$
3. $1111_b + 10101_b$
4. $111111_b + 101010_b$
5. $1010101_b + 1100110_b$

**Bonus:** Verdubbel $11011_b$ (Tip: wat gebeurt er als je een 0 achter een binair getal zet?)

---

## Reflectie

- Waarom is binair optellen eigenlijk makkelijker dan decimaal?
- Wat was het lastigste onderdeel?
- Hoe controleer je je antwoorden?

***

# 3. Negatieve getallen

***

## Het probleem

Hoe maken we een binair getal negatief?

In wiskunde: zet er een $-$ voor → $-1101_b$

Maar hoe doet een **computer** dit? Een computer heeft alleen `0` en `1`!

---

## Woordlengte en bits

Een **bit** is de kleinste eenheid: `0` of `1`

**8 bits** = 1 **byte**

Moderne processors hebben een **woordlengte** van 64 bits

Betekent: ze rekenen standaard met getallen van 64 bits

---

## Signed vs unsigned

**Unsigned byte:** alle 8 bits voor de grootte

- Bereik: $0$ tot $255$

**Signed byte:** eerste bit geeft teken aan

- `0` vooraan = positief
- `1` vooraan = negatief
- Bereik: $-128$ tot $+127$

***

## Twee-complements notatie

De standaardmanier om negatieve getallen te representeren!

**Stappenplan om getal negatief te maken:**

1. Flip alle bits (`0` → `1`, `1` → `0`)
2. Tel er 1 bij op

---

## Voorbeeld 4-bits

Positief getal: $4_d = 0100_b$

Negatief maken:
1. Flip bits: `0100` → `1011`
2. Tel 1 op: `1011` + `0001` = `1100`

Dus: $-4_d = 1100_b$ (in 4-bits twee-complement)

---

## Waarom twee-complement?

✓ Maar één nul (geen $+0$ en $-0$)

✓ Optellen werkt gewoon! Geen speciale logica nodig

✓ $2_d + (-4_d) = 0010_b + 1100_b = 1110_b = -2_d$ ✓

***

## Voordoen: Twee-complement
<!-- .slide: class="voordoen" -->

Bepaal het 8-bits twee-complement van $17_d$

Notes:

**Uitwerking:**

1. Converteer naar binair: $17_d = 00010001_b$
2. Flip alle bits: `00010001` → `11101110`
3. Tel 1 op: `11101110` + `00000001` = `11101111`

Dus: $-17_d = 11101111_b$ (in 8-bits twee-complement)

**Controle:** 
$17_d + (-17_d)$ moet $0$ zijn
`00010001` + `11101111` = `100000000` (9 bits)
Woordlengte is 8 bits, dus de voorste 1 valt weg → `00000000` = $0$ ✓

***

## Oefenen: Twee-complement

Zet de volgende signed bytes om in negatieve getallen (8-bits twee-complement):

1. `01110011`
2. `00110101`

Werk de volgende sommen binair uit (8-bits woordlengte):

3. $26_d - 15_d$ (Tip: maak $-15_d$ en tel op)
4. $-31_d - 6_d$
5. $44_d - 56_d$

---

## Reflectie

- Waarom gebruikt de computer deze ingewikkelde methode?
- Wat is het voordeel van twee-complement boven "gewoon een minteken"?
- Delen jullie oplossingsmethoden

***

# 4. Hexadecimale getallen

***

## Waarom hexadecimaal?

Byte in binair: `11010111` → moeilijk te lezen 😵

Byte in decimaal: $215_d$ → kost 3 posities

Byte in **hexadecimaal**: `0xD7` → precies 2 posities! 🎉

Programmeurs gebruiken hex omdat het compact en overzichtelijk is

---

## Het hexadecimale stelsel

**16 cijfers:** 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, F

Tellen in hex: $0, 1, 2, ..., 9, A, B, C, D, E, F, 10, 11, ..., 1F, 20, ...$

**1 hex-cijfer** = **4 bits** = **1 nibble** (half-byte)

**2 hex-cijfers** = **8 bits** = **1 byte**

---

## Conversietabel

| Binair (4 bits) | Hex | Decimaal |
|:---------------:|:---:|:--------:|
| `0000` | `0` | 0 |
| `0001` | `1` | 1 |
| `0010` | `2` | 2 |
| `0011` | `3` | 3 |
| `0100` | `4` | 4 |
| `0101` | `5` | 5 |
| `0110` | `6` | 6 |
| `0111` | `7` | 7 |

---

## Conversietabel (vervolg)

| Binair (4 bits) | Hex | Decimaal |
|:---------------:|:---:|:--------:|
| `1000` | `8` | 8 |
| `1001` | `9` | 9 |
| `1010` | `A` | 10 |
| `1011` | `B` | 11 |
| `1100` | `C` | 12 |
| `1101` | `D` | 13 |
| `1110` | `E` | 14 |
| `1111` | `F` | 15 |

---

## Binair naar hex

Verdeel binair getal in groepen van 4 bits (van rechts!)

Voorbeeld: `11010111`

→ `1101` `0111`

→ `D` `7`

→ `0xD7`

***

## Voordoen: Hex conversie
<!-- .slide: class="voordoen" -->

Zet `10101010` om naar hexadecimaal

Notes:

**Uitwerking:**

1. Verdeel in groepen van 4: `1010` `1010`
2. Vertaal naar hex:
   - `1010` = 10 in decimaal = `A` in hex
   - `1010` = 10 in decimaal = `A` in hex
3. Resultaat: `0xAA`

**Andersom:** `0xCA` naar binair
- `C` = 12 = `1100`
- `A` = 10 = `1010`
- Resultaat: `11001010`

***

## Oefenen: Hexadecimaal

Zet de volgende bytes om naar hex:

1. `01110011`
2. `00110101`
3. `10001000`
4. `10101010`

Zet de volgende hex-getallen om naar binair:

5. `0xFA`
6. `0xCA`
7. `0xDE`
8. `0x5A`

---

## Reflectie

- Wanneer zou je hex gebruiken in plaats van binair of decimaal?
- Wat is het verband tussen hex en binair?
- Waarom is dit handig voor programmeurs?

***

## Afsluiting

Wat heb je geleerd?

- ✓ Binaire getallen omrekenen (heen en weer)
- ✓ Binaire getallen optellen
- ✓ Negatieve getallen (twee-complement)
- ✓ Hexadecimale notatie

**Volgende keer:** Logische operaties en schakelingen

---

## Huiswerk

Maak de oefenopgaven uit de syllabus:

- Hoofdstuk: Binair Rekenen voor Informatici
- Alle "Oefenen" secties
- Extra uitdaging: programmeer een binair-decimaal converter!

***

## Vragen?

Succes met oefenen!
