# Navigace mezi obrazovkami

## Charakter otázky

Tato otázka je **praktická**. Ve specifikaci jsou zmíněny dva přístupy: **auto route** a **nativní navigace**, proto je potřeba vysvětlit oba. Student by měl popsat základní principy navigace ve Flutteru, práci se stackem obrazovek, předávání argumentů a rozdíly mezi ručním a generovaným routingem.

## Co je navigace v mobilní aplikaci

**Navigace** řeší pohyb uživatele mezi obrazovkami aplikace. Typické akce:

- otevření detailu položky
- návrat zpět
- přechod po přihlášení na hlavní obrazovku
- otevření modálního dialogu
- navigace pomocí spodní lišty nebo tabů

Ve Flutteru se navigace tradičně řeší pomocí **Navigator** a zásobníku rout.

## Základní princip: stack obrazovek

Flutter používá model **stacku**:

- nová obrazovka se vloží na vrchol zásobníku
- návrat odstraní horní obrazovku
- aktuálně viditelná je horní route

To odpovídá běžnému chování mobilních aplikací.

> [OBRÁZEK: stack rout s Home -> Detail -> Edit a následným pop návratem]

## Nativní navigace ve Flutteru

Tím se zde myslí standardní navigace pomocí Flutter API bez externího router generatoru.

### Navigator.push

Nejběžnější způsob otevření nové obrazovky:

```dart
Navigator.of(context).push(
  MaterialPageRoute(
    builder: (_) => const DetailPage(),
  ),
);
```

### Navigator.pop

Návrat na předchozí obrazovku:

```dart
Navigator.of(context).pop();
```

### Vrácení výsledku

Navigace může vracet hodnotu zpět:

```dart
final result = await Navigator.of(context).push<String>(
  MaterialPageRoute(builder: (_) => const EditPage()),
);
```

Na druhé obrazovce:

```dart
Navigator.of(context).pop('ulozeno');
```

To je praktické například pro výběr položky nebo potvrzení akce.

## Named routes

Flutter podporuje i pojmenované routy, například:

```dart
MaterialApp(
  routes: {
    '/': (_) => const HomePage(),
    '/detail': (_) => const DetailPage(),
  },
)
```

Volání:

```dart
Navigator.of(context).pushNamed('/detail');
```

### Výhody named routes

- jednodušší centralizace
- přehledněji u menších aplikací

### Nevýhody

- horší typová bezpečnost argumentů
- při větším projektu se trasy hůř spravují
- komplikovanější deep linking a guard logika

## Navigator 1.0 a Navigator 2.0

Je dobré zmínit, že Flutter má dva navigační modely:

- **Navigator 1.0** je imperativní přístup přes push/pop
- **Navigator 2.0** je deklarativnější a hodí se pro komplexnější routing, web a deep links

Ve většině běžných mobilních aplikací se vývojář setká buď s Navigator 1.0, nebo s knihovnami, které složitost Navigatoru 2.0 abstrahují.

## AutoRoute

**AutoRoute** je populární balíček pro správu routingu, který generuje kód a přináší silnější typovou bezpečnost, lepší organizaci a podporu pokročilejších scénářů.

### Proč ho používat

- přehledná centralizace rout
- typově bezpečné argumenty
- snadnější nested routing
- guardy pro autorizaci
- lepší práce s deep linkingem

## Základní princip AutoRoute

Vývojář nadefinuje routy v anotacích nebo konfigurační třídě a nástroj vygeneruje potřebný kód.

Příklad konceptuálně:

```dart
@AutoRouterConfig()
class AppRouter extends RootStackRouter {
  @override
  List<AutoRoute> get routes => [
    AutoRoute(page: HomeRoute.page, initial: true),
    AutoRoute(page: ProductDetailRoute.page),
  ];
}
```

Volání navigace:

```dart
context.router.push(ProductDetailRoute(productId: 42));
```

To je výrazně bezpečnější než ručně předávat mapu argumentů přes string klíče.

## Route guards

Jedna z hlavních výhod AutoRoute jsou **guards**. Umožňují rozhodnout, zda uživatel smí na danou route vstoupit.

Použití:

- uživatel musí být přihlášený
- onboarding musí být dokončen
- některé části aplikace jsou jen pro admina

Guard dokáže uživatele přesměrovat například na login.

## Nested navigation

Větší aplikace často obsahují vnořenou navigaci, například:

- spodní navigace s více taby
- každý tab má vlastní stack obrazovek

To bývá u ruční navigace složitější. Router knihovny jako AutoRoute to řeší přehledněji.

## Deep linking

**Deep link** je odkaz, který otevře konkrétní místo v aplikaci. Například notifikace může otevřít detail objednávky.

Navigační vrstva musí umět:

- rozpoznat cílovou route
- případně zpracovat parametry
- správně sestavit stack obrazovek

To je velmi důležité u větších aplikací a u webové podpory.

## Předávání argumentů

Při navigaci často potřebujeme předat data, například:

- ID produktu
- název kategorie
- objekt s filtrem

U ruční navigace se běžně předávají přes konstruktor obrazovky. U router knihoven přes generované route třídy.

## Dialogy, bottom sheet a modální navigace

Ne všechna navigace znamená plnou obrazovku. Flutter podporuje i další vrstvy interakce:

- **showDialog**
- **showModalBottomSheet**
- fullscreen dialog

I to je součást navigačního návrhu aplikace.

## Nejčastější chyby

- navigace přímo v build metodě
- neřešení back stack chování po loginu nebo logoutu
- předávání nevalidních argumentů bez typové kontroly
- nepřehledný routing rozesetý po celém projektu
- absence strategie pro deep links a guardy

## Kdy zvolit ruční navigaci a kdy AutoRoute

### Ruční navigace

Vhodná když:

- projekt je malý nebo střední
- navigační tok je jednoduchý
- nechceme code generation

### AutoRoute

Vhodná když:

- projekt je větší
- potřebujeme guardy a nested routing
- chceme typovou bezpečnost argumentů
- řešíme deep linking a škálovatelnost

## Související témata

- **struktura Flutter aplikace**
- **push notifikace**, protože často otevírají konkrétní route
- **state management**, například redirect podle stavu přihlášení
- **nasazení aplikace**, pokud řešíme app links nebo universal links

## Typické otázky u zkoušky

- Jak funguje `Navigator.push` a `Navigator.pop`?
- Jaký je rozdíl mezi ruční navigací a **AutoRoute**?
- Co je **route guard**?
- Jak předat data mezi obrazovkami?
- Jak řešit deep links?

## Shrnutí

Navigace je základní součást mobilní aplikace. Ve Flutteru lze použít buď standardní **Navigator** s imperativním push/pop přístupem, nebo robustnější routing knihovny jako **AutoRoute**. Pro menší aplikace často stačí ruční navigace, zatímco větší projekty těží z typové bezpečnosti, guardů a centralizace rout. Důležité je rozumět stack modelu, předávání argumentů i scénářům jako login redirect nebo deep linking.