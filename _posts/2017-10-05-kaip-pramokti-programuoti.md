---
title: "Draugams: Kaip padaryti nesudėtingą žodyno programą ir šitaip truputį pramokti programuoti vakarais ir savaitgaliais"
layout: post
category: lt
lang: lt
---

![](images/2017-10-05-kaip-pramokti-programuoti/jau-nusisekes-kompiuteristas.jpg)

Aną savaitę susiradęs telefono numerį skambino nepažįstamas jaunuolis, prisistatė penkiasdešimtmečiu technologijų entuziastu ir sakė, kad nori padaryti nedidelę žodyno programą ir tuo pačiu pramokti programuoti, tai galbūt aš jį galėčiau užvesti ant jį kelio? Taip pat ne vienas iš draugų ir pažįstamų pripažino, kad norėtų pabandyti tapti programuotoju (dėl pinigų ar kitų priežasčių), bet nežino nuo ko pradėti, ką skaityti, kaip sužinot ar čia jam / jai gaunasi, tinka ir patinka, kokios perspektyvos, ar verta leist pinigus kodo akademijoms, ar galima pačiam pasinagrinėt viską ir pan.

Taigi štai, lo and behold, Mokytojų dienos proga apačioj yra mano "from sofa to 3000 €"" tutorialas, kaip padaryti mažą prototipinę žodynėlio programą. OK, nuo tutorialo iki trijų štukių dar bus ką paveikti, bet stengiaus viską surašyti kuo paprasčiau, su bullet pointsais, linkais, tipsais ir triksais, abstrakčia žodynėlio duomenų bazės architektūra, žingsniais ir kitkuo. Jeigu norėjot pamėgint užsiėmimą, panašų į mano, tai dabar turit receptą. Komandų, kurias reikia įvest į terminalą, ir kodo, kurį nupeistinus viskas pasidaro, deja nėra - siūlau viską susiinstaliuot iš vienų nuorodų, tada spaust ant kitų nuorodų su dokumentacija, tada eit pabandyt kažką, tada eit googlint, tada apet' 25 - kol pavyks!

Sėkmės pamėginusiems! Jei kas nesigauna, rašykite žinoma.

(Kompiuterastų prašau pernelyg nesikabinėti, aš puikiai suprantu, kad Node.js programoj išvedinėt hardkodintą HTMLą yra nesąmonė, kad reikia templatų, kad yra AJAX, kad yra skirtumų tarp browserių, etc. Aš tik norėjau parodyt bendrą webappso principą, kad nontechui pavyktų pasibandyti kažką biškį.)

----

Sveiki, xxx,

Atsiprašau už vėlyvą atsakymą. Manau, kad šaunu, jog nusprendėte sugrįžti prie programavimo! Neabejoju, kad pasiryžus ir praleidus keletą savaičių / mėnesių vakarais mokantis programuoti, po kurio laiko jau visai neblogai pavyks ir galėsite tas žinias jau pradėti naudoti praktikoje.

C++ nėra labai paprasta kalba pradedantiesiems, o komercinių projektų su Qt gana sunku rasti (ypač Lietuvoje). Aš Jums siūlyčiau pamėginti lietuvių neblogai pramindytą taką ir savo žodynui sukurti websaitą. Šitaip turėsite universalų sprendimą, kuris be problemų veiks visuose įrenginiuose, galės būti perdarytas į desktopinę programą, o įgautas žinias tikrai rasite kam parduoti, jei to pageidausite.

Websaito padarymui galite rinktis iš krūvos technologijų, bet aš Jums siūlau viską daryti su JavaScript programavimo kalba (Node.js serverio pusėje ir paprastas JavaScript ir / arba jQuery kliento pusėje, jeigu prireiks) ir MySQL duomenų baze.


## Žodyno programos (websaito) ingredientai:

