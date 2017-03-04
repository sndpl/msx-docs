# D E   P R O G R A M M E E R T A A L   C - D E E L   V I E R 
                                                  
          
          
## H E T   F I L E S Y S T E E M   V A N C - I N L E I D I N G 
          
De meeste programmeertalen stammen nog uit de  tijd  van  de
teletypes en de grote mainframes.  Alle  informatie  die  de
computers toen konden tonen en verwerken bestond  uit  tekst
en  getallen.  Dit  heeft  grote  invloed   gehad   op   het
filesysteem van de huidige computers: deze zijn in  principe
allemaal karakter- en byte-georiânteerd. Files worden gezien
als een rij achter elkaar staande karakters of bytes. Wat al
die bytes betekenen, en wat ermee gedaan moet worden mag  de
gebruiker zelf beslissen. Vanuit C  kun  je  files  op  twee
manieren benaderen: als binaire file en als tekstfile.


## F I L E - H A N D L E S   E N   F I L E - P O I N T E R S 

Wat enigszins verwarrend is, is het  feit  dat  C  eigenlijk
twee verschillende niveaus van  fileroutines  kent.  Op  het
laagste niveau wordt  met  "file handles"  gewerkt,  op  het
andere met "file pointers". Beide benaderingen  zijn  totaal
verschillend, en kunnen niet  zonder meer door elkaar worden
gebruikt.

Het laagste niveau is het niveau van de "file handles".  Dit
komt overeen met het laagste niveau  zoals  dat  onder  UNIX
gebruikt werd op de PDP11. (Daar is de taal C tenslotte voor
ontwikkeld.) De functies op dit niveau waren - voor zover ik
weet - gewoon routines van het operating system, die  binnen
C gebruikt werden.


## F I L E   H A N D L E S 

Een file handle is in  feite  niets  anders  dan  een  klein
geheel getal. Natuurlijk kan  de  gebruiker  niet  zelf  dit
getal uitkiezen, dat doet het systeem op het moment dat  een
bestand wordt geopend. Met deze file-handle kunnen we  vanaf
dat  moment  bestanden  lezen  en  schrijven,  en  het  file
uiteindelijk sluiten. Op dit niveau  kunnen  we  alleen  een
(deel van een) bestand in een (zelfgekozen) buffer  inlezen,
of vanuit een buffer naar  een  bestand  schrijven.  Ook  de
grootte van de buffer is vrij te kiezen.

Dit klinkt vrij primitief, en dat is het ook.  Voor  sommige
toepassingen is het echter voldoende,  en  de  snelheid  van
deze functies is een stuk hoger  dan  de  meer  uitgebreide,
maar  daardoor  meer  gecompliceerde   functies   met   file
pointers. Niet  alle  C-compilers  kunnen  met  file handles
werken, en op deze manier met files werken komt vrij  zelden
voor. Vandaar dat file handles hier nogal  summier  aan  bod
zullen komen.

Nvdr.  Zie  ook  Sunrise  Special  #2,  de MSX-DOS  2 cursus 
behandelt ook file handles, en dit komt bijna overeen met de 
file handles in C.

## W E R K E N   M E T   F I L E   H A N D L E S 

Het openen van een file gaat als volgt:

      open(naam, mode)

Als parameters  hebben we 'naam' (een char *) en 'mode' (een 
int).  De laatste moet de waarde 0 hebben als we een bestand 
willen lezen,  1 om  te schrijven,  en 2 voor beide. Als het 
openen  gelukt is geeft de functie een file-handle terug, zo 
niet dan ontstaat de waarde ERROR (ofwel -1).

Lezen en schrijven kan met de functies:

      read(handle, buffer, aantal)
      write(handle, buffer, aantal)

De 'handle' moet  de  filehandle  van  een  correct  geopend
bestand zijn, 'buffer' een char * naar een blok geheugen, en
'aantal'  de  hoeveelheid  bytes  die  we  willen  lezen  of
schrijven.  De  functie  geeft  het  daadwerkelijke   aantal
gelezen of geschreven bytes terug, of -1 bij fouten.

Na afloop sluiten we het bestand met:

      close(handle)

Als alles goed ging krijgen we de waarde 0 terug, anders -1.


## F I L E   P O I N T E R S 

