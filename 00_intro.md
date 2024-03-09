{{meta {load_files: ["code/intro.js"]}}}

# Uvod

{{quote {author: "Ellen Ullman", title: "Blizu mašine: Tehnofilija i njena nezadovoljstva", chapter: true}

Mislimo da pravimo sistem za naše potrebe. Verujemo da ga pravimo po svojoj slici... Ali računar nije zaista kao mi. Računar je projekcija veoma malog dela nas samih: onog dela posvećenog logici, redu, pravilima i jasnoći.

quote}}

{{figure {url: "img/chapter_picture_00.jpg", alt: "Ilustracija šrafcigera pored matične ploče od otprilike iste veličine", chapter: "framed"}}}

Ovo je knjiga o upravljanju ((računarima)). Računari su danas skoro jednako uobičajeni kao i šrafcigeri, ali su dosta složeniji, i nije ih uvek lako naterati da rade ono što želite.

Ako je zadatak koji imate za svoj računar uobičajen, dobro razumljiv, kao što je prikazivanje elektronske pošte ili da se ponaša kao kalkulator, možete otvoriti odgovarajuću ((aplikaciju)) i odmah početi raditi. Ali za jedinstvene (unikatne) ili otvorene zadatke, često nema odgovarajuće aplikacije.

Tu negde bi ((programiranje)) moglo doći na red. _Programiranje_ je akt konstruisanja _programa_ — skupa preciznih instrukcija koje računaru govore šta da radi. Zato što su računari glupi, pedantni stvorovi, programiranje je suštinski dosadno i frustrirajuće.

{{index [programiranje, "radost"], brzina}}

Srećom, ako možete prevazići tu činjenicu — i možda čak i uživati u rigoroznosti razmišljanja u terminima koje glupi uređaji mogu da razumeju — programiranje može biti veoma ispunjavajuće. Ono vam omogućava da uradite stvari u sekundama koje bi trajale _zauvek_ kada bi se radile ručno. To je način da naterate svoj računarski alat da radi stvari koje ranije nije mogao. Osim toga, to je sjajna igra rešavanja zagonetki i apstraktnog razmišljanja.

Većina programiranja se obavlja pomoću ((programskih jezika)). _Programski jezik_ je veštački konstruisani jezik koji se koristi za davanje instrukcija računarima. Interesantno je da je najefikasniji način koji smo pronašli za komunikaciju s računarom u velikoj meri pozajmljen iz načina na koji se komuniciramo međusobno. Slično ljudskim jezicima, računarski jezici omogućavaju da se reči i fraze kombinuju na nove načine, što nam omogućava izražavanje novih pojmova.

{{index [JavaScript, "dostupnost"], "opušteno računarstvo"}}

Nekad pre, interfejsi bazirani na jezicima, kao što su BASIC i DOS promptovi iz osamdesetih i devedesetih godina, bili su glavni način interakcije s računarima. Za rutinsku upotrebu računara, ovi interfejsi su uglavnom zamenjeni vizuelnim interfejsima, koji su lakši za učenje ali nude manje slobode. Ali ako znate gde da tražite, jezici su i dalje tu. Jedan od njih, _JavaScript_, ugrađen je u svaki moderni internet ((pretraživač)) — i time je dostupan na skoro svakom uređaju.

Ova knjiga će pokušati da vas upozna dovoljno sa ovim jezikom da biste mogli da radite korisne i zabavne stvari s njim.

## O programiranju

{{index [programiranje, "težina"]}}

Pored objašnjavanja JavaScript-a, predstaviću vam osnovne principe programiranja. Programiranje, ispostavlja se, nije lako. Osnovna pravila su jednostavna i jasna, ali programi izgrađeni na osnovu ovih pravila često postaju dovoljno složeni da uvedu svoja vlastita pravila i složenost. U suštini gradite svoj sopstveni lavirint, i lako se možete izgubiti u njemu.

