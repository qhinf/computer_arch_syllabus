(hoofdstuk-binair)=
# Binair Rekenen voor Informatici

Je werkt waarschijnlijk al een tijdje met computers en andere digitale apparatuur. In de kern verwerken computers gegevens door middel van electronische schakelingen. In deze schakelingen kunnen slechts twee toestanden te onderscheiden. _Aan_ of _Uit_. Deze toestanden worden weergegeven als 0 en 1. Deze twee cijfers vormen de basis voor wat we _binair rekenen_ noemen. In deze lesmodule leer je wat binaire getallen zijn, hoe je binaire getallen kunt omrekenen naar decimale getallen en omgekeerd, hoe je binaire getallen bij elkaar kunt optellen en hoe je hoe je negatieve getallen kan representeren.

## Binaire getallen
Je bent al sinds de basisschool gewend om getallen op te schrijven met een combinatie van de cijfers 0 t/m 9. Je hebt hiermee leren optellen, de tafels uit je hoofd geleerd (en misschien nu weer vergeten), je hebt geleerd dat de volgorde van de cijfers een verschil maakt. Ik bedoel er is toch een behoorlijk verschil tussen een 8,3 of een 3,8 als resultaat van een toets. We gebruiken dus 10 verschillende symbolen, die we cijfers noemen, om onze getallen op te schrijven. Met een moeilijk woord noemen we dit het _decimale stelsel_ of _tientallige stelsel_. In dit decimale stelsel rekenen we met decimale getallen. In het _binaire stelsel_ rekenen we met _binaire getallen_. Het enige verschil tussen het decimale talstelsel en het binaire talstelsel is dat we niet met tien cijfers, maar met twee cijfers rekenen. De `0` en de `1`. Voorbeelden van binaire getallen zijn `10100` en `11001`. De binaire getallen zijn dus op dezelfde manier opgebouwd als decimale getallen, maar dan met minder cijfers.

Wanneer je binair gaat tellen ziet dat er zo uit: `0, 1, 10, 11, 100, 101, 110, 111, ...`. Dit is hetzelfde als $0, 1, 2, 3, 4, 5, 6, 7, \ldots$ in de decimale notatie. Je telt eigenlijk net zoals bij het decimale stelsel, alleen zijn de cijfers eerder 'op'.

## Van binair naar decimaal
Net zoals decimale getallen staan binaire getallen voor een hoeveelheid. Dus is het zinvol om van binair naar decimaal te rekenen en andersom. Om van binair naar decimaal te gaan, gaan we even kijken naar hoe een binair of decimaal getal is opgebouwd. Nemen we als voorbeeld het decimale getal $743$. Op de basisschool heb je geleerd dat je het getal $743$ kan splitsen in $700$, $40$ en $3$. Het hele getal is dan $700+40+3=743$. Hopelijk ben je het met me eens dat dit hetzelfde is als
$7\times 100 + 40 \times 10 +  3 \times 1$

Hier zie je de honderdtallen, tientallen en eentallen goed naar voren komen. We kunnen een honderdtal ook als macht van $10$ schrijven. Dan ziet het er zo uit:
$7\times 10^2 + 40 \times 10^1 +  3 \times 10^0$

Hier zie je duidelijk de machten van $10$ terugkomen, die bij het _tientallige stelsel_ horen. Net zo kun je kijken naar het binaire getal `1101`. Laten we het meteen met machten van $2$ schrijven.

$1101_b = 1\times 2^3 + 1\times 2^2 + 0\times 2^1 + 1 \times 2^0$

Wanneer we dit verder uitwerken, krijgen we:

$1101_b = 1\times 2^3 + 1\times 2^2 + 0\times 2^1 + 1 \times 2^0 = 8 + 4 + 1 = 13_d$

Zoals je misschien al gezien hebt, staat er een $b$-tje of een $d$-tje achter een getal. Dit is om aan te geven of het getal _b_inair of _d_ecimaal is.

**Nog een voorbeeld**

Opgave: Zet het getal $10110_b$ om naar decimale notatie.

$10110 = 1 \times 2^4 + 0 \times 2^3 + 1 \times 2^2 + 1 \times 2^1 + 0 \times 2^0 = 16+0+4+2+0=22$

