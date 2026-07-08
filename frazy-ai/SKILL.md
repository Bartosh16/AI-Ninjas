---
name: frazy-ai
description: Chirurgicznie usuwa z tekstu najpopularniejsze frazy i słowa nadużywane przez AI (PL i EN) – z krytyczną dokładnością, zero fałszywych pozytywów, minimalny diff. ZAWSZE używaj gdy użytkownik mówi "usuń frazy AI", "wytnij frazy AI", "defrazuj", "wyczyść tekst z fraz AI", "usuń słowa-wytrychy", "przeczyść z GPT-izmów ale nie przepisuj", "usuń delve/tapestry/kluczowy", albo wkleja tekst i prosi o usunięcie typowych fraz AI BEZ pełnego przepisywania stylu. RÓŻNICA od /humanizacja - humanizacja przepisuje cały tekst (rytm, osobowość, styl), frazy-ai robi wyłącznie punktowe cięcia fraz i zostawia resztę tekstu nietkniętą. Gdy user chce pełnej humanizacji lub mówi "shumanizuj" - użyj /humanizacja, nie tego skilla.
---

# Frazy AI – chirurgiczne usuwanie

Usuwasz z tekstu frazy statystycznie nadużywane przez LLM-y. To NIE jest humanizacja ani przepisywanie – to praca skalpelem. Każde cięcie musi być uzasadnione, a tekst poza cięciami zostaje bajt w bajt taki sam.

## Dlaczego krytyczna dokładność

Trzy fakty, które muszą sterować każdą decyzją:

1. **Słowa z list nie są nielegalne.** Najbardziej nadużywane frazy występują u AI 10–50x częściej niż u ludzi (delve 48x, tapestry 35x, "it's worth noting" 31x, "kluczowy" analogicznie w PL). To znaczy, że u człowieka też występują – tylko rzadko. Pojedyncze, trafne użycie słowa z listy w długim tekście jest normalne. Sygnałem AI jest **zagęszczenie**, nie samo wystąpienie.
2. **Mechaniczna podmiana synonimów niszczy tekst.** Zamiana "ważny" na "doniosły" tworzy sałatkę składniową gorszą od oryginału. Hierarchia edycji: **usuń całkowicie > skróć > zamień na słowo PROSTSZE**. Nigdy nie zamieniaj na słowo rzadsze lub bardziej wyszukane.
3. **Fałszywy pozytyw jest gorszy niż przepuszczona fraza.** Zepsuty cytat, zmienione znaczenie zdania albo wycięty termin branżowy to realna szkoda. Przepuszczona jedna fraza AI to kosmetyka. Gdy masz wątpliwość – zostaw i odnotuj w raporcie, zamiast ciąć.

## Workflow

### Krok 1: Przeczytaj cały tekst

Zanim cokolwiek zmienisz, przeczytaj całość i ustal: temat, rejestr (blog / akademicki / sprzedażowy), język (PL / EN / mieszany), czy są cytaty, nazwy własne, terminologia branżowa, frazy SEO. W tekstach akademickich i technicznych słownictwo formalne ("robust", "strukturalny", "istotny") jest naturalne – podnieś próg cięcia.

### Krok 2: Skan kandydatów

Załaduj odpowiednie listy referencyjne:

- [references/frazy-pl.md](references/frazy-pl.md) – polskie słowa i frazy (dla tekstów PL),
- [references/frazy-en.md](references/frazy-en.md) – angielskie słowa i frazy (dla tekstów EN),
- [references/konstrukcje.md](references/konstrukcje.md) – konstrukcje składniowe i sygnały typograficzne (zawsze, niezależnie od języka).

Oznacz każde wystąpienie kandydata z jego tierem (1/2/3 – w plikach referencyjnych).

### Krok 3: Bramka weryfikacyjna – każdy kandydat osobno

Kandydat przechodzi do edycji TYLKO jeśli zaliczy wszystkie testy:

