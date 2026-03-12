# Práce s nativními funkcemi pomocí platform channels

## Charakter otázky

Tato otázka je **praktická**, ale je nutné vysvětlit i teoretický princip, proč vůbec **platform channels** existují. U zkoušky je potřeba ukázat, že Flutter sice umožňuje psát většinu aplikace v Dartu, ale někdy je nutné komunikovat s nativní vrstvou Androidu nebo iOS.

## Proč jsou platform channels potřeba

Flutter sám nepokrývá všechna nativní API přímo v čistém Dart kódu. V praxi se často setkáme se situací, kdy potřebujeme:

- přístup k senzoru nebo specifické hardwarové funkci
- využít nativní SDK třetí strany
- zavolat platform-specific API, které Flutter balíček nepodporuje
- napsat část funkcionality v Kotlinu, Swiftu nebo jiném nativním jazyce

Flutter proto nabízí mechanismus **platform channels**, který propojuje Dart vrstvu s nativním kódem.

## Základní princip komunikace

Platform channel je komunikační kanál mezi:

- **Dart** částí aplikace
- **Android** částí, typicky v Kotlinu nebo Javě
- **iOS** částí, typicky ve Swiftu nebo Objective-C

Tok vypadá typicky takto:

1. Dart odešle požadavek na pojmenovaný channel
2. nativní strana tento požadavek zachytí
3. provede akci na platformě
4. vrátí výsledek nebo chybu zpět do Dartu

> [OBRÁZEK: schéma Dart -> MethodChannel -> Kotlin/Swift -> výsledek zpět]

## Typy platform channels

### MethodChannel

Nejpoužívanější typ. Slouží pro volání metod stylem request-response.

Použití:

- získání verze OS
- spuštění jednorázové nativní akce
- přístup k nastavení zařízení

### EventChannel

Používá se pro proud událostí ze systému do Flutteru.

Použití:

- senzorická data
- průběžné změny stavu
- streamování informací z nativní vrstvy

### BasicMessageChannel

Obecnější message-based komunikace, používá se méně často.

## MethodChannel v praxi

### Dart část

```dart
import 'package:flutter/services.dart';

class DeviceInfoService {
  static const _channel = MethodChannel('app/device_info');

  Future<String> getPlatformVersion() async {
    final version = await _channel.invokeMethod<String>('getPlatformVersion');
    return version ?? 'unknown';
  }
}
```

### Android část v Kotlinu

Konceptuálně se v `MainActivity` nebo pluginu zaregistruje listener:

```kotlin
MethodChannel(flutterEngine.dartExecutor.binaryMessenger, "app/device_info")
  .setMethodCallHandler { call, result ->
    if (call.method == "getPlatformVersion") {
      result.success("Android ${android.os.Build.VERSION.RELEASE}")
    } else {
      result.notImplemented()
    }
  }
```

### iOS část ve Swiftu

Podobně se registruje handler i na iOS.

## Datové typy a serializace

Při komunikaci přes platform channels se předávají serializovatelné hodnoty, například:

- `String`
- `int`
- `double`
- `bool`
- `List`
- `Map`

Pokud potřebujeme složitější objekt, obvykle ho mapujeme na slovník nebo JSON-like strukturu.

## Chyby a výjimky

Nativní vrstva může vrátit:

- úspěšný výsledek
- chybu
- informaci, že metoda není implementována

Na Dart straně je potřeba chyby zachytit například přes `PlatformException`.

```dart
try {
  await _channel.invokeMethod('openNativeFeature');
} on PlatformException catch (error) {
  print('Chyba platform channel: ${error.message}');
}
```

## Kdy psát vlastní platform channel a kdy použít plugin

Ve většině případů je lepší nejdřív hledat hotový balíček v ekosystému Flutteru. Například pro:

- kameru
- polohu
- push notifikace
- biometriku

Pokud ale:

- neexistuje vhodný plugin
- potřebujeme interní firemní SDK
- chceme specifické chování

pak má smysl napsat vlastní platform channel nebo celý vlastní plugin.

## Plugin vs app-specific channel

### App-specific channel

Rychlé řešení přímo v konkrétní aplikaci. Vhodné, když funkcionalita nebude znovu použitelná.

### Vlastní Flutter plugin

Lepší, pokud chceme funkcionalitu:

- použít ve více projektech
- oddělit od samotné aplikace
- lépe testovat a verzovat

## Typické use-casy

- přístup k nativnímu Bluetooth API
- komunikace s NFC
- propojení s interním platebním terminálem
- integrace proprietárního SDK výrobce zařízení
- přístup k OS-level nastavením

## Výhody platform channels

- umožňují využít plný potenciál nativní platformy
- odemykají integraci s platform-specific SDK
- zajišťují, že Flutter aplikace není omezená jen na čistý Dart ekosystém

## Nevýhody a rizika

- zvyšují složitost projektu
- je potřeba umět i nativní vývoj nebo alespoň rozumět základům
- vzniká větší rozdíl mezi Android a iOS implementací
- hůř se testují než čistě Dart části
- mohou komplikovat údržbu a upgrade balíčků

## Architektonická doporučení

- neschovávat volání kanálů přímo do widgetů
- vytvořit service vrstvu, například `DeviceService` nebo `NativeBridge`
- mapovat nativní chyby na srozumitelný doménový model
- pokud je logika větší, oddělit ji do pluginu

## Testování

Testování platform channels má více úrovní:

- unit test Dart vrstvy přes mock channel nebo mock service
- integrační testy aplikace
- manuální testování přímo na Android a iOS zařízení

Je důležité testovat obě platformy, protože implementace může být odlišná.

## Nejčastější chyby

- nejednotný název channelu mezi Dart a nativní vrstvou
- jiné názvy metod na obou stranách
- neřešené výjimky
- vracení neserializovatelných objektů
- přímé míchání platform channel logiky do UI

## Související témata

- **push notifikace**
- **geolokace**
- **nasazení aplikace**, protože nativní integrace může vyžadovat konfiguraci manifestu nebo Info.plist
- **práce s nativními SDK třetích stran**

## Typické otázky u zkoušky

- Co je **MethodChannel**?
- Jaký je rozdíl mezi **MethodChannel** a **EventChannel**?
- Kdy použít vlastní channel a kdy plugin?
- Jak se řeší chyby v komunikaci?
- Proč se platform channels používají, když Flutter je multiplatformní?

## Shrnutí

**Platform channels** jsou most mezi Dart světem Flutteru a nativní vrstvou Androidu a iOS. Nejčastěji se používá **MethodChannel** pro jednorázové volání a **EventChannel** pro proud událostí. Díky nim lze integrovat nativní SDK a platform-specific funkce, které nejsou dostupné přímo ve Flutteru. Nevýhodou je vyšší složitost a potřeba řešit více platforem odděleně, proto je vhodné používat je s rozvahou a ideálně je skrýt za servisní vrstvu nebo plugin.