# Nasazení aplikace na Google Play a App Store

## Charakter otázky

Tato otázka je hlavně **praktická**, ale dotýká se i procesních a obchodních souvislostí. Ve specifikaci jsou zmíněna témata jako **bundle identifier**, podporované formáty, klíče, build, release proces nebo review. U zkoušky je potřeba ukázat, že student rozumí nejen vytvoření buildu, ale i tomu, co je potřeba před publikací a po ní.

## Co znamená nasazení mobilní aplikace

**Nasazení** nebo release znamená převést vývojovou verzi aplikace do podoby, kterou lze distribuovat uživatelům přes oficiální obchody:

- **Google Play** pro Android
- **App Store** pro iOS

Nejde jen o vygenerování instalačního souboru. Součástí je i:

- správná identita aplikace
- podepisování
- metadata
- ikonky, screenshoty a popisy
- právní a bezpečnostní informace
- review proces

## Bundle identifier a application ID

Každá aplikace má jednoznačný identifikátor.

### Android

Na Androidu se používá **applicationId**, například:

- `cz.firma.moje_aplikace`

### iOS

Na iOS se používá **bundle identifier**, například:

- `cz.firma.mojeAplikace`

Tento identifikátor:

- musí být unikátní
- váže se na certifikáty, služby a store listing
- obvykle se po vydání nemění

Pokud bychom ho změnili, obchod to často chápe jako úplně novou aplikaci.

## Build formáty pro jednotlivé platformy

### Android

Nejběžnější formáty:

- **APK** pro lokální instalaci a testování
- **AAB** neboli Android App Bundle pro Google Play

V praxi se pro store release používá hlavně **AAB**.

### iOS

Pro App Store se používá build archivovaný přes Xcode a distribuovaný do App Store Connect. Historicky se mluví o formátu **IPA**, ale v běžném release procesu se často pracuje přes archiv a upload do App Store Connect.

## Jak vytvořit build ve Flutteru

### Android release build

Příkazy:

- `flutter build appbundle`
- `flutter build apk --release`

### iOS release build

Typicky:

- `flutter build ipa`
- nebo sestavení přes Xcode archive workflow

Je důležité vědět, že iOS build často vyžaduje konfiguraci signing v Apple ekosystému.

## Podepisování aplikace

### Android signing

Android release build musí být podepsaný **keystore** klíčem.

Je potřeba řešit:

- vytvoření keystore
- bezpečné uložení hesel
- konfiguraci signing v Gradlu

Pokud ztratíme podepisovací klíč a nemáme správně nastavený release proces, může to být velmi vážný problém. U Google Play dnes část rizika řeší **Play App Signing**, kdy Google drží distribuční klíč a vývojář používá upload klíč.

### iOS signing

Apple používá:

- **certifikáty**
- **provisioning profiles**
- propojení s **Apple Developer Account**

Podepisování je zde tradičně komplikovanější než na Androidu a je potřeba správně nastavit tým, bundle identifier a capabilities.

## Na co si dát pozor před vydáním

Ve specifikaci je přímo otázka „na co si dát pozor“. Tady je vhodné odpovědět systematicky.

### Technické věci

- release build nesmí obsahovat debug konfiguraci
- správně nastavená oprávnění
- ikona aplikace a splash screen
- správná verze a build number
- otestovaná kompatibilita na více zařízeních
- správná konfigurace API endpointů pro produkci

### Produktové a UX věci

- aplikace musí mít dokončené základní flow
- nesmí padat při prvním spuštění
- onboarding a registrace musí být srozumitelné
- je dobré mít připravený fallback při chybě backendu

### Právní a store požadavky

- zásady ochrany osobních údajů
- informace o sběru dat
- věkové hodnocení
- popisy funkcí a screenshoty odpovídající realitě

## Verze aplikace

Každý release má obvykle dvě důležité hodnoty:

- **version name** nebo marketing version, například `1.4.0`
- **build number** nebo interní číslo buildu