| Test | Pytanie | Jeśli NIE → |
|---|---|---|
| Cytat | Czy fraza jest poza cytatem, przytoczeniem, tytułem dzieła? | zostaw (cytaty są nietykalne) |
| Nazwa | Czy to nie jest część nazwy własnej, firmy, produktu, stanowiska? | zostaw |
| Kod / dane | Czy fraza jest poza blokiem kodu, URL-em, danymi technicznymi? | zostaw |
| Terminologia | Czy to nie jest poprawnie użyty termin branżowy w swoim kontekście (np. "kluczowy klucz API" w tekście o kryptografii, "landscape" w tekście o fotografii)? | zostaw |
| SEO | Czy to nie jest fraza kluczowa wskazana przez usera lub oczywista fraza pozycjonowana (H1/H2, tytuł)? | zostaw |
| Informacja | Czy po usunięciu/zmianie zdanie zachowa CAŁE znaczenie? | przepisz minimalnie zamiast usuwać, albo zostaw |
| Zagęszczenie (tylko tier 3) | Czy słowo występuje częściej niż raz-dwa razy w tekście, albo brzmi pusto w swoim zdaniu? | zostaw pojedyncze trafne użycie |

Tier 1 (czysty wypełniacz typu "warto zaznaczyć, że", "w dzisiejszym dynamicznym świecie") – tnij przy pierwszym wystąpieniu, o ile przechodzi testy cytatu/nazwy/kodu.
Tier 2 – tnij, gdy fraza nie niesie informacji albo występuje wielokrotnie.
Tier 3 – to sygnały, nie wyroki. Redukuj zagęszczenie (zostaw 0–1 wystąpień), nie eksterminuj.

### Krok 4: Edycja – zasada minimalnego diffa

- Zmieniaj wyłącznie zakwalifikowane frazy. Sąsiednie zdania dotykasz tylko, gdy wymaga tego gramatyka po cięciu (np. dostosowanie wielkiej litery, spójnika).
- Najczęstsza poprawna edycja to **usunięcie bez zamiennika**: "Warto zaznaczyć, że system działa" → "System działa".
- Przy konstrukcjach (kontrast "to nie X, to Y", triady, mikropodsumowania) przepisz zdanie najprościej jak się da, zachowując znaczenie.
- NIE zmieniaj: rytmu pozostałych zdań, słownictwa poza frazami, struktury akapitów, formatowania. To robota /humanizacja, nie twoja.
- Typografia w tekstach PL: em dash (—) zamieniaj na półpauzę ze spacjami ( – ). W tekstach EN nadużyte em dashe w środku zdań zamieniaj na przecinek lub kropkę, ale pojedyncze poprawne użycie zostaw.

### Krok 5: Autokontrola przed oddaniem

Przeczytaj wynik i sprawdź:

1. Czy każde zdanie po edycji znaczy dokładnie to samo co przed?
2. Czy żaden cytat, nazwa, liczba, fakt nie został naruszony?
3. Czy nie wprowadziłeś słowa rzadszego niż to, które wyciąłeś?
4. Test na głos: czy poprawione zdania brzmią jak coś, co człowiek powiedziałby znajomemu?

Jeśli którykolwiek punkt nie przechodzi – cofnij tę konkretną zmianę.

## Format wyjścia

Gdy input to plik – edytuj plik. Gdy input to wklejony tekst – zwróć czysty tekst w bloku. Zawsze dołącz raport:

```
## Raport zmian

| # | Przed | Po | Powód |
|---|-------|-----|-------|
| 1 | "Warto zaznaczyć, że..." | (usunięte) | tier 1, pusty wypełniacz |

**Zostawione celowo:**
- "kluczowy" w cytacie Jana Kowalskiego – cytaty nietykalne
- "landscape" (1x) – tekst o fotografii, termin użyty poprawnie

Zmian: N | Kandydatów odrzuconych przez bramkę: M
```

Sekcja "Zostawione celowo" jest obowiązkowa, gdy bramka odrzuciła jakiegokolwiek kandydata – user musi widzieć, że decyzja była świadoma, a nie przeoczona.
