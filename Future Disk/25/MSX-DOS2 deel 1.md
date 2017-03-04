       MSX-DOS2, deel 1

                             ����
                             ����
                             ����
                             ����


Waar moet dat heen met de FD?!
 
 Kreeg  ik toch  laatst zomaar een brief van een lezer, HELP!?! 
 Hoe kan  dit?!? Wat moet dat! Kom nou, FD-leden schrijven geen 
 brieven!!  Nou ja, uit deze aangename verrassing blijkt dat er
 toch nog  FD leden  zijn die zo nu en dan eens een keertje een 
 briefje  durven te schrijven. Om deze pen-vriend te beschermen 
 zal ik  zijn identiteit  niet prijsgeven.  Neen, deze held mag 
 anoniem blijven.

MSX-DOS 2
 
 Want daar ging de brief namelijk over, hoe onze vriend met MSX 
 DOS 2 kon werken in assembly, de calls dus... En als de FD een 
 brief over een onderwerp  ontvangt, dan moet dat wel iets HEEL
 belangrijks zijn. Vandaar dat ik naast de brief die onze held
 inmiddels heeft ontvangen, ook nog een stukje op de FD aan die 
 onzin wil wijden. Ik  meende zelf dat DOS2 eigenlijk een open
 boek  was, maar blijkbaar zijn er toch nog mensen die hier nog 
 wel 't een en ander over willen weten. Daar gaan we dus...

 
DOS2 in BASIC
 
 Ik weet niet hoe het bij de andere MSX-en zit, maar bij de MSX 
 Turbo-R [DOS2.30]  is het  zo dat de BASIC programmeur nog een 
 aantal  extra  commando's  tot  zijn  beschikking heeft.  Door 
 gebrek  aan documentatie  ken ik  deze niet allemaal, maar een 
 aantal ken ik er toevalling uit m'n hoofd. Okay...
 
 CALL RAMDISK (xx)     >     Maak een ramdisk  van [xx] kB aan.
 
 CALL CHDRV ("x:")     >     Maak van  [x]  de  default  drive.
 
 CALL CHDIR ("xx")     >     Ga naar directory [xx] (zoals DOS)
                             Hierbij behoren "\" & ".." ook tot
                             de mogelijkheden.
                             
 CALL MKDIR ("xx")     >     Maak  directory  aan,  als  CHDIR.

 CALL RMDIR ("xx")     >     Verwijder  directory,  als  CHDIR.

 Het zal best dat er nog meer commando's mogelijk zijn, maar ik 
 ken ze niet. Mocht iemand wel meer informatie hebben dan wordt
 een belletje altijd gewaardeerd. Tot zover BASIC, nu naar het
 echte werk, DOS2 in ML.
 

