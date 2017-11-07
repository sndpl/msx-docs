                                  U N C A L L 
                                               
          
          Een uitstapje  naar MSX-DOS zorgt ervoor dat als je in BASIC 
          een   programma  hebt   geladen,  dat  zichzelf  in  page  1 
          (#4000-#7FFF) van  het RAM  plaatst en gebruik maakt van een 
          CALL  commando, de  computer blijft  hangen bij nog eens een 
          CALL commando.
          
          UNCALL is hier de oplossing voor. UNCALL zorgt ervoor dat de 
          data, die  ervoor zorgt, dat er door de computer naar page 1 
          van het RAM plaatst, wordt weggehaald. Als je UNCALL.COM dus 
          aanroept in REBOOT.BAT, gaat het dus niet meer fout.
          
          Omdat  je  zonder  harddisk  toch  niet  vaak  naar  MSX-DOS 
          teruggaat,   en  je  zeker  geen  plaats  hebt  voor  kleine 
          utility's die  niet echt  nodig zijn, heb ik dit stukje toch 
          maar in de harddisk rubriek geplaatst.
          
          UNCALL  is   overigens  geschreven  door  Roderik  Muit  van 
          FCS-BBS: 01859-13957, 24 uur per dag.
          
                                                        Kasper Souren