* **MySQL duomenų bazė.** Žodyno antraštinius žodžius ir jų apibrėžimus kažkur reikės įrašyti, juos redaguoti, trinti, pridėti naujus ir t.t. Tam yra naudojama duomenų bazė, veikianti serverio pusėje (žinoma, programuojant serveris ir klientas gali būti vienas ir tas pats kompiuteris).

    Įdiekite MySQL pagal šią instrukciją: <https://dev.mysql.com/doc/mysql-getting-started/en/>; jeigu nepavyks, googlinkite "mysql tutorial", "mysql getting started" ir pan. frazes. Įdiegę, pamėginkite sukurti naują lentelę (`CREATE TABLE`), į ją pridėti vieną kitą įrašą (`INSERT`), pridėtus įrašus pakeisti (`UPDATE`), rasti įrašus pagal nurodytą sąlygą (`SELECT ... WHERE`) ir juos ištrinti (`DELETE`), taip pat galite išsiaiškinti, kas yra indeksas (`CREATE INDEX`). Tikrai neverta perskaityti MySQL (ar bet kurios kitos technologijos) dokumentacijos nuo viršelio iki viršelio - įdiekite MySQL, pasimėginkite keletą komandų, perpraskite, ką daugmaž tos komandos daro, ir to užteks, o jeigu neužteks, tai kas liko rasite toje dokumentacijoje vėliau arba išsigooglinsite. Tai galioja ir visoms kitoms technologijoms, aprašytoms apačioje.

    Pats MySQL neturi grafinės sąsajos, bet yra keletas GUI programų, kurios gali Jums padėti aiškinantis šią duomenų bazę, pvz. MySQL Workbench (<https://dev.mysql.com/downloads/workbench/>; nemokama) arba Navicat (<https://www.navicat.com/en/>; yra nemokama laikina versija).

    Oficiali MySQL dokumentacija gana paini; manau, kad pradžiai geriau būtų pasižiūrėti į <https://www.w3schools.com/sql/> ar <https://www.tutorialspoint.com/mysql/>. Jeigu nepatiks šie tutorialai, visada galite pasigooglinti kokį kitą, kurių prirašyta begalė ("mysql tutorial", "mysql getting started", "mysql quick start", ...).

    (Svarbiausias įgūdis bet kuriam programuotojui yra ne mokėti vieną ar kitą technologiją ar turėti kokį nors ten labai stiprų loginį mąstymą, bet mokėti googlinti :) Iš visų programavimo kalbų, kurias esą moku, aš atsimenu vos kelias esmines konstrukcijas, o visa kita - net ir primityviausius dalykus - visada išsigooglinu.)

* **HTML.** Tai nėra programavimo, bet išdėstymo (angl. *layout*) kalba, su kuria aprašomi kiekvieno websaito elementų - teksto, mygtukų, meniu, teksto įvedimo laukelių ir kt. - struktūra. Aš Jums siūlau su HTML sukurti savo būsimojo žodyno sąsają - jums prireiks teksto laukelio, kuriame įvesite ieškomą žodį, mygtuko, kurį reikės paspausti, kad paieška prasidėtų, sąrašo rastų žodžių ir vietos, kurioje parodysite žodžio aprašymą.

    Norint kurti HTML puslapius, Jums reikės paprasto teksto redaktoriaus (Wordas netinka!). Mano mėgstamiausi yra Nodepad++ Windowsams (<https://notepad-plus-plus.org/>; nemokamas) arba Sublime Text bet kuriai platformai (<https://www.sublimetext.com/>; irgi nemokamas, bet paleidus prašo pinigų). Pasižiūrėti sukurtą HTML puslapį prireiks bet kurios web naršyklės (Chrome, Firefox, Safari, ... - nesvarbu).

    Neblogas resursas pramokti HTML yra <https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Getting_started>. Kaip ir MySQL atveju, siūlau Jums išsiaiškinti tik tiek, kiek Jums reikia pirmai pradžiai norint padaryti žodyno interfeisą, t.y. kaip aprašyti laukelį žodžio įvedimui, padaryti nuorodą, įterpti paveikslėlį, etc.

* **CSS.** Jeigu su HTML websaite yra aprašoma, kokie elementai jame yra (ir iš dalies ką jie daro), tai CSS naudojamas aprašant, kaip šie HTML elementai atrodo - ar mygtukas turi būti mėlynas, ar teksto laukelis turi būti kairėje, ar dešinėje ir pan.

    CSS resursas: <https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS>, plius ką pavyks papildomai išsigooglinti. Siūlau į CSS pradiniame etape kreipti mažiausiai dėmesio - išsiaiškinkite, kaip pakeisti kokią nors ten teksto spalvą, uždėti rėmelį ir to tikrai užteks. Vėliau dailindamas savo žodyno išvaizdą galėsite įsijausti labiau :)

    CSS irgi kuriamas su paprasto teksto redaktoriumi (Sublime Text ar Notepad++), puikiai veiks Jūsų naršyklėje.

