{{meta {docid: values}}}

# Vrednosti, Tipovi i Operatori

{{quote {author: "Master Yuan-Ma", title: "The Book of Programming", chapter: true}

Ispod površine mašine, program se kreće. Bez napora, širi se i skuplja. U velikoj harmoniji, elektroni se raspršuju i ponovo grupišu. Oblici na monitoru su samo talasi na vodi. Suština ostaje nevidljiva ispod.

quote}}

{{index "Yuan-Ma", "Book of Programming"}}

{{figure {url: "img/chapter_picture_1.jpg", alt: "Ilustracija mora tamnih i svetlih tačaka (bitova) sa ostrvima u njemu", chapter: framed}}}

{{index "binarni podaci", podatak, bit, memorija}}

U svetu računara, postoje samo podaci. Možete čitati podatke, menjati podatke, stvarati nove podatke - ali ono što nije podatak ne može se pomenuti. Svi ovi podaci čuvaju se kao dugi nizovi bitova i stoga su suštinski veoma slični.

{{index CD, signal}}

_Bitovi_ su bilo koja stvar koja ima dve vrednosti, obično opisane kao nule i jedinice. Unutar računara, oni poprimaju oblike poput visokog ili niskog električnog napona, snažnog ili slabog signala, ili sjajne ili mutne tačke na površini CD-a. Svaki komad diskretne informacije može se svesti na niz nula i jedinica i tako predstaviti u bitovima.

{{index "binarni broj", radix, "decimalni broj"}}

Na primer, broj 13 možemo izraziti u bitovima. Ovo funkcioniše na isti način kao sa decimalnim brojem, ali umesto 10 različitih (cifara), imamo samo 2, i vrednost (ili težina) svake od njih se uvećava za faktor 2 (uduplava) kad idemo s desna u levo. Evo bitova koje sačinjavaju broj 13, njihove težine (tj. vrednosti) su prikazane ispod njih:

```{lang: null}
   0   0   0   0   1   1   0   1
 128  64  32  16   8   4   2   1
```

To je binarni broj 00001101. Njegove ne-nula cifre (jedinice) predstavljaju brojeve 8, 4 i 1, i kada ih saberemo, dobijemo broj 13.

## Vrednosti

{{index [memorija, organizacija], "volatile data storage", "hard disk"}}

Zamislite more bitova - čitav okean njih. Tipičan moderni računar ima više od 100 milijardi bitova u svom kratkotrajnom skladištenju podataka (radna memorija). Dugoročna memorija (hard disk ili ekvivalent) obično ima daleko više memorije.

Da biste mogli da radite s takvom količinom bitova, a da se ne izgubite, razdvajate ih u komade koji predstavljaju delove informacija. U JavaScript okruženju, ti komadi nazivaju se _((vrednosti))_ (eng. _values_). Iako su sve vrednosti sastaljene od bitova, one igraju različite uloge. Svaka vrednost ima ((tip)) (eng. _type_) koji određuje njenu ulogu. Neke vrednosti su brojevi, neke su komadi teksta, a neke su funkcije i tako dalje.

{{index "garbage kolekcija"}}

Da biste kreirali vrednost, jednostavno je morate pozvati imenom. Ovo je veoma praktično. Samo ih pozovete, i ta-da, imate ih. Naravno, vrednosti se ne stvaraju iz ničeg. Svaka vrednost mora biti negde sačuvana u memoriji, i ako želite koristiti ogroman broj njih istovremeno, možda ćete potrošiti svu računarsku memoriju. Srećom, to je problem samo ako ih sve trebate istovremeno. Čim više ne koristite neku vrednost, ona će se raspršiti, ostavljajući iza sebe svoje bitove da se recikliraju kao građevinski materijal za sledeću generaciju vrednosti.

Preostali deo ovog poglavlja predstavlja atomske (nedeljive, osnovne) elemente JavaScript programa, to jest jednostavne tipove vrednosti i operatore koji se mogu izvršiti na tim vrednosti.

## Brojevi

{{index [sintaksa, broj], broj, [broj, notacija]}}

Vrednosti tipa _broj_ su, očekivano, numeričke vrednosti. U JavaScript programu, pišu se na sledeći način:

```
13
```

{{index "binarni broj"}}

