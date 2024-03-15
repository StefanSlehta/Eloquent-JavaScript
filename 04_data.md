{{meta {load_files: ["code/journal.js", "code/chapter/04_data.js"], zip: "node/html"}}}

# Strukture podataka: Objekti i nizovi

{{quote {author: "Charles Babbage", title: "Passages from the Life of a Philosopher (1864)", chapter: true}

U dva navrata su me pitali, 'Gospodine Babidž, ako stavite pogrešne brojeve u mašinu, da li će izaći tačni odgovori?' [...] Nisam u mogućnosti pravilno da shvatim vrstu konfuzije ideja koja bi mogla izazvati takvo pitanje.

quote}}

{{index "Babbage, Charles"}}

{{figure {url: "img/chapter_picture_4.jpg", alt: "Ilustracija veverice pored gomile knjiga i para naočara. Mesec i zvezde su vidljivi u pozadini.", chapter: framed}}}

{{index object, "data structure"}}

Brojevi, Boolean i stringovi su atomi od kojih se grade strukture podataka. Međutim, mnoge vrste informacija zahtevaju više od jednog atoma. _Objekti_ nam omogućavaju da grupišemo vrednosti — uključujući i druge objekte — kako bismo izgradili složenije strukture.

Programi koje smo dosad gradili su bili ograničeni činjenicom da su radili samo sa jednostavnim tipovima podataka. Nakon što naučite osnove struktura podataka u ovom poglavlju, imaćete dovoljno znanja da počnete da pišete još korisnije programe.

Ovo poglavlje će proći kroz više manje realističan programski uvod, uvodeći koncepte i načine na se primenjuju na dati problem. Primeri koda često će se oslanjati na funkcije i varijable koja su ranije predstavljena u knjizi.