* **JavaScript.** JavaScript yra keista programavimo kalba, kurios aš šiaip pernelyg nemėgstu, bet ji šiomis dienomis yra naudojama visur, todėl vien dėl populiarumo ir universalumo siūlyčiau Jums rinktis būtent ją. JavaScript buvo sukurta kaip programavimo kalba, veikianti kliento pusėje, konkrečiai - Jūsų naršyklėje, bet su ja šiais laikais gamina ir serveryje veikiančias programas naudodami Node.js (apie tai vėliau).

    Kaip ir HTML / CSS atveju, norint išbandyti JavaScript nieko papildomai diegti nereikia, užteks Jūsų turimo browserio. Štai neblogas resursas apie ją: <https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/JavaScript_basics> ir kiti resursai, kuriuos Jums pavyks rasti.

    Pramokite pagrindinių duomenų tipų: eilutė (*string*), skaičius (*integer*), masyvas (*array*), žodynas (*dictionary*), ...; struktūrų (`if`, `while`, `for`, ...), kaip padaryti funkciją (`function()`), kaip kažką išspausdinti į browserio konsolę (`console.log()`), sukurti dialogą (`alert()`) ir kaip naudotis debuggeriu (<https://developers.google.com/web/tools/chrome-devtools/javascript/>). Dar kartą - jeigu skaitote, mėginatės pas save ir vis dar įdomu, tai tikrai nereikėtų sustoti, bet nesiūlau ir save forsuoti siekiant perskaityti VISĄ knygą / dokumentaciją ir šitaip išmokti VISĄ JavaScript - svarbu pagauti esmę ir vėliau improvizuoti (begooglinant!).

* **Node.js.** JavaScriptas sau ramiai metų metus veikė kliento (web naršyklės) pusėje, kol kažkokie pašlemėkai nesugalvojo, kad nebenori naudoti atskiros programavimo kalbos serverio programoms rašyti, todėl pritaikė JavaScriptą serverio programų rašymui, ir iš to gavosi Node.js (<https://nodejs.org/en/>).

    Node.js esmė būtų ta, kad su ja JavaScripto programas galima paleidinėti nebenaudojant browserio. Jūsų atveju, Node.js programa veiks kaip HTTP serveris ir "bendraus" su taip pat serveryje (kuris kūrimo metu bus Jūsų kompiuteris) veikiančia MySQL, į ją siųsdamas antraštinių žodžių paieškos užklausas, gaudamas atgal rastus žodžius ir jų apibrėžimus, iš tų apibrėžimų suformuodamas HTML puslapį ir išsiųsdamas jį naršyklei, kad ši galėtų viską parodyti.

    Apie Node.js galite paskaityti <https://www.w3schools.com/nodejs/nodejs_get_started.asp>, <https://blog.risingstack.com/node-hero-tutorial-getting-started-with-node-js/>, <https://www.airpair.com/javascript/node-js-tutorial> ir kitur. Su Node.js pamėginkite sukurti paprasčiausią HTTP serverį, kuris, atidarius adresą, išvestų frazę "Hello World" (instrukcija yra pirmojoje nuorodoje). Taip pat galite iškart pabandyti su Node.js prisijungti prie MySQL serverio, nes to prireiks vėliau: <https://www.w3schools.com/nodejs/nodejs_mysql.asp>.


## Planas:

Tai štai, su tais penkiais komponentais jau galite kažką pamėginti padaryti :). Mano siūloma veiksmų seka:

1. Įsidiekite MySQL, pamėginkite aukščiau aprašytus veiksmus (lentelių sukūrimas, `INSERT`, `SELECT` ir kt.) Jūsų programai pradžioje turbūt užteks vienintelės lentelės, kuri susidės iš trijų stulpelių - įrašo ID (*primary key*), antraštinio žodžio ir jo aprašymo - plius vieno papildomo indekso ant antraštinio žodžio, kad šį būtų galima atrinkti greičiau. Jeigu vėliau pageidausite, kad vienas antraštinis žodis galėtų nurodyti į kelis aprašymus, prireiks jau trijų lentelių (vienos su antraštiniais žodžiais, kitos su apibrėžimais, trečios vadinamosios "one-to-many", kurioje susirašysite, kuris apibrėžimas priskiriamas kuriam antraštiniam žodžiui). Taip pat pasimėginkite iš duomenų bazės rasti žodį pagal tikslų atitikimą (`SELECT ... FROM ... WHERE zodis = "..."`) ir žodžius kandidatus pagal jo pradžią (`SELECT ... FROM ... WHERE zodis LIKE "...%"`; atkreipkite dėmesį į procento ženklą).
2. Paskaitinėjęs apie HTML, pasidarykite paprasčiausią įmanomą žodyno sąsają, kuri greičiausiai susidės iš paieškos teksto įvedimo laukelio (`<input type="text">`), rastų žodžių sąrašo (`<ul><li>...</li><li>...</li>...</ul>`) ir surasto žodžio apibrėžimo (`<p>` arba `<div>`).
3. Pramokęs JavaScript ir Node.js pagrindų, galiausiai sujunkite visus tris komponentus:
    1. Pagaminkite HTTP serverį, kuris vietoje "Hello World" išveda Jūsų anksčiau paruoštą HTML kodą.
    2. Pakeiskite HTTP serverį taip, kad toje HTML kodo vietoje, kurioje planuojate rodyti rastus žodžius, serveris išvestų visus MySQL duomenų bazėje esančius žodžius.
    3. Pakeiskite HTTP serverį taip, kad žodžio paieškos formoje (`<form>`) įvedimo laukelyje (`<input type="text" />`) įvedus žodį ir paspaudus mygtuką (`<button type="submit" />`), serveris gautų įvestą žodį ir atvaizduotų jį kur nors puslapyje (pvz. "Šiuo metu ieškomas žodis: …"). Norint, kad paspaudus mygtuką serveris gautų žinią su teksto laukelio turiniu, reikės nustatyti HTML formos action atributą: <https://www.w3schools.com/TAGs/att_form_action.asp>. Tuomet JavaScriptu parašytas Node.js serveris turės kažkaip perskaityti jam ką tik perduotą reikšmę: <https://stackoverflow.com/a/19029437>. Papildomai turbūt norėsite, kad ką tik įvestas žodis niekur nedingtų, o liktų teksto įvedimo laukelyje; tam jį serverio iniciatyva reikės įrašyti į `input` elemento value atributą (`<input value="..." />`).
    4. Pakeiskite HTTP serverį taip, kad jis įprastiniu atveju (kai neperduotas joks parametras, pvz. antraštinis žodis) išvestų vien Jūsų anksčiau parašytą HTML'ą, bet gavęs antraštinio žodžio paieškos užklausą GET parametru (pvz. `http://localhost/zodynas?paieska=Peru`), kreiptųsi į MySQL duomenų bazę su šia užklausa, rastų reikiamą žodį arba žodžius kandidatus ir išvestų juos atgal HTMLu, kad naršyklė galėtų juos parodyti.
    5. Pakeiskite HTTP serverį taip, kad jis kiekvieną rastą žodį kandidatą išvestų kaip nuorodą, kuri nukreiptų į Jūsų žodyno puslapį ir turėtų įvestą paieškos žodį ir pasirinkto žodžio-apibrėžimo ID (`<a href="zodynas?paieska=Peru&pasirinktas_zodis=2">Peru</a>`).
    6. Pakeiskite HTTP serverį taip, kad jis, gavęs pasirinkto žodžio ID parametrą (`pasirinktas_zodis`), papildomai iš MySQL duomenų bazės atsisiųstų šio pasirinkto žodžio apibrėžimą ir įterptų jį Jūsų pasirinktoje HTML kodo vietoje.

Šitokį prototipą vėliau galėsite tobulinti ir plėsti, pvz. padaryti naujų žodžių pridėjimą, žodžių ištrynimą, žodžio apibrėžimo redagavimą (pvz. integruoti su kokiu nors ten <https://quilljs.com/> JavaScript biblioteka kliento pusėje) ir kt. Visgi, kad neatsibostų ir neužstrigtumėte, siūlau pirma pasimėginti padaryti štai tokią bandomąją programą.


## Keletas pastabų apie programavimą:

* Apie aprašytąsias technologojas tikrai neužteks vien paskaityti, jas privalu iškart mėgintis vos tik įdiegus. "Šperinti" siūlau kuo mažesniais žingsniais, t.y. vos ką pakeitėte programoje - iškart ir išbandykite, ar vis dar veikia arba bent jau esate teisingame kelyje. Visos žemiau aprašytos technologijos labai "jautrios" mažiems niuansams - kableliams, taškams, kabliataškiams, tarpams kokioje nors kritiškoje vietoje, todėl tik judėdamas pirmyn pamažu galėsite sukontroliuoti savąją programą.

* Labai normalu yra, kaip Jūs rašote, "šperinti" kitų parašytą kodą, kitaip tariant, googlinant ieškoti jau padarytų sprendimų.

* Taip pat labai normalu yra, kad "nusišperinus" ar parašius nuosavą programinį kodą niekas neveikia iš pirmo, antro, trečio, …, septyniasdešimto karto. Pavyzdžiui, aš, parašęs naują funkciją visada nemaloniai nustembu, jeigu ji veikia iš pirmo karto; pirmojo bandymo kode neturėti sintaksės arba logikos klaidų yra reta. Paslaptys yra dvi: 1) bent jau pradžioje programą rašyti mažais žingsneliais ir kiekvieną žingsnelį išbandyti vos ne kaskart pridėjus naują eilutę arba net simbolį; 2) kiek galima anksčiau išmokti naudotis debuggeriu ir sužinoti, kas yra breakpointas (nuoroda į Chrome Dev Tools yra viršuje).

