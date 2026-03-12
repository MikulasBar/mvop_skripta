# State management - Bloc

## Charakter otázky

Tato otázka je hlavně **praktická**, ale u zkoušky je důležité ukázat i architektonické souvislosti. Student by měl vysvětlit, proč se používá **Bloc**, jak funguje tok dat přes **eventy** a **stavy**, jaké jsou hlavní knihovny a kdy je Bloc vhodnější než jednodušší řešení typu Provider.

## Proč řešit state management formálněji

U jednoduché aplikace si vystačíme s lokálním stavem nebo Providerem. Jakmile ale aplikace roste, objevují se problémy:

- stav se mění z více míst
- je těžké dohledat, proč došlo ke konkrétní změně UI
- složitější asynchronní logika se míchá s widgety
- kód se hůř testuje

**Bloc** řeší tyto problémy tím, že zavádí jasně definovaný tok dat.

## Co znamená Bloc

**Bloc** znamená **Business Logic Component**. Je to architektonický přístup i konkrétní ekosystém knihoven, nejčastěji:

- **bloc**
- **flutter_bloc**

Základní myšlenka je oddělit:

- **prezentační vrstvu**
- **byznys logiku**
- **stav aplikace**

## Unidirectional data flow

Bloc používá **jednosměrný tok dat**:

1. uživatel nebo systém vyvolá **event**
2. Bloc event zpracuje
3. Bloc vytvoří nový **state**
4. UI zareaguje na nový state

Tento model je přehledný, protože změny stavu nejsou nahodilé, ale vždy se dějí přes definované eventy.

> [OBRÁZEK: schéma toků UI -> Event -> Bloc -> State -> UI]

## Základní pojmy

### Event

**Event** popisuje, co se stalo. Například:

- uživatel klikl na tlačítko
- aplikace otevřela obrazovku
- uživatel napsal text do vyhledávání

Příklady eventů:

- `LoginSubmitted`
- `ProductsRequested`
- `FilterChanged`

### State

**State** popisuje aktuální stav obrazovky nebo části aplikace. Například:

- initial
- loading
- success s daty
- error

### Bloc

**Bloc** přijímá eventy a emituje nové stavy.

### Cubit

**Cubit** je jednodušší varianta Blocu. Nemá samostatné eventy, pouze metody, které rovnou emitují stavy. Hodí se tam, kde není potřeba formální event-driven model.

## Bloc vs Cubit

### Cubit

Výhody:

- méně boilerplate kódu
- rychlejší implementace
- vhodný pro jednodušší use-casy

### Bloc

Výhody:

- explicitní eventy
- lepší auditovatelnost změn stavu
- vhodnější pro komplexní logiku a větší týmy

U zkoušky je dobré říct, že **Cubit** je prakticky zjednodušený Bloc, ale oba patří do stejného ekosystému.

## Typická struktura souborů

V projektu se často používá struktura:

- `feature/bloc/login_bloc.dart`
- `feature/bloc/login_event.dart`
- `feature/bloc/login_state.dart`

Nebo v modernější podobě se eventy a stavy zapisují do jednoho souboru, pokud je feature menší.

## Jednoduchý příklad Blocu

```dart
sealed class CounterEvent {}

final class CounterIncrementPressed extends CounterEvent {}

class CounterState {
  const CounterState(this.value);
  final int value;
}

class CounterBloc extends Bloc<CounterEvent, CounterState> {
  CounterBloc() : super(const CounterState(0)) {
    on<CounterIncrementPressed>((event, emit) {
      emit(CounterState(state.value + 1));
    });
  }
}
```

Napojení do UI:

```dart
BlocProvider(
  create: (_) => CounterBloc(),
  child: const CounterPage(),
)
```

Čtení stavu:

```dart
BlocBuilder<CounterBloc, CounterState>(
  builder: (context, state) {
    return Text('${state.value}');
  },
)
```

Vyvolání eventu:

```dart
context.read<CounterBloc>().add(CounterIncrementPressed());
```

## BlocBuilder, BlocListener a BlocConsumer

### BlocBuilder

Slouží ke **stavění UI** na základě state.