:::{Tip}
Leer de machten van $2$ van $2^0$ tot en met $2^{10}$ uit je hoofd!
$1, 2, 4, 18, 16, 32, 64, 128, 256, 512, 1024$. 
:::

:::{exercise} Oefenen

Zet de volgende binaire getallen om naar decimale notatie:

1. $1011$
2. $101101$
3. $1100111$
4. $10010110$
5. $1111001$
6. $100001010$
:::

## Van decimaal naar binair
Om een decimaal getal naar een binair getal om te zetten, heb je nodig dat je goed door twee kan delen en kan delen 'met rest'. We gaan als voorbeeld het getal $22_d$ omzetten naar een decimaal getal:
- $22/2 = 11$ rest $0$
- $11/2 = 5$ rest $1$
- $5/2 = 2$ rest $1$
- $2/2 = 1$ rest $0$
- $1/2 = 0$ rest $1$

Wat we gedaan hebben is dat we door $2$ zijn blijven delen, totdat we niks meer overhebben. We hebben wel steeds de resten opgeschreven. Het binaire getal wat we zoeken is af te lezen door de resten van onder naar boven over te nemen. Dus $22_d$ = $10110_b$.

Je kunt nu controleren of dit correct is door $10110_b$ weer om te zetten naar decimaal. Dan krijg je:
$10110_b = 1\times 2^4 + 0\times 2^3 + 1\times 2^2 + 1 \times 2^1 + 0 \times 2^0 = 16+0+4+2+0 = 22_d$

Het klopt!

```{admonition} Extra voorbeeld: Zet $46_d$ om in binaire notatie.
:class: dropdown
- $46/2 = 23$ rest $0$
- $23/2 = 11$ rest $1$
- $11/2 = 5$ rest $1$
- $5/2 = 2$ rest $1$
- $2/2 = 1$ rest $0$
- $1/2 = 0$ rest $1$

Dus $46_d = 101110_b$. 
```

:::{exercise} Oefenen
Zet de volgende decimale getallen om naar binaire notatie:

1. 25
2. 63
3. 99
4. 200
5. 327
:::

## Binaire getallen optellen
Het optellen van binaire getallen gaat eigenlijk hetzelfde als bij decimale getallen. Het lijkt anders omdat je minder cijfers hebt. Zoals je $5$ en $3$ bij elkaar optelt, zo tel je $1$ en $0$ bij elkaar op.

Voor getallen met maar één cijfer is het eenvoudig. 
- $0_b + 0_b = 0_b$
- $0_b + 1_b = 1_b$
- $1_b + 0_b = 1_b$
- $1_b + 1_b = ??$

Bij de laatste combinatie moeten we even stil staan. Los van alle grapjes, die je over $1+1$ kan maken, is $1_d+1_d = 2_d$. Maar het cijfer $2$ komt niet in het binaire stelsel voor. We weten inmiddels ook dat $2_d = 10_b$. Er gebeurt eigenlijk zelfde wanneer we in het decimale stelsel $5_d+7_d$ berekenen. Dat is $2_d$, maar we gaan de 'tiental-grens' over, dus zetten we er een $1$ voor. Deze 'grens' is bij het binaire stelsel natuurlijk best snel bereikt. Dus

$1_b + 1_b = 10_b$

Wanneer we langere binaire getallen bij elkaar op willen tellen, doen we dat op dezelfde manier als bij decimale getallen.
1. Zet de getallen precies onder elkaar, zodat de rechterkanten van de getallen gelijk zijn en elk cijfer precies onder elkaar staat.
2. Begin rechts en tel de cijfers bij elkaar op. Schrijf het resultaat recht onder de twee getallen. Ben je een 'grens' overgegaan? Onthoud dan $1$.
3. Ga verder met het getal wat er links van staat. Heb je in de vorige stap $1$ onthouden? Tel deze dan bij deze optelling op.
4. Ga door totdat je geen cijfers meer bij elkaar op hoeft te tellen.

### Voorbeeld decimale optelling
$498_d + 334_d$

```
         1     11     11 
 498    498    498    498
 334    334    334    334
 --- +  --- +  --- +  --- +
          2     32    832
```

### Voorbeeld binaire optelling
$1010_b + 1011_b$