File pointers zijn inderdaad pointers, maar waar ze  precies
naar 'pointen' gaat alleen het  filesysteem  wat  aan.  File
pointers worden - net als file handles - bij het openen  van
een file aan ons doorgegeven. Wij op onze  beurt  moeten  de
file pointer weer gebruiken in alle functies die we bij  dat
bestand willen gebruiken.

Door  files  via  file pointers  te  benaderen  worden  onze
mogelijkheden een  stuk  groter.  Alle  I/O  functies  lopen
intern via file pointers,  ook  bijvoorbeeld  'printf'.  Met
file pointers kunnen we niet alleen lezen en  schrijven  van
en naar bestanden op schijf, maar kunnen we ook  de  in-  en
uitvoerapparaten bereiken. We hoeven ook niet meer zelf onze
in- en uitvoerbuffers te kiezen, alles wordt verder door het
systeem geregeld.

Buiten dat zijn er nog  een  drietal  standaard  "bestanden"
beschikbaar, die we niet hoeven (sterker nog: mogen)  openen
of sluiten. Die aanhalingstekens staan  daar  omdat  het  in
feite invoer  van  het  toetsenbord,  en  uitvoer  naar  het
beeldscherm betreft. Deze drie bestanden zijn:

stdin   oftewel: standaard invoer. Dit is gebufferde  invoer
      via het toetsenbord. Gebufferd betekent hier dat  we
      de gewone regeleditor gebruiken zoals onder DOS,  en
      dat  pas  na  een   return   het   eerste   karakter
      beschikbaar is.

stdout  oftewel: standaard uitvoer, het beeldscherm.

stderr  oftewel: standaard foutboodschap  uitvoer,  eveneens
      het beeldscherm.

Functies zoals 'printf' sturen al hun uitvoer naar 'stdout', 
weer andere  halen hun gegevens uit 'stdin'. Door de waarden 
in  'stdin'  of  'stdout'  te  vervangen  door  andere  file 
pointers  kunnen we  bijvoorbeeld zorgen  dat 'printf'  zijn 
uitvoer op een ander apparaat doet.


## B I N A I R E   F I L E S   E N   T E K S T F I L E S 

Bij het openen van een file moeten we niet alleen kiezen  of
we willen lezen of schrijven (of beide) maar ook of het  als
een binair of een tekstfile moet worden behandeld.  Je  kunt
je  afvragen:  waarom  dit  onderscheid?  Tenslotte  is  het
onderscheid tussen een rij bytes  en  een  rij  8-bit  ASCII
karakters alleen een kwestie van interpretatie,  en  dat  is
typisch een  zaak  voor  de  programmeur.  Toch  is  er  een
onderscheid, en  dit hebben  we te danken (wijten?) aan UNIX 
en CP/M.

De ASCII code is oorspronkelijk ontworpen voor  gebruik  met
teletypes,  en uit die tijd stammen  nog  een  groot  aantal
controlecodes die we nog in  de  ASCII-tabellen  aantreffen,
meestal   met    geheimzinnig    aandoende    drie-letterige
afkortingen, zoals SYN, SOH, EOT, enzovoort.  Bij  teletypes
zijn  wagenterugloop  (carriage  return,  kortweg   CR)   en
regelopvoer  (line  feed,  kortweg  LF)  twee  verschillende
acties, die dus ook allebei een eigen code hadden  gekregen.
Voor een nieuwe regel moeten zowel een CR en een LF gebruikt
worden. In UNIX - die blijkbaar in  de  begintijd  niet  met
ASCII werkte - was voor  die  actie  een  enkele  code,  een
'newline' nodig (het karakter '\n').

Dit geeft problemen. C gaat uit van een enkele newline, waar
in onze ASCII files twee codes, een CR en  LF  staan.  Oeps.
Hoe lost C dit op? Wel, bij het lezen van een CR en  een  LF
wordt maar een van de codes doorgegeven, gewoonlijk  de  LF,
en die wordt binnen  de  C-omgeving  als  newline  gebruikt.
Omgekeerd wordt bij schrijven voor iedere newline een CR  en
een LF geschreven.

