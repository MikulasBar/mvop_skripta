# Druhy API

## Charakter otázky

Tato otázka je převážně **teoretická**, ale měla by být propojena s praxí mobilního vývoje. U zkoušky je vhodné vysvětlit, co je **API**, jaké existují hlavní typy a jak jejich volba ovlivňuje návrh mobilní aplikace, práci s daty, bezpečnost i výkon.

## Co je API

**API** znamená **Application Programming Interface**. Je to rozhraní, přes které spolu komunikují dvě části softwaru.

V mobilní aplikaci může API znamenat například:

- komunikaci s backendem
- přístup k cizí službě, například platební bráně
- rozhraní operačního systému pro kameru nebo polohu

V této otázce se nejčastěji myslí hlavně **webová API**, přes která mobilní aplikace komunikuje se serverem.

## Proč mobilní aplikace API potřebuje

Mobilní aplikace sama obvykle neobsahuje všechna data ani kompletní byznys logiku. Přes API získává nebo odesílá:

- uživatelské účty
- objednávky
- obsah aplikace
- stav plateb
- notifikace

API je tedy most mezi frontendem a backendem.

## REST API

Nejrozšířenější typ webového API je **REST**.

### Základní principy REST

- komunikace často probíhá přes **HTTP**
- data jsou typicky ve formátu **JSON**
- server vystavuje **resource** neboli zdroje
- používají se HTTP metody jako **GET**, **POST**, **PUT**, **PATCH**, **DELETE**

Příklad:

- `GET /products`
- `GET /products/42`
- `POST /orders`

### Výhody REST

- jednoduchost
- široká podpora
- snadná čitelnost a debugování

### Nevýhody REST

- někdy nadbytečné množství dat
- více endpointů pro složitější obrazovky
- riziko overfetchingu a underfetchingu

## GraphQL

**GraphQL** je alternativní přístup, kde klient posílá dotaz a přesně říká, jaká data chce.

Příklad výhody:

- detail produktu může získat produkt, recenze i autora v jednom dotazu
- klient dostane jen pole, která opravdu potřebuje

### Výhody GraphQL

- flexibilita dotazů
- menší riziko overfetchingu
- vhodné pro složitější datové závislosti

### Nevýhody GraphQL

- vyšší složitost backendu i klienta
- složitější cachování a monitoring
- pro jednoduché API může být zbytečně robustní

## RPC a gRPC

**RPC** znamená **Remote Procedure Call**. Klient volá vzdálenou proceduru podobně, jako by šlo o lokální funkci.

Moderní varianta je **gRPC**:

- často používá **Protocol Buffers**
- je velmi efektivní
- hodí se pro mikroservisní architektury a výkonnou komunikaci

V mobilních aplikacích se používá méně často než REST, ale v některých systémech dává smysl.

## SOAP

**SOAP** je starší protokol založený často na XML a formálních kontraktech. Dnes je v mobilním vývoji méně běžný, ale může se objevit ve firemních nebo legacy systémech.

Výhody:

- formálnost
- standardizace

Nevýhody:

- větší složitost
- objemnější komunikace
- méně pohodlné použití v moderních mobilních aplikacích

## Public API, private API a partner API

API lze dělit i podle toho, komu je určeno.

### Public API

Je veřejně dostupné pro externí vývojáře.

### Private API

Používá se jen interně v rámci jedné organizace nebo systému.

### Partner API

Je určeno konkrétním obchodním partnerům a má omezený přístup.

## Synchronicita komunikace

API může být:

- **synchronní** z pohledu request-response modelu
- **asynchronní**, například přes webhooky nebo messaging

V mobilní aplikaci se běžně volá request-response API, ale backend může některé procesy zpracovávat asynchronně.

## Realtime API a streaming

Některé aplikace potřebují data v reálném čase. Pak se používají například:

- **WebSocket**
- server-sent events
- realtime databázové služby

Použití:

- chat
- live tracking
- multiplayer nebo kolaborativní aplikace

## Autentizace a autorizace API

API téměř vždy řeší bezpečnost. Klient se často autentizuje pomocí:

- **JWT tokenu**
- session
- OAuth 2.0
- API key, spíše pro server-to-server nebo omezené scénáře

Je potřeba rozlišovat:

- **autentizaci**, tedy kdo uživatel je
- **autorizaci**, tedy co smí dělat

## Verzionování API

Aby změny nerozbily starší klienty, používá se verzování. Například:

- `/api/v1/products`
- `/api/v2/products`

Mobilní aplikace má navíc specifikum: uživatelé neaktualizují okamžitě. Backend musí často podporovat starší verze klienta delší dobu.

## Chybové stavy API

Mobilní vývojář musí počítat s tím, že API může vrátit:

- 401 Unauthorized
- 403 Forbidden
- 404 Not Found
- 500 Internal Server Error
- timeout nebo výpadek sítě

Proto je práce s API úzce spojená s asynchronitou, retry logikou a správou chyb v UI.

## Výběr typu API podle situace

### REST

Vhodný pro:

- běžné CRUD aplikace
- jednoduché a středně složité backendy
- širokou kompatibilitu

### GraphQL

Vhodný pro:

- komplexní datové struktury
- klienty s rozdílnými datovými potřebami
- aplikace s mnoha propojenými entitami

### gRPC

Vhodný pro:

- vysoký výkon
- interní systémy
- mikroservisy a binární komunikaci

### SOAP

Používá se hlavně tam, kde existují starší enterprise systémy.

## API z pohledu Flutter klienta

Flutter aplikace obvykle řeší:

- HTTP klienta
- serializaci JSON
- modely a DTO objekty
- repository vrstvu
- autentizační tokeny
- error handling
- případně cache a offline režim

To ukazuje, že API není izolované téma, ale zasahuje do velké části architektury aplikace.

## Nejčastější chyby

- předpoklad, že síť vždy funguje
- špatné zpracování chybových kódů
- neřešení verzování API
- ukládání tokenů nebezpečným způsobem
- těsné svázání UI s konkrétním formátem response

## Související témata

- **asynchronní operace**
- **databáze a cache**
- **BaaS služby**
- **state management**
- **bezpečnost a nasazení**

## Typické otázky u zkoušky

- Jaký je rozdíl mezi **REST** a **GraphQL**?
- Co je **gRPC**?
- Proč se verzují API?
- Jaké jsou běžné HTTP metody?
- Jak mobilní aplikace řeší nefunkční API?

## Shrnutí

**API** je rozhraní, přes které mobilní aplikace komunikuje s backendem nebo jinou službou. Nejčastější typy jsou **REST**, **GraphQL**, **gRPC** a méně často **SOAP**. Každý přístup má jiné výhody a nevýhody z hlediska flexibility, výkonu a složitosti. Ve Flutter vývoji je důležité nejen API zavolat, ale i správně řešit autentizaci, chyby, verzování a případně caching nebo realtime komunikaci.