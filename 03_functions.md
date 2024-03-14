# Funkcije

{{quote {author: "Donald Knuth", chapter: true}

Ljudi misle da je računarstvo umetnost za genije, ali zapravo je totalno drugačije, to je samo mnogo ljudi koji grade stvari na ono što je prethodno izgrađeno, kao zid on malih kamenčića.

quote}}

{{index "Knuth, Donald"}}

{{figure {url: "img/chapter_picture_3.jpg", alt: "Illustration of fern leaves with a fractal shape, bees in the background", chapter: framed}}}

{{index function, [code, "structure of"]}}

Funkcije su jedan od centralnih alata u JavaScript programiranju. Koncept čuvanja dela programa u neku varijably ima mnogo primena. Pruža nam način da strukturišemo veće programe, smanjimo ponavljanje koda, povežemo imena sa podprogramima i izolujemo ove podprograme jedan od drugog.

Najjasnija primena funkcija je definisanje novog ((vokabulara)). Stvaranje novih reči u prozi obično nije toliko poželjno, ali u programiranju je neophodno.

{{index abstraction, vocabulary}}

Tipični odrasli govornici engleskog jezika imaju oko 20.000 reči u svom vokabularu. Malo programskih jezika dolazi sa 20.000 ugrađenih komandi. I vokabular koji jeste dostupan obično je preciznije definisan, i samim tim manje fleksibilan, nego u ljudskom jeziku. Zbog toga moramo uvesti nove reči da bismo izbegli preteranu opširnost.

## Definisanje funkcije

{{index "square example", [function, definition], [binding, definition]}}

Definicija funkcije je obična varijabla čija je vrednost funkcija. Na primer, ovaj kod definiše `kvadrat` i odnosi se na funkciju koja proizvodi kvadrat datog broja:

```
const kvadrat = function(x) {
  return x * x;
};

console.log(kvadrat(12));
// → 144
```

{{indexsee "curly braces", braces}}
{{index [braces, "function body"], block, [syntax, function], "function keyword", [function, body], [function, "as value"], [parentheses, arguments]}}

Funkcija se kreira izrazom koji počinje ključnom rečju `function`. Funkcije imaju skup _((parametara))_ (u prethodnom slučaju, samo `x`) i _telo_, koje sadrži izjave koje treba izvršiti kada se funkcija pozove. Telo funkcije kreirane na ovaj način uvek mora biti obavijeno vitičastim zagradama, čak i kada se sastoji samo od jedne ((izjave)).

{{index "roundTo example"}}

Funkcija može imati više parametara ili uopšte ne mora imati parametre. U sledećem primeru, `makeNoise` ne navodi imena parametara, dok `roundTo` (koji zaokružuje `n` na najbliži višekratnik `step`) navodi dva:

```
const makeNoise = function() {
  console.log("Pling!");
};

makeNoise();
// → Pling!

const roundTo = function(n, step) {
  let remainder = n % step;
  return n - remainder + (remainder < step / 2 ? 0 : step);
};

console.log(roundTo(23, 10));
// → 20
```

{{index "return value", "return keyword", undefined}}

Neke funkcije, kao što su `roundTo` and `kvadrat` proizvode vrednost, a neke ne, kao na primer `makeNoise`, čiji je jedini rezultat sporedni efekat. `return` izjava određuje vrednost koju funkcija vraća. Kada kontrolni tok naiđe na takvu izjavu, on automatski iskače van trenutne funckije i vraća vrednost kodu koji je pozvao tu funkciju. `return` ključna reč bez ikakvog izraza posle nje, napraviće da funkcija vrati `undefined`. Funkcije koje nemaju `return` izjavu uopšte, kao na primer `makeNoise`, takođe vraćaju `undefined`.

{{index parameter, [function, application], [binding, "from parameter"]}}

Parametri funkcije se ponašaju kao obične varijable, ali njihove početne vrednosti daje pozivaoc funkcije, a ne kod u samoj funkciji.

## Bindings i skoup (eng. scope)

{{indexsee "top-level scope", "global scope"}}
{{index "var keyword", "global scope", [binding, global], [binding, "scope of"]}}

Skoup ćemo ugrubo prevesti kao domen, ali ćemo u tekstu koristiti reč skoup.

Svaki binding ima _((skoup))_, što predstavlja deo programa u kojem je taj binding vidljiv i iskoristiv. Za bindinge definisane van bilo koje funkcije, bloka ili modula (vidi [Poglavlje ?](modules)), skoup je čitav program - možete se pozivati na taj binding gde god želite. Oni se zovu _globalni_.

{{index "local scope", [binding, local]}}

Bindingovi koji su kreirani za ((parametre)) funkcije ili deklarisani unutar funkcije mogu se pozivati samo unutar te funkcije, pa se nazivaju lokalnim bindingovima. Svaki put kada se funkcija pozove, kreiraju se nove instance ovih bindingova. Ovo pruža određenu izolaciju između funkcija - svaki poziv funkcije postoji u svojem malom svetu (svojem lokalnom okruženju) i često se može razumeti bez poznavanja mnogo toga o tome šta se dešava u globalnom okruženju.

{{index "let keyword", "const keyword", "var keyword"}}

