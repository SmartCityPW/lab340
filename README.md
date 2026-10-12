# Dokumentacja laboratorium

Strona z dokumentacją obsługi laboratorium (stanowiska, instrukcje, aparatura),
budowana przez [Zensical](https://zensical.org/) i publikowana na GitHub Pages.
Treść edytuje się w przeglądarce przez [Pages CMS](https://pagescms.org/), bez znajomości gita ani Markdowna.

## Jak to działa

```
Pages CMS (przeglądarka) ──commit──▶ repo (main) ──GitHub Actions──▶ GitHub Pages
```

Każdy zapis w Pages CMS to commit do gałęzi `main`. Workflow `.github/workflows/docs.yml`
buduje stronę i publikuje ją, po ok. minucie zmiana jest widoczna.

## Pierwsza konfiguracja (jednorazowo, administrator)

1. **Repo:** wrzuć zawartość tego folderu do publicznego repo w organizacji, gałąź `main`.
2. **Adres strony:** w `zensical.toml` ustaw `site_url` (np. `https://NAZWA-ORG.github.io/NAZWA-REPO/`)
   i opcjonalnie `repo_url`.
3. **GitHub Pages:** *Settings → Pages → Build and deployment → Source:* **GitHub Actions**.
   Pierwszy build uruchomisz z zakładki *Actions → Dokumentacja → Run workflow* albo dowolnym pushem.
4. **Pages CMS:** wejdź na <https://app.pagescms.org>, zaloguj się przez GitHub i zainstaluj
   aplikację Pages CMS na tym repozytorium (dla organizacji może być potrzebna zgoda jej właściciela).
5. **Edytorzy:** w Pages CMS zaproś osoby mailem (współpracownicy). Nie potrzebują konta GitHub.
   Mogą edytować treść i zdjęcia, ale nie konfigurację.

## Struktura

| Ścieżka | Co to jest |
|---|---|
| `docs/index.md` | Strona główna |
| `docs/bhp-i-regulamin.md` | BHP i regulamin |
| `docs/stanowiska/` | Jedna strona na stanowisko. Lista aparatury dopisuje się automatycznie |
| `docs/instrukcje/` | Instrukcje i procedury |
| `docs/aparatura/` | Jedna strona na urządzenie (pola formularza → karta urządzenia). `index.md` pokazuje automatyczną tabelę |
| `docs/images/`, `docs/pliki/` | Zdjęcia i pliki wgrywane przez Pages CMS |
| `.pages.yml` | Konfiguracja panelu Pages CMS: sekcje i pola formularzy |
| `zensical.toml` | Konfiguracja strony |
| `overrides/` | Rozszerzenia szablonu: karta urządzenia, tabela aparatury, poprawka ścieżek do zdjęć |
| `docs/stylesheets/extra.css` | Style karty, tabeli i etykiet stanu |

## Dobre do wiedzenia

- **Menu buduje się samo** z folderów w `docs/`, więc nowe strony z Pages CMS pojawiają się automatycznie.
  Kolejność w sekcjach jest alfabetyczna (po nazwie pliku), a nazwa sekcji pochodzi od nazwy folderu,
  dlatego foldery mają nazwy bez polskich znaków, które ładnie wyglądają po zamianie pierwszej litery na wielką.
- **Pole stanu urządzenia nazywa się `stan`, nie `status`**, bo `status` jest zarezerwowane przez motyw
  (ikonki w menu).
- **Przypisanie urządzenia do stanowiska** zapisuje ścieżkę pliku stanowiska. Po zmianie nazwy pliku
  stanowiska trzeba ponownie wybrać stanowisko w urządzeniach, które były do niego przypisane.
- **Zdjęcia:** proś edytorów o wgrywanie zdjęć pomniejszonych (np. do ok. 1600 px), a nie prosto z telefonu.
  GitHub Pages ma limit ok. 1 GB na stronę.
- **Repo jest publiczne** razem z całą historią. Nie wpisuj kodów do drzwi, haseł ani danych osobowych.
- **Wersja Zensical** jest przypięta w `requirements.txt` (projekt jest przed wersją 1.0).
  Aktualizuj ją świadomie i sprawdź build lokalnie.

## Podgląd lokalny

```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
zensical serve          # http://localhost:8000
```
