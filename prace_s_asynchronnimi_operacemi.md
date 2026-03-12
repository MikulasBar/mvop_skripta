# Práce s asynchronními operacemi

## Charakter otázky

Tato otázka je **praktická**, ale teorie je zde velmi důležitá. V mobilních aplikacích prakticky neustále pracujeme s operacemi, které trvají nějaký čas: síťová komunikace, čtení databáze, práce se soubory, získání polohy nebo autentizace. Bez správného pochopení asynchronního programování nelze vytvořit stabilní Flutter aplikaci.

## Co je asynchronní operace

**Asynchronní operace** je taková operace, která neskončí okamžitě. Aplikace ji spustí, ale mezitím může pokračovat v další práci a neblokuje uživatelské rozhraní.

Příklady:

- načtení dat z REST API
- přihlášení uživatele
- čtení nebo zápis do databáze
- načtení souboru z disku
- přístup ke kameře nebo geolokaci

Pokud bychom takové operace spouštěli blokujícím způsobem na hlavním vlákně, aplikace by zamrzala a uživatel by měl špatný zážitek.

## Event loop a single-thread model

Dart funguje primárně na principu **event loop**. To znamená, že:

- aplikace zpracovává události postupně
- dlouhé operace se plánují asynchronně
- po dokončení se vrací výsledek zpět do hlavního toku

Pro běžný Flutter vývoj je důležité vědět, že UI běží na hlavním vlákně a nesmí být blokované.

## Future

Základním stavebním kamenem asynchronního programování v Dartu je **Future**. Future reprezentuje hodnotu, která bude k dispozici až v budoucnu.

Future může skončit:

- úspěchem s výsledkem
- chybou

Příklad:

```dart
Future<String> loadUsername() async {
  await Future.delayed(const Duration(seconds: 1));
  return 'Jan';
}
```

## async a await

Nejpohodlnější způsob práce s Future je pomocí klíčových slov **async** a **await**.

```dart
Future<void> fetchData() async {
  try {
    final user = await api.getUser();
    print(user.name);
  } catch (error) {
    print('Chyba: $error');
  }
}
```

Výhody:

- kód se čte podobně jako synchronní
- snadnější práce s chybami přes `try/catch`
- menší riziko nepřehledných callbacků

## then, catchError a whenComplete

Future lze zpracovávat i bez `await` pomocí callbacků.

```dart
api.getUser()
  .then((user) => print(user.name))
  .catchError((error) => print(error))
  .whenComplete(() => print('Hotovo'));
```

Tento styl je dobré znát, ale v moderním kódu je obvykle přehlednější používat `async/await`.

## Stream

Zatímco **Future** reprezentuje jeden budoucí výsledek, **Stream** reprezentuje posloupnost více hodnot v čase.

Použití:

- live data z databáze
- změny autentizačního stavu
- průběžné aktualizace polohy
- websocket komunikace

```dart
Stream<int> counter() async* {
  for (int i = 0; i < 5; i++) {
    await Future.delayed(const Duration(seconds: 1));
    yield i;
  }
}
```

## FutureBuilder

V UI se velmi často používá **FutureBuilder**. Ten reaguje na stav Future a podle něj vykresluje obsah.

Typické stavy:

- načítání
- úspěch
- chyba

```dart
FutureBuilder<User>(
  future: api.getUser(),
  builder: (context, snapshot) {
    if (snapshot.connectionState == ConnectionState.waiting) {
      return const CircularProgressIndicator();
    }
    if (snapshot.hasError) {
      return Text('Chyba: ${snapshot.error}');
    }
    if (!snapshot.hasData) {
      return const Text('Žádná data');
    }
    return Text(snapshot.data!.name);
  },
)
```

### Na co si dát pozor u FutureBuilderu

- nepředávat do `future:` nově vytvářené Future při každém buildu, pokud to nechceme
- uložit Future do stavu, pokud se má spustit jen jednou
- oddělit logiku načítání od samotného widgetu, pokud je složitější

## StreamBuilder

Pro streamovaná data se používá **StreamBuilder**.

To je vhodné například u:

- realtime databáze
- websocketu
- sledování změn stavu přihlášení