Biće trenutaka kada će čitanje ove knjige biti izuzetno frustrirajuće. Ako ste novi u programiranju, biće puno novog materijala za svariti. Većina ovog materijala će onda biti _kombinovana_ na načine koji zahtevaju da uspostavite dodatne veze između raličitih koncepata.

Na vama je da uložite potrebni trud. Kada se borite da pratite knjigu, nemojte odmah donositi nikakve zaključke o svojim sposobnostima, ili njihovom nedostatku. Sve je uredu — samo treba da nastavite s radom. Napravite pauzu, ponovo pročitajte neke stranice, i pobrinite se da pročitate i razumete primere programa i ((vežbe)). Učenje je težak posao, ali sve što naučite je vaše i olakšaće dalje učenje.

{{quote {author: "Ursula K. Le Guin", title: "Leva ruka tame"}

{{index "Le Guin, Ursula K."}}

Kada radnja postane neprofitabilna, sakupljajte informacije; kada informacije postanu neprofitabilne, spavajte.

quote}}

{{index [program, "nature of"], data}}

Program je mnogo stvari. To je parče teksta koje je otkucao programer, to je sila koja čini da računar radi ono što radi, to su podaci u memoriji računara, a istovremeno kontroliše i radnje koje se izvršavaju nad ovim podacima. Analogije koje pokušavaju da uporede programe sa poznatim stvarnim objektima često nisu dovoljne. Površinski odgovarajuća analogija je uporediti program sa mašinom — obično je u njen rad uključeno mnogo odvojenih delova, i da bi cela stvar funkcionisala, moramo razmotriti načine na koje se ti delovi međusobno povezuju i doprinose da mašina radi kao jedna celina.

((Računar)) je fizička mašina koja je domaćin ovim nematerijalnim mašinama koje zovemo programi. Sami računari mogu obavljati samo preglupo jednostavne stvari. Razlog zbog kojeg su tako korisni je taj što ove stvari rade neverovatno brzo. Program može fascinantno kombinovati ogroman broj ovih jednostavnih radnji da bi uradio veoma komplikovane stvari.

{{index [programiranje, "radost"]}}

Program je zgrada sačinjena od misli. Besplatno je izgraditi je, nema težine, i lako raste tipkanjem pod našim rukama. Ali kako program raste, tako raste i njegova ((kompleksnost)). Veština programiranja je veština izgradnje programa koji vas ne zbunjuju. Najbolji programi su oni koji uspevaju da urade nešto zanimljivo dok su i dalje lako razumljivi.

{{index "stil programiranja", "najbolje prakse"}}

Neki programeri veruju da se ova kompleksnost najbolje održava korišćenjem samo malog skupa dobro razumljivih tehnika u njihovim programima. Oni su sastavili stroga pravila ("najbolje prakse") koja propisuju oblik koji bi programi trebalo da imaju i pažljivo ostaju unutar njihove bezbedne male zone.

{{index experiment}}

Ovo ne samo da je dosadno, već je i neefikasno. Novi problemi često zahtevaju nova rešenja. Polje programiranja je mlado i još uvek se brzo razvija, i dovoljno je raznovrsno da ima mesta za potpuno različite pristupe. Postoji mnogo strašnih grešaka koje možete napraviti u dizajnu svog programa, i trebalo bi da ih napravite barem jednom da biste ih pravilno razumeli. Osećaj kako dobar program izgleda razvija se praksom, a ne učeći sa liste pravila.

## Zašto je programski jezik bitan

{{index "programski jezik", "mašinski kod", "binarni podaci"}}

Na samom početku računarstva, nije bilo programskih jezika. Programi su izgledali nešto poput ovoga:

```{lang: null}
00110001 00000000 00000000
00110001 00000001 00000001
00110011 00000001 00000010
01010001 00001011 00000010
00100010 00000010 00001000
01000011 00000001 00000000
01000001 00000001 00000001
00010000 00000010 00000000
01100010 00000000 00000000
```

{{index [programming, "history of"], "punch card", complexity}}

