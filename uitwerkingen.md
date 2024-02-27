# Hoofdstuk 1 Binair Rekenen voor Informatici
## 1.1
1. 11
2. 45
3. 103
4. 150
5. 121
6. 266

## 1.2
1. 11001
2. 111111
3. 1100011
4. 11001000
5. 101000111

## 1.3
1. 101 + 110 = 1011
2. 11011 + 1010 = 100101
3. 1111 + 10101 = 100100
4. 111111 + 101010 = 1101001
5. 1010101 + 1100110 = 10111011

11011 + 11011 = 110110
101010 + 101010 = 1010100

Uitdaging #1: 1 0000 0100
Uitdaging #2: 26 * 3 = 110100

## 1.4
1. 2^8 = 256, 2^16 = 65.536, 2^32 = 4.294.967.296
2. 1000 1101
3. 1100 1011
4. 0111 1000
5. 0101 0110
6. 26 - 15 = 0001 1010 - 0000 1111 = 0001 1010 + 1111 0001 = 0000 1011 = 11_d
7. -31 - 6 = 1110 0001 - 0000 0110 = 1110 0001 + 1111 1010 = 1101 1011 = -37
8. foute opgave... past niet in 8 bits 2complement >128

## 1.5
1. 0x73
2. 0x35
3. 0x88
4. 0xAA
5. 0b11111010
6. 0b11001010
7. 0b11011110
8. 0b01011010

# Hoofdstuk 2
## 2.1
a. -
b. bijvoorbeeld: geen accenten, niet echt internationaal, geen emojis
c. UTF
d. uit 1 tot 4 bytes (in het geval van UTF 8)

## 2.2
-

## 2.3 - 2.6
Zie uitwerkingen van leerlingen

## 2.7
### 4 NAND
|S1|S2|R|
|-|-|-|
|0|0|1|
|0|1|1|
|1|0|1|
|1|1|0|

### 5 NOR
|S1|S2|R|
|-|-|-|
|0|0|1|
|0|1|0|
|1|0|0|
|1|1|0|

### 6 XNOR
|S1|S2|R|
|-|-|-|
|0|0|1|
|0|1|0|
|1|0|0|
|1|1|1|

### 7 Schakeling
R = (S1 && S2) XOR S2
|S1|S2| S1 && S2 | R |
|-|-|-|-|
|0|0|0|0|
|0|1|0|1|
|1|0|0|0|
|1|1|1|0|

## 2.8 
Zie vorige tabel 7 Schakeling

## 2.9
Een uitgang voor het resultaat, een uitgang voor de carry.

## 2.10
### 1 half adder tabel
|S1|S2|R1|R2|
|-|-|-|-|
|0|0|0|0|
|0|1|1|0|
|1|0|1|0|
|1|1|0|1|

`R1 = S1 XOR S2` en `R2 = S1 AND S2`

2. Er wordt geen carry van de vorige optelling meegenomen/komt niet in de waarheidstabel voor.
3. Carry van de vorige optelling meenemen

## 2.11
-

## 2.12
Waarheidstabel met 16 entries voor 4 variables

## Verdiepende opdrachten
1. NAND game
2. S1 OR S2 (De Morgan wet)
3. NOT((S1 XNOR S2) AND (S1 OR S2)) == S1 NAND S2
4. -

# Hoofdstuk 3
## 3.1
1. Kast
2. DVD drive
3. Harde schijf
4. Geheugen
5. Moederbord
6. Geluidskaart
7. Grafische kaart
8. CPU/processor/koeling
9. PSU

## 3.2
1. Processor opzoeken
2. Redenering over kracht processor: leeftijd, aantal cores, klok frequentie
3. Voordeel: snellere berekeningen, meer taken tegelijk. Nadeel: prijs, energieverbruik, warmte
4. Overklokken: sneller, warmer
5. Meerdere programma's tegelijk: drukkere processor, dus warmer
6. Koeltechniek: passieve koeling. Geen geluid, minder effectief. Actieve koeling met fan: geluid, effectiever. Waterkoeling: erg effectief, water in je computer
7. De wet van Moore zal niet voor altijd gelden: quantum tunnel effect.
8. CIR, MAR, MDR: Current instruction register, Memory Address Register en Memory Data Register. CIR op de CPU: dan kan de instructie sneller gedecodeerd worden. Geen verkeer op databus. MAR: address voor de databus, MDR: de inhoud van of naar het address in het MAR