* Dar labai normalu yra ties kažkuo užstrigti valandomis, dienomis ar mėnesiais nesugalvojant gero sprendimo. Tuomet reikia mėginti kažką daryti iš naujo, kitaip, bandyti atsekti, iki kurios vietos programa veikia kaip tikėtasi, kažką trinti, ieškoti, kaip tą patį yra padarę kiti žmonės. Dažnai užtenka nueiti į dušą arba vien pasivaikščioti.

* Norint programuoti, prireiks daug anglų kalbos. Anglų, naudojama technologijose, nėra sudėtinga (ypač jau pramokus terminus), visi verčiasi per galvą mėgindami technologijas paaiškinti kuo suprantamiau, bet jeigu netyčia turite planą išmokti programuoti su lietuvių arba rusų kalbomis, tai turiu Jus nuvilti, kad greičiausiai rezultatas nebus patenkinamas. Nežinau, kaip pačiam su anglų, bet net jeigu ir nelabai, tai siūlau taikyti kalbai tą patį metodą, kurį taikytumėte ir bet kuriai kompiuterio technologijai - mėginti, kol pavyks! Aš programuoti pradėjau gal keturiolikos su beveik nulinėmis anglų žiniomis, bet man tai pasirodė taip įdomu, kad bandymų, klaidų ir vogto "Alkono" pagalba po kiek laiko jau pradėjau neblogai gaudytis (pirma išmokau žodžius "array" ir "string", o tik vėliau "beautiful" ir "amazing" :)).