Build number se zvyšuje při každém nahrání nové verze. Pokud se špatně spravuje verzování, může dojít k problémům při uploadu na store.

## Co dělat, pokud se release smaže

Ve specifikaci je i tato praktická otázka. Je potřeba rozlišit několik situací.

### Release smazaný v interním procesu

Pokud se smaže build nebo release artefakt interně, řešením bývá:

- znovu sestavit release z odpovídajícího commitu
- použít CI/CD artefakty nebo release storage
- navýšit build number a nahrát znovu

### Ztráta signing klíče

To je kritičtější problém.

Na Androidu může pomoci:

- **Play App Signing**
- reset upload klíče přes Google Play Console

Na iOS je potřeba znovu řešit certifikáty a provisioning profile v rámci Apple Developer ekosystému.

### Stažení release ze store

Pokud byl release stažen kvůli chybě nebo porušení pravidel:

- opravit problém
- navýšit verzi nebo build number
- znovu odeslat ke schválení
- analyzovat, proč se to stalo, a upravit proces

## Review proces na Google Play

Google Play má obvykle rychlejší a méně manuální proces než Apple, ale i tak kontroluje:

- bezpečnost a malware
- práci s daty a oprávněními
- soulad s pravidly obchodu
- obsah aplikace

V některých případech může být kontrola rychlá, jindy se protáhne. U citlivějších kategorií aplikací bývá přísnější.

## Review proces na App Store

Apple má tradičně přísnější review proces. Kontroluje například:

- stabilitu aplikace
- soulad s App Store Review Guidelines
- práci s platbami a předplatným
- používání systémových API
- kvalitu obsahu a UX

Apple může aplikaci odmítnout například kvůli:

- pádům při spuštění
- nefunkčnímu loginu
- nedostatečnému vysvětlení použití oprávnění
- zavádějícím screenshotům
- porušení pravidel pro platby

## Testovací distribuce před releasem

Před ostrým vydáním je vhodné používat testovací kanály.

### Android

- internal testing
- closed testing
- open testing

### iOS

- **TestFlight**

Tyto kanály umožňují ověřit chování aplikace na reálných zařízeních ještě před veřejným vydáním.

## Metadata v obchodech

Nasazení neznamená jen binárku. Je potřeba připravit:

- název aplikace
- popis
- krátký popis
- screenshoty
- promo grafiku
- ikony
- kontaktní údaje
- privacy policy

Špatně připravená metadata zhoršují schválení i marketingový výkon.

## Release checklist

- zkontrolovat verzi a build number
- přepnout aplikaci na produkční API
- ověřit signing
- spustit testy
- zkontrolovat crash-free start aplikace
- ověřit analytics a crash reporting
- připravit store metadata
- vytvořit rollback nebo hotfix plán

> [OBRÁZEK: release checklist od buildu přes signing až po store review]

## Související témata

- **Git a CI/CD**
- **Crashlytics a monitoring**
- **push notifikace**, pokud vyžadují produkční certifikáty
- **monetizace**, pokud aplikace obsahuje nákupy nebo předplatné

## Typické otázky u zkoušky

- Jaký je rozdíl mezi **APK** a **AAB**?
- Co je **bundle identifier**?
- Jak funguje signing na Androidu a iOS?
- Proč je důležité neztratit release klíče?
- Jak probíhá review na Apple a Google platformě?

## Shrnutí

Nasazení Flutter aplikace na **Google Play** a **App Store** je proces, který zahrnuje build, signing, verzování, metadata i schvalování. Na Androidu se dnes běžně publikuje **AAB**, na iOS se build distribuuje přes App Store Connect. Kritické je správně nastavit **bundle identifier/applicationId**, bezpečně spravovat klíče a mít pod kontrolou release proces. Úspěšné nasazení není jen technická kompilace, ale kombinace vývoje, testování, bezpečnosti, compliance a práce s obchodními pravidly platforem.