Bindingovi tj varijable deklarisane sa `let` i `const` su zapravo lokalni za _((blok))_ u kojem su deklarisani, tako da ako kreirate jedan od njih unutar petlje, kôd pre i posle petlje ne može ga "videti". U JavaScriptu pre 2015. godine, samo su funkcije stvarale nove opsege, tako da su varijable starog stila, kreirane sa ključnom rečju `var`, vidljive u celoj funkciji u kojoj se pojavljuju - ili u celom globalnom opsegu, ako nisu u funkciji.

```
let x = 10;   // global
if (true) {
  let y = 20; // lokalna za ovaj blok
  var z = 30; // takodje global
}
```

{{index [binding, visibility]}}

Svaki ((skoup)) može "pogledati" u opseg oko sebe, tako da je `x` vidljiv unutar bloka u primeru. Izuzetak je kada više varijabli ima isto ime - u tom slučaju, kôd može videti samo najunutrašniji. Na primer, kada se kôd unutar funkcije `halve` poziva na `n`, taj kod vidi svoj _sopstveni_ `n`, a ne globalni `n`.

```
const halve = function(n) {
  return n / 2;
};

let n = 10;
console.log(halve(100));
// → 50
console.log(n);
// → 10
```

{{id scoping}}

## Ugnežđeni skoup

{{index [nesting, "of functions"], [nesting, "of scope"], scope, "inner function", "lexical scoping"}}

JavaScript ne pravi samo razliku između globalnih i lokalnih varijabli. Blokovi i funkcije mogu biti kreirani unutar drugih blokova i funkcija, što proizvodi nekoliko novih stepeni lokaliteta.

{{index "landscape example"}}

Na primer, ova funkcija - čiji izlaz daje sastojke potrebne za pravljenje humusa - ima još jednu funkciju unutar sebe:

```
const hummus = function(faktor) {
  const sastojak = function(kolicina, mernaJedinica, ime) {
    let kolicinaSastojka = kolicina * faktor;
    if (kolicinaSastojka > 1) {
      mernaJedinica += "s";
    }
    console.log(`${kolicinaSastojka} ${mernaJedinica} ${ime}`);
  };
  sastojak(1, "konzerva", "leblebija");
  sastojak(0.25, "čaša", "tahini");
  sastojak(0.25, "čaša", "sok od limuna");
  sastojak(1, "glavica", "beli luk");
  sastojak(2, "supena kašika", "maslinovo ulje");
  sastojak(0.5, "kašičica", "kumin");
};
```

{{index [function, scope], scope}}

Kôd unutar funckije `sastojak` može videti `faktor` varijablu iz vanjske funkcije, ali njene lokalne varijable, kao što su `mernaJedinica` ili `kolicinaSastojka` nisu vidljive vanjskoj funkciji.

Skup varijabli vidljivih unutar bloka određen je mestom tog bloka u programskom tekstu. Svaki lokalni opseg takođe može videti sve lokalne opsege koji ga sadrže, dok svi opsezi mogu videti globalni opseg. Ovaj pristup vidljivosti varijabli naziva se _((leksičko opsegovanje))_ tj. _((lexical scoping))_.

## Funkcije kao vrednosti

{{index [function, "as value"], [binding, definition]}}

Binding funkcije obično funkcioniše kao ime za određeni deo programa. Takav binding se definiše jednom i nikada se ne menja. To često dovodi do zabune između funkcije i njenog imena.

{{index [binding, assignment]}}

Ali ovo dvoje zapravo predstavljaju različite stvari. Vrednost koja je tip funkcije može raditi sve što i druge vrednosti - možete je koristiti u proizvoljnim ((izrazima)), a ne samo je pozvati. Moguće je sačuvati vrednost funkcije u novi binding tj varijablu, proslediti je kao argument funkciji, i tako dalje. Slično tome, binding koji sadrži funkciju je i dalje samo običan binding i može, ako nije konstantan, dobiti novu vrednost, na primer:

```{test: no}
let lansirajRakete = function() {
  raketniSistem.lansiraj("sada");
};
if (safeMode) {
  lansirajRakete = function() {/* ne radi nista */};
}
```

{{index [function, "higher-order"]}}

U [Poglavlju ?](higher_order), razgovaraćemo o zanimljivim stvarima koje možemo postići slanjem vrednosti koje su tipa funkcija drugim funkcijama.

## Notacija deklaracije

{{index [syntax, function], "function keyword", "square example", [function, definition], [function, declaration]}}

Postoji nešto kraći način za kreiranje funkcije. Kada se ključna reč `function` koristi na početku izjave, ona radi na drugačiji način:

```{test: wrap}
function square(x) {
  return x * x;
}
```

{{index future, "execution order"}}

Ovo je _deklaracija_ funkcije. Ova izjava vezuje ime `square` za funkciju. Upravo smo definisali funkciju koja se zove `square`. Malo je lakše za pisanje i ne zahteva tačku-zarez nakon funkcije.

Postoji jedna suptilnost kod ove forme definisanja funkcije.

```
console.log("The future says:", future());

function future() {
  return "You'll never have flying cars";
}
```

