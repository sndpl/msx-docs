                    I N T E R F A C I N G   ( R S 2 3 2 C  )
                                                             
          
          
                   W A A R O M   E E N   S T A N D A A R D ? 
          
          Om   goede  interfacing   te  kunnen  doen,  dwz.  data  uit 
          wissellen, is het noodzakelijk dat de verschillende systemen 
          die dat  moeten kunnen  doen compatible  zijn. Hiervoor zijn 
          afspraken nodig.
          
          Deze afspraken houden het volgende in :
          - Hoe moet de verbinding gerealiseerd worden?
          - Wat betekenen deze verbindingen?
          - Wat zijn de defenities van de signalen die deze
            verbindingen voeren?
          
          
                 W I E   M A A K T   D E   A F S P R A K E N ? 
          
          De  afspraken die we nu gebruiken met de RS232C gedefinieerd 
          door EIA  (Electronic Industries  Association) zijn afgeleid 
          van  de V.24  en V.28  aanbeveling van  het CCITT (Committee 
          Consultative    International     de    Telegraphique     et 
          Telephonique),  een organisatie  die standariserings  normen 
          vaststeld.   V.24   worden   de   functies  van   de  pennen 
          gedefinieerd, terwijl V.28 de spanningsnivo's definieren.
          
          
                            W A T   I S   V . 2 8 ? 
          
          Om  data  over een  draad te  kunnen verplaatsen,  hebben we 
          duidelijke afspraken  nodig over de nivo's. Zo hoeven we met 
          de computer maar een "0" of een "1" over de lijn te krijgen, 
          of in geval van controle signalen een "on"/"off".
          Nu  is de afspraak gemaakt dat met een negatieve spanning we 
          een "1"  hebben, of in het geval van een control signaal een 
          "off". De maximale spanningen die gebruikt mogen worden zijn 
          -25  en +25  Volt. Het  gebied tussen  -3 en +3 Volt is niet 
          gedefinieerd.
          Dus zenden we een "1"(off) over, dan moet de spanning tussen 
          -25 en -3 Volt liggen.
          De  reden   dat  het  gebied  tussen  -3  en  +3  volt  niet 
          gedefinieerd  is, ligt  aan het  feit dat  je hardware nodig 
          hebt die  de lijn  meet, om  die hardware  simpel te  kunnen 
          houden,  moeten we  een hogere  spanning sturen dan in feite 
          nodig is.
          Meestal wordt met -12 en +12 gestuurd.
          
          
                            W A T   I S   V . 2 4 ? 
          
          Deze  afspraken  houden  in  wat  de  functies  van  diverse 
          signalen zijn.
          
          Hieronder een  overzicht van de defenities van de pinnen bij 
          een  25 pin's  sub-D aansluiting.  (Die op  de meeste RS232C 
          interfaces zit)
          
          Type    Pin MNEMONIC  Uitleg                         In/Out
          
          GROUND  7   GND       Ground                              -
          
          DATA    2   TMD,TxD   TransMit Data                       O
                  3   RCD,RxD   ReCeive Data                        I
          
          CONTROL 4   RTS       Ready To Send                       O
          and     5   RFS,CTS   Clear To Send                       I
          STATUS  6   DSR       Data Set Ready                      I
                  20  DTR       Data Terminal Ready                 O
                  8   DCD       Data Carrier Detect                 I
                  23  DRS       Data Signal rate Selector           O
                  22  CIN       Calling Indicatior                  I
                  11  STF       Select Transmit Frequency           O
          
          CLOCK   24  TSET,TxC  Transmitter Signal Element Timing   O
                  15  TSET,TxC  Transmitter Signal Element Timing   I
                  17  RSET,RxC  Receiver Signal Element Timing      I
          
          BACK-   14  TMD,TxD   TransMit backward channel Data      O
          WARD    16  RCD,RxD   ReCeive backward channel Data       I
          CHANNEL 19  RTS       Ready To Send backward channel      O
                  13  RFS,CTS   Clear To Send backward channel      I
                  12  DCD       backward channel DCD                I
          
          TEST    21  RIL       Loopback test                       O
                  18  LL        Local Loopback                      O
                  25  TI        Test Indicator                      I
          
          
                         U I T L E G   S I G N A L E N 
          
                                    D A T A 
          
            Via  TMD  wordt de  data verzonden,  de data  moet serieel 
            verstuurd worden,  dit heeft  als voordeel  dat we  maar 1 
            lijn  nodig  hebben.  Deze  methode  heeft echter  wel als 
            nadeel dat de doorvoersnelheid beperkt is.
            Nog  een  probleem  met  seriele  interfacing  is  dat  de 
            ontvanger dit signaal weer netjes in mekaar kan zetten tot 
            de oorspronkelijke  data. Hiervoor  moeten we  een methode 
            verzinnen  die redelijk  snel kan  zijn, zonder al te veel 
            moeilijkheden.  De  RS232C communicatie  die we  gebruiken 
            heeft  een   redelijk  simpele   methode  om  de  data  te 
            versturen.  Er wordt  aangenomen dat  de ontvanger weet op 
            welke   snelheid   er   verzonden   wordt,  en   stelt  de 
            ontvangstklok in  op die  snelheid. De  zender wil  op een 
            gegeven moment iets overzenden. Om de ontvanger opmerkzaam 
            hierop  te maken (triggeren) wordt het signaal wat normaal 
            gesproken in rusttoestang hoog is, laag gemaakt. Dit wordt 
            gedaan  voor  1  gehele  periode van  de zend  frequentie. 
            Hierna wordt  op dezelfde  snelheid de  data overgestuurd, 
            ook  weer geinverteerd.  Afhankelijk van  het protocol kan 
            hier achter  die data nog een parity bit komen. Hierachter 
            worden  nog 1  of 2  stopbits geplaatst.  Meestal wordt er 
            voor 1 stopbit gekozen voor een iets hogere doorvoer.
            Deze methode van zenden is A-SYNCHROON.
            Via RCD wordt op dezelde methode data ontvangen.
          
          
                          C O N T R O L / S T A T U S 
          
            Ik  bespreek alleen  de meest toegepaste signalen, enkelen 
            die niet  gebruik worden  op de  MSX, en ik dus helaas ook 
            niet ken, bespreek ik niet.
          
            RTS is  ervoor om te zorgen dat je aan de zender door kunt 
            geven  dat  je  op  dat  moment geen  data meer  kunt/wilt 
            ontvangen.  De  zender  hoort  hier  dan  rekening  mee te 
            houden.
            Door RFS  af te scannen tijdens het zenden, weet de zender 
            dat  deze op  een gegeven  moment op  moet houden,  als de 
            ontvanger dit wil.
            Deze   RTS/RFS   worden  in   sommige  oude   communicatie 
            programma's niet gebruikt, dan wordt er Xon/Xoff handshake 
            gebruikt, wat  inhoud dat  de zender  reageert op bepaalde 
            karaktercodes door te starten of stoppen met zenden.
            Met  DSR geeft een device aan dat ie actief is, dus dat er 
            data naartoe verzonden mag worden.
            DTR is het signaal wat je afgeeft, om aan te duiden dat je 
            actief  bent,  verwar  dit  niet met  RTS, want  RTS werkt 
            alleen als DTR geldig is.
            Met DCD  heeft de tegenpartij aan dat ie herkent heeft dat 
            er  data komt,  een modem kan hier bv. mee aangeven dat er 
            een verbinding is met een ander modem.
            CIN is een signaal wat de tegenpartij, met deze toepassing 
            meestal een modem, aan dat er een oproep is.
            TSET  en RSET  zijn klokfrequenties  voor ontvangen/zenden 
            van data.
            Het complete  backward channel  werkt zoals de equivalente 
            functies hierboven.
          
          
                             D E   P R A K T I J K 
          
          In de  praktijk zijn meestal niet alle functies opgenomen in 
          de  interface. Zo  heb ik nog nooit een MSX RS232C interface 
          gezien met  de TEST  functies. Een  MSX RS232C interface met 
          een  werkende backward channel is ook ver te zoeken, maar de 
          Philips NMS121x  is er  wel op  voorbereid. Dit  vereist een 
          andere  kabel, met  een andere  aansluiting IN de interface. 
          Heeft wel  als nadeel  dat je  dan maar  1 aansluiting  kunt 
          gebruiken,  daar channel  B dan  ook op  de aansluiting  van 
          channel A  komt. Wat  ik ook  nog niet  gezien heb op de MSX 
          RS232C interfaces is de rate/frequency selector.
          
          Even  een korte  opsomming over "tekortkomingen" op twee MSX 
          RS232C interfaces.  Deze tekortkomingen  zijn in  feite niet 
          echt nodig,  en worden op andere systemen ook niet gebruikt, 
          bij mijn weten.
          
          
          De NMS121x interfaces :
          
          Niet ondersteund : DRS, STF, RIL, LL, TI
          Op voorbereid    : complete backward channel
          
                         
          De SONY HBI1 RS232C interface (MSX standaard) :
          
          Niet ondersteund : DRS, STF, RIL, LL, TI, TSET in, TSET out, 
                             RSET, complete backward channel
          
          Deze functies zullen ook niet op andere standaard MSX-RS232C 
          interfaces  opgenomen  zijn, simpelweg  omdat deze  functies 
          niet gedefinieerd zijn door ASCII.
          
                                                             Erik Maas
