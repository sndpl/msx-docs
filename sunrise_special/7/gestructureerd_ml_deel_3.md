                   G E S T R U C T U R E E R D   M L   ( 3  )
                                                              
          
          Een kort  deel deze  keer, zoveel  valt er  nu ook weer niet 
          over  dit onderwerp  te vertellen  zonder op  details in  te 
          gaan, en daar hebben we al de rubriek ML technieken.
          
          Het is  een beetje  afhankelijk van  het project waar je mee 
          bezig   bent,  maar  zelf  geef  ik  duidelijk  programmeren 
          voorrang  boven  een  paar  bytes  minder  code of  een paar 
          klokpulsen  minder.  Wat  ik  bedoel is:  als ik  kan kiezen 
          tussen  een   duidelijke  oplossing   van  10  bytes  en  30 
          klokpulsen en een minder duidelijke oplossing van 8 bytes en 
          25  klokpulsen,  dan  zal  ik  toch de  duidelijke oplossing 
          kiezen. Als ik om snelheids- of ruimteredenen toch de minder 
          duidelijke  oplossing moet  kiezen, zal  ik extra commentaar 
          erbij zetten.
          
          Even  een  paar voorbeelden.  Als er  aan het  eind van  een 
          routine het volgende staat:
          
                          CALL    AndereRoutine
                          RET
          
          Scheelt het 1 byte en 18 klokpulsen om dit te vervangen door
          
                          JP      AndereRoutine
          
          Dit  is echter  niet het meest duidelijk, want het einde van 
          een routine is RET en niet JP. Nog een voorbeeld:
          
                          CP      #FF
                          JR      Z,Label
          
          Hier  kan de  CP #FF  worden vervangen  door een  INC A. Dat 
          scheelt  1   byte  en   3  klokpulsen.   Maar  het  is  veel 
          onduidelijker,  we willen  immers kijken  of A gelijk is aan 
          #FF en  we willen  A helemaal  niet verhogen. (De CP #FF kan 
          dan  ook niet  door INC  A worden vervangen als je de waarde 
          van A daarna nog nodig hebt.) Als je dit toch gebruikt omdat 
          je zeer krap zit qua tijd en/of geheugen, doe het dan zo:
          
                          INC     A       ; CP #FF
                          JR      Z,Label
          
          Zo  kun je in ieder geval nog aan het commentaar zien wat de 
          bedoeling was. Zelf gebruik ik wel AND A in plaats van CP 0, 
          maar dat  is iets  anders omdat  AND A  alleen die betekenis 
          heeft  (tenminste voor  mij), terwijl je INC A ook heel vaak 
          gebruikt om  gewoon A  te verhogen.  De carry  wissen doe ik 
          trouwens met OR A. Dat kan ook met SCF; CCF maar dat is veel 
          langzamer  en  neemt  meer bytes  in beslag.  AND A  zou ook 
          kunnen,  maar ik  heb met  mezelf afgesproken  dat ik  AND A 
          gebruik voor  CP 0  en OR A om de carry te wissen. Zo kan ik 
          ��n oogopslag zien wat een instructie betekent.
          
          
                               J R   O F   J P ? 
          
          JP  is sneller  dan JR  maar neemt  ��n byte meer in beslag. 
          Bovendien  is  een voorwaardelijke  JR sneller  als er  niet 
          wordt gesprongen. Wanneer gebruik je JR en wanneer JP?
          
          Ik  gebruik  zoveel  mogelijk  JR voor  sprongen binnen  ��n 
          routine. Zo  kun je  meteen zien  dat het  een sprong binnen 
          dezelfde   routine   is.   Soms  kan   dat  niet   omdat  de 
          sprongafstand te groot is, maar dat is vaak een teken dat de 
          routine   te  lang   is  en  in  deelproblemen  moet  worden 
          opgesplitst.  Dit  is  echter  zeker geen  wet van  meden en 
          perzen. JP  is bij  mij gereserveerd  voor sprongen naar een 
          andere  routine, al  moet je dat alleen in speciale gevallen 
          doen. Want  voor dat  je het weet heb je een onleesbaar stuk 
          spaghetti.   Verder  ondersteunt   JP  meer  voorwaardelijke 
          sprongen (P,  M, PE,  PO) dan  JR, maar  die gebruik je vrij 
          weinig.
          
          Voor  degenen die  in tijd- of ruimtenood zitten nog even de 
          gegevens van JP en JR:
          
                          klokcycli:      bytes:
          
          JP              11              3
          JR              13 (8)          2
          
          De tijd  tussen haakjes  bij JR geeft aan hoe lang het duurt 
          als er niet wordt gesprongen bij een voorwaardelijke sprong. 
          Het  is overigens  logisch dat JR langer duurt dan JP, omdat 
          bij JP  de Program  Counter gewoon met het adres geladen kan 
          worden,  terwijl bij  JR iets  bij de  Program Counter  moet 
          worden opgeteld.
          
          
                              N O G   Z O I E T S 
          
          Stel je  wilt wachten tot bit 0 van I/O poort #65 laag wordt 
          (wachten op CE bij V9990). Dan programmeer ik dat zo:
          
          WaitCE:         IN      A,(#65)
                          BIT     0,A
                          JR      NZ,WaitCE
          
          Zo  kun je  precies zien wat er gebeurt. Ik geef toe dat het 
          sneller is  om onderstaande  variant te gebruiken, maar hier 
          is het minder duidelijk:
          
          WaitCE:         IN      A,(#65)
                          RRCA
                          JR      C,WaitCE
          
          Bit 0 wordt namelijk in de carry geschoven door RRCA. Als je 
          dat er  in commentaar  achter zet  is het natuurlijk al weer 
          een stuk duidelijker. Het nadeel van deze methode is dat het 
          alleen  bij bit 0 werkt, bij een ander bit moet je toch weer 
          op BIT n,x terugvallen.
          
          
                    S N E L H E I D   O F   N E T H E I D ? 
          
          Nogmaals,  het  hangt  af  van het  soort programma  hoeveel 
          aandacht je  aan de  netheid KUNT  besteden. Want als je het 
          onderste uit de kast wilt halen dan kan dat alleen maar door 
          een  tabel met  klokcycli bij de hand te houden en overal de 
          snelste oplossing  voor te kiezen. Maar bij veel programma's 
          is dat onzin en dan is het belangrijker om de netheid in het 
          hoog  te houden, zodat je eigen sources straks tenminste nog 
          kunt  begrijpen.  Gebruik  in  ieder  geval  commentaar  bij 
          truukjes die je niet al veel vaker hebt gebruikt.
          
                                                          Stefan Boer
