# (10p) Opdracht 1
## (8p) Waarheidstabel
|in1|	in2|	in3|	in4|	out1|	out2|	out3|	out4|
|-|-|-|-|-|-|-|-|
|0 	|0 	|0 	|0 	|0 	|0 	|0 	|0 |
|0 	|0 	|0 	|1	|1	|1	|1	|1 |
|0 	|0 	|1	|0 	|1	|1	|1	|0 |
|0 	|0 	|1	|1	|1	|1	|0 	|1 |
|0 	|1	|0 	|0 	|1	|1	|0 	|0 |
|0 	|1	|0 	|1	|1	|0 	|1	|1 |
|0 	|1	|1	|0 	|1	|0 	|1	|0 |
|0 	|1	|1	|1	|1	|0 	|0 	|1 |
|1	|0 	|0 	|0 	|1	|0 	|0 	|0 |
|1	|0 	|0 	|1	|0 	|1	|1	|1 |
|1	|0 	|1	|0 	|0 	|1	|1	|0 |
|1	|0 	|1	|1	|0 	|1	|0 	|1 |
|1	|1	|0 	|0 	|0 	|1	|0 	|0 |
|1	|1	|0 	|1	|0 	|0 	|1	|1 |
|1	|1	|1	|0 	|0 	|0 	|1	|0 |
|1	|1	|1	|1	|0 	|0 	|0 	|1 |

In totaal 8 punten voor de schakeling.
De eerste 4 rijen zijn gegeven.
Er mogen in totaal 8 fouten gemaakt worden.
Per kolom maximaal 2. Wanneer een ll 1 kolom gemist heeft, zijn *alle* waardes fout. Dus per kolom nakijken.

## (2p) Functie van deze schakeling
(1p) Functie: negatief, 2 bits complement of iets wat hierop lijkt.
(1p) Onderbouwing: waaraan het te zien is.

# (8p) Opdracht 2
Locaties: ALU, CU, Registers, RAM, Gegevensbus, Adresbus
Acties: Fetch, Decode, Execute, Store, Load, Branch

## (4p) Instructie 1
Laad een getal uit het geheugen (0x002A) in register R1 en vermenigvuldig het met de waarde van R2.
1. RAM
2. Load
3. Gegevensbus
4. Fetch
5. CU
6. Decode
7. Load
8. Adresbus
9. RAM
10. Gegevensbus
11. Store
12. ALU
13. Adres

Deze opdracht klopt niet... hier moet ik nog serieus naar kijken.


## (4p) Instructie 2
Vergelijk de waarde van register R1 met R2. Als R1 groter is, spring naar instructie op adres 0x0030

Deze opdracht klopt ook niet. Hier moet ik nog serieus naar kijken.


# (27p) Opdracht 3

Zie Rubrics.

Testcases: 0, 1:N, 2:J, 3:J, 6:N, 7:J, 11:J
