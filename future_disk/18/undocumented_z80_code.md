         -- MCODE --


 ����
 ����
 ����
 ����
 ����

 Stop de persen, gooit uw PC uit het raam, speelt uw MOD's op
 via uw Simpl, schrijf u in voor een survival-tocht in
 Grozny, omhels S.B. en wordt lid van de EO. Wat is immers
 het geval?

 Sinds ik mijn tijd verdoe op de TU in Eindhoven, leer je
 allerlei dingen over computers en kom je andere MSX-ers
 tegen. Zo hoorde ik laatst van Zelly (wie dat is zoek je
 zelf maar uit) dat er alweer een ongedocumenteerde
 instructie van de Z80 bestaat:

  IN      F,(C)

 Huh, was dat niet een R800-commando?? In m'n Z80?
 Jawel hoor, die klojo's van Zilog hebben ons al bijna 20
 jaar instructies verzwegen (durven ze wel stelletje boeren).
 Een daarvan is dus deze. Je bereikt hem door in WBASS-2 in
 de editor het volgende te zetten:

  IN      (HL),(C)

 Als opcode houdt dit in: EDh,70h. En verdomt, zo staat ie
 ook gedocumenteerdt in de R800-manual.
 Intern doet de Z80 eigenlijk de volgende dinges:

  IN      A,(C)
  OR      A

 Zo zou deze opcode dan ook kunnen maken, ware het niet dat
 IN F,(C) register A ongeschonden houdt.

 Wat kan ik nu met deze instructie ???

 Wel, je kunt er heel gemakkelijk een I/O poort mee in de
 gaten houden en hoeft geen OR-ren of AND-en meer te
 gebruiken. Kortom sommige dingen kunnen sneller!

 Programmeurs van Nederland, omhels deze instructie en
 waardeer hem alsof hij er altijd al was. Schroom je niet om
 'm te gebruiken. Hij is eindelijk legaal verklaart !!!

 Veel plezier met deze instructie,

                                Digital JW

 p.s.   Er hebben mij geruchten bereikt dat de Z80 ook zou
        kunnen vermenigvuldigen. Laten we dat eens gauw uit-
        proberen...
