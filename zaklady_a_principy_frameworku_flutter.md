# Základy a principy frameworku Flutter

## Charakter otázky

Tato otázka je převážně **teoretická**, ale u zkoušky je vhodné doplnit i praktické dopady na vývoj aplikace. Cílem je vysvětlit, **co je Flutter**, proč vznikl, jak funguje jeho vykreslování a v čem se liší od jiných přístupů k mobilnímu vývoji.

## Co je Flutter

**Flutter** je open-source framework od společnosti **Google** pro vývoj multiplatformních aplikací. Umožňuje vytvářet aplikace pro:

- **Android**
- **iOS**
- **web**
- **desktop** platformy, například Windows, macOS a Linux

Hlavní myšlenka Flutteru je, že vývojář píše jednu aplikaci v jazyce **Dart** a stejný kód může být spuštěn na více platformách. To ale neznamená, že je aplikace automaticky stejná všude. Flutter dává možnost sdílet velkou část logiky i UI, ale současně zachovat rozdíly mezi platformami tam, kde je to potřeba.

## Proč Flutter vznikl

Před Flutterem existovalo několik běžných přístupů k multiplatformnímu vývoji:

- psát dvě oddělené nativní aplikace, tedy jednu v **Kotlin/Java** a druhou ve **Swift/Objective-C**
- používat hybridní řešení založená na **WebView**
- používat frameworky, které mapují komponenty na nativní prvky systému

Flutter vznikl jako reakce na několik problémů:

- vývoj dvou nativních aplikací je drahý a pomalý
- hybridní přístupy často trpí horším výkonem
- mapování na nativní komponenty vede někdy k rozdílnému chování mezi platformami

Flutter proto zvolil jiný přístup: místo použití nativních UI prvků si většinu rozhraní kreslí **sám**.

## Základní princip: vše je widget

Jedna z nejdůležitějších myšlenek Flutteru je, že **všechno je widget**. Widget reprezentuje část uživatelského rozhraní, ale i chování nebo rozložení.

Příklady widgetů:

- **Text** zobrazuje text
- **Container** slouží jako obal s velikostí, barvou nebo paddingem
- **Row** a **Column** řeší horizontální a vertikální rozložení
- **Scaffold** poskytuje základní kostru obrazovky
- **MaterialApp** reprezentuje kořen aplikace v Material Design stylu

Tento přístup vede k tomu, že se UI skládá jako strom widgetů. Vývojář nepopisuje jednotlivé kroky vykreslení, ale deklaruje, jak má UI vypadat pro aktuální stav.

> [OBRÁZEK: strom widgetů Flutter aplikace od MaterialApp přes Scaffold až po konkrétní prvky UI]

## Deklarativní přístup

Flutter používá **deklarativní UI**. To znamená, že vývojář neříká „změň tento konkrétní prvek na obrazovce“, ale spíš „pro tento stav aplikace má UI vypadat takto“.

V praxi to funguje tak, že:

1. aplikace má nějaký **stav**
2. podle stavu se vytvoří strom widgetů
3. při změně stavu se widgety znovu přepočítají
4. Flutter efektivně překreslí jen to, co je potřeba

To je podobný princip jako u frameworků typu **React**. Výhoda je lepší čitelnost a předvídatelnost kódu. Nevýhodou může být, že začátečník musí změnit způsob myšlení oproti imperativnímu přístupu.

## Architektura Flutteru

Flutter je možné chápat ve více vrstvách:

### Framework vrstva

Nejvyšší vrstva obsahuje:

- widgety
- navigaci
- animace
- správu gest
- přístup k tématům a stylům

Tato vrstva je napsaná hlavně v jazyce **Dart**.

### Engine vrstva

Pod frameworkem se nachází **Flutter Engine**, který je zodpovědný za:

- vykreslování grafiky
- práci s textem
- správu vstupů
- komunikaci s platformou

Historicky Flutter používal renderer **Skia**. V moderních verzích se čím dál více mluví i o rendereru **Impeller**, který řeší stabilnější výkon zejména na iOS a novějších zařízeních.

### Embedder vrstva

Nejnižší vrstva zajišťuje integraci s konkrétní platformou, tedy s Androidem, iOS, webem nebo desktopem. Tato vrstva propojuje Flutter s nativním operačním systémem.

## Jak Flutter vykresluje uživatelské rozhraní

Tohle je velmi důležitý bod, protože odlišuje Flutter od mnoha jiných frameworků.

Flutter ve většině případů nepoužívá nativní tlačítka, seznamy nebo textová pole dané platformy jako hlavní stavební prvky. Místo toho si je kreslí sám. Díky tomu:

- má konzistentní vzhled napříč platformami
- je chování více pod kontrolou frameworku
- lze vytvářet velmi bohatá a animovaná rozhraní

Současně to ale znamená, že vývojář musí více myslet na:

- **přístupnost**
- správné napodobení nativního chování tam, kde je to žádoucí
- velikost aplikace

## Widget, Element, RenderObject

U zkoušky se často hodí vysvětlit rozdíl mezi třemi pojmy:

### Widget

**Widget** je neměnný popis části UI. Je to konfigurace.

### Element

