# 🎲 Kostki RPG — Instrukcja konfiguracji

> Krok po kroku, bez znajomości programowania

---

## Krok 1: Pliki są gotowe ✅

W tym folderze masz już wszystkie potrzebne pliki:
- `index.html` — ładowany przez Miro w tle
- `app.html` — panel z przyciskami i logiką rzutów
- `SETUP.md` — ta instrukcja

---

## Krok 2: Uruchom lokalny serwer (do testów)

Otwórz **Terminal** (Mac: naciśnij `⌘ + Spacja`, wpisz "Terminal", Enter):

```bash
cd /Users/michalpolak/Documents/miro_roll
python3 -m http.server 3000
```

**Zostaw okno Terminala otwarte** — aplikacja działa pod adresem:
👉 `http://localhost:3000`

Żeby zatrzymać serwer: wciśnij `Ctrl + C` w Terminalu.

---

## Krok 3: Zarejestruj aplikację w Miro

1. Wejdź na **miro.com** → zaloguj się
2. Kliknij swoje **zdjęcie profilowe** (prawy górny róg) → **Ustawienia**
3. Kliknij zakładkę **"Your apps"** po lewej stronie
4. Kliknij **"+ Create new app"**
5. Wpisz nazwę: **Kostki RPG** → kliknij **"Create app"**
6. Kliknij **"Edit in Manifest"** (otwiera się edytor tekstowy)
7. **Usuń cały tekst** i wklej poniższy kod:

```yaml
appName: Kostki RPG
sdkVersion: SDK_V2
sdkUri: http://localhost:3000
scopes:
  - boards:read
  - boards:write
  - identity:read
```

8. Kliknij **"Save"**
9. Przewiń stronę w dół → kliknij **"Install app and get OAuth token"**
10. Wybierz swój zespół → **"Install & authorize"** → **"Close"**

---

## Krok 4: Użyj aplikacji na tablicy

1. Otwórz dowolną tablicę Miro
2. Po lewej stronie znajdź ikonę **🎲**
   - Jeśli jej nie widać: kliknij **"..."** lub **"+ More apps"** na dole paska
3. Kliknij ikonę → po prawej stronie otwiera się panel **"Kostki RPG"**

### Jak rzucić kośćmi:
4. Kliknij typ kości (np. **d20**) lub wpisz notację ręcznie (np. `2d6+3`)
5. Kliknij **"🎲 Rzuć"**
6. Wynik pojawi się w panelu — a jeśli inni gracze też mają otwarty panel, zobaczą Twój rzut w logu na żywo

### Żeby wynik był widoczny na tablicy (nawet bez otwartego panelu):
7. Po rzucie kliknij **"📌 Wyślij wynik na tablicę"**

---

## Krok 5: Wgraj na stałe — GitHub Pages (darmowe, automatyczny redeploy)

> Ten krok sprawi, że aplikacja będzie działać ZAWSZE,
> nawet gdy Twój komputer jest wyłączony — a każda przyszła zmiana
> w kodzie sama się wgra po `git push`.

1. Utwórz **publiczne** repozytorium na [github.com/new](https://github.com/new)
2. Wypchnij ten folder do repozytorium (`git push`)
3. W repozytorium: **Settings → Pages → Source: Deploy from a branch → main / (root)**
4. Poczekaj ~1 minutę → adres appki:
   ```
   https://<twoj-login>.github.io/<nazwa-repo>/
   ```

### Zaktualizuj Miro:
5. Wróć do **miro.com** → Ustawienia → Your apps → **Kostki RPG** → **Edit in Manifest**
6. Zmień `http://localhost:3000` na swój URL z GitHub Pages:
   ```yaml
   sdkUri: https://twoj-login.github.io/nazwa-repo
   ```
7. Kliknij **"Save"**
8. Ponownie kliknij **"Install app and get OAuth token"** → **Install & authorize**

✅ Gotowe! Aplikacja działa na stałe dla całego Twojego zespołu, a kolejne zmiany w kodzie wystarczy wypchnąć przez `git push`.

---

## Często zadawane pytania

**Q: Chcę żeby moi znajomi (spoza mojego teamu w Miro) też mogli używać appki.**
A: Dodaj ich do tego samego teamu w Miro (workspace), w którym zainstalowałeś aplikację — wtedy zobaczą ikonę 🎲 na pasku bez dodatkowej konfiguracji.

**Q: Notacja "500d20" zawiesza appkę?**
A: Nie — appka ma wbudowany limit 100 kości na rzut i pokaże czytelny komunikat błędu zamiast się zawieszać.

**Q: Skąd appka wie kto rzucał?**
A: Z uprawnienia `identity:read` — pobiera imię aktualnie zalogowanego użytkownika Miro (`miro.board.getUserInfo()`) i podpisuje nim wpis w logu.
