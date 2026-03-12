# Pokročilé widgety a layouty

## Charakter otázky

Tato otázka je převážně **praktická**, ale bez teoretického pochopení layout systému se nedá správně zodpovědět. Cílem je ukázat, jak ve Flutteru řešit složitější obrazovky, scrollovatelné rozhraní, adaptivní rozložení a animované přechody mezi prvky.

## Proč nestačí jen základní widgety

Jednoduchou obrazovku lze vytvořit pomocí **Column**, **Row**, **Container** a **ListView**. Jakmile ale aplikace obsahuje:

- složitější feed nebo dashboard
- kombinaci více scrollovacích částí
- responzivní rozložení pro telefon a tablet
- animované přechody a překryvy

je potřeba použít pokročilejší widgety a lépe chápat výkon i chování layoutu.

## SingleChildScrollView

**SingleChildScrollView** je vhodný, když máme menší množství obsahu, které se nevejde na obrazovku, ale stále tvoří jeden celek.

Použití:

- formuláře
- detail obrazovky
- krátké informační obrazovky

Nevhodné použití:

- velké seznamy dat
- dlouhé feedy

Na velké seznamy je lepší **ListView** nebo slivery.

## GridView

**GridView** slouží pro mřížkové uspořádání prvků. Typicky:

- galerie obrázků
- katalog produktů
- dashboard karty

Nejčastější varianty:

- `GridView.count()`
- `GridView.extent()`
- `GridView.builder()`

Prakticky je nejčastější builder varianta.

```dart
GridView.builder(
  gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
    crossAxisCount: 2,
    crossAxisSpacing: 12,
    mainAxisSpacing: 12,
  ),
  itemCount: 20,
  itemBuilder: (context, index) {
    return Card(child: Center(child: Text('Položka $index')));
  },
)
```

## Wrap

**Wrap** je užitečný tam, kde nechceme přetékání v jedné ose, ale automatické zalomení do více řádků nebo sloupců.

Použití:

- tagy
- filtry
- čipy
- skupiny menších tlačítek

Rozdíl oproti **Row** je, že Wrap umí prvky zalomit.

## PageView

**PageView** vytváří horizontálně nebo vertikálně přepínatelné stránky. Hodí se například pro:

- onboarding
- galerie
- swipeable obsah

Často se kombinuje s **PageController**.

## TabBar a TabBarView

Pokud aplikace obsahuje více logicky oddělených částí, je vhodné použít:

- **TabBar** pro záložky
- **TabBarView** pro jejich obsah

Tyto widgety se často napojují na **DefaultTabController**.

## CustomScrollView a slivery

Tohle je jedno z nejdůležitějších pokročilých témat.

**CustomScrollView** umožňuje kombinovat více typů scrollovatelného obsahu do jednoho scrollu. Je postavený na konceptu **sliverů**.

### Co je sliver

**Sliver** je speciální scrollovatelný blok, který se chová chytře vzhledem ke scroll pozici.

Příklady sliver widgetů:

- **SliverAppBar**
- **SliverList**
- **SliverGrid**
- **SliverToBoxAdapter**
- **SliverFillRemaining**

### Výhody sliverů

- lze kombinovat různé typy obsahu v jednom scroll kontextu
- lepší kontrola nad kolabující app bar lištou
- efektivnější řešení složitých layoutů než vnořené ListView

```dart
CustomScrollView(
  slivers: [
    const SliverAppBar(
      pinned: true,
      expandedHeight: 200,
      flexibleSpace: FlexibleSpaceBar(title: Text('Detail')),
    ),
    SliverList(
      delegate: SliverChildBuilderDelegate(
        (context, index) => ListTile(title: Text('Položka $index')),
        childCount: 30,
      ),
    ),
  ],
)
```

> [OBRÁZEK: obrazovka se SliverAppBar, kolabujícím headerem a navazujícím seznamem]

## NestedScrollView

**NestedScrollView** se používá, když chceme propojit více scrollovacích oblastí, typicky:

- kolabující header
- uvnitř taby
- v každém tabu vlastní scroll obsah

Je to užitečné, ale někdy i zrádné. Pokud lze problém vyřešit přes **CustomScrollView**, bývá to často přehlednější.

## LayoutBuilder

**LayoutBuilder** umožňuje reagovat na dostupné constraints v konkrétní části stromu widgetů. Hodí se pro:

