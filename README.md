# Ciasteczkowy wytrych: dane / The Cookie Loophole: data

**[English version below](#english)**

Dane do raportu „Ciasteczkowy wytrych. Co naprawdę wyłącza przycisk „nie wyrażam zgody” na 50 dużych serwisach internetowych” (Marcin Rybak, HeadFlash.news, październik 2026).

Raport: https://headflash.news/pl/raporty/ciasteczkowy-wytrych/

## Co tu jest

Ścieżki są takie same jak w przypisach raportu.

| Ścieżka | Co zawiera |
|---|---|
| `results/merged.csv` | jeden wiersz na serwis: wszystkie liczby z raportu |
| `results/summary.json` | mediany i maksima w pięciu grupach serwisów |
| `results/domain-categories.csv` | domeny z ruchu sieciowego z moim podziałem na kategorie i firmy |
| `data/runs/<serwis>/human/desktop/result.yaml` | zapis przejścia przez przeglądarkę (2 i 3 września 2026, 42 serwisy) |
| `data/runs/<serwis>/human/desktop/*.png` | zrzuty z tego przejścia |
| `data/runs/<serwis>/auto/desktop/<przebieg>/result.json` | zapis licznika automatycznego (4 września 2026, 50 serwisów, po trzy przebiegi) |
| `data/runs/<serwis>/auto/desktop/<przebieg>/*.png` | zrzuty licznika |
| `data/runs/onet/auto/desktop/<przebieg>/refuse_full_tc-request.txt` | zapytanie do serwera reklam Google po pełnej odmowie, z ciągiem TC |
| `data/runs/mbank/.../dom_layer2.html`, `data/runs/youtube/.../dom_layer2.html` | kod drugiej warstwy banera, z którego cytuję zdania niewidoczne na zrzutach |
| `data/sites.yaml` | lista serwisów z adresami i grupami |
| `data/discrepancies.md` | rozbieżności między przejściem a licznikiem i między przebiegami |

## Jak powstały

Dwa narzędzia, polskie łącze domowe (polski dostawca internetu, nie centrum danych).

1. Przejście przez przeglądarkę: Chrome 152 na Windows 11, język polski, bez blokerów, sterowany agentem AI (Claude in Chrome) pod moim nadzorem. Każdy serwis od zera: akceptacja, pełna odmowa, lista partnerów, wejście do zmiany decyzji. Stan po decyzji odczytany z interfejsu IAB TCF (`__tcfapi`), a na serwisach bez TCF z ciasteczek zgody.
2. Licznik automatyczny: Chromium w kontenerze, trzy przebiegi na serwis. Stany: przed kliknięciem (`pre`), odmowa z pierwszego ekranu (`reject`), pełna odmowa z uzasadnionym interesem (`refuse_full`), akceptacja (`accept`). W każdym stanie zapis TCF (`m3`), liczba ciasteczek i lista domen firm trzecich (`m6`).

W `merged.csv` przejście przez przeglądarkę ma pierwszeństwo dla M1, M2, M5, M7 i M9, a licznik dla M3, M4, M6 i M8 (mediana z trzech przebiegów). Kolumny `source_m1` i `source_m2` mówią, skąd jest wartość.

## Słownik kolumn `merged.csv`

| Kolumna | Znaczenie |
|---|---|
| `group` | `pl` Polska, `pl_daily` Polska, codzienne usługi, `nonEU` giganci spoza UE, `eu` Europa, `inst` instytucje |
| `m1` | odmowa jednym przyciskiem na pierwszym ekranie banera |
| `m2_accept`, `m2_refuse` | kliknięcia do akceptacji i do pełnej odmowy |
| `m2_scrolls` | przewinięcia ekranu na ścieżce odmowy |
| `m2_refuse_full` | kliknięcia licznika do pełnej odmowy |
| `m3_vendors_consent`, `m3_vendors_li` | firmy w zapisie TCF po akceptacji: ze zgodą, z uzasadnionym interesem |
| `m3_vendors_li_reject`, `m3_vendors_li_refuse_full` | firmy z uzasadnionym interesem po odmowie z pierwszego ekranu i po pełnej odmowie |
| `m3_purposes_li_reject` | cele na uzasadnionym interesie po odmowie z pierwszego ekranu |
| `m4_screens`, `m4_minutes` | długość listy partnerów: ekrany (wysokość listy podzielona przez wysokość okna, stąd ułamki) i minuty czytania (200 słów na minutę) |
| `m5_default_on` | przełączniki włączone domyślnie na drugim ekranie |
| `m6_cookies_third_accept`, `m6_cookies_third_reject` | ciasteczka firm trzecich po akceptacji i po odmowie |
| `m6_hosts_third_accept` | domeny firm trzecich po akceptacji |
| `m7_wall` | ściana „zgoda albo płatność” |
| `m8_cmp` | platforma zgody; `TCF: <firma>` to platforma zgłoszona w IAB, `none` to brak TCF i żadnej z rozpoznawanych platform (serwis ma własne okno albo nie ma banera) |
| `m9_withdraw` | widoczne wejście do zmiany decyzji po akceptacji |
| `variance_flag` | przebiegi licznika różnią się w M1 lub M2 |
| `no_banner` | licznik nie zobaczył banera |
| `banner_delay_s` | sekundy do pojawienia się banera |

## Jak czytać notatki z przejścia

Pole `observations` w `result.yaml` to robocze notatki agenta AI, pisane na bieżąco w trakcie przejścia, bez polskich znaków. Publikuję je bez redakcji. Usunąłem tylko dane mojego komputera i łącza oraz identyfikator, który jeden serwis pokazał w panelu zgód. Dlatego w notatkach są:

- znaczniki pracy („STATUS”, „do uzupelnienia w przebiegu 2”, „krok C”), także tam, gdzie krok później zrobiono;
- dopiski z drugiego przebiegu, miejscami urwane;
- opisy kłopotów z narzędziem (nieudane kliknięcia, zrzuty po przeładowaniu strony);
- porównania i określenia typu „jedyny”, „w całym badaniu”, pisane wobec serwisów sprawdzonych do tamtej chwili;
- hipotezy agenta, które później sprawdzałem albo odrzucałem.

Wszystkie 26 adresów z pierwszego dnia otwarto raz przed przejściem, bez klikania w baner, żeby rozszerzenie dostało dostęp do domen. Między przebiegami dane przeglądarki czyściłem ręcznie. Liczby z zapisu TCF (ile firm ma zgodę albo uzasadniony interes) i z ruchu sieciowego pochodzą w raporcie z licznika automatycznego, który za każdym razem startował od czystej przeglądarki. Kliknięcia i wejście do zmiany decyzji pochodzą z przejścia, a ich rozbieżności z licznikiem opisuje `data/discrepancies.md`.

Agent pomylił się na Biedronce (nie zauważył ikony zgód) i na rp.pl (nie znalazł przycisku odmowy za pierwszym razem). Oba przypadki opisuję w rozdziale o metodzie. Pola liczone różnie w różnych serwisach: `m5_toggles_default_on` (czasem z zablokowanym przełącznikiem „niezbędne”, czasem bez) i `m4_partner_count_shown` (czasem tylko partnerzy IAB, czasem suma). Rozstrzygają tekst raportu i `results/merged.csv`, z jednym wyjątkiem: dla Biedronki `merged.csv` ma `m9_withdraw` = false z zapisu przejścia, a raport liczy ją jako serwis z wejściem (ikona widoczna na zrzutach licznika).

## Czego tu nie ma i dlaczego

- Pełnych zapisów ruchu sieciowego (HAR) i zrzutów ciasteczek. Zawierają adres IP łącza, lokalizację wyliczoną z IP i identyfikatory sesji. Liczby z nich są w `result.json` (pole `m6`).
- Kodu banerów poza dwoma plikami wyżej. Cytaty z banerów widać na zrzutach.
- Adresu IP, nazwy dostawcy łącza i danych komputera. Usunąłem je z `result.yaml` i `result.json`. Reszta zapisów jest bez zmian, razem z notatkami z przejścia (pole `observations`).
- Skryptów.

## Ważne zastrzeżenia

- Zapis TCF to deklaracja, a nie dowód śledzenia.
- Zapytanie do domeny to nie dowód przetwarzania danych osobowych.
- Podział domen na kategorie jest mój. Jest publiczny, żeby dało się z nim nie zgodzić.
- Stan z 2 do 4 września 2026. Serwisy mogły się od tego czasu zmienić.

## Licencja

Wyniki i zapisy: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/deed.pl), podaj autora i link do raportu. Zrzuty ekranu i cytaty z banerów należą do serwisów i są przytoczone w celu udokumentowania pomiaru. Nazwy i cele firm w raporcie pochodzą z rejestru IAB Global Vendor List, wersja 174: https://vendor-list.consensu.org/v3/archives/vendor-list-v174.json

