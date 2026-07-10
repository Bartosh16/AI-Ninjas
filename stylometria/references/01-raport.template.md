# Stylometria {NAZWA_OSOBY_LUB_MARKI} – raport analityczny

> Korpus: {N} tekstów ze źródła {ŹRÓDŁO}.
> Pobrane: {DATA}. Surowe pliki: `{ŚCIEŻKA_DO_KORPUSU}`.
> Charakter materiału: {MÓWIONY/PISANY/MIX}. Tematyka: {TEMATY}.

## 1. Rozmiar próby

| Wymiar | Wartość |
|---|---|
| Tekstów | {N} |
| Tokenów (słów) | {TOKENY} |
| Słów unikalnych (types) | {TYPES} |
| Type-Token Ratio (TTR) | {TTR} |
| Zdań | {ZDANIA} |
| Średnia długość zdania | {AVG} słów |
| Mediana długości zdania | {MEDIAN} słów |
| Pytania | {Q} ({Q_PCT}%) |
| Stwierdzenia | {DECL} ({DECL_PCT}%) |
| Słowa ≤4 znaki | {SHORT_PCT}% |
| Słowa 5-8 znaków | {MID_PCT}% |
| Słowa ≥9 znaków | {LONG_PCT}% |
| Zdania ≤5 słów | {VERY_SHORT_N} ({VERY_SHORT_PCT}%) |
| Zdania ≥30 słów | {LONG_N} ({LONG_PCT}%) |

Interpretacja:
- {INTERPRETACJA_TTR — porównaj do typowego mówionego PL ~0,14 / pisanego PL ~0,20}
- {INTERPRETACJA_RYTMU — co mówi różnica średnia/mediana}
- {INTERPRETACJA_PROSTOTY — % słów krótkich}
- {INTERPRETACJA_PYTAŃ — czy autor pyta czy stwierdza}

## 2. Frazy-kotwice (top n-gramów)

### Bigramy (top 20)
{TABELA bigramów z liczbami}

### Trigramy i dłuższe (signature)
{TABELA trigramów + 4-gramów + 5-gramów z interpretacją czym są te wzorce}

### Słowa-naczynia (top content words)
{LISTA 20-30 najczęstszych content words z interpretacją}

**Tematycznie** dominuje: {LISTA TEMATÓW z liczbami}

## 3. Otwarcia zdań (pierwsze 2 słowa)

{TABELA top 15 otwarć}

Wniosek: {CO TO MÓWI O STYLU — czy filler-otwarcia dominują, czy autor wchodzi w meritum}

## 4. Zakończenia zdań (ostatnie 2 słowa)

{TABELA top 10 zakończeń}

Wniosek: {jakie kotwice/wytrychy zamyka — np. „cześć na razie pa", „dziękuję za uwagę", „pozdrawiam"}

## 5. Wzorzec otwarcia materiału

Z analizy pierwszych 3 zdań w {N} materiałach wyłaniają się {LICZBA} typów hooka:

1. **{NAZWA_TYPU_A}:** „{PRZYKŁAD}"
2. **{NAZWA_TYPU_B}:** „{PRZYKŁAD}"
3. **{NAZWA_TYPU_C}:** „{PRZYKŁAD}"
4. **{NAZWA_TYPU_D}:** „{PRZYKŁAD}"

We wszystkich typach hook to **{N} zdań, łącznie ~{M} słów**. {DODATKOWA_OBSERWACJA np. czy autor przedstawia się od razu, czy dopiero po hooku}.

## 6. Wzorzec zakończenia materiału

{OPIS TEMPLATE'U OUTRA z elementami:}
- {ELEMENT_1}
- {ELEMENT_2}
- {ELEMENT_3}

{OBSERWACJA czy to jest stała formuła czy luźny szkielet}

## 7. Mid-roll i sponsoring (jeśli dotyczy)

{OPIS — jak partner się pojawia, w którym momencie, jak wraca pod koniec, jaką ma strukturę}

## 8. Anatomia stylu – {N} sygnatur

1. **{NAZWA_SYGNATURY_1}.** {OPIS w 2-3 zdaniach z konkretną liczbą z korpusu}
2. **{NAZWA_SYGNATURY_2}.** {OPIS}
3. **{NAZWA_SYGNATURY_3}.** {OPIS}
4. ...
{minimum 5, optimum 10 sygnatur}

## 9. Czego {OSOBA} NIE robi w {ROZMIAR}

- **Nie używa {X}.** {DOWÓD — 0 lub bardzo mało wystąpień w korpusie}
- **Nie {Y}.** {DOWÓD}
- **Nie {Z}.** {DOWÓD}

{UWAGA O ARTEFAKTACH AUTO-TRANSKRYPCJI jeśli dotyczy — np. „cloud" zamiast „Claude"}

## 10. Charakterystyczne pary stylistyczne (do/avoid)

| Robi (charakter) | Nie robi (poza rejestrem) |
|---|---|
| {PRZYKŁAD_1A} | {PRZYKŁAD_1B} |
| {PRZYKŁAD_2A} | {PRZYKŁAD_2B} |
| {PRZYKŁAD_3A} | {PRZYKŁAD_3B} |
| ... | ... |
{minimum 8 par}

## 11. Adaptacja per kanał (jeśli korpus to pokrywa)

| Aspekt | {KANAŁ_A} | {KANAŁ_B} |
|---|---|---|
| Relacja | {OPIS} | {OPIS} |
| Otwarcia | {OPIS} | {OPIS} |
| CTA | {OPIS} | {OPIS} |
| Język liczb | {OPIS} | {OPIS} |

Wniosek: {KIEDY KORZYSTAĆ Z BAZY, KIEDY ADAPTOWAĆ}

## 12. Pliki źródłowe analizy

- `_stylometry.py` – skrypt liczący (Python 3.10+, stdlib only)
- `_stats.json` – surowe statystyki
- `_stats-raw.md` – czytelny dump statystyk z próbkami
- `{KORPUS_DIR}/` – surowe teksty
