 
 MACHINETAALCURSUS

 DEEL 2
 
 In het vorige deel behandel-
 den we in het kort even de 
 Z80 registers en hun functie.
 Om de gehele cursus de 
 begrijpen raden we aan dit 
 nog eens door te lezen.

 In dit deel zullen we 
 aandacht besteden aan de 
 verdere LOAD-instructies en
 het gebruik van LABELS 
 uitleggen.

 Een voorbeeld programma:
 
 	ORG 0A000H
 
 	LD     A,(GETAL1)
 	LD     B,A
 	LD     A,(GETAL2)
 	ADD    A,B
 	LD     (SOM),A
 	RET

 GETAL1:DEFB	06
 GETAL2:DEFB	09
 SOM:	DEFB	00


 Even als herhaling:

 ORG nn 

 ORG is een commando dat 
 aangeeft waar de machinetaal 
 in het geheugen moet 
 beginnen. Het staat na het 
 assembleren niet in de MCODE.
 
 RET

 RET betekent ongeveer 
 hetzelfde als RETURN in 
 BASIC. Waarom dan geen GOSUB?
 Simpel, MCODE mag gezien 
 worden als een "sub routine"
 van BASIC, waarbij BASIC het 
 hoofdprogramma is. Als je dus 
 vanuit MCODE terug wil naar 
 BASIC moet je dus een RET 
 geven.


 Dan nu, de LOAD-instructies.

 	LD	A,(GETAL1)

 GETAL1 noemt men een LABEL.
 Een LABEL kan men vergelijken
 met een regelnummer in BASIC.
 Je mag nu dus zelf een 
 geheugen adres een naam 
 geven. Deze naam of LABEL mag
 maximaal 6 letters lang zijn.
 
 Door de haakjes wordt 
 aangegeven dat register A 
 hier met de inhoud van GETAL1
 wordt "geladen". In A komt 
 dus nu de inhoud van het 
 adres waar het LABEL GETAL1 
 naar verwijst. 
 Zouden er geen haakjes omheen 
 gestaan hebben zou dit 
 betekenen dat A dan niet met
 de inhoud van GETAL1
 geladen zou worden maar met 
 het geheugen adres waar het 
 naar verwijst. Nu is A maar 
 een 8-bits register en kan 
 in A dus niet het geheugen 
 adres van GETAL1 gezet worden
 16-bits registers zoals b.v. 
 HL kunnen dit wel.

 	LD	B,A

 Dit commando zet in register 
 B de inhoud van register A,
 oftewel het eerste getal.
 De inhoud van het register A 
 wordt niet veranderd.

 	ADD	A,B

 Dit commando telt de 
 registers A en B bij elkaar 
 op en zet de som in register 
 A. (zie vorige cursus.)

 	LD	(SOM),A

 Je kunt ook een geheugenplaats
 laden met de inhoud van een 
 register. Het adres moet men 
 dan voorop zetten. Het werkt 
 hetzelfde als het laden van 
 registers met LABELS.
 Hier wordt dus in het adres
 SOM de som van de optelling 
 gezet. Ook de uitleg van de 
 haakjes geldt hier weer.
 
 GETAL1:DEFB	06
 GETAL2:DEFB	09
 SOM:	DEFB	00

 DEFB: DEFine Byte (bepaal 
 byte)

 DEFB is net als ORG een 
 commando dat niet in de MCODE 
 voorkomt. DEFB  06 wil zeggen 
 dat in deze geheugenplaats 
 het getal 6 wordt gezet.
 In het adres GETAL2 komt dus 
 het getal 9 en SOM wordt 
 gereserveerd voor de uitkomst

 Behalve DEFB bestaan er ook 
 nog:

 DEFS: DEFine Storage 
 (bepaal opslagruimte)

 DEFS 10 wil zeggen dat de 
 assembler 10 adressen 
 vrijhoudt waarin later iets 
 kan worden opgeslagen (b.v.
 de som van een 16-bits 
 optelling)

 DEFM: DEFine Memory
 (bepaal geheugen)

 DEFM "0123456789" wil zeggen
 dat de assembler 10 adressen 
 volzet met de ASCII codes van 
 de letters (of cijfers) 
 tussen de aanhalings-tekens.

 DEFW: DEFine Word
 (bepaal 2 geheugen adressen)

 DEFW 00 wil zeggen 2  adres-
 sen op 0 gezet worden om 
 later informatie op te slaan.	
 Dit commando wordt ook
 gebruikt om een geheugenadres
 aan te geven. 

 --------Woordenlijst--------

 ADRES/GEHEUGENADRES/GEHEUGEN-
 PLAATS:

 Een plaats in het geheugen 
 van de MSX. 
 
 POKE &HA000,10
 
 Zet op plaats &HA000 in het 
 geheugen de waarde 10.

 ASCII:

 Een internationaal aanvaarde 
 code om de letters en cijfers 
 voor de computer begrijpelijk 
 te maken.

 ASSEMBLER:

 Een programma dat het 
 assembleren uitvoert.
 
 ASSEMBLEREN:

 Het omzetten van machinetaal 
 naar MCODE. 
 Voorbeelden van machinetaal 
 zijn de listingen in deze 
 cursus.

 BIT:
 
 Een waarde die 0 of 1 kan 
 zijn.
 (zie vorige cursus)

 BYTE:

 Een groepje van 8-BITS.
 (zie vorige cursus)

 LABEL:

 Een naam voor een 
 geheugenplaats, vergelijkbaar 
 met een regelnummer in BASIC.

 MCODE:

 Dit is de taal waarin de 
 machinetaal commando's worden 
 omgezet en die de computer 
 kan begrijpen en uitvoeren.
 (zie vorige cursus)

 REGISTER:

 Een register is te 
 vergelijken met een variable 
 in BASIC.
 (zie vorige cursus)

 -----------------------------
  
 Hopelijk was deze cursus een 
 beetje te begrijpen en in de 
 volgende cursus zullen we 
 meer informatie geven over 
 o.a. INC,DEC,JP en CALL.

 Met vriendelijke programmeer-
 groet:
 
 Ruud Gelissen en
 Jan-Willem van Helden 
 
 Voor vragen zie de Helplines.

   
   
 
