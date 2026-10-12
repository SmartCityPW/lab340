# CLAUDE.md — kontekst projektu

Rozmawiaj ze mną po polsku.

## Cel

Dokumentacja obsługi laboratorium (stanowiska, instrukcje, wyposażenie) jako strona na GitHub Pages.
Kluczowy wymóg: **treść mają łatwo edytować osoby nietechniczne**, bez gita i bez Markdowna.

## Stack i dlaczego

- **Zensical** (nie MkDocs Material): Material for MkDocs ma koniec wsparcia 5.11.2026, a ten sam zespół
  rozwija Zensical jako następcę. Wersja przypięta w `requirements.txt` (projekt przed 1.0).
  Konfiguracja w `zensical.toml`, szablony to MiniJinja (nie Jinja2).
- **Pages CMS** (app.pagescms.org) jako panel edycji. Konfiguracja w `.pages.yml`. Edytorzy są zapraszani
  mailem jako „collaborators” i nie potrzebują konta GitHub. Każdy zapis to commit na `main`.
- **GitHub Actions** (`.github/workflows/docs.yml`): build i deploy na Pages przy każdym pushu na `main`.
  Pages ustawione na Source = GitHub Actions. Repo publiczne w organizacji, więc wszystko jest darmowe.

## Struktura

- `docs/index.md`, `docs/bhp-i-regulamin.md`: strony ogólne (w Pages CMS jako `type: file`).
- `docs/wyposazenie.md`: strona „Wyposażenie laboratorium” (`type: file`, dawniej `aparatura`). Lista jest
  prowadzona w Excelu, a pole `lista_wyposazenia` (URL) renderuje się jako przycisk. Kolekcja urządzeń została usunięta.
- `docs/stanowiska/`, `docs/instrukcje/`: kolekcje Pages CMS (`exclude: [index.md]`).
- `docs/images/`, `docs/pliki/`: media z Pages CMS (`output: /images`, `/pliki`).
- `overrides/main.html`: nadpisuje blok `content`.
- `overrides/partials/lab.html`: makro `media()`.
- `docs/stylesheets/extra.css`: style `.lab-*`.

## Ustalenia i pułapki (nie zmieniać bez powodu)

- **Nawigacja jest automatyczna** (brak `nav` w `zensical.toml`), żeby strony z Pages CMS same trafiały do menu.
  Wpisy katalogów w `nav` (`{ "X" = "folder/" }`) NIE rozwijają się w Zensical, bo strony znikają z `nav.pages`.
- **Nazwa sekcji w menu = nazwa folderu z wielkiej litery** (tytuł z `index.md` jest ignorowany). Dlatego foldery
  mają nazwy bez polskich znaków, które dobrze wyglądają (`stanowiska`, `instrukcje`). Pojedyncze pliki
  (np. `wyposazenie.md`) biorą nazwę w menu z `title`, więc mogą mieć polskie znaki.
  Kolejność jest alfabetyczna: najpierw pliki, potem foldery.
- **Pole `status` w front matter jest zarezerwowane przez motyw** (ikonki w nav), nie używać na własne dane.
- **Ścieżki mediów:** Pages CMS zapisuje `/images/...` od korzenia domeny, a strona żyje pod `/NAZWA-REPO/`.
  `main.html` przepisuje `src="/images/`, `href="/images/` i `href="/pliki/` na `base_url`, a pola obrazków
  z front matter idą przez makro `media()`. Działa też lokalnie (`zensical serve` serwuje pod ścieżką z `site_url`).
- **Strony rozpoznaję po `page.url`** (np. `== "wyposazenie/"`), a nie po `page.is_index` (zawodne w Zensical).
- **MiniJinja:** zamiast `.startswith()` używaj testu `x is startingwith("...")`. Wszystkie strony z meta są dostępne
  w `nav.pages` (lista obiektów z `title`, `url`, `meta`, `is_index`).

## Do zweryfikowania na żywym repo

- Czy `filename: "{primary}.md"` w Pages CMS daje nazwy bez polskich znaków i spacji.
- Jaką ścieżkę wstawia rich-text przy wklejaniu zdjęcia (oczekiwane `/images/...`).

## TODO
- Rozważyć akcję Pages CMS / workflow do automatycznego zmniejszania zdjęć (limit Pages ok. 1 GB).
- Repo jest publiczne: w treści nie umieszczać kodów, haseł ani danych osobowych.

## Weryfikacja zmian

```bash
pip install -r requirements.txt
zensical build --clean      # musi przejść bez błędów
zensical serve              # podgląd: http://localhost:8000
```
Po zmianach w szablonach sprawdź strony `wyposazenie/` i przykładowe stanowisko.