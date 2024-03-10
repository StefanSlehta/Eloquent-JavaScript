# Struktura programa

{{quote {author: "_why", title: "Why's (Poignant) Guide to Ruby", chapter: true}

I moje srce sija jarko crveno ispod mog prozirnog kožnog filma i moraju mi dati 10cc JavaScript-a da se vratim. (Dobro reagujem na toksine u krvi.)

quote}}

{{index why, "Poignant Guide"}}

{{figure {url: "img/chapter_picture_2.jpg", alt: "Ilustracija pokazuje pipke hobotnice kako drze razne sahovske figure", chapter: framed}}}

U ovom poglavlju, počećemo da radimo stvari koje se zapravo mogu nazvati _programiranjem_. Proširićemo naše poznavanje JavaScript-a izvan imenica i prostih rečenica koje smo dosad videli do tačke gde možemo pisati značajne proze.

## Expressions i statements

{{index grammar, [syntax, expression], [code, "structure of"], grammar, [JavaScript, syntax]}}

U [Poglavlju ?](values), kreirali smo vrednosti i primenili operatore na njih da bismo dobili nove vrednosti. Kreiranje vrednosti na ovaj način je osnovna suština svakog JavaScript programa. Međutim, ta suština mora biti oblikovana u veću strukturu da bi bila korisna. To ćemo obraditi u ovom poglavlju.

{{index "literal expression", [parentheses, expression]}}

Deo koda koji proizvodi vrednost naziva se _((izraz))_ (_expression_). Svaka vrednost koja je napisana doslovno (kao što su `22` ili `"psihoanaliza"`) je izraz. Izraz između zagrada takođe je izraz, kao i ((binarni operator)) primenjen na dva izraza ili ((unarni operator)) primenjen na jedan.

{{index [nesting, "of expressions"], "human language"}}

Ovo pokazuje deo lepote jezičkog interfejsa. Izrazi mogu sadržavati druge izraze na način sličan kao rečenice u ljudskim jezicima - jedna rečenica može sadržati više manjih rečenica u sebi, koje takođe mogu imati još manje rečenice u sebi i tako dalje. To nam omogućava da izgradimo izraze koji opisuju proizvoljno kompleksna računanja.

{{index statement, semicolon, program}}

Ako izraz analogno odgovara delu rečenice, onda je _statement_ (izjava) u JavaScript-u puna rečenica. Program je zapravo lista izjava tj. statementa.

{{index [syntax, statement]}}

Najjednostavnija vrsta statementa je izraz koji završava sa tačka-zapeta(;). Ovo je program:

```
1;
!false;
```

Doduše, ovo je beskoristan program. Izraz može biti samo da proizvede vrednost, koja se zatim može koristiti od strane drugog koda. Međutim, statement stoji sama za sebe, pa ako ne utiče na svet oko sebe, beskorisna je. Može prikazati nešto na ekranu, kao npr. `console.log`, ili promeniti stanje mašine na način koji će uticati na izjave koje dolaze nakon nje. Ove promene se nazivaju _((sporedni efekti))_. Izjave u prethodnom primeru samo proizvode vrednosti `1` i `true`, a zatim ih odmah odbacuju. To uopšte ne ostavlja efekat na svet oko sebe. Kada pokrenete ovaj program, ništa konkretno se ne dešava.

{{index "programming style", "automatic semicolon insertion", semicolon}}

U nekim slučajevima, JavaScript vam dozvoljava da izostavite tačku-zarez na kraju izjave. U drugim slučajevima, ona mora biti prisutna, ili će sledeći ((red)) biti tretiran kao deo iste izjave. Pravila kada je možete bezbedno izostaviti su prilično složena i podložna greškama. Stoga, u ovoj knjizi, svaka izjava koja zahteva tačku-zarez na kraju. Preporučujem vam da učinite isto, barem dok ne naučite više o suptilnostima nedostajućih tačaka-zareza.

## Bindings

{{indexsee variable, binding}}
{{index [syntax, statement], [binding, definition], "side effect", [memory, organization], [state, in binding]}}

Kako program održava interno stanje? Kako pamti stvari? Videli smo kako da proizvedemo nove vrednosti iz starih vrednosti, ali ovo ne menja stare vrednosti, i nova vrednost mora odmah da se koristi ili će opet nestati. Da sačuva vrednosti, JavaScript pruža nešto što se zove _binding_, ili _promenljiva_ tj _varijabla_:

```
let caught = 5 * 5;
```

{{index "let keyword"}}

Ovo nam daje drugu vrstu ((izjave)). Posebna reč (_((ključna reč))_) `let` ukazuje da će ova rečenica definisati varijablu. Za njom sledi ime varijable i, ako želimo odmah da joj damo vrednost, operator `=` i izraz.

Primer kreira varijablu nazvanu `caught` i koristi je da sačuva broj koji se dobija množenjem 5 sa 5.

Kada je varijabla definisana, njeno ime može biti korišćeno kao ((izraz)). Vrednost takvog izraza je vrednost koju trenutno drži vrijabla. Evo primera:

```
let deset = 10;
console.log(deset * deset);
// → 100
```

{{index "= operator", assignment, [binding, assignment]}}

