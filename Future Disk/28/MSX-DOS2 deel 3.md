 THE DOS2 COURSE GOES ON AND ON AND...
 ASCII'S WAY TO TORMENT MANKIND

                             ����
                             ����
                             ����
                             ����



 In other words, it time for DOS2 again. Not too much this time 
 as  time is indeed something I do not have... Just a few func- 
 tions a  little more  specified, we'll  reach $70  eventually, 
 some day... Right?!?   Ah well, we'll see...  By the way, I'm
 not going to go into all that DOS1 BDOS crap, just the speci-
 fic  DOS2 calls... DOS1 info  can be found practically every-
 where, but  if someone really wants  that info, a call -as in
 phonecall, letter - will suffice to solve that problem.

 

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
       


 And  that's all  folks. Sorry, I know, it aint much, but to be 
 hounest even  I have got better things to like. Like sleeping, 
 for  instance, because it's really terribly late... Till FD#29 
 I guess, with -hopefully- a little more than this time...
  
Tobias Keizer
