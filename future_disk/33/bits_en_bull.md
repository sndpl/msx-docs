 Programmeercursi zat dit keer...
 The lamest ML course in town...        Bits and bullshit...


                             ����
                             ����
                             ����
                             ����


 De DOS2.x text van deze keer was wat aan de korte kant, dus
 ik ga me maar eens op ML storten. En dit keer eens wat and-
 ers. We gaan het BIT commando helemaal uitdiepen. ALLES wat
 we er maar over moeten weten komt deze keer aan bod. Hou je
 vast dus, wat dit kon nog wel eens een flinke text worden.


She said byte me...

 BIT kan maar voor een ding gebruikt worden.. Om de toestand
 van een byte te bepalen. Door het juiste BIT commando te ge-
 bruiken kunnen we de  toestand van alle  bitjes in een byte
 afzonderlijk bepalen. Uiteraard kan dit op vele manieren en
 lang niet altijd zal BIT de handigste zijn,  maar BIT heeft
 wel een flink aantal mogelijkheden.
 
 
So I bit her...

 [BIT nummer,slachtoffer] Hierbij staat nummer voor het bit-
 nummer dat natuurlijk 0 tot 7 mag zijn. Het slachtoffer van
 deze bewerking kan werkelijk van alles zijn maar daar komen
 we zometeen op terug. BIT doet maar een belangrijk ding; de
 zero-flag wel of niet zetten... En dat wilden we nu net van
 het slachtoffer weten.


I must have misunderstood...

 Eerst alle mogelijkheden op een rijtje. Ik zal eerst de mo-
 gelijke BIT instructies maar eens op een rijtje zetten. Dan
 komt de onafhankelijke beschrijving later.

  BIT b,(HL)
  BIT b,(IX + n)
  BIT b,(IY + n)
  BIT b,r


'Coz she popped me dead...

 Nou, dat viel toch reuze mee of niet??? Laten we eerst alle
 instructies eens afzonderlijk onder de loep nemen, dan komt
 daarna de volledige lijst met tijdsduur etc wel...


BIT b,(HL)

 Bit b van de geheugen locatie geadresseerd door HL wordt ge-
 test en de zero-flag wordt overeenkomstig met het resultaat
 geset. De bbb in de object codes komen overeen met het geko-
 zen bit voor de test.

  OBJ CODE:     Byte 1:  %11001011 ($CB)
                Byte 2:  %01bbb110 ( - )


BIT b,(IX + n)

  Bit b van de geheugen-locatie geaddresseerd  door de inhoud
  van register IX plus de verplaatsing [n] wordt getest en de
  zero-flag wordt overeenkomstig met bit b geset.  De n in de
  object code staat voor de opgegeven verplaatsing.

  OBJ CODE:     Byte 1:  %11011101 ($DD)
                Byte 2:  %11001011 ($CB)
                Byte 3:  %nnnnnnnn ( n )
                Byte 4:  %01bbb110 ( - )


BIT b,(IY + n)

  Bit b van de geheugen-locatie geaddresseerd  door de inhoud
  van register IY plus de verplaatsing [n] wordt getest en de
  zero-flag wordt overeenkomstig met bit b geset.

  OBJ CODE:     Byte 1:  %11011101 ($FD)
                Byte 2:  %11001011 ($CB)
                Byte 3:  %nnnnnnnn ( n )
                Byte 4:  %01bbb110 ( - )


 BIT b,r

  Bit b van register r wordt  getest en de z-flag wordt over-
  eenkomstig geset. De r in de object code staat voor de bin-
  aire code achter elk register.

  r:  A %111  B %000  C %001  D %010  E %011  H %101   L %101

  OBJ CODE:     Byte 1:  %11001011 ($CB)
                Byte 2:  %01bbbrrr ( - )


And that's it...

 En dat was het al weer. Tijd dus om de timing van deze type
 BIT instructies eens te bekijken. Ik zal de timing in mach-
 ine cycli geven en in T-States.  Om dus de duur van een in-
 structie te berekenen moet het aantal T-States vermenigvul-
 digd worden met de duur van een cyclus... De M-Cycli hebben
 daar dus niets mee te maken. De snelheid in miliseconden is
 variabel (ligt aan het type processor; Z80a/Z80b/Z80h/R800)
 dus die geef ik niet op. De R800 is overigens een apart ge-
 val... Daar gaat het namelijk niet om de clock snelheid van
 7MHz maar om het feit dat de R800 ook nog eens minder Cycli
 nodig heeft om een BIT instructie uit te voeren... Zeker de
 instructie met de index registers gaan flink wat sneller!!!
 Hier is de R800 dus op twee punten sneller dan de Z80a. Ten
 eerste omdat de clock snelheid hoger ligt en ten tweede om-
 dat de R800 minder T-States nodig heeft.


       |  Instructie      |  M Cylci    |  T States     |
       +------------------+-------------+---------------+
       |  BIT b,(HL)      |  3 M Cycli  |  12 T States  |
       |  BIT b,(IX + n)  |  5 M Cycli  |  20 T States  |
       |  BIT b,(IY + n)  |  5 M Cylci  |  20 T States  |
       |  BIT b,r         |  2 M Cylci  |  08 T States  |


