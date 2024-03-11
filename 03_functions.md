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

Recursion is not always just an inefficient alternative to looping. Some problems really are easier to solve with recursion than with loops. Most often these are problems that require exploring or processing several "branches", each of which might branch out again into even more branches.

{{id recursive_puzzle}}
{{index recursion, "number puzzle example"}}

Consider this puzzle: by starting from the number 1 and repeatedly either adding 5 or multiplying by 3, an infinite set of numbers can be produced. How would you write a function that, given a number, tries to find a sequence of such additions and multiplications that produces that number? For example, the number 13 could be reached by first multiplying by 3 and then adding 5 twice, whereas the number 15 cannot be reached at all.

Here is a recursive solution:

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

Note that this program doesn't necessarily find the _shortest_ sequence of operations. It is satisfied when it finds any sequence at all.

It's okay if you don't see how this code works right away. Let's work through it, since it makes for a great exercise in recursive thinking.

The inner function `find` does the actual recursing. It takes two ((argument))s: the current number and a string that records how we reached this number. If it finds a solution, it returns a string that shows how to get to the target. If it can find no solution starting from this number, it returns `null`.

{{index null, "?? operator", "short-circuit evaluation"}}

To do this, the function performs one of three actions. If the current number is the target number, the current history is a way to reach that target, so it is returned. If the current number is greater than the target, there's no sense in further exploring this branch because both adding and multiplying will only make the number bigger, so it returns `null`. Finally, if we're still below the target number, the function tries both possible paths that start from the current number by calling itself twice, once for addition and once for multiplication. If the first call returns something that is not `null`, it is returned. Otherwise, the second call is returned, regardless of whether it produces a string or `null`.

{{index "call stack"}}

To better understand how this function produces the effect we're looking for, let's look at all the calls to `find` that are made when searching for a solution for the number 13:

```{lang: null}
find(1, "1")
  find(6, "(1 + 5)")
    find(11, "((1 + 5) + 5)")
      find(16, "(((1 + 5) + 5) + 5)")
        too big
      find(33, "(((1 + 5) + 5) * 3)")
        too big
    find(18, "((1 + 5) * 3)")
      too big
  find(3, "(1 * 3)")
    find(8, "((1 * 3) + 5)")
      find(13, "(((1 * 3) + 5) + 5)")
        found!
```

The indentation indicates the depth of the call stack. The first time `find` is called, the function starts by calling itself to explore the solution that starts with `(1 + 5)`. That call will further recurse to explore _every_ continued solution that yields a number less than or equal to the target number. Since it doesn't find one that hits the target, it returns `null` back to the first call. There the `??` operator causes the call that explores `(1 * 3)` to happen. This search has more luck—its first recursive call, through yet _another_ recursive call, hits upon the target number. That innermost call returns a string, and each of the `??` operators in the intermediate calls passes that string along, ultimately returning the solution.

## Growing functions

{{index [function, definition]}}

There are two more or less natural ways for functions to be introduced into programs.

{{index repetition}}

The first occurs when you find yourself writing similar code multiple times. You'd prefer not to do that, as having more code means more space for mistakes to hide and more material to read for people trying to understand the program. So you take the repeated functionality, find a good name for it, and put it into a function.

The second way is that you find you need some functionality that you haven't written yet and that sounds like it deserves its own function. You start by naming the function, then write its body. You might even start writing code that uses the function before you actually define the function itself.

{{index [function, naming], [binding, naming]}}

How difficult it is to find a good name for a function is a good indication of how clear a concept it is that you're trying to wrap. Let's go through an example.

{{index "farm example"}}

We want to write a program that prints two numbers: the numbers of cows and chickens on a farm, with the words `Cows` and `Chickens` after them and zeros padded before both numbers so that they are always three digits long:

```{lang: null}
007 Cows
011 Chickens
```

This asks for a function of two arguments—the number of cows and the number of chickens. Let's get coding.

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

Writing `.length` after a string expression will give us the length of that string. Thus, the `while` loops keep adding zeros in front of the number strings until they are at least three characters long.

Mission accomplished! But just as we are about to send the farmer the code (along with a hefty invoice), she calls and tells us she's also started keeping pigs, and couldn't we please extend the software to also print pigs?

{{index "copy-paste programming"}}

We sure can. But just as we're in the process of copying and pasting those four lines one more time, we stop and reconsider. There has to be a better way. Here's a first attempt:

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

It works! But that name, `printZeroPaddedWithLabel`, is a little awkward. It conflates three things—printing, zero-padding, and adding a label—into a single function.

{{index "zeroPad function"}}

Instead of lifting out the repeated part of our program wholesale, let's try to pick out a single _concept_:

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

A function with a nice, obvious name like `zeroPad` makes it easier for someone who reads the code to figure out what it does. Such a function is also useful in more situations than just this specific program. For example, you could use it to help print nicely aligned tables of numbers.

{{index [interface, design]}}

How smart and versatile _should_ our function be? We could write anything, from a terribly simple function that can only pad a number to be three characters wide to a complicated generalized number-formatting system that handles fractional numbers, negative numbers, alignment of decimal dots, padding with different characters, and so on.

A useful principle is to refrain from adding cleverness unless you are absolutely sure you're going to need it. It can be tempting to write general "((framework))s" for every bit of functionality you come across. Resist that urge. You won't get any real work done—you'll be too busy writing code that you never use.

