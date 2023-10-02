Lesmodule Binair Rekenen voor Informatici
-----

Je werkt waarschijnlijk al een tijdje met computers en andere digitale apparatuur. In de kern verwerken computers gegevens door middel van electronische schakelingen. In deze schakelingen kunnen slechts twee toestanden te onderscheiden. _Aan_ of _Uit_. Deze toestanden worden weergegeven als 0 en 1. Deze twee cijfers vormen de basis voor wat we _binair rekenen_ noemen. In deze lesmodule leer je wat binaire getallen zijn, hoe je binaire getallen kunt omrekenen naar decimale getallen en omgekeerd, hoe je binaire getallen bij elkaar kunt optellen en hoe je hoe je negatieve getallen kan representeren.

# Binaire getallen
Je bent al sinds de basisschool gewend om getallen op te schrijven met een combinatie van de cijfers 0 t/m 9. Je hebt hiermee leren optellen, de tafels uit je hoofd geleerd (en misschien nu weer vergeten), je hebt geleerd dat de volgorde van de cijfers een verschil maakt. Ik bedoel er is toch een behoorlijk verschil tussen een 8,3 of een 3,8 als resultaat van een toets. We gebruiken dus 10 verschillende symbolen, die we cijfers noemen, om onze getallen op te schrijven. Met een moeilijk woord noemen we dit het _decimale stelsel_ of _tientallige stelsel_. In dit decimale stelsel rekenen we met decimale getallen. In het _binaire stelsel_ rekenen we met _binaire getallen_. Het enige verschil tussen het decimale talstelsel en het binaire talstelsel is dat we niet met tien cijfers, maar met twee cijfers rekenen. De `0` en de `1`. Voorbeelden van binaire getallen zijn `10100` en `11001`. De binaire getallen zijn dus op dezelfde manier opgebouwd als decimale getallen, maar dan met minder cijfers.

Wanneer je binair gaat tellen ziet dat er zo uit: `0, 1, 10, 11, 100, 101, 110, 111, ...`. Dit is hetzelfde als $0, 1, 2, 3, 4, 5, 6, 7, \ldots$ in de decimale notatie. Je telt eigenlijk net zoals bij het decimale stelsel, alleen zijn de cijfers eerder 'op'.

# Van binair naar decimaal
Net zoals decimale getallen staan binaire getallen voor een hoeveelheid. Dus is het zinvol om van binair naar decimaal te rekenen en andersom. Om van binair naar decimaal te gaan, gaan we even kijken naar hoe een binair of decimaal getal is opgebouwd. Nemen we als voorbeeld het decimale getal $743$. Op de basisschool heb je geleerd dat je het getal $743$ kan splitsen in $700$, $40$ en $3$. Het hele getal is dan $700+40+3=743$. Hopelijk ben je het met me eens dat dit hetzelfde is als
$$7\times 100 + 40 \times 10 +  3 \times 1$$

Hier zie je de honderdtallen, tientallen en eentallen goed naar voren komen. We kunnen een honderdtal ook als macht van $10$ schrijven. Dan ziet het er zo uit:
$$7\times 10^2 + 40 \times 10^1 +  3 \times 10^0$$

Hier zie je duidelijk de machten van $10$ terugkomen, die bij het _tientallige stelsel_ horen. Net zo kun je kijken naar het binaire getal `1101`. Laten we het meteen met machten van $2$ schrijven.

$$1101_b = 1\times 2^3 + 1\times 2^2 + 0\times 2^1 + 1 \times 2^0$$

Wanneer we dit verder uitwerken, krijgen we:

$$1101_b = 1\times 2^3 + 1\times 2^2 + 0\times 2^1 + 1 \times 2^0 = 8 + 4 + 1 = 13_d$$

Zoals je misschien al gezien hebt, staat er een $b$-tje of een $d$-tje achter een getal. Dit is om aan te geven of het getal _b_inair of _d_ecimaal is.

**Nog een voorbeeld**

Opgave: Zet het getal $10110_b$ om naar decimale notatie.