```
                  1       1       1
 1010    1010    1010    1010    1010
 1011    1011    1011    1011    1011
 ---- +  ---- +  ---- +  ---- +  ---- +
            1      01     101   10101 
```

Het kan natuurlijk voorkomen dat je 3 `1`-en bij elkaar op moet tellen. Zie wat daarmee gebeurt in het volgende voorbeeld:

$1011_b + 1011_b$
```
           1      11      11      11
 1011    1011    1011    1011    1011
 1011    1011    1011    1011    1011
 ---- +  ---- +  ---- +  ---- +  ---- +
            0      10     110   10110 
```

Overigens zie je hier een fijne eigenschap van binaire getallen. Vermenigvuldigen met twee is heel eenvoudig. Je zet simpel een `0` achter het getal en je hebt het verdubbeld. Op zich is dit niet zo verrassend, want wanneer je in het decimale stelsel met $10$ vermenigvuldigd, is dat ook zo simpel als een $0$ achter het getal zetten.

:::{exercise} Oefenen
Tel de volgende binaire getallen bij elkaar op. Gebruik de methode zoals in de voorbeelden hierboven. Om het jezelf een stuk gemakkelijker te maken, gebruik potlood en papier!

1. $101_b + 110_b$
2. $11011_b + 1010_b$
3. $1111_b + 10101_b$
4. $111111_b + 101010_b$
5. $1010101_b + 1100110_b$

Verdubbel de volgende twee binaire getallen:

1. $11011_b$
2. $101010_b$

Uitdaging #1: vermenigvuldig het binaire getal $11010_b$ met $10_d$ en schrijf het resultaat als een binair getal op.

Uitdaging #2: vermenigvuldig het binaire getal $11010_b$ met $10_b$ en schrijf het resultaat als een binair getal op.

```{admonition} Hint voor de uitdaging
:class: dropdown
Vermenigvuldigen kun je ook doen door herhaald optellen. Denk aan $3 \times 4 = 4 + 4 + 4$.
```
:::

## Negatieve getallen

Op zich is het vrij simpel om met negatieve binaire getallen te werken. Je zet er een $-$-tje voor en klaar is Kees. Je hebt een negatief binair getal gemaakt. Alleen is dit de simpele, wiskundige oplossing. Je weet nu nog steeds niet hoe je het verschil tussen twee binaire getallen moet berekenen en hoe zet je dit in hemelsnaam in een computer?

### Binaire getallen in een computer

Belangrijk om te weten is dat een processor voor elke actie, die je op getallen kan uitvoeren een stukje logische schakelingen moet hebben. Dus moeten er door de ontwerpers keuzes gemaakt worden over bijvoorbeeld de maximale grootte van getallen en of je al dan niet een apart deel maakt voor het aftrekken van binaire getallen. Want meer schakelingen op je processor betekent meer warmte productie en zeer waarschijnlijk hogere productiekosten. Dus hou in je achterhoofd dat ontwerpers van processoren het aantal schakelingen op een processor zo laag mogelijk willen houden.

Laten we even kijken naar de (maximale) grootte van binaire getallen in een processor. In de computerarchitectuur noemen we dit de *woordlengte* van een architectuur of processor. De woordlengte van een processor bepaalt de maximale grootte van de binaire getallen en wordt uitgedrukt in het aantal *bits*. Een bit is de kleinste eenheid van data, die een computer kan opslaan. Een bit kan de waarde `1` of `0` en dat komt dan weer overeen met de binaire getallen, waar je nu mee aan het rekenen bent.

```{admonition} Bits en bytes
8 bits bij elkaar noemen we een *byte*.
```