Het tweede probleem stamt uit de CP/M tijd, de voorloper van
zowel MSX-DOS als MS-DOS. CP/M werkt  met  records  van  128
byte elk, en de grootte van een file is  alleen  in  records
bekend, en niet in bytes zoals  in  MSX-DOS  en  MS-DOS.  De
consequentie daarvan is, is dat er na het  laatste  karakter
in een tekstfile nog  maximaal  127  loze  karakters  konden
volgen. Lastig. Daarom  werd  er  afgesproken  dat  als  een
tekstfile het laatste 128 byte record  niet  exact  wist  te
vullen  er  een  zgn.  end-of-file  karakter  moest   worden
geschreven, code  26  oftewel  control-Z.  Veel  programma's
schreven zelfs altijd zo'n code  als  laatste  karakter,  of
rekenden erop dat die code altijd aanwezig was.  (Een  goede
kans: 127 tegen 1. Zulke programma's liepen niet vaak  tegen
de lamp.) C houdt zich aan  deze  conventie,  en  geeft  bij
tekstfiles een EOF zodra een  control-Z  wordt  gelezen,  of
schrijft een control-Z bij het sluiten ervan.

## G E B R U I K   V A N   F I L E - P O I N T E R S 

In STDIO.H staat de typedefinitie van een file  -  die  heet
FILE (met hoofdletters) - en  filepointer variabelen  kunnen
we declareren met bijvoorbeeld:

        FILE *een_file;

Met de '*'  geven  we  aan  dat  het  een  pointer-variabele
betreft. De variabele hebben we nodig om na het  openen  van
een file de filepointer te kunnen opslaan; tenslotte  willen
we het bestand later  nog  kunnen  lezen  of  schrijven,  en
uiteindelijk  sluiten,  en  voor  al  die   acties   is   de
filepointer nodig. Het openen gaat als volgt:

        fopen(name, mode)

De functie-return is van het type FILE *, en als het  openen
mislukt krijgen we een NULL. Zowel "name"  als  "mode"  zijn
van het type char *, en  wijzen  naar  strings.  Hierbij  is
"name" de naam van het bestand en "mode" de manier waarop we
het bestand willen gebruiken, wat kan zijn:

"r"     tekstfile lezen ("read")
"w"     tekstfile  schrijven;   wist   het   oude   bestand.
      ("write")
"a"     als "w", maar voegt tekst toe aan het einde van  een
      bestand, en wist het oude bestand niet. ("append")

Vaak kan lezen en schrijven worden gecombineerd:

"r+"    tekstfile lezen, maar ook schrijven
"w+"    tekstfile schrijven, maar ook lezen
"a+"    tekst toevoegen, maar ook lezen

Voor  binaire  bestanden  moet  er  nog   een   "b"   worden
toegevoegd, en dan  krijgen  we  "rb",  "wb",  "ab",  "r+b",
enzovoort.

Helaas kennen niet alle systemen alle manieren van lezen  en
schrijven; een minimum is echter wel "r", "w", "rb" en "wb".



## L E Z E N   M E T   F I L E   P O I N T E R S 

Een typische manier om een bestand te openen  in  c  is  als
volgt:

      if ((een_file = fopen("help.txt", "r")) == NULL) {
        printf("Helptekst niet op deze schijf!\n");
        return; }

Hier onderbreken we dus de huidige functie op het moment dat
een bestand niet beschikbaar blijkt  te  zijn,  en  als  het
bestand wel geopend kan  worden  is  de  filepointer  meteen
opgeslagen in 'een_file'. Dit laat maar weer eens  zien  hoe
er in  C  zowel  simpel,  robuust  als  compact  kan  worden
geprogrammeerd. (Probeer hetzelfde maar  eens  in  BASIC  of
Pascal. Turbo Pascal wel  te  verstaan,  want  in  standaard
Pascal is het al helemaal niet te doen.)

Maar laten we ervan uitgaan  dat  het  bestand  geopend  kon
worden. In dat geval kunnen we het file gaan lezen, en  daar
zijn verschillende functies voor, zoals:

      getc(filepointer)
      fgetc(filepointer)
      fgets(buffer, aantal, filepointer)

De functies 'getc' en 'fgetc'  doen  precies  hetzelfde,  al
zijn er - officieel tenminste - intern een paar verschillen.
Beide lezen een karakter van een  file,  en  verwachten  een
filepointer, voorgesteld door 'filepointer', als  parameter.
Het karakter wordt - let goed op! -  als  int  teruggegeven,
omdat de constante EOF (die we krijgen als we  een  karakter
proberen te lezen na het einde van het bestand) niet in  een
char kan worden doorgegeven.

