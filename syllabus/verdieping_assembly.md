(hoofdstuk_meer_machinetaal)=
# Meer machinetaal

In de hoofdstukken {doc}`machinetaal_instructies`, {doc}`machinetaal_gegevens` en {doc}`machinetaal_programma` heb je geleerd hoe je assembly code schrijft voor de RISC-simulator. Je hebt instructies leren kennen, geleerd werken met het geheugen, en gezien hoe je programmabesturing (branching) toepast. In dit verdiepingshoofdstuk gaan we dieper in op enkele geavanceerde onderwerpen die je nog meer controle geven over de processor.

## Flags: de toestandsregisters van de processor

Wanneer de ALU (Arithmetic Logic Unit) een berekening uitvoert, gebeurt er meer dan alleen het berekenen van een resultaat. De ALU houdt ook bij *hoe* de berekening verliep. Dat doet hij door speciale bits te zetten in het **flags register**. Deze flags (vlaggen) geven belangrijke informatie over het resultaat van de laatste ALU-operatie.

### De vier flags

De RISC-simulator heeft vier flags:

| Flag | Naam | Betekenis |
|:----:|:-----|:----------|
| **N** | Negative | Het resultaat is negatief (MSB = 1) |
| **Z** | Zero | Het resultaat is nul |
| **C** | Carry | Er was een carry (unsigned overflow) |
| **V** | oVerflow | Er was een overflow (signed overflow) |

Je kunt deze flags zien in de simulator rechtsbovenin, vlakbij de ALU.

### De N-flag (Negative)

De N-flag wordt gezet als de Most Significant Bit (MSB) van het resultaat `1` is. Bij twee-complement notatie betekent dit dat het getal negatief is.

**Voorbeeld:**
```assembly
MOV R1, #5
SUB R1, #10    // R1 = 5 - 10 = -5
```

Na de `SUB`-instructie:
- R1 bevat `0xFFFB` (binair: `1111 1111 1111 1011`)
- De MSB is `1`, dus **N = 1**
- In twee-complement is dit $-5$

### De Z-flag (Zero)

De Z-flag wordt gezet als het resultaat precies `0` is.

**Voorbeeld:**
```assembly
MOV R1, #7
SUB R1, #7     // R1 = 7 - 7 = 0
```

Na de `SUB`-instructie:
- R1 bevat `0x0000`
- **Z = 1**

De Z-flag is heel nuttig bij loops. Je kunt bijvoorbeeld een teller naar beneden laten tellen en stoppen wanneer deze 0 bereikt.

### De C-flag (Carry)

De C-flag geeft aan of er een *carry* (overdracht) was bij de laatste berekening, als je de getallen als **unsigned** (zonder teken) beschouwt.

**Bij optellen:**

```assembly
MOV R1, #65535    // 0xFFFF (het maximale 16-bit getal)
ADD R1, #1        // 65535 + 1
```

Het resultaat past niet in 16 bits! De berekening is eigenlijk:
```
  1111 1111 1111 1111  (65535)
+                    1
= 1 0000 0000 0000 0000
```

De voorste `1` past niet meer in 16 bits en gaat verloren. Die wordt opgeslagen in de C-flag:
- R1 bevat `0x0000` (alleen de onderste 16 bits)
- **C = 1** (er was een carry)
- **Z = 1** (het resultaat in R1 is 0)

**Bij aftrekken:**

Bij aftrekken werkt de C-flag anders: **C = 1** betekent *geen borrow* (de berekening was mogelijk zonder te lenen). **C = 0** betekent dat er wel geleend moest worden.

```assembly
MOV R1, #5
SUB R1, #3     // 5 - 3 = 2
```
- **C = 1** (geen borrow nodig)

```assembly
MOV R1, #3
SUB R1, #5     // 3 - 5 = -2 (als unsigned: 65534)
```
- **C = 0** (er was een borrow)

### De V-flag (oVerflow)

De V-flag geeft aan of er een overflow was bij de laatste berekening, als je de getallen als **signed** (met teken, in twee-complement) beschouwt.

**Wanneer treedt signed overflow op?**