---

## English

Data behind the report "The Cookie Loophole. What the "reject" button actually switches off on 50 large websites" (Marcin Rybak, HeadFlash.news, October 2026). The full report is in Polish.

Report page: https://headflash.news/reports/the-cookie-loophole/

### What is here

Paths match the report's footnotes.

| Path | Contents |
|---|---|
| `results/merged.csv` | one row per site: every number used in the report |
| `results/summary.json` | medians and maxima in five groups of sites |
| `results/domain-categories.csv` | domains seen in network traffic, with my categories and companies |
| `data/runs/<site>/human/desktop/result.yaml` | browser pass record (2 and 3 September 2026, 42 sites) |
| `data/runs/<site>/human/desktop/*.png` | screenshots from that pass |
| `data/runs/<site>/auto/desktop/<run>/result.json` | automated counter record (4 September 2026, 50 sites, three runs each) |
| `data/runs/<site>/auto/desktop/<run>/*.png` | counter screenshots |
| `data/runs/onet/auto/desktop/<run>/refuse_full_tc-request.txt` | request to a Google ad server after full refusal, carrying the TC string |
| `data/runs/mbank/.../dom_layer2.html`, `data/runs/youtube/.../dom_layer2.html` | second banner layer markup, source of quotes that no screenshot shows |
| `data/sites.yaml` | list of sites with addresses and groups |
| `data/discrepancies.md` | differences between the browser pass and the counter, and between runs |

