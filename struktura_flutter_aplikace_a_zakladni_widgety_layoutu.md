# Struktura Flutter aplikace a základní widgety layoutu

## Charakter otázky

Tato otázka je hlavně **praktická**. Zkoušející obvykle očekává, že student popíše, jak vypadá základ Flutter projektu, jak se skládá obrazovka a jak fungují nejběžnější layout widgety. Současně je vhodné krátce vysvětlit teorii layout systému.

## Základní struktura Flutter projektu

Po vytvoření nového projektu například příkazem `flutter create` vznikne několik důležitých složek a souborů.

### Nejdůležitější části projektu

- **lib/** obsahuje hlavní zdrojové kódy aplikace
- **main.dart** bývá vstupní bod aplikace
- **pubspec.yaml** definuje závislosti, assets a metadata projektu
- **android/** obsahuje nativní Android část
- **ios/** obsahuje nativní iOS část
- **test/** obsahuje testy

V běžné praxi se projekt postupně rozděluje do více vrstev, například:

- **presentation** pro UI
- **domain** pro byznys logiku
- **data** pro API, databáze a repository

## Vstupní bod aplikace

Základ obvykle začíná v souboru `main.dart`.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: const HomePage(),
    );
  }
}
```

Funkce **runApp()** vloží kořenový widget do stromu aplikace. Tím začíná vykreslování celé aplikace.

## MaterialApp a CupertinoApp

Na kořeni aplikace často stojí:

- **MaterialApp** pro Material Design styl
- **CupertinoApp** pro iOS-like styl

Většina projektů používá **MaterialApp**, protože nabízí:

- definici tématu
- navigaci
- lokalizaci
- pojmenované routy
- základní konfiguraci aplikace

## Scaffold jako kostra obrazovky

Na úrovni jednotlivých obrazovek je velmi častý widget **Scaffold**. Poskytuje typickou strukturu obrazovky.

Obsahuje například:

- **appBar**
- **body**
- **floatingActionButton**
- **drawer**
- **bottomNavigationBar**

```dart
class HomePage extends StatelessWidget {
  const HomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Domů')),
      body: const Center(child: Text('Obsah obrazovky')),
    );
  }
}
```

> [OBRÁZEK: schéma obrazovky se Scaffold, AppBar, Body a BottomNavigationBar]

## Základní princip layoutu ve Flutteru

Flutter používá systém **constraints go down, sizes go up, parent sets position**. To je velmi důležitá věta.

Znamená to, že:

1. rodič pošle dítěti omezení, tedy **constraints**
2. dítě si spočítá svoji velikost
3. rodič rozhodne, kam dítě umístí

Kdo tomuto rozumí, lépe chápe, proč někdy vznikají chyby typu:

- overflow
- unbounded height
- unbounded width

## Nejčastější základní layout widgety

### Container

**Container** je univerzální obalový widget. Umí řešit:

- velikost
- barvu
- margin
- padding
- dekoraci
- zarovnání

```dart
Container(
  padding: const EdgeInsets.all(16),
  color: Colors.blue,
  child: const Text('Ahoj'),
)
```

Je ale dobré vědět, že v produkčním kódu se někdy zbytečně nadužívá. Když stačí jen odsazení, může být vhodnější použít přímo **Padding**.

### Padding

**Padding** přidává vnitřní odsazení kolem dítěte.

```dart
Padding(
  padding: const EdgeInsets.all(12),
  child: Text('Text'),
)
```

### Center

**Center** umístí dítě doprostřed dostupného prostoru.

### SizedBox

**SizedBox** slouží pro:

- pevnou šířku nebo výšku
- mezeru mezi prvky

```dart
const SizedBox(height: 16)
```

### Row

**Row** rozkládá děti vodorovně.

Časté vlastnosti:

- **mainAxisAlignment**
- **crossAxisAlignment**
- **mainAxisSize**

```dart
Row(
  mainAxisAlignment: MainAxisAlignment.spaceBetween,
  children: const [
    Icon(Icons.menu),
    Text('Titulek'),
    Icon(Icons.search),
  ],
)
```

### Column

**Column** je vertikální obdoba Row. Velmi často se používá pro skládání formulářů, detailů obrazovky nebo skupin obsahu.

### Expanded a Flexible

Tyto widgety řeší, jak se mají děti v rámci **Row** nebo **Column** dělit o prostor.

- **Expanded** vyplní dostupný prostor
- **Flexible** dává dítěti flexibilitu, ale nevnucuje maximální roztažení vždy stejně přísně

```dart
Row(
  children: const [
    Expanded(child: Text('Dlouhý text vlevo')),
    SizedBox(width: 8),
    Icon(Icons.star),
  ],
)
```

### Stack

**Stack** umožňuje vrstvit widgety přes sebe.

Použití:

- badge přes ikonu
- text přes obrázek
- překrývání prvků v hero sekci

S widgetem **Positioned** lze určit přesnou pozici.

### ListView

**ListView** se používá pro posouvatelný seznam položek. Existuje více variant:

- `ListView()`
- `ListView.builder()`
- `ListView.separated()`

Pro větší seznamy je nejvhodnější **builder**, protože vytváří položky až podle potřeby.

```dart
ListView.builder(
  itemCount: items.length,
  itemBuilder: (context, index) {
    return ListTile(title: Text(items[index]));
  },
)
```

## Text, Image, Icon a základní obsahové widgety

Kromě layoutu je vhodné zmínit i běžné obsahové widgety:

- **Text** pro textový obsah
- **Image** pro obrázky z assetů, sítě nebo souboru
- **Icon** pro ikonky
- **ElevatedButton**, **TextButton**, **OutlinedButton** pro akce
- **TextField** pro vstup uživatele

## Jak se widgety skládají dohromady

V praxi většina obrazovek vzniká skládáním jednoduchých widgetů do větších celků.

Příklad jednoduché obrazovky:

```dart
Scaffold(
  appBar: AppBar(title: const Text('Profil')),
  body: Padding(
    padding: const EdgeInsets.all(16),
    child: Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: const [
        CircleAvatar(radius: 32),
        SizedBox(height: 16),
        Text('Jan Novák'),
        SizedBox(height: 8),
        Text('Mobilní vývojář'),
      ],
    ),
  ),
)
```

## Časté chyby začátečníků

### Overflow

Typicky vzniká, když se obsah nevejde do dostupného prostoru, například při použití **Column** bez scrollu.

Řešení:

- použít **SingleChildScrollView**
- použít **Expanded**
- upravit layout

### Nekonečné rozměry

Například **ListView** uvnitř **Column** bez omezení výšky může způsobit chybu, protože seznam neví, jak vysoký má být.

Řešení:

- obalit **Expanded**
- použít `shrinkWrap` jen opatrně, protože může mít dopad na výkon

### Přehnané zanoření

Pokud je strom widgetů příliš hluboký a nepřehledný, je vhodné rozdělit UI do menších vlastních widgetů.

## Responsivní layout

U mobilních aplikací je důležité myslet na různé velikosti obrazovek. Základní nástroje:

- **MediaQuery**
- **LayoutBuilder**
- adaptivní větvení podle šířky

Je vhodné nemít všechno natvrdo, například pevné šířky pro všechny prvky.

## Doporučení pro praxi

- rozdělovat velké obrazovky do menších widgetů
- používat správný widget pro konkrétní účel místo univerzálního Containeru všude
- chápat constraints a neřešit layout metodou pokus-omyl
- dbát na čitelné odsazení a konzistentní strukturu kódu

## Související témata

Na tuto otázku navazují:

- **pokročilé widgety a layouty**
- **navigace mezi obrazovkami**
- **state management**
- **asynchronní načítání dat do seznamů**

## Typické otázky u zkoušky

- Jaký je rozdíl mezi **Row** a **Column**?
- Kdy použít **Expanded** a kdy **Flexible**?
- Proč vzniká overflow?
- Jakou roli má **Scaffold**?
- Jak funguje layout systém Flutteru?

## Shrnutí

Základní struktura Flutter aplikace začíná v **main.dart** a kořenovém widgetu jako **MaterialApp**. Jednotlivé obrazovky často používají **Scaffold**. Layout ve Flutteru je založený na systému **constraints**, a proto je potřeba rozumět widgetům jako **Row**, **Column**, **Expanded**, **Stack** nebo **ListView**. Kdo dobře chápe tyto základy, dokáže navrhovat přehledné a stabilní obrazovky a snáze se orientuje i v pokročilejších layoutech.