# Push notifikace a geolokace

## Charakter otázky

Tato otázka je **praktická**. Ve specifikaci je zmíněno, že je potřeba pokrýt **push** i **scheduled notifikace**, a také geolokaci. U zkoušky je důležité vysvětlit architekturu, oprávnění, implementační kroky i rizika z hlediska UX, bezpečnosti a spotřeby baterie.

## Push notifikace

### Co jsou push notifikace

**Push notifikace** jsou zprávy doručené do zařízení z externího systému, typicky backendu. Mohou se zobrazit i tehdy, když aplikace není otevřená.

Použití:

- upozornění na novou zprávu
- připomenutí důležité akce
- marketingová komunikace
- změna stavu objednávky

## Architektura push notifikací

Typický tok:

1. aplikace získá device token
2. token odešle na backend
3. backend pošle zprávu přes push službu
4. zařízení zprávu přijme
5. aplikace podle stavu zobrazí notifikaci nebo zpracuje data

Nejčastější služby:

- **FCM** neboli Firebase Cloud Messaging pro Android i iOS
- **APNs** neboli Apple Push Notification service na iOS straně

## Typy notifikací

### Push notifikace z backendu

Přicházejí ze serveru a hodí se pro události, které backend zná.

### Lokální neboli scheduled notifikace

Ty jsou naplánované přímo v zařízení, například:

- připomenutí úkolu v konkrétní čas
- denní návykové upozornění
- lokální připomenutí bez nutnosti backendu

## Scheduled notifikace

Scheduled notifikace jsou důležité zmínit, protože nevyžadují aktivní push server. Aplikace si sama naplánuje budoucí zobrazení.

Použití:

- budík nebo připomenutí
- kalendář
- habit tracking aplikace

Ve Flutteru se často používají balíčky jako `flutter_local_notifications`.

## Oprávnění k notifikacím

Notifikace nelze vnímat jen jako technickou funkci. Uživatel musí často udělit oprávnění.

Je potřeba řešit:

- kdy o oprávnění požádat
- jak vysvětlit, proč je notifikace užitečná
- co dělat, když uživatel oprávnění odmítne

To je důležité i z pohledu UX, protože předčasná nebo nejasná žádost o oprávnění snižuje šanci na souhlas.

## Stav aplikace při přijetí notifikace

Chování se liší podle toho, zda je aplikace:

- v popředí
- na pozadí
- ukončená

Je potřeba definovat, co se stane po kliknutí na notifikaci. Často se otevírá konkrétní obrazovka, tedy deep link nebo detail obsahu.

> [OBRÁZEK: tok kliknutí na push notifikaci vedoucí na konkrétní route aplikace]

## Best practices pro push notifikace

- neposílat zbytečně často
- segmentovat uživatele
- posílat relevantní obsah
- umět notifikaci otevřít na správné místo v aplikaci
- měřit open rate a dopad na engagement

Špatně navržené notifikace vedou k vypnutí oprávnění nebo odinstalaci aplikace.

## Geolokace

### Co je geolokace

**Geolokace** znamená získávání polohy zařízení. Aplikace může pracovat například s:

- aktuální polohou
- průběžným sledováním pohybu
- geofencingem
- převodem souřadnic na adresu a obráceně

## Zdroje polohy

Poloha může být určována pomocí:

- GPS
- Wi-Fi
- mobilní sítě
- kombinace více zdrojů

Vývojář obvykle neřeší detaily fyzického měření, ale pracuje přes systémová API a Flutter pluginy.

## Oprávnění pro geolokaci

Geolokace je citlivé oprávnění. Je potřeba jasně vysvětlit:

- proč aplikace polohu potřebuje
- zda ji potřebuje jen při použití aplikace nebo i na pozadí
- jak uživatel může oprávnění změnit

Na iOS i Androidu jsou pravidla přísná a špatné použití může vést k zamítnutí aplikace nebo nedůvěře uživatelů.

## Geolokace na pozadí

Pokud aplikace potřebuje sledovat polohu i mimo aktivní používání, musí řešit:

- background permission
- zvýšenou spotřebu baterie
- právní a store požadavky
- srozumitelné vysvětlení účelu

Toto je velmi citlivá oblast.

## Praktické použití geolokace

- mapa a zobrazení aktuální polohy
- nalezení nejbližší pobočky
- sledování zásilky nebo kurýra
- sportovní aplikace a měření trasy
- geofencing, například upozornění po příchodu do lokality

## Geocoding a reverse geocoding

Tyto pojmy je dobré znát:

- **geocoding** převádí adresu na souřadnice
- **reverse geocoding** převádí souřadnice na lidsky čitelnou adresu

To je praktické pro zobrazení lokality v UI.

## Výkon a baterie

Notifikace i geolokace mají dopad na výkon zařízení.

U geolokace zejména:

- časté aktualizace polohy vybíjejí baterii
- přesné měření je náročnější než hrubá poloha
- background tracking musí být velmi dobře odůvodněný

Vývojář musí najít kompromis mezi přesností a energetickou náročností.

## Bezpečnost a soukromí

Notifikace a geolokace pracují s citlivým kontextem uživatele. Je potřeba řešit:

- minimalizaci dat
- bezpečný přenos tokenů a polohy
- soulad s privacy policy
- transparentnost vůči uživateli

## Nejčastější chyby

- žádost o oprávnění bez vysvětlení hodnoty
- posílání nerelevantních notifikací
- neřešené deep linking chování po kliknutí na notifikaci
- přehnaně časté získávání polohy
- ignorování store pravidel pro background location

## Související témata

- **platform channels**
- **nasazení aplikace**, protože notifikace a polohy vyžadují správnou konfiguraci
- **UX/UI design**, protože povolení a notifikace mají velký dopad na zkušenost
- **monitoring a analytika**, protože chceme měřit open rate a usage

## Typické otázky u zkoušky

- Jaký je rozdíl mezi **push** a **scheduled** notifikací?
- Jak funguje tok push notifikace od backendu k zařízení?
- Proč je geolokace citlivé oprávnění?
- Co je **geofencing**?
- Jaké jsou dopady geolokace na baterii?

## Shrnutí

**Push notifikace** a **geolokace** jsou silné funkce mobilních aplikací, ale vyžadují správný návrh. Push notifikace mohou být vzdálené nebo lokální plánované a musí být relevantní, jinak poškozují UX. Geolokace umožňuje pracovat s polohou zařízení, ale přináší otázky oprávnění, soukromí a spotřeby baterie. V obou oblastech je zásadní kombinovat technickou implementaci s ohledem na uživatele, bezpečnost a pravidla platforem.