Kada varijabla pokazuje na vrednost, to ne znači da je vezana za tu vrednost zauvek. Operator `=` može biti korišćen u bilo kom trenutku na postojećim varijablama da bi ih odvojio od njihove trenutne vrednosti i usmerio ih na novu:

```
let mood = "light";
console.log(mood);
// → light
mood = "dark";
console.log(mood);
// → dark
```

{{index [binding, "model of"], "tentacle (analogy)"}}

Trebalo bi da zamislite varijable kao pipke umesto kutija u kojima se čuvaju stvari. One ne _sadrže_ vrednosti; one ih _vežu_ — dva vezanja tj varijable mogu se odnositi na istu vrednost. Program može pristupiti samo vrednostima na koje još uvek ima referencu. Kada treba da zapamtite nešto, ili napravite nove pipke da biste se vezali za to što pamtite ili ponovo prikačite jedan od vaših postojećih pipaka za to.

Pogledajmo još jedan primer. Da biste zapamtili koliko vam dolara Luigi još uvek duguje, napravite varijablu. Kada vam vrati $35, dajete ovoj varijabli novu vrednost:

```
let luigisDebt = 140;
luigisDebt = luigisDebt - 35;
console.log(luigisDebt);
// → 105
```

{{index undefined}}

Kada definišete varijablu ne dajući joj vrednost, pipak nema ništa čega bi se uhvatio, tako da ostaje praznih ruku. Ako zatražite vrednost praznog pipka, dobićete vrednost `undefined`.

{{index "let keyword"}}

Jedna `let` izjava može definisati više varijabli. Definicije moraju biti odvojene zarezima:

```
let one = 1, two = 2;
console.log(one + two);
// → 3
```

Reči `var` i `const` takođe se mogu koristiti za kreiranje binding-a, na sličan način kao i `let`:

```
var name = "Ayda";
const greeting = "Hello ";
console.log(greeting + name);
// → Hello Ayda
```

{{index "var keyword"}}

`var` (skraćeno za "promenljiva" tj. varijabla), je način na koji su vezanja (bindings) deklarisana u JavaScriptu pre 2015. godine, kada `let` još nije postojao. Precizno ćemo opisati kako se razlikuje od `let` u [sledećem poglavlju](functions). Za sada, zapamtite da uglavnom radi istu stvar, ali ćemo je retko koristiti u ovoj knjizi jer se ponaša čudno u nekim situacijama.

{{index "const keyword", naming}}

Reč `const` predstavlja _((konstantu))_. Ona definiše konstantan binding, i pokazuje na istu vrednost dok god postoji. Ovo je korisno za vezanja koja samo daju ime vrednosti kako biste je kasnije lako mogli koristiti.

## Vezivanje imena

{{index "underscore character", "dollar sign", [binding, naming]}}

Imena binding-a mogu biti bilo koji niz od jednog ili više slova. Cifre mogu biti deo imena varijable — `catch22` je validno ime, na primer — ali ime ne sme početi cifrom. Ime varijable može uključivati dolarske znakove (`$`) ili donje crte (`_`), ali ne i druge interpunkcijske ili posebne znakove.

{{index [syntax, identifier], "implements (reserved word)", "interface (reserved word)", "package (reserved word)", "private (reserved word)", "protected (reserved word)", "public (reserved word)", "static (reserved word)", "void operator", "yield (reserved word)", "enum (reserved word)", "reserved word", [binding, naming]}}

Reči sa posebnim značenjem, poput `let`, su _((ključne reči))_, i ne mogu se koristiti kao imena varijable. Postoji i niz reči koje su "rezervisane za korišćenje" u ((budućim)) verzijama JavaScripta, koje takođe ne mogu biti korišćene kao imena varijable. Kompletna lista ključnih reči i rezervisanih reči je prilično duga:

```{lang: "null"}
break case catch class const continue debugger default
delete do else enum export extends false finally for
function if implements import interface in instanceof let
new package private protected public return static super
switch this throw true try typeof var void while with yield
```

{{index [syntax, error]}}

Nemojte se brinuti oko pamćenja ove liste. Kada kreiranje binding-a proizvede neočekivanu sintaksnu grešku, samo proverite da li pokušavate da definišete rezervisanu reč.

## Okruženje

{{index "standard environment", [browser, environment]}}

Skup svih varijabli i njihovih vrednosti koji postoje u datom trenutku naziva se _((okruženje))_. Kada program počne, ovo okruženje nije prazno. Uvek sadrži varijable koja su deo jezičkog ((standarda)), i većinu vremena, takođe ima varijable koje pružaju načine za interakciju sa okolnim sistemom. Na primer, u pretraživaču postoje funkcije za interakciju sa trenutno učitanom veb stranicom i za čitanje ((miša)) i ((tastature)).

## Funkcije

{{indexsee "application (of functions)", [function, application]}}
{{indexsee "invoking (of functions)", [function, application]}}
{{indexsee "calling (of functions)", [function, application]}}
{{index output, function, [function, application], [browser, environment]}}