$$10110 = 1 \times 2^4 + 0 \times 2^3 + 1 \times 2^2 + 1 \times 2^1 + 0 \times 2^0 = 16+0+4+2+0=22$$

## Oefenen

Zet de volgende binaire getallen om naar decimale notatie:

1. $1011$
2. $101101$
3. $1100111$
4. $10010110$
5. $1111001$
6. $100001010$

# Van decimaal naar binair
Om een decimaal getal naar een binair getal om te zetten, heb je nodig dat je goed door twee kan delen en kan delen 'met rest'. We gaan als voorbeeld het getal $21_d$ omzetten naar een decimaal getal:
- $21/2 = 10$ rest $1$
- $10/2 = 5$ rest $0$
- $5/2 = 2$ rest $1$
- $2/2 = 1$ rest $0$
- $1/2 = 0$ rest $1$

Wat we gedaan hebben is dat we door $2$ zijn blijven delen, totdat we niks meer overhebben. We hebben wel steeds de resten opgeschreven. Het binaire getal wat we zoeken is af te lezen uit de resten. Dus $21_d$ = $10101_b$.

Je kunt nu controleren of dit correct is door $10101_b$ weer om te zetten naar decimaal. Dan krijg je:
$$10101_b = 1\times 2^4 + 0\times 2^3 + 1\times 2^2 + 0 \times 2^1 + 1 \times 2^0 = 16+0+4+0+1 = 21_d$$

Het klopt!

Nog een voorbeeld: Zet $46_d$ om in binaire notatie.
- $46/2 = 23$ rest $0$
- $23/2 = 11$ rest $1$
- $11/2 = 5$ rest $1$
- $5/2 = 2$ rest $1$
- $2/2 = 1$ rest $0$
- $1/2 = 0$ rest $1$

Dus $46_d = 011101_b$. Net zoals bij de decimale notatie, laten we nullen, die 'vooraan' staan, weg. Dus $011101_b$ schrijven we dan als $11101_b$.

## Oefenen
Zet de volgende decimale getallen om naar binaire notatie:

1. 25
2. 63
3. 99
4. 200
5. 327

# Binaire getallen optellen
Het optellen van binaire getallen gaat eigenlijk hetzelfde als bij decimale getallen. Het lijkt anders omdat je minder cijfers hebt. Zoals je $5$ en $3$ bij elkaar optelt, zo tel je $1$ en $0$ bij elkaar op.

Voor getallen met maar één cijfer is het eenvoudig. 
- $0_b + 0_b = 0_b$
- $0_b + 1_b = 1_b$
- $1_b + 0_b = 1_b$
- $1_b + 1_b = ??$

Bij de laatste combinatie moeten we even stil staan. Los van alle grapjes, die je over $1+1$ kan maken, is $1_d+1_d = 2_d$. Maar het cijfer $2$ komt niet in het binaire stelsel voor. We weten inmiddels ook dat $2_d = 10_b$. Er gebeurt eigenlijk zelfde wanneer we in het decimale stelsel $5_d+7_d$ berekenen. Dat is $2_d$, maar we gaan de 'tiental-grens' over, dus zetten we er een $1$ voor. Deze 'grens' is bij het binaire stelsel natuurlijk best snel bereikt. Dus

$$1_b + 1_b = 10_b$$

Wanneer we langere binaire getallen bij elkaar op willen tellen, doen we dat op dezelfde manier als bij decimale getallen.
1. Zet de getallen precies onder elkaar, zodat de rechterkanten van de getallen gelijk zijn en elk cijfer precies onder elkaar staat.
2. Begin rechts en tel de cijfers bij elkaar op. Schrijf het resultaat recht onder de twee getallen. Ben je een 'grens' overgegaan? Onthoud dan $1$.
3. Ga verder met het getal wat er links van staat. Heb je in de vorige stap $1$ onthouden? Tel deze dan bij deze optelling op.
4. Ga door totdat je geen cijfers meer bij elkaar op hoeft te tellen.

## Voorbeeld decimale optelling
$498_d + 334_d$