Prethodni kôd radi, iako je funkcija definisana _ispod_, što znači _nakon_, koda koji je koristi. Deklaracije funkcija nisu deo redovnog toka kontrole od vrha ka dnu. Konceptualno se pomeraju na vrh svog opsega i mogu se koristiti od strane celog koda u tom opsegu. Ovo je ponekad korisno jer pruža slobodu da se kôd uređuje na način koji deluje najjasnije, bez brige o tome da li su sve funkcije definisane pre nego što budu korišćene.

## Strelica funkcije

{{index function, "arrow function"}}

Postoji i treća notacija za funkcije koja izgleda veoma drukčije od ostalih. Umesto ključne reči `function`, ova notacija koristi strelicu (`=>`) koja se sastoji od znaka jednakosti i znaka za "veće od" (nemojte ovo pomešati sa operatorom "veće ili jednako" koji se piše `>=`):

```{test: wrap}
const roundTo = (n, step) => {
  let remainder = n % step;
  return n - remainder + (remainder < step / 2 ? 0 : step);
};
```

{{index [function, body]}}

Strelica dolazi _posle_ liste parametara a zatim sledi telo funkcije. Možete to čitati otprilike na ovaj način: "ovaj ulaz (parametri) proizvodi ovaj rezultat (telo funkcije)".

{{index [braces, "function body"], "square example", [parentheses, arguments]}}

Kada postoji samo jedno ime parametra, možete izostaviti zagrade oko liste parametara. Ako je telo funkcije samo jedan izraz, umesto bloka u vitičastim zagradama, taj izraz će biti vraćen iz funkcije. Dakle, ove dve definicije `square` funkcije rade isto:

```
const square1 = (x) => { return x * x; };
const square2 = x => x * x;
```

{{index [parentheses, arguments]}}

Kada streličasta funkcija uopšte nema parametre, lista njenih parametara je samo prazan set zagrada.

```
const horn = () => {
  console.log("Toot");
};
```

{{index verbosity}}

Nema nekog posebnog razloga za postojanje i streličaste funkcije i `function` izraza u jeziku. Osim jednog sitnog detalja, o kojem ćemo raspravljati u [Poglavlju ?](object), one rade isto. Streličaste funkcije su dodate 2015. godine, uglavnom kako bi omogućile pisanje malih funkcionalnih izraza na kraći način. Često ćemo ih koristiti u [Poglavlju ?](higher_order).

{{id stack}}

## Call stack

{{indexsee stack, "call stack"}}
{{index "call stack", [function, application]}}

Način na koji programska kontrola teče kroz funkcije je pomalo složen. Hajde da ga detaljnije pogledamo. Evo jednostavnog programa koji vrši nekoliko poziva funkcija:

```
function greet(who) {
  console.log("Hello " + who);
}
greet("Harry");
console.log("Bye");
```

{{index ["control flow", functions], "execution order", "console.log"}}

Izvršavanje ovog programa ide otprilike ovako: poziv funkcije `greet` uzrokuje skok reda izvršavanja koda na početak te funkcije (linija 2). Funkcija poziva `console.log`, koji preuzima kontrolu, obavlja svoj posao, a zatim vraća kontrolu na liniju 2. Tamo stiže na kraj funkcije `greet`, pa se vraća na mesto koje ju je pozvalo - liniju 4. Linija nakon toga ponovo poziva `console.log`. Nakon što se to vrati, program dostiže kraj.

Tok kontrole možemo prikazati šematski na sledeći način:

```{lang: null}
nismo unutar funkcije
   u funkciji greet
        u funkciji console.log
   u funkciji greet
nismo unutar funkcije
   u funkciji console.log
nismo unutar funkcije
```

{{index "return keyword", [memory, call stack]}}

Zato što funkcija mora da se vrati na mesto koje ju je pozvalo, kada se vrati, računar mora da zapamti kontekst iz kojeg je poziv nastao. U prvom slučaju, `console.log` mora da se vrati u funkciju `greet` kada završi. U drugom slučaju, vraća se na kraj programa. Funkcija se uvek vraća na mesto koje ju je originalno pozvalo.

Mesto gde računar čuva ovaj kontekst zove se _((stek poziva))_ tj _((call stack))_. Svaki put kada se funkcija pozove, trenutni kontekst se čuva na vrhu ovog steka. Kada funkcija vrati vrednost, uklanja se poslednji kontekst sa steka i koristi taj kontekst za nastavak izvršavanja.

{{index "infinite loop", "stack overflow", recursion}}

Čuvanje ovog steka zahteva prostor u memoriji računara. Kada stek postane prevelik, program će se srušiti sa porukom poput "nedostatak prostora na steku" ili "previše rekurzije". Sledeći kod to ilustruje tako što računaru postavlja veoma teško pitanje koje uzrokuje beskonačno odlazak i vraćanje između dve funkcije. Ili bolje rečeno, _bilo bi_ beskonačno, da računar ima beskonačan stek. Potrošićemo sav prostor ili "premašiti stek".

```{test: no}
function chicken() {
  return egg();
}
function egg() {
  return chicken();
}
console.log(chicken() + " came first.");
// → ??
```

## Opcioni argumenti

{{index argument, [function, application]}}

Sledeći kod je potpuno validan i izvršava se bez ikakvih problema:

