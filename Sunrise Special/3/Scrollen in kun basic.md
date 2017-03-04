        S C R O L L I N G   I N   K U N - B A S I C 
                                                               
          
          Zo, nu gaan we weer eens terug naar het begin: BASIC! Ik heb 
          namelijk  een horizontale  scroll gemaakt,  geheel an  al in 
          BASIC geschreven!  Geen bitje  machinetaal! (Nou ja, die ene 
          sprong naar adres &H69 niet meegerekend, dat is om eventueel 
          aanwezige sprites te wissen. Die routine staat gewoon in het 
          BIOS.)  Het enige  wat je  nodig hebt  is KUN-BASIC, oftewel 
          XBASIC.
          
          Kijk  maar eens in de listing hoe ik het precies gedaan heb. 
          De  truuk  is:  VDP(19)  en  het  gebruik maken  van integer 
          variabelen.
          
          
                         V D P   R E G I S T E R   1 8 
          
          Zo!  Hier ben  ik nog  eens met wat aanvullende info over de 
          super-scroll in  BASIC. Zoals  eerder vermeld heb ik gebruik 
          gemaakt van VDP register 18 (VDP(19)) Dit register wordt ook 
          gebruikt  met  het  commando SET  ADJUST (X,Y).  Het is  ook 
          mogelijk om met dit register verticaal te scrollen, maar dat 
          zou  niet slim  zijn, want  met register  18 kun je dan over 
          slechts 16 pixels scrollen. vdp register 23 (VDP(23)) is dan 
          een beter  alternatief, omdat  dat register, nou ja, laat ik 
          maar  zeggen, speciaal  gemaakt is om verticaal te scrollen: 
          alle  8   bits  worden   gebruikt  voor  die  ene  richting: 
          verticaal.  Je kunt  zo dus heel eenvoudig en snel verticaal 
          scrollen, over  255 beeldlijnen  (ja! Onder  het scherm  zit 
          namelijk  ook nog het gedeelte waar de sprites, kleurwaardes 
          e.d.  opgeslagen  zijn,  maar  die  zie  je  normaal  niet). 
          Voorbeeld in de listing ALPHA.BAS in de laatste regels.
          
          Nu  even terug  naar register 20. Zoals gezegd worden 4 bits 
          voor   de   verticale  en   4  bits   voor  de   horizontale 
          positionering gebruikt.  Scrollen bestaat dus eigenlijk uit: 
          1  keer per interrupt 1 pixel "opschuiven" en in de tijd dat 
          je  zo   16  pixels  laat  opschuiven,  ÇÇn  keer  het  hele 
          beeldscherm m.b.v. het COPY-commando kopieert. Et voilÖ! Een 
          mooie  horizontale scroll!
          
          
                             P R O B L E E M P J E 
          
          Nou  is  er  nog  een  probleempje  met  de  ietwat  vreemde 
          programmering van het register: als je netjes 16 pixels wilt 
          scrollen, moet  je eerst  de waardes  8 t/m  15 invoeren, en 
          daarna pas 0 t/m 7, anders klopt er niet zoveel van.
          
          Zo,  dat was  het wel, ik hoop dat BASIC-programmeurs en ook 
          wel ML-programmeurs  in spÇ hiermee wel vooruit kunnen. Voor 
          voorbeelden kijk je maar in de listing.
          
          Och,  voor  ik  het  vergeet:  ik  gebruikte  wel  het woord 
          interrupt, maar  daarmee bedoel  ik de tijd die de VDP nodig 
          heeft  om een  beeld op  te bouwen, normaal dus 1/50 seconde 
          (of 1/60  bij Japanse  computers). Om  mooi die  tijd aan te 
          houden,  gebruik ik  de TIME  variabele, deze wordt namelijk 
          door de  VDP met  een opgehoogd, als de VDP klaar is met het 
          opbouwen van een beeld, dus elke 1/50 seconde.
          
          Volgende  keer  heb  ik  misschien  een  mooi programma  (in 
          BASIC!)  om  zelf  vector-graphics  te maken.  Een programma 
          waarmee je in principe elk willekeurig 3-dimensionaal object 
          kunt invoeren,  en dan  in elke richting kunt laten draaien, 
          verplaatsen,  vergroten e.d.  Lijnen die  dan (gedeeltelijk) 
          buiten het  beeld vallen  worden geclipt: ze worden dan toch 
          goed getekend!!
          
                                                         Randy Simons
          