Ovo je program za sabiranje brojeva od 1 do 10 i ispisivanje rezultata: `1 + 2 + ... + 10 = 55`. Mogao bi se izvršiti na jednostavnoj hipotetičkoj mašini. Da biste programirali prve računare, bilo je potrebno postaviti velike nizove prekidača na pravilne pozicije ili probušiti rupe u trakama od kartona i ubaciti ih u računar. Možete zamisliti koliko je ovaj postupak bio dosadan i podložan greškama. Čak i pisanje jednostavnih programa zahtevalo je puno domišljatosti i discipline. Složeni programi su bili skoro nezamislivi.

{{index bit, "čarobnjak"}}

Naravno, ručno unošenje ovih mističnih nizova bitova (jedinica i nula) davalo je programeru snažan osećaj da je moćan čarobnjak. I to mora nešto vredeti kada je u pitanju zadovoljstvo poslom.

{{index memorija, instrukcija}}

Svaka linija prethodnog programa sadrži jednu instrukciju. Može se napisati na engleskom jeziku na sledeći način:

 1. Sačuvaj broj 0 na memorijskoj lokaciji 0.
 2. Sačuvaj broj 1 na memorijskoj lokaciji 1.
 3. Sačuvaj vrednost memorijske lokacije 1 na memorijsku lokaciju 2.
 4. Oduzmi broj 11 od vrednosti u memorijskoj lokaciji 2.
 5. Ako je vrednost u memorijskoj lokaciji 2 broj 0, nastavi sa instrukcijom 9.
 6. Dodaj vrednost memorijske lokacije 1 na vrednost memorijske lokacije 0.
 7. Dodaj broj 1 na vrednost memorijske lokacije 1.
 8. Nastavi sa instrukcijom 3.
 9. Ispiši vrednost lokacije 0.

{{index citljivost, imenovanje, povezivanje}}

Iako je ovaj tekst već čitljiviji od one gore supe od bitova, i dalje je prilično nejasno. Korišćenje imena umesto brojeva za instrukcije i lokacije u memoriji pomaže:

```{lang: "null"}
 Postavi “ukupno” na 0.
 Postavi “brojač” na 1.
[petlja]
 Postavi “poređenje” na “brojač”.
 Oduzmi 11 od “poređenje”.
 Ako je “poređenje” nula, nastavi na [kraj].
 Dodaj “brojač” na “ukupno”.
 Dodaj 1 na “brojač”.
 Nastavi na [petlja].
[kraj]
 Ispiši “ukupno”.
```

{{index petlja, skok, "primer sabiranja"}}

Možete li videti kako program funkcioniše u ovom trenutku? Prve dve linije daju dvema lokacijama u memoriji njihove početne vrednosti: `ukupno` će se koristiti za čuvanje rezultata računanja, a `brojač` će pratiti broj koji trenutno posmatramo. Linije koje koriste `poređenje` verovatno su najviše zbunjujuće. Program želi da proveri da li je `brojač` jednak broju 11 da bi odlučio da li može prestati sa radom. Zato što je naša hipotetička mašina na kojoj se izvršava ovaj program prilično primitivna, može samo da proveri da li je broj nula i da donese odluku na osnovu te vrednosti. Zatim slede dve linije koje dodaju vrednost iz `brojač`a na rezultat i uvećavaju brojač za 1 svaki put kada program odluči da brojač još uvek nije 11.

Evo istog programa u JavaScript-u:

```
let ukupno = 0, brojac = 1;
while (brojac <= 10) {
  ukupno += brojac;
  brojac += 1;
}
console.log(ukupno);
// → 55
```

{{index "while petlja", petlja, [zagrade, blok]}}

Ova verzija sadrži nekoliko poboljšanja. Ono najvažnije, više nije potrebno specificirati način na koji želimo da program skače napred-nazad `while` petlja se brine o tome. Ova petlja nastavlja izvršavanje bloka ispod (obavijenog zagradama) sve dok uslov koji je dobila važi. Taj uslov je `brojac <= 10`, što znači “brojač je manji ili jednak 10”. Više nam nije potrebno kreirati privremenu vrednost i porediti je sa nulom. Deo snage programskih jezika je u tome što mogu brinuti o takvim detaljima umesto nas.