{{id pure}}
## Functions and side effects

{{index "side effect", "pure function", [function, purity]}}

Functions can be roughly divided into those that are called for their side effects and those that are called for their return value (though it is also possible to both have side effects and return a value).

{{index reuse}}

The first helper function in the ((farm example)), `printZeroPaddedWithLabel`, is called for its side effect: it prints a line. The second version, `zeroPad`, is called for its return value. It is no coincidence that the second is useful in more situations than the first. Functions that create values are easier to combine in new ways than functions that directly perform side effects.

{{index substitution}}

A _pure_ function is a specific kind of value-producing function that not only has no side effects but also doesn't rely on side effects from other code—for example, it doesn't read global bindings whose value might change. A pure function has the pleasant property that, when called with the same arguments, it always produces the same value (and doesn't do anything else). A call to such a function can be substituted by its return value without changing the meaning of the code. When you are not sure that a pure function is working correctly, you can test it by simply calling it and know that if it works in that context, it will work in any context. Nonpure functions tend to require more scaffolding to test.

{{index optimization, "console.log"}}

Still, there's no need to feel bad when writing functions that are not pure. Side effects are often useful. There's no way to write a pure version of `console.log`, for example, and `console.log` is good to have. Some operations are also easier to express in an efficient way when we use side effects.

## Summary

This chapter taught you how to write your own functions. The `function` keyword, when used as an expression, can create a function value. When used as a statement, it can be used to declare a binding and give it a function as its value. Arrow functions are yet another way to create functions.

```
// Define f to hold a function value
const f = function(a) {
  console.log(a + 2);
};

// Declare g to be a function
function g(a, b) {
  return a * b * 3.5;
}

// A less verbose function value
let h = a => a % 3;
```

A key part of understanding functions is understanding scopes. Each block creates a new scope. Parameters and bindings declared in a given scope are local and not visible from the outside. Bindings declared with `var` behave differently—they end up in the nearest function scope or the global scope.

Separating the tasks your program performs into different functions is helpful. You won't have to repeat yourself as much, and functions can help organize a program by grouping code into pieces that do specific things.

## Exercises

### Minimum

{{index "Math object", "minimum (exercise)", "Math.min function", minimum}}

The [previous chapter](program_structure#return_values) introduced the standard function `Math.min` that returns its smallest argument. We can write a function like that ourselves now. Define the function `min` that takes two arguments and returns their minimum.

{{if interactive

```{test: no}
// Your code here.

console.log(min(0, 10));
// → 0
console.log(min(0, -10));
// → -10
```
if}}

{{hint

{{index "minimum (exercise)"}}

If you have trouble putting braces and parentheses in the right place to get a valid function definition, start by copying one of the examples in this chapter and modifying it.

{{index "return keyword"}}

A function may contain multiple `return` statements.

hint}}

### Recursion

{{index recursion, "isEven (exercise)", "even number"}}

We've seen that we can use `%` (the remainder operator) to test whether a number is even or odd by using `% 2` to see whether it's divisible by two. Here's another way to define whether a positive whole number is even or odd:

- Zero is even.

- One is odd.

- For any other number _N_, its evenness is the same as _N_ - 2.

Define a recursive function `isEven` corresponding to this description. The function should accept a single parameter (a positive, whole number) and return a Boolean.

{{index "stack overflow"}}

Test it on 50 and 75. See how it behaves on -1. Why? Can you think of a way to fix this?

{{if interactive

```{test: no}
// Your code here.

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

Your function will likely look somewhat similar to the inner `find` function in the recursive `findSolution` [example](functions#recursive_puzzle) in this chapter, with an `if`/`else if`/`else` chain that tests which of the three cases applies. The final `else`, corresponding to the third case, makes the recursive call. Each of the branches should contain a `return` statement or in some other way arrange for a specific value to be returned.

{{index "stack overflow"}}

When given a negative number, the function will recurse again and again, passing itself an ever more negative number, thus getting further and further away from returning a result. It will eventually run out of stack space and abort.

hint}}

### Bean counting

{{index "bean counting (exercise)", [string, indexing], "zero-based counting", ["length property", "for string"]}}

You can get the *N*th character, or letter, from a string by writing `[N]` after the string (for example, `string[2]`). The resulting value will be a string containing only one character (for example, `"b"`). The first character has position 0, which causes the last one to be found at position `string.length - 1`. In other words, a two-character string has length 2, and its characters have positions 0 and 1.

Write a function `countBs` that takes a string as its only argument and returns a number that indicates how many uppercase B characters there are in the string.

Next, write a function called `countChar` that behaves like `countBs`, except it takes a second argument that indicates the character that is to be counted (rather than counting only uppercase B characters). Rewrite `countBs` to make use of this new function.

{{if interactive

```{test: no}
// Your code here.

console.log(countBs("BOB"));
// → 2
console.log(countChar("kakkerlak", "k"));
// → 4
```

if}}

{{hint

{{index "bean counting (exercise)", ["length property", "for string"], "counter variable"}}

Your function will need a ((loop)) that looks at every character in the string. It can run an index from zero to one below its length (`< string.length`). If the character at the current position is the same as the one the function is looking for, it adds 1 to a counter variable. Once the loop has finished, the counter can be returned.

{{index "local binding"}}

Take care to make all the bindings used in the function _local_ to the function by properly declaring them with the `let` or `const` keyword.

hint}}