```
function square(x) { return x * x; }
console.log(square(4, true, "Pero"));
// → 16
```

Definisali smo funkciju `square` sa samo jednim ((parametrom)). Ipak, kada je pozovemo sa tri, jezik ne pravi problem. Ignoriše dodatne argumente i računa kvadrat prvog.

{{index undefined}}

JavaScript je izuzetno ležeran jezik kada je u pitanju broj argumenata koje možete proslediti funkciji. Ako prosledite previše njih, dodatni će biti ignorisani. Ako prosledite premalo, nedostajući parametri će dobiti vrednost `undefined`.

Nedostatak ovoga je što je moguće - čak i veoma verovatno - da ćete slučajno proslediti pogrešan broj argumenata funkcijama. I niko vam neće reći ništa o tome. Prednost je što možete koristiti ovako ponašanje da biste omogućili pozivanje funkcije sa različitim brojevima argumenata. Na primer, ova funkcija `minus` pokušava da imitira `-` operator delovanjem na jedan ili dva argumenta:

```
function minus(a, b) {
  if (b === undefined) return -a;
  else return a - b;
}

console.log(minus(10));
// → -10
console.log(minus(10, 5));
// → 5
```

{{id roundTo}}
{{index "optional argument", "default value", parameter, ["= operator", "for default value"] "roundTo example"}}

Ako napišete `=` operator praćen nekim izrazom posle parametra, vrednost tog izraza će zameniti argument kada nije dat. Na primer, ova verzija `roundTo` funkcije pravi njen drugi argument opcionalnim. Ako ne dostavite drugi argument pri pozivu funkcije ili prosledite vrednost `undefined`, podrazumevana vrednost parametra step će biti 1:

```{test: wrap}
function roundTo(n, step = 1) {
  let remainder = n % step;
  return n - remainder + (remainder < step / 2 ? 0 : step);
};

console.log(roundTo(4.5));
// → 5
console.log(roundTo(4.5, 2));
// → 4
```

{{index "console.log"}}