{{index "console.log"}}

Na kraju programa, nakon što je `while` petlja završila, operacija `console.log` se koristi za ispisivanje rezultata.

{{index "funkcija sabiranja", "funkcija opsega", apstrakcija, funkcija}}

Konačno, evo kako bi program izgledao da imamo na raspolaganju korisne operacije `opseg` i `suma`, koje redom kreiraju ((kolekciju)) brojeva unutar opsega i računaju zbir kolekcije brojeva:

```{startCode: true}
console.log(suma(opseg(1, 10)));
// → 55
```

{{index citljivost}}

Pouka ove priče je da isti program može biti izražen na dug i kratak, čitak i nečitak način. Prva verzija programa bila je izuzetno nejasna, dok je ova poslednja gotovo ljudski jezik: `log`uj `ukupno` `opsega` brojeva od 1 do 10. (Videćemo u kasnijim poglavljima kako definisati operacije poput `suma` i `opseg`.)

{{index ["programming language", "power of"], composability}}

Dobar programski jezik pomaže programeru tako što mu omogućava da razgovara na višem nivou o komandama koje računar mora izvršiti. Pomaže u izostavljanju detalja, pruža prikladne gradivne blokove (poput `while` i `console.log`), omogućava vam da definišete sopstvene gradivne blokove (poput `suma` i `opseg`), i olakšava vam da te blokove komponujete.

## Šta je JavaScript?

{{index history, Netscape, browser, "web application", JavaScript, [JavaScript, "history of"], "World Wide Web"}}

{{indexsee WWW, "World Wide Web"}}

{{indexsee Web, "World Wide Web"}}

JavaScript je programski jezik koji je predstavljen 1995. godine od strane Netscape Navigator-a kao način dodavanja programa veb stranicama. Od tada je usvojen od strane svih glavnih grafičkih web pretraživača i postao je ključan u stvaranju modernih web aplikacija, omogućavajući korisnicima direktnu interakciju sa web sadržajem bez potrebe za ponovnim učitavanjem stranica nakon svake interakcije. JavaScript se takođe koristi i na tradicionalnim veb sajtovima kako bi pružio različite oblike interaktivnosti.

{{index Java, naming}}

Važno je napomenuti da JavaScript nema skoro ništa zajedničko sa programskim jezikom koji se zove Java. Slično ime inspirisano je marketinškim trikovima. Kada je JavaScript bio predstavljen, programski jezik Java se snažno reklamirao i sticao popularnost. Neko je smatrao da je dobra ideja pokušati se osloniti na taj uspeh. Sada smo zaglavili sa tim imenom.

{{index ECMAScript, kompatibilnost}}

Nakon što je usvojen izvan Netscape-a, napisan je dokument o ((standardu)) koji opisuje način na koji bi JavaScript trebalo da funkcioniše kako bi različiti softveri koji  podržavaju JavaScript mogli da se uvere da zapravo pružaju isti jezik. To se naziva ECMAScript standard, po organizaciji Ecma International koja je sprovela standardizaciju. U praksi, termini ECMAScript i JavaScript se mogu koristiti zamjenjivo - to su dva imena za isti jezik.

{{index [JavaScript, "slabosti"], debagovanje}}

Neki ljudi će reći _strašno loše_ stvari o JavaScript-u. I mnoge od tih stvari su istinite. Kada sam prvi put trebao da nešto napišem u JavaScript-u, brzo sam počeo da ga prezirem. Prihvatao je skoro sve što sam otkucao, ali bi izvršio to na potpuno drugačiji način od onoga što sam zamislio. To je imalo veze s činjenicom da nisam imao pojma šta radim, naravno, ali ovde postoji stvarni problem: JavaScript je ultra ležeran u onome što dozvoljava. Ideja iza ovog dizajna bila je da programiranje u JavaScript-u bude lakše za početnike. Međutim, to većinom otežava pronalaženje problema u vašim programima jer sistem neće ukazati na njih.