De functie 'fgets' leest een hele regel  tegelijk  (dus  t/m
'\n') in 'buffer' (een char *) maar nooit meer dan  'aantal'
- 1 karakters. Tevens wordt een afsluitende 0 toegevoegd. De
functie geeft de waarde 'buffer' terug, tenzij er  een  fout
is opgetreden, of voorbij het einde van  het  bestand  wordt
getracht te lezen: dan krijgen we NULL. Let er wel op dat de
buffer voldoende groot is. Als er een  char  array  gebruikt
wordt met tenminste 'aantal' elementen  kan  er  niets  fout
gaan.

Omdat dit zo vaak voorkomt zijn er  bovendien  nog  de  twee
functies

      getchar()
      gets(buffer, aantal)

Deze zijn precies gelijk aan respectievelijk:

      getc(stdin)
      fgets(buffer, aantal, stdin)

Na afloop moeten we een file sluiten met

      fclose(filepointer)

Ging  alles  goed  dan  krijgen  we  0  terug,  anders  EOF.
Overigens zullen bij het beeindigen van een  programma  alle
nog open files automatisch gesloten worden.


## S C H R I J V E N   M E T   F I L E   P O I N T E R S 

Voor het schrijven naar een file kunnen we van  de  volgende
functies gebruik maken:

      putc(karakter, filepointer)
      fputc(karakter, filepointer)
      fputs(string, filepointer)
      fprintf(filepointer, ...)

Met 'putc' of 'fputc' schrijven we een 'karakter' (een char)
naar een file. We krijgen EOF terug bij een fout, en  anders
0. Darentegen schrijft 'fputs' alle karakters  van  'string'
(een  char  *)  tegelijk  weg,  met  uitzondering   van   de
afsluitende 0. Bij  deze  functie  krijgen  we  geen  waarde
terug.  Tenslotte  'fprintf'  doet  precies  hetzelfde   als
'printf', alleen schrijft deze functie naar een file. De ...
geven hier aan dat hier een lijst met parameters moet volgen
zoals   bij   'printf'.   Ook   hier   wordt   geen   waarde
teruggegeven..

De functies

      putchar(karakter)
      puts(string)

komen overeen met respectievelijk:

      putc(karakter, stdout)
      fputs(string, stdout)


Ook na schrijven moet een bestand worden gesloten.

Als laatste een functie  die  eigenlijk  overal  buitenvalt,
namelijk:

      unlink(filenaam)

Hiermee kan een file 'filenaam' gewist worden. Deze  functie
is zeer gevaarlijk, omdat (meestal) ook  wildcards  ('?'  of
'*') zijn toegestaan, en een fout in het programma kan  hier
wel heel vervelende gevolgen hebben.  Bij  succes  wordt  de
waarde 0 teruggegeven, anders -1.

De naam 'unlink' is wederom een overblijfsel van  Unix,  die
op een wat andere manier  met  files  omgaat.  Met  'unlink'
wordt  uit  de  directory  van  de  huidige   gebruiker   de
referentie ('link') naar het file gehaald, en het file  zelf
wordt pas gewist als alle referenties ernaar verdwenen zijn.


## 2 . 3   W A T   N I E T   A A N   B O D   K W A M . . . 

In de officiâle C-standaard  bestaan  nog  een  hoop  andere
functies met  files.  Een  aantal  ervan  (zoals  'seek'  en
'tell') kunnen door de meeste C-compilers voor de  Z80  niet
op de juiste wijze worden  geãmplementeerd  omdat  het  type
'long'  ontbreekt.  Andere  functies  ontbreken  vanwege  de
verschillen tussen UNIX en CP/M  dan  wel  MSX-DOS.  Sommige
ontbreken gewoon omdat men  het  blijkbaar  niet  de  moeite
waard vond ze te programmeren. Voor microcomputers  moet  je
vaak een keuze maken, en de filesystemen nemen  zonder  meer
al het grootste deel van de standaardbibliotheken in beslag.
Wie  meer  wil   weten   moet   de   documentatie   van   de
desbetreffende compiler eens goed doorlezen.


## E E N   V O O R B E E L D   


Het aantal woorden in een file.

Ook  in  deze  aflevering  een  voorbeeldprogrammaatje.   In
afwijking met voorgaande afleveringen is hier niet  de  hele
source opgenomen, maar alleen de belangrijkste  functie.  De
rest  van  het  programma  moet  de  lezer  zelf  maar  eens
bestuderen; veel nieuws is er overigens niet in te vinden.

