# 🎲 Kostki RPG — dodatek do Miro

Dodatek (plugin) do Miro do rzucania kośćmi RPG bezpośrednio na tablicy — presety (d4–d100) albo pełna notacja (`2d6+3`, `4d6kh3`, advantage/disadvantage), z logiem rzutów widocznym na żywo dla wszystkich uczestników.

## Funkcjonalność

- **Presety kości** — d4, d6, d8, d10, d12, d20, d100 + liczba kości
- **Notacja RPG** — pole tekstowe akceptuje pełną składnię (`2d6+3`, `4d6kh3`, `2d20kh1`...)
- **Advantage / Disadvantage** — jednym kliknięciem dla d20
- **Animacja rzutu** — krótkie "losowanie" cyfr przed pokazaniem wyniku
- **Log rzutów na żywo** — widoczny dla wszystkich, którzy mają panel otwarty (synchronizacja przez `miro.board.storage`)
- **Wyślij wynik na tablicę** — stawia wynik jako element tekstowy widoczny dla wszystkich, nawet bez otwartego panelu

## Pliki

- `index.html` — inicjalizacja Miro SDK, otwiera panel po kliknięciu ikony
- `app.html` — cały UI i logika dodatku

## Jak zacząć

Pełna instrukcja krok po kroku znajduje się w [`SETUP.md`](./SETUP.md).

## Technologia

- Miro SDK v2 (`https://miro.com/app/static/sdk/v2/miro.js`)
- Parser notacji: [`@dice-roller/rpg-dice-roller`](https://dice-roller.github.io/documentation/) (z CDN, bez build toola)
- Log rzutów trzymany w `miro.board.storage`, dzięki czemu aktualizuje się na żywo u wszystkich uczestników
- Hosting: GitHub Pages (statyczne pliki, auto-deploy przy każdym `git push`)
