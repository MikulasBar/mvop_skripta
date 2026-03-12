# Práce s databázemi a úložišti

## Charakter otázky

Tato otázka je **praktická**. U zkoušky je potřeba vysvětlit, jak mobilní aplikace ukládá data lokálně, kdy použít jednoduché klíč-hodnota úložiště a kdy už je potřeba databáze. Ve specifikaci jsou výslovně zmíněny **shared prefs** a **local SQL**, proto je nutné je pokrýt detailněji.

## Proč mobilní aplikace potřebuje lokální úložiště

Lokální úložiště se používá například pro:

- uchování nastavení uživatele
- zapamatování přihlášení nebo tokenu
- cache dat z API
- offline režim
- ukládání konceptů, formulářů nebo historie

Bez lokální persistence by aplikace po zavření často ztratila důležitý kontext.

## Typy lokálního úložiště ve Flutteru

Nejčastější přístupy:

- **shared preferences** pro jednoduchá data typu klíč-hodnota
- **SQL databáze** pro strukturovaná data a relace
- **NoSQL lokální databáze** jako Hive nebo Isar
- soubory v interním úložišti
- secure storage pro citlivá data

V této otázce se zaměříme hlavně na první dva body.

## Shared Preferences

**Shared Preferences** jsou jednoduché perzistentní úložiště ve stylu **key-value**. Ve Flutteru se používá balíček jako `shared_preferences`.

Hodí se pro malá a jednoduchá data:

- boolean přepínače
- jazyk aplikace
- theme mode
- poslední otevřená karta
- jednoduchý token nebo flag, že uživatel prošel onboardingem

### Jaká data se běžně ukládají

Podporované bývají primitivní typy:

- **String**
- **int**
- **double**
- **bool**
- seznam stringů

### Příklad použití

```dart
final prefs = await SharedPreferences.getInstance();
await prefs.setBool('is_dark_mode', true);

final isDarkMode = prefs.getBool('is_dark_mode') ?? false;
```

### Výhody Shared Preferences

- jednoduché API
- rychlá implementace
- vhodné pro drobná nastavení

### Nevýhody Shared Preferences

- nevhodné pro složitější datové modely
- bez relací a dotazování
- nehodí se na velké objemy dat
- není to správné místo pro citlivá data bez další ochrany

## Secure Storage

I když není explicitně ve specifikaci, je dobré ho zmínit. **Secure Storage** se používá pro citlivé informace, například:

- access token
- refresh token
- tajné klíče nebo citlivé identifikátory

Na Androidu a iOS bývá navázaný na systémové bezpečné úložiště. U zkoušky je dobré říct, že citlivé údaje není vhodné ukládat do běžných shared preferences bez ochrany.

## Lokální SQL databáze

Pokud aplikace pracuje se strukturovanými daty, více tabulkami nebo potřebuje filtrovat, řadit a dotazovat se nad větším množstvím dat, používá se lokální databáze typu **SQLite**.

Ve Flutteru se často používá:

- **sqflite**
- někdy vyšší abstrahující vrstva jako **Drift**

## Co je SQL databáze

**SQL databáze** ukládá data do tabulek. Podporuje:

- sloupce a datové typy
- primární klíče
- relace mezi tabulkami
- dotazy přes SQL jazyk
- filtrování, řazení a agregaci

Příklad tabulky úkolů:

| id | title | is_done | created_at |
| --- | --- | --- | --- |
| 1 | Nakoupit | 0 | 2026-03-10 |
| 2 | Zavolat | 1 | 2026-03-10 |

## Základní CRUD operace

V databázích se často mluví o **CRUD**:

- **Create**
- **Read**
- **Update**
- **Delete**

To jsou základní operace, které aplikace nad daty potřebuje.

## Příklad práce se SQLite

```dart
final database = await openDatabase(
  'app.db',
  version: 1,
  onCreate: (db, version) async {
    await db.execute('''
      CREATE TABLE tasks(
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        title TEXT NOT NULL,
        is_done INTEGER NOT NULL
      )
    ''');
  },
);
```

Insert:

```dart
await database.insert('tasks', {
  'title': 'Nakoupit',
  'is_done': 0,
});
```

Query:

```dart
final result = await database.query('tasks');
```

## Migrace databáze

Jakmile aplikace roste, struktura databáze se mění. Proto je potřeba řešit **migrace**.

Například:

- přidání nového sloupce
- rozdělení jedné tabulky do více tabulek
- změna indexů

Migrace musí být navržené tak, aby nepřišla o data uživatele při aktualizaci aplikace.

## Repository vrstva nad databází

Stejně jako u API není vhodné, aby UI komunikovalo přímo s databází. Lepší je použít **repository pattern**.

Například:

- `TaskLocalDataSource` pracuje se SQLite
- `TaskRepository` rozhoduje, odkud data číst
- UI nebo Bloc/Provider pracuje jen s repository

Tím se zlepší testovatelnost a oddělení odpovědností.

## Cache a offline-first přístup

Lokální databáze se často nepoužívá jen pro čistě offline aplikaci, ale i jako **cache** pro data ze serveru.

Scénář:

1. aplikace stáhne data z API
2. uloží je lokálně
3. při dalším spuštění zobrazí cache okamžitě
4. na pozadí provede synchronizaci

To vede k rychlejšímu načítání a lepšímu UX.

## Kdy použít Shared Preferences a kdy SQL

### Shared Preferences

Použijeme když:

- ukládáme jednoduchou konfiguraci
- jde o malé množství dat
- nepotřebujeme složité dotazy

### SQL databáze

Použijeme když:

- pracujeme s větším množstvím záznamů
- potřebujeme vztahy mezi entitami
- chceme filtrovat, řadit a vyhledávat
- potřebujeme robustnější offline cache

## Nejčastější chyby

- ukládání složitých objektů do shared preferences bez rozmyslu
- neřešení verzování databáze
- databázové volání přímo z widgetu
- neoddělení datové vrstvy od UI
- ukládání citlivých údajů do nechráněného úložiště

## Výkon a konzistence dat

U lokální persistence je potřeba myslet na:

- asynchronní přístup k datům
- případné konflikty při synchronizaci
- konzistenci při zápisu více kroků
- výkon při větším objemu dat

V praxi je důležité navrhnout si dopředu, která data jsou:

- dočasná cache
- dlouhodobá uživatelská data
- citlivé údaje

## Související témata

- **asynchronní operace**
- **druhy API**
- **BaaS služby**
- **state management**
- **životní cyklus aplikace a offline režim**

## Typické otázky u zkoušky

- Kdy použít **shared preferences** a kdy **SQLite**?
- Jak řešit citlivá data?
- Co je **CRUD**?
- Proč používat repository vrstvu?
- Jak řešit migrace databáze?

## Shrnutí

Lokální úložiště je zásadní pro výkon, offline režim i uchování uživatelského nastavení. **Shared Preferences** jsou vhodné pro jednoduchá data typu klíč-hodnota, zatímco **SQL databáze** jako SQLite jsou vhodné pro strukturovaná data, větší objem záznamů a složitější dotazy. Kvalitní návrh úložiště vždy zahrnuje oddělení datové vrstvy, řešení asynchronity, bezpečnosti a případně i synchronizace se serverem.