### How the data was collected

Two instruments, one Polish home connection (a Polish residential ISP, not a data centre).

1. Browser pass: Chrome 152 on Windows 11, Polish language, no blockers, driven by an AI agent (Claude in Chrome) under my supervision. Each site from scratch: accept, full refusal, partner list, entry point to change the decision. State after the decision read from the IAB TCF interface (`__tcfapi`), and on sites without TCF from consent cookies.
2. Automated counter: Chromium in a container, three runs per site. States: before any click (`pre`), refusal from the first screen (`reject`), full refusal including legitimate interest (`refuse_full`), accept (`accept`). In each state: the TCF record (`m3`), cookie counts and the list of third-party domains (`m6`).

In `merged.csv` the browser pass takes precedence for M1, M2, M5, M7 and M9, the counter for M3, M4, M6 and M8 (median of three runs). Columns `source_m1` and `source_m2` say where a value comes from.

### `merged.csv` columns

| Column | Meaning |
|---|---|
| `group` | `pl` Poland, `pl_daily` Poland, everyday services, `nonEU` non-EU giants, `eu` Europe, `inst` institutions |
| `m1` | one-button refusal on the first banner screen |
| `m2_accept`, `m2_refuse` | clicks to accept and to refuse fully |
| `m2_scrolls` | screen scrolls on the refusal path |
| `m2_refuse_full` | counter clicks to full refusal |
| `m3_vendors_consent`, `m3_vendors_li` | companies in the TCF record after accepting: with consent, with legitimate interest |
| `m3_vendors_li_reject`, `m3_vendors_li_refuse_full` | companies with legitimate interest after first-screen refusal and after full refusal |
| `m3_purposes_li_reject` | purposes on legitimate interest after first-screen refusal |
| `m4_screens`, `m4_minutes` | length of the partner list: screens (list height divided by window height, hence fractions) and reading minutes (200 words per minute) |
| `m5_default_on` | toggles switched on by default on the second screen |
| `m6_cookies_third_accept`, `m6_cookies_third_reject` | third-party cookies after accepting and after refusing |
| `m6_hosts_third_accept` | third-party domains after accepting |
| `m7_wall` | "consent or pay" wall |
| `m8_cmp` | consent platform; `TCF: <company>` is a platform registered with IAB, `none` means no TCF and none of the recognised platforms (the site has its own dialog or no banner) |
| `m9_withdraw` | visible entry point to change the decision after accepting |
| `variance_flag` | counter runs differ on M1 or M2 |
| `no_banner` | the counter saw no banner |
| `banner_delay_s` | seconds until the banner appeared |