[U sledećem poglavlju](data#rest_parameters) će biti predstavljen način na koji telo funkcije može da pristupi celokupnoj listi argumenata koji su joj prosleđeni. Ovo je korisno jer omogućava funkciji da prihvati bilo koji broj argumenata. Na primer, `console.log` to radi, prikazujući sve vrednosti koje su joj prosleđene:

```
console.log("C", "O", 2);
// → C O 2
```

## Closure (čitaj kloužr)

{{index "call stack", "local binding", [function, "as value"], scope}}

Mogućnost da funkcije tretiramo kao vrednosti, zajedno sa činjenicom da se lokalne varijable rekreiraju svaki put kada se funkcija pozove, postavlja interesantno pitanje: šta se dešava sa lokalnim varijablama kada poziv funkcije koji ih je stvorio više nije aktivan?

Sledeći kôd prikazuje primer ovoga. Definiše funkciju, `wrapValue`, koja kreira lokalnu varijablu `local`. Zatim vraća novu funkciju koja pristupa toj varijabli i vraća ovu lokalnu varijablu:

```
function wrapValue(n) {
  let local = n;
  return () => local;
}

let wrap1 = wrapValue(1);
let wrap2 = wrapValue(2);
console.log(wrap1());
// → 1
console.log(wrap2());
// → 2
```

Ovo je dozvoljeno i radi kao što biste se nadali - obe instance varijable i dalje mogu biti pristupljene. Ova situacija je dobra demonstracija činjenice da se lokalna vezivanja stvaraju iznova za svaki poziv, i različiti pozivi ne utiču na lokalne varijable jedni drugih.

Ova osobina - mogućnost pozivanja na određenu instancu lokalnog vezivanja u spoljašnjem opsegu - naziva se _((closure))_. Funkcija koja referiše na varijable iz lokalnih opsega oko nje naziva se _closure_. Ovo ponašanje ne samo što vas oslobađa brige o vremenu trajanja varijable, već takođe omogućava kreativnu upotrebu vrednosti koje su tipa funkcije.

{{index "multiplier function"}}

Uz malu promenu, možemo promeniti prošli primer u način kreiranja funkcija koje množe argument nekom nasumičnom vrednošću:

```
function multiplikator(faktor) {
  return broj => broj * faktor;
}

let duplo = multiplikator(2);
console.log(duplo(5));
// → 10
```

{{index [binding, "from parameter"]}}

Eksplicitna `local` varijabla iz primera `wrapValue` zapravo nije ni potrebna, jer je sam parametar lokalna varijabla.

{{index [function, "model of"]}}

Razmišljanje o programima poput ovih zahteva vežbanje. Dobar mentalni model za ovo je da razmišljate o vrednostima koje su tipa funkcija, kao o nečemu što sadrži kôd te funkcije, ali i okruženje u kojem su kreirane. Kada se pozove, telo funkcije vidi okruženje u kojem je kreirano, a ne okruženje u kojem je pozvano.

U prethodnom primeru, `multiplikator` funkcija je pozvana i pravi okruženje u kojem njen `faktor` parametar postaje 2. Vrednost, koji je tipa funkcija, koju `multiplikator` vraća, a koja je sačuvana u `duplo` varijabli, pamti ovo okruženje tako da kad je pozovemo, množi argument brojem 2.

## Rekurzija

{{index "power example", "stack overflow", recursion, [function, application]}}

Sasvim je u redu da funkcija pozove samu sebe, sve dok to ne radi toliko često da prelije stek. Funkcija koja poziva samu sebe se naziva _rekurzivna_. Rekurzija omogućava nekim funkcijama da budu napisane na drugačiji način. Na primer, pogledajte ovu `power` funkciju, koja radi isto što i `**` (stepenovanje) operator:

```{test: wrap}
function power(base, exponent) {
  if (exponent == 0) {
    return 1;
  } else {
    return base * power(base, exponent - 1);
  }
}

console.log(power(2, 3));
// → 8
```

{{index loop, readability, mathematics}}

Ovo je prilično slično načinu na koji matematičari definišu stepenovanje i možda opisuje koncept jasnije nego petlja koju smo koristili u [Poglavlju ?](program_structure). Funkcija se poziva više puta sa sve manjim eksponentima kako bi postigla ponovljeno množenje.

{{index [function, application], efficiency}}

Međutim, ova implementacija ima jedan problem: u tipičnim JavaScript implementacijama, ona je otprilike tri puta sporija od verzije koja koristi `for` petlju. Pokretanje kroz jednostavnu petlju je generalno brže od višestrukog pozivanja funkcije.

{{index optimization}}

Dilema između brzine i ((elegantnosti)) je interesantna i veoma česta u programiranju. Možete je posmatrati kao vrstu kontinuiteta između prijateljske nastrojenosti prema čoveku i prijateljske nastrojenosti prema mašini. Skoro svaki program može postati brži ako ga učinite većim i složenijim. Programer mora pronaći odgovarajuću ravnotežu.

U slučaju `power` funkcije, neelegantna (ona sa petljama) verzija je i dalje prilično jednostavna i lako čitljiva. Nema mnogo smisla zameniti je rekurzivnom funkcijom. Međutim, često se programi bave toliko složenim konceptima da odustajanje od neke efikasnosti kako bi se program učinio jednostavnijim može biti korisno.

{{index profiling}}

Briga o efikasnosti može vas ometati. To je još jedan faktor koji komplikuje dizajn programa, i kada već radite nešto što je teško, dodatna stvar o kojoj treba brinuti može vas paralisati u radu.

{{index "premature optimization"}}

Stoga, generalno je najbolje početi tako što ćete napisati nešto što je ispravno i lako razumljivo. Ako brinete da je previše sporo - što obično nije slučaj, jer većina kôda jednostavno nije dovoljno često izvršena da bi oduzela značajno vreme - možete kasnije izmeriti i poboljšati ga ako je potrebno.

{{index "branching recursion"}}

Rekurzija nije baš uvek samo neefikasnija alternativa petaljama. Neki problemi se zaista jednostavnije rešavaju korištenjem rekurzije umesto petlji. Najčešće to su problemi koji zahtevaju istraživanje ili obrađivanje više "grana", od kojih bi se svaka mogla dodatno razgranavati na još dodatnih grana.

{{id recursive_puzzle}}
{{index recursion, "number puzzle example"}}

Razmislite o sledećem zadatku: počinjući od broja 1, konstantno dodavajući mu broj 5, ili množeći ga sa 3, možemo konstruisati beskonačan skup brojeva. Kako biste napisali funkciju koja, za dati broj, pokušava da pronađe sekvencu takvih sabiranja i množenja, koje će proizvesti dati broj? Na primer, broj 13 se može postići tako što prvo pomnožimo sa 3, pa onda saberemo sa 5, dva puta, a broj 15 se ne može dostići nikada.

Evo rekurzivnog rešenja:

```
function findSolution(target) {
  function find(current, history) {
    if (current == target) {
      return history;
    } else if (current > target) {
      return null;
    } else {
      return find(current + 5, `(${history} + 5)`) ??
             find(current * 3, `(${history} * 3)`);
    }
  }
  return find(1, "1");
}

console.log(findSolution(24));
// → (((1 * 3) + 5) * 3)
```

Napomena da ovaj program ne mora nužno da pronađe _najkraći_ niz operacija. On je zadovoljan kada pronađe bilo koji niz operacija.

U redu je ako odmah ne vidite kako ovaj kod funkcioniše. Hajde da radimo na njemu, pošto je odlična vežba za rekurzivno razmišljanje.

Unutrašnja funkcija `find` vrši stvarnu rekurziju. Ona uzima dva ((argumenta)): trenutni broj i string koji beleži kako smo došli do ovog broja. Ako pronađe rešenje, vraća string koji pokazuje kako doći do cilja. Ako ne može pronaći rešenje počevši od ovog broja, vraća `null`.

{{index null, "?? operator", "short-circuit evaluation"}}

Da bi ovo postigla, naša funkcija obavlja 3 operacije. Ako je trenutni broj onaj koji tražimo, onda je trenutna istorija način na koji smo došli do tog broja, pa vraćamo to kao rezultat. Ako je trenutni broj veći od broja koji tražimo, nema poente da dalje istražujemo ovu granu, jer i sabiranje i množenje će nas samo odvesti u još veće brojeve, pa u tom slučaju vraćamo `null`. Na kraju, ako smo na broju manjem od onog koji tražimo, funkcija će pokušati oba moguća grananja koja počinju od broja na kojem se trenutno nalazimo, tako što će pozvati sama sebe 2 puta - jednom za sabiranje, jednom za množenje. Ako nam prvi poziv vrati nešto što nije `null`, onda ćemo vratiti taj rezultat. U suprotnom, vratićemo ono što nam vraća drugi poziv funkcije, nezavisno od toga da li nam on vraća string ili `null`.

{{index "call stack"}}

Da bismo bolje razumeli kako ova funkcija proizvodi efekat koji tražimo, pogledajmo kako izgledaju pozivi `find` funkciji koje pravimo dok pretražujemo rešenje za broj 13:

```{lang: null}
find(1, "1")
  find(6, "(1 + 5)")
    find(11, "((1 + 5) + 5)")
      find(16, "(((1 + 5) + 5) + 5)")
        preveliko
      find(33, "(((1 + 5) + 5) * 3)")
        preveliko
    find(18, "((1 + 5) * 3)")
      preveliko
  find(3, "(1 * 3)")
    find(8, "((1 * 3) + 5)")
      find(13, "(((1 * 3) + 5) + 5)")
        pronadjeno!
```

Indentacija označava dubinu call stacka. Prvi put kada se `find` poziva, funkcija počinje pozivanjem same sebe da istraži rešenje koje počinje sa `(1 + 5)`. Taj poziv će rekurzivno istražiti _svako_ sledeće rešenje koje daje broj manji ili jednak ciljanom broju. Pošto ne nađe ono što traži, vraća `null` na prvi poziv. Tu operator `??` dovodi do poziva koji istražuje `(1 * 3)`. Ova pretraga ima više sreće—njen prvi rekurzivni poziv, preko još _jednog_ rekurzivnog poziva, pogađa ciljni broj. Taj najunutrašnji poziv vraća niz, i svaki od operatora `??` u posredničkim pozivima prosleđuje taj niz, na kraju vraćajući rešenje.

## Rastuće funkcije

{{index [function, definition]}}

Postoje dva, manje više prirodna načina da se funkcije uvedu u program.

{{index repetition}}

Prvi način je kada primetite da više puta pišete sličan kod. Ovo je bolje izbeći ako možete, jer više koda znači više prostora za skrivene greške i više materijala za čitanje ljudima koji pokušavaju da razumeju program. Zato uzimate funkcionalnost koja se ponavlja na više mesta, pronalazite dobro ime za nju i stavljate je u funkciju.

Drugi način je kada primetite da vam je potrebna neka funkcionalnost koju još niste napisali i koja zvuči kao da zaslužuje svoju sopstvenu funkciju. Počinjete tako što imenujete funkciju, zatim pišete njeno telo. Čak možete početi pisati kod koji koristi funkciju pre nego što zapravo definišete samu funkciju.

{{index [function, naming], [binding, naming]}}

Koliko je teško pronaći adekvatno ime za funkciju može biti dobra indikacija koliko je razumljiv koncept koji pokušavate staviti u funkciju. Hajde da pogledamo primer.

{{index "farm example"}}

Želimo napisati program koji ispisuje dva broja: broj krava i kokoški na farmi, sa rečima `Cows` (eng. krave) i `Chickens` (eng. kokoške) nakon broja, i sa nulama ispred oba broja, tako da su brojevi uvek dugi tačno tri cifre:

```{lang: null}
007 Cows
011 Chickens
```

Ovaj program zahteva funkciju sa dva argumenta - broj krava i broj kokoški. Ajmo na programiranje.

```
function printFarmInventory(cows, chickens) {
  let cowString = String(cows);
  while (cowString.length < 3) {
    cowString = "0" + cowString;
  }
  console.log(`${cowString} Cows`);
  let chickenString = String(chickens);
  while (chickenString.length < 3) {
    chickenString = "0" + chickenString;
  }
  console.log(`${chickenString} Chickens`);
}
printFarmInventory(7, 11);
```

{{index ["length property", "for string"], "while loop"}}

Pisanje `.length` nakon stringa će nam dati dužinu tog stringa. `while` petlje dodaju nule ispred brojevnih stringova sve dok nisu dugački bar tri karaktera .

To je to! Ali baš kada smo spremni da pošaljemo farmeru kod (uz pristojan račun), on nas zove i kaže nam da je takođe počeo da čuva svinje, i da li bismo mogli proširiti softver da takođe štampa broj svinja?

{{index "copy-paste programming"}}

Naravno da možemo. Ali u sred procesa kopiranja i lepljenja te četiri linije koda još jedan put, zastaćemo i porazmisliti. Mora postojati bolji način. Evo prvog pokušaja:

```
function printZeroPaddedWithLabel(number, label) {
  let numberString = String(number);
  while (numberString.length < 3) {
    numberString = "0" + numberString;
  }
  console.log(`${numberString} ${label}`);
}

function printFarmInventory(cows, chickens, pigs) {
  printZeroPaddedWithLabel(cows, "Cows");
  printZeroPaddedWithLabel(chickens, "Chickens");
  printZeroPaddedWithLabel(pigs, "Pigs");
}

printFarmInventory(7, 11, 3);
```

{{index [function, naming]}}

Funkcioniše! Ali ovaj naziv `printZeroPaddedWithLabel` ("ispiši oznaku sa nulama ispred") je malo čudno. Ovo ime meša 3 različite stvari, ispisivanje, postavaljanje nula ispred i dodavanje oznake - sve u jednoj funkciji.

{{index "zeroPad function"}}

Umesto da izvlačimo deo programa koji se ponavlja, hajde da pokušamo izvući jedan _koncept_ umesto toga:

```
function zeroPad(number, width) {
  let string = String(number);
  while (string.length < width) {
    string = "0" + string;
  }
  return string;
}

function printFarmInventory(cows, chickens, pigs) {
  console.log(`${zeroPad(cows, 3)} Cows`);
  console.log(`${zeroPad(chickens, 3)} Chickens`);
  console.log(`${zeroPad(pigs, 3)} Pigs`);
}

printFarmInventory(7, 16, 3);
```

{{index readability, "pure function"}}

Funkcija sa lepim, očiglednim imenom kao što je `zeroPad`, "dodaj nule, olakšava onom ko čita kod da razazna šta ta funkcija zapravo radi. Takva funkcija je korisna u više situacija, ne samo u ovom specifičnom programu. Na primer, mogli bismo je iskorisiti za ispisivanje lepo poravnanih tabela brojeva.

{{index [interface, design]}}

Koliko pametna i svestrana jedna funkcija treba da bude? Mi možemo napisati bilo šta, od prejednostavne funkcije koja može samo formatirati broj da uvek ima 3 cifre, do nekih komplikovanih, generalizovanih formatera brojnih sistema, koji mogu da rade i sa razlomcima, negativnim brojevima, poravnanju decimalnih mesta, poravnanju sa različitim karakterima itd.

Jedan korisan princip koji bi trebalo da pratite je da ne dodajete neke dodatne funkcionalnosti ako niste zaista sigurni da ćete ih trebati. Može biti veoma primamljujuće da otkucate generalizovan "framework" za svaku funkcionalnost koja vam zatreba. Ali oduprite se tom porivu. Nećete zapravo uraditi ništa, a bićete prezauzeti pisanjem koda koji vam nikada neće trebati.

{{id pure}}
## Funkcije i sporedni efekti

{{index "side effect", "pure function", [function, purity]}}

Funkcije se ugrubo mogu podeliti u one koje se pozivaju zbog sporednih efekata koje prave, i one koje se pozivaju zbog vrednosti koje vraćaju (ali je takođe moguće da proizvode i sporedne efekte, i vraćaju vrednost).

{{index reuse}}

Prva pomoćna funkcija u primeru frame `printZeroPaddedWithLabel` je pozivana zbog sporednih efekata koje proizvodi: ona ispisuje liniju. Druga verzija, `zeroPad` je pozvana zbog vrednosti koju vraća. Nije slučajnost da je druga funkcija korisnija u više situacija od prve. Funkcije koje vraćaju vrednosti se lakše kombinuju na nove načine nego funkcije koje direktno proizvode sporedne efekte.

{{index substitution}}

Čista ili _pure_ funkcija je specifičan tip funkcije koja proizvodi vrednost, koje ne samo da nema sporedne efekte, već se takođe ne oslanja na sporedne efekte drugog koda - na primer, ne čita globalne varijable čija se vrednost može promeniti. Čista funkcija ima veoma lepu osobinu da, kad je pozovete sa istim argumentima, uvek proizvodi istu vrednost (i ništa više). Poziv takvoj funkciji može se zameniti sa vrednošću koju ona vraća, bez da to promeni značenje koda. Kada niste sigurni da čista funkcija radi kako treba, možete je testirati tako što ćete je pozvati i znati da ako radi u tom kontekstu, radiće i u bilo kojem drugom kontekstu. Nečiste funkcije generalno zahtevaju više pripreme za testiranje.

{{index optimization, "console.log"}}

Nema potrebe da se osećate loše kada pišete funkcije koje nisu čiste. Sporedni efekti su često korisni. Na primer, nije moguće napisati čistu verziju `console.log`, a `console.log` je korisno imati. Neki operacije je takođe lakše izraziti na efikasan način kada koristimo sporedne efekte.

## Da rezimiramo

Ovo poglavlje vas je naučilo kako da pišete svoje funkcije. Ključna reč `function`, kada se koristi kao izraz, može stvoriti vrednost funkcije. Kada se koristi kao izjava, može se koristiti za deklarisanje varijabli i davanje funkcije kao njene vrednosti. Streličaste funkcije su još jedan način za kreiranje funkcija.

```
// Definise f koja za vrednost drzi tip funkcije
const f = function(a) {
  console.log(a + 2);
};

// Definise g funkciju
function g(a, b) {
  return a * b * 3.5;
}

// Manje verbozna vrednost tipa funkcije
let h = a => a % 3;
```

Važan deo razumevanja funkcija je razumevanje opsega. Svaki blok kreira novi opseg. Parametri i varijable deklarisane u datom opsegu su lokalni i nisu vidljivi spolja. Varijable deklarisane sa `var` se ponašaju drugačije - završavaju u najbližem opsegu funkcije ili globalnom opsegu.

Razdvajanje zadataka koje vaš program obavlja u različite funkcije je korisno. Nećete morati toliko da ponavljate iste stvari, a funkcije mogu pomoći u organizaciji programa grupisanjem koda u delove koji rade određene stvari.

## Zadaci

### Minimum

{{index "Math object", "minimum (exercise)", "Math.min function", minimum}}

[Prethodno poglavlje](program_structure#return_values) vas je upoznalo sa standardnom funkcijom `Math.min` koja vraća svoj najmanji argument. Možemo napisati takvu funkciju i sami. Definišite funkciju `min` koja uzima dva argumenta i vraća manji od njih.

{{if interactive

```{test: no}
// Vas kod ovde.

console.log(min(0, 10));
// → 0
console.log(min(0, -10));
// → -10
```
if}}

{{hint

{{index "minimum (exercise)"}}

Ako imate problem sa postavljanjem vitičastih i malih zagrada na pravo mesto da dobijete validnu definiciju funkcije, započnite kopiranjem jednog od prethodnih primera iz ovog poglavlja, i modifikujte to.

{{index "return keyword"}}

Funkcija može sadržati više `return` izjava.

hint}}

### Rekurzija

{{index recursion, "isEven (exercise)", "even number"}}

Videli smo da možemo koristiti `%` (operator ostataka) da bismo testirali da li je broj paran ili neparan tako što koristimo `% 2` da bismo videli da li je deljiv sa dva. Evo još jednog načina da definišemo da li je pozitivan ceo broj paran ili neparan:

- Nula je paran broj.

- Jedan je neparan broj.

- Za bilo koji drugi broj _N_, njegova parnost je ista kao i _N_ - 2.

Napišite rekurzivnu funkciju `isEven` koja odgovara ovoj definiciji. Funkcija treba da prihvati jedan parametar (pozitivan ceo broj) i da vrati boolean vrednost da li je taj broj paran.

{{index "stack overflow"}}

Testirajte sa 50 i 75. Pogledajte kako se ponaša sa -1? Zašto? Možete li naći način da to popravite?

{{if interactive

```{test: no}
// Vas kod ovde.

console.log(isEven(50));
// → true
console.log(isEven(75));
// → false
console.log(isEven(-1));
// → ??
```

if}}

{{hint

{{index "isEven (exercise)", ["if keyword", chaining], recursion}}

Vaša funkcija će verovatno izgledati slično unutrašnjoj funkciji `find` u rekurzivnom primeru `findSolution` [ovde](functions#recursive_puzzle) u ovom poglavlju, sa lancom `if`/`else if`/`else` koji testira koji od tri slučaja se primenjuje. Konačno `else`, što odgovara trećem slučaju, čini rekurzivni poziv. Svaka grana treba da sadrži `return` izjavu ili na neki drugi način organizuje da se vrati određena vrednost.

{{index "stack overflow"}}

Kada se funkciji prosledi negativan broj, ona će rekurzivno pozivati sebe sve više i više, prosleđujući sebi sve negativnije brojeve, čime će se udaljavati od vraćanja rezultata. Na kraju će potrošiti sav prostor za stek i prekinuti izvršavanje.

hint}}

### Brojanje zrna

{{index "bean counting (exercise)", [string, indexing], "zero-based counting", ["length property", "for string"]}}

Možete dobiti *N*-ti karakter, ili slovo, iz stringa tako što ćete nakon stringa napisati `[N]` (na primer, `string[2]`). Dobijena vrednost će biti string koji sadrži samo jedan karakter (na primer, `"b"`). Prvi karakter ima poziciju 0, što uzrokuje da se poslednji nalazi na poziciji `string.length - 1`. Drugim rečima, string sa dva karaktera ima dužinu 2, a njegovi karakteri imaju pozicije 0 i 1.

Napišite funkciju `countBs` koja kao svoj jedini argument uzima string i vraća broj koji ukazuje na to koliko velikih slova B ima u stringu.

Zatim napišite funkciju nazvanu `countChar` koja se ponaša kao `countBs`, osim što uzima drugi argument koji ukazuje na karakter koji treba brojati (umesto brojanja samo velikih slova B). Prepravite funkciju `countBs` da koristi ovu novu funkciju.

{{if interactive

```{test: no}
// Vas kod ovde.

console.log(countBs("BOB"));
// → 2
console.log(countChar("kakkerlak", "k"));
// → 4
```

if}}

{{hint

{{index "bean counting (exercise)", ["length property", "for string"], "counter variable"}}

Vašoj funkciji će trebati petlja koja pregleda svaki karakter u stringu. Može pokretati indeks od nula do jedan manje od dužine stringa (`< string.length`). Ako je karakter na trenutnoj poziciji isti kao onaj koji funkcija traži, dodaje 1 varijabli brojača. Kada se petlja završi, brojač se može vratiti.

{{index "local binding"}}

Pobrinite se za to da vam sve varijable korištene u funkciji budu _lokalne_ funkciji tako što ćete ih pravilno deklarisati korištenjem `let` ili `const` ključnih reči.

hint}}