{{index [JavaScript, "fleksibilnost"], fleksibilnost}}

Ova fleksibilnost ipak ima i svoje prednosti. Ostavlja nam prostora za tehnike koje su nemoguće u strožijim jezicima i omogućava prijatan, neformalan stil programiranja. Nakon što sam pravilno ((naučio)) jezik i radio s njim neko vreme, počeo sam zaista _voleti_ JavaScript.

{{index buducnost, [JavaScript, "verzije"], ECMAScript, "ECMAScript 6"}}

Postojalo je nekoliko verzija JavaScript-a. ECMAScript verzija 3 je bila široko podržana verzija tokom uspona JavaScript-a, otprilike između 2000. i 2010. U ovom periodu, radilo se na ambicioznoj verziji 4, u kojoj je planirano nekoliko radikalnih poboljšanja i proširenja jezika. Ovakva promena jednog živog, široko korišćenog jezika na tako radikalan način pokazala se kontroverznom, i rad na verziji 4 je napušten 2008. godine. Mnogo manje ambiciozna verzija 5, koja je napravila samo neka politički nesporna poboljšanja, izašla je 2009. godine. U 2015. godini, izašla je verzija 6, jedan od najvećih update-a, koje je uključilo neke ideje planirane za verziju 4. Od tada imamo nove, male promene svake godine.

Činjenica da se JavaScript razvija znači da web pretraživači moraju stalno da ga prate u korak. Ako koristite stariji pretraživač, on možda neće podržavati svaku funkciju jezika. Dizajneri jezika paze da ne prave promene koje bi mogle da pokvare postojeće programe, tako da novi pretraživači još uvijek mogu pokretati stare programe. U ovoj knjizi, koristićemo verziju JavaScript-a iz 2023. godine.

{{index [JavaScript, "uses of"]}}

Web pretraživači nisu jedine platforme na kojima se koristi JavaScript. Neke baze podataka, poput MongoDB-a i CouchDB-a, koriste JavaScript kao svoj skriptni i query jezik. Nekoliko platformi za desktop i serversko programiranje, posebno projekat ((Node.js)) (tema [Poglavlja ?](node)), pružaju okruženje za programiranje JavaScript-a van pretraživača (browsera).

## Kod, i šta s njim raditi

{{index "čitanje koda", "pisanje koda"}}

_Kod_ je tekst koji čini programe. Većina poglavlja u ovoj knjizi sadrži dosta koda. Verujem da je čitanje i pisanje ((koda)) nezamenjiv deo učenja programiranja. Pokušajte ne samo da preletite očima preko primera - pažljivo ih pročitajte i razumite. To može biti sporo i zbunjujuće u početku, ali obećavam da ćete brzo postati upraksani u tome. Isto važi i za ((vežbe)). Ne pretpostavljajte da ih razumete dok stvarno ne napišete funkcionalno i tačno rešenje.

{{index interpretacija}}

Preporučujem da isprobate vaša rešenja za vežbe u stvarnom JavaScript interpretatoru. Na taj način ćete odmah dobiti povratnu informaciju o tome da li ono što radite funkcioniše, i, nadam se, bićete motivisani da ((eksperimentišete)) i da idete dalje i šire od vežbi.

{{if interactive

Kada čitate ovu knjigu u pretraživaču, možete promeniti (i pokrenuti) sve primere programa klikom na njih.

if}}

