# Přístupnost mobilních aplikací - screen readery, sémantika widgetů, kontrastní poměry, WCAG standardy

## Charakter otázky

Tato otázka je převážně **teoretická**, ale má velmi silný praktický dopad. Ve specifikaci jsou zmíněny **kontrasty**, **velikosti textu** a pohled „na úrovni developera - knihovny“. Student by měl vysvětlit, co znamená **přístupnost** v mobilních aplikacích, proč je důležitá a jak ji vývojář reálně podporuje ve Flutteru.

## Co je přístupnost

**Přístupnost** neboli **accessibility** znamená, že aplikaci mohou používat i lidé s různými omezeními, například:

- zrakovým postižením
- sluchovým postižením
- motorickým omezením
- kognitivními obtížemi
- dočasným omezením, například používání jednou rukou nebo na ostrém slunci

Přístupnost není jen pro menšinu. Lepší čitelnost, větší ovládací plochy nebo srozumitelnější texty pomáhají téměř všem uživatelům.

## Proč je přístupnost důležitá

- zvyšuje použitelnost aplikace
- rozšiřuje cílovou skupinu
- může být právním nebo firemním požadavkem
- zlepšuje kvalitu UX obecně

Z pohledu vývojáře je důležité chápat, že přístupnost není doplněk „na konec“, ale součást návrhu a implementace.

## Screen readery

**Screen reader** je nástroj, který převádí obsah obrazovky do mluveného výstupu. Na mobilních platformách jde například o:

- **TalkBack** na Androidu
- **VoiceOver** na iOS

Tyto nástroje umožňují nevidomým nebo slabozrakým uživatelům procházet aplikaci pomocí hlasového výstupu a gest.

## Co screen reader potřebuje

Aby screen reader fungoval dobře, musí mít aplikace:

- správnou **sémantiku** prvků
- srozumitelné popisy
- logické pořadí navigace
- čitelné názvy akcí

Pokud vývojář vytvoří hezké UI bez sémantiky, screen reader může číst obsah špatně nebo vůbec.

## Sémantika widgetů ve Flutteru

Flutter obsahuje vrstvu **Semantics**, která pomáhá popsat význam prvků pro asistivní technologie.

Příklady:

- tlačítko musí být rozpoznatelné jako tlačítko
- ikonka bez textu potřebuje popis
- obrázek s významem potřebuje alternativní informaci

Ve Flutteru lze použít například widget **Semantics**.

```dart
Semantics(
  label: 'Otevřít nastavení',
  button: true,
  child: IconButton(
    onPressed: onPressed,
    icon: const Icon(Icons.settings),
  ),
)
```

## Význam sémantiky u běžných prvků

Mnoho standardních widgetů sémantiku už obsahuje, ale problémy vznikají například když:

- použijeme jen ikonku bez textu
- vytvoříme vlastní komplexní widget bez popisu
- vizuálně kombinujeme více prvků, ale logicky by měly tvořit jeden celek

Vývojář musí myslet na to, co screen reader skutečně oznámí uživateli.

## Kontrastní poměry

Specifikace výslovně zmiňuje **kontrasty**. To je velmi důležitý bod.

Kontrast řeší čitelnost textu a rozlišitelnost prvků vůči pozadí. Nedostatečný kontrast je problém například pro:

- slabozraké uživatele
- uživatele ve slunečním světle
- starší osoby

### Praktické zásady

- tmavý text na světlém pozadí nebo naopak
- nevyjadřovat informaci jen barvou
- testovat chybové a disabled stavy

## WCAG standardy

**WCAG** znamená **Web Content Accessibility Guidelines**. Přestože vznikly primárně pro web, jejich principy se používají i u mobilních aplikací.

Čtyři základní principy WCAG jsou:

- **Perceivable** neboli vnímatelné
- **Operable** neboli ovladatelné
- **Understandable** neboli srozumitelné
- **Robust** neboli robustní

