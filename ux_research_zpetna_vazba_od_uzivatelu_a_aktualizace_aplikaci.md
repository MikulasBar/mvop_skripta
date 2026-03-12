# UX research, zpětná vazba od uživatelů a aktualizace aplikací

## Charakter otázky

Tato otázka je převážně **teoretická**, ale měla by být doplněna praktickými příklady z vývoje aplikace. Ve specifikaci jsou zmíněna témata **před samotným vývojem**, **monitoring použití** a **Crashlytics**, takže je potřeba pokrýt celý cyklus: od zjišťování potřeb uživatelů až po sledování aplikace po vydání.

## Proč nestačí aplikaci jen naprogramovat

Technicky funkční aplikace ještě nemusí řešit skutečný problém uživatele. Pokud neznáme uživatele, jejich cíle a bariéry, můžeme vyvinout něco, co:

- nikdo nepotřebuje
- je složité na použití
- má špatně zvolené funkce
- neodpovídá očekáváním cílové skupiny

Proto je důležité řešit **UX research** a sběr zpětné vazby průběžně, nejen na konci.

## Fáze před samotným vývojem

Specifikace výslovně zmiňuje období před vývojem. To je velmi důležitá fáze.

### Zjištění problému a cílové skupiny

Nejdřív je potřeba pochopit:

- kdo je uživatel
- jaký problém řeší
- jak ho řeší dnes
- co ho frustruje
- co je pro něj skutečně hodnotné

### Metody výzkumu před vývojem

- rozhovory s uživateli
- dotazníky
- pozorování uživatelů
- analýza konkurence
- persony
- mapování user journey

### Výstupy této fáze

- definice cílové skupiny
- seznam hlavních use-case scénářů
- MVP scope
- hypotézy, které chceme ověřit

## MVP a ověřování hypotéz

**MVP** neboli minimum viable product znamená nejmenší verzi produktu, která je schopná ověřit, zda řešení dává smysl.

Smyslem není dodat polovičatou aplikaci, ale rychle ověřit:

- zda uživatelé funkci opravdu potřebují
- zda rozumí navrženému flow
- zda je produkt ochotně používán

## Prototypy a usability testing

Ještě před vývojem plné aplikace lze testovat:

- wireframy
- klikatelné prototypy ve Figma
- jednoduché demo scénáře

Při **usability testu** dostane uživatel úkol, například:

- zaregistrovat se
- objednat službu
- najít konkrétní informaci

Sleduje se:

- jestli úkol dokončí
- kde se zasekne
- čemu nerozumí
- co očekával jinak

## Zpětná vazba po vydání

Po vydání aplikace výzkum nekončí. Naopak začíná fáze reálného používání.

Zdroje zpětné vazby:

- recenze ve store
- support a zákaznická podpora
- uživatelské rozhovory
- interní testovací skupiny
- analytická data
- crash reporting

## Monitoring použití aplikace

Specifikace výslovně uvádí **monitoring použití**. To znamená sledování toho, jak lidé aplikaci skutečně používají.

Typicky se měří:

- kolik uživatelů otevře konkrétní obrazovku
- kde odcházejí z flow
- jak často používají konkrétní funkci
- jaká je retence
- kolik lidí dokončí registraci nebo nákup

To pomáhá odhalit rozdíl mezi tím, co si tým myslí, a tím, co se skutečně děje.

## Produktové metriky

U mobilní aplikace se často sledují:

- **DAU/MAU**
- retention
- churn
- konverzní poměr
- délka session
- počet dokončených klíčových akcí

Výběr metrik závisí na typu aplikace. U e-commerce se sleduje konverze a objednávky, u sociální aplikace engagement, u utilitky třeba frekvence používání a návratnost.

## Crashlytics a technický monitoring

Ve specifikaci je zmíněn **Crashlytics**, což je důležitý bod.

**Firebase Crashlytics** slouží k monitoringu pádů a technických chyb aplikace. Umožňuje sledovat:

- počet crashů
- zařízení a OS verze, kde k pádu došlo
- stack trace
- četnost výskytu
- dopad na uživatele

To je zásadní po release, protože tým rychle zjistí, zda nová verze nezpůsobila kritický problém.

## Rozdíl mezi UX a technickými daty

Je důležité kombinovat dva pohledy:

- **kvantitativní data**, například analytika a crash reporting
- **kvalitativní data**, například rozhovory, recenze a uživatelské testy

Analytika ukáže **co** se děje. Rozhovory často vysvětlí **proč** se to děje.

## Aktualizace aplikace

Na základě výzkumu a dat se aplikace průběžně aktualizuje. Aktualizace mohou řešit:

- opravy chyb
- zlepšení UX flow
- nové funkce
- výkonnostní optimalizace
- bezpečnostní změny

Je důležité, aby změny nebyly náhodné, ale vycházely z priorit a dat.

## Prioritizace změn

Po vydání obvykle existuje více nápadů a problémů, než stihne tým řešit. Proto se prioritizuje například podle:

- dopadu na uživatele
- technické závažnosti
- obchodní hodnoty
- náročnosti implementace

Typický příklad:

- crash při registraci má vyšší prioritu než kosmetická změna ikonky

## Release cyklus a zpětná vazba

Kvalitní tým pracuje v iteracích:

1. navrhne hypotézu nebo změnu
2. vydá aktualizaci
3. sleduje data a reakce
4. vyhodnotí dopad
5. rozhodne o dalším kroku

To je základ produktového vývoje.

> [OBRÁZEK: cyklus research -> návrh -> vývoj -> release -> měření -> další iterace]

## Store reviews jako zdroj dat

Recenze v Google Play a App Store poskytují cennou zpětnou vazbu, ale mají limity:

- bývají emotivní
- často popisují jen extrémy
- někdy nejsou dost konkrétní

Přesto mohou rychle odhalit opakující se problém, například pády po poslední aktualizaci.

## Etika a práce s daty

Při sběru analytiky je potřeba myslet na:

- ochranu soukromí
- minimalizaci sbíraných dat
- souhlas uživatele tam, kde je potřeba
- transparentnost

Výzkum a analytika nesmí být v rozporu s právními a etickými pravidly.

## Nejčastější chyby

- vývoj bez předběžného ověření potřeb uživatelů
- rozhodování jen podle názoru týmu bez dat
- ignorování store recenzí a support požadavků
- absence crash monitoringu
- vydávání aktualizací bez vyhodnocení jejich dopadu

## Související témata

- **UX/UI design**
- **životní cyklus vývoje mobilní aplikace**
- **Git a CI/CD**
- **monetizace**, protože změny UX ovlivňují konverzi

## Typické otázky u zkoušky

- Co je **UX research**?
- Jaké metody použít před vývojem?
- K čemu slouží **Crashlytics**?
- Jak sledovat, zda uživatelé funkci opravdu používají?
- Proč je důležité kombinovat analytiku a rozhovory?

## Shrnutí

**UX research** a zpětná vazba od uživatelů jsou klíčové před vývojem i po vydání aplikace. Před vývojem pomáhají pochopit problém a správně navrhnout MVP. Po vydání je nutné sledovat používání aplikace, recenze, support požadavky i technické chyby pomocí nástrojů jako **Crashlytics**. Kvalitní aplikace nevzniká jedním releasem, ale průběžnými iteracemi založenými na datech a skutečných potřebách uživatelů.