That's all folks...

 En dan zit het er echt op!! Op een ding na natuurlijk... De
 lijst van alle mogelijke bit instructies en hun object code
 natuurlijk.  Om de routine van Smael niet al te erg te tar-
 ten zal ik hiervoor maar een aparte text typen, lees dus in
 deel twee verder voor alle instucties, met alle OBJ codes.

 Voor nu, slaap lekker, en tot de volgende FD...


 Dhr Keizer...ah whatever ik weet het ook niet
     To byte, bit, bit....

                             ����
                             ����
                             ����
                             ����


 Okay, beloofd is beloofd,  alle mogelijke instructies,  met
 alle objectcodes... Tja, hoe krijg je zo'n diskmagazine an-
 der vol tegewoordig. (Hint: Met lezersbrieven...) Oh wacht,
 laat ik wel oppassen met wat ik zeg.. ik ga natuurlijk niet
 alle 256 mogelijke verplaatsingen bij de indexregisters op-
 geven; dan wordt Smael z'n routine echt depressief...

  BIT 0,(HL)      CB 46      
  BIT 0,(IX+d)    DD CB 05 46
  BIT 0,(IY+d)    FD CB 05 46
  BIT 0,A         CB 47      
  BIT 0,B         CB 40      
  BIT 0,C         CB 41      
  BIT 0,D         CB 42      
  BIT 0,E         CB 43      
  BIT 0,H         CB 44      
  BIT 0,L         CB 45      

  BIT 1,(HL)      CB 4E      
  BIT 1,(IX+d)    DD CB 05 4E
  BIT 1,(IY+d)    FD CB 05 4E
  BIT 1,A         CB 4F      
  BIT 1,B         CB 48      
  BIT 1,C         CB 49      
  BIT 1,D         CB 4A      
  BIT 1,E         CB 4B      
  BIT 1,H         CB 4C      
  BIT 1,L         CB 4D      

  BIT 2,(HL)      CB 56      
  BIT 2,(IX+d)    DD CB 05 56
  BIT 2,(IY+d)    FD CB 05 56
  BIT 2,A         CB 57      
  BIT 2,B         CB 50      
  BIT 2,C         CB 51      
  BIT 2,D         CB 52      
  BIT 2,E         CB 53      
  BIT 2,H         CB 54      
  BIT 2,L         CB 55      

  BIT 3,(HL)      CB 5E      
  BIT 3,(IX+d)    DD CB 05 5E
  BIT 3,(IY+d)    FD CB 05 5E
  BIT 3,A         CB 5F      
  BIT 3,B         CB 58      
  BIT 3,C         CB 59      
  BIT 3,D         CB 5A      
  BIT 3,E         CB 5B      
  BIT 3,H         CB 5C      
  BIT 3,L         CB 5D      

  BIT 4,(HL)      CB 66      
  BIT 4,(IX+d)    DD CB 05 66
  BIT 4,(IY+d)    FD CB 05 66
  BIT 4,A         CB 67      
  BIT 4,B         CB 60      
  BIT 4,C         CB 61      
  BIT 4,D         CB 62      
  BIT 4,E         CB 63      
  BIT 4,H         CB 64      
  BIT 4,L         CB 65      

  BIT 5,(HL)      CB 6E      
  BIT 5,(IX+d)    DD CB 05 6E
  BIT 5,(IY+d)    FD CB 05 6E
  BIT 5,A         CB 6F      
  BIT 5,B         CB 68      
  BIT 5,C         CB 69      
  BIT 5,D         CB 6A      
  BIT 5,E         CB 6B      
  BIT 5,H         CB 6C      
  BIT 5,L         CB 6D      

  BIT 6,(HL)      CB 76      
  BIT 6,(IX+d)    DD CB 05 76
  BIT 6,(IY+d)    FD CB 05 76
  BIT 6,A         CB 77      
  BIT 6,B         CB 70      
  BIT 6,C         CB 71      
  BIT 6,D         CB 72      
  BIT 6,E         CB 73      
  BIT 6,H         CB 74      
  BIT 6,L         CB 75      

  BIT 7,(HL)      CB 7E      
  BIT 7,(IX+d)    DD CB 05 7E
  BIT 7,(IY+d)    FD CB 05 7E
  BIT 7,A         CB 7F      
  BIT 7,B         CB 78      
  BIT 7,C         CB 79      
  BIT 7,D         CB 7A      
  BIT 7,E         CB 7B      
  BIT 7,H         CB 7C      
  BIT 7,L         CB 7D      


 Nou, nou!! Dat dat een flink stuk typen was  mag wel duide-
 lijk zijn. Och mensen wat een klus.  Maar goed, dat was het
 waar, want dergelijke  tabelletjes kunnen wel degelijk heel
 handig zijn. Gelukkig maar dat een print optie in de FD zit
 want overschijven zou een flinke klus zijn... Tenzij je na-
 tuurlijk Patrick heet en flink getrainde armen hebt...

 Maar goed, dit besluit dus de bit-cursus.  De volgende keer
 gaan we vast wel verder met iets, maar waar over??


Tobias Keizer