Korišćenje toga u programu dovešće do nastanka niza bitova za broj 13 unutar memorije računara, kao što smo ranije prikazali.

{{index [broj, reprezentacija], bit}}

JavaScript koristi fiksni broj bitova, njih 64, kako bi se sačuvala jedna brojčana vrednost. Postoji fiksan broj kombinacija koje možete napraviti sa 64 bita, što ograničava količinu brojeva koji se mogu predstaviti. Sa N decimalnih ((cifara)), možete predstaviti 10^N^ brojeva. Isto tako, sa 64 binarna broja, možete predstaviti 2^64^ različitih brojeva, što je oko 18 kvintiliona (18 sa 18 nula iza njega). To je mnogo.

U početku, računarska memorija je bila mnogo manja, i ljudi su često koristili grupe od 8 ili 16 bitova da predstave svoje brojeve. Bilo je veoma lako slučajno "((_preliti_))" (eng. _overflow_) takve male brojeve - tj. završiti s brojem koji se nije uklapao u dati broj bitova. Danas, čak i računari koji stanu u vaš džep imaju dovoljno memorije, tako da slobodno možete koristiti 64-bitne blokove, i brinete se o overflow samo kada se bavite zaista astronomskim brojevima.

{{index znak, "floating-point broj", "sign bit"}}

Međutim, ne mogu svi celi brojevi manji od 18 kvintiliona stati u JavaScript broj. Ti bitovi koji prikazuju pozitivan broj, takođe prikazuju i negativne brojeve, pa jedan bit označava _znak_ broja. Veći problem je predstavljanje decimalnih (ne celih) brojeva. Da bi se to postiglo, neki od bitova se koriste za čuvanje pozicije decimalne tačke. Stvarni maksimalni ceo broj koji se može čuvati je više u opsegu od 9 kvadriliona (15 nula) — što je i dalje prilično ogromno.

{{index [broj, notacija], "razlomci"}}

Decimalni brojevi se pisu koristenjem tačke:

```
9.81
```

{{index exponent, "naučna notacija", [broj, notacija]}}

Za veoma velike ili veoma male brojeve, takođe možete koristiti naučnu notaciju, dodavanjem slova _e_ (skraćenica za _eksponent_), praćeno eksponentom broja:

```
2.998e8
```

Ovo predstavlja broj 2.998 × 10^8^ = 299,800,000.

{{index pi, [broj, "preciznost"], "decimalni number"}}

Računanja sa celim brojevima (eng. _integer_) koji su manji od pomenutih 9 kvadriliona uvek su garantovano precizna. Nažalost, računanja sa decimalnim brojevima obično nisu. Baš kao što π (pi) ne može tačno biti izražen konačnim brojem decimalnih cifara, mnogi brojevi gube neku preciznost kada su dostupna samo 64 bita za njihovo čuvanje. Nije idealno, ali to nam prouzrokuje praktične probleme samo u određenim situacijama. Važno je biti svestan toga i tretirati decimalne digitalne brojeve kao _približne_ vrednosti, a ne kao precizne vrednosti.

### Aritmetika

{{index [sintaksa, operator], operator, "binarni operator", aritmetika, sabiranje, množenje}}

Glavna stvar koja se radi sa brojevima je aritmetika. Aritmetičke operacije poput sabiranja ili množenja uzimaju dve numeričke vrednosti i proizvode novi broj iz njih. Evo kako to izgleda u JavaScriptu:

```{meta: "expr"}
100 + 4 * 11
```

{{index [operator, primena], zvezdica, "plus karakter", "* operator", "+ operator"}}

`+` i `*` simboli se zovu _operatori_. Prvi predstavlja sabiranje, a drugi množenje. Stavljanje operatora između dve vrednosti primenjuje taj operator na te vrednosti, i proizvodi novu vrednost.

{{index grupisanje, zagrade, redosled}}

Da li ovaj primer znači "Saberi 4 i 100, i pomnoži rezultat sa 11", ili se množenje vrši pre sabiranja? Kao što ste možda pretpostavili, množenje se obavlja prvo. Kao i u matematici, možete promeniti ovo tako što ćete sabiranje staviti u zagrade:

```{meta: "expr"}
(100 + 4) * 11
```

