# Správa verzí (Git) a CI/CD ve Flutteru

## Charakter otázky

Tato otázka je převážně **praktická**, ale má i procesní rozměr. U zkoušky je potřeba umět vysvětlit, proč se používá **Git**, jak funguje týmová spolupráce nad zdrojovým kódem a jak **CI/CD** automatizuje testování, build a distribuci Flutter aplikací.

## Proč je správa verzí důležitá

Vývoj mobilní aplikace je týmová a dlouhodobá činnost. Bez správy verzí bychom měli problémy s:

- přepisováním cizí práce
- návratem ke starší verzi
- dohledáním, kdo změnu provedl a proč
- bezpečným nasazením nové verze

**Git** řeší historii změn a umožňuje paralelní práci více vývojářů.

## Základní principy Gitu

Git je **distribuovaný verzovací systém**. Každý vývojář má lokálně celou historii repozitáře.

Základní pojmy:

- **repository** neboli repozitář
- **commit** jako uložený logický krok změny
- **branch** jako větev vývoje
- **merge** pro spojení větví
- **rebase** pro přeuspořádání historie
- **remote** jako vzdálený repozitář, například GitHub nebo GitLab

## Běžný workflow ve vývoji

Typický týmový postup:

1. vývojář vytvoří novou větev pro feature nebo bugfix
2. provede změny a uloží je do commitů
3. odešle větev na remote
4. vytvoří pull request nebo merge request
5. proběhne code review
6. po schválení se změny spojí do hlavní větve

Tento proces pomáhá udržovat kvalitu a přehlednost.

## Commit a kvalitní commit message

Commit by měl být:

- logicky ucelený
- co nejmenší, ale smysluplný
- srozumitelně popsaný

Dobrá commit message vysvětluje, **co** se změnilo a ideálně i **proč**.

Špatné příklady:

- update
- fix
- changes

Lepší příklady:

- add product detail loading state
- fix crash when location permission is denied

## Branching strategie

Mezi časté přístupy patří:

- **main/master + feature branches**
- **Git Flow**
- **trunk-based development**

### Feature branches

Jednoduchý a častý přístup. Každá změna vzniká ve vlastní větvi.

### Git Flow

Formálnější model s větvemi jako:

- main
- develop
- feature
- release
- hotfix

Je přehledný, ale někdy zbytečně těžkopádný pro menší týmy.

### Trunk-based development

Preferuje malé, časté změny do hlavní větve. Je vhodný pro týmy s dobrou automatizací testů a CI/CD.

## Merge konflikty

Pokud dva lidé upraví stejný kus kódu, vznikne **merge conflict**. Je třeba:

- pochopit obě změny
- rozhodnout správnou výslednou podobu
- konflikt neřešit mechanicky bez porozumění

U zkoušky je dobré zmínit, že konflikty jsou běžná součást týmové práce a je potřeba je umět řešit disciplinovaně.

## Tagy a release management

Git umí vytvářet **tagy**, které se často používají pro označení releasů, například:

- `v1.0.0`
- `v1.2.3`

To je užitečné pro:

- dohledání přesné verze, která šla do produkce
- rollback
- propojení s CI/CD a release notes

## Co je CI

**CI** znamená **Continuous Integration**. Jde o průběžné ověřování změn v kódu.

Po pushi nebo pull requestu se automaticky spouští například:

- instalace závislostí
- statická analýza
- testy
- build aplikace

Smysl CI je odhalit problém co nejdříve.

## Co je CD

**CD** může znamenat:

- **Continuous Delivery**: aplikace je kdykoliv připravená k nasazení
- **Continuous Deployment**: změny se po splnění podmínek nasazují automaticky

V mobilním světě se často používá spíš delivery než plné deployment, protože store review bývá externí krok.

## CI/CD ve Flutteru

Flutter projekty v pipeline obvykle řeší:

- `flutter pub get`
- `flutter analyze`
- `flutter test`
- build Android nebo iOS artefaktů
- podpis a distribuci buildů

Pro iOS je často potřeba běh na macOS runneru.

## Typické CI/CD nástroje

- **GitHub Actions**
- **GitLab CI**
- **Bitrise**
- **Codemagic**
- **Jenkins**

Ve Flutter projektech jsou populární hlavně nástroje, které dobře podporují mobilní buildy a signing.

## Praktický pipeline scénář

Například:

1. vývojář otevře pull request
2. CI spustí analýzu a testy
3. po merge do main vznikne release build
4. build se podepíše
5. odešle se do interní distribuce nebo store konzole

To zkracuje manuální práci a snižuje riziko lidské chyby.

## Automatizace release procesů

CI/CD může řešit i:

- generování changelogu
- inkrementaci build number
- distribuci do TestFlight nebo Google Play internal testing
- nahrání symbolů pro crash reporting
- spouštění smoke testů

## Tajné údaje a bezpečnost v pipeline

CI/CD běžně pracuje s citlivými údaji:

- signing keys
- API tokeny
- service account přístupy
- certifikáty

Tyto údaje nesmí být natvrdo v repozitáři. Používají se:

- **secrets** v CI platformě
- bezpečné úložiště certifikátů
- oddělení přístupů podle prostředí

## Testy v CI

Automatizovaná pipeline dává největší smysl, pokud má projekt kvalitní testy:

- **unit testy**
- **widget testy**
- případně **integration testy**

Bez testů je CI často jen automatické buildování, ale ne skutečná kontrola kvality.

## Výhody CI/CD

- rychlejší zpětná vazba
- menší riziko regresí
- konzistentní build proces
- snížení manuálních chyb při release
- lepší auditovatelnost změn

## Nevýhody a limity

- počáteční investice do nastavení
- složitější signing a secrets management
- buildy mobilních aplikací mohou být pomalé
- bez kvalitního procesu v týmu samotná pipeline problémy nevyřeší

## Nejčastější chyby

- dlouhé větve, které se integrují pozdě
- nečitelná historie commitů
- chybějící code review
- tajné údaje uložené přímo v repozitáři
- release build dělaný ručně bez opakovatelného procesu

## Související témata

- **nasazení na Google Play a App Store**
- **monitoring a crash reporting**
- **životní cyklus vývoje aplikace**
- **projektové řízení**

## Typické otázky u zkoušky

- Co je rozdíl mezi **CI** a **CD**?
- Proč používat feature branches?
- Jaké kroky by měla mít Flutter pipeline?
- Jak bezpečně řešit signing klíče?
- Proč je code review důležité?

## Shrnutí

**Git** je základ správy zdrojového kódu a týmové spolupráce. **CI/CD** automatizuje ověřování, build a distribuci aplikace, čímž snižuje chybovost a zrychluje release proces. Ve Flutteru je důležité propojit verzování, testy, signing a distribuci do testovacích i produkčních kanálů. Dobře nastavený proces výrazně zvyšuje kvalitu i předvídatelnost vývoje.