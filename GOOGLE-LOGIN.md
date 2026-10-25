# Logowanie Google — instrukcja krok po kroku (ok. 3 minuty)

Cel: po tych krokach kliknięcie **„Google"** w logowaniu otwiera **normalne okno Google
z wyborem konta**. Robisz to **raz** — potem już nigdy tu nie wracasz.

Gotowy Client ID wklej mi w czacie, ja wpiszę go do `tools/config.json`,
przebuduję stronę i wyślę nowy `qcurx-site.zip`.

---

## Krok 1 — wejdź do Google Cloud

Otwórz: **https://console.cloud.google.com/**

Zaloguj się na swoje konto Google (to samo, na którym chcesz mieć projekt).
Zaakceptuj regulamin, jeśli wyskoczy.

## Krok 2 — utwórz projekt

- U góry kliknij listę projektów (obok logo „Google Cloud") → **New project / Nowy projekt**
- Nazwa: `QcurX` (cokolwiek)
- **Create / Utwórz** → poczekaj kilka sekund → wybierz ten projekt z listy u góry

> Projekt to tylko „pudełko" na konfigurację. Nic nie kosztuje, karta nie jest potrzebna.

## Krok 3 — „Utwórz element marki" (ekran, który widzisz)

Google zmienił interfejs: zamiast „OAuth consent screen" jest teraz
**Platforma uwierzytelniania Google → Elementy marki**.

Masz przed sobą kreator z 4 sekcjami:

**1. Informacje o aplikacji**
| pole | co wpisać |
|---|---|
| Nazwa aplikacji | `QcurX` |
| Adres e-mail do użytkowników potrzebujących pomocy | wybierz swój e-mail z listy |

→ **Dalej**

**2. Odbiorcy** — wybierz **Zewnętrzni** (nie „Wewnętrzni") → **Dalej**

**3. Dane kontaktowe** — wpisz swój e-mail → **Dalej**

**4. Zakończ** — zaznacz zgodę z warunkami → **Utwórz**

> Baner u góry o „300 USD" i przycisk **Zacznij bez opłat** możesz zignorować —
> logowanie Google jest darmowe i **karta płatnicza nie jest potrzebna**.

## Krok 4 — ⚠️ wyłącz tryb testowy

To najczęstszy powód „czemu koledzy nie mogą się zalogować".

W lewym menu → **Odbiorcy** (sekcja *Publiczność*) → **Status publikacji** →
kliknij **Opublikuj aplikację** → potwierdź.

Ma być: **W wersji produkcyjnej / In production**.

W trybie „Testowanie" logują się **tylko** konta z listy testerów (max 100).
W trybie produkcyjnym może się zalogować **każdy** — Google nie wymaga weryfikacji,
bo prosimy tylko o podstawowe dane (e-mail, imię, zdjęcie).

## Krok 5 — utwórz Client ID

Menu ☰ → **Klienci** (sekcja *Dostęp do danych* / *Klienci*) →
**+ Utwórz klienta** → **Typ aplikacji: Aplikacja internetowa**

- **Nazwa:** `QcurX web`

### Authorized JavaScript origins

Kliknij **+ Add URI** i dodaj **dwa** adresy:

```
http://localhost:8000
https://TWOJ-LOGIN.github.io
```

⚠️ Ważne:
- **bez** ukośnika na końcu i **bez** nazwy repozytorium — dla GitHub Pages
  origin to samo `https://login.github.io`, nawet jeśli strona jest pod `/repo/`
- jeśli później podłączysz własną domenę, dopiszesz tu drugi wiersz, np. `https://qcurx.pl`

### Authorized redirect URIs

**Nic nie wpisuj** — nasze logowanie działa w okienku (popup), bez przekierowań.

## Krok 6 — skopiuj Client ID

Kliknij **Utwórz**. Wyskoczy okienko z **Client ID** — wygląda tak:

```
123456789012-abcdefghijklmnopqrstuvwxyz123456.apps.googleusercontent.com
```

Skopiuj go (przycisk „Copy”). **Client Secret nie jest potrzebny** — nie wysyłaj go nikomu.
Możesz go zignorować.


## Twoje adresy (skopiuj do Google Cloud)

| gdzie w Google Cloud | co wkleić |
|---|---|
| **Autoryzowane źródła JavaScript** (Klienci → Twój klient) | `http://localhost:8000` |
| tamże, drugi wiersz | `https://qiuxproject.github.io` |
| **Strona główna aplikacji** (Elementy marki) | `https://qiuxproject.github.io/QcurX-Site/` |
| **Link do polityki prywatności** (Elementy marki) | `https://qiuxproject.github.io/QcurX-Site/prywatnosc.html` |
| **Autoryzowane domeny** (Elementy marki) | `qiuxproject.github.io` |
| **Dane kontaktowe** (Elementy marki) | Twój e-mail |

⚠️ W *Autoryzowanych źródłach JavaScript* podaj **sam origin** —
`https://qiuxproject.github.io`, **bez** `/QcurX-Site/` i bez ukośnika na końcu.
Nazwa repozytorium nie wchodzi do originu.

⚠️ W *Autoryzowanych identyfikatorach URI przekierowania* — **nic nie wpisuj**.

---

## Krok 7 — wyślij mi Client ID

Wklej go w czacie. Ja:

1. wpisuję go do `tools/config.json`
2. uruchamiam `python3 tools/build.py`
3. wysyłam Ci nowy `qcurx-site.zip`

Na stronie pojawi się wtedy prawdziwy przycisk Google, który otwiera
**okno wyboru konta Google**. Odwiedzający nie widzą żadnych kluczy.

---

## Jak to sprawdzić, zanim opublikujesz

1. Serwer działa: `python3 -m http.server 8000` w folderze strony
2. Otwórz `http://localhost:8000`
3. Kliknij **Dodaj kursor** → zakładka **Google**
4. Powinno wyskoczyć okno Google z listą Twoich kont

Jeśli wyskoczy błąd `origin_mismatch` — adres w przeglądarce nie zgadza się
z tym, co wpisałeś w *Authorized JavaScript origins*. Sprawdź, czy nie ma
ukośnika na końcu i czy port to dokładnie `8000`.

---

## Publikacja na GitHub Pages

1. Utwórz repozytorium, np. `qcurx`
2. Wgraj **zawartość** zipa (`index.html`, `pack.html`, folder `p/`, `.nojekyll`)
3. **Settings → Pages → Source: Deploy from a branch → main / (root)**
4. Po chwili strona działa pod `https://TWOJ-LOGIN.github.io/qcurx/`

Potem wróć do **Krok 5** i upewnij się, że w *Authorized JavaScript origins*
jest `https://TWOJ-LOGIN.github.io` (sam login, bez `/qcurx`).

> `.nojekyll` w zipie jest po to, żeby GitHub Pages nie przetwarzał plików przez Jekylla.

---

## Najczęstsze problemy

| objaw | przyczyna | rozwiązanie |
|---|---|---|
| `origin_mismatch` | adres strony ≠ origin w Google Cloud | dopisz dokładny origin (bez `/` na końcu) |
| nie ma sekcji „Klienci" w menu | nowy interfejs | menu ☰ → **Klienci** jest pod *Dostęp do danych* |
| okno się otwiera, ale „Access blocked" | aplikacja w trybie **Testing** | Krok 4 → **Publish app** |
| `invalid_client` | Client ID z innym projektem | skopiuj ID jeszcze raz z Credentials |
| przycisk Google pokazuje komunikat | Client ID pusty w `tools/config.json` | Krok 7 |