{{index "znak crtice", "znak slash", deljenje, oduzimanje, minus, "- operator", "/ operator"}}

Za oduzimanje koristi se operator `-`. Deljenje se vrši korišćenjem `/` operatora.

Kada se operatori pojave u istom izrazu bez zagrada, redosled njihove primene određuje se _redosledom_ operatora. Prethodni primer pokazuje da množenje dolazi pre sabiranja. Operator `/` ima isti prioritet kao i `*`. Isto tako, `+` i `-` imaju isti prioritet. Kada se više operatora istog prioriteta pojavi jedan pored drugog, kao ovde `1 - 2 + 1`, oni se primenjuju s leva na desno: `(1 - 2) + 1`.

Ne morate previše brinuti oko redosleda operatora i njihovih pravila. Ako niste sigurni, samo dodajte zagrade.

{{index "modul operator", deljenje, "operator ostatka", "% operator"}}

Postoji još jedan aritmetički operator koji možda nećete odmah prepoznati. Simbol `%` koristi se za predstavljanje operacije _ostatak_ (eng. _modulo_). `X % Y` je ostatak deljenja `X` sa `Y`. Na primer, `314 % 100` daje rezultat `14`, a `144 % 12` daje `0`. Prioritet operatora _modulo_ je isti kao i kod množenja i deljenja.

### Specijalni brojevi

{{index [broj, "specijalne vrednosti"], beskonacnost}}

Postoje tri posebne vrednosti u JavaScriptu koje se smatraju brojevima ali se ne ponašaju kao normalni brojevi. Prve dve su `Infinity` i `-Infinity`, koje predstavljaju pozitivnu i negativnu beskonačnost. `Infinity - 1` je i dalje `Infinity`. Međutim, nemojte previše verovati računima koji koriste beskonačnost. To nije matematički ispravno i brzo ćete doći do sledeće specijalne vrednosti: `NaN`.

{{index NaN, "nije broj", "deljenje nulom"}}

`NaN` znači "not a number" (u prevodu "nije broj"), iako to zapravo _jeste_ vrednost koja ima tip broja. Ovaj rezultat, ili vrednost, dobijate kada recimo pokušate izračunati `0 / 0` (nula deljeno nulom), `Infinity - Infinity` (beskonacno minus beskonacno), ili bilo koji drugi niz numeričkih operacija koje ne daju smislen rezultat. 

## Stringovi

{{indexsee "grave accent", backtick}}

{{index [syntax, string], text, character, [string, notation], "single-quote character", "double-quote character", "quotation mark", backtick}}

Sledeći osnovni tip podataka je _string_. Stringovi se koriste za predstavljanje teksta. Pišu se tako što se sadržaj stavi između navodnika.

```
`Down on the sea`
"Lie on the ocean"
'Float on the ocean'
```

Možete koristiti apostrofe, navodnike ili obrnute navodnike tj. `backticks` \` kako biste označili stringove, dokle god se navodnici na početku i kraju stringa poklapaju.

{{index "prelom linije", "znak za novu liniju"}}

Možete staviti gotovo bilo šta između navodnika kako biste JavaScriptu omogućili da od toga napravi string. Ali nekoliko znakova je teže staviti. Možete zamisliti kako bi bilo teško staviti navodnike između navodnika, budući da bi izgledali kao kraj stringa. _Novi redovi_ (eng. _newline_) (karakteri koje dobijete kada pritisnete [enter]{keyname}) mogu biti uključeni samo kada je string citiran koristenjem backticka (`` ` ``).

{{index [escaping, "u stringovima"], ["backslash znak", "u stringovima"]}}