## Error handling

Správná práce s chybami je zásadní. Nesmíme předpokládat, že síť nebo databáze vždy odpoví správně.

Je potřeba řešit:

- výpadek internetu
- timeout
- nevalidní data
- neautorizovaný přístup
- lokální výjimky v kódu

Praktická doporučení:

- používat `try/catch`
- vracet smysluplné chybové stavy
- v UI zobrazit uživatelsky srozumitelnou hlášku
- logovat technické detaily pro vývojáře

## Stav načítání v UI

Každá asynchronní operace by měla mít v UI jasně vyřešené stavy:

- **loading**
- **success**
- **empty**
- **error**

To je důležité i z pohledu UX. Uživatel musí rozumět tomu, co se děje.

> [OBRÁZEK: jednoduchý stavový diagram pro loading, success, empty a error]

## Paralelní spuštění více operací

Někdy potřebujeme načíst více věcí naráz. Místo sekvenčního čekání můžeme použít například **Future.wait**.

```dart
final results = await Future.wait([
  api.getUser(),
  api.getSettings(),
  api.getNotifications(),
]);
```

To je výhodné pro výkon, pokud na sobě operace nejsou závislé.

## Timeout a zrušení operací

U síťových volání je vhodné řešit **timeout**. Jinak může uživatel čekat příliš dlouho.

V Dartu lze nastavit například:

```dart
await api.getUser().timeout(const Duration(seconds: 10));
```

Zrušení operací je složitější a závisí na použité knihovně. Například HTTP klient nebo stream subscription mohou mít vlastní mechanismy.

## Isolates

Pro náročnější výpočty nestačí běžná asynchronní operace. Pokud je úkol výpočetně těžký, může stále blokovat hlavní vlákno. V takovém případě lze použít **Isolate**.

Použití:

- parsování velkého JSON
- zpracování obrázků
- složité výpočty

Je důležité rozlišovat:

- **I/O operace** se řeší přes Future a async/await
- **CPU heavy operace** mohou vyžadovat isolate

## Asynchronní operace a state management

V reálné aplikaci bývá asynchronní načítání obvykle řízeno přes state management, například:

- **Provider**
- **Bloc**
- **Riverpod**

Stav pak obsahuje například:

- `isLoading`
- `data`
- `errorMessage`

To je čistší než držet všechen stav přímo ve widgetu.

## Praktický příklad načtení dat po otevření obrazovky

Typický scénář:

1. otevře se detail obrazovky
2. spustí se požadavek na API
3. zobrazí se loading
4. po úspěchu se vykreslí data
5. při chybě se zobrazí možnost opakovat akci

Tento postup je v mobilních aplikacích velmi častý.

## Časté chyby

- volání asynchronní operace přímo v `build()` bez kontroly
- neřešení chybového stavu
- absence loading indikace
- aktualizace stavu po zničení widgetu
- blokování UI těžkou synchronní operací

## Doporučení pro praxi

- oddělit datovou vrstvu od UI
- explicitně modelovat stavy načítání
- používat `async/await` pro čitelný kód
- myslet na retry scénáře
- logovat chyby a monitorovat je přes nástroje typu **Crashlytics**

## Související témata

- **state management**
- **práce s databázemi a úložišti**
- **druhy API**
- **BaaS služby**
- **monitoring a crash reporting**

## Typické otázky u zkoušky

- Jaký je rozdíl mezi **Future** a **Stream**?
- Kdy použít **FutureBuilder** a kdy raději state management?
- Jak zabránit opakovanému spouštění Future při rebuildu?
- Co je **isolate**?
- Jak by měla aplikace reagovat na chybu při načítání dat?

## Shrnutí

Asynchronní operace jsou základ mobilního vývoje. Ve Flutteru se řeší hlavně přes **Future**, **Stream**, `async/await`, **FutureBuilder** a **StreamBuilder**. Nestačí umět zapsat síťové volání, je potřeba umět navrhnout i správné stavy UI, chybové scénáře a případně oddělení logiky do state management vrstvy. Kvalitní práce s asynchronitou výrazně ovlivňuje výkon, stabilitu i uživatelský zážitek.