Mnoge vrednosti koje se pružaju u podrazumevanom okruženju imaju tip _((funkcije))_. Funkcija je deo programa sačuvan u vrednost. Takve vrednosti se mogu _primeniti_ (aplicirati) kako bi se pokrenuo sačuvani program. Na primer, u pretraživaču, binding `prompt` sadrži funkciju koja prikazuje mali ((dijalog)) koji traži korisnički unos. Koristi se na sledeći način:

```
prompt("Enter passcode");
```

{{figure {url: "img/prompt.png", alt: "Dijalog koji prikazuje tekst 'enter passcode'", width: "8cm"}}}

{{index parameter, [function, application], [parentheses, arguments]}}

Izvršavanje funkcije se naziva _pozivanje_, ili _primena_. Funkciju možete pozvati tako što ćete staviti zagrade nakon izraza koji proizvodi vrednost funkcije. Obično ćete direktno koristiti ime binding-a koji drži funkciju. Vrednosti između zagrada se prosleđuju programu unutar funkcije. U prethodnom primeru, funkcija `prompt` koristi string koji joj dajemo kao tekst koji treba prikazati u dijalogu. Vrednosti prosleđene funkcijama se nazivaju _((argumenti))_. Funkcije mogu zahtevati različit broj ili različite tipove argumenata.

Funkcija `prompt` se ne koristi mnogo u modernom veb programiranju, uglavnom zato što nemate kontrolu nad izgledom dijaloga koji ona pravi, ali može biti korisna u igricama i eksperimentima.

## console.log funkcija

{{index "JavaScript console", "developer tools", "Node.js", "console.log", output, [browser, environment]}}

U nekim primerima, koristio sam `console.log` da bih ispisao vrednosti. Većina JavaScript sistema (uključujući sve moderne veb pretraživače i Node.js) pruža funkciju `console.log` koja ispisuje svoje argumente na _neki_ uređaj za tekstualni izlaz. U pretraživačima, izlaz završava u ((JavaScript konzoli)). Ovaj deo browser interfejsa je podrazumevano sakriven, ali većina pretraživača ga otvara kada pritisnete F12 ili, na Mac-u, [command]{keyname}-[option]{keyname}-I. Ako to ne radi, potražite kroz meni stavku nazvanu Developer Tools ili slično.

{{if interactive

Kada pokrećete primere (ili svoj sopstveni kod) na stranicama ove knjige, izlaz `console.log` će biti prikazan odmah ispod primera, umesto u konzoli za JavaScript pregledača.

```
let x = 30;
console.log("the value of x is", x);
// → the value of x is 30
```

if}}

{{index [object, property], [property, access]}}

