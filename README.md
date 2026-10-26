# Monitor Polski 2000–2011 w Markdown

Teksty aktów z **Monitora Polskiego** z lat 2000–2011 w Markdown i jako drzewo jednostek w JSON, z metadanymi
z API ELI Sejmu.
*Texts of acts published in Monitor Polski (Poland's official gazette for non-statutory acts) in 2000–2011, as
Markdown and as a JSON tree of units, converted from the official PDFs. The Sejm ELI API has no HTML for any of them.*

> **Nieoficjalne.** Teksty powstają przez automatyczną konwersję PDF-ów, więc mogą zawierać błędy.
> Wiążący jest PDF w Monitorze Polskim (link `source_pdf` w każdym pliku).

<!-- zbiory:start -->
**Wszystkie zbiory** (ten sam format plików, konwerter [eli2md](https://github.com/PolskiAgentW/eli2md)). Akty, które API ELI
podaje w HTML (np. większość Dziennika Ustaw 2012–2024), nie są tu powielane; wyjątek: teksty jednolite ustaw
z lat 2012–2024 w zbiorze od 2012 (zapas, gdy HTML z API nie działa).

| lata | Dziennik Ustaw | Monitor Polski |
|---|---|---|
| od 2012 | [GitHub](https://github.com/PolskiAgentW/dziennik-ustaw-md) · [HF](https://huggingface.co/datasets/PolskiAgentW/dziennik-ustaw-md): od 2025 r. wszystkie, wcześniej 98 aktów bez HTML i 3 862 teksty jednolite ustaw; codziennie | [GitHub](https://github.com/PolskiAgentW/monitor-polski-md) · [HF](https://huggingface.co/datasets/PolskiAgentW/monitor-polski-md): wszystkie z PDF (API nie ma HTML); codziennie |
| 2000–2011 | [GitHub](https://github.com/PolskiAgentW/dziennik-ustaw-2000-2011-md) · [HF](https://huggingface.co/datasets/PolskiAgentW/dziennik-ustaw-2000-2011-md): akty bez HTML w API | [GitHub](https://github.com/PolskiAgentW/monitor-polski-2000-2011-md) · [HF](https://huggingface.co/datasets/PolskiAgentW/monitor-polski-2000-2011-md): wszystkie z PDF |
| 1990–1999 | [GitHub](https://github.com/PolskiAgentW/dziennik-ustaw-1990-1999-md) · [HF](https://huggingface.co/datasets/PolskiAgentW/dziennik-ustaw-1990-1999-md): akty bez HTML w API (OCR skanów) | brak |
| 1918–1989 | [GitHub](https://github.com/PolskiAgentW/dziennik-ustaw-1918-1989-md) · [HF](https://huggingface.co/datasets/PolskiAgentW/dziennik-ustaw-1918-1989-md): akty bez HTML w API (OCR skanów; pomiar jakości w README) | brak |

Kolumny są we wszystkich zbiorach te same, więc lata można wczytać razem (nadal bez większości aktów, które API ELI podaje w HTML):

```python
from datasets import load_dataset

du = load_dataset("parquet", split="train", data_files=[
    "hf://datasets/PolskiAgentW/dziennik-ustaw-1918-1989-md/data/*.parquet",
    "hf://datasets/PolskiAgentW/dziennik-ustaw-1990-1999-md/data/*.parquet",
    "hf://datasets/PolskiAgentW/dziennik-ustaw-2000-2011-md/data/*.parquet",
    "hf://datasets/PolskiAgentW/dziennik-ustaw-md/data/*.parquet",
])  # 62 627 aktów (2026-10-11); Monitor Polski: monitor-polski-2000-2011-md + monitor-polski-md
```
<!-- zbiory:end -->

## Dlaczego

API ELI Sejmu (`api.sejm.gov.pl/eli`) podaje akty z Monitora Polskiego tylko jako PDF. Z lat 2000–2011 ma 11 980
aktów: 11 977 z PDF i żadnego z HTML (sprawdzone 2026-10-03). Bez PDF są MP/2000/0220469, MP/2000/232 i MP/2008/436.

W tym zbiorze (według typu w API): postanowienia 5678 (głównie Prezydenta: ordery i odznaczenia, nominacje),
obwieszczenia 2096, uchwały 1291, komunikaty 1122 (m.in. GUS i Państwowej Komisji Wyborczej), zarządzenia 707,
oświadczenia rządowe 411, umowy międzynarodowe 271, pozostałe 401.

PDF-y z tych lat to strony całych zeszytów: dwa łamy, kilka aktów na jednej stronie. W latach 2000–2008 polskie
litery w fontach QuarkXPress są zakodowane błędnie, więc pdftotext, pdfplumber i podobne narzędzia dają np.
„Paƒstwowej”, „Za∏àcznik”, „ÂRODKÓW” zamiast „Państwowej”, „Załącznik”, „ŚRODKÓW” (warstwa tekstowa MP/2007/589).
W 12 losowych aktach (po jednym z roku, 2026-10-01) tak zepsute litery miały wszystkie akty z lat 2000–2008 (9 z 9),
a akty z lat 2009–2011 żadne. Konwerter [eli2md](https://github.com/PolskiAgentW/eli2md) poprawia kodowanie, czyta
łamy po kolei i wycina akt spośród sąsiednich. Ten sam problem w Dzienniku Ustaw, z pomiarem kilku narzędzi:
[dziennik-ustaw-2000-2011-md](https://github.com/PolskiAgentW/dziennik-ustaw-2000-2011-md#co-daje-pdftotext-i-podobne-narzędzia).

Monitor Polski od 2012 r., aktualizowany codziennie: [monitor-polski-md](https://github.com/PolskiAgentW/monitor-polski-md).

## Stan

Wszystkie lata 2000–2011 (stan API z 2026-10-03). Akty, którym API później zmieni PDF, nie są tu aktualizowane.
Metadane (daty, tytuł, status, słowa kluczowe) uzgadnia z API ELI codziennie workflow „Metadane” (od 2026-10-14):
akty, którym w API zmienił się `changeDate`, dostają nowe metadane w `index.csv`, front matter, linii `# tytuł`
i `title` w `.json`; tekst zostaje z konwersji. Potem workflow „Eksport” odświeża plik w wydaniu „dane” i na HF.

<!-- stats:start -->
Stan na 2026-10-26 09:05 UTC (liczone z `index.csv`).

| rok | aktów w indeksie | przekonwertowanych | błędów |
|---|---:|---:|---:|
| 2000 | 857 | 857 | 0 |
| 2001 | 787 | 787 | 0 |
| 2002 | 881 | 881 | 0 |
| 2003 | 937 | 937 | 0 |
| 2004 | 959 | 959 | 0 |
| 2005 | 1223 | 1223 | 0 |
| 2006 | 964 | 964 | 0 |
| 2007 | 1092 | 1092 | 0 |
| 2008 | 853 | 853 | 0 |
| 2009 | 1040 | 1040 | 0 |
| 2010 | 1185 | 1185 | 0 |
| 2011 | 1199 | 1199 | 0 |

Akty ze stronami bez warstwy tekstowej (skany, grafiki): 547, razem 20226 z 48251 stron. Tekst z OCR (oznaczony) ma 19036 z nich w 485 aktach; treści pozostałych brak.
Akty ze stronami z dużymi obrazami (wzory, rysunki; ich treści brak): 800.

Rodzaje aktów: Postanowienie 5678, Obwieszczenie 2096, Uchwała 1291, Komunikat 1122, Zarządzenie 707, Oświadczenie rządowe 411, Umowa międzynarodowa 271, Ogłoszenie 169, Porozumienie 122, Decyzja 33, Orzeczenie 32, Rezolucja 12, Protokół 9, Oświadczenie 7, Apel 5, Zawiadomienie 3, Sprawozdanie 3, Deklaracja 2, Stanowisko 2, Akt 1, Regulamin 1.
Wersje konwertera: eli2md 0.6.22 (10975), eli2md 0.6.24 (387), eli2md 0.6.64 (334), eli2md 0.6.72 (137), eli2md 0.6.62 (126), eli2md 0.6.66 (6), eli2md 0.6.65 (6), eli2md 0.6.63 (3), eli2md 0.6.61 (2), eli2md 0.6.69 (1).
<!-- stats:end -->

## Zawartość

- `MP/<rok>/MP-<rok>-<pozycja>.md`: jeden akt. Front matter YAML z metadanymi ELI, potem tekst:
  `##### § N.` (albo `##### Art. N.`), akapity, `## Załącznik …`, przypisy `[^n]`. Ust., pkt i lit. zaczynają
  akapit.
- `MP/<rok>/MP-<rok>-<pozycja>.json`: ten sam akt jako drzewo jednostek (`art`, `par` (§), `ust`, `pkt`, `lit`,
  `tir`) z numerem, ścieżką (`par_2/ust_1/pkt_3`), tekstem i dziećmi. Opis:
  [README eli2md](https://github.com/PolskiAgentW/eli2md#json-drzewo-jednostek-od-053).
- Cały zbiór w jednym pliku: `monitor-polski-2000-2011-md.jsonl.gz` w wydaniu
  [„dane”](https://github.com/PolskiAgentW/monitor-polski-2000-2011-md/releases/tag/dane) (jeden akt w wierszu)
  i Parquet na Hugging Face: [PolskiAgentW/monitor-polski-2000-2011-md](https://huggingface.co/datasets/PolskiAgentW/monitor-polski-2000-2011-md).
- `index.csv`: jeden wiersz na akt, także nieudany: `eli, year, pos, type, title, announcement_date,
  promulgation, change_date, pdf_sha256, pages, words, no_text_pages, image_pages, ocr_pages, image_ocr_pages,
  status, error, converter, converted_at`.

Strony bez warstwy tekstowej czyta OCR (tesseract). W tym zbiorze to głównie skany dużych tabel: sprawozdania
z wykonania budżetu państwa, komunikaty PKW o sprawozdaniach finansowych partii, obwieszczenia z wykazami (np.
MP/2007/802: 721 stron z OCR). Tekst z OCR jest oznaczony: przed stroną stoi notka `> [Strona 1 PDF nie ma czytelnej
warstwy tekstowej. Tekst poniżej odczytał OCR …]`, a akapity OCR są cytatami blokowymi (`> …`); w JSON to węzły
`ocr`, bez podziału na jednostki.

## Jak dobre jest

Wzorca nie ma: API ELI nie ma HTML-a dla żadnego aktu z Monitora Polskiego, więc wyniku nie da się porównać
z oficjalnym tekstem. Miara bez wzorca z [monitor-polski-md](https://github.com/PolskiAgentW/monitor-polski-md)
(słowa wyniku wobec warstwy tekstowej PDF) tu nie działa, bo PDF aktu to całe strony zeszytu razem z sąsiednimi
aktami. Konwerter jest ten sam co w Dzienniku Ustaw 2000–2011, gdzie na aktach z HTML-em odzyskuje 99,8–99,9% słów
oficjalnego tekstu we właściwej kolejności (dwie próby po 56 i 62 akty, szczegóły w
[dziennik-ustaw-2000-2011-md](https://github.com/PolskiAgentW/dziennik-ustaw-2000-2011-md#jak-dobre-jest)).
To głównie ustawy; akty Monitora Polskiego mogą wypadać inaczej.

**Kontrola wzrokowa** (2026-10-03, strony PDF obok pełnego tekstu wyniku):
- 12 aktów bez skanów i obrazów (eli2md 0.6.22), wylosowanych po jednym z roku (ziarno 6203): MP/2000/263, MP/2001/349, MP/2002/143,
  MP/2003/177, MP/2004/576, MP/2005/838, MP/2006/556, MP/2007/1005, MP/2008/158, MP/2009/348, MP/2010/284,
  MP/2011/526. We wszystkich tekst jest kompletny, akt poprawnie wycięty ze stron wspólnych z innymi aktami (bez
  sąsiednich aktów i stopki zeszytu), polskie litery poprawne. W 3 z nich (MP/2001/349, MP/2002/143, MP/2008/158)
  akapit jest sklejony albo rozcięty na granicy łamów, np. „6. Stachowski Mieczysław, na wniosek Prezesa Rady
  Ministrów…”. Większość aktów w zbiorze to krótkie postanowienia o orderach i nominacjach; próba to odzwierciedla.
- 2 dłuższe akty (środkowa strona, ziarno 6204): MP/2003/383 (wytyczne PKW, 10 stron): łamy po kolei, bez uwag.
  MP/2007/589 (komunikat PKW): lista partii w załączniku 1, złożona w dwóch łamach, jest czytana wierszami
  („1. … 17. … 2. … 18. …”) w jednym akapicie; pozycje są kompletne.
- 12 aktów z co najmniej jedną stroną z OCR (eli2md 0.6.24), wylosowanych spośród 480 takich aktów
  (ziarno 6301); przy każdym jedna losowa strona z OCR (ziarno 6302) obok odpowiadającej jej części wyniku:
  MP/2002/298, MP/2002/688, MP/2003/216, MP/2008/189, MP/2008/204, MP/2008/520,
  MP/2010/191, MP/2010/643, MP/2010/761, MP/2010/1183, MP/2011/137, MP/2011/560.
  - Tekst kompletny w MP/2002/298, MP/2002/688 (tabela, pary nazwa–liczba zgodne), MP/2003/216 i MP/2008/189 s. 5.
    Błędy znaków: „Goldap” zamiast „Gołdap”, „3” zamiast „2.”; niemieckie litery w MP/2002/298 źle („fiir”, „Bricke”),
    bo OCR czyta strony słownikiem polskim i angielskim (bez niemieckiego). Nagłówek listu w dwóch łamach czytany
    wierszami.
  - MP/2008/204 i MP/2008/520: strony obrócone o 90° z tabelą. Wiersze są, spłaszczone do akapitów; w MP/2008/520
    komórki sąsiednich wierszy się przeplatają (np. 14 i 15), „EN 1636” zamiast „EN 1836”.
  - MP/2008/189, ostatnia strona (podpisy umowy): brak „For the Kingdom of Norway” i jednego „Signed in …”, a podpisy
    odręczne dają śmieci. Tych słów nie ma już w surowym wyniku OCR tej strony, więc gubi je OCR, nie wycinanie aktu.
  - Tekst kompletny w MP/2010/191 s. 9 (ostatnia, podpisy umowy), MP/2010/643 s. 65 i 70 (tabela), MP/2010/761 s. 4
    i MP/2010/1183 s. 27 (ciągły tekst, bez uwag poza sklejonymi akapitami). Daty wpisane ręcznie i podpisy dają
    śmieci („Wa YJAM |. on. ZOdk…”). W MP/2010/643 kolumny tabeli są czytane osobno: najpierw same numery wierszy,
    potem nazwy jednostek, potem uprawnienia; kolejność w każdej kolumnie zgodna, ale pary trzeba składać po pozycji.
    W MP/2010/761 numery ustępów są zgubione albo przekłamane („LO” zamiast „2.”, „l.” zamiast „1.”), plus luźne
    znaki („ta”, „122”).
  - MP/2011/137 (umowa o Sekretariacie Partnerstwa Wymiaru Północnego, po angielsku) s. 16 i 25 (ostatnia): tekst
    angielski kompletny i dokładny. Na s. 16 numery punktów (1)–(9) są wypisane osobno przed treścią, a treść
    punktów sklejona w jeden akapit; dwa numery przekłamane („6)” zamiast „(3)”, „©)” zamiast „(9)”). Na s. 25
    nagłówek białoruskiego ministerstwa (cyrylica) to łacińskie śmieci („TPAHCNAPTY I KAMYHIKALIbIA”), bo OCR nie ma
    słownika rosyjskiego ani białoruskiego; nazwisko pod podpisem jako „fm van Shcherbo”.
  - MP/2011/560 (plan gospodarowania wodami w dorzeczu Dunaju, 199 stron) s. 163: mapa na stronie obróconej o 90°.
    Tytuł mapy i legenda są odczytane poprawnie, ale nazw miejscowości i rzek z mapy prawie nie ma (jest tylko
    „Lipnica Mała”); na końcu nagłówek zeszytu odczytany do góry nogami. S. 199 (ostatnia, z OCR) to ogłoszenie
    wydawcy w zeszycie („Szanowni Państwo”, ISSN), nie tekst aktu; konwerter usuwa je celowo, więc w wyniku jej nie ma.
  Wycinanie aktu nie zgubiło tekstu w żadnym z 12 aktów. W jednym (MP/2008/189) słowa gubi sam OCR.

Próba jest mała: odsetka błędnych aktów na tej podstawie nie da się ocenić.

Zmiana 2026-10-03 (eli2md 0.6.23 i 0.6.24, wszystkie 480 aktów z OCR przeliczone ponownie):
- 0.6.23: gdy OCR skleił nagłówek strony zeszytu („Monitor Polski Nr 53 — 2020 — Poz. 470”) z dalszym tekstem w jeden
  akapit, konwerter usuwał cały akapit. Teraz usuwa tylko nagłówek. Np. MP/2008/470 (skan jednej strony) miał 0 słów, teraz 54.
- 0.6.24: na stronach z OCR akt jest wycinany także wtedy, gdy między numerem pozycji a rodzajem aktu stoi numer
  rejestru („626 / Rej. 182/2000 / POSTANOWIENIE …”). Wcześniej np. MP/2000/626 zawierał całą stronę: koniec
  poz. 625, poz. 626 i początek poz. 627 (455 słów). Teraz tylko poz. 626 (96 słów).
- Skutek obu zmian dla 480 aktów z OCR (porównanie index.csv przed i po): więcej słów w 131 aktach (razem +8438),
  tyle samo w 324, mniej w 25 (razem −9907). Te 25 to MP/2000/611–634 (bez 618) oraz MP/2002/121 i MP/2002/122:
  wcześniej plik zawierał też tekst sąsiednich pozycji ze wspólnej strony, teraz tylko swój. W MP/2009/978 jedna
  strona więcej idzie przez OCR (3 → 4).
- Pozostałe 11 497 aktów (bez stron z OCR) mają w polu `converter` wersję 0.6.22: obie zmiany dotyczą tylko stron
  z OCR, więc nie były przeliczane.

Zmiana 2026-10-07 (eli2md 0.6.39, 152 akty; opis w README eli2md, wpis 0.6.39): w umowach międzynarodowych i podobnych
aktach „Artykuł N” jest w JSON węzłem `art` z polem `label`. `.md` się nie zmienił, więc pole `converter` (w `.md`,
`.json` i `index.csv`) zostaje wersją, w której powstał `.md`; `.json` zbudowano z niego kodem drzewa 0.6.39. Słowa
w drzewach: zgubione 0. Pozostałe akty bez zmian (porównanie drzew wszystkich aktów zbioru).

Zmiana 2026-10-10 (drzewo JSON, kod drzewa eli2md 0.6.50, 3 akty; opis w README eli2md, wpis 0.6.50): nagłówki z
liczebnikiem słownym („DZIAŁ PIĄTY”, „CZĘŚĆ PIERWSZA”), z numerem rzymskim z wielką literą („DZIAŁ IVA”) albo z
odnośnikiem po numerze („Rozdział 5a[^28]”) są w `.json` węzłami `heading`. Wcześniej trafiały jako tekst do artykułu,
paragrafu albo punktu przed nimi. Nowych nagłówków: 13, żaden nie zniknął. `.md` się nie zmienił, więc pole
`converter` (w `.md`, `.json` i `index.csv`) zostaje wersją, w której powstał `.md`. Liczba jednostek bez zmian, słowa
w drzewach: zgubione 0. Pozostałe akty bez zmian (porównanie drzew wszystkich aktów zbioru).

Zmiana 2026-10-13 (metadane, 100 aktów; tekst bez zmian): API ELI zmieniło po konwersji metadane 100 aktów
(`changeDate` 2026-10-06–09). 46 z nich to rekordy z list niezgodności z drukiem, które ISAP poprawił po zgłoszeniu
([daty-ogloszenia](https://github.com/PolskiAgentW/daty-ogloszenia)). W tych aktach `index.csv` (`title`,
`announcement_date`, `promulgation`, `change_date`), front matter `.md` (klucze z API), linia `# tytuł` w `.md` i pole
`title` w `.json` mają teraz wartości z API z 2026-10-13. Zmienione: data wejścia w życie w 55 aktach (w 34 API jej
już nie podaje, w 21 inna), data ogłoszenia w 35, tytuł w 14, status w 5 („obowiązujący” → „akt indywidualny” albo
„akt jednorazowy”), data publikacji w 3 (wcześniej jej nie było). W 7 aktach zmienił się tylko `change_date`. Tekst,
`converter`, `pdf_sha256` i liczby w `index.csv` zostają z konwersji. PDF w API jest ten sam (sha256) w 99 aktach.
Wyjątek: MP/2001/114. Jego PDF w API to teraz cała strona 151 zeszytu (poz. 113, 114 i 115), a tekst w zbiorze powstał
z wcześniejszego PDF z samą poz. 114, więc zostaje. Porównanie całego zbioru z listami API z 2026-10-13: poza tymi
100 aktami różnic w polach z API nie ma.

Zmiana 2026-10-24 (eli2md 0.6.61, 2 akty; opis w README eli2md, wpis 0.6.61): oznaczenie punktu z odnośnikiem na początku
wiersza doklejało się do poprzedniego akapitu: „14d)[^117] roczne sprawozdania Prezesa Narodowego Banku Polskiego…,
14e)[^117] coroczne sprawozdania…” (MP/2009/47). Teraz zaczyna osobną jednostkę. Przeliczyłem ponownie (z eli2md 0.6.22
na 0.6.61) 2 akty, w których to wystąpiło: MP/2009/47 (te same słowa) i MP/2010/542. W MP/2010/542 wracają też
wyrazy rozdzielone po pierwszej literze: „a rt. 2” → „art. 2”, „z leca” → „zleca” (te same znaki).

Zmiana 2026-10-25 (eli2md 0.6.62, 161 aktów; opis w README eli2md, wpis 0.6.62): nagłówek strony numeru
(„Monitor Polski Nr 56 — 4642 — Poz. 567”) i notka drukarni zostawały w tekście aktu, gdy nie były pierwszą linią
strony (strona z obróconą tabelą, nagłówek pod notką, strona czytana przez OCR). Przeliczyłem ponownie 172 akty,
w których był taki nagłówek (1706 wystąpień). Nowy plik zostaje tylko wtedy, gdy nagłówków jest mniej, a reszta tekstu
(bez nagłówków, notek, białych znaków i ogonków) różni się od poprzedniej najwyżej o 0,5% znaków. Tak jest w 161 aktach
(w 114 z nich reszta tekstu ma te same znaki). W 154 z nich nagłówka już nie ma; w 7 (MP/2011/424, 425, 560, 561, 566,
567, 579) został jeden, na stronie, której znaki w PDF nie mają kodów Unicode. W 4 aktach (MP/2003/159, MP/2009/355,
MP/2010/464, MP/2011/451) `ocr_pages` w `index.csv` jest o 1 mniejsze.
Bez zmian zostało 11 aktów. W 3 (MP/2007/566, MP/2008/801, MP/2009/780) nagłówka w nowym pliku nie było, ale reszta
tekstu różniła się o więcej niż 0,5%. W 8 nagłówek zostawał także w nowym pliku: w 4 nad nagłówkiem jest notka drukarni
„korekta: 16.07.02” (MP/2002/489, 491, 493, 495), w 4 (MP/2006/504, 565, 566, 567) przyczyny nie znam.

Tego samego dnia eli2md 0.6.63, 7 aktów (opis w README eli2md, wpis 0.6.63): nagłówek na stronie, której znaki nie mają
kodów Unicode, też jest usuwany. W MP/2011/424, 425, 560, 561, 566, 567 i 579 zniknął ten jeden nagłówek, reszta tekstu
jest bez zmian. Przeliczyłem też 4 akty z notką „korekta: 16.07.02” (MP/2002/489, 491, 493, 495). Nagłówka w nowym pliku
nie ma, ale strony, których warstwa tekstowa ma tylko notkę i nagłówek, a reszta jest skanem, 0.6.63 czyta przez OCR,
więc dochodzi ich tekst (w MP/2002/493 niemiecka nota werbalna). Reguła nie przyjmuje takiej zmiany bez osobnego
sprawdzenia, więc te 4 pliki są bez zmian. W zbiorze zostało 20 nagłówków w 11 aktach.

Tego samego dnia eli2md 0.6.64, 334 akty z lat 2001–2002 (opis w README eli2md, wpis 0.6.64): w tych latach „Nr”
w nagłówku strony jest złożone z odstępem i warstwa tekstowa daje „N r” („Monitor Polski N r 43 — 1077 — Poz. 700”).
Do 0.6.63 taka strona nie była rozpoznawana jako strona numeru. Skutki: nagłówek zostawał w tekście (785 wystąpień
w 336 aktach), strona w dwóch łamach była czytana w poprzek, a plik aktu zawierał też teksty innych pozycji z tych
samych stron (MP/2001/111 miał też poz. 109, 110 i 112). Przeliczyłem ponownie te 336 aktów. Nowy plik zostaje, jeśli
nagłówków jest mniej, a nowy tekst nie ma znaków, których nie było w starym (najwyżej 0,5% znaków nowego, nie licząc
znaczników Markdown), bo poprawka ma tylko odciąć sąsiednie pozycje, ułożyć łamy i usunąć nagłówek. Tak jest w 334
aktach (w 330 nie doszedł żaden znak). Razem mają teraz 1 261 556 znaków zamiast 1 835 391. Obejrzałem od początku do
końca 9 z nich (MP/2001/111, 173, 404, 419, 466, 516, 529, 588, MP/2002/431): każdy ma tylko własny tytuł, treść
i podpis. W MP/2001/432 11 stron map, których warstwa tekstowa miała tylko nagłówek, jest teraz liczonych jako strony
bez tekstu, a OCR 3 z nich (s. 26, 74, 85) dodaje trochę tekstu (0,17% znaków). W MP/2001/114 plik i `pdf_sha256`
są z obecnego PDF w API (zmienionego 2026-10-09; tekst ten sam). Bez zmian zostały 2 akty: MP/2001/569 i MP/2002/783,
w których skany stron z samym nagłówkiem w warstwie tekstowej są teraz czytane przez OCR i dochodzi ich tekst. W zbiorze
zostały 103 nagłówki w 13 aktach.

Tego samego dnia eli2md 0.6.65, 7 aktów (opis w README eli2md, wpis 0.6.65): na stronach czytanych przez OCR nagłówek
numeru odczytany bez numeru strony i pozycji („Monitor Polski Nr 26”) i linia drukarni z górnego pasa strony nie trafiają
już do tekstu. Przeliczyłem ponownie 6 aktów, w których taki nagłówek był osobnym akapitem na górze strony z OCR. Reguła
jak dla 0.6.62 (nagłówków i notek mniej, reszta tekstu różni się najwyżej o 0,5% znaków; nagłówek bez pozycji też jest
wycinany przed porównaniem). Nowy plik zostaje w 3 (MP/2003/417, MP/2008/332, MP/2009/355; nagłówków 4 → 0, w 417 i 355
reszta tekstu ma te same znaki). Bez zmian zostały 3: w MP/2005/456 nowy plik ma tyle samo nagłówków, w MP/2007/856
i MP/2009/780 reszta tekstu różniła się od wersji z eli2md 0.6.24 o 0,69% i 0,78%. Osobno MP/2002/489, 491, 493 i 495
(opisane wyżej przy 0.6.63): strony, których warstwa tekstowa ma tylko linię drukarni i nagłówek, są czytane przez OCR.
Nowy plik zostaje, bo spełnia regułę zapisaną przed przeliczeniem: z tekstu nic nie ubywa, nowy tekst pochodzi tylko ze
stron, które wcześniej były obrazem albo nie miały warstwy tekstowej, a w nowym pliku nie ma nagłówków (było 12) ani
linii drukarni. MP/2001/569 i MP/2002/783 zostają bez zmian. W zbiorze zostało 91 nagłówków w 9 aktach.

Tego samego dnia eli2md 0.6.66, 8 aktów (opis w README eli2md, wpis 0.6.66): ucięty nagłówek numeru był też w aktach
spoza list wyżej — w środku strony albo doklejony do akapitu. Przeliczyłem ponownie (głównie z eli2md 0.6.24) 10 aktów,
w których po nazwie dziennika jest numer bez „poz.” (to wyklucza cytaty). Reguła jak dla 0.6.65. Nowy plik zostaje w 8
aktach (w 2 z nich reszta tekstu ma te same znaki, w pozostałych różni się najwyżej o 0,4%, głównie odczytem OCR polskich
znaków); uciętych nagłówków było w nich 12, zostały 2 (MP/2010/464). Bez zmian zostały 2: w MP/2000/170 nowy plik ma tyle
samo nagłówków, w MP/2002/503 reszta tekstu różniła się o 14,6% (OCR tabeli czyta więcej komórek, ale numery wierszy
„51.” bierze za „§ 1.”).

Zmiana 2026-10-26 (eli2md 0.6.69, 3 akty; opis w README eli2md, wpisy 0.6.68 i 0.6.69): poprawka usterki z przeliczeń
2026-10-25. OCR brał numer wiersza listy albo tabeli zaczynający się od „5” albo „8” za sklejone „§”. Przeliczyłem 59
aktów, w których nagłówki „§” różnią się od wersji sprzed 2026-10-25. W 56 przeliczenie nie zmienia tekstu. W MP/2000/544
„885. | Vitaminum C …” jest znów wierszem tabeli, a nie „§ 85.”. MP/2006/444 i MP/2010/775 wracają do wersji sprzed
2026-10-25, bo sklejony „8N.” bez „|” nadal byłby brany za „§” („81. W okresie badanym …” → „§ 1.”).

Tego samego dnia bilans nagłówków „§” po przeliczeniach z 2026-10-25 i 2026-10-26: obejrzałem zmiany nagłówków „§”
w aktach, w których różnią się od wersji sprzed 2026-10-25 (56 w tym zbiorze). MP/2009/355 wraca do wersji sprzed
2026-10-25: numer rachunku bankowego „89109010560000000104291776.” był wzięty za „§”. W pozostałych 55 zmiany, które
obejrzałem, są poprawą: z dwóch łamów czytanych w poprzek „§ N.” w środku wiersza staje się nagłówkiem z ciągłym tekstem
(np. MP/2001/668, sprawdzone z PDF: §§ 1–47 w kolejności), a znikające nagłówki należały do tekstu sąsiednich pozycji,
który był w pliku (MP/2001/115: § 1 z poz. 114).

Tego samego dnia eli2md 0.6.72, 137 aktów (opis w README eli2md, wpis 0.6.72): wiersz z samym numerem strony nie jest
już tekstem. Taki wiersz ma np. strona, której cała treść jest obrazem (wzór, skan załącznika): w warstwie tekstowej
jest tylko numer („— 1831 —”, MP/2008/380 s. 3). Numer takiej strony bywał też doklejony na końcu ostatniego wiersza
strony poprzedniej („Minister Sprawiedliwości: Z. Ćwiąkalski — 1830 —”, s. 1) i teraz też znika. Przeliczyłem ponownie
208 aktów, w których stoi taki wiersz. Nowy plik zostaje tylko wtedy, gdy ciąg słów różni się od poprzedniego wyłącznie
brakiem numerów stron i kawałków nagłówka, a jednostki drzewa są te same. Tak jest w 137 aktach; w 3 z nich zmienia się
też podział akapitów przy tych samych słowach: w MP/2010/1130 reklama wydawcy po zniknięciu numeru jest jednym akapitem,
w MP/2011/565 i MP/2011/578 (tabela i legenda mapy z OCR, pliki z eli2md 0.6.62) to zmiana konwertera między 0.6.62 a
0.6.71. W 71 zostaje poprzedni plik: w 5 nowy miał te same słowa, w 66 różnił się też w innych miejscach (65 plików z
eli2md 0.6.22 i 0.6.24 ze zmianami konwertera od tamtych wersji, np. pierwsza litera akapitu odklejona od wyrazu, „s
topy CIRR” w MP/2010/208; MP/2011/549 z 0.6.62: inna kolejność wiersza tabeli), których tu nie publikuję. Wierszy z
samym numerem strony jest w zbiorze 116 w 72 plikach (było 322 w 209). Z przeliczonych 208 aktów w wyniku 0.6.72 zostaje
ich 20 w 7 aktach (0.6.71 na tych samych PDF: 335): 18 to numeracja stron załącznika, na dole strony albo pod nagłówkiem
numeru, niżej niż górne 8% strony (np. „-3-” w MP/2011/1178, ok. 10% wysokości strony), 1 na stronie poziomej, na której
nagłówek stoi z boku (MP/2003/159), 1 to wartość na osi wykresu (MP/2011/452). Metadane stron w `index.csv` bez zmian.

**Czego te liczby nie mówią:**

- Dla Monitora Polskiego nie ma wzorca. Jakość sprawdzam tylko kontrolą wzrokową (wyżej, 26 aktów).
- OCR myli znaki (np. „8 1.” zamiast „§ 1.”). Gdy skan ma dwa łamy, OCR potrafi czytać je wierszami w poprzek
  (MP/2008/470).
- Tabele są spłaszczone do akapitów, zwykle wiersz po wierszu, czasem kolumna po kolumnie (MP/2010/643); komórki
  wielowierszowe mogą się przeplatać. Numery wierszy i ustępów OCR często gubi albo przekłamuje. Listy złożone
  w dwóch łamach bywają czytane wierszami (wyżej).
- Na stronach obróconych o 90° OCR czyta też nagłówek zeszytu do góry nogami, co daje szum na końcu strony, np.
  „0Z JN PISIOd 10HUOJN”, „— G6v —”. Wierszy z „PISIOd” albo „DISIOd” jest 6509 w 96 plikach (najwięcej w MP/2007/802).
- Podpisy odręczne OCR czyta jako śmieci i może przy nich zgubić sąsiednie słowa (MP/2008/189).
- Teksty niemieckie z OCR mają błędne umlauty („fiir” zamiast „für”), a cyrylica z OCR to łacińskie śmieci
  (MP/2011/137): OCR ma tylko słownik polski i angielski.
- Załączniki, które są grafiką, mają w tekście tylko znacznik obrazu (`> [Na stronie N PDF jest obraz …]`).

Błędy konwersji zgłaszaj w Issues. Najlepiej podaj pozycję aktu i fragment.

## Licencja

Akty normatywne i ich urzędowe projekty oraz urzędowe dokumenty i materiały nie są przedmiotem prawa
autorskiego (art. 4 pkt 1 i 2 ustawy o prawie autorskim i prawach pokrewnych). Pozostała zawartość
(indeks, skrypty): CC0 1.0.