Signed overflow treedt op als:
- Twee **positieve** getallen optellen en een **negatief** resultaat krijgen
- Twee **negatieve** getallen optellen en een **positief** resultaat krijgen
- Een **negatief** getal van een **positief** getal aftrekken en een **negatief** resultaat krijgen
- Een **positief** getal van een **negatief** getal aftrekken en een **positief** resultaat krijgen

**Voorbeeld van signed overflow:**

```assembly
MOV R1, #32767    // Maximale positieve 16-bit signed getal (0x7FFF)
ADD R1, #1        // 32767 + 1
```

Het resultaat zou $32768$ moeten zijn, maar in 16-bit twee-complement is het maximale positieve getal $32767$ ($2^{15}-1$). Het resultaat wordt:
```
  0111 1111 1111 1111  (+32767)
+                    1
= 1000 0000 0000 0000  (-32768 in twee-complement!)
```

- R1 bevat `0x8000` (wordt geïnterpreteerd als $-32768$)
- **V = 1** (signed overflow!)
- **N = 1** (het resultaat lijkt negatief)
- **C = 0** (geen unsigned overflow)

**Verschil tussen C en V:**

- **C-flag**: Voor **unsigned** getallen (0 tot 65535)
- **V-flag**: Voor **signed** getallen (-32768 tot 32767)

:::{exercise}

