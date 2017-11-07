
# F I L E   H A N D L E S 
                                         

In deel  ÇÇn van  de DOS2-cursus heb je kunnen zien wat File 
Handles  zijn. In  deze tekst wordt duidelijk gemaakt hoe je 
ze  dient  te  gebruiken.  Dit  gaat  aan  de  hand van  een 
voorbeeld-source om een file mee te kopiâren.

Bij  kopiâren  komen  de  belangrijkste  aspecten  aan  bod: 
openen, aanmaken, lezen, schrijven en sluiten.


## M A C R O O T J E 

```
AdrBDOS:        EQU    5        ;Adres BDOS onder DOS
                              ;Is #F37D onder BASIC

BDOS:           MACRO  @Func    ;Macrootje
              LD     C,@Func
              CALL   AdrBDOS
              ENDM
```

Deze macro  maakt het voor mij iets makkelijker. Het scheelt 
typewerk  en het  is duidelijk. Als je niet weet hoe macro's 
werken,  moet  je  de  tekst  daarover  lezen  op  de vorige 
Special.


## E Q U A T E S 
```
Open:           EQU    #43      ;File Handle openen
Create:         EQU    #44      ;File aanmaken en openen
Close:          EQU    #45      ;File Handle sluiten
Read:           EQU    #48      ;File Handle lezen
Write:          EQU    #49      ;File Handle schrijven
TermError:      EQU    #62      ;Terminate with errorcode
```
Dit zijn de BDOS-functies die ik gebruik in deze source.



## O P E N E N 
```
              LD     DE,Filename
              LD     A,%0001  ;No write
              BDOS   Open
              JR     NZ,Error
              PUSH   BC       ;File Handle in B
```
Met  dit stukje  source open je een File Handle. Functie 43H 
werkt als volgt:

      Input:  C = 43H (_OPEN)
             DE = Drive/path/file in ASCIIZ
              A = Open mode. b0 geset => niet schrijven
                             b1 geset => niet lezen
      Output: A = Errorcode
              B = Nieuwe File Handle

Een ASCIIZ-string  is een  string die  afgesloten wordt  met 
ASCII  code 0  (Zero). Als er geen fout optreedt, is A 0. De 
BDOS van  DOS2 geeft  zelf voor het terugkeren een OR A, dus 
je  hoeft dat  zelf niet meer te doen. Je kunt meteen testen 
op het  Z bit,  en evt.  springen naar de error routine (zie 
onderaan).

B  wordt hier gePUSHt, omdat B wordt gebruikt in de volgende 
BDOS  routine.  B wordt  weer gePOPt  om de  File Handle  te 
sluiten.  Als  je een  File Handle  de hele  tijd (niet  erg 
exact, maar  het is  duidelijk) nodig  hebt, kun  je hem het 
beste  ergens in  de buurt van de ASCIIZ string zetten. Mijn 
favoriete stekje  is de  Zero byte,  die je immers toch niet 
meer gebruikt.


## L E Z E N 
```
              LD     DE,Buffer
              LD     HL,Length
              BDOS   Read
              JR     NZ,Error
```
      Input:  C = 48H (_READ)
              B = File Handle               
             DE = Buffer adres
             HL = Aantal te lezen bytes
      Output: A = Errorcode
             HL = Aantal gelezen bytes

De  File Handle  was nog  goed. Length  staat bij de string, 
da's overzichtelijker. Buffer is gewoon de byte die volgt na 
de laatste  byte van  het programma.  Als je  een grote file 
inleest,  moet je  erop letten  dat je niet over het hoogste 
adres heengaat.  Daar wordt  later misschien nog op ingegaan 
bij de bespreking van de Mapper Support Routines van DOS2.

Een file  van onbekende  grootte moet je gewoon inlezen door 
in  HL een  zo groot  mogelijk (rekening houdend met hoogste 
adres) getal  in te  vullen. Als  de HL  die terugkomt  even 
groot  is als  de input-HL,  moet het  lezen gewoon herhaald 
worden.


## O V E R   E N   S L U I T E N 
```
              POP    BC       ;File Handle terughalen
              BDOS   Close
              JR     NZ,Error
```
      Input:  C = 45H (_CLOSE)
              B = File Handle
      Output: A = Errorcode

Dit is niet noodzakelijk, maar wel netter. (Had ik maar zo'n 
functie om de troep op mijn kamer op te ruimen...)


## C R E A T E 
```
              LD     DE,Filename
              LD     A,DestDrive
              LD     (DE),A
              LD     A,%0010  ;No read
              LD     B,0      ;File attributen
              BDOS   Create
              JR     NZ,Error
              PUSH   BC
```
Eerst even  het adres  van de string inlezen, die vervolgens 
aanpassen en de registers goed zetten.


      Input:  C = 44H (_CREATE)
             DE = Drive/path/file in ASCIIZ
              A = Open mode. b0 geset => niet schrijven
                             b1 geset => niet lezen
              B = Gewenste attributen
      Output: A = Errorcode
              B = File Handle


## S C H R I J V E N 
```
              LD     DE,Buffer
              LD     HL,Length
              BDOS   Write
```
      Input:  C = 49H (_WRITE)
              B = File Handle
             DE = Buffer adres
             HL = Aantal te lezen bytes
      Output: A = Errorcode
             HL = Aantal gelezen bytes


## F O U T M E L D I N G E N 
```
Error:          LD     C,TermError
              LD     B,A      ;Als A=0 dan geen error
              JP     AdrBDOS
```
DOS2 heeft een hele mooie functie: Terminate with errorcode:

      Input:  C = 62H (_TERM)
              B = Errorcode

Als  B=0,   komt  er   geen  error   op  het   scherm.  Alle 
foutmeldingen zijn (uiteraard) in DOS2-stijl.

```
DestDrive:      EQU    "H"
Filename:       DB     "A:\FILHANDL.GEN",0
Length:         EQU    1057

Buffer:         END
```
DestDrive is de bestemmingsdrive. De lengte moet kloppen met 
de  originele file. Als de lengte te groot is, krijg je "*** 
End of file".

Kasper Souren
        