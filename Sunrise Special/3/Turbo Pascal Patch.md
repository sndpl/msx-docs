                     T P A T C H   1 . 1 
                                                   
          
          Nieuw in deze versie: aanpassing heap adres
          
          TPATCH  patcht Turbo Pascal 3.0A MSX programma's automatisch 
          voor het  gebruik met  de MST  Memory Manager.  MemMan TSR's 
          functioneren  alleen  correct  als  de  stackpointer in  RAM 
          pagina  3 staat.  Turbo Pascal  zet echter  zelf een  lokale 
          stack op,  waarbij de  stackpointer in pagina 1 of 0 terecht 
          kan komen. Dat kan tot een crash leiden.
          
          Het   is   weliswaar   mogelijk   met   een   debugger   het 
          stackpointer-adres in TP-programma's aan te passen, maar dat 
          neemt  veel  tijd  in beslag.  Daarom is  TPATCH geschreven. 
          TPATCH  zet de stackpointer van het TP-programma automatisch 
          op het default adres (0C500H in de distributie-versie) of op 
          een adres  dat als parameter in de commandline opgegeven is. 
          Versie 1.1 van TPATCH past bovendien de heap-pointer aan.
          
          
          Turbo Pascal deelt programma's als volgt in:
          
          00100H   Start runtime
          020EAH   Start programma
          0xxxxH   Ruimte voor stack & heap
          0yyyyH   Start data
          0zzzzH   Eindadres programma
          
          Na het gebruik van TPATCH ziet de indeling er als volgt uit:
          
          00100H   Start runtime
          020EAH   Start programma
          0xxxxH   Loze ruimte (zo klein mogelijk houden)
          0yyyyH   Start data
          0zzzzH   Eindadres programma
          0zzzzH+1 Ruimte voor stack & heap
          0ssssH   Opgegeven top van de stack
          
          
          Doordat  het  heap-adres  in  de  vorige  versie  niet  werd 
          veranderd, gaf de MAXAVAIL functie verkeerde resultaten. Die 
          functie geeft  namelijk het verschil weer tussen de adressen 
          waar  stackpointer en  heappointer zich  bevinden. Wordt  de 
          stackpointer BOVEN het programmagedeelte geplaatst, dan ziet 
          MAXAVAIL veel  meer ruimte  dan er  is, en kan programmacode 
          overschreven  worden.
          
          De ruimte die voorheen voor heap en stack gereserveerd werd, 
          dient  nu zo  klein mogelijk  gemaakt te worden, door in het 
          compiler-Options menu van Turbo Pascal het eindadres zo laag 
          mogelijk  te  zetten  (zodat  nog nät  geen memory  overflow 
          ontstaat). Een  bijkomend voordeel  is dat  de uiteindelijke 
          COM-file die Turbo Pascal aanmaakt kleiner wordt.
          
          Houd er  bij het  programmeren verder  rekening mee,  dat de 
          heap   en  stack   voldoende  ruimte   moeten  hebben.  Maak 
          programma's  niet   te  groot.   Overlays  gebruiken   extra 
          heapruimte.
          
          
          Het gebruik van TPATCH:
          
                  TPATCH FILENAAM[.COM] [hhhh]
          
          Vul  voor  de  hhhh  het  (hexadecimale)  adres  in  waar de 
          stackpointer  op  moet  komen  te  staan.  Wordt  alleen  de 
          filenaam opgegeven, dan zet TPATCH de stackpointer op $C500. 
          Die zgn.  'default' waarde  is aan  te passen  door met  een 
          debugger  of disk-editor (eenmalig) het programma TPATCH.COM 
          aan  te  passen. Op  de adressen  $102 en  $103 (START+2  en 
          START+3)  staat  nu:  00  C5, zijnde  $C500 in  hexadecimale 
          notatie (LSB,MSB). Dat kan naar believen veranderd worden.
          
                                                        Pierre Gielen
         