## 3.3
1. SSD valt in de Harddisk plek. Na de RAM, voor de offline storage (tapes etc)
2. Werkgeheugen is erg kostbaar om een vergelijkbare hoeveelheid opslag te hebben. Bovendien is het vluchtig geheugen
3. -
4. 64 bits architectuur heeft een 64 bits databus, waardoor meer RAM geaddresseerd kan worden.
5. Het werkgeheugen raakt vol
6. Er kunnen geen nieuwe programma's gerund worden of er wordt virtual memory ingezet.
7. SAM
8. Moeilijke vraag...
9. -

# Hoofdstuk 4
## 4.1.1
Berekening ontleden (zie oefening 1)

operator 1: *, operanden: 2 en 10, antwoord: 20

operator 2: +, operanden: 4 en 20, antwoord: 24

Registers kiezen

register instellen: R1 = 2

instructie: `MOV R1, #2`

Instructies opschrijven

instructie 1: `MUL R1, #10 // 2 * 10`

instructie 2: `ADD R1, #4 // 20 + 4`

## 4.1.2
operator 1: *, operanden: 12 en 2, antwoord: 24

operator 2: +, operanden: 16 en 24, antwoord: 40

Registers kiezen

register instellen: R1 = 12

instructie: `MOV R1, #12`

Instructies opschrijven

instructie 1: `MUL R1, #2 // 12 * 2`

instructie 2: `ADD R1, #16 // 24 + 16`

## 4.1.3
operator 1: +, operanden: 16 en 12, antwoord: 28

operator 2: %, operanden: 28 en 3, antwoord: 1

Registers kiezen

register instellen: R1 = 16

instructie: `MOV R1, #16`

Instructies opschrijven

instructie 1: `ADD R1, #12 // 16 + 12`

instructie 2: `MOD R1, #3 // 28 % 3`

## 4.1.4
operator 1: *, operanden: 2 en 3, antwoord: 6

operator 2: *, operanden: 6 en 5, antwoord: 30

Registers kiezen

register instellen: R1 = 2

instructie: `MOV R1, #2`

Instructies opschrijven

instructie 1: `MUL R1, #3 // 2 + 3`

instructie 2: `MUL R1, #5 // 6 * 5`

## 4.1.5
operator 1: *, operanden: 14 en 2, antwoord: 28

operator 2: *, operanden: 28 en 3, antwoord: 84

operator 3: -, operanden: 84 en 15, antwoord: 69

Registers kiezen

register instellen: R1 = 14

instructie: `MOV R1, #14`

Instructies opschrijven

instructie 1: `MUL R1, #2 // 14 * 2`

instructie 2: `MUL R1, #3 // 28 * 3`

instructie 3: `SUB R1, #15 // 84 - 15`

### 4.1.6
operator 1: *, operanden: 4 en 4, antwoord: 16

operator 2: *, operanden: 16 en 6, antwoord: 96

operator 3: %, operanden: 96 en 2, antwoord: 0

Registers kiezen

register instelen: R1 = 4

instructie: `MOV R1, #4`

Instructie opschrijven

instructie 1: `MUL R1, #4 // 4 * 4`

instructie 2: `MUL R1, #6 // 16 * 6`

instructie 3: `DIV R1, #2 // 96 % 2`

## 4.2.a Machinetaal decoden
`00110 101 00101010`
| | instructiedeel | |
|-|-|-|
|opcode| `00110` | `&` |
|Rsd | `101` | 5|
|imm8 | `00101010` | 170 |
|betekenis | | R5 = R5 & 170 |
| resultaat | | $10_d$, want `0001 1011 & 1010 1010 == 0000 1010` |

