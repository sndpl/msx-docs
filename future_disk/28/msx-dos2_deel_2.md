 De DOS2 cursus gaat weer verder...

 ASCII'S WAY TO TORMENT MANKIND

                             ����
                             ����
                             ����
                             ����



 Oftewel, het is  weer tijd  voor DOS2. Niet zoveel deze keer,
 want echt veel tijd heb ik niet. Voor deze keer maar een paar
 functies specifiek onder  de loep,  we komen vanzelf een keer
 aan bij  $70. Overigens  ga ik  inderdaad niet de DOS1  calls
 behandelen. Deze info heeft  wel vaker ergens anders gestaan.
 Mocht er nu echt iemand zijn die daar toch intresse in heeft,
 dan is een belletje naar de redactie genoeg om dat probleem
 op te lossen.


 GET DISK PARAMETERS                                  [_DPARM]

 IN:   C  = $31
       DE = Pointer to 32byte buffer
       L  = Drive number
 RES:  A  = Error code
       DE = Preserved
 
 Created block:        +$00    Physical drive number
                       +$01    Sector size ($01FF)
                       +$03    Sector per cluster
                       +$04    Number of reserved sectors
                       +$06    Number of FATs
                       +$07    Entries in root directory
                       +$09    Number of logical sectors
                       +$0B    Media descriptor byte
                       +$0C    Sectors per FAT
                       +$0D    First root directory sector
                       +$0F    First data sector
                       +$11    Maximum cluster number
                       +$13    Dirty disk flag
                       +$14    Volume ID
                       +$18 1F Unused
 

 
 FIND FIRST ENTRY                                    [_FFIRST]
 
 IN:   C  = $40
       DE = File info block / ASCIIZ string pointer
       HL = ASCIIZ filename pointer (If DE = FIB)
       B  = Attributes
       IX = New file info block
 RES:  A  = Error code
       IX = Matching entry
 
 

 FIND NEXT ENTRY                                      [_FNEXT]
 
 IN:   C  = $41
       IX = Pointer to file info block from _FFIRST
 RES:  A  = Error code
       IX = Matching next entry


 FIND NEW ENTRY                                        [_FNEW]
 
 IN:   C  = $42
       DE = File info block / ASCIIZ string pointer
       HL = ASCIIZ filename pointer (If DE = FIB)
       B  = Attributes
       IX = New file info block
 RES:  A  = Error code
       IX = New entry
 

 
 OPEN FILE HANDLE                                      [_OPEN]
 
 IN:   C  = $43
       DE = Drive/path/file ASCIIZ of FIB
       A  = Mode:   b0, no write  b1, no read  b2 inheritable
 RES:  A  = Error code
       B  = New file handle
       

 En dat  was 't  alweer. Sorry,  ik weet  't, het is niet veel, 
 maar  om eerlijk  te zijn  heb ik  ook wel wat beters te doen. 
 Zoals slapen,  want het is al weer gruwelijk laat... Tot FD 29 
 maar weer, met hopelijk iets meer dan deze aflevering...
 
Tobias Keizer