### How to read the browser pass notes

The `observations` field in `result.yaml` holds the AI agent's working notes, written during the pass, in Polish without diacritics. I publish them unedited. I removed only details of my computer and connection, and one identifier a site showed in its consent panel. So the notes contain:

- work markers ("STATUS", "do uzupelnienia w przebiegu 2", "krok C"), also where the step was done later;
- additions from the second run, cut off in places;
- descriptions of tool trouble (failed clicks, screenshots after a page reload);
- comparisons and words like "the only one" or "in the whole study", written against the sites checked up to that moment;
- hypotheses of the agent that I later checked or dropped.

All 26 addresses of the first day were opened once before the pass, without touching the banner, so the browser extension could get access to the domains. I cleared browser data by hand between runs. The TCF numbers in the report (how many companies have consent or legitimate interest) and the traffic numbers come from the automated counter, which started from a clean browser every time. Clicks and the entry point to change the decision come from the browser pass; where they differ from the counter, `data/discrepancies.md` says so.

The agent got two things wrong: Biedronka (it missed the consent icon) and rp.pl (it did not find the refusal button the first time). The method chapter of the report describes both. Some fields were counted differently on different sites: `m5_toggles_default_on` (sometimes including the locked "necessary" toggle, sometimes not) and `m4_partner_count_shown` (sometimes IAB partners only, sometimes the total). The report text and `results/merged.csv` are authoritative, with one exception: for Biedronka `merged.csv` has `m9_withdraw` = false from the browser pass record, while the report counts it as having an entry point (the icon is visible on the counter screenshots).

### What is not here and why

- Full network recordings (HAR) and cookie dumps. They contain the connection's IP address, IP-based location and session identifiers. Their numbers are in `result.json` (field `m6`).
- Banner markup, except the two files above. Banner quotes are visible on the screenshots.
- The IP address, ISP name and machine details. I removed them from `result.yaml` and `result.json`. Everything else in the records is unchanged, including the browser pass notes.
- Scripts.

### Caveats

- A TCF record is a declaration, not proof of tracking.
- A request to a domain is not proof of personal data processing.
- The domain categories are mine. They are public so you can disagree with them.
- State as of 2 to 4 September 2026. The sites may have changed since.

### Licence

Results and records: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/); credit the author and link the report. Screenshots and banner quotes belong to the websites and are quoted to document the measurement. Company names and purposes in the report come from the IAB Global Vendor List, version 174: https://vendor-list.consensu.org/v3/archives/vendor-list-v174.json