Iako imena binding-a (varijabli) ne mogu sadržavati ((tačku)), `console.log` izgleda da sadrži jednu. To je zato što `console.log` nije jednostavna varijabla, već izraz koji poziva svojstvo `log` iz vrednosti koju drži varijabla `console`. Saznaćemo šta to tačno znači u [Poglavlju ?](data#properties).

{{id return_values}}
## Return values

{{index [comparison, "of numbers"], "return value", "Math.max function", maximum}}

Prikazivanje dijaloga ili pisanje teksta na ekran je _((sporedni efekat))_. Mnoge funkcije su korisne zbog sporednih efekata koje proizvode. Funkcije takođe mogu proizvoditi vrednosti, u tom slučaju im nije potreban sporedni efekat da bi bile korisne. Na primer, funkcija `Math.max` uzima bilo koji broj argumenata i vraća najveći:

```
console.log(Math.max(2, 4));
// → 4
```

{{index [function, application], minimum, "Math.min function"}}

Kada funkcija proizvode vrednost, kaže se da ona _vraća_ tu vrednost. Bilo šta što proizvodi vrednost je ((izraz)) u JavaScriptu, što znači da pozivi funkcija mogu biti korišćeni unutar većih izraza. U sledećem kodu, poziv funkcije `Math.min`, koji je suprotan od `Math.max`, koristi se kao deo izraza za sabiranje:

```
console.log(Math.min(2, 4) + 100);
// → 102
```

[Poglavlje ?](functions) će objasniti kako da pišete svoje funkcije.

## Kontrola toka programa

{{index "execution order", program, "control flow"}}

Kada vaš program sadrži više od jedne ((izjave)), izjave se izvršavaju kao da su priča, od vrha do dna. Na primer, sledeći program ima dve izjave. Prva traži broj od korisnika, a druga, koja se izvršava nakon prve, prikazuje kvadrat tog broja:

```
let theNumber = Number(prompt("Odaberite broj"));
console.log("Broj koji ste izabrali je kvadratni koren broja " +
            theNumber * theNumber);
```

{{index [number, "conversion to"], "type coercion", "Number function", "String function", "Boolean function", [Boolean, "conversion to"]}}

Funkcija `Number` pretvara vrednost u broj. Potrebna nam je ta konverzija jer je rezultat `prompt` funkcije string vrednost, a mi želimo broj. Postoje slične funkcije nazvane `String` i `Boolean` koje pretvaraju vrednosti u te tipove.

Evo prilično trivijalne shematske reprezentacije kontrolnog toka u ravnoj liniji:

{{figure {url: "img/controlflow-straight.svg", alt: "Dijagram koji prikazuje ravnu strelicu", width: "4cm"}}}

## Uslovno izvršavanje (conditionals)

{{index Boolean, ["control flow", conditional]}}

Nisu svi programi ravne linije. Možda želimo da kreiramo granajuću stazu gde program ide na odgovarajuću granu na osnovu situacije koja je na vidiku. Ovo se naziva _((uslovno izvršavanje))_ ili _conditional_.

{{figure {url: "img/controlflow-if.svg", alt: "Diagram of an arrow that splits in two, and then rejoins again",width: "4cm"}}}

{{index [syntax, statement], "Number function", "if keyword"}}

Uslovno izvršavanje se kreira pomoću `if` ključne reči u JavaScriptu. U jednostavnom slučaju, želimo da određeni kod bude izvršen ako, i samo ako, određeni uslov važi. Na primer, možda želimo da prikažemo kvadrat ulaza samo ako je ulaz zaista broj:

```{test: wrap}
let theNumber = Number(prompt("Pick a number"));
if (!Number.isNaN(theNumber)) {
  console.log("Your number is the square root of " +
              theNumber * theNumber);
}
```

Uz ovu promenu, ako unesete "papagaj", nikakav rezultat neće biti prikazan.

{{index [parentheses, statement]}}

Ključna reč `if` izvršava ili preskače izjavu u zavisnosti od vrednosti boolean izraza. Conditional izraz se piše nakon ključne reči, između zagrada, a zatim sledi izjava koju treba izvršiti.

{{index "Number.isNaN function"}}

Funkcija `Number.isNaN` je standardna JavaScript funkcija koja vraća `true` samo ako je argument koji joj je dat `NaN`. Funkcija `Number` vraća `NaN` kada joj date string koji ne predstavlja validan broj. Dakle, uslov se prevodi kao "ako `theNumber` nije broj, uradi ovo".

{{index grouping, "{} (block)", [braces, "block"]}}

Izjava posle `if` je obavijena zagradama (`{` i `}`) u ovom primeru. Zagrade se mogu koristiti da grupišu bilo koji broj izjava u jednu izjavu, nazvanu _((blok))_. Takođe biste ih mogli izostaviti u ovom slučaju, pošto sadrže samo jednu izjavu, ali da biste izbegli razmišljanje o tome da li su potrebne, većina JavaScript programera ih koristi u svakoj blok izjavi poput ove. Većinom ćemo pratiti tu konvenciju u ovoj knjizi, osim za povremene jednolinijske izjave.

```
if (1 + 1 == 2) console.log("It's true");
// → It's true
```

{{index "else keyword"}}

Često ćete imati kod koji se izvršava ne samo kada uslov važi, već i kod koji obrađuje drugi slučaj. Ovaj alternativni put predstavljen je drugom strelicom na dijagramu. Možete koristiti `else` ključnu reč, zajedno sa `if`, da biste kreirali dva odvojena, alternativna puta izvršavanja:

```{test: wrap}
let theNumber = Number(prompt("Pick a number"));
if (!Number.isNaN(theNumber)) {
  console.log("Your number is the square root of " +
              theNumber * theNumber);
} else {
  console.log("Hey. Why didn't you give me a number?");
}
```

{{index ["if keyword", chaining]}}

Ako imate više od dva puta za biranje, možete "vezati" više `if`/`else` izjava zajedno. Evo primera:

```
let num = Number(prompt("Pick a number"));

if (num < 10) {
  console.log("Small");
} else if (num < 100) {
  console.log("Medium");
} else {
  console.log("Large");
}
```

Program će prvo proveriti da li je `num` manje od 10. Ako jeste, izabraće tu granu, prikazati `"Small"`, i to je to. Ako nije, uzima `else` granu, koja sama sadrži drugi `if`. Ako drugi uslov (`< 100`) važi, to znači da je broj barem 10 ali ispod 100, i prikazuje se `"Medium"`. Ako nije, birana je druga i poslednja `else` grana.

Šema za ovaj program izgleda otprilike ovako:

{{figure {url: "img/controlflow-nested-if.svg", alt: "Dijagram prikazuje strelicu koja se razdvaja na dve strane, jedna grana se dodatno razdvaja, a onda se sve grane opet spajaju u jednu", width: "4cm"}}}

{{id loops}}
## while i do petlje

Razmotrite program koji ispisuje sve ((parne brojeve)) od 0 do 12. Jedan način da se to napiše je sledeći:

```
console.log(0);
console.log(2);
console.log(4);
console.log(6);
console.log(8);
console.log(10);
console.log(12);
```

{{index ["control flow", loop]}}

To funkcioniše, ali ideja pisanja programa je da se napravi _manje_ posla, ne više. Ako su nam potrebni svi parni brojevi manji od 1.000, ovaj pristup bi bio neizvodljiv. Ono što nam treba je način da izvršimo komad koda, više puta. Ova forma kontrolnog toka se naziva _((petlja))_.

{{figure {url: "img/controlflow-loop.svg", alt: "Dijagram koji pokazuje strelicu ka tački koja ima cikličnu strelicu koja se vraća na sebe i još jednu strelicu koja ide dalje.", width: "4cm"}}}

{{index [syntax, statement], "counter variable"}}

Petlja nam omogućava da se vratimo na neku tačku u programu gde smo bili pre i ponovimo je sa našim trenutnim stanjem programa. Ako to kombinujemo sa varijablom koja broji, možemo uraditi nešto poput ovoga:

```
let number = 0;
while (number <= 12) {
  console.log(number);
  number = number + 2;
}
// → 0
// → 2
//   … itd
```

{{index "while loop", Boolean, [parentheses, statement]}}

((Izjava)) koja počinje ključnom rečju `while` kreira petlju. Reč `while` je praćena ((izrazom)) u zagradama, a zatim izjavom, slično kao `if`. Petlja nastavlja da izvršava tu izjavu sve dok izraz proizvodi vrednost koja daje `true` kada se konvertuje u Boolean.

{{index [state, in binding], [binding, as state]}}

Varijabla `number` pokazuje kako varijabla može pratiti tok programa. Svaki put kada se petlja ponavlja, `number` dobija vrednost koja je za 2 veća od njene prethodne vrednosti. Na početku svake iteracije, upoređuje se sa brojem 12 da bi se odlučilo da li je završen rad programa.

{{index exponentiation}}

Kao primer koji zaista nešto radi korisno, sada možemo napisati program koji izračunava i prikazuje vrednost 2^10^ (2 na 10-ti stepen). Koristimo dve varijable: jednu da pratimo naš rezultat, a drugu da brojimo koliko smo puta pomnožili taj rezultat sa 2. Petlja testira da li je druga varijabla već dostigla 10, i ako nije, ažurira obe.

```
let result = 1;
let counter = 0;
while (counter < 10) {
  result = result * 2;
  counter = counter + 1;
}
console.log(result);
// → 1024
```

Brojač takođe može početi od `1` i proveriti da li je `<= 10`, ali iz razloga koji će postati očigledni u [Poglavlju ?](data#array_indexing), dobra je ideja naviknuti se na brojanje od 0.

{{index "** operator"}}

Napomena da JavaScript takođe ima operator za stepenovanje (`2 ** 10`), što biste koristili da izračunate ovo u stvarnom kodu — ali to bi nam pokvarilo primer.

{{index "loop body", "do loop", ["control flow", loop]}}

`do` petlja je kontrolna struktura slična `while` petlji. Razlikuje se samo po jednoj tački: `do` petlja uvek izvršava svoj blok barem jednom, i počinje testiranje da li treba da se zaustavi tek nakon te prve izvršene iteracije. Dakle, test se pojavljuje nakon tela petlje:

```
let ime;
do {
  ime = prompt("Ko ste vi?");
} while (!ime);
console.log("Hello " + ime);
```

{{index [Boolean, "conversion to"], "! operator"}}

Ovaj program će vas primorati da unesete ime. Pitaće stalno iznova dok ne dobije nešto što nije prazan string. Primena operatora `!` će pretvoriti vrednost koju ste uneli u Boolean tip pre negiranja, a svi stringovi osim `""` se pretvaraju u `true`. Ovo znači da petlja nastavlja da se vrti dok ne unesete ime koje nije prazno.

## Formatiranje koda

{{index [code, "structure of"], [whitespace, indentation], "programming style"}}

U dosadašnjim primerima, dodavao sam razmake ispred izjava u kodu koje su deo neke veće izjave. Ti razmaci nisu obavezni — računar će prihvatiti program i bez njih. Zapravo, čak su i ((nove linije)) u programima opcionalni. Mogli biste napisati program kao jednu dugu liniju ako baš želite.

Uloga ovog ((uvlačenja)) koda unutar ((blokova)) je da istakne strukturu koda čitaocima. U kodu gde se novi blokovi otvaraju unutar drugih blokova, može postati teško videti gde se jedan blok završava, a drugi počinje. Sa odgovarajućim uvlačenjem, vizuelni oblik programa odgovara obliku blokova unutar njega. Volim da koristim dva razmaka za svaki otvoreni blok, ali ukusi se razlikuju — neki ljudi koriste četiri razmaka, a neki koriste ((tab karakter))e. Važno je da svaki novi blok dodaje istu količinu prostora.

```
if (false != true) {
  console.log("That makes sense.");
  if (1 < 2) {
    console.log("No surprise there.");
  }
}
```

Većina programa za uređivanje koda[ (uključujući i ovaj u knjizi)]{if interactive} pomažu automatskim uvlačenjem novih linija u odgovarajućoj meri.

## for petlje

{{index [syntax, statement], "while loop", "counter variable"}}

Mnoge petlje prate obrazac prikazan u primerima sa `while` petljama. Prvo se kreira "brojač" varijabla kako bi se pratila progresija petlje. Zatim dolazi `while` petlja, obično sa testnim izrazom koji proverava da li je brojač dostigao svoju krajnju vrednost. Na kraju tela petlje, brojač se ažurira kako bi se pratio progres.

{{index "for loop", loop}}

Pošto je ovaj obrazac tako čest, JavaScript i slični jezici pružaju nešto kraću i sveobuhvatniju formu petlji, `for` petlju:

```
for (let number = 0; number <= 12; number = number + 2) {
  console.log(number);
}
// → 0
// → 2
//   … itd
```

{{index ["control flow", loop], state}}

Ovaj program je potpuno ekvivalentan [ranijem](program_structure#loops) primeru ispisa parnih brojeva. Jedina promena je što su sve ((izjave)) koje su vezane za "stanje" petlje grupisane zajedno nakon `for`.

{{index [binding, as state], [parentheses, statement]}}

Zagrade nakon ključne reči `for` moraju sadržati dva ((tačka-zarez)) znaka. Deo pre prvog tačka-zareza _inicijalizuje_ petlju, obično definisanjem varijable. Drugi deo je ((izraz)) koji _proverava_ da li petlja treba da se nastavi. Poslednji deo _ažurira_ stanje petlje nakon svake iteracije. U većini slučajeva, ovo je kraće i jasnije od konstrukcije `while`.

{{index exponentiation}}

Ovo je kod koji računa 2^10^ koristeći `for` umesto `while` petlje:

```{test: wrap}
let result = 1;
for (let counter = 0; counter < 10; counter = counter + 1) {
  result = result * 2;
}
console.log(result);
// → 1024
```

## Prekid petlje, ili, iskakanje iz petlje

{{index [loop, "termination of"], "break keyword"}}

Cekanje da uslov petlje postane `false` nije jedini način na koji petlja može završiti. Kljucna reč `break` ima efekat direktnog izlaska iz  petlje u kojoj se nalazi. Njena upotreba je prikazana u sledećem programu, koji pronalazi prvi broj koji je veći ili jednak 20 i deljiv sa 7:

```
for (let current = 20; ; current = current + 1) {
  if (current % 7 == 0) {
    console.log(current);
    break;
  }
}
// → 21
```

{{index "remainder operator", "% operator"}}

Korišćenje modulo operatora (`%`) je jednostavan način da se testira da li je broj deljiv drugim brojem. Ako jeste, ostatak njihovog deljenja je nula.

{{index "for loop"}}

`for` petlja u prethodnom primeru nema deo koji proverava kraj petlje. To znači da će petlja nastaviti da se izvršava sve dok se `break` izjava unutar nje ne izvrši.

Ako biste uklonili tu `break` izjavu ili slučajno napisali uslov koji uvek proizvodi `true`, vaš program bi zaglavio u _((beskonačnoj petlji))_. Program zaglavljen u beskonačnoj petlji nikada neće prekinuti izvršavanje, što je obično loša stvar.

{{if interactive

Ako napravite beskonačnu petlju u jednom od primera na ovim stranicama, obično će vam biti rečeno da zaustavite skriptu nakon nekoliko sekundi. Ako to ne uspe, moraćete da zatvorite tab na kojem radite da biste je prekinuli.

if}}

{{index "continue keyword"}}

Ključna reč `continue` je slična kao `break` jer utiče na napredak petlje. Kada se `continue` naže u telu petlje, kontrola izlazi iz tela i nastavlja sa sledećom iteracijom petlje.

## Kraći način ažuriranja varijabli

{{index assignment, "+= operator", "-= operator", "/= operator", "*= operator", [state, in binding], "side effect"}}

Posebno unutar petlji, program često treba "ažurirati" varijable kako bi zadržao vrednost na osnovu prethodne vrednosti te varijable.

```{test: no}
counter = counter + 1;
```

JavaScript nudi prečicu za ovaj tip izjave

```{test: no}
counter += 1;
```

Slični prečaci rade i za mnoge druge operatore, kao što je `rezultat *= 2` da se udvostruči `rezultat` ili `brojač -= 1` da se broji unazad.

Ovo nam omogućava da dalje skratimo naš primer brojanja:

```
for (let number = 0; number <= 12; number += 2) {
  console.log(number);
}
```

{{index "++ operator", "-- operator"}}

Za `brojač += 1` i `brojač -= 1`, postoje još kraći ekvivalenti: `brojač++` i `brojač--`.

## Raspodela na vrednosti pomoću `switch`

{{index [syntax, statement], "conditional execution", dispatch, ["if keyword", chaining]}}

Neretko će kod izgledati ovako:

```{test: no}
if (x == "value1") action1();
else if (x == "value2") action2();
else if (x == "value3") action3();
else defaultAction();
```

{{index "colon character", "switch keyword"}}

Postoji konstrukt nazvan `switch` koji je namenjen izražavanju takve "raspodele" na direktniji način. Nažalost, sintaksa koju JavaScript koristi za ovo (koja je nasleđena od programskih jezika poput C/Java) je pomalo nezgrapna — lanac `if` izjava može izgledati bolje. Evo primera:

```
switch (prompt("Kakvo je vreme?")) {
  case "kisa":
    console.log("Seti se da poneses kisobran.");
    break;
  case "sunce":
    console.log("Obuci se lagano.");
  case "oblacno":
    console.log("Izadji u setnju.");
    break;
  default:
    console.log("Nepoznat tip vremena!");
    break;
}
```

{{index fallthrough, "break keyword", "case keyword", "default keyword"}}

Možete staviti bilo koji broj `case` oznaka unutar bloka otvorenog sa `switch`. Program će početi izvršavanje na oznaci koja odgovara vrednosti koju je `switch` dobio, ili na `default` ako nije pronađena odgovarajuća vrednost. Nastaviće izvršavanje, čak i preko drugih oznaka, sve dok ne stigne do `break` izjave. U nekim slučajevima, kao što je slučaj `"sunce"` u primeru, ovo se može koristiti da se podeli neki kod između slučajeva (preporučuje se izlazak napolje i za sunčano i za oblačno vreme). Ipak, budite oprezni—lako je zaboraviti `break`, što će uzrokovati da program izvrši kod koji ne želite da se izvrši.

## Velika početna slova

{{index capitalization, [binding, naming], [whitespace, syntax]}}

Imena varijabli ne smeju sadržati razmake, ali često je korisno koristiti više reči kako bi jasno opisali šta varijabla predstavlja. Ovo su skoro sve opcije koje imate za pisanje imena varijabli sa više reči u njima:

```{lang: null}
fuzzylittleturtle
fuzzy_little_turtle
FuzzyLittleTurtle
fuzzyLittleTurtle
```

{{index "camel case", "programming style", "underscore character"}}

Prvi stil može biti teško čitljiv. Meni se sviđa izgled donjih crta, iako je taj stil malo naporan za kucanje. ((Standardne)) JavaScript funkcije, i većina JavaScript programera, prate poslednji stil — svaku reč osim prve pišu velikim početnim slovom. Nije teško naviknuti se na male stvari poput toga, a kod sa mešanim stilovima imenovanja može biti težak za čitanje, pa zato pratimo ovu ((konvenciju)).

{{index "Number function", constructor}}

U nekoliko slučajeva, kao što je funkcija `Number`, i prvo slovo varijable je takođe veliko. To je urađeno da bi se označilo ovu funkciju kao konstruktor. Biće jasno šta je konstruktor u [Poglavlje ?](object#constructors). Za sada, važno je da vas ne muči ovaj očigledan nedostatak ((konzistentnost)).

## Komentari

{{index readability}}

Često, sam kod ne prenosi sve informacije koje želite da program prenese ljudskim čitaocima, ili to čini na tako kriptičan način da ljudi možda neće razumeti šta zapravo znači. U ostalim slučajevima, možda samo želite da zapišete neke misli kao deo vašeg programa. Za takve stvari tu su _((komentari))_.

{{index "slash character", "line comment"}}

Komentar je deo teksta koji je deo teksta programa, ali ga računar potpuno ignoriše. JavaScript ima dva načina pisanja komentara. Da biste napisali komentar u jednom redu, možete koristiti dve kose crte (`//`) i zatim tekst komentara:

```{test: no}
let accountBalance = calculateBalance(account);
// It's a green hollow where a river sings
accountBalance.adjust();
// Madly catching white tatters in the grass.
let report = new Report();
// Where the sun on the proud mountain rings:
addToReport(accountBalance, report);
// It's a little valley, foaming like light in a glass.
```

{{index "block comment"}}

`//` komentar ide samo do kraja jedne linije. Odeljak teksta između `/*` i `*/` biće potpuno ignorisan, bez obzira na to da li sadrži prelome linije. Ovo je korisno za dodavanje blokova informacija o datoteci ili delu programa:

```
/*
  Ovaj broj sam prvi put pronašao napisan na poleđini stare knjige. 
  Od tada, često sam ga viđao, pojavljujući se u telefonskim brojevima
  i serijskim brojevima proizvoda koje sam kupio. 
  Veoma mi se sviđa, pa sam odlučio da ga zadržim.
*/
const myNumber = 11213;
```

## Da rezimiramo

Sada znate da je program sastavljen od izjava, koje se ponekad i same sastoje od drugih izjava. Izjave na engleskom zovemo _statements_. Izjave obično sadrže izraze (expressions), koji se mogu sastojati od manjih izraza.

Stavljanje izjava jednu za drugom daje vam program koji se izvršava od vrha prema dnu. Možete uvesti promene u tok kontrole korišćenjem uslovnih konstrukata (`if`, `else` i `switch`) i petlji (`while`, `do` i `for`).

Varijable se mogu koristiti za čuvanje komada podataka pod nekim imenom, i korisne su za praćenje stanja u vašem programu. Okruženje je skup varijabli koje su definisane. JavaScript sistemi uvek postavljaju brojne korisne standardne varijable u vaše okruženje.

Funkcije su posebne vrednosti koje enkapsuliraju deo programa. Možete ih pozivati tako što ćete napisati `nazivFunkcije(argument1, argument2)`. Takav poziv funkcije je izraz i može proizvesti vrednost.

## Vežbe

{{index exercises}}

Ako niste sigurni kako da testirate vaša rešenja vežbi, pogledajte [Uvod](intro).

Svaka vežba počinje sa opisom problema. Pročitajte opis i pokušajte da rešite vežbu. Ako naiđete na probleme, razmislite o čitanju sugestija [nakon vežbe]{if interactive}[na [kraju knjige](hints)]{if book}. Mnoga rešenja možete pronaći online na [_https://eloquentjavascript.net/code_](https://eloquentjavascript.net/code#2). Ako želite nešto da naučite iz vežbi, preporučujem da pogledate rešenja tek nakon što rešite vežbu, ili bar nakon što se dovoljno dugo borite sa njom da vas malo zaboli glava.

### Petlja za trougao

{{index "triangle (exercise)"}}

Napišite petlju koja poziva `console.log` 7 puta, i ispisuje sledeći trougao:

```{lang: null}
#
##
###
####
#####
######
#######
```

{{index [string, length]}}

Može vam biti korisno da znate da možete naći dužinu stringa tako sto ćete napisati `.length` nakon stringa. Primer:

```
let abc = "abc";
console.log(abc.length);
// → 3
```

{{if interactive

Većina vežbi sastoji deo koda koji možete modifikovati da biste rešili tu vežbu. Zapamtite da možete kliknuti na blokove koda da ih editujete (izmenite).

```
// Vaš kod ovde.
```
if}}

{{hint

{{index "triangle (exercise)"}}

You can start with a program that prints out the numbers 1 to 7, which you can derive by making a few modifications to the [even number printing example](program_structure#loops) given earlier in the chapter, where the `for` loop was introduced.

Now consider the equivalence between numbers and strings of hash characters. You can go from 1 to 2 by adding 1 (`+= 1`). You can go from `"#"` to `"##"` by adding a character (`+= "#"`). Thus, your solution can closely follow the number-printing program.

hint}}

### FizzBuzz

{{index "FizzBuzz (exercise)", loop, "conditional execution"}}

Napišite program koji koristi `console.log` da ispiše sve brojeve od 1 do 100, sa dva izuzetka. Za brojeve deljive sa 3, umesto broja ispišite `"Fizz"`, a za brojeve deljive sa 5 (ali ne sa 3), ispišite `"Buzz"`.

Kada to uradite, izmenite program tako da ispiše `"FizzBuzz"` za brojeve koji su deljivi i sa 3 i sa 5 (a i dalje ispišite `"Fizz"` ili `"Buzz"` za brojeve koji su deljivi samo jednim od njih).

(Ovo je zapravo veoma često pitanje za ((intervju)) koje je navodno korisno za eliminaciju značajnog procenta kandidata za programera. Dakle, ako ste ga rešili, vaša vrednost na tržištu rada upravo je porasla.)

{{if interactive
```
// Vaš kod ovde.
```
if}}

{{hint

{{index "FizzBuzz (exercise)", "remainder operator", "% operator"}}

Prolazak kroz brojeve je očigledno posao za petlje, a odabir šta ispisati je stvar uslovnih izjava. Zapamtite trik korišćenja modulo operatora(`%`) za proveru da li je broj deljiv drugim brojem (ima ostatak nula).

U prvoj verziji, za svaki broj postoje tri moguća ishoda, tako da ćete morati napraviti lanac `if`/`else if`/`else`.

{{index "|| operator", ["if keyword", chaining]}}

Druga verzija programa ima jedno jednostavno rešenje i jedno pametno rešenje. Jednostavno rešenje je dodavanje još jedne uslovne "grane" kako bi se precizno testirao dati uslov. Za pametno rešenje, izgradite string koji sadrži reč ili reči za ispis i ispišite ili tu reč ili broj ako nema reči, potencijalno koristeći `||` operator.

hint}}

### Chessboard

{{index "chessboard (exercise)", loop, [nesting, "of loops"], "newline character"}}

Napišite program koji kreira string koji predstavlja tablu 8×8, koristeći znakove za novi red kako biste prešli u novu liniju. Na svakoj poziciji table nalazi se ili prazno mesto (spejs) ili znak "#" . Znakovi bi trebalo da formiraju šahovsku tablu.

Prosleđivanje ovog stringa funkciji `console.log` trebalo bi da prikaže nešto poput ovoga:

```{lang: null}
 # # # #
# # # # 
 # # # #
# # # # 
 # # # #
# # # # 
 # # # #
# # # # 
```

Kada napravite program koji generiše ovaj obrazac, definišite varijablu `size = 8` i promenite program tako da funkcioniše za bilo koju vrednost `size` varijable, tako što će ispisivati tablu zadate dužine i širine.

{{if interactive
```
// Vaš kod ovde.
```
if}}

{{hint

{{index "chess board (exercise)"}}

String možete kreirati počevši sa praznim (`""`) i konstantno dodavati nove znakovove u njega. Novi red se piše `"\n"`.

{{index [nesting, "of loops"], [braces, "block"]}}

Da biste radili sa dve ((dimenzije)), trebaće vam ((petlja)) unutar petlje. Stavite zagrade oko tela obe petlje kako biste lakše videli gde počinju i završavaju. Pokušajte da pravilno uvučete ta tela da vam kod bude čitak. Redosled petlji mora pratiti redosled u kojem gradimo string (red po red, sleva nadesno, odozgo nadole). Dakle, spoljašnja petlja upravlja linijama, a unutrašnja petlja upravlja znakovima na liniji.

{{index "counter variable", "remainder operator", "% operator"}}

Trebaće vam dve varijable da pratite napredak petlji. Varijabla da biste znali da li treba staviti razmak ili "#" na određenoj poziciji, možete testirati da li je zbir dva brojača za petlje paran (`% 2`).

Završavanje linije dodavanjem znaka novog reda `"\n"`, mora se desiti nakon što je red izgrađen, tako da to uradite posle unutrašnje petlje ali unutar spoljne petlje.

hint}}