**Element** je objekt, který propojuje widget se skutečným umístěním ve stromu. Udržuje vztahy mezi widgety a životním cyklem v rámci obrazovky.

### RenderObject

**RenderObject** řeší konkrétní layout a vykreslení. Tedy jak velký prvek bude a kde bude umístěn.

Zjednodušeně:

- widget říká **co** se má zobrazit
- element drží widget ve stromu a sleduje životní cyklus
- render object řeší **jak** to bude rozloženo a vykresleno

## StatelessWidget a StatefulWidget

Flutter dlouhou dobu stavěl základ dělení widgetů na:

- **StatelessWidget**
- **StatefulWidget**

### StatelessWidget

Používá se tam, kde se obsah sám interně nemění. Typicky:

- statický text
- ikona
- jednoduché rozložení

### StatefulWidget

Používá se tam, kde se mění stav v čase, například:

- zaškrtnutí checkboxu
- načítání dat
- přepínání tabu
- validace formuláře

Je důležité chápat, že změna stavu neznamená přepsání části obrazovky ručně, ale vyvolání nového sestavení widgetů metodou **build**.

## Význam jazyka Dart

Flutter je úzce spojený s jazykem **Dart**. Ten byl zvolen proto, že:

- podporuje **just-in-time** kompilaci pro rychlý vývoj
- podporuje **ahead-of-time** kompilaci pro produkční výkon
- má dobrou podporu pro asynchronní programování pomocí **Future** a **Stream**
- nabízí silné typování a moderní syntaxi

Ve vývoji se často vyzdvihují dva režimy:

- **JIT** během vývoje umožňuje například hot reload
- **AOT** při buildu pro produkci zajišťuje vysoký výkon

## Hot Reload a Hot Restart

Flutter je známý díky **Hot Reload**. To je jedna z jeho největších výhod při vývoji.

### Hot Reload

- aplikuje změny v kódu téměř okamžitě
- zachovává většinu aktuálního stavu aplikace
- zrychluje ladění UI

### Hot Restart

- znovu spustí aplikaci od začátku
- nezachová runtime stav
- je užitečný při větších změnách struktury aplikace

Pro produktivitu týmu je hot reload významný, protože zkracuje zpětnou vazbu při návrhu obrazovek a opravách.

## Výhody Flutteru

Mezi hlavní výhody patří:

- **jeden kód** pro více platforem
- rychlý vývoj díky **hot reload**
- vysoká kontrola nad vzhledem UI
- dobrý výkon díky nativně kompilovanému kódu
- silný ekosystém balíčků
- snadná tvorba animací a vlastních komponent

## Nevýhody a omezení

Je důležité říct i slabší stránky:

- větší výsledná velikost aplikace než u čistě nativního řešení
- závislost na ekosystému Flutteru a balíčků třetích stran
- někdy potřeba dopisovat nativní kód přes **platform channels**
- ne vždy stoprocentně nativní pocit aplikace, pokud není dobře navržená

## Flutter vs. nativní vývoj

| Oblast | Flutter | Nativní vývoj |
| --- | --- | --- |
| Kódová báze | jedna hlavní | oddělená pro každou platformu |
| UI | většinou vlastní vykreslení | nativní komponenty |
| Rychlost vývoje | obvykle vyšší | nižší při dvou platformách |
| Přístup k novým API | někdy se čeká nebo se píše bridge | okamžitě |
| Konzistence designu | vysoká | liší se podle platformy |

## Kdy Flutter dává smysl

Flutter je vhodný hlavně když:

- chceme rychle dodat aplikaci pro více platforem
- tým nechce vyvíjet dvě samostatné mobilní aplikace
- je důležitá jednotná vizuální identita produktu
- aplikace obsahuje bohaté UI, animace nebo vlastní komponenty

Naopak méně vhodný může být tam, kde:

- je extrémně důležité využít nejnovější nativní API okamžitě
- aplikace je úzce svázaná s konkrétní platformou
- firma už má silné nativní týmy a hotovou infrastrukturu

## Související témata

Na tuto otázku přirozeně navazují další oblasti:

- **struktura Flutter aplikace**
- **layout systém a widgety**
- **state management**
- **asynchronní operace**
- **platform channels**
- **nasazení aplikace**

U zkoušky je dobré ukázat, že Flutter není jen UI framework, ale celý ekosystém pro vývoj, testování a nasazení aplikací.

## Typické doplňující otázky u zkoušky

- Proč Flutter nepoužívá přímo nativní komponenty?
- Jaký je rozdíl mezi **widgetem** a **render objectem**?
- Jak funguje **hot reload**?
- Jaké jsou rozdíly mezi Flutterem a React Native?
- Kdy by bylo lepší zvolit nativní vývoj?

## Shrnutí

**Flutter** je multiplatformní framework založený na jazyce **Dart**, který používá **deklarativní přístup** a vlastní vykreslovací engine. Klíčová myšlenka je, že **všechno je widget**. Díky tomu lze vytvářet konzistentní a výkonné aplikace pro více platforem z jedné kódové báze. Pro správné pochopení Flutteru je zásadní rozumět stromu widgetů, principu přestavování UI při změně stavu a rozdílům oproti nativnímu vývoji.