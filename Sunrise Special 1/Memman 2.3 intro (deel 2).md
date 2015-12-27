M E M M A N   I N T R O D U C T I E


(Nvdr. Dit  is het tweede gedeelte, het eerste gedeelte kunt
u in het submenu vinden.)


        T S R S   L A D E N

TSR  programma's zijn  te herkennen  aan de  extensie van de
bestandsnaam: ze eindigen op .TSR. Deze files bevatten naast
de eigenlijke programmacode ook alle informatie die nodig is
om de TSR goed in het geheugen te installeren.

Om bijvoorbeeld  de demonstratie  TSR 'CAPS' - die overigens
niets  anders doet  dan het Caps lampje laten knipperen - te
laden moet onder MSX-DOS ingetikt worden:

TL CAPS

Er  mogen  meerdere TSR-bestandsspecificaties  achter elkaar
worden opgegeven. Indien MSX-DOS2 gebruikt wordt, mag ook de
subdirectory worden  opgegeven, waaruit  de TSR geladen moet
worden.  Bijvoorbeeld, door middel van het volgende commando
worden zowel  de TSR  `CAPS' ingeladen,  als ook  alle TSR's
waarvan de bestandsnaam begint met `DEMO'.

TL CAPS DEMO*

`TL'  betekent `TSR-Load',  het is  het programma dat de TSR
laadt en  in het  geheugen plaatst.  Ook onder  BASIC is een
versie  van TSR-Load beschikbaar, zie hiervoor het hoofdstuk
`MemMan BASIC statements'.

Zodra TL  de TSR in het geheugen geinstalleerd heeft zal het
programma  actief worden.  In bovenstaand  voorbeeld wil dat
zeggen dat  het CAPS lampje zal gaan knipperen en bij iedere
toetsaanslag het cassetterelais aan of uit geschakeld wordt.

TL  is een  slim programma.  Zolang er in het MemMan segment
nog ruimte  is voor  TSR's zullen  ze daar geplaatst worden.
Alleen  als dat  ook echt  nodig is  wordt een nieuw segment
gebruikt,  dat  voor overige  toepassingen dan  onbereikbaar
gemaakt  wordt.   Dat  gebeurt   bijvoorbeeld  als   er  een
uitzonderlijk  groot TSR programma wordt geladen. Wanneer er
vervolgens weer een kleinere wordt geladen zal TL eerst alle
bestaande TSR  segmenten aflopen om te zien of er ergens nog
ruimte  is. De  volgorde waarin  de TSR's geladen worden zal
dan ook geen invloed hebben op het geheugengebruik.


     T S R S   B E K I J K E N

Het is  ten aller tijde mogelijk te kijken welke TSR's er op
dit  moment in  het geheugen  actief zijn. Daartoe bevat het
MemMan pakket  de utility  TV, TSR-View.  Het gebruik  is de
eenvoud zelf: gewoon achter de DOS prompt intikken:

TV

Er  zal  een  overzicht  verschijnen  van de  op dit  moment
actieve  TSR's, compleet  met hun  volledige naam. Deze naam
moet voor  iedere TSR  uniek zijn,  en zal  dan ook  vrijwel
altijd  de initialen  van de programmeur bevatten. Deze naam
is dus een andere dan de bestandsnaam!

Het is  deze volledige  naam - het TSR ID - die nodig is als
een  TSR  uit  het  geheugen  verwijderd  moet  worden.  Ook
programma's  die direct  met TSR's  samenwerken kunnen  deze
naam  gebruiken  om  te  zien  of  een TSR  in het  geheugen
aanwezig is.



  T S R S   V E R W I J D E R E N

Zoals gezegd  is het  ook mogelijk  TSR programma's weer uit
het  geheugen te  verwijderen. Het  benodigde programma heet
TK, TSR-Kill. TK zorgt er voor dat een TSR netjes verwijderd
wordt. Alle  andere TSR's blijven vlekkeloos doorwerken, als
de TSR als enige in een segment stond wordt dat segment weer
vrijgegeven voor gebruik door overige toepassingen.

Om  bijvoorbeeld het  het Caps  lampje weer normaal te laten
werken en  het knipperen uit te schakelen is het verwijderen
van de betrokken TSR voldoende. Daartoe tikt u:

TK "MJVcapsblink"

Hierbij   dient  de   volledige  naam   van  de  TSR  tussen
aanhalingstekens  opgegeven  te worden.  Er kunnen  meerdere
TSR's in  ��n keer  worden verwijderd,  door de namen achter
elkaar in te voeren. Bijvoorbeeld:

TK "TSR naam 1" "TSR naam 2" "TSR naam 3"

TSR-Kill  kan behalve  geheugen weer vrijmaken voor gebruik,
ook gebruikt worden om vastgelopen TSR's uit het geheugen te
verwijderen. Een  TSR die  om welke  reden dan ook niet meer
vlekkeloos  functioneert zal  met behulp  van TK meestal nog
wel verwijderd  kunnen worden.  Vervolgens kan de TSR met TL
weer  geladen  worden,  op dezelfde  manier als  het opnieuw
starten van gewone programma's nog wel eens wil helpen geldt
dat ook voor TSR's.


M E M M A N   B A S I C - S T A T E M E N T S

Vanaf  versie 2.3 bevat MemMan enkele statements en functies
die  vanuit  MSX-BASIC toegepast  kunnen worden.  Hiertoe is
MemMan van  een systeem  TSR voorzien,  met de  ID-naam "MST
TsrUtils".  Deze TSR  heeft volgnummer  0 en  kan niet  door
TSR-Kill verwijderd worden. Hieronder volgt een beschrijving
van de  beschikbare BASIC-instructies.  De betekenis  van de
gebruikte symbolen is als volgt:

[]   Wat tussen vierkante haken staat mag worden weggelaten.
<>   Omschrijvingen van parameters staan tussen gehoekte
haken.
()", Ronde haken moeten worden ingetikt, evenals leestekens
en comma's.


Commando: TSR-Load
Syntax:   CMD TL("<filename>",[T],[<N$>],[<F>])
Soort:    Statement
Voorbeeld:CMD TL("PB")
CMD TL("ALARM",T,,A)
Functie:  Laadt een TSR-bestand in het geheugen.
<filename> = Naam   van  het   TSR-bestand.  Onder
           MSX-DOS2 mag  een subdirectory worden
           opgegeven.
T          = Toon de intro-tekst van de TSR.
<N$>       = Naam  van een string-variabele waarin
           de TSR ID-naam zal worden opgeslagen.
<F>        = Variabele waarin  een foutcode  wordt
           opgeslagen.   Indien  deze  variabele
           wordt  weggelaten, zal  in geval  van
           laadfouten   een   standaard   BASIC-
           foutmelding   worden    gegeven.   De
           foutcodes zijn als volgt:

        0 = TSR met succes ingeladen
        1 = Installatie door de TSR afgebroken
        2 = Structuurfout in TSR bestand
        3 = TSR-tabel vol
        4 = Hook-tabel vol
        5 = Geen vrij MemMan segment
        6 = Te weinig vrij BASIC werkgeheugen


Commando: TSR-Kill
Syntax:   CMD TK("<TSR ID-naam>")
Soort:    Statement
Voorbeeld:CMD TK("MJV printbuf")
Functie:  Verwijdert een TSR uit het geheugen.
<TSR ID-Naam> = Identificatienaam van de te
              verwijderen  TSR.  Deze  naam  kan
              worden  opgevraagd door middel van
              TSR-View of Find-TSR.


Commando: TSR-View
Syntax:   CMD TV
Soort:    Statement
Functie:  Toont een overzicht van de ID-namen van alle
actieve TSR's.


Commando: Find-TSR name
Syntax:   ATTR$ FT("<TSR ID-naam>")
Soort:    Functie
Voorbeeld:IF ATTR$ FT("CAPS") THEN CMD TK("CAPS")
Functie:  Levert de waarde -1 indien de opgegeven TSR is
ge�nstalleerd, levert anders de waarde 0.
<TSR ID-Naam> = Identificatienaam van de TSR.


Commando: Find-TSR number
Syntax:   ATTR$ FT(<TSR volgnummer>)
Soort:    Functie
Voorbeeld:N$ = ATTR$ FT(0)
Functie:  Levert de  ID-naam van  de TSR  met het  opgegeven
volgnummer. Indien  het volgnummer  groter is  dan
het  aantal  actieve  TSR's,  dan  wordt  een lege
string met lengte 0 teruggeven.