De woordlengte van de processor in je mobiele telefoon of de processor in je computer is waarschijnlijk 64 bits. Dat wil zeggen dat de standaardeenheid waar de processor mee rekent, 64 bits groot is. Het maximale getal wat we in een woord van 64-bits kunnen uitdrukken is $2^64-1 = 
18.446.744.073.709.551.616$. Retro-gaming consoles, zoals de [NES](https://nl.wikipedia.org/wiki/Nintendo_Entertainment_System), [Sega Mega Drive](https://nl.wikipedia.org/wiki/Sega_Mega_Drive) of de [Playstation 1](https://nl.wikipedia.org/wiki/PlayStation_(spelcomputer)) hebben respectievelijk woordlengtes van 8, 16 en 32 bit. Dit zijn allemaal machten van 2. Dus nu zou je kunnen denken dat de alle computers een woordlengte hebben, die een macht van 2 is. Dat is niet helemaal waar. Grote computers uit het verleden (meer dan 60 jaar geleden) hadden woordlengtes als 30, 36 of 33 bits. De eerste Nederlandse computer, de [ARRA I](https://nl.wikipedia.org/wiki/ARRA_I) uit 1954 had een woordlengte van 30 bit.

```{admonition} Woord
Een woord is de standaardeenheid van data waarmee een processor rekent.
```

### Een minnetje ervoor zetten
We gaan voor het gemak van het uitleggen even kijken naar een architectuur met een woordlengte van 4 bits. Kijk even naar de volgende tabel:

| Binair | Alleen positief | Minnetje ervoor |
| :----: | :-------------: | :-------------: |
| `0000` |       $0$       |       $0$       |
| `0001` |       $1$       |       $1$       |
| `0010` |       $2$       |       $2$       |
| `0011` |       $3$       |       $3$       |
| `0100` |       $4$       |       $4$       |
| `0101` |       $5$       |       $5$       |
| `0110` |       $6$       |       $6$       |
| `0111` |       $7$       |       $7$       |
| `1000` |       $8$       |      $-0$       |
| `1001` |       $9$       |      $-1$       |
| `1010` |      $10$       |      $-2$       |
| `1011` |      $11$       |      $-3$       |
| `1100` |      $12$       |      $-4$       |
| `1101` |      $13$       |      $-5$       |
| `1110` |      $14$       |      $-6$       |
| `1111` |      $15$       |      $-7$       |

In de derde kolom 'Minnetje ervoor' zie je dat de eerste bit van de 4 bits gebruiken om een minnetje aan te geven. Een `0` vooraan betekent dat het getal positief is, een `1` vooraan betekent dat het getal negatief is.  Aan deze representatie zitten twee grote nadelen:

1. Er zijn twee nullen. Dus je kunt effectief minder getallen representeren.
2. Niet nadenken bij het binair optellen van deze getallen kan tot grote problemen leiden. Kijk maar.
   $2_d - 4_d = 2_d + -4_d = -2_d$
   wordt binair-met-een-minnetje-ervoor:
   $0010_b + 1100_b = 1110_b = -6_d \neq -2_d$.
   Dit betekent dat we alle processor logica voor het optellen moeten herzien.

Er is een oplossing voor beide problemen. Wanneer negatieve getallen met de *twee-complements notatie* geschreven worden is er slechts één $0$ en hoeven we niet moeilijke oplossingen voor het optellen van getallen te verzinnen. We komen bij deze twee-complements notatie via een kleine omweg. We gaan eerst naar de *één-complements notatie*

### Één-complements notatie

Bij deze notatie gebruiken we nog steeds de eerste bit om een minnetje aan te geven. Maar we draaien de volgorde van de negatieve getallen om. Hoe doen we dit? We *flippen* alle bitten in een getal om het negatief te maken. Kijk nauwkeurig naar de volgende tabel:

| Binair | Alleen positief | Minnetje ervoor | Één-complement |
| :----: | :-------------: | :-------------: |:-:|
| `0000` |       $0$       |       $0$       | $0$|
| `0001` |       $1$       |       $1$       | $1$ |
| `0010` |       $2$       |       $2$       | $2$|
| `0011` |       $3$       |       $3$       |$3$|
| `0100` |       $4$       |       $4$       |$4$|
| `0101` |       $5$       |       $5$       |$5$|
| `0110` |       $6$       |       $6$       |$6$|
| `0111` |       $7$       |       $7$       |$7$|
| `1000` |       $8$       |      $-0$       |$-7$ |
| `1001` |       $9$       |      $-1$       |$-6$|
| `1010` |      $10$       |      $-2$       |$-5$|
| `1011` |      $11$       |      $-3$       |$-4$|
| `1100` |      $12$       |      $-4$       |$-3$|
| `1101` |      $13$       |      $-5$       |$-2$|
| `1110` |      $14$       |      $-6$       |$-1$|
| `1111` |      $15$       |      $-7$       |$-0$|

Laten we even kijken naar het getal $6_d$. Dat is `0110` binair. We kunnen hier een negatief getal van maken in één-complementsnotatie door alle bitten te *flippen*. Elke `0` wordt een `1` en elke `1` wordt een `0`. Dus `0110` wordt `1001`. Volgens de tabel is dit $-6_d$.

Gaat het nu ook goed met optellen? Zonder hier een wiskundig sluitend bewijs voor te geven, kijken we weer even naar het voorbeeld bij de vorige tabel.
$2_d - 4_d = 2_d + -4_d = 0010_b + 1011_b = 1101_b = -2_d$. Gelukkig 😀

Maar we zijn er nog niet helemaal. Door het bestaan van de twee nullen `0` en `-0` kan er nog steeds een probleem ontstaan bij grensgevallen. Één van de mogelijke oplossingen van dit probleem is de zogenaamde twee-complements notatie.

(twee-complement)=
### Twee-complements notatie

Deze notatie is een kleine, maar belangrijke wijziging. Bekijk het stappenplan hieronder:

```{admonition} Stappenplan 
**Negatief maken van een getal in twee-complements notatie**
1. Flip alle bitten, zoals bij de één-complements notatie.
2. Tel er 1 bij op.
```

```{admonition} Voorbeeld 1: *Bepaal het 4-bits twee-complement van $4_d$.*
:class: dropdown
1. Converteer naar binair: $4_d = 0100_b$.
2. Flip alle bitten: `0100` wordt `1011`.
3. Tel er 1 bij op: `1100`.
4. Klaar.
```

```{admonition} Voorbeeld 2: *Bepaal het 8-bits twee-complement van $17_d$.*
:class: dropdown
1. Converteer naar binair: $17_d = 0001 0001_d$
2. Flip alle bitten: `0001 0001` wordt `1110 1110`.
3. Tel er 1 bij op: `1110 1111`.
4. Klaar.
```

**De voordelen van twee-complements notatie**

1. Het optellen van negatieve binaire getallen is hetzelfde als het optellen van positieve binaire getallen. Daardoor heb je minder logica nodig (zie hoofdstuk ***TODO***). Dit levert een snelheidswinst en een kostenbesparing op.
2. Er is maar een enkele $0$. Voor het systeem zijn $0$ en $-0$ niet hetzelfde, daardoor kunnen er fouten ontstaan en onverwachte resultaten komen bij berekening waar deze getallen in voorkomen. Je zou kunnen denken, daar kun je prima omheen programmeren. Dat klopt. Maar dat zorgt ervoor dat je weer meer logica in moet bouwen voor het detecteren van die situaties. Dus wordt de processor nodeloos groter voor iets wat met twee-complements notatie op te lossen is.

**Soorten bytes**

Wanneer we een byte omzetten naar een decimaal getal, is het natuurlijk wel belangrijk om te weten of er een minnetje voor kan staan of niet *en* wat voor soort negatieve-getalsnotatie er gebruikt wordt. Gelukkig wordt meestal de twee-complementsnotatie gebruikt. Maar dan is het nog steeds belangrijk om te weten of die ene bit in een byte bij de hoeveelheid hoort of dat er iets met een minnetje gebeurd. 
> We noemen een getal waar een minnetje voor kan staan **signed**. Een getal zonder een mogelijk minnetje noemen we **unsigned**.


:::{exercise} Oefenen

1. Wat is de maximale grootte van getallen in een 8, 16 of 32 bit machine?

Zet de de volgende signed bytes om in negatieve getallen in twee-complements notatie

2. `01110011`
3. `00110101`
4. `10001000`
5. `10101010`

Werk de volgende decimale sommen binair uit mbv twee-complements notatie:

6. $26_d - 15_d$
7. $-31_d - 6_d$
8. $144_d - 156_d$
:::

## Hexadecimale getallen
Programmeurs zijn over het algemeen mensen, die snel en in één oogopslag willen kunnen zien, wat er voor hun neus staat. Wanneer je de byte `11010111` voor je ziet, is het niet gemakkelijk te zien welk getal dit is. Ze zijn het nu eenmaal niet gewend. Ok, ok, er zijn programmeurs, die dit gemakkelijk kunnen. Maar toch. We kunnen het getal natuurlijk als decimaal getal opschrijven. Dat heb je nu geleerd: $11010111_b = 215_d$ wanneer we dit als *unsigned byte* beschouwen en $-41_d$ wanneer we dit als een *signed byte* beschouwen. Het probleem van de decimale notatie en bytes is dat we soms twee posities en meestal drie posities nodig hebben om een byte als decimaal getal uit te drukken. Hier is gelukkig een mooie oplossing voor. Schrijf een byte in de zogenaamde *hexadecimale* notatie. In normaal Nederlands noemen we dit ook wel *zestientallige* notatie. [^1]. We zijn gewend om met tientallige notatie te werken. Wanneer we bij de 9 zijn, beginnen we weer bij 0 en zetten er een 1 voor. In de zestientallige notatie hebben we nog extra symbolen nodig om tot de zestien te komen. Tellen in hexadecimaal ziet er dan zo uit: $1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, F, 10, 11, 12, 13, \ldots, 1E, 1F, 20, 21, \ldots$.

In hexadecimale notatie kan *elke* byte geschreven worden met twee posities. Één cijfer in hexadecimale notatie is precies 4 bits ook wel een *half-byte*. Bekijk de onderstaande tabel. We gebruiken maar meteen de notatie, die programmeurs ook gebruiken om aan te geven of een getal binair of *hex* is. `0b` staat voor binair, `0x` staat voor hexadecimaal.

| Binair | Hexadecimaal     | Binair | Hexadecimaal |
| :-: | :-: | :-:|:-:|
| `0b0000` | `0x0` | `0b1000` | `0x8`
| `0b0001` | `0x1` | `0b1001` | `0x9`
| `0b0010` | `0x2` | `0b1010` | `0xA`
| `0b0011` | `0x3` | `0b1011` | `0xB`
| `0b0100` | `0x4` | `0b1100` | `0xC`
| `0b0101` | `0x5` | `0b1101` | `0xD`
| `0b0110` | `0x6` | `0b1110` | `0xE`
| `0b0111` | `0x7` | `0b1111` | `0xF`

De eerder genoemde byte `0b11010111` wordt in hex dan `0xD7` of `D7`.

:::{exercise} Oefenen

Zet de volgende bytes om in hexadecimale notatie

1. `01110011`
2. `00110101`
3. `10001000`
4. `10101010`

Zet de volgende hexadecimale getallen om in binaire getallen

5. `0xFA`
6. `0xCA`
7. `0xDE`
8. `0X5A`

:::

## Verdiepende opgaven:

### Met binaire getallen
Schrijf in je favoriete programmeertaal een programma, dat
1. Een string met een binair getal omzet naar een string met een decimaal getal.
2. Een string met een decimaal getal omzet naar een string met een binair getal.
3. Twee strings met binaire getallen bij elkaar optelt.
8. Het 2-complements negatieve binaire getal geeft van een gegeven binair getal(string).
16. Beschrijf een procedure voor het aftrekken van twee binaire getallen (net zoals bij de paragraaf over optelling).

### Met andere dan binaire getallen
Nu je weet hoe je binair kan rekenen, zou je kunnen uitzoeken hoe het achttallige stelsel werkt? Of zelfs het zestientallige stelsel! Voor dit laatste zul je een trukje moeten uithalen met de cijfers. Want 'na' $9$ zijn de cijfers op. Dit los je op door als volgt te tellen: 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, F. Het achttallige stelsel heet ook wel octaal en het zestientallige stelsel hexadecimaal. Hexadecimale getallen zul je misschien ook wel weer herkennen in computers. Het handige aan een hexadecimaal getal van lengte twee, is dat je dan precies een byte (8 bits) aan informatie voor je hebt staan.

1. Zoek uit hoe je van octaal naar decimaal en weer terug kan rekenen.
2. Zoek uit hoe je van hexadecimaal naar decimaal en weer terug kan rekenen.
3. Zoek uit hoe je van hexadecimaal naar binair en weer terug kan rekenen.
8. Misschien ook iets met hexadecimale getallen doet?

[^1]: Ik weet zeker dat je dit heel normaal Nederlands vindt en het woord dagelijks meerder keren gebruikt 😜 
