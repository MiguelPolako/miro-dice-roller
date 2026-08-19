# 📓 Dziennik projektu — Kostki RPG (Miro Dice Roller)

Historia decyzji i zmian w tym projekcie — dla pamięci, gdybyśmy wrócili do tego za jakiś czas.

## Cel

Dodatek do Miro do rzucania kośćmi RPG bezpośrednio na tablicy: presety (d4–d100) albo pełna notacja RPG (`2d6+3`, `4d6kh3`, advantage/disadvantage), z wynikiem widocznym na żywo dla wszystkich uczestników sesji.

## Kluczowe decyzje architektoniczne

Podjęte na starcie, przez wybór spośród opcji:

1. **Brak backendu.** Multiplayer działa przez `miro.board.storage` (live sync między otwartymi panelami — subskrypcja `onValue`) + opcjonalne wysyłanie wyniku jako element na tablicę. Alternatywa (własny serwer + WebSocket, jak projekt `dice.io`) odrzucona jako niepotrzebnie skomplikowana — Miro storage wystarcza.
2. **Pełna notacja RPG**, nie tylko proste presety. Użyta biblioteka [`@dice-roller/rpg-dice-roller`](https://dice-roller.github.io/documentation/) v5.5.1, ładowana z CDN (jsDelivr/unpkg) bez build toola. Wymaga w konkretnej kolejności: `mathjs@11.8.2` → `random-js@2.1.0` → `rpg-dice-roller` (global `rpgDiceRoller`).
3. **Prosta animacja liczb** (flicker ~8 tików co 70ms) zamiast animowanych kości 3D — szybsze do zbudowania, zero dodatkowych zależności.
4. Architektura plikowa 1:1 wzorowana na wcześniejszym projekcie użytkownika **Fog of War** (`~/Downloads/fog-of-war-miro-main`): `index.html` (init + `icon:click`) + `app.html` (cały UI/logika), zero build toola, ten sam styl wizualny (akcent `#4262ff`).

## Hosting i deploy

- Wybrany **GitHub Pages** zamiast Netlify Drop — bo chcieliśmy automatyczny redeploy przy `git push`, nie ręczne przeciąganie folderu za każdą zmianą.
- Repo: **https://github.com/MiguelPolako/miro-dice-roller** (publiczne — wymóg darmowego GitHub Pages)
- Live URL: **https://miguelpolako.github.io/miro-dice-roller/**
- Pages: Settings → Pages → Deploy from a branch → `main` / `(root)`
- Autoryzacja do pusha: użytkownik wygenerował Personal Access Token na GitHubie i użył go **tylko lokalnie we własnym Terminalu** (świadomie NIE wklejany na czacie — token to jak hasło, a chat trafia do historii konwersacji).
- Lokalny git identity w tym repo: `Michal Polak` / `michal.polak0@gmail.com`.

## Napotkane problemy i jak je rozwiązaliśmy

- **Pusta strona pod live URL** — to nie błąd. `index.html` ma celowo pusty `<body>`, bo Miro ładuje go w tle tylko po to, żeby nasłuchiwać `icon:click`. Cały widoczny UI jest w `app.html`, otwieranym jako panel dopiero po kliknięciu ikony w Miro.
- **404 "There isn't a GitHub Pages site here" przy klikaniu ikony w Miro** — okazało się być cache przeglądarki/Miro (w oknie incognito działało od razu). Przy okazji naprawiliśmy też realny potencjalny błąd: `openPanel({ url: 'app.html' })` używał względnego URL-a, który źle się wyliczał, gdyby `sdkUri` w manifeście nie miał ukośnika na końcu. Teraz `index.html` liczy pełny adres panelu przez `new URL('app.html', document.baseURI)` — działa niezależnie od tego, jak zapisany jest `sdkUri`.
- **Konflikt nazwy appki** — użytkownik miał już dwie inne appki "Dice Roller" w Miro. Zmieniliśmy `appName` (i UI: `<title>`, `<h1>`) na **"Kostki RPG"**.

## Iteracje funkcji (po pierwszej wersji)

- **"Wyślij wynik na tablicę"** — pierwotnie zawsze tworzył nowy element tekstowy w losowym miejscu koło środka widoku. Zmienione: jeśli w momencie kliknięcia zaznaczony jest dokładnie jeden element `text`/`sticky_note` na tablicy, wynik jest **dopisywany** do niego (przez `item.content += ...; item.sync()`), zamiast tworzyć nowy element. Fallback (tworzenie nowego) zostaje dla przypadku braku zaznaczenia lub zaznaczenia wielu elementów.
- **Treść wysyłana na tablicę uproszczona do samej liczby** — początkowo wysyłany był pełny breakdown (`🎲 Michał: 2d20kl1+2 → [18d, 4]+2 = 6`), na życzenie użytkownika zmienione na sam wynik końcowy (`6`) — czyściej wygląda np. w initiative trackerze. Panel dalej pokazuje pełny breakdown, zmieniło się tylko to, co ląduje na tablicy/w dopisywanym tekście.

## Znane ograniczenie platformy (nie do naprawienia po naszej stronie)

**Natywne tabele Miro (widget "Table") nie są zapisywalne przez Web SDK** — potwierdzone przez pracowników Miro na forum community, dotyczy zarówno SDK v1 jak i v2. Apki mogą jedynie odczytać pozycję/wymiary takiej tabeli albo ją usunąć — nie da się programowo dopisać/zmienić tekstu w komórce. Dlatego "Wyślij wynik na tablicę" nie zadziała, jeśli zaznaczona jest komórka natywnej tabeli.

**Workaround** (jeszcze niezaimplementowany, do rozważenia): zamiast natywnej tabeli, zbudować siatkę osobnych elementów tekstowych/sticky notes imitującą tabelę (np. tracker inicjatywy: imię | wynik) — takie elementy SĄ zapisywalne, więc obecny mechanizm dopisywania zadziała na nich bez zmian.

## Struktura plików

- `index.html` — inicjalizacja SDK, `icon:click` → `openPanel`
- `app.html` — cały UI i logika (presety, notacja, rzut, animacja, log live, wysyłanie na tablicę)
- `README.md` — opis projektu
- `SETUP.md` — pełna instrukcja rejestracji appki w Miro i deployu (dla osoby nietechnicznej)
- `URUCHAMIANIE.md` — skrócona instrukcja lokalnego dev servera
- `.gitignore`

## Zabezpieczenia w kodzie

- Limit **100 kości na rzut** (regex liczący `NdX` w notacji) — chroni przed zawieszeniem appki przez np. `1000d20`.
- `escapeHtml()` na danych wstawianych jako HTML (log, dopisywana treść tekstu) — dane wejściowe to imię z Miro i notacja, ale i tak escapowane dla bezpieczeństwa.
