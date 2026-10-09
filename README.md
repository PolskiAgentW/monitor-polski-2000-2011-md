# Monitor Polski 2000–2011 w Markdown

Teksty aktów z **Monitora Polskiego** z lat 2000–2011 w Markdown i jako drzewo jednostek w JSON, z metadanymi
z API ELI Sejmu.
*Texts of acts published in Monitor Polski (Poland's official gazette for non-statutory acts) in 2000–2011, as
Markdown and as a JSON tree of units, converted from the official PDFs. The Sejm ELI API has no HTML for any of them.*

> **Nieoficjalne.** Teksty powstają przez automatyczną konwersję PDF-ów, więc mogą zawierać błędy.
> Wiążący jest PDF w Monitorze Polskim (link `source_pdf` w każdym pliku).

<!-- zbiory:start -->
**Wszystkie zbiory** (ten sam format plików, konwerter [eli2md](https://github.com/PolskiAgentW/eli2md)). Akty, które API ELI
podaje w HTML (np. większość Dziennika Ustaw 2012–2024), nie są tu powielane.

| lata | Dziennik Ustaw | Monitor Polski |
|---|---|---|
| od 2012 | [GitHub](https://github.com/PolskiAgentW/dziennik-ustaw-md) · [HF](https://huggingface.co/datasets/PolskiAgentW/dziennik-ustaw-md): od 2025 r. wszystkie, wcześniej 98 aktów bez HTML; codziennie | [GitHub](https://github.com/PolskiAgentW/monitor-polski-md) · [HF](https://huggingface.co/datasets/PolskiAgentW/monitor-polski-md): wszystkie z PDF (API nie ma HTML); codziennie |
| 2000–2011 | [GitHub](https://github.com/PolskiAgentW/dziennik-ustaw-2000-2011-md) · [HF](https://huggingface.co/datasets/PolskiAgentW/dziennik-ustaw-2000-2011-md): akty bez HTML w API | [GitHub](https://github.com/PolskiAgentW/monitor-polski-2000-2011-md) · [HF](https://huggingface.co/datasets/PolskiAgentW/monitor-polski-2000-2011-md): wszystkie z PDF |
| 1990–1999 | [GitHub](https://github.com/PolskiAgentW/dziennik-ustaw-1990-1999-md) · [HF](https://huggingface.co/datasets/PolskiAgentW/dziennik-ustaw-1990-1999-md): akty bez HTML w API (OCR skanów) | brak |
| 1918–1989 | [GitHub](https://github.com/PolskiAgentW/dziennik-ustaw-1918-1989-md) · [HF](https://huggingface.co/datasets/PolskiAgentW/dziennik-ustaw-1918-1989-md): akty bez HTML w API (OCR skanów; pomiar jakości w README) | brak |

Kolumny są we wszystkich zbiorach te same, więc lata można wczytać razem (nadal bez aktów, które API ELI podaje w HTML):

```python
from datasets import load_dataset

du = load_dataset("parquet", split="train", data_files=[
    "hf://datasets/PolskiAgentW/dziennik-ustaw-1918-1989-md/data/*.parquet",
    "hf://datasets/PolskiAgentW/dziennik-ustaw-1990-1999-md/data/*.parquet",
    "hf://datasets/PolskiAgentW/dziennik-ustaw-2000-2011-md/data/*.parquet",
    "hf://datasets/PolskiAgentW/dziennik-ustaw-md/data/*.parquet",
])  # 58 761 aktów (2026-10-09); Monitor Polski: monitor-polski-2000-2011-md + monitor-polski-md
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

Wszystkie lata 2000–2011 (stan API z 2026-10-03). Akty, którym API później zmieni PDF albo metadane, nie są tu
aktualizowane.

<!-- stats:start -->
Stan na 2026-10-06 22:09 UTC (liczone z `index.csv`).

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

Akty ze stronami bez warstwy tekstowej (skany, grafiki): 542, razem 20203 z 48251 stron. Tekst z OCR (oznaczony) ma 19025 z nich w 480 aktach; treści pozostałych brak.
Akty ze stronami z dużymi obrazami (wzory, rysunki; ich treści brak): 805.

Rodzaje aktów: Postanowienie 5678, Obwieszczenie 2096, Uchwała 1291, Komunikat 1122, Zarządzenie 707, Oświadczenie rządowe 411, Umowa międzynarodowa 271, Ogłoszenie 169, Porozumienie 122, Decyzja 33, Orzeczenie 32, Rezolucja 12, Protokół 9, Oświadczenie 7, Apel 5, Zawiadomienie 3, Sprawozdanie 3, Deklaracja 2, Stanowisko 2, Akt 1, Regulamin 1.
Wersje konwertera: eli2md 0.6.22 (11497), eli2md 0.6.24 (480).
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
