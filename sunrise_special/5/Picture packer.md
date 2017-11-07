                    P I C T U R E   P A C K E R 
                                                       
          
          Vorige keer was er iets behoorlijk mis gegaan met de picture 
          packer.  Op SRS#4  stond namelijk  een verouderde versie van 
          het programma, zodat sommige dingen die ik in de handleiding 
          had vermeld  niet werkten,  omdat ze  niet in  het programma 
          waren opgenomen!
          
          Op  deze disk  vindt u - hoop ik - de goede en iets snellere 
          versie van  het programma.  Dat betekent wel, dat uw plaatje 
          gepackt  met de oude versie niet meer kan worden bekeken met 
          de nieuwe.  Maar ik  vond dat  de voordelen  van deze nieuwe 
          versie groot genoeg waren.
          
          
                            D E   B E D I E N I N G 
          
          Deze  tekst  stond  ook  op  SRS#3, maar  goed. Eerst  dient 
          KUN-BASIC  ingeladen te  worden. (Via  DOS of met BLOAD, dat 
          ligt aan  uw versie.  Mensen met KUN-BASIC ingebouwd, tikken 
          invoudigweg  CALL BC.) [Nvdr. Of gewoon _BC of helemaal niks 
          voor mensen die KUN-BASIC in ROM hebben zitten.]
          
          Om een plaatje te packen start u vervolgens PICPACKR.BAS op. 
          Het programma  vraagt om  de te  packen filenaam. Als uit de 
          extensie  al duidelijk  is, in  welk schermtype  het plaatje 
          staat, (zoals bij .SC8 of .S12) stelt het programma dit zelf 
          in en  bewaart dit  later ook  in de  PCD file,  zodat u het 
          plaatje altijd in het juiste schermtype kunt bekijken.
          
          Indien het niet te achterhalen is, vraagt het programma u in 
          welk  schermtype de  plaat behoort  te staan. Als het packen 
          gelukt is  kan het  programma vragen,  of het plaatje "high" 
          geladen  moet worden. Later meer hierover. Daarna vraagt het 
          programma of het oude plaatje verwijderd moet worden. Het is 
          met de  nieuwe versie  WEL mogelijk  om plaatjes weer in hun 
          oorspronkelijk  formaat terug  te zetten!  (Zie info over de 
          depacker.)
          
          
                 B E K I J K E N   E N   T E R U G Z E T T E N 
          
          Om  plaatjes  te bekijken  of weer  terug te  zetten in  hun 
          normale vorm  laadt men  PICDPACKR.BAS. Na het starten geeft 
          het  programma een  overzicht van  alle PCD's  in de huidige 
          directory. U  hoeft nu alleen de filenaam zonder de extensie 
          PCD in te typen.
          
          Als u  een piep  hoort, betekent dat, dat het plaatje "hoog" 
          in  het schermgeheugen  staat. Het plaatje wordt nu getoond. 
          Typt u S als hij klaar is, dan wordt het plaatje gesaved met 
          dezelfde filenaam,  maar met  de extensie  .SC7 .SC8 of .S12 
          voor  respectievelijk SCREEN 7, 8 en 12 plaatjes. U kunt het 
          plaatje nu  weer gewoon  met BLOAD ,S inladen. Typt u P, dan 
          savet  (wat  een  woord) het  programma het  plaatje met  de 
          extensie .PIC zodat u het met Designer Plus [Nvdr. en andere 
          rotte programma's] gelijk kunt inladen en bewerken.
          
          Nog  even wat "technische" informatie. Om plaatjes te packen 
          moeten ze  in een  vorm staan  zoals ze  met BSAVE ,S worden 
          bewaard. De indeling van de eerste bytes van een PCD zien er 
          als volgt uit:
          
          00H  altijd waarde 224 (=alpha)
          01H  geeft aan of plaatje hoog of laag in het geheugen staat
               108 (="L") is laag en 104 (="H") is hoog.
          02H  deze byte bevat het schermnummer waarin het plaatje
               staat.
          03H
           ..  zijn ongebruikt. Hier kan nog aanvullende info komen.
           ..
          09H
          0Ah  het begin van het plaatje
          
          Opmerking:  er staat  nergens hoelang de PCD is. De depacker 
          kijkt gewoon, of het scherm (0000H-D3FFH) vol is.
          
          Als het  plaatje hoog  in het  VRAM wordt  geladen wordt bij 
          deze indeling gwoon D400H opgeteld. D401H bevat dan 104. Het 
          hoog  laden heeft  als nut,  dat bij  het depacken slechts 1 
          page wordt gebruikt en ook nog iets sneller is.
          
          Het lijkt  me niet  alt te moeilijk, om van deze programma's 
          ook  een machinetaal versie van te maken, zodat het nog veel 
          sneller werkt. Wie?
          
          
                           V O L G E N D E   K E E R 
          
          Op die  vectorgraphics maker zult u nog even moeten wachten. 
          Ik  heb na drie maanden wachten EINDELIJK het boek te pakken 
          gekregen dat mij misschien verteld hoe ik onzichtbare lijnen 
          ook onzichtbaar moet laten worden. (Wat een smoes...)
          
          Volgende  keer  een  spraaksynthese  programma  dat,  jawel, 
          NEDERLANDS  praat!  Wel  met  een  Frans  accent (ja  accent 
          spreekt-ie ook  goed uit)  maar het  was oorspronkelijk  een 
          Frans  programma. Alleen  nog even maken dat hij ook woorden 
          als "mooie" goed uitspeekt... (mooieju)
          
          Groeten!
                                                         Randy Simons
      