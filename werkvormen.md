Werkvormen bij computerarchitectuur
---

# Architectuur

## Von Neumann architectuur
Kritische vraagstellingen:
- Wat zijn de grootste beperkingen van de Von Neumann-architectuur?
- Hoe zou je deze architectuur moeten veranderen om deze beperkingen tegen te gaan?

## AlU, CU, Registers
Werkvorm: Begripsketting

Doel: Leerlingen ontdekken de volgorde waarin gegevens en instructies door de CPU bewegen.

Hier is een voorstel voor de kaarten die je kunt aanbieden aan de leerlingen. Deze kaarten vertegenwoordigen de belangrijkste concepten en acties binnen de CPU. Je kunt ze in drie categorieën verdelen: onderdelen, acties, en gegevens/instructies.

### Categorie 1: Onderdelen

Deze kaarten vertegenwoordigen fysieke of logische onderdelen van de CPU:

1.	ALU (Arithmetic Logic Unit)
2.	CU (Control Unit)
3.	Registers
4.	Geheugen (RAM)
5.	Gegevensbus
6.	Adresbus
7.	Controlebus

### Categorie 2: Acties

Deze kaarten beschrijven wat de onderdelen doen:

1.	Fetch (Ophalen van een instructie uit het geheugen)
2.	Decode (Vertalen van de instructie door de CU)
3.	Execute (Uitvoeren van de instructie in de ALU)
4.	Store (Opslaan van een resultaat in een register of geheugen)
5.	Load (Laden van gegevens uit het geheugen naar een register)
6.	Branch (Een sprong maken naar een andere instructie)

### Categorie 3: Gegevens en instructies

Deze kaarten beschrijven wat er door de CPU beweegt:

1.	Instructie (bijvoorbeeld ADD, SUB, etc.)
2.	Gegevens (bijvoorbeeld een getal zoals 42)
3.	Adres (bijvoorbeeld geheugenlocatie 0x002A)

Hoe gebruik je de kaarten?

1.	Stap 1: Elk groepje krijgt een set kaarten uit de drie categorieën.
•	Bij een eenvoudigere opdracht kun je de categorieën beperken (bijvoorbeeld alleen onderdelen en acties).
•	Bij een complexere opdracht gebruik je alle drie de categorieën.
2.	Stap 2: Laat de leerlingen de kaarten in de juiste volgorde leggen om een gegevensstroom of instructie-uitvoering te simuleren.
•	Bijvoorbeeld: Geheugen → Fetch → Controlebus → CU → Decode → ALU → Execute → Registers → Store.
3.	Stap 3: Vraag hen om bij elke stap te noteren:
•	Wat gebeurt er precies?
•	Welk onderdeel voert deze actie uit?
•	Welke gegevens bewegen door de CPU?
4.	Stap 4: Presenteren en bespreken.
•	Laat elk groepje uitleggen waarom zij hun ketting zo hebben gelegd.
•	Laat andere groepen kritisch nadenken en vragen stellen: “Is dit echt de juiste volgorde?” of “Wat als een register vol is?”

Extra uitdaging

•	Voeg foutscenario’s toe, zoals: “De gegevensbus is overbelast” of “De instructie is niet volledig gedecodeerd”, en vraag hoe dat het proces beïnvloedt.
•	Laat ze een alternatieve ketting maken waarin inefficiënties worden aangepakt.

Hier is een uitgewerkte set met voorbeelden voor kaarten, inclusief kant-en-klare teksten, en enkele scenario’s om de werkvorm uitdagend te maken.

Kaarten: Tekstvoorbeelden per categorie

Categorie 1: Onderdelen (Blauwe kaarten)

	1.	ALU (Arithmetic Logic Unit): Voert berekeningen en logische operaties uit, zoals optellen (ADD) of vergelijken.
	2.	CU (Control Unit): Coördineert de werking van de CPU door instructies te decoderen en onderdelen aan te sturen.
	3.	Registers: Slaan gegevens tijdelijk op die snel toegankelijk moeten zijn voor de CPU.
	4.	Geheugen (RAM): Slaat programma-instructies en gegevens op.
	5.	Gegevensbus: Verplaatst gegevens tussen onderdelen.
	6.	Adresbus: Geeft aan waar in het geheugen gegevens of instructies te vinden zijn.
	7.	Controlebus: Stuurt signalen die aangeven welke acties moeten worden uitgevoerd.

Categorie 2: Acties (Groene kaarten)

	1.	Fetch: Haalt een instructie op uit het geheugen via de adresbus.
	2.	Decode: De CU vertaalt de opgehaalde instructie in acties.
	3.	Execute: De ALU voert de bewerking van de instructie uit (bijvoorbeeld optellen of vergelijken).
	4.	Store: Het resultaat van de bewerking wordt opgeslagen in een register of in het geheugen.
	5.	Load: Gegevens worden uit het geheugen geladen naar een register.
	6.	Branch: De CU bepaalt of de uitvoering naar een andere instructie moet springen op basis van een voorwaarde.

Categorie 3: Gegevens en instructies (Rode kaarten)

	1.	Instructie: ADD R1, R2 → Optel de waarde van register R1 bij R2.
	2.	Gegevens: 42 → Een getalwaarde die verwerkt moet worden.
	3.	Adres: 0x002A → De locatie in het geheugen waar de instructie of gegevens staan.
	4.	Resultaat: 84 → Het resultaat van een berekening of operatie.

Scenario’s om te gebruiken

Gebruik deze scenario’s om de leerlingen uit te dagen hun begripsketting te bouwen en te verdedigen:

Scenario 1: Basisstroom

•	Instructie: “Tel de waarde van register R1 en register R2 op en sla het resultaat op in register R3.”
•	Mogelijke oplossing:
•	Geheugen → Fetch → Adresbus → CU → Decode → Register → ALU → Execute → Register → Store

Scenario 2: Gegevens uit geheugen laden

•	Instructie: “Laad een getal uit het geheugen (0x002A) in register R1 en vermenigvuldig het met de waarde van R2.”
•	Mogelijke oplossing:
•	Geheugen → Fetch → Adresbus → CU → Decode → Geheugen → Load → ALU → Execute → Register → Store

Scenario 3: Branch bij voorwaarde

•	Instructie: “Vergelijk de waarde van register R1 met R2. Als R1 groter is, spring naar instructie op adres 0x0030.”
•	Mogelijke oplossing:
•	Geheugen → Fetch → Adresbus → CU → Decode → Register → ALU → Execute → CU → Branch → Adresbus

Extra uitdagingen

•	Scenario 4: Fout in de gegevensbus
•	Vraag: Wat gebeurt er als de gegevensbus de verkeerde gegevens doorstuurt? Hoe beïnvloedt dit de ketting?
•	Laat leerlingen een alternatieve ketting maken om de fout te corrigeren.
•	Scenario 5: Optimalisatie
•	Vraag: Hoe kun je de ketting efficiënter maken, bijvoorbeeld door registers her te gebruiken of minder geheugenacties uit te voeren?

Praktische uitvoering

1.	Print de kaarten in verschillende kleuren (bijvoorbeeld blauw, groen, rood) voor herkenbaarheid.
2.	Laat de leerlingen de kaarten fysiek op tafel leggen en schuiven om de ketting te vormen.
3.	Stimuleer discussie door vragen te stellen zoals:
•	“Waarom kies je deze volgorde?”
•	“Wat gebeurt er als deze stap wordt overgeslagen?”
4.	Geef feedback en laat groepen hun keuzes verdedigen voor de klas.

Wil je ook een visueel ontwerp voor de kaarten of aanvullende scenario’s?