                    N O G M A A L S   H E X   P R I N T E N 
                                                             
          
          Op  Sunrise  Special  #3 stond  een tekst  van mij  over het 
          printen  van een  binair, hexadecimaal  of decimaal getal in 
          ML. De  decimale routine  was goed  en de binaire zeer mooi, 
          maar  de hexadecimale was nogal eh... vreemd geprogrammeerd. 
          Dat komt  ervan als je na 2:00 's nachts nog teksten voor de 
          Special gaat schrijven!
          
          Om het goed te maken hierbij een fatsoenlijke routine om een 
          getal hexadecimaal af te drukken. Onderstaande source stuurt 
          de waarde in A hexadecimaal naar het scherm.
          
          PRTHEX: LD    C,A               ; bewaar A voor andere helft
                  AND   &HF0              ; bovenste nibble
                  RRCA
                  RRCA
                  RRCA
                  RRCA                    ; verschuif naar onderste
                  CALL  UITHEX            ; printen
                  LD    A,C               ; oude waarde weer terug
                  AND   &H0F              ; onderste nibble
                  CALL  UITHEX            ; printen
                  RET
          UITHEX: ADD   A,"0"
                  CP    "9"+1             ; is het een cijfer?
                  JP    C,CIJFER          ; ja, dan printen
                  ADD   A,"A"-"9"-1       ; nee, dan letter van maken
          CIJFER: CALL  &HA2              ; BIOS routine CHPUT
                  RET
          
          
                               S P A G H E T T I 
          
          Dit is een nette routine. Er zullen echter programmeurs zijn 
          die  zeggen dat  het sneller  kan. Dat  kan ook  wel, en het 
          werkt  ook  wel, maar  het is  dan niet  zo netjes  meer. Je 
          krijgt dan het volgende:
          
          PRTHEX: LD    C,A
                  AND   &HF0
                  RRCA
                  RRCA
                  RRCA
                  RRCA
                  CALL  UITHEX
                  LD    A,C
                  AND   &H0F
          UITHEX: ADD   A,"0"
                  CP    "9"+1
                  JP    C,&HA2
                  ADD   A,"A"-"9"-1
                  JP    &HA2
          
          Het  is  korter  en  sneller, en  toch zou  ik het  nooit zo 
          programmeren. In zo'n kleine routine kun je niet zo gauw van 
          spaghettistijl  spreken,  maar dit  begint er  aardig op  te 
          lijken. De  routine roept namelijk halverwege zijn staart al 
          een keer aan, en dat soort dingen vallen nu eenmaal onder de 
          noemer  "spaghetti". Ook  het vervangen van CALL &HA2 en RET 
          door een  JP en  is niet echt netjes, en het weglaten van de 
          tweede CALL UITHEX al helemaal niet.
          
          Maar goed,  welke routine u ook kiest, het is in ieder geval 
          beter  dan die  maffe routine van de vorige keer (al was dat 
          geen spaghetti!).
          
                                                          Stefan Boer
          
          
          Nvdr. Hier  verschillen Stefan  en ik  duidelijk van mening. 
          Vandaar  dat ik  deze tekst  toch laat  staan. In het stukje 
          over een  goede random routine gebruik ik nl. de routine die 
          volgens Stefan "fout" is.
