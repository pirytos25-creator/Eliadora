# Eliadora — drzewo genealogiczne

Interaktywne drzewo genealogiczne dla projektu wsparcia dobrostanu Seniorów.

## Szybki start

Otwórz `index.html` w przeglądarce.

Aby obrazy portretów się załadowały do PDF, plik powinien być serwowany przez
lokalny serwer HTTP, np.:

```bash
python3 -m http.server
```

a potem otwórz `http://localhost:8000/` w przeglądarce.

Otwarcie przez `file://` działa, ale wkład fotografii do PDF zawiedzie
z powodu reguł CORS.

## Publikacja na GitHub Pages

1. Stwórz nowe repozytorium na GitHubie (np. `lucid-academy` albo `eliadora`).
2. Wrzuć całą zawartość tego folderu do repo:

   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/<twoj-uzytkownik>/<nazwa-repo>.git
   git push -u origin main
   ```

3. W repozytorium na GitHubie wejdź w **Settings → Pages**.
4. W sekcji **Source** wybierz **Deploy from a branch**, gałąź `main`, folder `/ (root)`.
5. Po chwili strona będzie dostępna pod adresem
   `https://<twoj-uzytkownik>.github.io/<nazwa-repo>/`.

Aplikacja jest w 100% statyczna (HTML + JS + assety), więc nie wymaga
żadnego backendu ani buildowania.

## Pliki

- `index.html` — cała aplikacja (UI + logika)
- `eliadora-data.js` — dane drzewa (osoby, powiązania)
- `vendor/*.js` — lokalne biblioteki D3 i jsPDF
- `assets/` — tekstury pergaminu, ikona, muzyka tła
- `p/*.png` — portrety osób
- `adult_f.png`, `adult_m.png`, `child_f.png`, `child_m.png`, `elder_f.png`, `elder_m.png` —
  placeholdery wg płci i wieku (zostają w root, bo aplikacja pyta użytkownika
  o samą nazwę pliku w polu „Zdjęcie portretowe”)

## Co nowego (wersja 10)

- **Księga pełnoekranowa** — okładka zajmuje cały viewport, ma grzbiet, metalowe narożniki, klamry, medaliony genealogiczne i otwiera się jak duża karta księgi.
- **4 wygenerowane formaty pergaminu** — klasyczny pergamin, papirus, welin i przydymiony stary pergamin, przełączane kółkami w nagłówku.
- **Czysta papirusowo-pergaminowa baza** — papierowe tło bez sztucznych linii, także pod kartami i zdjęciami w samym drzewie.
- **Portret MP4** — w edycji osoby można wpisać ścieżkę do MP4/WebM albo wczytać mały plik jako portret wideo. Galeria i lightbox rozpoznają też pliki wideo.
- **Wygodniejsza nawigacja** — pasek cofania/dalej, powrót do osoby głównej, wyszukiwarka osób, centrowanie i zoom drzewa.
- **Wygenerowane tła pergaminowe** — nowe assety `assets/parchment-classic.png`, `assets/parchment-papyrus.png`, `assets/parchment-vellum.png` i `assets/parchment-smoke.png` używane jako papierowe tła strony i planszy.
- **Pergamin jako tło całości** — tło obejmuje stronę, nagłówek, planszę i panel boczny.
- **Stabilne tło wewnątrz drzewa** — plansza drzewa znowu używa tekstury pergaminu, ale jako lekkiego tła CSS, bez ciężkiego obrazkowego patternu w SVG.
- **Szybkie przełączanie osób** — kliknięcie nowej karty przerywa poprzedni ruch drzewa i od razu przechodzi do najnowszego wyboru.
- **Lepsze centrowanie drzewa** — małe układy są automatycznie rozciągane na szerokość widoku roboczego, więc drzewo nie przykleja się do lewej strony ekranu.
- **Przesuwanie myszką** — drzewo można przeciągać przez przytrzymanie lewego przycisku myszy i ruch po planszy.
- **Senior Mode** — przycisk `SENIOR` powiększa litery, przyciski, pola formularzy i panel boczny oraz zapisuje wybór w przeglądarce.
- **Muzyka w tle** — dodany plik `assets/The_Keeper_s_Ledger.mp3`, przycisk odtwarzania i regulator głośności w nagłówku.
- **Wygodniejszy układ nagłówka** — wysokość paska jest mierzona automatycznie, więc drzewo i panel boczny nie wchodzą pod kontrolki po zmianie rozmiaru lub trybu Senior.
- **Lokalne biblioteki** — D3 i jsPDF są zapisane w `vendor/`, więc drzewo i eksport PDF nie zależą już od CDN przy otwieraniu z pulpitu.
- **Odchudzony rdzeń drzewa** — usunięte długie animacje linii i opóźnienia wyłaniania, które spowalniały podstawowe klikanie.

## Co nowego (wersja 9)

- **Okładka księgi z Białym Drzewem Gondoru** — pojawia się przy pierwszym otwarciu
  (zapamiętane w `localStorage`). Animacja otwarcia jak pierwszej strony książki.
  W dowolnej chwili można ją przywołać przyciskiem 📖 w nagłówku.
- **Nowe tło sepia/stara księga** — zamiast surowej kartki w linijki, ciepły
  pergamin z subtelną aged-paper teksturą.
- **Eksport do PDF (📄 PDF)** — aktualny widok drzewa zapisywany do A4
  (orientacja auto-wybierana). Polskie znaki zachowane jako pixele.
- **Druk (🖨)** — natywne drukowanie przeglądarki, panele i pasek narzędzi
  są ukrywane stylem `@media print`.
- **Drobne poprawki kodu** — naprawione martwe operatory ternarne i
  niekompletna logika sprawdzania wieku rodziców.
