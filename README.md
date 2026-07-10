# AI Ninjas – skille do pracy ze stylem tekstu

Skille do Claude Code do roboty z polskim tekstem: czyszczenie z sygnałów „stylu AI" (od skalpela po pełną przebudowę) i wyciąganie stylu konkretnej osoby czy marki, żeby AI pisało jej głosem, nie swoim.

## Co jest w środku

### /frazy-ai – skalpel

Chirurgiczne usuwanie fraz statystycznie nadużywanych przez LLM-y („warto zaznaczyć", „nie tylko... ale także", „kluczowy", em dashe). Zasada minimalnego diffa: tekst poza cięciami zostaje bajt w bajt taki sam. Każdy kandydat przechodzi przez bramkę weryfikacyjną (cytaty, nazwy własne, terminologia branżowa i frazy SEO są nietykalne), a na końcu dostajesz raport zmian z uzasadnieniem każdego cięcia.

W zestawie listy referencyjne: frazy polskie, frazy angielskie i konstrukcje składniowe niezależne od języka.

### /humanizacja – pełna przebudowa

Audyt tekstu pod kątem 13 kategorii sygnałów AI (monotonny rytm zdań, bezosobowość, miękka komunikacja, słowa-wytrychy, zaimkoza, kalki z angielskiego, trójki anaforyczne i inne), a potem przepisanie całości tak, żeby brzmiała jak napisana przez człowieka. Sens i długość zostają, styl się zmienia.

### /stylometria – odcisk palca stylu

Analiza stylu konkretnej osoby lub marki z jej tekstów (transkrypty YT, posty, artykuły, e-booki, wklejony tekst) i zapakowanie go w trzy gotowe do użycia artefakty: raport analityczny ze statystykami (n-gramy, TTR, rytm zdań, otwarcia i zamknięcia), styleguide do ręcznej pracy oraz execution-ready system prompt dla agentów piszących w tym głosie. Działa na pojedynczym pliku, folderze, URL-u albo wklejonym tekście – minimum użyteczne to ~10 000 słów, optimum 30 000+.

Skill jest self-contained (Python stdlib only) – nie potrzebuje folderu `_shared`. W zestawie templaty skryptu analizującego, pobieraczki transkryptów z YouTube i trzech dokumentów wyjściowych.

### _shared – polska typografia

Wspólne zasady typograficzne (cudzysłowy „", półpauza, kolejność interpunkcji przy cytatach) plus walidator w Pythonie, który programowo sprawdza tekst po edycji. Skill humanizacja z niego korzysta – bez tego folderu nie zadziała w całości.

## Instalacja

Skopiuj foldery do katalogu skilli Claude Code:

```
cp -r frazy-ai humanizacja stylometria _shared ~/.claude/skills/
```

Na Windows: `C:\Users\<user>\.claude\skills\`.

## Użycie

W Claude Code:

- `/frazy-ai` + wklejony tekst lub ścieżka do pliku – gdy chcesz tylko wyciąć AI-izmy, bez zmiany stylu,
- `/humanizacja` + wklejony tekst lub ścieżka do pliku – gdy tekst wymaga pełnej przebudowy stylistycznej,
- `/stylometria` + plik, folder, URL albo wklejony tekst – gdy chcesz wyciągnąć styl danej osoby/marki i dostać system prompt do pisania jej głosem.

Zasada kciuka: najpierw /frazy-ai. Jeśli po defrazowaniu tekst nadal brzmi jak AI (rytm metronomu, zero osobowości), dopiero wtedy /humanizacja.

## Autor

Daniel Bartosiewicz – [DBest Content](https://dbest-content.com). Skille powstały do codziennej pracy nad treściami, nie jako demo.
