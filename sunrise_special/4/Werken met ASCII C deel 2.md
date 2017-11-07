                  H E T   W E R K E N   M E T   A S C I I   C 
                                                               
          
          
                                  D E E L   2 
          
                         C   M E T   A S S E M B L E R 
          
          Hoewel ASCII  C behoorlijk  goede machinecode  genereert, is 
          het  soms toch  handig om  bepaalde routines in assembler te 
          schrijven. Zo is het bijvoorbeeld niet mogelijk om direct de 
          BIOS aan  te roepen,  hier is  toch weer een library functie 
          voor  nodig. Ook  VDP I/O  kan het beste in assembler worden 
          gedaan, omdat dit toch een stuk sneller gaat.
          
          Gelukkig is  dit met  ASCII C  geen enkel  probleem, het  is 
          namelijk  mogelijk om  een library in assembler te schrijven 
          en  de  functies die  in deze  library staan,  vervolgens te 
          gebruiken vanuit een C programma.
          
          Als de  library goed  wordt opgezet is het zelfs mogelijk om 
          uit  1  enkele  source  file  zowel  de  .TCO  file  voor de 
          parameter checker te maken, als de .REL file voor de linker.
          
          In  dit  deel  van  de  cursus  zullen  achtereenvolgens  de 
          volgende onderwerpen worden besproken:
          
          - Het doorgeven van variabelen aan de functies.
          - Het opzetten van de source zodat er zowel een .TCO als 
            een .REL file van gemaakt kan worden.
          - Een voorbeeld library.
          
          
                    V A R I A B E L E N   D O O R G E V E N 
          
          ASCII-C kent 2 typen functies:
          
          - Fixed parameter functies: de functie heeft een vast aantal 
            parameters.
          - Variabele parameter functies: de functie heeft een 
            variabel aantal parameters.
          
          Normaal gaat  de C-compiler  er uit  dat een functie van het 
          fixed  parameter type  is. Een  variabele parameter  functie 
          moet eerst als volgt gedeclareerd worden:
          
                          int func(.);
          
          Bij  een   fixed  parameter   functie  worden  de  eerste  3 
          variabelen  in de  Z80 registers  doorgegeven, de rest wordt 
          allemaal  op  de  stack  gezet.  Hierbij  wordt de  volgende 
          conventie gebruikt:
          
          parameter  | char   | niet-char 
          -----------+--------+--------------- 
          1e         | A      | HL 
          2e         | E      | DE 
          3e         | C      | BC 
          4e         | (SP+2) | (SP+2, SP+3) 
          5e         | (SP+4) | (SP+4, SP+5) 
          etc.
          
          Bij  een   variabele  parameter  functie  wordt  het  aantal 
          variabelen  in  HL  gezet,  en  de  rest  wordt op  de stack 
          gepushed.  De volgorde is hierbij weer hetzelfde als bij een 
          fixed parameterfunctie,  dus de  variabelen worden als volgt 
          doorgegeven:
          
          parameter  | char   | niet-char
          -----------+--------+---------------
          aantal vars| HL     | HL
          1e         | (SP+2) | (SP+2, SP+3)
          2e         | (SP+4) | (SP+4, SP+5) etc.
          
          Als  de functie  een bepaalde waarde berekent, dan moet deze 
          ook weer worden teruggegeven aan de aanroepende functie, dit 
          gaat altijd  via de registers. Als het functie type char is, 
          dan  wordt de  waarde teruggegeven  in de A en in het andere 
          geval komt ze terug in HL.
          
          Een functie mag in principe alle registers veranderen.
          
          Voorbeeld:
          
          Stel je  wilt een functie schrijven die aan karakter afdrukt 
          via  de MSX1  main rom  (CHPUT: adres 00A2H), en een functie 
          die een  karakter van  het toetsenbord  leest (CHGET:  adres 
          009FH). Je zult deze functies dan moeten declareren in het C 
          programma  dat er gebruik van gaat maken. Om dit te bereiken 
          kun je  het beste  alle declaraties  die nodig  zijn voor je 
          library  bij  elkaar  zetten  in  1 header  file die  je dan 
          include  vanuit je C programma's. In deze header file moeten 
          dan de  volgende declaraties  staan:
          
          void bchput(); /* zet karakter via bios op scherm */
          char bchget(); /* lees karakter uit via bios */
          
          In  de  assembly  source  (mlvb1.mac)  zet je  vervolgens de 
          volgende routines:
          
          bchput::        ld      ix,0a2h         ;Druk karakter in A
                          jr      domsx1          ; af via de BIOS
          
          bchget::        ld      ix,09fh         ;Lees een karakter
          domsx1:         ld      iy,(0fcc0h)     ; en zet dat in A
                          jp      01ch
          
                          end
          
          
          Deze source kun je assembleren met: 
          
                          M80 = MLVB1 /Z
          
          om het bestand MLVB1.REL te maken.
          
          In  een  C  programma  (CVB1.C)  kun  je  ze  dan  als volgt 
          gebruiken:
          
          typedef char void;
          
          void bchput(); 
          char bchget();
          
          void main() 
          { 
            char test;
          
            do { 
              test = bchget();  /* Lees karakter uit via bios */ 
              bchput(test); 
            } while (test != 13); 
          }
          
          Deze  listing kun  je vervolgens  compileren, assembleren en 
          linken met:
          
                          CF CVB1
                          CG -K CVB1
                          M80 = CVB1 /Z
                          L80 CVB1,MLVB1,CVB1/N/Y/E:MAIN@
          
          Als je  dit allemaal  hebt gedaan, dan heb je nu een (klein) 
          programma met de naam CVB1.COM, waarmee je kunt typen tot je 
          op de return toets duwt.
          
          
              Z O W E L   . T C O   A L S   . R E L   V E R S I E 
          
                          V A N   E E N   S O U R C E 
          
          Zoals  je in bovenstaand voorbeeld wel kunt zien is het niet 
          zoveel  werk  om C  te combineren  met assembler.  Het wordt 
          echter  iets  meer  werk  als  je  de  library  netter  wilt 
          opzetten.
          
          In  het  bovenstaande  voorbeeld  kun  je bijvoorbeeld  geen 
          gebruik  maken van  de functie parameter checker (FPC) om te 
          kijken of het type van de variabelen wel klopt.
          
          Ook  de linker  wordt nu niet optimaal benut. Met L80 is het 
          namelijk mogelijk  om een  library zo  samen te  stellen dat 
          alleen functies die gebruikt worden, ook worden meegelinkt.
          
          Om  dit te bereiken moet de source echter worden opgesplitst 
          in  allemaal  losse sources  (��n source  per onafhankelijke 
          functie).  Deze   sources  moeten  vervolgens  allemaal  los 
          geassembleerd  worden (zodat  er allemaal  losse .REL  files 
          ontstaan),  en  deze  .REL  files kunnen  dan met  LIB80 aan 
          elkaar worden gekoppeld tot 1 grote library.
          
          Om  nu  te  voorkomen  dat  je  disk vol  komt te  staan met 
          honderden losse library routines heeft ASCII corporation het 
          hulp programma  MX gemaakt.  Dit programma  splitst 1  grote 
          source  op in losse deel sources, maakt een batchfile aan om 
          deze sources  afzonderlijk te assembleren en weer te deleten 
          en om vervolgens de .REL files te linken tot 1 .LIB file.
          
          Om  goed gebruik  te maken  van MX moet de source echter wel 
          goed zijn  opgezet. Hierbij  wordt gebruik  gemaakt van  het 
          feit  dat M80 een macro assembler is. In je source dien je 3 
          (verder lege) macro's te defini�ren:
          
          module          macro
                          endm
          
          endmodule       macro
                          endm
          
          extrn           macro
                          endm
          
          Alles  wat   in  principe  echt  bij  elkaar  hoort  kun  je 
          vervolgens  tussen  MODULE  FILENAME  en  ENDMODULE  zetten. 
          Indien  je vanuit dit stuk machinecode ook nog routines wilt 
          aanroepen die  in een  andere module staan, dan dien je deze 
          routines te declareren met EXTRN ROUTINE.
          
          MX zal dan alles wat tussen MODULE en ENDMODULE staat in het 
          bestand  met de  naam FILENAME zetten. In dit bestand worden 
          dan  de  EXTRN  ROUTINE  declaraties omgezet  in de  normale 
          EXTERN ROUTINE declaraties die nodig zijn voor M80.
          
          Het is wel belangrijk dat je de juiste volgorde aanhoudt als 
          bepaalde routines gebruik maken van andere routines:
          
          Als een  routine uit  MODULE A gebruik maakt van een routine 
          uit  MODULE B, dan dient MODULE B VOOR MODULE A in de source 
          te staan!!
          
          Als we bij het bovenstaande voorbeeld blijven, dan kunnen we 
          de volgende source maken (met de naam MLVB2.MAC):
          
          module          macro
                          endm
          
          endmodule       macro
                          endm
          
          extrn           macro
                          endm
          
                          MODULE  DOMSX1
          domsx1::        ld      iy,(0fcc0h)
                          jp      01ch
                          ENDMODULE
          
                          MODULE  BCHPUT
                          extrn   domsx1       ;Deze routine is nodig
          bchput::        ld      ix,0a2h      ;Zet karakter op scherm
                          jp      domsx1
                          ENDMODULE
          
                          MODULE  BCHGET
                          extrn   domsx1
          bchget::        ld      ix,09fh      ;Lees karakter uit
                          jp      domsx1
                          ENDMODULE
          
                          end
          
          
          Deze nieuwe  source kun je nog altijd direct assembleren met 
          M80.  Wil je  er echter een library van maken waaruit alleen 
          de functies  worden meegelinkt  die aangeroepen  worden, dan 
          dien je de volgende stappen te ondernemen:
          
          - Zorg ervoor dat AREL.BAT op je disk staat
          - Zorg ervoor dat MX.COM, M80.COM en LIB80.COM op je disk 
            staan.
          - Typ in:
            MX -L MLVB2 > TEMP.BAT TEMP REN MLVB2.LIB MLVB2.REL
          
          Deze nieuwe library kun je vervolgens meelinken met:
          
                       L80 CVB1,MLVB2/S,CVB1/N/Y/E:MAIN@
          
          De /s  achter mlvb2 betekent dat L80 alleen die modules moet 
          meelinken waar routines in zitten die worden aangeroepen.
          
          In het  eerste C  voorbeeld worden  zowel bchput  als bchget 
          gebruikt.  Ze  zullen  in dit  geval dan  ook beiden  worden 
          meegelinkt  en er  is nu  dus geen echt voordeel behaald. In 
          het volgende  voorbeeld is  het gebruik  van de  (lastigere) 
          methode via MX wel al voordeliger (CVB2.C):
          
          typedef char void; 
          void bchput();
          
          void bputstr(string) 
          char *string; 
          { 
            while (*string) 
              bchput(*string++); 
          }
          
          void main() 
          { 
            bputstr("Dit is C voorbeeld 2"); 
          }
          
          Deze  listing kun  je vervolgens  compileren, assembleren en 
          linken met:
                          CF CVB2
                          CG -K CVB2
                          M80 = CVB2 /Z
                          L80 CVB2,MLVB2/S,CVB2/N/Y/E:MAIN@
          
          CVB2.COM zal nu 58 bytes groot zijn. Indien je echter met de 
          oude     library      had     gelinked      (middels     l80 
          cvb2,mlvb1,cvb2/n/y/e:main@),  dan  was  CVB2.COM echter  61 
          bytes  groot geweest  omdat dan ook de overbodige bchget was 
          meegelinked.
          
          Om  ook nog gebruik te kunnen maken van de functie parameter 
          checker  (fpc),  dient  de  source  nog  iets uitgebreid  te 
          worden. Om  FPC te  gebruiken, moeten namelijk lege C body's 
          worden  opgenomen zodat  FPC weet  hoeveel en  wat voor type 
          parameters  gebruikt  worden door  de functie.  Aangezien de 
          source hiervoor  echter door  CF.COM en door M80.COM gehaald 
          moet kunnen worden, is er een klein probleem (de programma's 
          verwachten namelijk een compleet verschillende syntax). Door 
          nu echter van een truuk gebruik te maken, kunnen evengoed de 
          lege  C routine body's en de echte ML routines in ��n enkele 
          source worden  gebruikt. De  truuk bestaat  eruit dat ; voor 
          M80  betekent dat  er commentaar begint (en dat alles wat er 
          achter staat  dus niet  van belang  is), terwijl het voor CF 
          een scheidings teken tussen te statements is, dus CF negeert 
          een losse ;.
          
          De source  kan dan  beginnen met "; /*". Alles wat daarna op 
          dezelfde  regel staat,  is dan  zowel voor  M80 als  voor CF 
          commentaar. Door  hierna goed  af te wisselenen met /* en */ 
          (voor  CF) en  .comment #  en # voor M80, is het mogelijk om 
          alles in 1 source te houden.
          
          
                   E E N   V O O R B E E L D   L I B R A R Y 
          
          De  library XS-CLIB1.MAC  is de  volledig ontwikkelde versie 
          van  de  eerste  voorbeelden.  Er  zitten  ook nog  een paar 
          functies bij om de VDP aan te sturen. De volledig afgewerkte 
          library  staat  tevens  op  de  disk  (XS-CLIB1.REL). Om  er 
          gebruik  van  te  maken,  dient de  parameter XS-CLIB1/S  te 
          worden  opgenomen  achter  L80  in in  de batchfile  waar je 
          normaal  je  C  programma's  mee  linkt.  Om de  functies te 
          gebruiken, dien  je ook  nog de header file XS-CLIB1.H op te 
          nemen in je source.
          
          Met  CF.COM kan  uit deze  source nu  de .TCO  file voor  de 
          function parameter  checker worden  gehaald. De library zelf 
          kan   worden  aangemaakt   met  MX.COM.   Het  aanmaken  van 
          XS-CLIB1.TCO en XS-CLIB1.REL gaat nu als volgt:
          
          - Zorg ervoor dat CF.COM, MX.COM, M80.COM, LIB80.COM en 
            AREL.BAT op de disk staan.
          - Typ in: cf xs-clib1.mac om de .TCO file te maken.
          - En typ vervolgens in:
                  MX -L XS-CLIB1 > TEMP.BAT TEMP
                  REN XS-LIB1.LIB XS-LIB1.REL
            om XS-CLIB1.REL te maken.
          
          Het  bestand XS-VB.C  is een  voorbeeld van  het gebruik van 
          deze library. Dit bestand is te compileren met MK-XSVB.BAT.
          
                                                           Alex Wulms
