---
name: stylometria
description: Analizuje styl konkretnej osoby/marki na podstawie dostarczonego korpusu tekstów (transkrypty YT, posty social, artykuły blogowe, e-booki, transkrypcje podcastów, wklejony tekst). Wynik: 3 artefakty — raport analityczny ze statystykami (n-gramy, TTR, długość zdań, otwarcia/zamknięcia), styleguide (frazy-kotwice, rytm, hooki, do/don't, adaptacja per kanał) oraz execution-ready system prompt dla agentów piszących w tym głosie. ZAWSZE używaj tego skilla gdy użytkownik mówi „zrób stylometrię", „przeanalizuj styl X", „styleguide z tych tekstów", „nauczcz mnie pisać jak Y", „system prompt w stylu Z", „głos marki z tych transkryptów", „voice fingerprint", „idiolekt", „stylowy odcisk palca", „klonuj styl pisania", „analiza języka X", „raport stylu autora". Pierwsza akcja skilla to ZAWSZE pytanie „Masz tekst, czy mam znaleźć?" przez AskUserQuestion. Skill działa na pojedynczym pliku, folderze plików, URL do bloga/kanału YT/profilu social albo wklejonym tekście — minimum użyteczne to ~10 000 słów, optimum 30 000+. Dla profili medialnych preferuj 20-40 ostatnich materiałów.
---

# Stylometria – analiza stylu i ekstrakcja głosu

Cel: wyciągnąć z korpusu tekstów (lub transkryptów) charakterystyczny styl konkretnej osoby/marki i przepakować go w 3 użyteczne artefakty — raport analityczny, styleguide do ręcznej pracy i system prompt do automatyzacji.

Skill jest celowo agnostyczny względem domeny — działa tak samo dla twórcy YouTube, autora bloga, klienta Daniela, jak i dla samego Daniela. Wszystko zależy od korpusu, który dostanie na wejściu.

## Workflow

### Krok 0 — pytanie startowe (OBOWIĄZKOWE, ZAWSZE jako pierwsza akcja)

Zanim cokolwiek zaczniesz, zadaj jedno pytanie przez `AskUserQuestion`:

> **Masz tekst, czy mam znaleźć?**

Dwie opcje:

1. **Mam tekst** — użytkownik wskaże plik, folder, URL albo wklei treść.
2. **Znajdź** — użytkownik poda osobę/markę, Ty znajdziesz źródła.

Nie zakładaj odpowiedzi. Nie zaczynaj research'u zanim user nie wybierze. Jedyny wyjątek: jeśli w komendzie startowej użytkownik wyraźnie podał już ścieżkę/URL/tekst (np. „zrób stylometrię z pliku C:\X\Y.txt") — pomijasz Krok 0 i lecisz dalej.

### Krok 1A — branch „Mam tekst"

Dopytaj o lokalizację jeśli nie podana. Akceptowane wejścia:

- **Pojedynczy plik** (.txt, .md, .docx, .pdf, .json3, .vtt). Dla .docx użyj skilla `docx`, dla .pdf — `pdf`. Dla json3/vtt (napisy YT) — zekstrahuj plaintext z eventów.
- **Folder** z plikami tekstowymi — załaduj wszystkie pliki tekstowe rekurencyjnie.
- **URL** — bloga (lista postów), kanału YT (transkrypty filmów), profilu Instagram/Threads/LinkedIn (eksport postów), strony autorskiej. Dla YT skorzystaj z procesu opisanego w „Pozyskiwanie z YouTube" niżej.
- **Wklejony tekst** w wiadomości — zapisz do tymczasowego pliku, idź dalej.

Sprawdź rozmiar:
- < 10 000 słów: ostrzeż użytkownika, że to mało, zapytaj czy idziesz dalej czy dorzuca.
- 10 000-30 000: OK, ostrzeż że n-gramy mogą być zaszumione.
- 30 000+: OK, leć.
- 100 000+: dział, ale powiedz że analiza zajmie chwilę dłużej.

### Krok 1B — branch „Znajdź"

Dopytaj o:
- Imię i nazwisko / nazwę marki.
- Preferowane źródło (kanał YT, blog, social media, e-book, książka).
- Jeśli wiadomo — handle/URL (oszczędza search).

Pipeline znajdowania:

1. **WebSearch** — zlokalizuj profile/kanały tej osoby (YouTube, LinkedIn, blog, Twitter/X, Threads).
2. **Spytaj o preferencję źródła** (jeśli jest wybór).
3. Dla **YouTube** użyj sekcji „Pozyskiwanie z YouTube" niżej.
4. Dla **bloga/strony autorskiej** użyj WebFetch na listingu + iteracyjnie pobierz top 20-30 artykułów.
5. Dla **social** — jeśli user ma eksport (CSV/JSON), poproś o ścieżkę. Bez eksportu social to ślepa uliczka, bo platformy nie dają lekkiego dostępu.

### Krok 2 — pozyskiwanie z YouTube (subrutyna)

Daniel ma lokalną aplikację Next.js do batch-pobierania transkryptów: `C:\Users\danie\Documents\Firmowe\DanielProgramista\YT mass transcript` (port 3000). Apka ma 2 znane bugi (sortuje języki alfabetycznie → trafia w `ab`/abchaski; przerywa pobieranie URL na 429 z jednego języka, gubiąc plik z innego). **Pomijaj apkę, idź yt-dlp bezpośrednio** według wzorca:

1. **Scan kanału:**
   ```powershell
   yt-dlp --flat-playlist --dump-json --no-warnings --playlist-end 50 "https://www.youtube.com/channel/UC..."
   ```
   Jeśli handle ma kropkę (np. `@geekwork.pl`) — używa channel ID, nie handle (handle z kropką rzuca 404).
2. **Wybierz N najnowszych** dłuższych niż 5 min (shorty się nie nadają — za krótki sygnał stylu).
3. **Pobierz transkrypty sekwencyjnie z opóźnieniem** (3-5 s między requestami):
   ```powershell
   yt-dlp --quiet --skip-download --write-subs --write-auto-subs --sub-format json3 --sub-langs pl,pl-PL,en,en-US -o "tmp/%(id)s.%(ext)s" "URL"
   ```
4. **Retry na 429** (Too Many Requests) z backoffem 20-60 s, max 3 próby na język.
5. **Parsuj json3** — wyciągnij `events[].segs[].utf8`, zlej w plaintext.
6. Zapisz do `transkrypcje-yt/01_VIDEOID.txt` z nagłówkiem (tytuł, URL, ID, długość, język) + separator `---` + treść.

Wzorzec skryptu PowerShell — w `references/fetch_youtube.ps1.template` (sąsiednie pliki skilla).

### Krok 3 — przygotowanie korpusu

1. Zlicz wszystkie pliki i znajdź ich łączną długość w słowach.
2. Stwórz folder roboczy `stylometria/` — w folderze klienta jeśli kontekst sugeruje konkretnego klienta, inaczej w folderze, z którego pochodzi korpus.
3. Skopiuj skrypt analizujący jako `_stylometry.py` (template w `references/stylometry.py.template`) i dostosuj ścieżki:
   - `ROOT` = folder roboczy
   - `CORPUS_DIR` = folder z surowymi tekstami (transkrypty / posty / artykuły)
   - `OUT_JSON` = `_stats.json`
   - `OUT_MD` = `_stats-raw.md`

### Krok 4 — analiza Pythonem

Uruchom skrypt. Skrypt liczy:

- Summary: liczba tekstów, tokenów, types, TTR, zdań, średnia/mediana długości zdań, % pytań, dystrybucja długości słów, zdania krótkie/długie.
- Top content words (50) — bez stopwordów.
- Top n-gramy: bigramy (60), trigramy (60), 4-gramy (40), 5-gramy (30).
- Top sentence openers (40) — pierwsze 2 słowa zdania.
- Top sentence enders (30) — ostatnie 2 słowa zdania.
- Próbki: 30 bardzo krótkich zdań (≤5 słów), 10 otwarć filmów/postów (pierwsze 3 zdania), 10 zamknięć.
- Signature phrases — szukaj zdefiniowanych przez Ciebie fraz (możesz dostosować listę po obejrzeniu top n-gramów).

Skrypt zapisuje stats JSON i markdown z próbkami.

### Krok 5 — analiza jakościowa (czytaj próbki)

Otwórz `_stats-raw.md` i wyciągnij:

- **5-10 sygnatur stylu** — co tę osobę wyróżnia (np. dwutakt rytmu, filler-otwarcia, frazy-kotwice).
- **Wzorzec hooka** — jak otwiera materiały. Sklasyfikuj w 2-4 typy.
- **Wzorzec outra** — czy ma stały template zamknięcia.
- **Czego NIE robi** — które typowe AI-izmy/korpożargon/kotwice marketingowe są nieobecne.
- **Sponsoring / wewnętrzny CTA** — jak buduje mid-roll, jak CTA.
- **Liczba pierwszej/drugiej/trzeciej osoby** — czy mówi „ja", „my", „on".

Jeśli korpus pokrywa wiele rejestrów (np. YT + LinkedIn + landing), dodatkowo:
- **Tabela różnic per kanał** — gdzie luźniejszy, gdzie twardszy, co adaptujesz.

**Lesson learned — code-switching i wtręty obcojęzyczne.** Polskie auto-caption YT (i większość ASR) zżera krótkie wtręty obcojęzyczne — transkrybuje je fonetycznie po polsku albo gubi. Jeśli korpus pochodzi z auto-caption, a osoba żyje w branży tech / IT / startup, **zapytaj usera off-corpus**, czy autor wtrąca anglicyzmy („what can I say?", „who cares", „doesn't matter", „come on", „by the way", „game changer", „trust me", „big deal", „end of story"). Jeśli tak — dodaj to do styleguide jako sygnaturę rytmiczną z konkretną częstotliwością (typowo 1 wtręt na 100-200 słów mowy, 1 na 300-400 słów pisma LinkedIn, 0-1 na całą formalną formę). Auto-caption NIE jest tu wiarygodnym źródłem — wprost ostrzegaj o tym w raporcie.

### Krok 6 — wygenerowanie 3 artefaktów

Stwórz w `stylometria/`:

#### 6.1 `01-raport-analityczny.md`

Struktura (wzorzec — patrz `references/01-raport.template.md`):

1. Rozmiar próby i tabela summary.
2. Frazy-kotwice (top n-gramy + content words) z interpretacją.
3. Otwarcia zdań — co to mówi o stylu.
4. Zakończenia zdań.
5. Wzorzec hooka (typy + przykłady).
6. Wzorzec outra (template + warianty).
7. Mid-roll / sponsoring (jeśli dotyczy).
8. 10 sygnatur stylu (każda 2-3 zdania z konkretem liczbowym z korpusu).
9. Czego NIE robi w piśmie/mowie.
10. Charakterystyczne pary stylistyczne (robi / nie robi).
11. Adaptacja per kanał (jeśli korpus to pokrywa).
12. Pliki źródłowe analizy.

#### 6.2 `02-styleguide.md`

Działający styleguide do ręcznej pracy (wzorzec — `references/02-styleguide.template.md`):

1. 5 reguł rdzennych (niepodlegających dyskusji).
2. Frazy-kotwice z dawkowaniem (ile na 500 słów — żeby nie brzmieć parodyjnie).
3. Rytm — wzorzec długie/krótkie zdania z przykładami.
4. Hook — wzorce z konkretami.
5. Struktura wniosków / list.
6. Storytelling mikroskali (scenka → wniosek).
7. CTA — wzorce per kanał.
8. Sponsoring / mid-roll (jeśli dotyczy).
9. Lista do/don't — szybka ściąga.
10. Obowiązkowe sygnatury w dłuższych formach.
11. Co naprawiać w draftach AI „uciekających" w korpo (tabela typowych błędów + fixów).
12. Adaptacja per kanał.
13. Test końcowy — checklist „czy to brzmi jak X?".

#### 6.3 `03-system-prompt.md`

Execution-ready prompty (wzorzec — `references/03-system-prompt.template.md`):

- **Wersja A** — uniwersalna (główny ton bazy).
- **Wersja B** — adaptacja per kanał (jeśli korpus to obejmuje, np. „LinkedIn vs YT" albo „landing vs newsletter").
- Każda wersja: reguły rdzenne, frazy-kotwice, hooki, struktura wniosków, CTA, test końcowy.
- **Few-shot examples** — 2 realne mini-przykłady user→assistant w stylu osoby.
- Sekcja „Jak używać tych promptów" + sekcja „Aktualizacja stylu" (kiedy odświeżyć).

### Krok 7 — surowe dane razem z artefaktami

Skopiuj do `stylometria/`:
- `_stats.json` (output Pythona)
- `_stats-raw.md` (output Pythona)
- `_stylometry.py` (dostosowany skrypt — do reuse'u przy aktualizacji)

Surowy korpus (`transkrypcje-yt/` lub odpowiednik) — zostaw w nadrzędnym folderze (nie kopiuj, żeby nie duplikować).

### Krok 8 — zapis do pamięci projektu (jeśli kontekst)

Jeśli stylometria była robiona dla konkretnego klienta / projektu z istniejącą pamięcią (`~/.claude/projects/.../memory/MEMORY.md`):

1. Stwórz wpis `<nazwa>-stylometria.md` z opisem co jest, gdzie i kluczowymi sygnaturami stylu (3-5 punktów).
2. Dodaj linię do `MEMORY.md`.

Spytaj usera czy dodajesz (krótko, bez naciskania). Jeśli stylometria była dla samego Daniela / DBest Content — domyślnie tak.

### Krok 9 — raport podsumowujący

Na koniec zwróć Danielowi (lub użytkownikowi):

- **Lista plików** ze ścieżkami.
- **Skrócone summary** — rozmiar korpusu, najmocniejsza obserwacja (np. „275× «po prostu»").
- **Top 3 sygnatury stylu** — najsilniejsze odkrycia.
- **Kiedy odświeżyć** — propozycja kadencji (zwykle 6 miesięcy).

## Zasady i ograniczenia

- **Minimum korpusu:** 10 000 słów to próg sensowności. Poniżej rezultaty są zaszumione przez przypadkowe powtórzenia.
- **Jeden styl = jeden rejestr.** Nie mieszaj różnych rejestrów (np. e-mail prywatny + sales letter) bez tablicy adaptacji.
- **Transkrypty YT auto-caption** mają artefakty (np. „cloud" zamiast „Claude" jeśli ktoś mówi „kloud"). Zaznacz to w raporcie żeby agenci nie powielali pomyłki.
- **Nie zmyślaj liczb własnych osoby.** W styleguide i system promptach możesz dawać slot „LICZBA TUTAJ" / [LICZBA Z FIRMY] — agent generujący treści dostanie liczby od użytkownika.
- **Zero przedstawiania się** — jeśli korpus pochodzi z YouTuba, styleguide MA opisywać kiedy i jak osoba się przedstawia, ale system prompt powinien dawać slot „[INTRO/SKIP — decyzja zależy od formatu]".
- **Anti-em-dash:** w wynikowych dokumentach Daniela zawsze używaj półpauzy (–) ze spacjami, nigdy em-dasha (—).

## Output finalny

Folder `stylometria/` zawiera:

```
stylometria/
├── 01-raport-analityczny.md
├── 02-styleguide.md
├── 03-system-prompt.md
├── _stats.json
├── _stats-raw.md
└── _stylometry.py
```

Korpus (`transkrypcje-yt/` lub równoważnik) — w folderze nadrzędnym.

Pamięć projektu — opcjonalnie zaktualizowana.

## References

Pliki szablonowe leżą w `references/` obok SKILL.md:

- `stylometry.py.template` — skrypt analizujący (Python 3.10+, stdlib only).
- `fetch_youtube.ps1.template` — skrypt pobierający transkrypty YT z yt-dlp.
- `01-raport.template.md` — szablon raportu analitycznego.
- `02-styleguide.template.md` — szablon styleguide'u.
- `03-system-prompt.template.md` — szablon system prompta.

Czytaj te szablony przed pisaniem artefaktów — utrzymują spójność dokumentów między różnymi przebiegami skilla.