Voorspel de waarden van de flags (N, Z, C, V) na de volgende instructies. Test je antwoorden in de [RISC-simulator](https://peterhigginson.co.uk/RISC/).

1. ```assembly
   MOV R1, #10
   SUB R1, #10
   ```

2. ```assembly
   MOV R1, #255
   ADD R1, #255
   ```

3. ```assembly
   MOV R1, #0
   SUB R1, #1
   ```

4. ```assembly
   MOV R1, #32767
   ADD R1, #32767
   ```

5. Leg uit waarom bij opdracht 4 de C-flag en V-flag verschillende waarden hebben.

6. Schrijf een stuk assembly code dat **alleen** de N-flag en V-flag op 1 zet, maar Z en C op 0 laat.

:::

### Flags en branch-instructies

De branch-instructies die je kent (`BEQ`, `BNE`, `BLT`, `BGT`, etc.) kijken eigenlijk naar de flags! Laten we kijken hoe dat werkt.

**BEQ (Branch if Equal)**
- Springt als **Z = 1**
- Na een `CMP R1, R2` betekent Z=1 dat R1 == R2

**BNE (Branch if Not Equal)**
- Springt als **Z = 0**
- Na een `CMP R1, R2` betekent Z=0 dat R1 ≠ R2

**BLT (Branch if Less Than)** - voor signed getallen
- Springt als **N ≠ V**
- Als N en V verschillend zijn, was er of een negatief resultaat zonder overflow, of een overflow die het teken omkeerde

**BGT (Branch if Greater Than)** - voor signed getallen
- Springt als **Z = 0** EN **N = V**
- Het resultaat is niet nul, en N en V zijn gelijk (betekent: geen overflow die het teken omkeerde)

**BLE (Branch if Less or Equal)** - voor signed getallen
- Springt als **Z = 1** OF **N ≠ V**

**BGE (Branch if Greater or Equal)** - voor signed getallen
- Springt als **N = V**

```{mermaid}
graph TD
    CMP[CMP R1, R2] --> ALU[ALU berekent R1 - R2]
    ALU --> FLAGS[Zet N, Z, C, V flags]
    FLAGS --> BEQ{BEQ?}
    FLAGS --> BNE{BNE?}
    FLAGS --> BLT{BLT?}
    FLAGS --> BGT{BGT?}
    
    BEQ --> |Z = 1| JUMP1[Spring naar label]
    BNE --> |Z = 0| JUMP2[Spring naar label]
    BLT --> |N ≠ V| JUMP3[Spring naar label]
    BGT --> |Z=0 EN N=V| JUMP4[Spring naar label]
```

**Voorbeeld: Maximum van twee getallen**

```assembly
// Bepaal het maximum van twee getallen
    LDR R1, num1     // Laad eerste getal
    LDR R2, num2     // Laad tweede getal
    CMP R1, R2       // Vergelijk: R1 - R2
    BGT r1_greater   // Als R1 > R2, spring naar r1_greater
    MOV R3, R2       // Anders: R3 = R2
    BRA done
r1_greater:
    MOV R3, R1       // R3 = R1
done:
    STR R3, maximum  // Sla maximum op
    HLT

num1: DAT 42
num2: DAT 17
maximum: DAT 0
```

Laten we stap voor stap bekijken wat er gebeurt:
1. `CMP R1, R2` berekent $42 - 17 = 25$
2. Het resultaat is positief, dus **N = 0**
3. Het resultaat is niet nul, dus **Z = 0**
4. Er was geen overflow, dus **V = 0**
5. `BGT` controleert: is Z = 0 EN N = V? Ja! (Z=0 en N=V=0)
6. De branch wordt genomen, R3 krijgt de waarde 42

:::{exercise}

1. Schrijf een programma dat drie getallen vergelijkt en het kleinste getal vindt. Gebruik `CMP` en de juiste branch-instructie.

2. Schrijf een programma dat een getal in R1 heeft en controleert of het:
   - Positief is (branch naar `positive`)
   - Negatief is (branch naar `negative`)
   - Nul is (branch naar `zero`)

3. **Uitdaging:** Schrijf een programma dat een absolute waarde berekent (als het getal negatief is, maak het positief). 
   
   Tip: Het negeren van een getal in twee-complement doe je door alle bits te flippen (met `MVN`) en er 1 bij op te tellen.

4. Leg uit waarom je voor unsigned vergelijkingen andere branch-instructies zou willen hebben dan voor signed vergelijkingen.

:::

### Instructies die flags beïnvloeden

Niet alle instructies veranderen de flags! Hier is een overzicht:

**Instructies die flags zetten:**
- Alle rekenkundige instructies: `ADD`, `SUB`, `MUL`, `DIV`, `MOD`
- Logische instructies: `AND`, `ORR`, `EOR`, `MVN`
- Shift instructies: `LSL`, `LSR`, `ASR`
- Vergelijkingen: `CMP`

**Instructies die flags NIET veranderen:**
- Data transfer: `MOV`, `LDR`, `STR`
- I/O: `INP`, `OUT`
- Branch instructies: `BRA`, `BEQ`, `BNE`, etc.
- Adresberekeningen voor SP (stack pointer)

**Waarom is dit belangrijk?**

Stel je doet een vergelijking en wilt daarna iets uit het geheugen laden voordat je brancht:

```assembly
    CMP R1, #10      // Flags worden gezet
    LDR R2, value    // Flags blijven onveranderd!
    BGT greater      // Branch gebruikt nog steeds de flags van CMP
```

Maar let op bij dit voorbeeld:

```assembly
    CMP R1, #10      // Flags worden gezet
    ADD R2, #5       // Flags worden OVERSCHREVEN!
    BGT greater      // Branch gebruikt nu de flags van ADD, niet van CMP!
```

:::{exercise}

1. Leg uit waarom de volgende code niet werkt zoals bedoeld:
   ```assembly
       LDR R1, num1
       LDR R2, num2
       CMP R1, R2
       MOV R3, #100    
       ADD R3, #50     
       BGT num1_greater
   ```

2. Herstel de code uit vraag 1 zodat deze wel correct werkt.

3. **Onderzoeksproject:** In sommige processoren (zoals ARM) hebben instructies een optionele "S" suffix (bijv. `ADDS` in plaats van `ADD`) om aan te geven of flags gezet moeten worden. Zoek uit waarom dit nuttig kan zijn.

:::

## Strings printen: werken met karakters

Tot nu toe heb je alleen met getallen gewerkt. Maar computers moeten ook tekst kunnen verwerken! In dit onderdeel leer je hoe je strings (reeksen van karakters) kunt opslaan en printen in assembly.

### ASCII: karakters als getallen

Elk karakter (letter, cijfer, leesteken) heeft een nummer. De standaard die hiervoor gebruikt wordt heet **ASCII** (American Standard Code for Information Interchange). Je hebt dit al gezien in {doc}`logica`.

Enkele voorbeelden:
- `'A'` = 65 (of `0x41` in hexadecimaal)
- `'B'` = 66 (of `0x42`)
- `'a'` = 97 (of `0x61`)
- `'0'` = 48 (of `0x30`)
- `' '` (spatie) = 32 (of `0x20`)
- `'\n'` (newline) = 10 (of `0x0A`)

### Één karakter printen

In de RISC-simulator kun je een karakter printen door een getal naar device 7 te sturen:

```assembly
    MOV R1, #65      // ASCII voor 'A'
    OUT R1, 7        // Print naar device 7 (character output)
    HLT
```

Dit print de letter `A` in het output venster.

```assembly
    MOV R1, #72      // 'H'
    OUT R1, 7
    MOV R1, #105     // 'i'
    OUT R1, 7
    MOV R1, #33      // '!'
    OUT R1, 7
    MOV R1, #10      // '\n' (newline)
    OUT R1, 7
    HLT
```

Dit print `Hi!` gevolgd door een nieuwe regel.

### Een string opslaan in het geheugen

Een string is een reeks van karakters. We kunnen deze opslaan in het geheugen met `DAT`:

```assembly
    // ... programma ...
    HLT

// Data section
message:
    DAT 72    // 'H'
    DAT 101   // 'e'
    DAT 108   // 'l'
    DAT 108   // 'l'
    DAT 111   // 'o'
    DAT 0     // Null-terminator (einde van string)
```

De `0` aan het einde noemen we de **null-terminator**. Dit markeert het einde van de string.

### Een string printen met een loop

Om een string te printen, moeten we:
1. Door de karakters heen lopen
2. Elk karakter laden en printen
3. Stoppen bij de null-terminator (0)

**Methode 1: Direct adresseren (inefficiënt)**

```assembly
    LDR R1, message+0   // Laad 'H'
    OUT R1, 7
    LDR R1, message+1   // Laad 'e'
    OUT R1, 7
    LDR R1, message+2   // Laad 'l'
    OUT R1, 7
    // ... etc
    HLT

message:
    DAT 72    // 'H'
    DAT 101   // 'e'
    DAT 108   // 'l'
    DAT 108   // 'l'
    DAT 111   // 'o'
    DAT 0
```

Dit werkt, maar is heel omslachtig! Voor elke letter een aparte instructie.

**Methode 2: Indexed addressing (efficiënt!)**

De RISC-simulator ondersteunt **indexed addressing**: `LDR Rd, label(Rn)`. Dit betekent: laad van adres `label + waarde_in_Rn`.

```assembly
    MOV R2, #0          // R2 = index (start bij 0)
loop:
    LDR R1, message(R2) // Laad karakter op positie message + R2
    CMP R1, #0          // Is het de null-terminator?
    BEQ done            // Ja? Stop.
    OUT R1, 7           // Print het karakter
    ADD R2, #1          // Verhoog index
    BRA loop            // Herhaal
done:
    HLT

message:
    DAT 72    // 'H'
    DAT 101   // 'e'
    DAT 108   // 'l'
    DAT 108   // 'l'
    DAT 111   // 'o'
    DAT 0
```

**Hoe werkt dit?**

1. R2 start op 0
2. `LDR R1, message(R2)` laadt van adres `message + 0` → karakter 'H'
3. We printen 'H'
4. R2 wordt 1
5. `LDR R1, message(R2)` laadt van adres `message + 1` → karakter 'e'
6. We printen 'e'
7. ... enzovoort
8. Als we karakter 0 laden, stoppen we

### Meerdere strings

Je kunt meerdere strings in het geheugen zetten:

```assembly
    MOV R2, #0
print_hello:
    LDR R1, hello(R2)
    CMP R1, #0
    BEQ print_world_start
    OUT R1, 7
    ADD R2, #1
    BRA print_hello

print_world_start:
    MOV R1, #32         // Spatie
    OUT R1, 7
    MOV R2, #0
print_world:
    LDR R1, world(R2)
    CMP R1, #0
    BEQ done
    OUT R1, 7
    ADD R2, #1
    BRA print_world

done:
    HLT

hello:
    DAT 72    // 'H'
    DAT 101   // 'e'
    DAT 108   // 'l'
    DAT 108   // 'l'
    DAT 111   // 'o'
    DAT 0

world:
    DAT 87    // 'W'
    DAT 111   // 'o'
    DAT 114   // 'r'
    DAT 108   // 'l'
    DAT 100   // 'd'
    DAT 0
```

Dit print: `Hello World`

:::{exercise}

1. Schrijf een programma dat je eigen naam print.

2. Schrijf een programma dat de volgende output geeft:
   ```
   Hello
   World
   ```
   (Twee regels, dus gebruik de newline karakter 10)

3. Schrijf een programma dat een string achterste voren print. Begin met de lengte van de string te tellen, en print dan vanaf het einde.

4. **Uitdaging:** Schrijf een programma dat alle HOOFDLETTERS in een string omzet naar kleine letters voordat het de string print.
   
   Tip: Het verschil tussen een hoofdletter en de bijbehorende kleine letter is altijd 32. ('A' = 65, 'a' = 97, verschil = 32)

5. **Project:** Schrijf een functie die twee strings vergelijkt en checkt of ze identiek zijn. Print "EQUAL" of "NOT EQUAL".

:::

### Hexadecimale notatie voor ASCII

Het is vervelend om steeds ASCII-codes op te zoeken. Gelukkig accepteert de RISC-simulator hexadecimale getallen. De ASCII-waarden voor letters zijn gemakkelijker te onthouden in hex:

| Karakter | Decimaal | Hexadecimaal |
|:--------:|:--------:|:------------:|
| 'A' - 'Z' | 65-90 | 0x41-0x5A |
| 'a' - 'z' | 97-122 | 0x61-0x7A |
| '0' - '9' | 48-57 | 0x30-0x39 |
| ' ' (spatie) | 32 | 0x20 |
| '\n' | 10 | 0x0A |

Dus je kunt schrijven:

```assembly
message:
    DAT 0x48    // 'H'
    DAT 0x65    // 'e'
    DAT 0x6C    // 'l'
    DAT 0x6C    // 'l'
    DAT 0x6F    // 'o'
    DAT 0
```

### Getallen als strings printen

Wat als je een getal (zoals 123) als tekst wilt printen? Je moet elk cijfer omzetten naar zijn ASCII-waarde!

Het cijfer `'0'` heeft ASCII-waarde 48. Het cijfer `'1'` heeft 49, etc.

Dus: `cijfer_als_ascii = cijfer + 48`

**Voorbeeld: print een enkel cijfer**

```assembly
    MOV R1, #5          // Het cijfer 5
    ADD R1, #48         // Converteer naar ASCII ('5' = 53)
    OUT R1, 7           // Print '5'
    HLT
```

**Een twee-cijferig getal printen:**

Voor een getal zoals 42:
1. Deel door 10: 42 / 10 = 4 (tiental)
2. Modulo 10: 42 % 10 = 2 (eenheid)
3. Converteer beide naar ASCII en print

```assembly
    MOV R1, #42         // Het getal
    MOV R2, #10         
    
    // Bereken tiental (4)
    DIV R3, R1, R2      // R3 = 42 / 10 = 4
    ADD R3, #48         // ASCII voor '4'
    OUT R3, 7           // Print '4'
    
    // Bereken eenheid (2)
    MOD R1, R2          // R1 = 42 % 10 = 2
    ADD R1, #48         // ASCII voor '2'
    OUT R1, 7           // Print '2'
    
    HLT
```

:::{exercise}

1. Schrijf een programma dat een drie-cijferig getal (bijvoorbeeld 456) print als tekst.

2. **Uitdaging:** Schrijf een algemene functie die een willekeurig getal (0-65535) print als decimaal getal. Je moet:
   - De cijfers van rechts naar links bepalen (met herhaald delen door 10)
   - Ze opslaan (in omgekeerde volgorde)
   - Ze vervolgens in de juiste volgorde printen

3. **Project:** Schrijf een programma dat de ASCII-tabel print (karakters 32 tot 126). Formaat:
   ```
   32: 
   33:!
   34:"
   ...
   ```

:::

## Branching in machinetaal: hoe het echt werkt

Je hebt geleerd om branch-instructies te gebruiken in assembly, zoals `BEQ loop`. Maar hoe werkt dit eigenlijk in machinetaal (in binaire vorm)?

### Instructieformaten in de RISC-simulator

De RISC-simulator gebruikt 16-bit instructies. Verschillende instructies hebben verschillende formaten. Je hebt in het hoofdstuk over machinetaal al gezien hoe instructies zijn opgebouwd uit velden (opcode, registers, operand).

Voor branch-instructies zijn er twee belangrijke formaten:

**Format voor korte branches (zoals BEQ, BNE, BLT, BGT):**

```
| opcode (7 bits) | offset (9 bits) |
```

De 9 bits voor offset geven aan hoeveel posities vooruit of achteruit gesprongen moet worden (relatieve addressing).

**Format voor BRA (branch always):**

```
| opcode (4 bits) | address (12 bits) |
```

De 12 bits geven een absoluut adres aan (direct addressing).

### Relatieve vs absolute addressing

**Absolute addressing** (`BRA`):
- Het adres staat direct in de instructie
- `BRA 100` springt naar adres 100
- Voordeel: Kan overal heen springen (binnen 12-bit bereik: 0-4095)
- Nadeel: Als je code verplaatst, moet je alle adressen aanpassen

**Relatieve addressing** (conditionele branches):
- De offset ten opzichte van het huidige adres staat in de instructie
- `BEQ +5` springt 5 posities vooruit ten opzichte van het huidige PC
- Voordeel: Code is "relocatable" (kan verplaatst worden zonder aanpassingen)
- Nadeel: Beperkt bereik (9 bits signed: -256 tot +255)

**Hoe relatieve addressing werkt:**

```
Adres  Instructie         PC na fetch
10     MOV R1, #5         11
11     loop: ADD R1, #1   12
12     CMP R1, #10        13
13     BNE loop           14
14     HLT
```

Bij adres 13: `BNE loop`
- `loop` is op adres 11
- PC staat nu op 14 (al verhoogd door fetch)
- Offset = 11 - 14 = -3
- De instructie bevat dus offset `-3`
- Bij uitvoeren: PC = 14 + (-3) = 11 ✓

### De assembler doet het werk

Gelukkig hoef je dit niet zelf te berekenen! De assembler:
1. Kent alle labels en hun adressen
2. Berekent automatisch de juiste offset
3. Kiest het juiste instructieformaat
4. Converteert naar machinetaal

**Voorbeeld: Van assembly naar machinetaal**

```assembly
      MOV R1, #0
loop: ADD R1, #1
      CMP R1, #10
      BNE loop
      HLT
```

De assembler maakt hiervan (vereenvoudigd):

```
Adres  Assembly        Machinetaal (hex)  Toelichting
0      MOV R1, #0      0x2100             Format A: MOV
1      ADD R1, #1      0x3101             Format A: ADD  
2      CMP R1, #10     0x490A             Format A: CMP
3      BNE loop        0x9FFD             Format B: BNE met offset -3
4      HLT             0xE000             Format: HLT
```

Bij adres 3:
- Opcode voor `BNE` (ongeveer `1001`)
- Offset = 1 - 4 = -3 (in binair two's complement: `1111111101`)
- Samen: `1001 1111 1111 1101` = `0x9FFD`

### Forward references

Een interessant probleem: wat als je naar een label springt dat *later* in de code komt?

```assembly
      BEQ forward
      MOV R1, #5
      HLT
forward:
      MOV R2, #10
      HLT
```

Bij de eerste pass weet de assembler nog niet waar `forward` is! Oplossing:

**Two-pass assembler:**
1. **Eerste pass**: Verzamel alle labels en hun adressen
2. **Tweede pass**: Genereer de machinetaal met de nu bekende adressen

### Bereikbeperkingen

Omdat relatieve branches maar 9 bits hebben voor de offset, kun je maar beperkt ver springen:

- 9 bits signed: $-256$ tot $+255$
- Als je verder moet springen, krijg je een assembler error!

**Oplossing: Use BRA:**

```assembly
      CMP R1, #10
      BGT far_away    // Error: te ver!
      // ... veel code ...
      // (meer dan 255 instructies)
far_away:
      MOV R2, #100
```

Oplossing met `BRA`:

```assembly
      CMP R1, #10
      BLE skip        // Omgedraaide conditie!
      BRA far_away    // BRA heeft 12 bits, kan verder
skip:
      // ... veel code ...
far_away:
      MOV R2, #100
```

### De machinetaal bekijken in de simulator

In de RISC-simulator kun je de machinetaal zien:
1. Schrijf je assembly code
2. Klik "Submit"
3. De memory view toont de machinetaal (in hex of binary)
4. Selecteer "hex" onder OPTIONS om het hexadecimaal te zien

Probeer eens:
```assembly
loop: ADD R1, #1
      BRA loop
```

Kijk naar de machinetaal voor `BRA loop`. Zie je het adres van `loop` in de instructie?

:::{exercise}

1. Wat is het verschil tussen absolute en relatieve addressing voor branches?

2. Leg uit waarom de assembler twee passes moet doen.

3. Schrijf de volgende code en bekijk de machinetaal in de simulator:
   ```assembly
       MOV R1, #0
   loop:
       ADD R1, #1
       CMP R1, #5
       BNE loop
       HLT
   ```
   Noteer de hexadecimale waarde van de `BNE loop` instructie. Kun je de offset herkennen?

4. Wat gebeurt er als je probeert te branchen naar een label dat meer dan 255 instructies weg is? Test dit.

5. **Uitdaging:** Bereken handmatig de machinetaal voor:
   ```assembly
       BEQ target
       MOV R1, #5
       MOV R2, #10
   target:
       HLT
   ```
   Controleer je antwoord in de simulator.

6. **Onderzoeksproject:** Sommige processoren hebben een "branch prediction" mechanisme. Zoek uit wat dit is en waarom het nuttig is voor prestaties.

:::

### Subroutines en de call stack (geavanceerd)

Een belangrijk gebruik van branching is het aanroepen van **subroutines** (functies). Hoewel de basis RISC-simulator geen ingebouwde call/return instructies heeft, kun je dit wel zelf implementeren met `BRA` en een **stack**.

**Probleem: Hoe keer je terug?**

```assembly
main:
    MOV R1, #5
    BRA print_number    // Spring naar functie
    // Hoe kom je hier terug?
    HLT

print_number:
    OUT R1, 4
    // Hoe spring je terug naar main?
```

**Oplossing: Sla het return adres op**

Je moet onthouden *waar* je vandaan kwam zodat je terug kunt:

```assembly
main:
    MOV R1, #5
    MOV R7, #3          // R7 = return address (adres na BRA)
    BRA print_number
    // Adres 3: hier komen we terug
    HLT

print_number:
    OUT R1, 4
    MOV PC, R7          // Spring terug (verander Program Counter)
```

Dit werkt! Maar met geneste functie-aanroepen (functie A roept B roept C) moet je meerdere return adressen onthouden. Daarvoor gebruik je een **stack**.

De stack is een gebied in het geheugen waar je return adressen (en andere data) tijdelijk opslaat:

```assembly
// Stack pointer in R6
    MOV R6, #1000       // Stack start op adres 1000

// Call een functie
call_func:
    MOV R7, #...        // Return adres
    STR R7, 0(R6)       // Push return adres op stack
    SUB R6, #1          // Verlaag stack pointer
    BRA function

// Return van een functie
return:
    ADD R6, #1          // Verhoog stack pointer
    LDR R7, 0(R6)       // Pop return adres van stack
    MOV PC, R7          // Spring terug
```

:::{exercise}

1. Leg uit waarom je een stack nodig hebt voor geneste functie-aanroepen.

2. **Project:** Schrijf een programma met een subroutine `multiply_by_10` die:
   - De waarde in R1 met 10 vermenigvuldigt
   - Terugkeert naar de aanroeper
   - Gebruik R7 voor het return adres

3. **Geavanceerd project:** Implementeer een recursive functie voor faculteit berekening (n!) met behulp van een stack voor return adressen en parameters.

:::

## Afsluiting

In dit verdiepingshoofdstuk heb je geleerd over:

- **Flags**: Hoe de processor de toestand van berekeningen bijhoudt met N, Z, C en V flags, en hoe branch-instructies deze gebruiken
- **Strings printen**: Hoe je karakters en tekst verwerkt met ASCII-codes, indexed addressing en loops
- **Branching in machinetaal**: Hoe branch-instructies echt werken met relatieve en absolute addressing, en de beperkingen hiervan

Deze geavanceerde concepten geven je veel meer controle over de processor en laten zien hoe hogere programmeertalen (met strings, if-statements, en functies) vertaald worden naar machinetaal.

Je hebt nu een solide basis in assembly programmeren voor de RISC-architectuur. In een vervolgopleiding zou je kunnen leren over:
- Interrupts en exception handling
- Memory-mapped I/O
- DMA (Direct Memory Access)
- Pipeline hazards en branch prediction
- Meer geavanceerde instructiesets (ARM, x86, RISC-V)