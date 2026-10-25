# Jak opublikować QcurX — żeby każdy wszedł z linku (nic nie pobiera)

Strona jest w 100% statyczna: HTML + CSS + JS w plikach, wszystkie obrazki wklejone
jako `data:` URI. Nie ma bazy, instalacji ani budowania na serwerze — wystarczy
wrzucić pliki na dowolny hosting statyczny. Odwiedzający nie pobiera niczego:
otwiera link i ogląda.

## Co wrzucasz na hosting

```
index.html          ← galeria
pack.html           ← strona paczki (fallback: pack.html?p=slug)
p/<slug>.html       ← lekkie strony poszczególnych paczek (tylko ich dane)
.nojekyll           ← wyłącza Jekylla na GitHub Pages
```

Katalogi `packs/`, `tools/`, `downloads/` **nie są potrzebne** na hostingu —
są tylko do budowania strony. Gotowa paczka do wgrania to `qcurx-site.zip`.

---

## Opcja A — Netlify Drop (najszybciej, ~1 minuta)

1. Wejdź na <https://app.netlify.com/drop>.
2. Przeciągnij **rozpakowany** folder z plikami (albo sam `qcurx-site.zip`).
3. Dostajesz adres typu `https://qcurx-abc123.netlify.app` — działa od razu.

## Opcja B — GitHub Pages (darmowe, stały adres)

1. Utwórz repozytorium na <https://github.com/new>.
2. Wgraj pliki (`index.html`, `pack.html`, folder `p/`, `.nojekyll`) do gałęzi `main`.
3. Repozytorium → **Settings → Pages** → Branch: `main`, folder `/ (root)` → **Save**.
4. Po chwili strona działa pod `https://twojlogin.github.io/nazwa-repo/`.

## Opcja C — Cloudflare Pages

1. <https://dash.cloudflare.com/> → **Workers & Pages → Create → Pages → Upload assets**.
2. Wgraj folder, zatwierdź — dostajesz `https://nazwa.pages.dev`.

## własna domena

W Netlify/Cloudflare: **Domain settings → Add custom domain** i wpisz rekordy DNS,
które pokaże panel. Po podłączeniu domeny dodaj ją też do Google Cloud
(Authorized JavaScript origins), inaczej logowanie Google przestanie działać.

---

## Po publikacji — logowanie

- **Adres e-mail** (bez hasła) i **e-mail + hasło** działają od razu, bez konfiguracji.
- **Pobieranie paczek nie wymaga logowania.** Logowanie potrzebne jest tylko do
  dodawania własnych paczek i do Ustawień (kody do archiwów `.rar`).
- **Google**: żeby przycisk „Zaloguj przez Google” otwierał normalne okno wyboru
  konta, wystarczy **jednorazowo** wpisać Client ID w `tools/config.json`
  i przebudować stronę:

  ```json
  { "googleClientId": "1234567890-abc.apps.googleusercontent.com" }
  ```

  ```bash
  python3 tools/build.py
  ```

  Potem wgrywasz `qcurx-site.zip` na hosting. **Odwiedzający nigdy nie widzą
  żadnych kluczy** — klikają zwykły przycisk Google i wybierają konto.
  Dopóki Client ID jest pusty, zakładka Google jest po prostu ukryta
  (zostaje logowanie e-mailem) — nikt nie jest proszony o klucze.

  W Google Cloud → *Authorized JavaScript origins* dodaj **adres produkcyjny**
  strony (np. `https://qcurx-abc123.netlify.app`), nie tylko `localhost`.

## O czym pamiętać

- Konta i paczki dodane przez stronę są zapisywane w `localStorage` **przeglądarki
  użytkownika** — nie na serwerze. Każdy widzi galerię bazową; swoje dodatki widzi
  tylko u siebie. Współdzielona galeria dla wszystkich wymaga backendu
  (Supabase / Firebase) — mogę to podpiąć.
- Pliki `.cur` do pobrania: w paczkach z linkiem zewnętrznym (np. Google Drive)
  przycisk „Pobierz” prowadzi do Drive. Paczki bez linku pokazują „Wkrótce”.