Da bi bilo moguće uključiti takve znakove (karaktere) u string, koristi se sledeća notacija: kosa crta (`\`) unutar citiranog teksta označava da karakter posle nje ima posebno značenje. To se naziva _escaping_ karaktera. Navodnik koji prethodi kosoj crti neće završiti string već će biti deo njega. Kada se karakter `n` pojavi posle kose crte, tumači se kao novi red. Slično, `t` posle kose crte znači ((karakter tabulatora)). Uzmimo za primer sledeći string:

```
"Ovo je prva linija\nA ovo je druga"
```

Ovo je zapravo stvarni tekst stringa:

```{lang: null}
Ovo je prva linija
A ovo je druga
```

Naravno da postoje situacije i u kojima želite da kosa crta u stringu bude samo kosa crta, a ne specijalni kod. Ako se dve kose crte nalaze jedna za drugom, one će se spojiti i u stringu će ostati samo jedna. Na taj način se može izraziti string "_Novi red se piše ovako `"`\n`"`._":

```
"Novi red se piše ovako \"\\n\"."
```

{{id unicode}}

{{index [string, reprezentacija], Unicode, karakter}}

Stringovi takođe moraju biti modelovani kao niz bitova da bi mogli da postoje unutar računara. Način na koji JavaScript to radi zasniva se na _((Unicode))_ standardu. Ovaj standard dodeljuje broj skoro svakom karakteru koji bi ikada mogao da vam zatreba, uključujući karaktere iz jezika poput grčkog, arapskog, japanskog, armenskog, i tako dalje. Ako imamo broj za svaki karakter, onda string može biti opisan nizom brojeva. I to je upravo ono što JavaScript radi.

{{index "UTF-16", emotikon}}

Ipak, postoji jedna komplikacija: JavaScript koristi 16 bita za svaki element stringa, što može opisati do 2^16^ različitih karaktera. Međutim, Unicode definiše više karaktera od toga - otprilike dvostruko više, u ovom trenutku. Dakle, neki karakteri, poput mnogih emotikona, zauzimaju dve "pozicije karaktera" u JavaScript stringovima. Na ovo ćemo se vratiti u [Poglavlju ?](higher_order#code_units).

{{index "+ operator", sastavljanje}}

Stringovi se ne mogu deliti, množiti ili oduzimati. Ali `+` operator se _moze_ koristiti na njima, ne za dodavanje, vec za _spajanje_ - lepljenje dva stringa jedan za drugi. Sledeća linija će rezultirati stringom `"concatenate"`:

```{meta: "expr"}
"con" + "cat" + "e" + "nate"
```

Vrednosti stringova imaju niz povezanih funkcija (_metoda_) koje se mogu koristiti za obavljanje drugih operacija nad njima. Više reči o tome u [Poglavlju ?](data#methods).

{{index interpolacija, backtick}}

Stringovi napisani jednostrukim (apostrof) ili dvostrukim navodnicima ponašaju se veoma slično - jedina razlika je u tome koji tip navodnika morate izbegavati _unutar_ njih. Stringovi citirani backtickovima, obično nazvani _((template literals))_, mogu uraditi još nekoliko trikova. Osim što mogu obuhvatiti više linija, mogu takođe ugraditi i druge vrednosti.

```{meta: "expr"}
`polovina od 100 je ${100 / 2}`
```

Kada napišete nešto unutar `${}` u template literalu, taj rezultat će biti izračunat, pretvoren u string i stavljen na tu poziciju. Ovaj primer proizvodi "_polovina od 100 je 50_".

## Unarni operatori

{{index operator, "typeof operator", type}}

Nisu svi operatori simboli. Neki su napisani kao reči. Jedan primer je operator `typeof`, koji proizvodi string vrednost koja govori ime  vrednosti koju mu dajete.

```
console.log(typeof 4.5)
// → number
console.log(typeof "x")
// → string
```

{{index "console.log", output, "JavaScript console"}}

{{id "console.log"}}

Koristićemo `console.log` u primerima koda kako bismo naznačili da želimo videti rezultat evaluacije nečega. Više o tome u [sledećem poglavlju](program_structure).

{{index negation, "- operator", "binary operator", "unary operator"}}

Svi operatori prikazani do sada izvršavali su se na dve vrednosti, ali `typeof` uzima samo jednu. Operatori koji koriste dve vrednosti nazivaju se _binarni_ operatori, dok se oni koji uzimaju jednu nazivaju _unarni_ operatori. Operator minus može se koristiti i kao binarni operator i kao unarni operator.

```
console.log(- (10 - 2))
// → -8
```

## Boolean vrednosti

{{index Boolean, operator, true, false, bit}}

Često je korisno imati vrednost koja razlikuje samo dve mogućnosti, poput "da" i "ne" ili "uključeno" i "isključeno". U tu svrhu, JavaScript ima _Boolean_ tip, koji ima samo dve vrednosti, `true` i `false`, napisane kao te reči.

### Poređenje

{{index comparison}}

Evo jednog načina za proizvodnju Boolean vrednosti:

```javascript
console.log(3 > 2)
// → true
console.log(3 < 2)
// → false
```

{{index [comparison, "of numbers"], "> operator", "< operator", "greater than", "less than"}}

Znakovi `>` i `<` su simboli za "je veće od" i "je manje od". To su binarni operatori. Njihova primena rezultira Boolean vrednošću koja ukazuje da li su izrazi istiniti.

Stringovi se mogu upoređivati na isti način:

```javascript
console.log("Aardvark" < "Zoroaster")
// → true
```

{{index [comparison, "of strings"]}}

Način na koji su stringovi poređani je otprilike abecedni (tj. alfabetni), ali nije baš ono što biste očekivali da vidite u rečniku: velika slova su uvek "manja" od malih, tako da `"Z" < "a"`, i nealfabetni karakteri (!, -, i ostali) takođe su uključeni u poređenje. Prilikom upoređivanja stringova, JavaScript prolazi kroz karaktere s leva na desno, uporedjujuci njihove ((Unicode)) vrednosti jednu po jednu.

{{index equality, ">= operator", "<= operator", "== operator", "!= operator"}}

Drugi slični operatori su `>=` (veće ili jednako), `<=` (manje ili jednako), `==` (jednako), i `!=` (nije jednako).

```javascript
console.log("Garnet" != "Ruby")
// → true
console.log("Pearl" == "Amethyst")
// → false
```

{{index [comparison, "of NaN"], NaN}}

Postoji samo jedna vrednost u JavaScript-u koja nije jednaka sama sebi, a to je `NaN` ("nije broj").

```
console.log(NaN == NaN)
// → false
```

`NaN` bi trebalo da označava rezultat besmislenog računanja i kao takav, nije jednak rezultatu bilo kog _drugog_ besmislenog računanja.

### Logički operatori

{{index reasoning, "logical operators"}}

Postoje i neke operacije koje se mogu primeniti na same Boolean vrednosti. JavaScript podržava tri logička operatora: _i_, _ili_ i _ne_ (eng. _and_, _or_ i _not_). Oni se mogu koristiti za "rezonovanje" o Boolean vrednostima.

{{index "&& operator", "logical and"}}

`&&` operator predstavlja logičko _i_ (_and_). To je binarni operator, i njegov rezultat je _true_ samo ako su vrednosti sa obe strane operatora true(tačne).

```
console.log(true && false)
// → false
console.log(true && true)
// → true
```

{{index "|| operator", "logical or"}}

Operator `||` označava logičko _ili_ (_or_). Proizvodi true ako je bilo koja od vrednosti koje mu se daju true.

```
console.log(false || true)
// → true
console.log(false || false)
// → false
```

{{index negation, "! operator"}}

_Not_ je znak negacije koji se piše kao uzvičnik (`!`). To je unarni operator koji menja vrednost koja mu se daje - `!true` proizvodi `false` i `!false` daje `true`.

{{index precedence}}

Kada mešate ove logičke operatore sa aritmetičkim i drugim operatorima, nije uvek očigledno kada su vam potrebne zagrade. U praksi, obično je dovoljno da znate da od operatora koje smo do sada videli, `||` ima najniži prioritet, zatim `&&`, zatim operatori poređenja (`>`, `==`...), i na kraju ostali operatori. Ovaj redosled je izabran tako da, u tipičnim izrazima poput sledećeg bude potrebno što je manje moguće zagrada:

```{meta: "expr"}
1 + 1 == 2 && 10 * 10 > 50
```

{{index "conditional execution", "ternary operator", "?: operator", "conditional operator", "colon character", "question mark"}}

Poslednji logički operator koji ćemo pogledati nije unarni, niti binarni, već _ternarni_, operiše sa tri vrednosti. Piše se sa upitnikom i dvotačkom, kao što je prikazano:

```
console.log(true ? 1 : 2);
// → 1
console.log(false ? 1 : 2);
// → 2
```

Ovaj se naziva _uslovni_ ili _conditional_ operator (ili ponekad jednostavno _ternarni operator_ pošto je jedini takav operator u jeziku). Operator koristi vrednost levo od upitnika da odluči koju od dve druge vrednosti "izabrati". Ako napišete `a ? b : c`, rezultat će biti `b` kada je `a` tačno a `c` u suprotnom.

## Prazne vrednosti

{{index undefined, null}}

Postoje dve posebne vrednosti, `null` i `undefined`, koje se koriste da označe odsustvo _značajne_ vrednosti. Same su po sebi vrednosti, ali ne nose nikakvu informaciju.

Mnoge operacije u JavaScript-u koje ne proizvode nikakvu značajnu vrednost rezultiraju sa `undefined` jednostavno zato što moraju proizvesti _neku_ vrednost.

Razlika u značenju između `undefined` i `null` je nesrećan slučaj u dizajnu JavaScript-a, i većinu vremena nije bitna. U slučajevima kada zapravo morate da razmišljate o ovim vrednostima, preporučujem da ih tretirate kao uglavnom zamjenjive.

## Automatska konverzija tipova

{{index NaN, "type coercion"}}

U Uvodu sam spomenuo da JavaScript čini sve da prihvati skoro svaki program koji mu date, čak i programe koji rade čudne stvari. To se lepo može videti u sledećim linijama koda:

```
console.log(8 * null)
// → 0
console.log("5" - 1)
// → 4
console.log("5" + 1)
// → 51
console.log("five" * 2)
// → NaN
console.log(false == 0)
// → true
```

{{index "+ operator", arithmetic, "* operator", "- operator"}}

Kada se operator primeni na "pogrešan" tip vrednosti, JavaScript će tiho konvertovati tu vrednost u tip koji mu je potreban, koristeći skup pravila koji često nisu ono što želite ili očekujete. Ovo se naziva _((type coercion))_ ili koerzija tipova. `null` u prvom izrazu postaje `0`, a `"5"` u drugom izrazu postaje `5` (iz stringa u broj). Ipak, u trećem izrazu, `+` pokušava konkatenaciju stringova pre numeričkog sabiranja, pa se `1` konvertuje u `"1"` (iz broja u string).

{{index "type coercion", [number, "conversion to"]}}

Kada se nešto što nije očigledno mapirano na broj (kao što su `"five"` ili `undefined`) konvertuje u broj, dobijate vrednost `NaN` (Not a Number). Dalje aritmetičke operacije na `NaN` uvek proizvode `NaN`, pa ako pronađete ovu vrednost na neočekivanom mestu, pregledajte da li vam kod sadrži mesta u kojima se slučajno konvertuju tipovi na neočekivan način.

{{index null, undefined, [comparison, "of undefined values"], "== operator"}}

Kada uporedjujete vrednosti istog tipa koristeći operator `==`, ishod je lako predvideti: trebali biste dobiti true kada su obe vrednosti iste, osim u slučaju `NaN` vrednosti. Ali kada se tipovi razlikuju, JavaScript koristi složen i zbunjujući skup pravila da odredi šta se treba dogoditi. U većini slučajeva, jednostavno pokušava da konvertuje jednu od vrednosti u tip druge vrednosti. Međutim, kada se `null` ili `undefined` pojave s jedne ili druge strane operatora, rezultat će biti true samo ako se sa obe strane nalazi jedna od `null` ili `undefined` vrednosti.

```
console.log(null == undefined);
// → true
console.log(null == 0);
// → false
```

Ovakvo ponašanje često je korisno. Kada želite da testirate da li vrednost ima stvarnu vrednost umesto `null` ili `undefined`, možete je uporediti sa `null` koristeći `==` ili `!=` operator.

{{index "type coercion", [Boolean, "conversion to"], "=== operator", "!== operator", comparison}}

Šta ako želite da testirate da li nešto upućuje na _tačnu_ vrednost `false`? Izrazi poput `0 == false` i `"" == false` takođe su tačni zbog automatske konverzije tipova. Kada _ne_ želite da se vrše bilo kakve konverzije tipova, postoje još dva operatora: `===` i `!==`. Prvi testira da li je vrednost _precizno_ jednaka drugoj, dok drugi testira da li nije precizno jednaka. Tako da `"" === false` daje false kako se i očekuje, tj. prazan string _nije_ jednak verdnosti `false`.

Preporučujem korišćenje trokarakternih operatora za defanzivno poređenje kako biste sprečili da vas neočekivane konverzije tipova iznenade. Ali kada ste sigurni da će tipovi na oba kraja biti isti, nema problema sa korišćenjem kraćih operatora.

### Kratki spoj logičkih operatora

{{index "type coercion", [Boolean, "conversion to"], operator}}

Logički operatori `&&` i `||` barataju vrednostima različitih tipova na specifičan način. Oni će konvertovati vrednost na levoj strani u tip Boolean kako bi odlučili šta da rade, ali u zavisnosti od operatora i rezultata te konverzije, oni će vratiti ili _originalnu_ vrednost sa leve strane ili vrednost sa desne strane.

{{index "|| operator"}}

Na primer, operator `||`, će vratiti vrednost sa svoje leve strane kada se ta vrednost može konvertovati u `true`, u suprotnom će vratiti vrednost sa desne strane. Ovo ima očekivani efekat kada su vrednosti tipa Boolean i radi nešto analogno za vrednosti drugih tipova.

```
console.log(null || "user")
// → user
console.log("Agnes" || "user")
// → Agnes
```

{{index "default value"}}

Možemo koristiti ovu funkcionalnost kao način prelaska na podrazumevanu (default) vrednost. Ako imate vrednost koja može biti prazna, možete staviti `||` iza nje sa zamenskom vrednošću. Ako se početna vrednost može konvertovati u `false`, dobićete zamenu umesto nje. Pravila za konverziju stringova i brojeva u Boolean vrednosti kažu da `0`, `NaN`, i prazan string (`""`) predstavljaju `false`, dok svi drugi vrednosti predstavljaju `true`. To znači da `0 || -1` daje `-1`, a `"" || "!?"` daje `"!?"`.

{{index "?? operator", null, undefined}}

Operator `??` liči na `||`, ali vraća vrednost sa desne strane samo ako je vrednost sa leve strane `null` ili `undefined`, a ne ako je neka druga vrednost koja se može konvertovati u `false`. Često je ovo poželjnije ponašanje od ponašanja `||`. Evo primera:

```
console.log(0 || 100);
// → 100
console.log(0 ?? 100);
// → 0
console.log(null ?? 100);
// → 100
```

{{index "&& operator"}}

Operator `&&` radi slično, ali obrnuto. Kada je vrednost sa leve strane nešto što se konvertuje u `false`, vraća tu vrednost, a u suprotnom vraća vrednost sa desne strane.

Još jedna važna osobina ovih operatora je da se deo sa njihove desne strane evaluira samo kada je to potrebno. U slučaju `true || X`, bez obzira na to šta je `X`—čak i ako je deo programa koji radi nešto _grozno_ — rezultat će biti true, i `X` se nikada neće evaluirati. Isto važi i za `false && X`, koji je false i ignoriše `X`. Ovo se naziva _((evaluacija kratkog spoja))_ ili _((short-circuit evaluation))_.

{{index "ternary operator", "?: operator", "conditional operator"}}

Uslovni (ternarni) operator radi na sličan način. Od druge i treće vrednosti, samo ona koja je odabrana se evaluira.

## Da rezimiramo

U ovom poglavlju pregledali smo četiri tipa JavaScript vrednosti: brojeve, stringove, Boolean vrednosti i vrednosti koje nisu definisane. Ove vrednosti se kreiraju unošenjem njihovog imena (`true`, `null`) ili njihove vrednosti (`13`, `"abc"`).

Možete kombinovati i transformisati vrednosti pomoću raznih operatora. Videli smo binarne operatore za aritmetiku (`+`, `-`, `*`, `/` i `%`), konkatenaciju (spajanje) stringova (`+`), poređenje (`==`, `!=`, `===`, `!==`, `<`, `>`, `<=`, `>=`) i logiku (`&&`, `||`, `??`), kao i nekoliko unarnih operatora (`-` za negiranje broja, `!` za logičko negiranje i `typeof` za pronalaženje tipa vrednosti) i ternarni operator (`?:`) za izbor jedne od dve vrednosti na osnovu treće vrednosti.

Ovo vam daje dovoljno informacija da koristite JavaScript kao džepni kalkulator, ali ne mnogo više. [Sledeće poglavlje](program_structure) će početi da povezuje ove izraze u osnovne programe.