# AI Ninjas – skille do odsztuczniania tekstów AI

Dwa skille do Claude Code, które czyszczą polskie teksty z sygnałów „stylu AI". Różnią się skalą ingerencji – od skalpela po pełną przebudowę.

## Co jest w środku

### /frazy-ai – skalpel

Chirurgiczne usuwanie fraz statystycznie nadużywanych przez LLM-y („warto zaznaczyć", „nie tylko... ale także", „kluczowy", em dashe). Zasada minimalnego diffa: tekst poza cięciami zostaje bajt w bajt taki sam. Każdy kandydat przechodzi przez bramkę weryfikacyjną (cytaty, nazwy własne, terminologia branżowa i frazy SEO są nietykalne), a na końcu dostajesz raport zmian z uzasadnieniem każdego cięcia.

W zestawie listy referencyjne: frazy polskie, frazy angielskie i konstrukcje składniowe niezależne od języka.

### /humanizacja – pełna przebudowa

Audyt tekstu pod kątem 13 kategorii sygnałów AI (monotonny rytm zdań, bezosobowość, miękka komunikacja, słowa-wytrychy, zaimkoza, kalki z angielskiego, trójki anaforyczne i inne), a potem przepisanie całości tak, żeby brzmiała jak napisana przez człowieka. Sens i długość zostają, styl się zmienia.

### _shared – polska typografia

Wspólne zasady typograficzne (cudzysłowy „", półpauza, kolejność interpunkcji przy cytatach) plus walidator w Pythonie, który programowo sprawdza tekst po edycji. Skill humanizacja z niego korzysta – bez tego folderu nie zadziała w całości.

## Instalacja

Skopiuj trzy foldery do katalogu skilli Claude Code:

```
cp -r frazy-ai humanizacja _shared ~/.claude/skills/
```

Na Windows: `C:\Users\<user>\.claude\skills\`.

## Użycie

W Claude Code:

- `/frazy-ai` + wklejony tekst lub ścieżka do pliku – gdy chcesz tylko wyciąć AI-izmy, bez zmiany stylu,
- `/humanizacja` + wklejony tekst lub ścieżka do pliku – gdy tekst wymaga pełnej przebudowy stylistycznej.

Zasada kciuka: najpierw /frazy-ai. Jeśli po defrazowaniu tekst nadal brzmi jak AI (rytm metronomu, zero osobowości), dopiero wtedy /humanizacja.

## Autor

Daniel Bartosiewicz – [DBest Content](https://dbest-content.com). Skille powstały do codziennej pracy nad treściami, nie jako demo.