- adaptivní layout
- změnu počtu sloupců podle šířky
- přepínání mezi mobilním a tabletovým rozvržením

```dart
LayoutBuilder(
  builder: (context, constraints) {
    if (constraints.maxWidth > 600) {
      return const TwoPaneLayout();
    }
    return const OnePaneLayout();
  },
)
```

## FractionallySizedBox a AspectRatio

Tyto widgety pomáhají s přesnějším řízením rozměrů.

### FractionallySizedBox

Umožňuje nastavit velikost jako podíl rodičovského prostoru.

### AspectRatio

Zachovává poměr stran. Hodí se například pro:

- náhledy videí
- bannery
- obrázky v kartách

## FittedBox a Intrinsic widgety

### FittedBox

**FittedBox** přizpůsobuje velikost dítěte tak, aby se vešlo do dostupného prostoru.

### IntrinsicHeight a IntrinsicWidth

Tyto widgety umí pomoci u specifických layout problémů, ale bývají náročnější na výkon. Proto se nemají používat bez rozmyslu.

## Reusable composable widgets

Pokročilý layout není jen o správném widgetu, ale i o struktuře kódu. Větší obrazovky by se měly skládat z menších částí:

- header sekce
- content sekce
- action panel
- list item widget

To přináší:

- lepší čitelnost
- snadnější testování
- menší riziko chyb při změnách

## Animované widgety

Do pokročilých widgetů se často řadí i základní animační stavebnice:

- **AnimatedContainer**
- **AnimatedOpacity**
- **AnimatedSwitcher**
- **Hero**

### Hero

**Hero** vytváří plynulý přechod stejného prvku mezi dvěma obrazovkami. Typicky obrázek produktu nebo avatar.

### AnimatedSwitcher

Hodí se pro přepínání obsahu s automatickou animací mezi dvěma stavy.

## CustomMultiChildLayout a CustomSingleChildLayout

Tyto widgety se používají méně často, ale jsou důležité pro speciální případy. Umožňují definovat vlastní layout logiku pomocí delegáta.

V praxi jsou vhodné spíše pro opravdu nestandardní rozložení. Ve většině běžných obrazovek stačí standardní widgety.

## Výkon u pokročilých layoutů

Složitý layout může negativně ovlivnit výkon. Je třeba myslet na:

- zbytečné rebuildy
- příliš hluboký strom widgetů
- nevhodné kombinace scroll widgetů
- renderování velkého množství prvků najednou

Praktická doporučení:

- pro dlouhé seznamy používat builder varianty
- zbytečně nevnořovat více scroll pohledů
- rozdělovat velké widgety do menších částí
- používat `const`, kde to dává smysl

## Nejčastější chyby

- **ListView** uvnitř **Column** bez omezení
- **SingleChildScrollView** pro obrovské množství dat
- kombinace více scrollů bez pochopení, který widget řídí scrollování
- příliš mnoho logiky přímo v metodě `build`

## Kdy použít který přístup

- pro jednoduchý seznam: **ListView**
- pro jednoduchou mřížku: **GridView**
- pro obsah s přesahem přes více řádků: **Wrap**
- pro onboarding nebo swipe stránky: **PageView**
- pro složitou scroll obrazovku: **CustomScrollView** a **slivery**
- pro adaptivní rozložení: **LayoutBuilder**

## Související témata

- **základní widgety layoutu**
- **navigace mezi obrazovkami**
- **asynchronní načítání do seznamů**
- **UX/UI design**
- **performance optimalizace ve Flutteru**

## Typické otázky u zkoušky

- K čemu slouží **SliverAppBar**?
- Jaký je rozdíl mezi **ListView** a **CustomScrollView**?
- Kdy použít **Wrap** místo **Row**?
- Jak řešit adaptivní layout pro tablet?
- Proč může být **IntrinsicHeight** drahý na výkon?

## Shrnutí

Pokročilé widgety a layouty umožňují vytvářet profesionální a škálovatelné rozhraní. Klíčové je umět pracovat s **GridView**, **PageView**, **Wrap**, **LayoutBuilder** a hlavně se systémem **sliverů** přes **CustomScrollView**. Nestačí znát názvy widgetů, důležité je rozumět tomu, kdy je použít, jaké mají dopady na výkon a jak se vyhnout zbytečně složitým nebo nestabilním layoutům.