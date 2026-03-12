# Integrace Backend as a Service (BaaS) - Supabase a Firebase

## Charakter otázky

Tato otázka je převážně **praktická**, ale je potřeba dobře vysvětlit i teoretický koncept **BaaS**. Student by měl ukázat, že rozumí tomu, co Backend as a Service řeší, kdy je vhodné ho použít a jaké jsou rozdíly mezi službami **Firebase** a **Supabase**.

## Co je Backend as a Service

**Backend as a Service** je model, kdy vývojář nebuduje celý backend od nuly, ale využije hotovou cloudovou platformu, která poskytuje běžné backendové funkce jako službu.

Typicky nabízí:

- autentizaci
- databázi
- úložiště souborů
- notifikace
- serverless funkce
- analytiku a monitoring

Smyslem je zrychlit vývoj, zejména u MVP nebo menšího týmu.

## Proč BaaS dává smysl

Při vývoji produktu tým často nechce řešit od prvního dne:

- správu serverů
- databázovou infrastrukturu
- autentizační mechanizmy
- realtime synchronizaci
- základní administraci backendu

BaaS umožňuje soustředit se více na produkt a frontend aplikace.

## Typické funkce BaaS platforem

- **Auth**: registrace, login, reset hesla, social login
- **Database**: ukládání dat a dotazy
- **Storage**: nahrávání souborů a obrázků
- **Realtime**: živé aktualizace dat
- **Functions**: vlastní serverová logika
- **Analytics/Monitoring**: sledování používání a chyb

## Firebase

**Firebase** je BaaS platforma od Google. Ve Flutter světě je velmi rozšířená.

### Hlavní služby Firebase

- **Firebase Authentication**
- **Cloud Firestore**
- **Realtime Database**
- **Firebase Storage**
- **Cloud Functions**
- **Firebase Cloud Messaging**
- **Crashlytics**
- **Analytics**

### Výhody Firebase

- silná integrace s Flutter ekosystémem
- rozsáhlá dokumentace
- rychlý start projektu
- dobrá podpora notifikací a monitoringu
- snadné použití pro MVP i produkční aplikace

### Nevýhody Firebase

- vendor lock-in
- některé části návrhu databáze vyžadují jiné myšlení než klasické SQL
- náklady mohou růst s provozem
- omezenější transparentnost oproti open-source řešením

## Firestore vs Realtime Database

Firebase historicky nabízí dvě databázová řešení.

### Realtime Database

- starší řešení
- JSON strom
- velmi jednoduchá realtime synchronizace

### Cloud Firestore

- modernější a častější volba
- kolekce a dokumenty
- lepší škálování
- silnější dotazovací schopnosti

U zkoušky je dobré zmínit, že dnes se častěji používá **Cloud Firestore**.

## Supabase

**Supabase** je moderní open-source orientovaná BaaS platforma, často vnímaná jako alternativa k Firebase.

### Hlavní služby Supabase

- autentizace
- PostgreSQL databáze
- storage
- realtime funkce
- edge functions

### Výhody Supabase

- SQL databáze **PostgreSQL**
- open-source přístup
- transparentnější datový model
- vhodné pro týmy, které chtějí klasické relační schéma

### Nevýhody Supabase

- menší ekosystém než Firebase
- některé služby nemusí být tak vyspělé nebo přímo integrované jako u Firebase
- u některých scénářů je potřeba více backendového porozumění

## Firebase vs Supabase

| Oblast | Firebase | Supabase |
| --- | --- | --- |
| Databáze | NoSQL dokumentová | PostgreSQL relační |
| Ekosystém | velmi široký | menší, ale silný |
| Open source | spíše ne | silnější open-source orientace |
| Realtime | velmi dobře podporovaný | také podporovaný |
| Monitoring | silný, včetně Crashlytics | slabší vestavěný ekosystém |

## Autentizace v BaaS

Jednou z nejčastějších funkcí je **auth**. Platforma řeší například:

- registraci e-mailem a heslem
- potvrzovací e-mail
- reset hesla
- přihlášení přes Google, Apple nebo jiné identity
- správu session a tokenů

To výrazně šetří čas oproti vlastní implementaci.

## Databáze a datový model

### Firebase přístup

Vývojář často přemýšlí více dokumentově. Musí řešit, jak strukturovat data bez klasických joinů.

### Supabase přístup

Vývojář přemýšlí relačněji. Může využít:

- tabulky
- cizí klíče
- SQL dotazy
- pohledy a funkce

Proto bývá Supabase atraktivní pro týmy, které chtějí klasickou databázovou logiku.

## Bezpečnost a pravidla přístupu

BaaS neznamená, že bezpečnost je automaticky vyřešená. Je nutné správně nastavit:

- pravidla přístupu k datům
- autentizaci
- role a oprávnění
- ochranu citlivých operací

Například Firestore security rules nebo Supabase row-level security jsou kritickou součástí návrhu.

## Výhody BaaS obecně

- rychlejší vývoj
- menší potřeba backend týmu na začátku
- hotové řešení běžných problémů
- rychlejší cesta k MVP

## Nevýhody BaaS obecně

- závislost na poskytovateli
- horší kontrola nad infrastrukturou
- riziko rostoucích nákladů
- limity pro velmi specifické byznys požadavky

## Kdy BaaS použít

BaaS je vhodný když:

- tým je malý
- chceme rychle ověřit produkt
- nepotřebujeme složitou vlastní backend logiku
- chceme rychle získat auth, databázi a storage

Méně vhodný může být když:

- máme velmi specifické enterprise požadavky
- potřebujeme plnou kontrolu nad infrastrukturou
- čekáme velmi nestandardní datový model nebo komplexní backend doménu

## Praktická integrace ve Flutteru

Ve Flutter aplikaci typicky:

1. přidáme SDK balíčky
2. nakonfigurujeme projekt pro Android a iOS
3. inicializujeme službu v `main()`
4. vytvoříme datové a auth service vrstvy
5. napojíme UI přes state management

## Související témata

- **asynchronní operace**
- **druhy API**
- **push notifikace**
- **databáze a úložiště**
- **nasazení aplikace**

## Typické otázky u zkoušky

- Co je **BaaS**?
- Jaký je rozdíl mezi **Firebase** a **Supabase**?
- Kdy je vhodné použít BaaS místo vlastního backendu?
- Jak BaaS řeší autentizaci?
- Jaká rizika přináší vendor lock-in?

## Shrnutí

**Backend as a Service** umožňuje rychle postavit backendové funkce bez vývoje celé serverové infrastruktury. **Firebase** nabízí silný uzavřenější ekosystém s výbornou podporou pro mobilní aplikace, zatímco **Supabase** staví na **PostgreSQL** a open-source přístupu. Volba mezi nimi závisí na typu projektu, požadovaném datovém modelu, rychlosti vývoje a míře kontroly, kterou tým nad backendem potřebuje.