* Nėra paslaptis, kad daug žmonių iš įvairiausių sričių pastaruoju metu nori išmokti programuoti dėl pinigų, nes taip pat ne paslaptis, kad kompiuterių technologams šiomis dienomis visai neblogai einasi piniginės atžvilgiu. Rašote, kad esate tiksliukas ir technarius, todėl neabejoju, kad Jums turėtų visai neblogai eitis. Vis dėlto, jeigu per kurį laiką nerasite savyje nuoširdaus susidomėjimo, noro knistis kompiuteryje ir gilintis į jį savo nuosavu laiku, vakarais, naktimis ir savaitgaliais, beveik liguistai, gali nepavykti prasibrauti net iki Jūsų pageidaujamo "pagalbinio darbuotojo" technologijų įmonėje. Ta dabartinė karta, kuriai, kaip rašiau, visai neblogai einasi - barklajininkai, besiženijantys 70-80% manekenes, perkantys butus po du (vieną sau, kitą nuomai) ir bevažinėjantys po tailandus atostogų - prieš 10 metų buvo nesikirpę paaugliai, kurie bėgdavo iš pamokų, kad galėtų sau ramiai tyloj prie kompo pasiaiškint kaip jiems ten kokį nors žaidimuką su Flashu pasidaryt ar kaip MySQL'o `INNER JOIN`as veikia. Praleidus prie kompiuterio šiek tiek laiko jau galima šiek tiek atsipalaiduot jeigu norisi, išeiti iš darbo penktą vakaro palikus laptopą biure, gal net pakilnot kokią štangą ar užsiimti humanitariniu hobiu, bet norint tapt kompiuteristu, tuos pirmus bent penkis metus reikia normaliai pavirint - ir nemanau, kad tam užtenka vien valios, "striuka su grynais" motyvo, veikiau būtinas nuoširdus noras ir manijos lygio užsidegimas - to Jums ir linkiu!

Rašykite bet kada, jei kas neaišku mano instrukcijoje ar kils kokių klausimų (aišku, prieš tai pagooglinkite!).

Sėkmės,