{{if book

Online sandbox za knjigu ([_https://eloquentjavascript.net/code_](https://eloquentjavascript.net/code)) pruža način da se kod pokreće u kontekstu određenog poglavlja. Ako odlučite da radite kroz primere u drugom okruženju, prvo preuzmite sav kod za ovo poglavlje sa stranice.

if}}

## Weresquirrel (igra reči na vukodlak, gdje umesto "vuk" koristimo "veverica")

{{index "weresquirrel example", lycanthropy}}

S vremena na vreme, obično između 20h i 22h, ((Žak)) primeti kako se pretvara u malog dlakavog glodara sa bujnim repom.

S jedne strane, Žak je prilično srećan što nema klasičnu likantropiju. Pretvaranje u vevericu izaziva manje problema nego pretvaranje u vuka. Umesto što bi morao da brine o tome da slučajno ne pojede komšiju (_to_ bi bilo neugodno), mora da se brine da ga ne pojede mačka komšije. Nakon dva puta kada se probudio na nestabilno tankoj grani u krošnji hrasta, go i dezorijentisan, navikao je da noću zaključava vrata i prozore svoje sobe i ostavlja nekoliko oraha na podu da bi se zabavio.

Ali Žaku bi više odgovaralo da se potpuno oslobodi svog stanja. Neregularna pojava transformacije navodi ga da posumnja da bi mogla biti izazvana nečim. Neko vreme je verovao da se to dešava samo onih dana kada je bio blizu hrastova. Međutim, izbegavanje hrastova nije rešilo problem.

{{index journal}}

Prebacujući se na naučni pristup, Žak je počeo da vodi dnevnik svega što radi u toku jednog dana i da beleži da li je promenio oblik. Sa ovim podacima nada se da će suziti uslove koji pokreću transformacije.

Prva stvar koja mu je potrebna je struktura podataka za čuvanje ovih informacija.

## Skupovi podataka

{{index ["data structure", collection], [memory, organization]}}

Da bismo radili sa skupom digitalnih podataka, prvo moramo pronaći način da ih predstavimo u memoriji našeg računara. Recimo, na primer, da želimo da predstavimo ((kolekciju)) brojeva 2, 3, 5, 7 i 11.

{{index string}}

Mogli bismo biti kreativni sa stringovima — stringovi mogu imati bilo koju dužinu, pa u njih možemo staviti mnogo podataka — i koristiti `"2 3 5 7 11"` kao jedan način predstavljanja podataka. Ali ovo je nepraktično. Morali bismo na neki način izdvojiti cifre i pretvoriti ih nazad u brojeve da bismo im pristupili.

{{index [array, creation], "[] (array)"}}

Srećom, JavaScript pruža tip podataka specifičan za čuvanje sekvenci vrednosti. Naziva se _niz_ i piše se kao lista vrednosti između ((uglastih zagrada)), razdvojenih zarezima:

```
let listOfNumbers = [2, 3, 5, 7, 11];
console.log(listOfNumbers[2]);
// → 5
console.log(listOfNumbers[0]);
// → 2
console.log(listOfNumbers[2 - 1]);
// → 3
```

{{index "[] (subscript)", [array, indexing]}}

Notacija za pristupanje elementima unutar niza takođe koristi ((uglaste zagrade)). Par uglastih zagrada odmah nakon izraza, sa još jednim izrazom unutar njih, tražiće element u izrazu sa leve strane zagrade, koji odgovara _((indeksu))_ koji se dobije računanjem izraza unutar zagrada. 

{{id array_indexing}}
{{index "zero-based counting"}}

Prvi indeks niza je nula, a ne jedan, tako da se prvi element dobija sa `listOfNumbers[0]`. Brojanje koje počinje od nule ima dugu tradiciju u tehnologiji i na određene načine ima smisla, ali zahteva malo navikavanja. Zamislite indeks kao broj stavki koje treba preskočiti, brojeći od početka niza.

{{id properties}}

## Properties (svojstva)

{{index "Math object", "Math.max function", ["length property", "for string"], [object, property], "period character", [property, access]}}

Videli smo nekoliko izraza poput `myString.length` (za dobijanje dužine stringa) i `Math.max` (funkcija za maksimum) u prošlim poglavljima. Ovi izrazi pristupaju _svojstvu_ neke vrednosti. U prvom slučaju, pristupamo svojstvu `length` vrednosti u `myString`. U drugom slučaju, pristupamo svojstvu nazvanom `max` u objektu `Math` (koji je kolekcija matematičkih konstanti i funkcija).

{{index [property, access], null, undefined}}

Skoro sve JavaScript vrednosti imaju svojstva. Iznimke su `null` i `undefined`. Ako pokušate pristupiti svojstvu nekoj od ovih vrednosti, dobićete grešku:

```{test: no}
null.length;
// → TypeError: null has no properties
```

{{indexsee "dot character", "period character"}}
{{index "[] (subscript)", "period character", "square brackets", "computed property", [property, access]}}

Dva glavna načina pristupa svojstvima u JavaScript-u su tačkom i uglastim zagradama. Oba načina `value.x` i `value[x]` pristupaju svojstvu na `value` — ali ne nužno istom svojstvu. Razlika je u tome kako se `x` tumači. Kada koristimo tačku, reč nakon tačke je doslovno ime svojstva. Kada koristimo uglaste zagrade, izraz između njih se _evaluira_ kako bi se dobilo ime svojstva. Dok `value.x` uzima svojstvo iz `value` nazvano "x", `value[x]` uzima vrednost promenljive nazvane `x` i koristi je, konvertovanu u string, kao ime svojstva.

Ako znate da je svojstvo koje vas zanima nazvano _color_, kucate `value.color`. Ako želite da izdvojite svojstvo nazvano vrednošću varijable `i`, kažete `value[i]`. Imena svojstava su stringovi. Mogu biti bilo koji string, ali tačkasta notacija radi samo sa imenima koja izgledaju kao validna imena varijabli — počevši sa slovom ili donjom crtom, i sadržeći samo slova, brojeve i donje crte. Ako želite da pristupite svojstvu nazvanom _2_ ili _John Doe_, morate koristiti uglaste zagrade: `value[2]` ili `value["John Doe"]`.

Elementi u nizu se čuvaju kao svojstva niza, koristeći brojeve kao imena svojstava. Zato što ne možete koristiti tačkastu notaciju sa brojevima a i obično želite koristiti varijablu koja drži indeks, morate koristiti notaciju uglastim zagradama da biste im pristupili.

{{index ["length property", "for array"], [array, "length of"]}}

Baš kao i stringovi, nizovi imaju svojstvo `length` koje nam govori koliko elemenata niz sadrži.

{{id methods}}

## Metode

{{index [function, "as property"], method, string}}

I stringovi i nizovi sadrže, pored svojstva `length`, niz drugih svojstava koja drže vrednosti tipa funkcija.

```
let doh = "Doh";
console.log(typeof doh.toUpperCase);
// → function
console.log(doh.toUpperCase());
// → DOH
```

{{index "case conversion", "toUpperCase method", "toLowerCase method"}}

Svaki string ima svojstvo `toUpperCase`. Kada se pozove, vratiće kopiju stringa u kojoj su sva slova pretvorena u velika slova. Takođe postoji i `toLowerCase`, koji ide u suprotnom smeru.

{{index "this binding"}}

Interesantno je da, iako poziv funkciji `toUpperCase` ne prosleđuje nikakve argumente, funkcija na neki način ima pristup stringu `"Doh"`, vrednosti čije svojstvo smo pozvali. Saznaćete kako ovo funkcioniše u [Poglavlju ?](object#obj_methods).

Svojstva koja sadrže funkcije, obično se nazivaju _metodama_ vrednosti kojoj pripadaju, dakle "`toUpperCase` je metoda stringa".

{{id array_methods}}

Ovaj primer demonstrira dve metode koje možete koristiti za manipulaciju nizovima:

```
let sequence = [1, 2, 3];
sequence.push(4);
sequence.push(5);
console.log(sequence);
// → [1, 2, 3, 4, 5]
console.log(sequence.pop());
// → 5
console.log(sequence);
// → [1, 2, 3, 4]
```

{{index collection, array, "push method", "pop method"}}

`push` metoda dodaje vrednosti na kraj niza. Metoda `pop` radi suprotno, uklanja poslednju vrednost u nizu i vraća je.

{{index ["data structure", stack]}}

Ovi pomalo smešni nazivi su tradicionalni termini za operacije na _((steku))_. Stek, u programiranju, je struktura podataka koja vam omogućava da gurate vrednosti u nju i izvlačite ih ponovo u obrnutom redosledu, tako da se ono što je dodato poslednje uklanja se prvo. Stekovi su česti u programiranju — možda se sećate ((call stack))-a iz [prethodnog poglavlja](functions#stack), što je instanca iste ideje.

## Objekti

{{index journal, "weresquirrel example", array, record}}

Vratimo se veverici - čoveku. Skup dnevnih unosa može se predstaviti kao niz, ali unosi se ne sastoje samo od broja ili stringa — svaki unos mora da čuva listu aktivnosti i boolean vrednost koja označava da li se Žak pretvorio u vevericu ili ne. Idealno bi bilo da ove vrednosti grupišemo zajedno u jednu vrednost, a zatim te grupisane vrednosti stavimo u niz dnevnika unosa.

{{index [syntax, object], [property, definition], [braces, object], "{} (object)"}}

Vrednosti koje su tipa _((objekat))_ su suštinski kolekcija svojstava. Jedan način da kreirate objekat je korištenjem vitičastih zagrada kao izraz:

```
let day1 = {
  squirrel: false,
  events: ["work", "touched tree", "pizza", "running"]
};
console.log(day1.squirrel);
// → false
console.log(day1.wolf);
// → undefined
day1.wolf = false;
console.log(day1.wolf);
// → false
```

{{index [quoting, "of object properties"], "colon character"}}

Unutar vitičastih zagrada, pišete listu svojstava razdvojenih zarezima. Svako svojstvo ima ime, zatim dvotačku i vrednost. Kada se objekat piše preko više linija, uvlačenje kao što je prikazano u ovom primeru pomaže u čitljivosti. Svojstva čija imena nisu validna imena varijabli ili validni brojevi moraju biti u navodnicima:

```
let descriptions = {
  work: "Went to work",
  "touched tree": "Touched a tree"
};
```

{{index [braces, object]}}

Ovo znači da vitičaste zagrade imaju _dva_ značenja u JavaScript-u. Na početku ((izjave)), one započinju ((blok)) izjava. Na bilo kojoj drugoj poziciji, opisuju objekat. Srećom, retko je korisno započeti izjavu sa objektom u vitičastim zagradama, tako da nedoslednost između ovo dvoje nije veliki problem. Jedan slučaj kada se ovo pojavi je kada želite da vratite objekat iz skraćene funkcije sa strelicom—ne možete napisati `n => {prop: n}`, jer će se vitičaste zagrade tumačiti kao telo funkcije. Umesto toga, morate staviti skup zagrada oko objekta da biste jasno naznačili da je to izraz.

{{index undefined}}

Čitanje svojstva koje ne postoji će vam dati vrednost `undefined`.

{{index [property, assignment], mutability, "= operator"}}

Moguće je dodeliti vrednost izrazu svojstva operatorom `=`. Ovo će zameniti vrednost svojstva ako već postoji ili kreirati novo svojstvo na objektu ako ne postoji.

{{index "tentacle (analogy)", [property, "model of"], [binding, "model of"]}}

Da se kratko vratimo na naš model ((vezivanja)) — veza sa svojstvima je slična. One _hvataju_ vrednosti, ali druge veze i svojstva mogu držati te iste vrednosti. Možete zamisliti objekte kao hobotnice sa bilo kojim brojem pipaka, pri čemu svaki ima ime napisano na njemu.

{{index "delete operator", [property, deletion]}}

Operator `delete` otkida pipak sa takve hobotnice. To je unarni operator koji, kada se primeni na svojstvo objekta, ukloniće nazvano svojstvo iz objekta. Ovo nije uobičajena radnja, ali je moguća.

```
let anObject = {left: 1, right: 2};
console.log(anObject.left);
// → 1
delete anObject.left;
console.log(anObject.left);
// → undefined
console.log("left" in anObject);
// → false
console.log("right" in anObject);
// → true
```

{{index "in operator", [property, "testing for"], object}}

Binarni operator `in`, kada se primeni na string i objekat, govori vam da li taj objekat ima svojstvo sa tim imenom. Razlika između postavljanja svojstva na `undefined` i stvarnog brisanja je u tome što u prvom slučaju objekat još uvek _ima_ svojstvo (samo nema veoma zanimljivu vrednost), dok u drugom slučaju svojstvo više nije prisutno i `in` će vratiti `false`.

{{index "Object.keys function"}}

Da biste saznali koja sve svojstva objekat ima, možete koristiti funkciju `Object.keys`. Dajte funkciji objekat i ona će vratiti niz stringova—imenа svojstava objekta:

```
console.log(Object.keys({x: 0, y: 0, z: 2}));
// → ["x", "y", "z"]
```

Postoji funkcija `Object.assign` koja kopira sva svojstva iz jednog objekta u drugi:

```
let objectA = {a: 1, b: 2};
Object.assign(objectA, {b: 3, c: 4});
console.log(objectA);
// → {a: 1, b: 3, c: 4}
```

{{index array, collection}}

Nizovi su samo vrsta objekata specijalizovana za čuvanje sekvenci stvari. Ako evaluirate `typeof []`, proizvodi `"object"`. Možete vizualizovati nizove kao dugačke, ravne hobotnice sa svim svojim pipcima u urednom redu, označenim brojevima.

{{index journal, "weresquirrel example"}}

Naš prijatelj Žak će predstaviti dnevnik koji vodi kao niz objekata:

```{test: wrap}
let journal = [
  {events: ["work", "touched tree", "pizza",
            "running", "television"],
   squirrel: false},
  {events: ["work", "ice cream", "cauliflower",
            "lasagna", "touched tree", "brushed teeth"],
   squirrel: false},
  {events: ["weekend", "cycling", "break", "peanuts",
            "beer"],
   squirrel: true},
  /* and so on... */
];
```

## Promenjivost

Uskoro ćemo preći na stvarno programiranje, ali prvo, postoji još jedan teorijski deo koji treba razumeti.

{{index mutability, "side effect", number, string, Boolean, [object, mutability]}}

Videli smo da se vrednosti objekata mogu menjati. Tipovi vrednosti o kojima je bilo reči u ranijim poglavljima, kao što su brojevi, stringovi i Booleans, su svi _((nepromenljivi))_ — nemoguće je promeniti vrednosti tih tipova. Možete ih kombinovati i izvoditi nove vrednosti iz njih, ali kada uzmete određenu string vrednost, ta vrednost će uvek ostati ista. Tekst unutar nje ne može se promeniti. Ako imate string koji sadrži `"pas"`, nije moguće da drugi kod promeni karakter u vašem stringu da bi ga pretvorio u `"pad"`.

Objekti rade drugačije. _Možete_ promeniti njihova svojstva, uzrokujući da jedna objektna vrednost ima različiti sadržaj u različito vreme.

{{index [object, identity], identity, [memory, organization], mutability}}

Kada imamo dva broja, 120 i 120, možemo ih smatrati tačno istim brojem, bez obzira da li se odnose na iste fizičke bitove. Sa objektima, postoji razlika između imanja dva referenciranja na isti objekat i imanja dva različita objekta koji sadrže ista svojstva. Razmotrite sledeći kod:

```
let object1 = {value: 10};
let object2 = object1;
let object3 = {value: 10};

console.log(object1 == object2);
// → true
console.log(object1 == object3);
// → false

object1.value = 15;
console.log(object2.value);
// → 15
console.log(object3.value);
// → 10
```

{{index "tentacle (analogy)", [binding, "model of"]}}

Varijable `object1` i `object2` drže _isti_ objekat, zbog čega promena `object1` takođe menja vrednost `object2`. Kaže se da imaju isti _identitet_. Vezivanje `object3` pokazuje na drugačiji objekat, koji inicialno sadrži ista svojstva kao `object1`, ali živi odvojenim životom.

{{index "const keyword", "let keyword", [binding, "as state"]}}

Varijable takođe mogu biti promenljive ili konstantne, ali ovo je odvojeno od načina na koji se njihove vrednosti ponašaju. Iako se vrednosti brojeva ne menjaju, možete koristiti `let` varijablu da pratite promenljiv broj menjanjem vrednosti na koju varijabla pokazuje. Slično tome, iako `const` varijabla za objekat ne može biti promenjena i nastaviće da pokazuje na isti objekat, _sadržaj_ tog objekta može da se menja.

```{test: no}
const score = {visitors: 0, home: 0};
// Ovo je uredu
score.visitors = 1;
// Ovo nije dozvoljeno
score = {visitors: 1, home: 1};
```

{{index "== operator", [comparison, "of objects"], "deep comparison"}}

Kada poredite objekte pomoću `==` operatora u JavaScript-u, oni se porede po identitetu: proizvešće `true` samo ako su oba objekta tačno ista vrednost. Upoređivanje različitih objekata će vratiti `false`, čak i ako imaju identična svojstva. Zamislite da poredite osobe na osnovu svojstva imena i prezimena. Iako dve osobe mogu imati identično ime i prezime, to mogu biti dve potpuno različite osobe. Ne postoji "duboka" operacija poređenja ugrađena u JavaScript koja upoređuje objekte po sadržaju, ali je moguće da je napišete sami (što je jedan od [zadataka](data#exercise_deep_compare) na kraju ovog poglavlja).

## Dnevnik vukodlaka

{{index "weresquirrel example", lycanthropy, "addEntry function"}}

Žak pokreće svoj JavaScript interpreter i postavlja okruženje koje mu je potrebno da čuva svoj ((dnevnik)):

```{includeCode: true}
let journal = [];

function addEntry(events, squirrel) {
  journal.push({events, squirrel});
}
```

{{index [braces, object], "{} (object)", [property, definition]}}

Napomena da objekat dodat u dnevnik izgleda malo čudno. Umesto deklarisanja svojstava kao `events: events`, samo daje ime svojstva: `events`. Ovo je skraćenica koja znači istu stvar — ako ime svojstva u notaciji vitičastih zagrada nije praćeno vrednošću, njegova vrednost se uzima iz varijable sa istim imenom.

Svake večeri u 22 časa — ili ponekad sledećeg jutra, nakon što siđe sa gornje police svoje biblioteke — Žak zabeležava dan:

```
addEntry(["work", "touched tree", "pizza", "running",
          "television"], false);
addEntry(["work", "ice cream", "cauliflower", "lasagna",
          "touched tree", "brushed teeth"], false);
addEntry(["weekend", "cycling", "break", "peanuts",
          "beer"], true);
```

Kada prikupi dovoljno podataka, Žak namerava da koristi statistiku kako bi saznao koje od ovih događaja mogu biti povezani sa pretvaranjem u vevericu.

{{index correlation}}

_Korelacija_ je mera _zavisnosti_ između statističkih promenljivih. Statistička promenljiva nije baš isto što i programerska promenljiva. U statistici obično imate skup _merenja_, i svaka promenljiva se meri za svako merenje. Korelacija između promenljivih obično se izražava kao vrednost koja varira od -1 do 1. Nula korelacija znači da promenljive nisu povezane. Korelacija od 1 ukazuje da su dve promenljive savršeno povezane — ako znate jednu, znate i drugu. Negativna 1 takođe znači da su promenljive savršeno povezane ali su suprotnosti — kada je jedna tačna, druga je netačna.

{{index "phi coefficient"}}

Da bismo izračunali meru korelacije između dve booleovske promenljive, možemo koristiti _phi koeficijent_ (_ϕ_). Ovo je formula čiji je ulaz _tabela frekvencija_ koja sadrži broj puta kada su različite kombinacije promenljivih posmatrane. Izlaz formule je broj između -1 i 1 koji opisuje korelaciju.

Mogli bismo uzeti događaj jedenja ((pice)) i staviti ga u tabelu frekvencija ovako, gde svaki broj označava broj puta kada se ta kombinacija pojavila u našim merenjima.

{{figure {url: "img/pizza-squirrel.svg", alt: "Tabela dva sa dva koja prikazuje promenljivu za pizzu na horizontalnoj, a promenljivu za vevericu na vertikalnoj osi. Svaka ćelija pokazuje koliko puta se ta kombinacija pojavila. U 76 slučajeva, ni jedna nije bila istinita. U 9 slučajeva, samo je pica bila istinita. U 4 slučaja, samo je veverica bila istinita. I u jednom slučaju su se oba desila.", width: "7cm"}}}

Ako nazovemo tu tabelu _n_, možemo izračunati _ϕ_ koristeći sledeću formulu:

{{if html

<div> <table style="border-collapse: collapse; margin-left: 1em;"><tr>   <td style="vertical-align: middle"><em>ϕ</em> =</td>   <td style="padding-left: .5em">     <div style="border-bottom: 1px solid black; padding: 0 7px;"><em>n</em><sub>11</sub><em>n</em><sub>00</sub> −       <em>n</em><sub>10</sub><em>n</em><sub>01</sub></div>     <div style="padding: 0 7px;">√<span style="border-top: 1px solid black; position: relative; top: 2px;">       <span style="position: relative; top: -4px"><em>n</em><sub>1•</sub><em>n</em><sub>0•</sub><em>n</em><sub>•1</sub><em>n</em><sub>•0</sub></span>     </span></div>   </td> </tr></table> </div>

if}}

{{if tex

[\begin{equation}\varphi = \frac{n_{11}n_{00}-n_{10}n_{01}}{\sqrt{n_{1\bullet}n_{0\bullet}n_{\bullet1}n_{\bullet0}}}\end{equation}]{latex}

if}}

(Ako u ovom trenutku ostavljate knjigu po strani da se fokusirate na užasni flashback iz matematike u 2. razredu srednje — držite se! Ne nameravam da vas mučim beskrajnim stranicama kriptične notacije — samo je ova jedna formula sada. I čak i sa ovom jednom, sve što radimo je pretvaranje u JavaScript.)

Oznaka [_n_~01~]{if html}[[$n_{01}$]{latex}]{if tex} označava broj merenja gde je prva promenljiva (veveričenost) lažna (0) i druga promenljiva (pica) istinita (1). U tabeli za picu, [_n_~01~]{if html}[[$n_{01}$]{latex}]{if tex} je 9.

Vrednost [_n_~1•~]{if html}[[$n_{1\bullet}$]{latex}]{if tex} se odnosi na zbir svih merenja gde je prva promenljiva istinita, što je 5 u primeru tabele. Slično tome, [_n_~•0~]{if html}[[$n_{\bullet0}$]{latex}]{if tex} se odnosi na zbir merenja gde je druga promenljiva lažna.

{{index correlation, "phi coefficient"}}

Dakle, za tabelu za picu, deo iznad linije deljenja (deljenik) bio bi 1×76−4×9 = 40, a deo ispod nje (delilac) bio bi kvadratni koren od 5×85×10×80, ili [√340,000]{if html}[[$\sqrt{340,000}$]{latex}]{if tex}. To iznosi _ϕ_ ≈ 0.069, što je veoma malo. Jedenje ((pice)) se čini da nema uticaja na transformacije.

## Računanje korelacije

{{index [array, "as table"], [nesting, "of arrays"]}}

Dva puta dva ((tabelu)) možemo predstaviti u JavaScript-u pomoću četvoroelementnog niza (`[76, 9, 4, 1]`). Takođe možemo koristiti i druge reprezentacije, poput niza koji sadrži dva niza od po dva elementa (`[[76, 9], [4, 1]]`) ili objekat sa imenima svojstava poput `"11"` i `"01"`, ali ravni niz je jednostavan i čini izraze koji pristupaju tabeli prijatno kratkim. Tumačićemo indekse niza kao dvo-((bitne)) ((binarne brojeve)), gde se levi (najznačajniji) bit odnosi na promenljivu za vevericu, a desni (najmanje značajan) bit se odnosi na promenljivu za događaj. Na primer, binarni broj `10` se odnosi na slučaj kada se Žak pretvorio u vevericu, ali se događaj (recimo, "pica") nije dogodio. To se dogodilo četiri puta. A pošto binarni `10` predstavlja broj 2 u decimalnom zapisu, ovaj broj ćemo sačuvati na indeksu 2 u nizu.

{{index "phi coefficient", "phi function"}}

{{id phi_function}}

Ovo je funkcija koja izračunava koeficijent _ϕ_ iz takvog niza:

```{includeCode: strip_log, test: clip}
function phi(table) {
  return (table[3] * table[0] - table[2] * table[1]) /
    Math.sqrt((table[2] + table[3]) *
              (table[0] + table[1]) *
              (table[1] + table[3]) *
              (table[0] + table[2]));
}

console.log(phi([76, 9, 4, 1]));
// → 0.068599434
```

{{index "square root", "Math.sqrt function"}}

Ovo je direktna konverzija _ϕ_ formule u JavaScript. `Math.sqrt` je funkcija korena kvadratnog, koja je obezbeđena od strane `Math` objekta u standardnom JavaScript okruženju. Moramo dodati dva polja iz tabele kako bismo dobili polja poput [n~1•~]{if html}[[$n_{1\bullet}$]{latex}]{if tex} jer sume redova ili kolona nisu direktno sačuvane u našoj strukturi podataka.

{{index "JOURNAL data set"}}

Žak vodi svoj dnevnik tri meseca. Rezultirajući ((skup podataka)) dostupan je u [sanboxu za kodiranje](https://eloquentjavascript.net/code#4) za ovo poglavlje[ ([_https://eloquentjavascript.net/code#4_](https://eloquentjavascript.net/code#4))]{if book}, gde je sačuvan u varijabli `JOURNAL`, i u [fajlu](https://eloquentjavascript.net/code/journal.js) za preuzimanje.

{{index "tableFor function"}}

Da bismo izvukli dvobajtnu ((tabelu)) za određeni događaj iz dnevnika, moramo proći kroz sve unose i prebrojati koliko puta se događaj pojavljuje u odnosu na transformacije u vevericu:

```{includeCode: strip_log}
function tableFor(event, journal) {
  let table = [0, 0, 0, 0];
  for (let i = 0; i < journal.length; i++) {
    let entry = journal[i], index = 0;
    if (entry.events.includes(event)) index += 1;
    if (entry.squirrel) index += 2;
    table[index] += 1;
  }
  return table;
}

console.log(tableFor("pizza", JOURNAL));
// → [76, 9, 4, 1]
```

{{index [array, searching], "includes method"}}

Nizovi imaju `includes` metod koji proverava da li određena vrednost postoji u nizu. Funkcija koristi to da bi utvrdila da li je ime događaja koji je zanima deo liste događaja za određeni dan.

{{index [array, indexing]}}

Telo petlje u `tableFor` određuje u koju kutiju u tabeli svaki unos u dnevnik pada proverom da li unos sadrži određeni događaj koji ga zanima i da li se događaj dešava zajedno sa incidentom veverice. Zatim petlja dodaje jedan u odgovarajuću kutiju u tabeli.

Sada imamo alate koji su nam potrebni da izračunamo pojedinačne ((korelacije)). Jedini korak koji preostaje je da pronađemo korelaciju za svaki tip događaja koji je zabeležen i vidimo da li se nešto ističe.

{{id for_of_loop}}

## Petlje za nizove

{{index "for loop", loop, [array, iteration]}}

U funkciji `tableFor`, postoji ova petlja:

```
for (let i = 0; i < JOURNAL.length; i++) {
  let entry = JOURNAL[i];
  // Uradi nesto sa unosom
}
```

Ovaj tip petlje je čest u klasičnom JavaScript-u — prolazak kroz nizove po jedan element je nešto što se često javlja, i da biste to uradili, koristili biste brojač dužine niza i izabrali svaki element redom.

Postoji jednostavniji način pisanja takvih petlji u modernom JavaScript-u:

```
for (let entry of JOURNAL) {
  console.log(`${entry.events.length} events.`);
}
```

{{index "for/of loop"}}

Kada `for` petlja koristi reč `of` nakon definicije svoje promenljive, petlja će prolaziti kroz elemente vrednosti date posle `of`. Ovo radi ne samo za nizove već i za stringove i neke druge strukture podataka. Razmotrićemo _kako_ to funkcioniše u [Poglavlje ?](object).

{{id analysis}}

## Finalna analiza

{{index journal, "weresquirrel example", "journalEvents function"}}

Potrebno je izračunati korelaciju za svaki tip događaja koji se pojavljuje u skupu podataka. Da bismo to uradili, prvo moramo _pronaći_ svaki tip događaja.

{{index "includes method", "push method"}}

```{includeCode: "strip_log"}
function journalEvents(journal) {
  let events = [];
  for (let entry of journal) {
    for (let event of entry.events) {
      if (!events.includes(event)) {
        events.push(event);
      }
    }
  }
  return events;
}

console.log(journalEvents(JOURNAL));
// → ["carrot", "exercise", "weekend", "bread", …]
```

Dodavanjem svih imena događaja koji već nisu u nizu `events`, funkcija prikuplja svaki tip događaja.

Koristeći tu funkciju, možemo videti sve ((korelacije)):

```{test: no}
for (let event of journalEvents(JOURNAL)) {
  console.log(event + ":", phi(tableFor(event, JOURNAL)));
}
// → carrot:   0.0140970969
// → exercise: 0.0685994341
// → weekend:  0.1371988681
// → bread:   -0.0757554019
// → pudding: -0.0648203724
// itd...
```

Većina korelacija izgleda da leže blizu nule. Čini se da konzumiranje šargarepe, hleba ili pudinga očigledno ne pokreće veveričju likantropiju. Transformacije se, čini se dešavaju nešto češće vikendom. Filtrirajmo rezultate da prikažemo samo korelacije veće od 0.1 ili manje od -0.1:

```{test: no, startCode: true}
for (let event of journalEvents(JOURNAL)) {
  let correlation = phi(tableFor(event, JOURNAL));
  if (correlation > 0.1 || correlation < -0.1) {
    console.log(event + ":", correlation);
  }
}
// → weekend:        0.1371988681
// → brushed teeth: -0.3805211953
// → candy:          0.1296407447
// → work:          -0.1371988681
// → spaghetti:      0.2425356250
// → reading:        0.1106828054
// → peanuts:        0.5902679812
```

Aha! Postoje dva faktora sa korelacijom jasno jačom od ostalih. Konzumiranje ((kikirikija)) ima snažan pozitivan uticaj na šansu da se pretvori u vevericu, dok četkanje zuba ima značajan negativan efekat.

Interesantno. Hajde da pokušamo nešto:

```
for (let entry of JOURNAL) {
  if (entry.events.includes("peanuts") &&
     !entry.events.includes("brushed teeth")) {
    entry.events.push("peanut teeth");
  }
}
console.log(phi(tableFor("peanut teeth", JOURNAL)));
// → 1
```

Ovo je snažan rezultat. Fenomen se dešava tačno kada Žak jede ((kikiriki)) i propusti da opere zube. Da nije toliko nemaran po pitanju dentalne higijene, nikada ne bi ni primetio svoje oboljenje.

Znajući ovo, Žak potpuno prestaje da jede kikiriki i primeti da se njegove transformacije zaustavljaju.

{{index "weresquirrel example"}}

Ali treba mu samo nekoliko meseci da primeti da nešto nedostaje iz ovog potpuno ljudskog načina života. Bez svojih divljih avantura, Žak se jedva oseća živim. Odlučuje da radije bude stalno divlja životinja. Nakon što izgradi prelepu malu kućicu na drvetu u šumi i opremi je dozatorom kikiriki putera i desetogodišnjim zalihama kikirikija, poslednji put menja oblik i živi kratki i energični život veverice.

## Terminologija nizova

{{index [array, methods], [method, array]}}

Pre nego što završim poglavlje, želim da vas upoznam sa još nekoliko koncepata vezanih za objekte. Počeću tako što ću predstaviti nekoliko općenito korisnih metoda nizova.

{{index "push method", "pop method", "shift method", "unshift method"}}

Videli smo `push` i `pop`, koji dodaju i uklanjaju elemente na kraju niza, [ranije](data#array_methods) u ovom poglavlju. Odgovarajuće metode za dodavanje i uklanjanje stvari na početku niza nazivaju se `unshift` i `shift`.

```
let todoList = [];
function remember(task) {
  todoList.push(task);
}
function getTask() {
  return todoList.shift();
}
function rememberUrgently(task) {
  todoList.unshift(task);
}
```

{{index "task management example"}}

Ovaj program upravlja redom zadataka. Zadatke dodajete na kraj reda pozivom `remember("groceries")`, a kada ste spremni da nešto uradite, pozivate `getTask()` da biste dobili (i uklonili) prvi element iz reda. Funkcija `rememberUrgently` takođe dodaje zadatak, ali ga dodaje na početak umesto na kraj reda.

{{index [array, searching], "indexOf method", "lastIndexOf method"}}

Za pretragu određene vrednosti, nizovi pružaju `indexOf` metodu. Ova metoda pretražuje niz od početka do kraja i vraća indeks na kojem je pronađena tražena vrednost — ili -1 ako nije pronađena. Za pretragu od kraja umesto od početka, postoji slična metoda koja se zove `lastIndexOf`:

```
console.log([1, 2, 3, 2, 1].indexOf(2));
// → 1
console.log([1, 2, 3, 2, 1].lastIndexOf(2));
// → 3
```

I `indexOf` i `lastIndexOf` uzimaju opcioni drugi argument koji označava odakle počinje pretraga.

{{index "slice method", [array, indexing]}}

Još jedan osnovni metod nizova je `slice`, koji uzima početni i krajnji indeks i vraća niz koji sadrži samo elemente između njih. Početni indeks je uključen, a krajnji indeks je isključen.

```
console.log([0, 1, 2, 3, 4].slice(2, 4));
// → [2, 3]
console.log([0, 1, 2, 3, 4].slice(2));
// → [2, 3, 4]
```

{{index [string, indexing]}}

Kada krajnji indeks nije dat, `slice` će uzeti sve elemente posle početnog indeksa. Takođe možete izostaviti početni indeks da biste kopirali ceo niz.

{{index concatenation, "concat method"}}

Metoda `concat` može se koristiti za spajanje nizova kako bi se stvorio novi niz, slično tome kako operator `+` radi za stringove.

U sledećem primeru su prikazane i `concat` i `slice` u akciji. U ovom primeru uzimamo niz i indeks i vraća novi niz koji je kopija originalnog niza sa elementom na datom indeksu uklonjenim:

```
function remove(array, index) {
  return array.slice(0, index)
    .concat(array.slice(index + 1));
}
console.log(remove(["a", "b", "c", "d", "e"], 2));
// → ["a", "b", "d", "e"]
```

Ako metodi `concat` prosledite argument koji nije niz, ta vrednost će biti dodata u novi niz kao da je niz od jednog elementa.

## Stringovi i njihova svojstva

{{index [string, properties]}}

Možemo pročitati svojstva iz stringova poput `length` i `toUpperCase`, ali ne možemo im dodati novo svojstvo.

```
let kim = "Kim";
kim.age = 88;
console.log(kim.age);
// → undefined
```

Vrednosti tipa string, number i boolean nisu objekti, i iako jezik ne prigovara ako pokušate postaviti nove osobine na njih, zapravo ne čuva te osobine. Kao što je ranije pomenuto, ovi tipovi vrednosti su nepromenljivi.

{{index [string, methods], "slice method", "indexOf method", [string, searching]}}

Ali ovi tipovi imaju ugrađene osobine koje dolaze uz njih. Svaka vrednost stringa ima određeni broj metoda. Neke veoma korisne su `slice` i `indexOf`, koje podsećaju na metode niza istog imena:

```
console.log("coconuts".slice(4, 7));
// → nut
console.log("coconut".indexOf("u"));
// → 5
```

Jedna razlika je što `indexOf` stringa može tražiti string koji sadrži više od jednog karaktera, dok odgovarajuća metoda niza traži samo pojedinačni element:

```
console.log("one two three".indexOf("ee"));
// → 11
```

{{index [whitespace, trimming], "trim method"}}

Metoda `trim` uklanja whitespace karaktere (razmake, nove linije, tabulacije i slične karaktere) sa početka i kraja stringa:

```
console.log("  okay \n ".trim());
// → okay
```

{{id padStart}}

Funkcija `zeroPad` iz [prethodnog poglavlja](functions) takođe postoji kao metoda. Zove se `padStart` i prima željenu dužinu i karakter za dopunu kao argumente:

```
console.log(String(6).padStart(3, "0"));
// → 006
```

{{id split}}

{{index "split method"}}

Možete razdvojiti string na svako pojavljivanje drugog podstringa pomoću `split` i ponovo ga spojiti sa `join`:

```
let sentence = "Secretarybirds specialize in stomping";
let words = sentence.split(" ");
console.log(words);
// → ["Secretarybirds", "specialize", "in", "stomping"]
console.log(words.join(". "));
// → Secretarybirds. specialize. in. stomping
```

{{index "repeat method"}}

String se može ponoviti pomoću metode `repeat`, koja stvara novi string koji sadrži višestruke kopije originalnog stringa, zalepljene zajedno:

```
console.log("LA".repeat(3));
// → LALALA
```

{{index ["length property", "for string"], [string, indexing]}}

Već smo videli svojstvo `length` za tip string. Pristupanje pojedinačnim karakterima u stringu izgleda kao pristupanje elementima niza (sa komplikacijom o kojoj ćemo diskutovati u [Poglavlju ?](higher_order#code_units)).

```
let string = "abc";
console.log(string.length);
// → 3
console.log(string[1]);
// → b
```

{{id rest_parameters}}

## Rest parametar

{{index "Math.max function", "period character", "max example", spread, [array, "of rest arguments"]}}

Može biti korisno da funkcija prihvati bilo koji broj argumenata. Na primer, `Math.max` računa maksimum _svih_ argumenata koje dobije. Da biste napisali takvu funkciju, stavite tri tačke pre poslednjeg ((parametra)) funkcije, na ovaj način:

```{includeCode: strip_log}
function max(...numbers) {
  let result = -Infinity;
  for (let number of numbers) {
    if (number > result) result = number;
  }
  return result;
}
console.log(max(4, 1, 9, -2));
// → 9
```

Kada se takva funkcija pozove, _((rest parametar))_ je vezan za niz koji sadrži sve dalje argumente. Ako postoje drugi parametri pre njega, njihove vrednosti nisu deo tog niza. Kada je, kao u `max`, jedini parametar, sadržaće sve argumente.

{{index [function, application]}}

Možete koristiti sličnu notaciju sa tri tačke i za _poziv_ funkcije sa nizom argumenata:

```
let numbers = [5, 1, 7];
console.log(max(...numbers));
// → 7
```

Ovo "((spread))uje" (raširuje) niz u poziv funkcije, prosleđujući njegove elemente kao odvojene argumente. Moguće je uključiti takav niz zajedno sa drugim argumentima, kao u `max(9, ...numbers, 2)`.

{{index "[] (array)"}}

Notacija niza uglastih zagrada takođe omogućava trostruki operator tačaka da "((spread))uje" drugi niz u novi niz:

```
let words = ["never", "fully"];
console.log(["will", ...words, "understand"]);
// → ["will", "never", "fully", "understand"]
```

{{index "{} (object)"}}

Ovo radi čak i u objektima vitičastih zagrada, gde dodaje sve osobine iz drugog objekta. Ako se ista osobina dodaje više puta, poslednja dodata vrednost pobeđuje:

```
let coordinates = {x: 10, y: 0};
console.log({...coordinates, y: 5, z: 1});
// → {x: 10, y: 5, z: 1}
```

## Math objekat

{{index "Math object", "Math.min function", "Math.max function", "Math.sqrt function", minimum, maximum, "square root"}}

Kao što smo videli, `Math` je kutija alatki za funkcije vezane za rad sa brojevima kao što su `Math.max` (maksimum), `Math.min` (minimum) i `Math.sqrt` (kvadratni koren).

{{index namespace, [object, property]}}

{{id namespace_pollution}}

Objekat `Math` se koristi kao kontejner za grupisanje velikog broja povezanih funkcionalnosti. Postoji samo jedan objekat `Math`, i gotovo nikada nije koristan kao vrednost. Umesto toga, pruža _namespace_ tako da sve ove funkcije i vrednosti ne moraju biti globalne varijable.

{{index [binding, naming]}}

Imati previše globalnih varijabli "zagađuje" namespace. Što više imena bude zauzeto, veća je verovatnoća da ćete slučajno prepisati vrednost neke postojeće veze. Na primer, nije nemoguće da ćete poželeti da nazovete nešto `max` u jednom od svojih programa. Budući da je ugrađena JavaScript-ova funkcija `max` sigurno smeštena unutar objekta `Math`, ne morate da brinete o tome da ćete napisati svoju funkciju preko nje, i time obisati JavaScript `max` iz svog programa.

{{index "let keyword", "const keyword"}}

Mnogi jezici će vas zaustaviti, ili bar upozoriti, kada definišete varijablu, funkciju ili objekat sa imenom koje već postoji. JavaScript to radi za bindinge koje ste deklarisali sa `let` ili `const`, ali paradoksalno ne radi to za standardne bindinge niti za bindinge deklarisane sa `var` ili `function`.

{{index "Math.cos function", "Math.sin function", "Math.tan function", "Math.acos function", "Math.asin function", "Math.atan function", "Math.PI constant", cosine, sine, tangent, "PI constant", pi}}

Povratak na `Math` objekat. Ako vam je potrebna trigonometrija, `Math` može pomoći. Sadrži `cos` (kosinus), `sin` (sinus) i `tan` (tangens), kao i njihove inverzne funkcije, `acos`, `asin` i `atan`, redom. Broj π (pi) - ili barem najbliža aproksimacija koja stane u JavaScript broj - dostupan je kao `Math.PI`. Stara programerska tradicija je da se imena ((konstantnih)) vrednosti pišu velikim slovima:

```{test: no}
function randomPointOnCircle(radius) {
  let angle = Math.random() * 2 * Math.PI;
  return {x: radius * Math.cos(angle),
          y: radius * Math.sin(angle)};
}
console.log(randomPointOnCircle(2));
// → {x: 0.3667, y: 1.966}
```

Ako niste upoznati sa sinusima i kosinusima, nemojte brinuti. Objasniću ih kada se budu koristili u ovoj knjizi, u [Poglavlju ?](dom#sin_cos).

{{index "Math.random function", "random number"}}

Prethodni primer koristio je `Math.random`. Ovo je funkcija koja vraća novi pseudo - radnom (nasumičan, slučajan) broj između nula (uključivo) i jedan (isključivo) svaki put kada je pozovete:

```{test: no}
console.log(Math.random());
// → 0.36993729369714856
console.log(Math.random());
// → 0.727367032552138
console.log(Math.random());
// → 0.40180766698904335
```

{{index "pseudorandom number", "random number"}}

Iako su računari determinističke mašine - uvek reaguju na isti način ako im se pruži isti ulaz - moguće je da proizvedu brojeve koji deluju nasumično. Da bi to postigao, računar čuva neku skrivenu vrednost, i svaki put kada zatražite novi nasumičan broj, izvodi komplikovane računske operacije nad ovom skrivenom vrednošću da bi stvorio novu vrednost. Čuva novu vrednost i vraća neki broj izveden iz nje. Na taj način može proizvoditi nove, teško predvidive brojeve na način koji _izgleda_ nasumično.

{{index rounding, "Math.floor function"}}

Ako želimo ceo slučajni broj umesto decimalnog, možemo koristiti `Math.floor` (koji zaokružuje nadole na najbliži ceo broj) na rezultat `Math.random`:

```{test: no}
console.log(Math.floor(Math.random() * 10));
// → 2
```

Množenjem random broja sa 10 dobijamo broj veći ili jednak 0 i manji od 10. Pošto `Math.floor` zaokružuje nadole, ovaj izraz će proizvesti, sa jednakom šansom, bilo koji broj od 0 do 9.

{{index "Math.ceil function", "Math.round function", "Math.abs function", "absolute value"}}

Postoje i funkcije `Math.ceil` (za "plafon", koja zaokružuje na gore na ceo broj), `Math.round` (zaokružuje na najbliži ceo broj) i `Math.abs`, koja uzima apsolutnu vrednost broja, što znači da negira negativne vrednosti ali ostavlja pozitivne vrednosti kao takve.

## Destruktuisanje

{{index "phi function"}}

Vratimo se `phi` funkciji na momenat.

```{test: wrap}
function phi(table) {
  return (table[3] * table[0] - table[2] * table[1]) /
    Math.sqrt((table[2] + table[3]) *
              (table[0] + table[1]) *
              (table[1] + table[3]) *
              (table[0] + table[2]));
}
```

{{index "destructuring binding", parameter}}

Jedan razlog zašto je ova funkcija nezgodna za čitanje je taj što imamo ime koje pokazuje na naš niz, ali bismo radije imali imena za _elemente_ niza - to jest, `let n00 = table[0]` i tako dalje. Srećom, postoji sažet način da se to uradi u JavaScriptu:

```
function phi([n00, n01, n10, n11]) {
  return (n11 * n00 - n10 * n01) /
    Math.sqrt((n10 + n11) * (n00 + n01) *
              (n01 + n11) * (n00 + n10));
}
```

{{index "let keyword", "var keyword", "const keyword", [binding, destructuring]}}

Ovo takođe radi za varijable kreirane sa `let`, `var`, ili `const`. Ako znate da je vrednost koju vezujete tipa niz, možete koristiti ((uglaste zagrade)) da "zagledate unutar" vrednosti, vezujući njen sadržaj.

{{index [object, property], [braces, object]}}

Sličan trik radi i na objektima, korištenjem vitičastih zagrada umesto uglastih:

```
let {name} = {name: "Faraji", age: 23};
console.log(name);
// → Faraji
```

{{index null, undefined}}

Važno je napomenuti da ako pokušate da destrukturirate `null` ili `undefined`, dobićete grešku, baš kao i kada direktno pokušate da pristupite svojstvu tih vrednosti.

## Opcioni pristup osobinama

{{index "optional chaining", "period character"}}

Kada niste sigurni da li određena vrednost proizvodi objekat, ali ipak želite da pročitate osobinu iz nje u slučaju da jeste, možete koristiti varijantu tačkaste notacije: `object?.property`.

```
function city(object) {
  return object.address?.city;
}
console.log(city({address: {city: "Banja Luka"}}));
// → Banja Luka
console.log(city({name: "Vera"}));
// → undefined
```

Izraz `a?.b` znači isto što i `a.b` kada `a` nije `null` ili `undefined`. Kada jeste, evaluira se u `undefined`. Ovo može biti korisno kada, kao u primeru, niste sigurni da određena osobina postoji ili kada promenljiva može sadržati vrednost `undefined`.

Slična notacija može se koristiti i sa pristupom uglastim zagradama, čak i sa pozivima funkcija, stavljajući `?.` ispred zagrada ili uglastih zagrada:

```
console.log("string".notAMethod?.());
// → undefined
console.log({}.arrayProp?.[0]);
// → undefined
```

## JSON

{{index [array, representation], [object, representation], "data format", [memory, organization]}}

Zato što osobine objekta "hvataju" svoju vrednost umesto što je sadrže, objekti i nizovi se čuvaju u memoriji računara kao sekvence bitova koji drže _((adrese))_ - mesto u memoriji - njihovog sadržaja. "Hvatanje" vrednosti znači da se neka vrednost, recimo broj 120, nalazi na određenoj memorijskoj adresi u računaru, a osobina koja "pokazuje" na tu vrednost može biti na totalno drugoj adresi. Ta osobina samo treba da zna na kojoj memorijskoj adresi pronalazi vrednost na koju pokazuje. Niz sa drugim nizom unutar njega sastoji se (barem) od jedne memorijske regije za unutrašnji niz i još jedne za spoljni niz, koji sadrži (između ostalog) broj koji predstavlja memorijsku adresu unutrašnjeg niza.

Ako želite da sačuvate podatke u datoteku za kasnije ili ih pošaljete na drugi računar putem mreže, morate nekako pretvoriti ove gomile memorijskih adresa u opis koji može biti sačuvan ili poslat. _Mogli biste_ poslati celokupnu memoriju računara zajedno sa adresom vrednosti koja vas zanima, pretpostavljam, ali to ne izgleda kao najbolji pristup.

{{indexsee "JavaScript Object Notation", JSON}}

{{index serialization, "World Wide Web"}}

Ono što možemo uraditi je _serializovati_ podatke. Popularan format za serializaciju se zove _((JSON))_ (izgovara se "Džejson"), što predstavlja JavaScript Object Notation. Jako često se koristi kao format za skladištenje podataka i komunikaciju na vebu, čak i u jezicima koji nisu JavaScript. JSON je jedna od ključnih stvari koju trebate znati kao web developer.

{{index [array, notation], [object, creation], [quoting, "in JSON"], comment}}

JSON izgleda slično JavaScript-ovom načinu pisanja nizova i objekata, sa nekoliko ograničenja. Sva imena svojstava moraju biti okružena dvostrukim navodnicima, i dozvoljeni su samo jednostavni izrazi podataka - nema poziva funkcija, varijabli ili bilo čega što uključuje stvarno računanje. Komentari nisu dozvoljeni u JSON-u.

Ulaz u dnevnik može izgledati ovako kada je predstavljen kao JSON podatak:

```{lang: "json"}
{
  "squirrel": false,
  "events": ["work", "touched tree", "pizza", "running"]
}
```

{{index "JSON.stringify function", "JSON.parse function", serialization, deserialization, parsing}}

JavaScript nam pruža funkcije `JSON.stringify` i `JSON.parse` za konverziju podataka u ovaj format i iz ovog formata. Prva uzima JavaScript vrednost i vraća JSON enkodiran string. Druga uzima takav string i konvertuje ga u JavaScript vrednost koju enkodira:

```
let string = JSON.stringify({squirrel: false,
                             events: ["weekend"]});
console.log(string);
// → {"squirrel":false,"events":["weekend"]}
console.log(JSON.parse(string).events);
// → ["weekend"]
```

## Siže

Objekti i nizovi pružaju načine da se nekoliko vrednosti grupiše u jednu vrednost. To nam omogućava da stavimo grupu povezanih stvari u torbu i trčimo sa torbom umesto da obavijamo ruke oko svih pojedinačnih stvari i pokušavamo da ih držimo odvojeno.

Većina vrednosti u JavaScriptu ima osobine (properties), s izuzecima `null` i `undefined`. Osobine se pristupaju koristeći `value.prop` ili `value["prop"]`. Objekti obično koriste imena za svoje osobine i čuvaju više ili manje fiksni skup njih. Nizovi obično sadrže promenljive količine konceptualno identičnih vrednosti i koriste brojeve (počevši od 0) kao imena svojih osobina.

Postoje neke osobine sa imenom u nizovima, poput `length` i brojnih metoda. Metode su funkcije koje žive u osobinama i (obično) deluju na vrednost čija su osobina. Recimo `length` je osobina niza, i nalazi dužinu niza, jer je osobina te vrednosti.

Možete iterirati preko nizova koristeći poseban tip `for` petlje: `for (let element of array)`.

## Zadaci

### Zbir opsega

{{index "summing (exercise)"}}

U uvodu ove knjige je aludirano na sledeći kod kao lep način za izračunavanje zbira opsega brojeva:

```{test: no}
console.log(sum(range(1, 10)));
```

{{index "range function", "sum function"}}

Napišite funkciju `range` koja prima dva argumenta, `start` i `end`, i vraća niz koji sadrži sve brojeve od `start` do, i uključujući `end`.

Zatim, napišite funkciju `sum` koja prima niz brojeva i vraća zbir tih brojeva. Pokrenite primer programa i proverite da li zaista vraća 55.

{{index "optional argument"}}

Kao dodatni zadatak, modifikujte vašu funkciju `range` tako da prihvata opcioni treći argument koji označava "korak" vrednost korišćenu prilikom kreiranja niza. Ako korak nije dat, elementi bi trebalo da idu u koracima od jedan, što odgovara originalnom ponašanju. Poziv funkcije `range(1, 10, 2)` treba da vrati `[1, 3, 5, 7, 9]`. Dakle korak za koji se povećava svaki sledeći broj u ovom slučaju je 2. Proverite da li ovo takođe radi sa negativnim vrednostima koraka tako da `range(5, 2, -1)` proizvodi `[5, 4, 3, 2]`.

{{if interactive

```{test: no}
// Vas kod ovde.

console.log(range(1, 10));
// → [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
console.log(range(5, 2, -1));
// → [5, 4, 3, 2]
console.log(sum(range(1, 10)));
// → 55
```

if}}

{{hint

{{index "summing (exercise)", [array, creation], "square brackets"}}

Izgradnja niza se najlakše vrši prvo inicijalizovanjem varijable na `[]` (nov, prazan niz) i ponovnim pozivom njegove metode `push` da dodate vrednost. Ne zaboravite da vratite niz na kraju funkcije.

{{index [array, indexing], comparison}}

S obzirom da je gornja granica uključujuća, moraćete koristiti operator `<=` umesto `<` da biste proverili kraj vaše petlje.

{{index "arguments object"}}

Parametar koraka može biti opcioni parametar koji podrazumevano (koristeći operator `=`) ima vrednost 1.

{{index "range function", "for loop"}}

Verovatno je najbolje da `range` razume negativne vrednosti koraka tako što će se napisati dve odvojene petlje - jedna za brojanje nagore i jedna za brojanje nadole - jer poređenje koje proverava da li je petlja završena treba da bude `>=` umesto `<=` kada se broji nadole.

Takođe bi bilo korisno koristiti drugačiju podrazumevanu vrednost koraka, na primer, -1, kada je kraj opsega manji od početka. Na taj način, `range(5, 2)` vraća nešto smisleno, umesto da se zaglavi u beskonačnoj petlji. Moguće je pozvati se na prethodne parametre u podrazumevanoj vrednosti parametra.

hint}}

### Obrtanje redosleda niza

{{index "reversing (exercise)", "reverse method", [array, methods]}}

Nizovi imaju metod `reverse` koji menja niz tako što invertuje redosled pojavljivanja njegovih elemenata. Za ovu vežbu, napišite dve funkcije, `reverseArray` i `reverseArrayInPlace`. Prva, `reverseArray`, treba da uzme niz kao argument i proizvede _novi_ niz koji ima iste elemente u inverznom redosledu. Druga, `reverseArrayInPlace`, treba da radi ono što i metod `reverse`: _modifikuje_ niz dat kao argument tako što invertuje njegove elemente. Nijedna funkcija ne sme da koristi standardni metod `reverse`.

{{index efficiency, "pure function", "side effect"}}

Razmislite o napomenama o sporednim efektima i čistim funkcijama koje smo pominjali u [prethodnom poglavlju](functions#pure), koju varijantu očekujete da će biti korisna u više situacija? Koja varijanta se izvršava brže?

{{if interactive

```{test: no}
// Vas kod ovde.

let myArray = ["A", "B", "C"];
console.log(reverseArray(myArray));
// → ["C", "B", "A"];
console.log(myArray);
// → ["A", "B", "C"];
let arrayValue = [1, 2, 3, 4, 5];
reverseArrayInPlace(arrayValue);
console.log(arrayValue);
// → [5, 4, 3, 2, 1]
```

if}}

{{hint

{{index "reversing (exercise)"}}

Postoje dva očigledna načina za implementaciju `reverseArray`. Prvi je jednostavno prolazak kroz ulazni niz od početka do kraja i korišćenje metode `unshift` na novom nizu da bismo svaki element umetnuli na početak. Drugi način je prolazak kroz ulazni niz unazad i korišćenje metode `push`. Iteriranje kroz niz unazad zahteva (pomalo nezgodnu) specifikaciju `for` petlje, kao što je `(let i = array.length - 1; i >= 0; i--)`.

{{index "slice method"}}

Obrtanje niza na licu mesta je teže. Morate biti oprezni da ne prepišete elemente koje ćete kasnije trebati. Korišćenje `reverseArray` ili na bilo koji drugi način kopiranje celog niza (`array.slice()` je dobar način kopiranja niza) radi ali je varanje.

Trik je u tome da _zamenite_ prvi i poslednji element, zatim drugi i predposlednji, i tako dalje. Možete to uraditi prolaskom kroz polovinu dužine niza (koristite `Math.floor` da zaokružite nadole - nije potrebno da dodirnete srednji element u nizu sa neparnim brojem elemenata) i zamenom elementa na poziciji `i` sa onim na poziciji `array.length - 1 - i`. Možete koristiti lokalnu varijablu da privremeno zadržite jedan od elemenata, prepišete taj element njegovim parom, a zatim stavite vrednost iz lokalne varijable na mesto gde je njegov par nekada bio.

hint}}

{{id list}}

### Lista

{{index ["data structure", list], "list (exercise)", "linked list", array, collection}}

Kao generički blokovi vrednosti, objekti se mogu koristiti za izgradnju različitih struktura podataka. Česta struktura podataka je _lista_ (ne treba je mešati sa nizovima). Lista je ugnežđeni set objekata, pri čemu prvi objekat drži referencu na drugi, drugi na treći, i tako dalje:

```{includeCode: true}
let list = {
  value: 1,
  rest: {
    value: 2,
    rest: {
      value: 3,
      rest: null
    }
  }
};
```

Rezultirajući objekti formiraju lanac, kao što je prikazano na sledećem dijagramu:

{{figure {url: "img/linked-list.svg", alt: "Dijagram koji prikazuje strukturu memorije povezane liste. Postoje 3 ćelije, svaka sa poljem vrednosti koje sadrži broj, i poljem 'rest' sa strelicom ka ostatku liste. Strelica prve ćelije pokazuje na drugu ćeliju, strelica druge ćelije na poslednju ćeliju, a polje 'rest' poslednje ćelije sadrži null.",width: "8cm"}}}

{{index "structure sharing", [memory, structure sharing]}}

Lepa stvar kod listi je što mogu deliti delove svoje strukture. Na primer, ako kreiram dve nove vrednosti `{value: 0, rest: list}` i `{value: -1, rest: list}` (gde `list` referiše na vezu definisanu ranije), one su obe nezavisne liste, ali dele strukturu koja čini njihova poslednja tri elementa. Originalna lista i dalje predstavlja validnu listu od tri elementa.

Napišite funkciju `arrayToList` koja gradi strukturu liste kao što je prikazano kada joj se prosledi `[1, 2, 3]` kao argument. Takođe napišite funkciju `listToArray` koja proizvodi niz iz liste. Dodajte pomoćne funkcije `prepend`, koja uzima element i listu i kreira novu listu koja dodaje element na početak ulazne liste, i `nth`, koja uzima listu i broj i vraća element na datoj poziciji u listi (gde nula označava prvi element) ili `undefined` kada takav element ne postoji.

{{index recursion}}

Ako već niste, napišite i rekurzivnu verziju funkcije `nth`.

{{if interactive

```{test: no}
// Vas kod ovde.

console.log(arrayToList([10, 20]));
// → {value: 10, rest: {value: 20, rest: null}}
console.log(listToArray(arrayToList([10, 20, 30])));
// → [10, 20, 30]
console.log(prepend(10, prepend(20, null)));
// → {value: 10, rest: {value: 20, rest: null}}
console.log(nth(arrayToList([10, 20, 30]), 1));
// → 20
```

if}}

{{hint

{{index "list (exercise)", "linked list"}}

Izgradnja liste je lakša kada se radi od nazad ka napred. Dakle, `arrayToList` može iterirati unazad kroz niz (videti prethodnu vežbu) i za svaki element dodati objekat u listu. Možete koristiti lokalnu varijablu da zadržite deo liste koji je do sada izgrađen i koristiti dodelu poput `list = {value: X, rest: list}` da biste dodali element.

{{index "for loop"}}

Da biste iterirali kroz listu (u `listToArray` i `nth`), možete koristiti `for` petlju na sledeći način:

```
for (let node = list; node; node = node.rest) {}
```

Možete li videti kako ovo funkcioniše? Svaka iteracija petlje, `node` pokazuje na trenutnu podlistu, a telo petlje može čitati njeno svojstvo `value` kako bi dobilo trenutni element. Na kraju iteracije, `node` se premešta na sledeću podlistu. Kada je to null, dostigli smo kraj liste, i petlja je završena.

{{index recursion}}

Rekurzivna verzija funkcije `nth` će, slično tome, posmatrati sve manji deo "repa" liste i istovremeno smanjivati indeks dok ne dođe do nule, u kom trenutku može vratiti svojstvo `value` čvora na koji gleda. Da biste dobili nulti element liste, jednostavno uzmete svojstvo `value` njenog glavnog čvora. Da biste dobili element _N_ + 1, uzimate *N*-ti element liste koji se nalazi u svojstvu `rest` ove liste.

hint}}

{{id exercise_deep_compare}}

### Duboko uporedjivanje

{{index "deep comparison (exercise)", [comparison, deep], "deep comparison", "== operator"}}

Operator `==` upoređuje objekte po identitetu, ali ponekad biste želeli da uporedite vrednosti njihovih stvarnih svojstava.

Napišite funkciju `deepEqual` koja uzima dve vrednosti i vraća true samo ako su ista vrednost ili su objekti sa istim svojstvima, gde su vrednosti svojstava jednake kada se uporede rekurzivnim pozivom funkcije `deepEqual`.

{{index null, "=== operator", "typeof operator"}}

Da biste saznali da li vrednosti treba uporediti direktno (koristeći operator `===` za to) ili uporediti svojstva, možete koristiti operator `typeof`. Ako on proizvodi `"object"` za obe vrednosti, trebalo bi da uradite duboku poređenje. Ali morate uzeti u obzir jedan smešan izuzetak: zbog istorijske slučajnosti, `typeof null` takođe proizvodi `"object"`.

{{index "Object.keys function"}}

Funkcija `Object.keys` će biti korisna kada trebate proći kroz svojstva objekata da biste ih uporedili.

{{if interactive

```{test: no}
// Vas kod ovde.

let obj = {here: {is: "an"}, object: 2};
console.log(deepEqual(obj, obj));
// → true
console.log(deepEqual(obj, {here: 1, object: 2}));
// → false
console.log(deepEqual(obj, {here: {is: "an"}, object: 2}));
// → true
```

if}}

{{hint

{{index "deep comparison (exercise)", [comparison, deep], "typeof operator", "=== operator"}}

Vaš test da li imate posla sa pravim objektom izgledaće nešto poput ovog `typeof x == "object" && x != null`. Gledajte da uporedite svojstva samo kada su _oba_ argumenta objekti. U svim ostalim slučajevima možete odmah vratiti rezultat primene `===`.

{{index "Object.keys function"}}

Koristite `Object.keys` da prođete kroz sva svojstva. Treba da testirate da li oba objekta imaju isti skup imena svojstava i da li ta svojstva imaju identične vrednosti. Jedan način da to uradite je da obezbedite da oba objekta imaju isti broj svojstava (dužine liste svojstava su iste). Zatim, kada prolazite kroz svojstva jednog objekta da biste ih uporedili, uvek prvo proverite da li drugi objekat zapravo ima svojstvo tim imenom. Ako imaju isti broj svojstava i sva svojstva u jednom takođe postoje u drugom, imaju isti skup imena svojstava.

{{index "return value"}}

Vraćanje tačne vrednosti iz funkcije najbolje je uraditi tako što ćete odmah vratiti false kada pronađete neusaglašenost i vratiti true na kraju funkcije ukoliko nema nijedne nusaglasenosti.

hint}}