{{if book

{{index download, sandbox, "running code"}}

Najlakši način za pokretanje koda u knjizi - i za eksperimentisanje s njim - je da ga potražite u online verziji knjige na [_https://eloquentjavascript.net_](https://eloquentjavascript.net). Tamo možete kliknuti na bilo koji primer koda da biste ga editovali i pokrenuli, i videli rezultat koji proizvodi. Za rad na vežbama, idite na [_https://eloquentjavascript.net/code_](https://eloquentjavascript.net/code), gde su dati početni kodovi za svaku vežbu i gde možete videti rešenja.

if}}

{{index "developer tools", "JavaScript konzola"}}

Pokretanje programa napisanih u ovoj knjizi izvan web lokacije knjige zahteva određenu pažnju. Mnogi primeri bi trebalo bi da rade u bilo kojem JavaScript okruženju. Medjutim, kod u kasnijim poglavljima često je napisan za određeno okruženje (pretraživač ili Node.js) i može se pokrenuti samo tamo. Pored toga, mnoga poglavlja definišu veće programe, a delovi koda koji se pojavljuju u njima zavise jedni od drugih ili od spoljnih datoteka. [Sandbox](https://eloquentjavascript.net/code) na veb lokaciji pruža linkove do ZIP datoteka koje sadrže sve skripte i datoteke potrebne za pokretanje koda za određeno poglavlje.

## Pregled ove knjige

Ova knjiga otprilike sadrži tri dela. Prvih 12 poglavlja govore o JavaScript jeziku. Sledećih sedam poglavlja govore o web pretraživačima i načinu na koji se JavaScript koristi za njihovo programiranje. Na kraju, dva poglavlja su posvećena ((Node.js)), još jednom okruženju za programiranje u JavaScript-u. U knjizi postoji pet projektnih poglavlja koja prikazuju veće programe kako bi vam pružili ukus pravog programiranja.

Deo knjige koji govori o JavaScript jeziku, počinje sa četiri poglavlja koja predstavljaju osnovnu strukturu JavaScript-a. Ova poglavlja objašnjavaju kontrolne strukture (poput `while` ključne reči koju ste videli u ovom uvodu), [funkcije](functions) (pisanje vaših sopstvenih gradivnih blokova) i [strukture podataka](data). Nakon što pročitate ova poglavlja, moći ćete da napišete osnovne programe. Zatim, Poglavlja [?](higher_order) i [?](object) uvode tehnike korišćenja funkcija i objekata za pisanje _apstraktnog_ koda i držanje kompleksnosti pod kontrolom.

Nakon [prvog projektnog poglavlja](robot) u kojem pravimo dostavljačkog robota, deo o JavaScript jeziku nastavlja se poglavljima o [hvatanju i ispravljanju grešaka](error), [regular expressions](regexp) (važan alat za rad sa tekstom), [modularnosti](modules) (još jedna odbrana od kompleksnosti) i [asinhronog programiranja](async) (rad sa događajima kojim je potrebno neko vreme da zavrse). [Drugo projektno poglavlje](language), gde implementiramo programski jezik, završava prvi deo knjige.

Drugi deo knjige, poglavlja od [?](browser) do [?](paint), opisuje alate u pretrazivacu kojima JavaScript ima pristup. Naučićete kako da prikažete stvari na ekranu (Poglavlja [?](dom) i [?](canvas)), reagujete na korisnički unos ([Poglavlje ?](event)) i komunicirate preko mreže ([Poglavlje ?](http)). I u ovom delu postoje dva projektna poglavlja, koja prave [platformer igru](game) i [program za crtanje](paint).

[Poglavlje ?](node) opisuje Node.js, a [Poglavlje ?](skillsharing) gradi malu veb stranicu koristeći Node.

{{if commercial

Konačno, [Poglavlje ?](fast) opisuje neke od stvari koje je potrebno razmotriti prilikom optimizacije brzine JavaScript programa.

if}}

## Tipografske konvencije

{{index "faktorijel funkcija"}}

U ovoj knjizi, tekst napisan u `monospaced` fontu će predstavljati elemente programa. Ponekad su to samostalni fragmenti, a ponekad samo se odnose na deo neposrednog programa. Programi (od kojih ste već videli nekoliko) su napisani na sledeći način:

```
function factorial(n) {
  if (n == 0) {
    return 1;
  } else {
    return factorial(n - 1) * n;
  }
}
```

{{index "console.log"}}

Ponekad, da biste prikazali rezultat koji program proizvodi, očekivani rezultat se piše posle njega, sa dva kosmička i strelicom ispred.

```
console.log(factorial(8));
// → 40320
```

Srećno!