### BlocListener

Slouží pro jednorázové reakce, které nejsou čisté UI vykreslení. Například:

- zobrazení snackbaru
- navigace
- dialog

### BlocConsumer

Kombinuje builder i listener v jednom widgetu.

Je důležité rozlišovat, co patří do builderu a co do listeneru. Navigaci nebo snackbary nechceme dělat přímo v build metodě.

## Asynchronní operace v Blocu

Bloc se často používá právě pro řízení asynchronních use-case scénářů.

Typický tok:

1. UI vyšle event `ProductsRequested`
2. Bloc nastaví `loading` state
3. zavolá repository
4. podle výsledku emituje `success` nebo `error`

```dart
on<ProductsRequested>((event, emit) async {
  emit(const ProductsLoading());
  try {
    final products = await repository.fetchProducts();
    emit(ProductsSuccess(products));
  } catch (error) {
    emit(ProductsError(error.toString()));
  }
});
```

## Equatable a porovnávání stavů

U Blocu se často používá balíček **equatable**. Ten zjednodušuje porovnávání objektů podle hodnot místo reference.

To je důležité, protože framework potřebuje rozpoznat, zda se state skutečně změnil.

## Repository pattern a Bloc

Bloc by neměl přímo znát detaily API nebo databáze. Lepší přístup je:

- **repository** vrstva komunikuje s daty
- Bloc orchestrace používá repository
- UI komunikuje jen s Blocem

Tím se zlepší:

- testovatelnost
- čitelnost
- možnost výměny zdroje dat

## Testování Blocu

Jedna z velkých výhod Blocu je velmi dobrá testovatelnost. Lze testovat, že pro danou sekvenci eventů vzniknou očekávané states.

Například:

- při úspěšném načtení: `loading -> success`
- při chybě: `loading -> error`

To je výrazně přehlednější než testovat logiku rozesetou přímo po widgetech.

## Výhody Blocu

- jasný a předvídatelný tok dat
- dobrá škálovatelnost větších aplikací
- výborná testovatelnost
- oddělení UI od byznys logiky
- snadnější spolupráce ve větším týmu

## Nevýhody Blocu

- více boilerplate kódu
- vyšší vstupní složitost pro začátečníky
- pro malou aplikaci může být zbytečně robustní

## Kdy použít Bloc

Bloc je vhodný když:

- aplikace je střední nebo velká
- stavové toky jsou složitější
- tým chce formální a disciplinovaný přístup
- projekt vyžaduje dobrou testovatelnost a škálovatelnost

Cubit bývá vhodný když:

- logika je jednodušší
- není potřeba explicitní modelování eventů

## Časté chyby

- příliš mnoho logiky přímo ve widgetech i přes použití Blocu
- používání listener efektů v builderu
- obrovské stavy a eventy bez jasné struktury
- absence repository vrstvy
- přehnané použití Blocu i na triviální lokální stav

## Srovnání s Providerem

| Oblast | Provider | Bloc |
| --- | --- | --- |
| Složitost | nižší | vyšší |
| Boilerplate | menší | větší |
| Formalizace toku dat | nižší | vysoká |
| Testovatelnost | dobrá | velmi dobrá |
| Vhodnost pro větší aplikace | střední | vysoká |

## Související témata

- **state management - Provider**
- **asynchronní operace**
- **práce s API a databázemi**
- **architektura Flutter aplikace**

## Typické otázky u zkoušky

- Jaký je rozdíl mezi **Bloc** a **Cubit**?
- Proč je Bloc považovaný za unidirectional data flow?
- Kdy použít `BlocListener` místo `BlocBuilder`?
- Jak testovat Bloc?
- Jakou roli má repository vrstva?

## Shrnutí

**Bloc** je robustní přístup ke state managementu založený na toku **event -> logic -> state -> UI**. Přináší jasnou strukturu, vysokou testovatelnost a dobré oddělení odpovědností. Je vhodný především pro střední a větší aplikace nebo tam, kde tým potřebuje disciplinovaný architektonický styl. Menší nevýhodou je větší množství kódu a vyšší vstupní složitost.