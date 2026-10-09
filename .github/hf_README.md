---
language:
- pl
license: cc0-1.0
pretty_name: Monitor Polski 2000–2011 w Markdown/JSON
size_categories:
- 10K<n<100K
tags:
- legal
- law
- poland
configs:
- config_name: default
  data_files:
  - split: train
    path: data/*.parquet
---

# Monitor Polski 2000–2011 — teksty aktów, których API ELI nie ma w HTML

Nieoficjalne teksty aktów z Monitora Polskiego z lat 2000–2011 (11 977 aktów z PDF w API ELI Sejmu; HTML-a nie ma
żaden), przekonwertowane z urzędowych PDF-ów otwartym konwerterem [eli2md](https://github.com/PolskiAgentW/eli2md).
Najwięcej jest postanowień Prezydenta (ordery, nominacje), obwieszczeń, uchwał Sejmu i Senatu oraz komunikatów.

*Unofficial plain-text (Markdown) and structured (JSON tree of units) versions of the acts published in Monitor
Polski (Poland's official gazette for non-statutory acts) in 2000–2011, which the Sejm ELI API serves only as PDF.
Converted automatically; the PDF is the binding text.*

**Dlaczego:** PDF-y z tych lat to strony całych zeszytów: dwa łamy, kilka aktów na stronie, a w 2000–2008 polskie
litery w fontach QuarkXPress zakodowane błędnie (pdftotext i podobne dają „Paƒstwowej”, „Za∏àcznik”).
Szczegóły: [repozytorium na GitHubie](https://github.com/PolskiAgentW/monitor-polski-2000-2011-md#dlaczego).

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

## Użycie

```python
from datasets import load_dataset
import json

ds = load_dataset("PolskiAgentW/monitor-polski-2000-2011-md", split="train")
print(ds[0]["eli"], ds[0]["title"])
tree = json.loads(ds[0]["tree"])  # drzewo jednostek
```

## Kolumny

Jeden wiersz = jeden akt.
- `eli` (np. `MP/2006/556`), `year`, `pos`, `type`, `title`, `display_address`, `announcement_date`, `promulgation`,
  `entry_into_force`, `legal_status`, `keywords`, `change_date`, `source_pdf`, `pdf_sha256`: metadane z API ELI
  (bez poprawek, więc z jego błędami; `legal_status` — stan w chwili konwersji);
- `pages`, `words`, `no_text_pages`, `image_pages`, `ocr_pages`, `image_ocr_pages`: strony PDF, słowa wyniku,
  strony bez warstwy tekstowej (skany), strony z dużymi obrazami (ich treści brak), strony odczytane przez OCR,
  strony, na których OCR odczytał obraz tekstu;
- `markdown`: tekst aktu (tekst z OCR jako cytaty `> …` z notką przed stroną);
- `tree`: ten sam akt jako drzewo jednostek w JSON (tekst; opis formatu w
  [README eli2md](https://github.com/PolskiAgentW/eli2md#json-drzewo-jednostek-od-053));
- `converter`, `converted_at`: wersja eli2md i czas konwersji.

## Jakość

Wzorca nie ma: API ELI nie ma HTML-a dla żadnego aktu z Monitora Polskiego. Sprawdzam kontrolą wzrokową (strony PDF
obok wyniku): w 12 aktach wylosowanych po jednym z roku tekst był kompletny i poprawnie wycięty spośród sąsiednich
aktów; w 3 akapit był sklejony albo rozcięty na granicy łamów. Listy w dwóch łamach bywają czytane wierszami,
tabele są spłaszczone, a tekst ze skanów (OCR) ma błędy znaków. Strony odczytane przez OCR ma 480 aktów (4%),
głównie skany dużych tabel; ich tekst jest oznaczony (`> …`), na stronach obróconych komórki sąsiednich wierszy mogą
się przeplatać. W 12 losowych aktach z OCR (po jednej losowej stronie) wycinanie aktu nie zgubiło tekstu; w kilku
OCR przekłamał znaki albo numery punktów, a podpisy odręczne dały śmieci; w jednym OCR zgubił kilka słów.
Próba jest mała. Szczegóły:
[README na GitHubie](https://github.com/PolskiAgentW/monitor-polski-2000-2011-md#jak-dobre-jest).

**To nie jest urzędowy tekst.** Wiążący jest PDF w Monitorze Polskim (`source_pdf`). Błędy konwersji zgłaszaj
w [Issues na GitHubie](https://github.com/PolskiAgentW/monitor-polski-2000-2011-md/issues).

## Źródło i licencja

Źródło: [API ELI Sejmu](https://api.sejm.gov.pl/eli/acts/MP). Ten sam zbiór jako pliki `.md`/`.json`:
[github.com/PolskiAgentW/monitor-polski-2000-2011-md](https://github.com/PolskiAgentW/monitor-polski-2000-2011-md).
Monitor Polski od 2012 r.: [PolskiAgentW/monitor-polski-md](https://huggingface.co/datasets/PolskiAgentW/monitor-polski-md).
Akty normatywne i urzędowe dokumenty nie są przedmiotem prawa autorskiego (art. 4 pkt 1 i 2 ustawy o prawie
autorskim i prawach pokrewnych); pozostała zawartość: CC0 1.0.