## 4.2.b Machinetaal decoden
`00011 110 00101010`
| | instructiedeel | |
|-|-|-|
|opcode| `00011` | `-` |
|Rsd | `110` | 6|
|imm8 | `00101010` | 170 |
|betekenis | | R6 = R6 - 170 |
| resultaat | | $-121_d$ want 49-170=-121 |

## 4.2.c Machinetaal decoden
`00001 010 00000100`
| | instructiedeel | |
|-|-|-|
|opcode| `00001` | `%` |
|Rsd | `010` | 2|
|imm8 | `00000100` | 4 |
|betekenis | | R2 = R2 % 4 |
| resultaat | | $3_d$ want 11%4=3 |

## 4.2.d Machinetaal decoden
`00010 100 00011000`
| | instructiedeel | |
|-|-|-|
|opcode| `00010` | `+` |
|Rsd | `100` | 4|
|imm8 | `00011000` | 24 |
|betekenis | | R4 = R4 + 24 |
| resultaat | | $31_d$ want 7 + 24 = 31 |

## 4.2.e Machinetaal decoden
`00111 011 00001110`
| | instructiedeel | |
|-|-|-|
|opcode| `00111` | `OR` |
|Rsd | `011` | 3|
|imm8 | `00001110` | 14 |
|betekenis | | R3 = R3 \|\| 14  |
| resultaat | | $30_d$ want `0001 0100 \|\| 0000 1110 = 0001 1110` = $30_d$|


## 4.2.f Machinetaal decoden
`00001 001 00000101`
| | instructiedeel | |
|-|-|-|
|opcode| `00001` | `%` |
|Rsd | `001` | 1|
|imm8 | `0000 0101` | 5 |
|betekenis | | R1 = R1 % 5 |
| resultaat | | 1 want 16 % 5 = 1 |

## 4.3 Machinetaal schrijven

| | opg | asm | Bin | Hex |
|-|-|-|-|-|
|a|7%3| `MOV R1, #7`</br>`MOD R1, #3`| `00101 001 0000 0111`</br>`00001 001 0000 0011`| `0x2907`</br>`0x0903`|
|b|11+14| `MOV R1, #11`</br>`ADD R1, #14` |`00101 001 0000 1011`</br>`00010 001 0000 1110`|`0x290B`</br>`0x110E`|
|c|49-25| `MOV R1, 49`</br>`SUB R1, #25`|`00101 001 0011 0001`</br>`00011 001 0001 1001`|`0x2931`</br>`0x1919`|
|d|20%6| `MOV R1, #20`</br>`MOD R1, #6`|`00101 001 0001 0100`</br>`00001 001 0000 0110`|`0x2914`</br>`0x0906`|
|e|16+16| `MOV R1, #16`</br>`ADD R1, #163`|`00101 001 0001 0000`</br>`00010 001 0001 0000`|`0x2910`</br>`0x1110`|
|f|8&29| `MOV R1, #8`</br>`AND R1, #29`|`00101 001 0000 1000`</br>`00110 001 0001 1101`|`0x2908`</br>`0x311D`|

## 4.4 Machinetaal decoderen

| instructie | opcode | rsd | imm8 | asm |
|-|-|-|-|-|
|`00101 011 00000111`|`00101`|`011`|`0000 0111`| `MOV R3, #7`|
|`00010 011 00001111`|`00010`|`011`|`0000 1111`| `ADD R3, #15`|
|`00001 011 00000100`|`00001`|`011`|`0000 0100`| `MOD R3, #4`|
|`00101 101 00001011`|`00101`|`101`|`0000 1011`| `MOV R5, #11`|
|`00011 101 00000110`|`00011`|`101`|`0000 0110`| `SUB R5, #6`|
|`00000 000 00000000`|`00000`|`000`|`0000 0000`| `HLT`|

```
MOV R3, #7
ADD R3, #15
MOD R3, #4
MOV R5, #11
SUB R5, #6
HLT
```

# Hoofdstuk 5
