# (10p) Opdracht 1
## (8p) Waarheidstabel
|in1|	in2|	in3|	in4|	out1|	out2|	out3|	out4|
|---|---|---|---|---|---|---|--|
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
(1p) Functie: negatief, 2 bits complement  of iets wat hierop lijkt.
(1p) Onderbouwing: waaraan het te zien is.

# (8p) Opdracht 2

## (3p) `ADD R1, #42`
**Fetch**
Register: lees PC en stuur naar CU <br/>
Register: hoog PC 1 op<br/>
CU: laad inhoud van geheugen op plek [PC] in CU.<br/>

**Decode**
CU: Decodeer instructie <br/>
`0001 0001 0010 1010`<br/>
>  Opcode: `00` (Formaat A)<br/>
>  Func: `010` (Add)<br/>
>  Rsd: `001` (R1)<br/>
>  Imm8: `0010 1010` (#42)<br/>

**Execute/store**</br>
Register: Lees register R1 en stuur naar ALU of CU</br>
ALU: voer rekenkundige bewerking 'add' uit</br>
Register: Schrijf register R1</br>

*Per juist weergegeven fase (FDE) 1 punt.*

## (4p) `CMP R2, R3`
**Fetch**

Register: lees PC en stuur naar CU <br/>
Register: hoog PC 1 op<br/>
CU: laad inhoud van geheugen op plek [PC] in CU.<br/>

**Decode**
CU: Decodeer instructie <br/>
`0111 0110 1001 0011`<br/>
>  Opcode: `011101` (Formaat B10)<br/>
>  Func: `1010` (CMP)<br/>
>  Rs: `010` (R2)<br/>
>  Rb: `011` (R3)</br>

**Execute**
Register: lees register R2 en stuur naar ALU of CU</br>
Register: lees register R3 en stuur naar ALU of CU</br>
ALU: voer andere bewerking `compare` uit</br>
CU: Zet vlag `Z` op `1`</br>
CU: Zet vlag `C` op `1`</br>

*Per juist weergegeven fase (FDE) 1 punt. Dat geeft in totaal 3 punten. Het vierde punt is voor het zetten van de juiste vlaggen.*

## (4p) `BNE 42`
**Fetch**

Register: lees PC en stuur naar CU <br/>
Register: hoog PC 1 op<br/>
CU: laad inhoud van geheugen op plek [PC] in CU.<br/>

**Decode**

CU: Decodeer instructie <br/>
`1000 0100 0010 1010`<br/>
>  Opcode: `100` (Formaat Branch)<br/>
>  Func: `0010` (BNE)<br/>
>  Address: `0 0010 1010` (#42)<br/>

**Execute**
CU: Lees statusvlag `Z` (=0) </br>
Register: Schrijf 42 in PC

*1 punt voor Fetch juist, 1 punt voor Decode juist, 2 punten voor Execute juist*


# (27p) Opdracht 3

Zie Rubrics.

Testcases: 0, 1:N, 2:J, 3:J, 6:N, 7:J, 11:J
