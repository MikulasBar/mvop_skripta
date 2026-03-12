# Životní cyklus vývoje mobilní aplikace

## Charakter otázky

Tato otázka je převážně **teoretická**, ale velmi úzce souvisí s praxí. Ve specifikaci je zmíněno i „když chci vytvořit aplikaci, co je potřeba vyřešit“, takže je nutné odpovědět systematicky od prvotního nápadu až po údržbu a další rozvoj.

## Co je životní cyklus vývoje aplikace

**Životní cyklus vývoje mobilní aplikace** popisuje jednotlivé fáze, kterými projekt prochází:

1. nápad a definice problému
2. analýza a validace
3. návrh řešení
4. implementace
5. testování
6. nasazení
7. provoz, monitoring a iterace

Je důležité chápat, že to není jen lineární jednorázový proces. Po vydání aplikace často začíná další iterace.

## 1. Nápad a definice problému

První otázka není „jakou technologii použijeme“, ale:

- jaký problém aplikace řeší
- pro koho je určená
- jaká je její hodnota pro uživatele
- jak se odliší od konkurence

Pokud toto není jasné, technická implementace sama o sobě nestačí.

## 2. Co je potřeba vyřešit, když chci vytvořit aplikaci

Tohle je důležitá praktická část zadání.

Je potřeba vyřešit minimálně:

- cílovou skupinu
- hlavní use-case scénáře
- business model nebo účel aplikace
- platformy, pro které bude aplikace určena
- rozsah MVP
- právní a bezpečnostní požadavky
- kdo bude aplikaci vyvíjet, testovat a provozovat
- rozpočet a časový rámec

Technologická volba přichází až poté, co rozumíme produktu.

## 3. Analýza a validace

V této fázi se ověřuje, zda má řešení smysl.

Používají se například:

- rozhovory s uživateli
- analýza konkurence
- wireframy
- prototypy
- odhad technické náročnosti

Výstupem bývá:

- seznam požadavků
- prioritizace funkcí
- návrh MVP
- identifikace hlavních rizik

## 4. Návrh aplikace

Tato fáze zahrnuje více vrstev.

### Produktový návrh

- user flow
- prioritizace funkcí
- navigační struktura

### UX/UI návrh

- wireframy
- vizuální styl
- design systém

### Technický návrh

- architektura aplikace
- volba frameworku, například Flutter
- backend a API strategie
- state management
- databáze a storage

> [OBRÁZEK: diagram životního cyklu od nápadu přes návrh a vývoj až po monitoring a iterace]

## 5. Implementace

V implementační fázi tým převádí návrh do reálné aplikace. Typicky řeší:

- strukturu projektu
- obrazovky a navigaci
- state management
- integraci API
- lokální úložiště
- autentizaci
- notifikace
- build konfiguraci

Zde je důležitá i práce s Gitem, code review a průběžná integrace.

## 6. Testování

Bez testování není aplikace připravená pro reálné uživatele.

### Typy testování

- **unit testy**
- **widget testy**
- **integration testy**
- manuální testování
- testování na reálných zařízeních

### Co testovat

- hlavní uživatelské scénáře
- chybové stavy
- výkon
- offline režim
- různá oprávnění a edge cases

## 7. Nasazení

Před release je potřeba řešit:

- signing
- build čísla a verze
- store metadata
- privacy policy
- crash reporting
- interní test distribuci

Nasazení není konec práce, ale začátek provozní fáze.

## 8. Provoz a monitoring

Po vydání aplikace sledujeme:

- pády aplikace
- výkon
- chování uživatelů
- recenze ve store
- technické a produktové metriky

Používají se nástroje jako:

- analytics
- Crashlytics
- logging
- customer support zpětná vazba

## 9. Iterace a další rozvoj

Na základě dat a zpětné vazby se produkt dále rozvíjí. Může jít o:

- opravy bugů
- zlepšení UX
- nové funkce
- optimalizaci výkonu
- monetizační experimenty

Tím se životní cyklus opakuje v další iteraci.

## Role různých profesí

Na vývoji mobilní aplikace se obvykle podílí více rolí:

- product owner nebo zákazník
- UX/UI designer
- mobilní vývojář
- backend vývojář
- QA tester
- DevOps nebo release odpovědnost

I v malém týmu může jednu roli zastávat jeden člověk, ale odpovědnosti je dobré znát.

## Rizika v životním cyklu

- nerealistický rozsah MVP
- nejasné zadání
- podcenění testování
- ignorování zpětné vazby po vydání
- technický dluh způsobený spěchem

## Dokumentace a znalostní kontinuita

Součástí kvalitního vývoje je i dokumentace:

- architektonická rozhodnutí
- release postup
- nastavení prostředí
- popis API integrací

To je důležité pro udržitelnost projektu.

## Proč je životní cyklus iterativní

Trh, uživatelé i technologie se mění. Mobilní aplikace proto není jednorázový projekt, ale živý produkt. Každá aktualizace přináší nové poznatky a mění priority.

## Související témata

- **projektové řízení**
- **UX research**
- **Git a CI/CD**
- **nasazení aplikace**
- **monetizace**

## Typické otázky u zkoušky

- Jaké jsou hlavní fáze vývoje mobilní aplikace?
- Co je potřeba vyřešit před samotným programováním?
- Proč nestačí aplikaci jen vydat do store?
- Jakou roli hraje monitoring po release?
- Proč je důležité definovat MVP?

## Shrnutí

Životní cyklus vývoje mobilní aplikace začíná definicí problému a pokračuje přes validaci, návrh, implementaci, testování, nasazení a provoz. Po vydání aplikace přichází monitoring, sběr zpětné vazby a další iterace. Kvalitní produkt nevzniká jen programováním, ale propojením produktového myšlení, designu, techniky, testování a průběžného zlepšování.