### Perceivable

Obsah musí být vnímatelný. Například dostatečný kontrast a alternativní texty.

### Operable

Aplikace musí být ovladatelná, například i bez velmi přesných gest.

### Understandable

Obsah a chování musí být srozumitelné.

### Robust

Aplikace má být kompatibilní s asistivními technologiemi.

## Velikosti textu a škálování

Specifikace zmiňuje i **velikosti textu**. Mobilní OS umožňují uživatelům zvětšovat text. Aplikace by to měla respektovat.

Vývojář musí myslet na:

- dynamické škálování písma
- to, aby se text po zvětšení nerozpadl v layoutu
- dostatečné rozestupy

Pokud UI funguje jen při jedné pevné velikosti textu, není přístupné.

## Velikost klikacích ploch

Přístupnost není jen o screen readeru. Důležité jsou i dostatečně velké dotykové plochy. Malé ikony bez prostoru kolem nich jsou problém pro uživatele s horší jemnou motorikou.

## Formuláře a přístupnost

U formulářů je potřeba:

- mít jasné labely
- vysvětlit chybu srozumitelně
- nespoléhat jen na barvu jako indikaci chyby
- zachovat logické pořadí fokusů

## Animace a pohyb

Někteří uživatelé mohou mít problém s výrazným pohybem nebo animacemi. Je vhodné:

- nepřehánět animace
- respektovat systémové nastavení omezení pohybu, pokud to platforma umožňuje

## Flutter a knihovny z pohledu developera

Ve specifikaci je zmíněna i „úroveň developera - knihovny“. Ve Flutteru je přístupnost podporována jak jádrem frameworku, tak ekosystémem.

Vývojář typicky využívá:

- vestavěné **Semantics** API
- standardní Material nebo Cupertino widgety, které už mají část přístupnosti vyřešenou
- nástroje pro testování přístupnosti na platformě

Klíčové ale není slepě spoléhat na knihovny, nýbrž aplikaci reálně testovat.

## Testování přístupnosti

Přístupnost je potřeba ověřovat prakticky.

Testujeme například:

- průchod aplikací přes TalkBack nebo VoiceOver
- zvětšení textu v systémovém nastavení
- kontrast na různých displejích
- ovládání bez perfektní motoriky

> [OBRÁZEK: checklist přístupnosti pro screen reader, kontrast, velikost textu a ovládací prvky]

## Nejčastější chyby

- ikonky bez popisku
- text s nízkým kontrastem
- příliš malé klikací plochy
- informace sdělená jen barvou
- nefunkční layout při zvětšení textu
- vlastní custom widget bez sémantického popisu

## Přístupnost jako součást kvality

Kvalitní aplikace by měla přístupnost řešit průběžně:

- při návrhu designu
- při implementaci widgetů
- při testování
- při každé větší úpravě UI

Dodatečné opravování bývá dražší a méně kvalitní než přístupnost řešit od začátku.

## Související témata

- **UX/UI design**
- **struktura a widgety layoutu**
- **pokročilé widgety**
- **životní cyklus vývoje**, protože accessibility má být součást vývoje od začátku

## Typické otázky u zkoušky

- Co je **screen reader**?
- K čemu slouží **Semantics** ve Flutteru?
- Proč nestačí sdělovat informaci jen barvou?
- Co znamená **WCAG**?
- Jak ověřit, že je aplikace přístupná?

## Shrnutí

**Přístupnost** znamená, že mobilní aplikaci mohou používat i lidé se zrakovým, motorickým nebo jiným omezením. V praxi to zahrnuje správnou sémantiku widgetů, podporu pro **screen readery**, dostatečný **kontrast**, respekt k větším velikostem textu a logicky navržené ovládání. Ve Flutteru pomáhá například vrstva **Semantics**, ale rozhodující je, zda vývojář na přístupnost myslí už při návrhu a reálně ji testuje na zařízeních a s asistivními nástroji.