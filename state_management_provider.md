# State management - Provider

## Charakter otázky

Tato otázka je převážně **praktická**. U zkoušky se očekává, že student vysvětlí, proč je potřeba **state management**, jak funguje balíček **Provider**, jaké má hlavní stavební kameny a v jakých situacích je vhodné ho použít.

## Co je state management

**State management** je způsob, jak ve aplikaci uchovávat a šířit **stav** mezi různými částmi uživatelského rozhraní. Stav může být například:

- přihlášený uživatel
- obsah košíku
- vybraný filtr
- načtená data z API
- informace o tom, zda probíhá loading

Bez state managementu by aplikace rychle sklouzla k nepřehlednému předávání dat přes konstruktory a callbacky mezi widgety.

## Proč nestačí jen StatefulWidget

**StatefulWidget** je vhodný pro lokální stav konkrétního widgetu, například:

- otevření nebo zavření panelu
- aktuální hodnota v textovém poli
- přepnutí switche

Jakmile ale stav potřebuje více obrazovek nebo více větví stromu widgetů, je vhodnější centrálnější řešení. A právě zde přichází **Provider**.

## Co je Provider

**Provider** je populární balíček pro Flutter, který zjednodušuje sdílení dat a stavu ve stromu widgetů. Interně staví na konceptu **InheritedWidget**, ale dává nad ním mnohem pohodlnější API.

Provider řeší hlavně:

- zpřístupnění objektu potomkům ve stromu
- notifikaci widgetů o změně stavu
- přehlednější oddělení stavu od UI

## Hlavní princip

Objekt se „vystaví“ výše ve stromu a potomci si ho mohou přečíst pomocí kontextu. Když se stav změní, relevantní widgety se přestaví.

To znamená:

1. nadefinujeme stavový objekt
2. poskytneme ho přes Provider
3. UI se na něj napojí pomocí `watch`, `read` nebo `Consumer`
4. při změně stavu se přestaví jen potřebná část UI

## Nejčastější typy providerů

### Provider

Základní varianta pro poskytování objektu, který se sám nemění notifikačním způsobem.

Použití:

- repository
- service
- konfigurace

### ChangeNotifierProvider

Nejčastější varianta pro jednoduchý stav. Poskytuje objekt, který dědí z **ChangeNotifier**.

### FutureProvider

Používá se pro poskytnutí asynchronně načtené hodnoty.

### StreamProvider

Používá se pro streamovaná data v čase.

### MultiProvider

Pomáhá přehledně zabalit více providerů na jednom místě.

## ChangeNotifier

**ChangeNotifier** je jednoduchý mechanismus, kdy objekt drží stav a při změně zavolá **notifyListeners()**.

```dart
class CounterProvider extends ChangeNotifier {
  int count = 0;

  void increment() {
    count++;
    notifyListeners();
  }
}
```

Napojení v aplikaci:

```dart
MultiProvider(
  providers: [
    ChangeNotifierProvider(create: (_) => CounterProvider()),
  ],
  child: const MyApp(),
)
```

Čtení v UI:

```dart
final counter = context.watch<CounterProvider>();

Text('${counter.count}')
```

## watch, read a select

Tohle je velmi důležité téma.

### watch

`context.watch<T>()` poslouchá změny a při změně poskytovaného objektu vyvolá rebuild.

### read

`context.read<T>()` objekt pouze načte, ale neposlouchá změny. Vhodné například v handleru tlačítka.

### select

`context.select<T, R>()` umožňuje poslouchat jen konkrétní část stavu. To pomáhá optimalizovat rebuildy.

## Consumer a Selector

### Consumer

**Consumer** je widget, který odebírá provider a přestaví jen svou část podstromu.

```dart
Consumer<CounterProvider>(
  builder: (context, counter, child) {
    return Text('${counter.count}');
  },
)
```

### Selector

**Selector** je podobný, ale zaměřuje se jen na část dat a tím snižuje počet zbytečných rebuildů.

## Praktický scénář použití

Typický příklad je seznam produktů s košíkem.

Provider může držet:

- seznam položek v košíku
- celkovou cenu
- metodu pro přidání a odebrání položky

UI pak na několika obrazovkách čte stejný stav:

- seznam produktů
- detail produktu
- ikona košíku v app baru
- stránka košíku

To je přesně scénář, kde Provider dává smysl.

## Provider a čistá architektura

V menších a středně velkých projektech se Provider často používá tak, že:

- **service** nebo **repository** vrstva získává data
- **provider** objekt drží stav a řídí loading nebo error
- widgety jen vykreslují data a volají akce

Příklad stavového objektu pro načítání:

```dart
class UserProvider extends ChangeNotifier {
  UserProvider(this._repository);

  final UserRepository _repository;
  bool isLoading = false;
  String? errorMessage;
  User? user;

  Future<void> loadUser() async {
    isLoading = true;
    errorMessage = null;
    notifyListeners();

    try {
      user = await _repository.getUser();
    } catch (error) {
      errorMessage = error.toString();
    } finally {
      isLoading = false;
      notifyListeners();
    }
  }
}
```

## Výhody Provideru

- jednoduchý na pochopení
- dobře integrovaný do Flutter ekosystému
- vhodný pro menší a střední aplikace
- jasné propojení stavu s widget stromem
- nízká bariéra vstupu pro začátečníka

## Nevýhody Provideru

- u velmi rozsáhlých aplikací může být správa složitější
- při špatném návrhu může docházet ke zbytečným rebuildům
- stav přes **ChangeNotifier** může časem vést k méně disciplinované architektuře
- není tak striktně unidirectional jako některé jiné přístupy

## Nejčastější chyby

- volání `notifyListeners()` příliš často
- ukládání příliš mnoha odpovědností do jednoho provideru
- logika API, validace, mapping i UI stav v jedné třídě
- používání `watch` tam, kde stačí `read`
- špatné umístění provideru ve stromu a tím zbytečně široké rebuildy

## Kdy Provider použít

Provider je vhodný když:

- tým chce jednoduché a čitelné řešení
- aplikace není extrémně komplexní
- potřebujeme rychle a přehledně sdílet stav mezi obrazovkami
- chceme snadno napojit repository na UI

Méně vhodný může být tam, kde:

- aplikace má velmi složitý stavový tok
- tým vyžaduje striktní oddělení eventů a stavů
- je potřeba silnější formalizace architektury

## Srovnání s jinými přístupy

- oproti čistému **StatefulWidget** umí sdílet stav napříč stromem
- oproti **Blocu** je jednodušší, ale méně formální
- oproti **Riverpodu** je více navázaný na `BuildContext`

## Související témata

- **state management - Bloc**
- **asynchronní operace**
- **práce s databázemi a API**
- **architektura Flutter aplikace**

## Typické otázky u zkoušky

- Jaký je rozdíl mezi `watch`, `read` a `select`?
- Proč Provider stojí na **InheritedWidget**?
- Kdy použít **ChangeNotifierProvider**?
- Jaké jsou nevýhody ChangeNotifier přístupu?
- Jak zabránit zbytečným rebuildům?

## Shrnutí

**Provider** je jednoduchý a rozšířený způsob state managementu ve Flutteru. Nejčastěji se používá s **ChangeNotifier**, který drží stav a pomocí `notifyListeners()` informuje UI o změnách. Hlavní síla Provideru je v jednoduchosti, čitelnosti a dobré použitelnosti pro menší až středně velké projekty. Aby byl návrh kvalitní, je nutné rozumně dělit odpovědnosti, hlídat rebuildy a oddělit stavovou logiku od datové vrstvy.