MSX-DOS2 in Assembly langue.
 
 Ik  wil hier  verder niet  al te diep op ingaan, simpelweg een 
 beschrijving van de DOS2 calls, gewijzigde registers en input
 & output registers.  That's it! Hou er dus rekening mee dat
 alle registers  gewijzigd worden als ze al niet als output re-
 gister dienen. Eerst  de zooi  PUSHen dus. DOS1 voor de call
 betekent dat  de routine  DOS1 compatible is, anders dus NIET.
 Here it goes...
 
 Kort overzicht:
 
 $00 [DOS1]  Bye Bye!
 $01 [DOS1]  Console in
 $02 [DOS1]  Console out
 $03 [DOS1]  Aux in
 $04 [DOS1]  Aux out
 $05 [DOS1]  Printer out
 $06 [DOS1]  Direct console I/O
 $07 [DOS1]  Direct console in
 $08 [DOS1]  Console in without echo
 $09 [DOS1]  String out
 $0A [DOS1]  Buffered line in
 $0B [DOS1]  Console status
 $0C [DOS1]  DOS version
 $0D [DOS1]  Disk reset
 $0E [DOS1]  Select disk
 $0F [DOS1]  Open file                                 FCB
 $10 [DOS1]  Close file                                FCB
 $11 [DOS1]  Search first                              FCB
 $12 [DOS1]  Search next                               FCB
 $13 [DOS1]  Delete file                               FCB
 $14 [DOS1]  Sequential read                           FCB
 $15 [DOS1]  Sequential write                          FCB
 $16 [DOS1]  Create file                               FCB
 $17 [DOS1]  Rename file                               FCB
 $18 [DOS1]  Get login vector
 $19 [DOS1]  Get current drive
 $1A [DOS1]  Set Disk transfer address
 $1B [DOS1]  Get allocation info
 $1C [----]  N.U.
 $1D [----]  N.U.
 $1E [----]  N.U.
 $1F [----]  N.U.
 $20 [----]  N.U.
 $21 [DOS1]  Random read                               FCB
 $22 [DOS1]  Random write                              FCB
 $23 [DOS1]  Get file size                             FCB
 $24 [DOS1]  Set random record                         FCB
 $25 [----]  N.U.
 $26 [DOS1]  Random block write                        FCB
 $27 [DOS1]  Random block read                         FCB
 $28 [DOS1]  Random zero fill                          FCB
 $29 [----]  N.U.
 $2A [DOS1]  Get date       (Though I prefer to use Roses)
 $2B [DOS1]  Set date        Hmmm, almost funny �W�W�W�W�[
 $2C [DOS1]  Get time
 $2D [DOS1]  Set time
 $2E [DOS1]  -re-set verify flag
 $2F [DOS1]  Sector read
 $30 [DOS1]  Sector write
 
 $31 [DOS2]  Get disk parameters
 $32 [----]  N.U.
 $33 [----]  N.U.
 $34 [----]  N.U.
 $35 [----]  N.U.
 $36 [----]  N.U.
 $37 [----]  N.U.
 $38 [----]  N.U.
 $39 [----]  N.U.
 $3A [----]  N.U.
 $3B [----]  N.U.
 $3C [----]  N.U.
 $3D [----]  N.U.
 $3E [----]  N.U.
 $3F [----]  N.U.
 $40 [DOS2]  Find first entry
 $41 [DOS2]  Find next entry
 $42 [DOS2]  Find new entry
 $43 [DOS2]  Open file handle
 $44 [DOS2]  Create file handle
 $45 [DOS2]  Close file handle
 $46 [DOS2]  Ensure file handle
 $47 [DOS2]  Dupe file handle
 $48 [DOS2]  Read from file handle
 $49 [DOS2]  Write to file handle
 $4A [DOS2]  Move file handle pointer
 $4B [DOS2]  Device I/O control
 $4C [DOS2]  Test file handle
 $4D [DOS2]  Delete file/dir
 $4E [DOS2]  Rename file/dir
 $4F [DOS2]  Move file/dir
 $50 [DOS2]  Set/Get file attributes
 $51 [DOS2]  Set/Get file time//date
 $52 [DOS2]  Delete file handle
 $53 [DOS2]  Rename file handle
 $54 [DOS2]  Move file handle
 $55 [DOS2]  Set/Get file handle attributes
 $56 [DOS2]  Set/Get file handle date//time
 $57 [DOS2]  Get disk transfer address
 $58 [DOS2]  Get verify flag
 $59 [DOS2]  Get current dir
 $5A [DOS2]  Change dir
 $5B [DOS2]  Parse pathname
 $5C [DOS2]  Parse filename
 $5D [DOS2]  Check character
 $5E [DOS2]  Get whole path string
 $5F [DOS2]  Flush disk buffers
 $60 [DOS2]  Fork a child process
 $FD [DOS6]  Spoon an adult process
 $61 [DOS2]  Rejoin parent process        <- T'is net spoorloos
 $62 [DOS2]  Terminate with error code
 $63 [DOS2]  Define abort exit routine
 $64 [DOS2]  Define disk error handler routine
 $65 [DOS2]  Get previous error code
 $66 [DOS2]  Explain error code
 $67 [DOS2]  Format a disk
 $68 [DOS2]  Ramdisk settings
 $69 [DOS2]  Allocate sector buffers
 $6A [DOS2]  Logical drive assignment
 $6B [DOS2]  Get environment item
 $6C [DOS2]  Set environment item
 $6D [DOS2]  Find environment item
 $6E [DOS2]  Set/Get disk check status
 $6F [DOS2]  Get DOS version
 $70 [DOS2]  Set/Get redirection status


 Hoppa, da's  een hele  lijst. Maar  dan wel  een lijst waar je 
 niets  aan  hebt.  Voor  de  calls moet  je uiteraard  ook de
 specifieke  gegevens hebben. En laat ik nou net zo aardig zijn 
 om die je niet te geven... Sorry, geen tijd meer. Volgende FD!


Tobias "Gna, gna, gna" Keizer

