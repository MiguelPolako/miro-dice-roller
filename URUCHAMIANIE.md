# 🚀 Jak uruchomić Kostki RPG

## Każdorazowo przed użyciem w Miro (dev lokalny)

Otwórz Terminal (`⌘ + Spacja` → wpisz "Terminal" → Enter) i wpisz:

```
cd /Users/michalpolak/Documents/miro_roll
python3 -m http.server 3000
```

**Zostaw Terminal otwarty** — gdy go zamkniesz, aplikacja przestanie działać.

---

## Jak zatrzymać serwer

W oknie Terminala wciśnij: `Ctrl + C`

---

## Jak sprawdzić czy działa

Otwórz przeglądarkę i wejdź na:
```
http://localhost:3000
```
Jeśli widzisz listę plików (index.html, app.html) — działa ✅

---

## Używanie w Miro

1. Uruchom serwer (patrz wyżej)
2. Otwórz tablicę Miro
3. Kliknij ikonę 🎲 na lewym pasku → otwiera się panel
4. Wybierz kość (np. d20) lub wpisz notację (np. `2d6+3`) → kliknij **"Rzuć"**
5. Opcjonalnie: **"Wyślij wynik na tablicę"**, żeby był widoczny dla wszystkich

---

## Uwaga

Serwer lokalny (`python3 -m http.server`) musi być uruchomiony **za każdym razem**, gdy testujesz appkę pod `http://localhost:3000`.
Po wdrożeniu na GitHub Pages (patrz `SETUP.md`, krok 5) appka działa bez tego — pod stałym publicznym adresem.
