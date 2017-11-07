                         I N D E X   R E G I S T E R S 
                                                        
          
          De  Z80 is uitgerust met twee indexregisters, IX en IY. Deze 
          registers worden  door beginnende  ML programmeurs vaak niet 
          of  op de  verkeerde manier  gebruikt. In dit artikel zal ik 
          uitleggen waarvoor  je de indexregisters goed kunt gebruiken 
          en hoe,  en waarvoor  niet. Eerst even een korte uitleg voor 
          degenen die nog helemaal niets van IX en IY weten.
          
          
                                 I   E N   X ? 
          
          De registers  IX en  IY zijn  beiden 16 bits, maar ze kunnen 
          (bij   de  Z80  althans)  niet  op  dezelfde  manier  worden 
          gesplitst als de andere 16 bits registers: BC, DE en HL. Het 
          is dus  onzin om  het hoge  deel van IX met I aan te duiden, 
          vooral  omdat het I register al bestaat en een totaal andere 
          functie  heeft.  Bij  de  R800 kunnen  IX en  IY wel  worden 
          gesplitst, maar  dat wordt  aangeduid met  IXL en IXH, resp. 
          IYL en IYH. IX betekent gewoon Indexregister X.
          
          
                G E I N D E X E E R D E   A D R E S S E R I N G 
          
          De  naam zegt  het al:  IX en IY kunnen worden gebruikt voor 
          ge�ndexeerde adressering.  Dit werkt  ongeveer hetzelfde als 
          indirecte  adressering, met het verschil dat er bij het door 
          het register gegeven adres nog een constante wordt opgeteld. 
          Dit klinkt  misschien erg  ingewikkeld, maar  met een simpel 
          voorbeeld is het al snel duidelijk hoe het werkt.
          
                  LD      IX,TABEL
                  LD      A,(IX+3)
          
          Eerst  wordt in  IX het beginadres van een tabel geladen, en 
          daarna wordt  de vierde (!) byte uit die tabel in A geladen. 
          Bijna alle instructies met HL en (HL) zijn er ook voor IX en 
          IY,  waarbij (HL)  dan wordt  vervangen door (IX+d). Dit was 
          overigens niet de slimste manier om dit te programmeren, het 
          volgende is niet alleen korter maar ook veel sneller:
          
                  LD      A,(TABEL+3)
          
          En zo  kom ik  bij het eigenlijke onderwerp van dit artikel: 
          wanneer  heeft het  zin om de indexregisters te gebruiken en 
          wanneer niet.
          
          
                                V A R I A B E L 
          
          Eigenlijk  is het  heel simpel:  het heeft  alleen zin om de 
          indexregisters  te  gebruiken als  de waarde  van de  index- 
          registers variabel is.
          
          Wat bedoel ik daarmee? Even terug naar het voorbeeld:
          
                  LD      IX,TABEL
                  LD      A,(IX+3)
          
          De  waarde van  IX is  hier niet  variabel, die  is namelijk 
          altijd  gelijk  aan  TABEL.  Daarom  kan  er  dus  beter een 
          absolute adressering worden gebruikt:
          
                  LD      A,(TABEL+3)
          
          Dit  is niet alleen korter en sneller, je gebruikt bovendien 
          minder registers.  Het heeft wel zin om de indexregisters te 
          gebruiken  als de  waarde van  de indexregisters variabel is 
          Dit komt bijvoorbeeld voor bij tabellen die uit entry's zijn 
          opgebouwd  die  langer  zijn  dan  1  byte. Het  is dan  erg 
          makkelijk om  IX of  IY telkens naar het begin van een entry 
          uit  de tabel  te laten  wijzen, want dan kunnen de gegevens 
          uit op  zeer eenvoudige  wijze uit die entry worden gehaald. 
          Ik zal proberen dit duidelijk te maken met een voorbeeld.
          
          
                        L E N G T E   V A N   E N T R Y 
          
          De lengte  van de  entry is maximaal 128. d is g��n unsigned 
          byte,  dus 255  is eigenlijk  -1. Als het indexregister naar 
          het begin  van de  entry wijst,  kunnen er  127 andere  byte 
          boven het begin en 128 bytes onder het begin.
          
          
                            V E C H T R O U T I N E 
          
          Als  ik  de  vechtroutine  voor  Pumpkin  Adventure  III  ga 
          schrijven,  dan komen  daar zeker indexregisters aan te pas. 
          Stel  ik  heb  de  volgende  tabel  met de  gegevens van  de 
          vijanden:
          
          ENDAT:  DB      100,20,25       ; vijand #0
                  DB      200,50,40       ; vijand #1
                  DB      150,30,35       ; vijand #2
          
          ENDAT  staat voor  ENemy DATa,  de getallen  zijn achtereen- 
          volgens power, strength en defense. In werkelijkheid zijn er 
          natuurlijk  veel  meer vijanden  en veel  meer gegevens  per 
          vijand.
          
          De  lengte van een entry in deze tabel is 3 bytes. Het begin 
          van de vechtroutine zou er dan als volgt uit kunnen zien:
          
          ENEMY:  LD      B,A
                  ADD     A,A
                  ADD     A,B             ; A=A*3
                  LD      C,A
                  LD      B,0
                  LD      IX,ENDAT
                  ADD     IX,BC
          
          Bij aanroep  van de  routine staat  in A  het nummer  van de 
          vijand.  We vermenigvuldigen dit eerst met de lengte van een 
          entry (3) en tellen dat vervolgens bij het beginadres van de 
          tabel op. In IX staat nu het beginadres van de entry dat bij 
          de juiste  vijand hoort.  Het is nu heel eenvoudig om power, 
          strength  en defense  uit te  lezen, want op (IX+0) staat de 
          power, op (IX+1) de strength en op (IX+2) de defense. Het is 
          hopelijk duidelijk  dat IX  hier een variabele waarde heeft, 
          die is immers afhankelijk van de gekozen vijand.
          
          Dit  is ook wel met HL te programmeren, en dat is soms zelfs 
          iets sneller, maar dat is een stuk minder makkelijk omdat je 
          dan met  INC en  DEC instructies  moet zorgen dat HL naar de 
          gewenste  positie  in  de entry  wijst. Als  je bijvoorbeeld 
          eerst  het gegeven  op (IX+3)  nodig hebt  en daarna  dat op 
          (IX+28), dan  is dat  met indexregisters een fluitje van een 
          cent, terwijl je met HL iets als
          
                  LD      BC,25
                  ADD     HL,BC
          
          zult  moeten gaan  gebruiken, waarbij  je dus ook nog het BC 
          (of DE)  register nodig hebt. Bovendien is het prettig om HL 
          vrij te hebben voor andere zaken.
          
          De indexregisters  kunnen dus zeer goed worden toegepast bij 
          het  werken  met  tabellen waarvan  de entry's  uit meerdere 
          bytes bestaan.
          
          
                               L D   I X , B C ? 
          
          Een LD  IX,BC instructie  bestaat niet,  en ook LD BC,IX, LD 
          DE,IY  etc. zijn  niet aanwezig. Gelukkig is daar een simpel 
          truukje voor: PUSHen en POPpen. Bijvoorbeeld:
          
                  PUSH    BC
                  POP     IX              ; LD IX,BC
          
                  PUSH    IY
                  POP     DE              ; LD DE,IY
          
          
                         S T R I N G S   M E T   U S R 
          
          Ik zie  vaak dat er een indexregister wordt gebruikt bij het 
          verwerken van een
          
          A$=USR(B$)
          
          aanroep  vanuit BASIC.  De BASIC  interpreter zet  in DE het 
          beginadres van  drie bytes  waar de  string wordt beschreven 
          door  de  lengte  op (DE)  en het  beginadres op  (DE+1), en 
          springt  vervolgens naar de ML routine. Je wilt nu de lengte 
          in B hebben en het beginadres van de string in DE. Dit wordt 
          vaak zo geprogrammeerd:
          
                  PUSH    DE              1       11
                  POP     IX              2       14
                  LD      B,(IX+0)        3       19
                  LD      E,(IX+1)        3       19
                  LD      D,(IX+2)        3       19
                                         ---     ----
                                         12       82
          
          Dit zijn 12 bytes en 82 T-states. Het kan echter korter:
          
                  EX      DE,HL           1       4
                  LD      B,(HL)          1       7
                  INC     HL              1       6
                  LD      E,(HL)          1       7
                  INC     HL              1       6
                  LD      D,(HL)          1       7
                                         ---     ---
                                          6      37
          
          Dit is  ��n regel  meer in assembly, maar het zijn slechts 6 
          bytes en 37 T-states. Dus twee keer zo kort en meer dan twee 
          keer  zo snel. Hoewel de waarde van IX hier wel variabel is, 
          is het  hier dus  toch beter om HL te gebruiken. HL had hier 
          toch  geen waarde die je nog nodig hebt, en bovendien hoeven 
          de gegevens  hier maar  ��n keer  en op  volgorde te  worden 
          gelezen.   Het  werken   met  indexregisters   heeft  vooral 
          voordelen als je de gegevens door elkaar nodig hebt.
          
                                                          Stefan Boer