```
         1     11     11 
 498    498    498    498
 334    334    334    334
 --- +  --- +  --- +  --- +
          2     32    832
```

## Voorbeeld binaire optelling
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

## Oefenen
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

# Negatieve getallen
Op zich is het vrij simpel om met negatieve binaire getallen te werken. Je zet er een $-$-tje voor en klaar is Kees. Je hebt een negatief binair getal gemaakt. Alleen is dit de simpele, wiskundige oplossing. Je weet nu nog steeds niet hoe je het verschil tussen twee binaire getallen moet berekenen en hoe zet je dit in hemelsnaam in een computer?

## Binaire getallen in een computer
Belangrijk om te weten is dat een processor voor elke actie, die je op getallen kan uitvoeren een stukje logische schakelingen moet hebben. Dus moeten er door de ontwerpers keuzes gemaakt worden over bijvoorbeeld de maximale grootte van getallen en of je al dan niet een apart deel maakt voor het aftrekken van binaire getallen. Want meer schakelingen op je processor betekent meer warmte productie en zeer waarschijnlijk hogere productiekosten. Dus hou in je achterhoofd dat ontwerpers van processoren het aantal schakelingen op een processor zo laag mogelijk willen houden.

Laten we even kijken naar de (maximale) grootte van binaire getallen in een processor. In de computerarchitectuur noemen we dit de *woordlengte* van een architectuur of processor. De woordlengte van een processor bepaalt de maximale grootte van de binaire getallen en wordt uitgedrukt in het aantal *bits*. Een bit is de kleinste eenheid van data, die een computer kan opslaan. Een bit kan de waarde `1` of `0` en dat komt dan weer overeen met de binaire getallen, waar je nu mee aan het rekenen bent.

De woordlengte van de processor in je mobiele telefoon of de processor in je computer is waarschijnlijk 64 bits. Dat wil zeggen dat de standaardeenheid waar de processor mee rekent, 64 bits groot is. Het maximale getal wat we in een woord van 64-bits kunnen uitdrukken is $2^64-1 = 
18.446.744.073.709.551.616$. Retro-gaming consoles, zoals de NES, Sega Mega Drive of de Playstation 1 hebben respectievelijk woordlengtes van 8, 16 en 32 bit.

## Een minnetje ervoor zetten
We gaan voor het gemak van het uitleggen even kijken naar een architectuur met een woordlengte van 4 bits.

## Oefenen
1. Wat is de maximale grootte van getallen in een 8, 16 of 32 bit machine?


ZEBRA 33 bit woordlengte

# Verdiepende opgaven:

## Met binaire getallen
Schrijf in je favoriete programmeertaal een programma, dat
1. Een string met een binair getal omzet naar een string met een decimaal getal.
2. Een string met een decimaal getal omzet naar een string met een binair getal.
3. Twee strings met binaire getallen bij elkaar optelt.
4. Het 2-complements negatieve binaire getal geeft van een gegeven binair getal(string).
5. Beschrijf een procedure voor het aftrekken van twee binaire getallen (net zoals bij de paragraaf over optelling).

## Met andere getallen
Nu je weet hoe je binair kan rekenen, zou je kunnen uitzoeken hoe het achttallige stelsel werkt? Of zelfs het zestientallige stelsel! Voor dit laatste zul je een trukje moeten uithalen met de cijfers. Want 'na' $9$ zijn de cijfers op. Dit los je op door als volgt te tellen: 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, F. Het achttallige stelsel heet ook wel octaal en het zestientallige stelsel hexadecimaal. Hexadecimale getallen zul je misschien ook wel weer herkennen in computers. Het handige aan een hexadecimaal getal van lengte twee, is dat je dan precies een byte (8 bits) aan informatie voor je hebt staan.

1. Zoek uit hoe je van octaal naar decimaal en weer terug kan rekenen.
2. Zoek uit hoe je van hexadecimaal naar decimaal en weer terug kan rekenen.
3. Zoek uit hoe je van hexadecimaal naar binair en weer terug kan rekenen.
8. misschien ook iets met hexadecimale getallen doet?