Onderstaand programma zou vooral handig zijn geweest  in  de
hoogtijdagen van de Amerikaanse pulp-magazines. Vooral Isaac
Asimov -  vorig  jaar  overleden  -  schroomde  niet  om  in
heruitgaves van zijn  oude  verhalen  precies  te  vertellen
hoeveel hem voor het  publiceren  van  een  bepaald  verhaal
betaald werd, en hoe het bedrag werd berekend.

De  uitgevers  betaalden  namelijk  per  geschreven   woord.
Afhankelijk  van  de  kwaliteit  van  het  verhaal   en   de
bekendheid en populariteit van  de  auteur  lag  dit  bedrag
tussen de halve en twee dollarcent per woord. We praten hier
weliswaar over de dertiger en veertiger  jaren,  maar  zelfs
dan is wel duidelijk dat de schrijverij voor de meesten niet
meer kon zijn dan een liefhebberij. Voor Asimov zelf  duurde
het tot in de jaren vijftig  voor  hij  full-time  schrijver
werd, en tegen die tijd had hij al een carriäre als chemicus
achter de rug.

```
  /* Tel de woorden. Geeft het aantal woorden in  */      
  /* een file. Als het file niet geopend kon      */      
  /* worden -1                                    */      
  int telw(file_naam)                                     
  char *file_naam;                                        
  {                                                       
    FILE *infile; /* File om uit te lezen         */      
    int teller;   /* Lopend totaal                */      
    int karakter; /* Moet int zijn vanwege EOF    */      
    char inwoord; /* TRUE --> midden in een woord */      
                                                          
                                                          
    if ((infile = fopen(file_naam, "r")) == NULL)         
      return -1; /* File kan niet geopend worden  */      
                                                          
    teller = 0;           /* Geen woorden geteld  */      
    inwoord = FALSE;      /* Start niet in woord  */      
                                                          
  /* Al 't werk gebeurt in dit while-statement    */      
    while ((karakter = getc(infile)) != EOF)              
      if (inwoord)                                        
        inwoord = ! leeg((char)karakter);                 
      else                                                
        if (! leeg((char)karakter)) {                     
          inwoord = TRUE;                                 
          teller++; }                                     
                                                          
    fclose(infile);                                       
                                                          
    return teller;                                        
  }  
```                            

Allereerst wordt geprobeerd het file te openen, en  als  dat
mislukt wordt de waarde  -1  teruggegeven.  Daarna  kan  het
echte werk beginnen.

De  gebruikte  methode  is  heel  eenvoudig.  De   variabele
'inwoord' geeft aan of we midden in  een  woord  zitten,  of
erbuiten. Zodra we van buiten een woord in een  woord  komen
verhogen we 'teller' met  1.  Na  afloop  bevat  teller  dan
precies het aantal woorden.

Elders in het programma is de functie  'leeg'  gedefinieerd,
die aangeeft of een karakter 'lege ruimte' voorstelt,  zoals
een spatie, een newline, enzovoort. Een klein probleem  hier
is dat de variabele 'karakter'  van  het  type  int  is,  en
'leeg' een char als parameter wil. Met  '(char)'  kunnen  we
zo'n conversie afdwingen; dit wordt een 'cast' genoemd. Meer
hierover in de volgende aflevering.

Het losse uitroepteken is  de  logisch-niet  operator.  Deze
geeft 0 (FALSE) als de uitdrukking  erachter  niet-0  (TRUE)
is, en omgekeerd.

In de uitdrukking achter 'while'  wordt  'getc'  net  zolang
aangeroepen  tot  een  EOF  gelezen  wordt.   Dit   is   een
gebruikelijke manier om een heel file te lezen. De variabele
'karakter' moet perse van het type 'int' zijn, omdat  anders
de int-waarde automatisch naar een char wordt geconverteerd,
waarna er niet meer op EOF kan worden getest.

Wie zich overigens afvraagt  waarom  er  na  de  'gets'  (in
'main') zo ingewikkeld wordt gedaan, dit is omdat de newline
die na de aanroep van 'gets' in de  string  zit  niet  wordt
geaccepteerd als laatste karakter van een filenaam,  en  dan
zou het file niet geopend kunnen worden. Vandaar.

Zo, dat was het dan weer voor deze  keer.  Zo  langzamerhand
zal  de  lezer  genoeg  van  C  kennen  om  er  al  serieuze
programmaatjes in te kunnen schrijven. Dus  hou  vol,  zelfs
als je denkt: "C is a harsh mistress!"

Robert Amesz
        