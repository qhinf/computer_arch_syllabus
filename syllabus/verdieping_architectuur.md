(hoofdstuk_meer_architectuur)=
# Meer computerarchitectuur

In het hoofdstuk {doc}`architectuur` heb je de basis van de von Neumann architectuur geleerd. Je hebt gezien hoe een processor werkt, hoe geheugen is georganiseerd en wat het verschil is tussen CISC en RISC. In dit verdiepingshoofdstuk gaan we kijken naar geavanceerde architectuurconcepten die in moderne computers en gespecialiseerde hardware worden gebruikt.

## SIMD en MIMD: parallelle verwerking

Computers worden steeds sneller, niet alleen door hogere kloksnelheden, maar ook door *parallellisme*: meerdere berekeningen tegelijk uitvoeren. Er zijn verschillende manieren om dit te doen. De computerarchitect Michael Flynn bedacht in 1966 een classificatie die nog steeds gebruikt wordt: [Flynn's taxonomie](https://nl.wikipedia.org/wiki/Taxonomie_van_Flynn).

### Flynn's taxonomie

Flynn onderscheidt vier categorieën op basis van twee vragen:
1. Hoeveel instructiestromen (instruction streams) zijn er?
2. Hoeveel datastromen (data streams) zijn er?

Dit geeft vier combinaties:

| Type | Instructies | Data | Beschrijving |
|:----:|:-----------:|:----:|:-------------|
| **SISD** | Single | Single | Één instructie op één stuk data tegelijk |
| **SIMD** | Single | Multiple | Dezelfde instructie op meerdere data tegelijk |
| **MISD** | Multiple | Single | Meerdere instructies op hetzelfde stuk data |
| **MIMD** | Multiple | Multiple | Meerdere instructies op meerdere data tegelijk |

**SISD** (Single Instruction, Single Data) is de basis {ref}`von Neumann architectuur <von-neumann-architectuur>` die je al kent. Eén processor voert één instructie uit op één stuk data.

**MISD** (Multiple Instruction, Single Data) komt in de praktijk bijna niet voor. Het theoretische voorbeeld is een systeem waarbij meerdere processors verschillende berekeningen doen op hetzelfde getal.

We gaan ons focussen op **SIMD** en **MIMD**, die wel veel worden gebruikt.

### SIMD: Single Instruction, Multiple Data

SIMD betekent dat je *dezelfde* instructie tegelijk uitvoert op *verschillende* stukjes data. Dit is ideaal voor taken waar je dezelfde bewerking moet doen op een grote hoeveelheid data.

**Voorbeeld zonder SIMD:**

Stel je wilt 4 getallen met 2 vermenigvuldigen:
```
A = [10, 20, 30, 40]
```

Zonder SIMD doe je dit stap voor stap:
```
Cyclus 1: 10 × 2 = 20
Cyclus 2: 20 × 2 = 40
Cyclus 3: 30 × 2 = 60
Cyclus 4: 40 × 2 = 80
```
Totaal: 4 cycli

**Met SIMD:**

Met SIMD kun je alle vier tegelijk doen:
```
Cyclus 1: [10, 20, 30, 40] × 2 = [20, 40, 60, 80]
```
Totaal: 1 cyclus!

Dit is 4× sneller!

```{mermaid}
graph TD
    A[Instructie: Vermenigvuldig met 2] --> B[Data 1: 10]
    A --> C[Data 2: 20]
    A --> D[Data 3: 30]
    A --> E[Data 4: 40]
    B --> F[Resultaat: 20]
    C --> G[Resultaat: 40]
    D --> H[Resultaat: 60]
    E --> I[Resultaat: 80]
```

**Waar wordt SIMD gebruikt?**

SIMD is perfect voor:
- **Afbeeldingsverwerking**: dezelfde filter toepassen op alle pixels
- **Videobewerking**: kleurcorrectie op alle frames
- **Geluidbewerking**: volume aanpassen van alle samples
- **3D graphics**: transformaties op alle vertices
- **Wetenschappelijke berekeningen**: matrixoperaties
- **Machine learning**: operaties op grote tensoren

**SIMD in processors**

Moderne processoren hebben speciale SIMD-instructies:
- **Intel/AMD**: SSE, AVX, AVX-512
- **ARM**: NEON
- **RISC-V**: Vector Extension

Deze instructies werken met speciale brede registers die bijvoorbeeld 128, 256 of 512 bits groot zijn. In een 256-bit register passen bijvoorbeeld 8 getallen van 32-bit, die je dan allemaal tegelijk kunt bewerken.

**Voorbeeld: AVX-instructie**

Een normale instructie telt 2 getallen op:
```asm
ADD R1, R2    ; R1 = R1 + R2
```

Een AVX-instructie telt 8 paren getallen tegelijk op:
```asm
VADDPS YMM1, YMM2, YMM3    ; 8 floating points tegelijk optellen
```

**Beperkingen van SIMD**

SIMD werkt alleen goed als:
1. Je dezelfde operatie doet op veel data
2. De data mooi achter elkaar in het geheugen staat
3. Er geen afhankelijkheden tussen de data zijn

Voor taken met veel `if`-statements of onvoorspelbare patronen werkt SIMD minder goed.

:::{exercise}

1. Leg uit waarom het toepassen van een filter op een foto (zoals zwart-wit maken) ideaal is voor SIMD.

2. Stel je hebt een array van 1000 getallen en je wilt ze allemaal met 3 vermenigvuldigen. Hoeveel cycli heb je nodig:
   - Zonder SIMD?
   - Met SIMD waarbij je 8 getallen tegelijk kunt verwerken?

3. Waarom is SIMD minder geschikt voor het sorteren van een lijst met getallen?

4. Zoek op welke SIMD-instructieset de processor van jouw computer of telefoon ondersteunt (SSE, AVX, AVX2, AVX-512, NEON, etc.). Zoek van die SIMD-instructieset een paar voorbeelden op. 

:::

### MIMD: Multiple Instruction, Multiple Data

MIMD betekent dat je *verschillende* instructies tegelijk uitvoert op *verschillende* data. Dit is wat er gebeurt bij multi-core processors en in gedistribueerde systemen.

**Multi-core processors**

Je laptop of PC heeft waarschijnlijk meerdere cores (4, 8, of zelfs meer). Elke core kan een ander programma of een ander deel van hetzelfde programma uitvoeren.

```{mermaid}
graph LR
    subgraph Core1
        I1[Instructie A] --> D1[Data X]
    end
    subgraph Core2
        I2[Instructie B] --> D2[Data Y]
    end
    subgraph Core3
        I3[Instructie C] --> D3[Data Z]
    end
    subgraph Core4
        I4[Instructie D] --> D4[Data W]
    end
```

**Voorbeeld:**

- Core 1: Compileert code
- Core 2: Speelt muziek af
- Core 3: Rendert een video
- Core 4: Download een bestand

Allemaal tegelijk, allemaal verschillende taken!

**Shared Memory vs Distributed Memory**

Bij MIMD zijn er twee hoofdmodellen:

**Shared Memory (Gedeeld geheugen)**
- Alle cores hebben toegang tot hetzelfde werkgeheugen
- Voorbeeld: een multi-core processor in je laptop
- Voordeel: gemakkelijk data delen tussen cores
- Nadeel: geheugen kan een bottleneck worden

**Distributed Memory (Gedistribueerd geheugen)**
- Elke processor heeft zijn eigen geheugen
- Processors communiceren via netwerk
- Voorbeeld: een cluster van computers (supercomputer)
- Voordeel: schaalbaar naar duizenden processors
- Nadeel: communicatie tussen processors is traag

**Parallellisme in software**

Om gebruik te maken van MIMD moet software speciaal geschreven worden voor *multi-threading* of *parallel processing*. Een programma moet opgedeeld worden in taken die onafhankelijk uitgevoerd kunnen worden.

Voorbeeld in Python:
```python
# Zonder parallellisme
for image in images:
    process_image(image)  # 1 voor 1

# Met parallellisme (MIMD)
from multiprocessing import Pool
with Pool(4) as p:  # 4 cores
    p.map(process_image, images)  # 4 tegelijk!
```

**Amdahl's wet**

[Amdahl's wet](https://nl.wikipedia.org/wiki/Wet_van_Amdahl) is een belangrijke wet in de computerwetenschap die de theoretische snelheidswinst van parallellisme beschrijft. De wet is geformuleerd door Gene Amdahl in 1967.

**Het kernprincipe:**

De snelheidswinst van parallellisme wordt beperkt door het deel van het programma dat *niet* geparallelliseerd kan worden. Dit is cruciaal om te begrijpen voordat je investeert in meer cores of parallel programmeren.

Als 90% van je programma parallel kan en je hebt oneindig veel cores, dan is de maximale snelheidswinst 10× (niet oneindig!). Dit komt omdat je nog steeds die 10% sequentieel moet uitvoeren.

**De formule:**

$$S(n) = \frac{1}{(1-p) + \frac{p}{n}}$$

Waarbij:
- $S(n)$ = snelheidswinst met $n$ cores
- $p$ = fractie van het programma dat parallel kan
- $n$ = aantal cores

**Voorbeeld:**
- 75% van je programma is te paralleliseren ($p = 0.75$)
- Je hebt 4 cores ($n = 4$)

$$S(4) = \frac{1}{0.25 + \frac{0.75}{4}} = \frac{1}{0.4375} = 2.29$$

Je programma wordt 2.29× sneller, niet 4×!

**Praktische implicatie:** Wil je meer snelheidswinst? Dan moet je eerst proberen het sequentiële deel (25%) te verkleinen, voordat je meer cores toevoegt!

:::{exercise}

1. Leg uit wat het verschil is tussen SIMD en MIMD.

2. Geef twee voorbeelden van taken die beter geschikt zijn voor:
   - SIMD
   - MIMD

3. Bereken de theoretische snelheidswinst met 8 cores voor een programma waarbij:
   - 50% parallel kan
   - 90% parallel kan
   - 99% parallel kan

4. Waarom wordt een programma met 4 cores meestal niet exact 4× sneller?

5. **Uitdaging:** Zoek uit wat *hyperthreading* (of *SMT* - Simultaneous Multithreading) is en leg uit hoe dit verschilt van echte meerdere cores.

:::

## GPU: Graphics Processing Unit

Een GPU (Graphics Processing Unit) is een gespecialiseerde processor die oorspronkelijk ontworpen was voor het renderen van graphics, maar tegenwoordig ook gebruikt wordt voor veel andere taken.

### Waarom een aparte processor voor graphics?

Het renderen van 3D graphics vereist enorm veel parallelle berekeningen. Voor een 4K-scherm (3840×2160 pixels) moet je bij 60 frames per seconde ongeveer 500 miljoen pixels per seconde berekenen! Voor elke pixel moet je:
- 3D coördinaten transformeren
- Lichtberekeningen doen
- Texturen toepassen
- Schaduwberekeningen uitvoeren

Al deze berekeningen zijn *onafhankelijk* voor elke pixel en volgen hetzelfde patroon. Perfect voor massief parallellisme!

### CPU vs GPU architectuur

Je kunt een GPU eigenlijk zien als een extreme vorm van SIMD: in plaats van 4 of 8 data-elementen tegelijk te verwerken, verwerkt een GPU duizenden pixels of vertices tegelijk met dezelfde instructie. Maar de architectuur is wel fundamenteel anders dan een CPU.

**CPU (Central Processing Unit):**
- Weinig cores (4-16 in consumentenprocessors)
- Elke core is krachtig en complex
- Goede branch prediction en out-of-order execution
- Groot cache geheugen
- Ontworpen voor *latency* (snelle individuele taken)

**GPU (Graphics Processing Unit):**
- Duizenden simpele cores (bijv. NVIDIA RTX 4090: >16.000 cores!)
- Elke core is eenvoudig
- Weinig cache per core
- Ontworpen voor *throughput* (veel taken tegelijk)

```{mermaid}
graph TD
    subgraph CPU [CPU: 4 krachtige cores]
        C1[Core 1<br/>Complex]
        C2[Core 2<br/>Complex]
        C3[Core 3<br/>Complex]
        C4[Core 4<br/>Complex]
    end
    
    subgraph GPU [GPU: Duizenden eenvoudige cores]
        G1[Core]
        G2[Core]
        G3[Core]
        G4[Core]
        G5[Core]
        G6[...]
        G7[...]
        G8[...]
        G1021[Core]
        G1022[Core]
        G1023[Core]
        G1024[Core]
    end
```

**Analogie:**

Een CPU is als een klein team van experts die complexe problemen kunnen oplossen.
Een GPU is als een enorm leger van werkers die elk een simpele taak doen, maar dat wel *heel* snel en *heel* veel tegelijk.

### GPU architectuur

Een moderne GPU bestaat uit:

**Streaming Multiprocessors (SM)**
- Een GPU bevat tientallen SM's
- Elke SM bevat tientallen tot honderdden cores
- Elke SM heeft eigen shared memory en registers

**Cores**
- Veel eenvoudiger dan CPU cores
- Gespecialiseerd in floating point operaties
- Werken in groepen (warps bij NVIDIA, wavefronts bij AMD)

**Geheugen hiërarchie**
- Registers (zeer snel, per thread)
- Shared memory (snel, per SM)
- L1/L2 cache
- VRAM (video RAM, groot maar trager)

### SIMT: Single Instruction, Multiple Threads

GPU's gebruiken een model genaamd SIMT (Single Instruction, Multiple Threads). Dit lijkt op SIMD, maar met threads in plaats van alleen data.

Bij SIMT:
- Groepen van threads (bijvoorbeeld 32 threads = 1 warp) voeren dezelfde instructie uit
- Elke thread heeft eigen registers en kan eigen data hebben
- Threads kunnen divergeren (verschillende paden nemen), maar dat is inefficiënt

**Voorbeeld: Beeldbewerking**

Je wilt een afbeelding blurren. Met SIMT:
```
Thread 0: Blur pixel (0,0)
Thread 1: Blur pixel (0,1)
Thread 2: Blur pixel (0,2)
...
Thread 1000: Blur pixel (10,10)
```

Alle threads voeren de *blur* functie uit, maar elk op een andere pixel. Duizenden pixels tegelijk!

### GPGPU: General Purpose GPU

Sinds circa 2006 kunnen GPU's ook gebruikt worden voor niet-graphics taken. Dit heet GPGPU (General Purpose computing on GPU).

**CUDA en OpenCL**

Om GPU's te programmeren gebruiken we frameworks zoals:
- **CUDA** (NVIDIA, meest populair)
- **OpenCL** (open standaard, werkt op verschillende GPU's)
- **Metal** (Apple)
- **DirectCompute** (Microsoft)

**Voorbeeldcode (simpel CUDA concept):**
```cuda
// GPU kernel - wordt uitgevoerd door duizenden threads
__global__ void vectorAdd(float* A, float* B, float* C, int N) {
    int i = blockIdx.x * blockDim.x + threadIdx.x;
    if (i < N) {
        C[i] = A[i] + B[i];  // Elke thread doet 1 optelling
    }
}

// Roep aan met 1000 threads
vectorAdd<<<10, 100>>>(A, B, C, 1000);
// 10 blocks × 100 threads = 1000 threads
```

### Toepassingen van GPU computing

**Machine Learning / AI**
- Training van neurale netwerken (miljoenen matrixvermenigvuldigingen)
- Deep learning frameworks (TensorFlow, PyTorch) gebruiken GPU's
- Een taak die 1 week duurt op CPU kan in uren op GPU

**Wetenschappelijke berekeningen**
- Weermodellen en klimaatsimulaties
- Moleculaire dynamica
- Astronomische simulaties

**Cryptocurrency mining**
- Hash-berekeningen (miljoenen per seconde)
- Bitcoin, Ethereum (voorheen), etc.

**Video rendering**
- Film special effects (Pixar, Industrial Light & Magic)
- Real-time ray tracing in games

**Dataverwerking**
- Database queries (GPU-databases)
- Data analytics op grote datasets

:::{exercise}

1. Leg uit waarom een GPU met 1000 eenvoudige cores vaak sneller is voor beeldbewerking dan een CPU met 8 krachtige cores.

2. Geef een voorbeeld van een taak die *niet* geschikt is voor een GPU en leg uit waarom niet.

3. Zoek op hoeveel cores de GPU in jouw computer (of game console) heeft. Vergelijk dit met het aantal cores van de CPU.

4. Waarom is het *divergeren* van threads in een GPU inefficiënt? (Tip: denk aan SIMT)

5. **Programmeer uitdaging:** Als je Python kent, probeer de bibliotheek `numba` of `cupy` te gebruiken om een eenvoudige berekening op een GPU uit te voeren (bijv. twee arrays optellen).

:::

## Tensor Processing Unit (TPU): AI-gespecialiseerde hardware

Terwijl GPU's al enorm snel zijn voor machine learning, zijn er nog gespecialiseerdere chips ontwikkeld speciaal voor kunstmatige intelligentie: Tensor Processing Units (TPU's).

### Wat is een TPU?

Een TPU is een chip die door Google is ontworpen specifiek voor machine learning workloads, met name voor *tensor operaties*. Een tensor is een multi-dimensionale array van getallen - de fundamentele datastructuur in machine learning.

Voorbeelden van tensors:
- Een getal: 0D tensor (scalar)
- Een lijst: 1D tensor (vector) `[1, 2, 3]`
- Een matrix: 2D tensor `[[1,2], [3,4]]`
- Een afbeelding: 3D tensor (hoogte × breedte × kleurkanalen)
- Een batch afbeeldingen: 4D tensor

### Waarom nog meer specialisatie?

**CPU → GPU → TPU: steeds gespecialiseerder**

| Aspect | CPU | GPU | TPU |
|:-------|:----|:----|:----|
| **Doel** | Algemeen | Graphics & parallel | ML/AI |
| **Cores** | 4-16 | 1.000-10.000+ | Tensor cores |
| **Precisie** | 64-bit float | 32-bit float | 16-bit, 8-bit, zelfs 4-bit |
| **Operatie** | Algemeen | SIMT | Matrix multiply |
| **Energie** | Medium | Hoog | Efficiënt voor ML |

**De kracht van lagere precisie**

Machine learning heeft vaak geen 64-bit precisie nodig. Door te werken met 16-bit of 8-bit getallen:
- Kun je 4-8× meer berekeningen doen
- Verbruik je 4-8× minder geheugen
- Verbruik je minder energie
- Wordt het sneller

TPU's zijn geoptimaliseerd voor deze lagere precisie operaties.

### Systolic array: het hart van een TPU

Het belangrijkste onderdeel van een TPU is de *systolic array* - een 2D grid van simpele rekeneenheden die data doorpassen aan elkaar, zoals hartslag (systole) bloed rondpompt.

**Hoe werkt een systolic array?**

Stel je voor: een 4×4 grid van vermenigvuldigers voor matrixvermenigvuldiging.

```
Matrix A (input van links):    Matrix B (input van boven):
[a1 a2 a3 a4]                  [b1]
                               [b2]
                               [b3]
                               [b4]
```

In de systolic array:
```
[×] → [×] → [×] → [×]
 ↓     ↓     ↓     ↓
[×] → [×] → [×] → [×]
 ↓     ↓     ↓     ↓
[×] → [×] → [×] → [×]
 ↓     ↓     ↓     ↓
[×] → [×] → [×] → [×]
 ↓     ↓     ↓     ↓
Results
```

Data stroomt van links en van boven naar binnen, wordt vermenigvuldigd en opgeteld, en stroomt door naar rechts en naar beneden. In een paar cycli is de hele matrixvermenigvuldiging klaar!

**Waarom is dit zo efficiënt?**

1. **Data reuse**: Elk getal wordt meerdere keren gebruikt zonder opnieuw uit geheugen te halen
2. **Pipelining**: Terwijl de ene berekening bezig is, begint de volgende al
3. **Geen geheugen bottleneck**: Data blijft in de array, minimale geheugen toegang
4. **Energie-efficiënt**: Simpele operaties, veel hergebruik

### TPU generaties

Google heeft meerdere generaties TPU's ontwikkeld:

**TPU v1 (2015)**
- Alleen *inference* (voorspellingen maken)
- 8-bit integer operaties
- 92 TeraOps/seconde

**TPU v2 & v3 (2017-2018)**
- Ook *training* (modellen trainen)
- Floating point support
- Tot 420 TeraFlops

**TPU v4 (2021)**
- 2× sneller dan v3
- Gebruikt in Google's datacenters

**TPU v5 (2023)**
- Nog krachtiger
- Speciaal voor grote taalmodellen (GPT-achtig)

### Andere AI accelerators

TPU's zijn niet de enige gespecialiseerde AI chips:

**NVIDIA Tensor Cores**
- Ingebouwd in moderne NVIDIA GPU's (vanaf Volta architectuur)
- Geoptimaliseerd voor matrix operaties
- Gebruikt in A100, H100 datacenter GPU's

**Google Edge TPU**
- Kleine, energiezuinige versie voor smartphones en IoT
- Gebruikt in Google Pixel telefoons

**Apple Neural Engine**
- Deel van Apple's M-serie en A-serie chips
- Tot 15.8 trillion operaties per seconde (M1)

**Intel Nervana / Habana**
- Intel's AI accelerators
- Competitie voor NVIDIA en Google

**AMD MI-serie**
- AMD's datacenter GPU's voor AI
- Alternatief voor NVIDIA

### Waar worden TPU's gebruikt?

**Google's eigen diensten:**
- Google Search (ranking, vertaling)
- Google Photos (beeldherkenning)
- Google Translate
- Gmail (spam detectie, smart compose)
- YouTube (aanbevelingen, ondertiteling)

**Google Cloud:**
- Beschikbaar voor klanten via Google Cloud Platform
- Training en inference van ML modellen
- Vaak gebruikt voor:
  - Natuurlijke taalverwerking (NLP)
  - Computer vision
  - Aanbevelingssystemen

**Vergelijking voor een typische ML-taak:**

Training een groot taalmodel:
- CPU: 1 maand
- GPU: 1 week
- TPU: 1-2 dagen

### De toekomst: specialisatie voor AI

We zien een trend naar steeds meer gespecialiseerde hardware:

**Domain-Specific Architectures (DSA)**
- Chips voor specifieke taken
- Nog efficiënter dan general-purpose

**In-memory computing**
- Berekeningen doen waar data opgeslagen is. Nu wordt de data nog van het geheugen naar de verwerkingseenheid getransporteerd en weer terug.
- Elimineert geheugen bottleneck

**Neuromorphic chips**
- Geïnspireerd door de menselijke hersenen
- Spiking neural networks
- Extreem energie-efficiënt (Intel Loihi, IBM TrueNorth)

**Quantum computing voor ML**
- Nog in onderzoeksfase
- Potentieel voor bepaalde ML-problemen

:::{exercise}

1. Leg uit waarom een TPU efficiënter is voor machine learning dan een GPU, ondanks dat een GPU meer algemene toepassingen heeft.

2. Waarom is lagere precisie (8-bit in plaats van 64-bit) vaak voldoende voor machine learning?

3. Zoek uit welke AI accelerator in jouw smartphone zit (als je er een hebt). Wat kan deze chip?

4. Leg uit wat het voordeel is van een systolic array voor matrixvermenigvuldiging.

5. **Onderzoeksproject:** Zoek informatie over "quantization" in machine learning en leg uit hoe dit samenhangt met de lage-precisie operaties van TPU's.

6. **Debat:** Sommigen beweren dat gespecialiseerde chips zoals TPU's de toekomst zijn, anderen denken dat general-purpose processors (met steeds betere software) voldoende zijn. Wat denk jij en waarom?

:::

## Harvard Architectuur en DSP

Tot nu toe hebben we vooral gesproken over de von Neumann architectuur, waarbij instructies en data in hetzelfde geheugen zitten. Maar er is een alternatief: de Harvard architectuur.

### Von Neumann vs Harvard

**Von Neumann architectuur (wat je al kent):**
- Eén geheugen voor instructies en data
- Eén bus tussen processor en geheugen
- Simpeler ontwerp
- Gebruikt in de meeste computers

```{mermaid}
graph LR
    CPU[CPU] <-->|Bus| MEM[Geheugen<br/>Instructies + Data]
```

**Harvard architectuur:**
- Gescheiden geheugen voor instructies en data
- Twee aparte bussen
- Parallelle toegang mogelijk
- Gebruikt in embedded systems en DSP's

```{mermaid}
graph LR
    CPU[CPU] <-->|Instructie bus| IMEM[Instructie<br/>Geheugen]
    CPU <-->|Data bus| DMEM[Data<br/>Geheugen]
```

### Voordelen van Harvard architectuur

**1. Parallelle toegang**

Bij von Neumann kan de processor niet tegelijk een instructie en data ophalen (ze delen dezelfde bus). Bij Harvard kan dit wel!

Von Neumann:
```
Cyclus 1: Haal instructie op
Cyclus 2: Haal data op
Cyclus 3: Voer uit
Cyclus 4: Haal instructie op
...
```

Harvard:
```
Cyclus 1: Haal instructie op + haal data op (tegelijk!)
Cyclus 2: Voer uit + haal volgende instructie op (tegelijk!)
...
```

**2. Geen von Neumann bottleneck**

De bus tussen CPU en geheugen kan een bottleneck zijn. Met twee aparte bussen heb je dit probleem niet (of minder).

**3. Verschillende geheugentypen**

Je kunt verschillende soorten geheugen gebruiken:
- Snel ROM voor instructies (niet-wijzigbaar)
- Snel RAM voor data (wijzigbaar)

**4. Veiligheid**

Je kunt niet per ongeluk code overschrijven met data (of andersom). Dit voorkomt bepaalde types aanvallen.

### Nadelen van Harvard architectuur

**1. Complexer**

Je hebt twee bussen nodig, twee geheugensystemen, meer pinnen op de chip.

**2. Minder flexibel**

Je moet van tevoren weten hoeveel ruimte je nodig hebt voor instructies en voor data. Je kunt niet zomaar geheugen "verplaatsen" van het ene naar het andere.

**3. Code kan niet zichzelf wijzigen**

In sommige toepassingen (zoals just-in-time compilers) wil je code tijdens runtime genereren. Dit is moeilijker bij Harvard.

### Modified Harvard architectuur

Moderne processoren gebruiken vaak een *modified Harvard* architectuur:
- Intern (in de processor) gescheiden caches voor instructies en data
- Extern (buiten de processor) gedeeld geheugen

Dit combineert de voordelen van beide:
- Snelheid van Harvard (parallelle cache toegang)
- Flexibiliteit van von Neumann (gedeeld hoofdgeheugen)

### DSP: Digital Signal Processor

Een belangrijke toepassing van Harvard architectuur is de **DSP** (Digital Signal Processor). Dit is een gespecialiseerde processor voor signaalverwerking.

**Wat is signaalverwerking?**

Signaalverwerking gaat over het manipuleren van signalen:
- Audio (geluid filteren, echo's, compressie)
- Video (filters, compressie, stabilisatie)
- Telecommunicatie (modulatie, demodulatie)
- Radar en sonar
- Medische apparatuur (ECG, MRI verwerking)
- Sensoren (beeldstabilisatie in camera's)

**Typische DSP operaties:**

**1. Convolutie**

[Convolutie](https://nl.wikipedia.org/wiki/Convolutie) is een wiskundige operatie waarbij je twee signalen combineert om een nieuw signaal te maken. Dit wordt veel gebruikt voor:
- **Filters**: Bijvoorbeeld een blur filter op een foto - elk pixel wordt het gemiddelde van zijn buren
- **Echo's en reverb**: Het originele geluid wordt gemengd met vertraagde kopieën
- **Edge detection**: In beeldverwerking om randen te vinden

**Wat betekent de formule?**

De formule $y[n] = \sum_{k} x[k] \cdot h[n-k]$ zegt eigenlijk:
- $x[k]$ is het ingangssignaal (bijvoorbeeld geluid of een rij pixels)
- $h[n-k]$ is de "impulse response" of filter-kern (bijvoorbeeld: hoeveel echo, of welke pixels tel je mee)
- Voor elke output positie $n$ tel je alle $x[k]$ waarden op, gewogen met $h[n-k]$

**Simpel voorbeeld:** Gemiddelde van 3 getallen
```
Ingang:  [5, 10, 15, 20, 25]
Filter:  [1/3, 1/3, 1/3]  (gemiddelde van 3)
Output:  [10, 15, 20]     (elk getal is het gemiddelde van 3 buren)
```

```{figure} assets/verdieping_architectuur/convolutie_voorbeeld.png
---
scale: 50%
align: center
---
Convolutie: elk output sample is een gewogen som van input samples
```

**2. FFT (Fast Fourier Transform)**

De [Snelle Fourier-transformatie](https://nl.wikipedia.org/wiki/Snelle_Fouriertransformatie) zet een signaal om van het tijdsdomein naar het frequentiedomein. Dit klinkt ingewikkeld, maar het idee is simpel:

**Tijdsdomein**: Je ziet hoe een signaal verandert in de tijd
**Frequentiedomein**: Je ziet welke frequenties (tonen) in het signaal zitten

**Toepassingen:**
- **MP3 compressie**: Verwijder frequenties die mensen niet kunnen horen
- **JPEG compressie**: Verwijder hoge frequenties (details) die je oog niet ziet
- **Stemherkenning**: Analyseer welke tonen iemand uitspreekt
- **Equalizer**: Pas specifieke frequenties aan (bas, midden, hoog)

```{figure} assets/verdieping_architectuur/fft_tijdfrequentie.png
---
scale: 50%
align: center
---
FFT zet een tijdsignaal om naar frequenties
```

**3. Correlatie**

[Correlatie](https://nl.wikipedia.org/wiki/Correlatie) meet hoe vergelijkbaar twee signalen zijn. Het is vergelijkbaar met convolutie, maar dan zoek je naar overeenkomsten.

**Toepassingen:**
- **Patroonherkenning**: Zoek een specifiek geluid in een audio-opname
- **GPS synchronisatie**: Vind het juiste tijdstip in satelliet signalen
- **Barcode/QR-code scanning**: Herken eenvoudige patronen in camera beelden
- **Radar en sonar**: Detecteer een echo van een uitgezonden puls

**Voorbeeld:** Zoek de melodie "do-re-mi" in een lange muziekopname. Correlatie geeft een hoge waarde waar de melodie voorkomt.

```{figure} assets/verdieping_architectuur/correlatie_patroon.png
---
scale: 50%
align: center
---
Correlatie vindt patronen in signalen
```

**4. FIR/IIR filters**

[Digitale filters](https://nl.wikipedia.org/wiki/Digitaal_filter) zijn essentieel in signaalverwerking om bepaalde frequenties te behouden of te verwijderen.

**FIR (Finite Impulse Response)**
- Gebruikt alleen input samples
- Altijd stabiel (kan niet "ontploffen")
- Gebruikt voor: lineaire-fase filters, equalizers

**IIR (Infinite Impulse Response)**
- Gebruikt ook eerdere output samples (heeft "geheugen")
- Efficiënter (minder berekeningen)
- Gebruikt voor: bas/treble filters, compressors

**Toepassingen:**
- **Audio equalizer**: Verhoog bas, verlaag treble
- **Noise cancellation**: Filter achtergrondgeluid eruit
- **Anti-aliasing**: Voorkom vervelende artefacten bij sampling
- **Beeldverwerking**: Blur, sharpen, edge detection

```{figure} assets/verdieping_architectuur/fir_iir_filters.png
---
scale: 50%
align: center
---
FIR en IIR filters in actie
```

### DSP architectuur kenmerken

Een typische DSP heeft:

**1. Harvard architectuur**
- Parallelle toegang tot instructies en data
- Cruciaal voor real-time verwerking

**2. MAC-eenheid (Multiply-Accumulate)**

De meest voorkomende operatie in signaalverwerking is:
```
accumulator += a * b
```

Een DSP kan dit in één cyclus:
```
MAC: acc = acc + (x * y)  // 1 cyclus!
```

Een normale CPU zou nodig hebben:
```
MULT temp, x, y   // 1 cyclus
ADD acc, acc, temp // 1 cyclus
```

**3. Speciale adresseringsmodi**

**Circulaire buffers**: voor audio buffers die constant worden opgevuld
```
buffer[i++ % N]  // automatisch in hardware!
```

**Bit-reversed addressing**: voor efficiënte FFT

**4. Zero-overhead loops**

Loops zonder extra cycli voor loop-controle (teller, vergelijking, sprong)

**5. Fixed-point arithmetic**

Veel DSP's gebruiken {ref}`fixed-point <fixed-point-notatie>` (vaste komma) in plaats van floating point:
- Sneller
- Minder energie
- Voldoende precisie voor veel toepassingen

### Voorbeelden van DSP's

**Texas Instruments TMS320 serie**
- Een van de populairste DSP families
- Gebruikt in audio apparatuur, telecom, automotive

**Analog Devices SHARC**
- Hoge prestaties
- Gebruikt in professionele audio apparatuur

**ARM Cortex-M met DSP extensions**
- DSP mogelijkheden in een microcontroller
- Gebruikt in embedded systems (drones, wearables)

**Qualcomm Hexagon**
- DSP in Snapdragon processors (smartphones)
- Voor camera ISP, audio, sensoren

### DSP in je telefoon

Je smartphone heeft waarschijnlijk meerdere DSP's:

**Audio DSP**
- Noise cancellation bij telefoongesprekken
- Equalizer, bass boost
- Voice recognition ("Hey Siri", "OK Google")

**Image Signal Processor (ISP)**
- RAW sensor data → JPEG
- Auto-focus, auto-exposure
- Face detection
- HDR, night mode

**Modem DSP**
- 4G/5G signaalverwerking
- Modulatie, demodulatie
- Error correction

**Sensor Hub**
- Verwerking van accelerometer, gyroscoop, kompas
- Stappenteller
- Altijd aan, zeer energie-efficiënt

:::{exercise}

1. Leg uit waarom de Harvard architectuur sneller kan zijn dan von Neumann voor real-time signaalverwerking.

2. Waarom heeft een DSP speciale hardware voor multiply-accumulate (MAC) operaties?

3. Geef drie voorbeelden van signaalverwerking die nu in jouw smartphone plaatsvindt.

4. Leg uit waarom een circulaire buffer nuttig is voor audio-verwerking.

5. **Onderzoeksproject:** Zoek uit wat een "Image Signal Processor" (ISP) doet in een smartphone camera en waarom dit geen normale CPU is.

6. **Programmeer uitdaging:** Implementeer een simpel lopend gemiddelde filter in je favoriete programmeertaal:
   ```
   output[n] = (input[n] + input[n-1] + input[n-2]) / 3
   ```
   Test het op een array van random getallen.

:::

## Cache theorie: snelheidswinst door slim geheugen

Een van de grootste uitdagingen in moderne computers is de enorme snelheidsverschil tussen de processor en het hoofdgeheugen. Dit heet de *memory wall* of *von Neumann bottleneck*. De oplossing: **cache geheugen**.

TODO: afbeelding von Neumann architectuur

### Het snelheidsprobleem

**Processor snelheid vs geheugen snelheid**

Een moderne processor draait op bijvoorbeeld 3 GHz (3 miljard cycli per seconde). Een geheugenoperatie duurt ongeveer 100-200 cycli. Als de processor bij elke instructie op het geheugen moet wachten, staat hij 99% van de tijd stil!

Vergelijking:
- Processor: sportwagen (300 km/h)
- RAM: vrachtwagen (30 km/h)
- Harde schijf: wandelaar (3 km/h)

```{mermaid}
graph TD
    CPU[CPU<br/>3 GHz<br/>~0.3 ns] 
    L1[L1 Cache<br/>~1 ns]
    L2[L2 Cache<br/>~3-10 ns]
    L3[L3 Cache<br/>~10-20 ns]
    RAM[RAM<br/>~100 ns]
    SSD[SSD<br/>~50,000 ns]
    HDD[HDD<br/>~5,000,000 ns]
    
    CPU --> L1
    L1 --> L2
    L2 --> L3
    L3 --> RAM
    RAM --> SSD
    SSD --> HDD
    
    style CPU fill:#f99
    style L1 fill:#ff9
    style L2 fill:#9f9
    style L3 fill:#9ff
    style RAM fill:#99f
    style SSD fill:#bbf
    style HDD fill:#ddf
```

### Wat is cache?

**Cache** is een klein, snel geheugen tussen de processor en het hoofdgeheugen. Het bevat kopieën van vaak gebruikte data uit het langzamere hoofdgeheugen. Wanneer een CPU data nodig heeft uit het geheugen en het is opgeslagen in de cache, kan de data sneller naar de CPU verstuurd worden vanuit de cache. Dit is dus een mooie tijdwinst.

**Cache hiërarchie**

Moderne processors hebben meestal 3 niveaus (levels) cache:

**L1 Cache (Level 1)**
- Klein (32-64 KB per core)
- Zeer snel (1-2 cycli)
- Vaak gesplitst: L1i (instructies) en L1d (data) → Harvard!
- Direct in de processor core

**L2 Cache (Level 2)**
- Middelgroot (256 KB - 1 MB per core)
- Snel (10-20 cycli)
- Meestal ook per core

**L3 Cache (Level 3)**
- Groot (8-64 MB)
- Langzamer (30-70 cycli), maar nog steeds veel sneller dan RAM
- Gedeeld tussen alle cores

### Hoe werkt cache?

**Basis principe: lokaliteit**

Cache werkt goed door twee vormen van lokaliteit:

**1. Temporal locality (tijdslokaliteit)**

Als je data net gebruikt hebt, gebruik je het waarschijnlijk binnenkort weer.

Voorbeeld:
```python
sum = 0
for i in range(1000):
    sum += i  # variabele 'sum' wordt steeds opnieuw gebruikt
```

Wanneer de variabele `sum` in de cache zit, hoeft deze dus niet steeds uit het geheugen gehaald te worden. Nog sneller zou natuurlijk zijn om de variabele `sum` in een register te zetten, maar dat is niet altijd mogelijk.

**2. Spatial locality (ruimtelijke lokaliteit)**

Als je data op adres X gebruikt, gebruik je waarschijnlijk ook data op adres X+1, X+2, etc.

Voorbeeld:
```python
for i in range(1000):
    array[i] = 0  # opeenvolgende array elementen
```

**Het voordeel van spatial locality**

Spatial locality zorgt voor veel cache hits omdat de cache werkt met *cache lines*: blokken van bijvoorbeeld 64 bytes die in één keer uit RAM worden geladen. Als je één byte nodig hebt, worden automatisch ook de 63 bytes erna in de cache geladen.

**Voorbeeld: 2D array traversal**

Stel je hebt een matrix van 1000×1000 integers (4 bytes per integer). Cache line grootte is 64 bytes = 16 integers.

*Methode 1: Rij voor rij (goede spatial locality)*
```python
for i in range(1000):
    for j in range(1000):
        matrix[i][j] = 0  # [0][0], [0][1], [0][2], ...
```

In het geheugen liggen rij-elementen naast elkaar. Bij toegang tot `matrix[0][0]` worden `matrix[0][0]` t/m `matrix[0][15]` in de cache geladen. De volgende 15 toegangen zijn dus cache hits!
- Eerste toegang per 16: cache miss
- Volgende 15 toegangen: cache hits
- Hit rate: ~94% (15/16)

*Methode 2: Kolom voor kolom (slechte spatial locality)*
```python
for j in range(1000):
    for i in range(1000):
        matrix[i][j] = 0  # [0][0], [1][0], [2][0], ...
```

Nu spring je tussen rijen: `matrix[0][0]`, dan `matrix[1][0]`, dan `matrix[2][0]`, etc. Elke rij ligt 4000 bytes verder in het geheugen. De cache line die je laadt bij `matrix[0][0]` (met `matrix[0][0]` t/m `matrix[0][15]`) gebruik je niet voor de volgende toegang!
- Bijna elke toegang: cache miss
- Hit rate: ~5-10%

**Time difference:**
```
Methode 1: 1 miljoen × (6% × 100ns + 94% × 2ns) = 7.9 ms
Methode 2: 1 miljoen × (95% × 100ns + 5% × 2ns) = 95.1 ms
```

Methode 1 is **12× sneller** door goede spatial locality! Dit is waarom programmeurs altijd proberen data sequentieel te benaderen.

### Cache hits en misses

**Cache hit**: Wanneer de door de CPU opgevraagde data in de cache zit, noemen we dit een *cache hit*. Met zo'n hit is de data dus erg snel toegankelijk voor de CPU en worden er geen kostbare cycles gewacht op data. 
**Cache miss**: In het geval dat de door de CPU opgevraagde data niet in de cache is opgeslagen, dan spreken we van een *cache miss*. Zo'n miss zorgt ervoor dat de data uit het tragere RAM geheugen moet komen.

**Hit rate**: Je kunt de kwaliteit van de afstemming van de programmacode op de cache meten met de zogenaamde *hit rate*. Dit is het percentage van de memory toegangen dat in cache zit. Je zult dit misschien herkennen als een procent-berekening van wiskunde. Nou, dat is het precies :)
```
Hit rate = hits / (hits + misses)
```

Voorbeeld:
- 95 hits, 5 misses uit 100 toegangen
- Hit rate = 95 / 100 = 95%

**Gemiddelde toegangstijd:** Wanneer je de hitrate weet, kun je ook de *gemiddelde toegangstijd* voor een geheugenoperatie berekenen. Je hebt hiervoor de *hit rate* en *miss rate* nodig. De *miss rate* bereken je als `miss rate = 100 - hit rate`. Daarnaast heb je de toegangstijden voor de cache en het RAM geheugen nodig. Wanneer je een berekening wil maken voor een CPU zonder enige cache, dan zet je de hit rate op `0` en de miss rate op `100`.


```
Average = (hit rate × cache tijd) + (miss rate × RAM tijd)
```

Voorbeeld:
- Cache: 2 ns
- RAM: 100 ns
- Hit rate: 95%

```
Average = (0.95 × 2) + (0.05 × 100)
        = 1.9 + 5
        = 6.9 ns
```

Zonder cache zou elke toegang 100 ns duren. Met cache is het ~14× sneller!

### Cache mapping strategieën
Het rekenen en inzien dat een cache daadwerkelijk tijdwinst op kan leveren is mooi en aardig, maar hoe besluit de cache welke data op te slaan en waar? Dat kan grofweg op drie verschillende manieren.

**1. Direct Mapped Cache**

Elk geheugenadres kan op maar één plek in de cache.
```
Cache positie = (adres) % (cache grootte)
```

Dus in een rekenvoorbeeld: stel dat je een (superkleine) cache hebt van 8 bytes. Dan komen RAM-adressen `0, 8, 16, ...` op plek `0` in de cache. RAM-adressen `1, 9, 17, ...` staan op plek `1` in de cache, enzovoorts. 

Het voordeel van deze methode is dat het mappen van de cache heel snel kan plaats vinden omdat het een 'simpele', haast naieve methode is. Het nadeel is dat het veel cache conflicts kan veroorzaken. 

**Een cache conflict** is wanneer verschillende RAM-adressen concurreren om dezelfde cache plek. Hier is een concreet voorbeeld van wat er misgaat:

*Scenario:* Je programma wisselt constant tussen RAM-adres 0 en RAM-adres 8:
```
1. Lees adres 0  → cache positie 0 (0 % 8 = 0) → MISS, laad in cache
2. Lees adres 8  → cache positie 0 (8 % 8 = 0) → MISS, gooi adres 0 eruit, laad adres 8
3. Lees adres 0  → cache positie 0 (0 % 8 = 0) → MISS, gooi adres 8 eruit, laad adres 0
4. Lees adres 8  → cache positie 0 (8 % 8 = 0) → MISS, gooi adres 0 eruit, laad adres 8
...
```

Resultaat: **100% cache misses**, terwijl 7 van de 8 cache posities leeg blijven! Dit is het probleem van een direct mapped cache: twee veelgebruikte adressen kunnen elkaar constant uit de cache gooien.

**Samengevat**

Voordeel: Simpel, snel

Nadeel: Veel cache conflicts (verschillende adressen concurreren om dezelfde cache plek)
 
**2. Fully Associative Cache**

Elk geheugenadres kan op elke willekeurige plek in de cache.

Bij deze methode wordt voor elke cache-plek een **tag** bijgehouden die aangeeft welk RAM-adres erin zit. Bij elke cache-toegang moeten alle tags gecontroleerd worden om te zien of het gezochte adres in de cache staat. Dit heet een *associative search*.  Dit zorgt natuurlijk voor vertraging. Dus dan is de CPU weer aan het wachten op data. En dat was nou precies wat we met de cache wilden oplossen. Een absoluut voordeel aan deze manier van organiseren, is dat je geen enkele cache miss krijgt.

**Samengevat**

Voordeel: Geen conflicts, beste hit rate

Nadeel: Complex, traag om te zoeken

**3. N-way Set Associative Cache** (meest gebruikt)

De N-way set associative cache combineert het beste van beide vorige methoden. De cache wordt opgedeeld in *sets*, en elk geheugenadres heeft een vaste set waar het in kan - maar binnen die set mag het op elke vrije plek staan.

**Voorbeeld: 4-way set associative cache van 8 sets**

```
Set = (adres) % (aantal sets)   → bepaalt welke set (snel, zoals direct mapped)
Binnen de set: keuze uit 4 plekken → voorkomt conflicts (zoals fully associative)
```

Terug naar het conflict-voorbeeld: RAM-adressen 0 en 8 komen nu allebei in set 0, maar die set heeft 4 plekken. Ze kunnen dus *naast elkaar* in de cache bestaan zonder elkaar eruit te gooien!

```
Set 0: [adres 0 | adres 8 | leeg | leeg]   ← geen conflict meer!
Set 1: [leeg | leeg | leeg | leeg]
...
```

Pas als er 5 of meer adressen in dezelfde set zitten, ontstaat er een conflict. De kans daarop is bij 4 plekken per set veel kleiner dan bij direct mapped.

De *N* in "N-way" staat voor het aantal plekken per set. Een 2-way cache heeft 2 plekken per set, een 8-way cache heeft er 8. Moderne processors gebruiken vaak 4-way of 8-way set associative caches als L1, en 16-way voor L3.

**Samengevat**

Voordeel: Goede balans tussen snelheid en hit rate

Nadeel: Complexer dan direct mapped

### Cache replacement policies

Als de cache vol is, welke data gooi je eruit?

**LRU (Least Recently Used)**
- Gooi de data eruit die het langst niet gebruikt is
- Meest gebruikt in praktijk
- Goede prestaties voor de meeste programma's. Klinkt eigenlijk ook best logisch. Heb je iets lang niet meer gebruikt, dan is de kans klein dat je het binnenkort nodig hebt. Behalve die ene kabel uit die doos met oude kabels (IYKYK).

**FIFO (First In, First Out)**
- Gooi de oudste data eruit. De ouderdom wordt op basis van tijdstip van toevoeging bepaald en niet zozeer door het laatste gebruik (zoals de LRU hierboven werkt).
- Simpeler dan LRU
- Iets slechtere prestaties. 

**Random**
- Kies willekeurige data om eruit te gooien
- Zeer simpel
- Soms verrassend goed

**LFU (Least Frequently Used)**
- Gooi data eruit die het minst gebruikt is
- Complexer
- Kan beter zijn voor bepaalde workloads (een database buffercache of een Web/CDN cache).

### Write policies

Wat gebeurt er als je data in cache wijzigt? Je hebt twee instanties van de data. Eentje in het RAM-geheugen en eentje in de cache. Wat je te alle tijde wil voorkomen is dat er een mismatch is tussen RAM en cache, zonder dat dit bekend is. Dus wanneer de CPU data wil opslaan, zal dit eerst in de cache gewijzigd worden. Dan zijn er twee keuzes te maken over wanneer je de data terugschrijft naar RAM. Direct of uitgesteld.

**Write-through (direct)**
- Schrijf naar cache EN direct naar RAM
- Voordeel: RAM is altijd up-to-date
- Nadeel: Langzamer (elke write gaat naar RAM). Voor read-heavy workloads maakt dit niet veel uit.

**Write-back (uitgesteld)**
- Schrijf alleen naar cache, pas later naar RAM
- Voordeel: Sneller
- Nadeel: Deze aanpak is complexer, want je per cache-entry bijhouden of de cache data gewijzigd is. Dit gebeurt middels een **dirty bit**, die aangeeft of de cache-data gewijzigd is. Dan weet je dat die data nog naar de RAM geschreven moet worden.


### Cache coherency (multi-core)

We zijn er tot nu toe van uit gegaan dat er slechts 1 CPU betrokken is bij de cache. Dus is er ook maar 1 CPU, die schrijven en lezen in het geheugen via de cache. Wat nou als je meerdere CPU's of cores hebt? En wat nou als die cores ieder een eigen cache hebben. Hoe hou je de boel gesynchroniseerd?

**Probleem:**
```
Core 1: Leest X = 5 (in L1 cache)
Core 2: Leest X = 5 (in L1 cache)
Core 1: Schrijft X = 10 (in zijn L1)
Core 2: Leest X... krijgt 5! (verouderd!)
```

**Oplossing: Cache coherence protocols**

**MESI protocol** (meest gebruikt):
- **M**odified: Deze cache heeft de enige geldige kopie (en hij is gewijzigd)
- **E**xclusive: Deze cache heeft de enige kopie (maar niet gewijzigd)
- **S**hared: Meerdere caches hebben een kopie
- **I**nvalid: Deze cache heeft een verouderde kopie

Bij een write naar cache in Modified/Exclusive mode:
1. Invalideer kopieën in andere caches
2. Forceer ze opnieuw uit RAM te halen

### False sharing

Een subtiel multi-core probleem:

```c
struct {
    int counter1;  // gebruikt door core 1
    int counter2;  // gebruikt door core 2
} data;
```

Ook al gebruiken de cores verschillende variabelen, ze zitten in dezelfde **cache line** (typisch 64 bytes). Elke write door één core invalideert de cache van de andere!

Oplossing: **padding**
```c
struct {
    int counter1;
    char padding[60];  // Forceer verschillende cache lines
    int counter2;
} data;
```

Het detecteren van _false sharing_ doe je door middel van een profiler. Daarmee kun je de prestatie van code fragmenten meten. De toepassingsgebieden, waarin er actief gezocht wordt naar false sharing bij multi-core performance zijn game engines, database engines, high frequency trading en operating system kernels. De padding-oplossing wordt ook pas gebruikt, nadat er gemeten is!

### Cache als beveiligingsrisico: Spectre en Meltdown

De cache is gedeelde hardware tussen processen. Dat maakt het ook een doelwit voor aanvallen. In 2018 werden twee van de meest impactvolle CPU-kwetsbaarheden ooit ontdekt: [Meltdown](https://nl.wikipedia.org/wiki/Meltdown_(beveiligingslek)) en [Spectre](https://nl.wikipedia.org/wiki/Spectre_(beveiligingslek)).

**Het aanvalsprincipe: timing als zijkanaal**

De aanval maakt gebruik van *speculatieve uitvoering* (de CPU voert alvast instructies uit die misschien niet nodig zijn) én de cache:

1. Verleid de CPU om *speculatief* verboden data te lezen — data die eigenlijk van een ander proces of de kernel is
2. Die verboden data landt in de **cache**, ook al wordt de instructie achteraf door de CPU teruggedraaid
3. Meet hoe lang geheugentoegangen duren: een snelle toegang = cache hit = die data *was* al geladen
4. Zo kun je bit voor bit verboden geheugen *aflezen via timing*, zonder het ooit formeel te "lezen"

**Waarom is dit zo ernstig?**

Een normaal programma kan in principe het geheugen van andere programma's of de kernel niet lezen — het besturingssysteem en de hardware voorkomen dat. Maar via dit zijkanaal omzeil je die bescherming volledig, zonder dat de CPU een foutmelding geeft.

**De fix en de prijs**

De oplossing heet **KPTI** (Kernel Page-Table Isolation): de page tables van de kernel en van gebruikersprogramma's worden strikt gescheiden gehouden. Dit sluit het zijkanaal, maar kost wel ~5-30% performance — elke keer dat een programma de kernel aanroept (voor bijvoorbeeld bestandstoegang of netwerk) moet de CPU een dure context-switch maken.

Dit is een mooi voorbeeld van de spanning tussen **prestatie** en **veiligheid** in hardware-ontwerp. De speculatieve uitvoering die CPU's zo snel maakt, blijkt ook een beveiligingsrisico te zijn.

### Cache-friendly programmeren

Je kunt code schrijven die beter gebruik maakt van cache:

**Goed: Spatial locality**
```python
# Rijen eerst (cache-friendly)
for i in range(1000):
    for j in range(1000):
        matrix[i][j] = 0
```

**Slecht: Cache misses**
```python
# Kolommen eerst (cache-unfriendly voor row-major arrays)
for j in range(1000):
    for i in range(1000):
        matrix[i][j] = 0
```

In het eerste geval lees je opeenvolgende memory locaties (cache hit!).
In het tweede geval spring je steeds door het geheugen (cache miss!).

**Loop tiling/blocking**

Voor grote data: verdeel in kleine blokken die in cache passen.

```python
# Zonder blocking
for i in range(N):
    for j in range(N):
        for k in range(N):
            C[i][j] += A[i][k] * B[k][j]

# Met blocking (cache-friendly)
for ii in range(0, N, BLOCK):
    for jj in range(0, N, BLOCK):
        for kk in range(0, N, BLOCK):
            for i in range(ii, min(ii+BLOCK, N)):
                for j in range(jj, min(jj+BLOCK, N)):
                    for k in range(kk, min(kk+BLOCK, N)):
                        C[i][j] += A[i][k] * B[k][j]
```

:::{exercise}

1. Leg uit waarom een computer met cache geheugen veel sneller is dan zonder, ook al is de CPU even snel.

2. Bereken de gemiddelde toegangstijd voor:
   - L1 cache: 2 ns, hit rate 90%
   - L2 cache: 10 ns, hit rate 95% (van L1 misses)
   - RAM: 100 ns

3. Waarom is het lopen door een array rij-voor-rij meestal sneller dan kolom-voor-kolom?

4. Leg uit wat het verschil is tussen write-through en write-back cache.

5. **Experiment:** Schrijf een programma dat een grote 2D array doorloopt op twee manieren (rij-eerst vs kolom-eerst) en meet het tijdsverschil. Gebruik bij voorkeur een gecompileerde taal (C, C++, Rust, GoLang) en niet een geinterpreteerde taal als JavaScript of Python.

6. **Onderzoeksproject:** Zoek uit hoeveel L1, L2 en L3 cache jouw processor heeft. Gebruik tools zoals `lscpu` (Linux/Mac) of CPU-Z (Windows).

7. **Uitdaging:** Leg uit waarom "loop tiling" de cache hit rate kan verbeteren bij matrix vermenigvuldiging.

:::

## Afsluiting

In dit verdiepingshoofdstuk heb je geavanceerde computerarchitectuur concepten geleerd:

- **SIMD en MIMD**: Verschillende vormen van parallellisme en hoe ze gebruikt worden
- **GPU's**: Massief parallelle processors voor graphics en general-purpose computing
- **TPU's**: Gespecialiseerde hardware voor machine learning met systolic arrays
- **Harvard architectuur en DSP's**: Alternatieve architecturen voor gespecialiseerde toepassingen
- **Cache theorie**: Hoe geheugen hiërarchieën computers snel maken ondanks de memory wall

Deze concepten zijn fundamenteel voor het begrijpen van moderne computing, van je smartphone tot supercomputers en AI-systemen. De trend is duidelijk: steeds meer specialisatie voor specifieke taken, gecombineerd met slimme geheugen hiërarchieën om de kloof tussen processor en geheugen te overbruggen.

In een vervolgopleiding zul je hier nog veel dieper op ingaan, met bijvoorbeeld vakken als computerarchitectuur, parallel computing, en hardware design!