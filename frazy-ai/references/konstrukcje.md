# Konstrukcje i sygnały typograficzne AI (niezależne od języka)

To wzorce zdaniowe, nie pojedyncze słowa. Wykrywasz je po kształcie zdania. Napraw = przepisz zdanie najprościej jak się da, zachowując pełne znaczenie. Zmiana dotyczy TYLKO zdania z wzorcem.

## Konstrukcje zdaniowe

### Contrastive framing (4+ źródeł)
Wzorzec: "It's not X, it's Y" / "To nie X, to Y" / "Not just X, but Y".
Sygnał AI: sztuczna analityczność i dramatyzm. Jedno takie zdanie w tekście może zostać, jeśli kontrast jest realny. Dwa i więcej = maniera, przepisz na zdania oznajmujące mówiące, czym rzecz JEST.

### Throat-clearing / meta-zapowiedzi
Wzorzec: zdanie informuje, co tekst zaraz zrobi, zamiast to robić ("Przyjrzyjmy się teraz...", "Let's explore...").
Napraw: usuń zdanie, treść zaczyna się od sedna.

### Summary closer / mic drop
Wzorzec: każdy akapit kończy się wymuszonym mini-podsumowaniem lub wzniosłą puentą ("Ultimately...", "Warto więc pamiętać...").
Napraw: usuń puentę – akapit kończy się ostatnią informacją. Jeśli KAŻDY akapit tak się kończy, to silny sygnał: tnij wszystkie, nie co drugą.

### Hedging chain
Wzorzec: łańcuchy asekuracji w jednym fragmencie ("może", "w niektórych przypadkach", "nie zawsze", "zaleca się" / "one might argue", "generally speaking").
Napraw: zostaw JEDNO zastrzeżenie, jeśli merytorycznie zasadne; resztę tnij. Nie zamieniaj asekuracji na kategoryczność, której autor nie deklarował – jeśli teza naprawdę jest niepewna, hedging zostaje.

### Forced sass / pytania retoryczne
Wzorzec: "The result?" / "Here's the deal:" / "Hot take:" / "Efekt?".
Napraw: połącz w zwykłe zdanie ("The result? More sales." → "The result was more sales."). Pojedyncze użycie w luźnym blogu może zostać.

### Rule of three (triady)
Wzorzec: przymiotniki/korzyści/argumenty obsesyjnie grupowane w trójki ("fast, reliable, and affordable" / "szybko, sprawnie i efektywnie").
Napraw: gdy triada jest ozdobna – zostaw 1–2 najmocniejsze elementy. Gdy trzy elementy to trzy realne, różne informacje – zostaw. Jedna triada w tekście = norma; triada w co drugim akapicie = maniera.

### "No X. No Y. Just Z."
Wzorzec: szablon skrótowych puent ("No fluff. No filler. Just results.").
Napraw: przepisz na jedno zwykłe zdanie albo usuń.

### Setup-payoff
Wzorzec: naprzemiennie bardzo krótkie zdanie deklaratywne + długie wyjaśnienie, w kółko.
To sygnał do raportu (rytm to działka /humanizacja) – NIE przepisuj rytmu w tym skillu, tylko odnotuj.

### Listicle formatting
Wzorzec: każdy punkt listy w formacie "**Pogrubiony nagłówek z dwukropkiem:** zdanie wyjaśnienia".
Napraw tylko gdy user prosił o pracę nad formatowaniem; domyślnie odnotuj w raporcie.

## Sygnały typograficzne

### Em-dash abuse
Wzorzec: em dash (—) wciskany w środek zdań dla emfazy ("The result—an undeniable success").
Napraw w PL: zawsze zamieniaj — na półpauzę ze spacjami ( – ). Napraw w EN: nadmiarowe em dashe → przecinek lub kropka; jedno zasadne użycie na tekst może zostać.

### Comma splice z imiesłowem
Wzorzec (głównie EN): "...processes the data, revealing key insights" – zdanie doklejone przecinkiem i imiesłowem. U AI 2–5x częściej niż u ludzi.
Napraw: rozbij na dwa zdania albo "which reveals...". W PL analogicznie nadużywane "...co pozwala na...", "...umożliwiając...".

### Cudzysłowy typograficzne w kontekstach technicznych
Wzorzec: zakrzywione “ ” wewnątrz fragmentów kodu, komend, configów.
Napraw: w kodzie/komendach proste " ". W polskiej prozie „" są POPRAWNE – nie ruszaj.

## Czego ten skill NIE naprawia (odnotuj w raporcie, wskaż /humanizacja)

- monotonny rytm zdań (low burstiness), "prostokątne akapity",
- brak osobowości, doświadczeń, konkretów autora ("The I Factor"),
- przesadnie perfekcyjna, mechaniczna poprawność całości,
- struktura dokumentu (nagłówek-tekst-nagłówek-tekst).

Jeśli tekst ma te problemy w dużym natężeniu, napisz w raporcie: "Tekst po defrazowaniu nadal będzie brzmiał jak AI – rekomendacja: /humanizacja".
