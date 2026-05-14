# Eliadora v10 — naprawy bezpieczeństwa i bugów

Poprawki nałożone na oryginał `eliadora8.html`.

## 🔴 Krytyczne — bezpieczeństwo (XSS)

1. **Dodane funkcje `esc()` i `escAttr()`** na początku skryptu.
   Przetestowane na 6 wektorach ataku — wszystkie neutralizują złośliwy input.

2. **`escAttr()` we wszystkich `onclick="..."` z interpolacją ID osoby**:
   - `injectSidebarActions` — przyciski Add Parent/Child/Spouse
   - `openEditForm` — submit, unlink, delete, toggle nationality, switch bio tab
   - Bez tej zmiany: zaimportowany plik z `id: "');alert(1);//"` wykonywał kod.

3. **`esc()` w `sb-form-title`** — wcześniej `${person.fn} ${person.ln}` w innerHTML
   bez escape. Nazwisko `<script>alert(1)</script>` po wejściu w edycję = XSS.

4. **`esc()` w `sb-info`** — `person.bp`, `person.dp`, `formatDate()` były
   wstawiane do innerHTML bez escape.

5. **`esc()` dla wszystkich `t(...)`** w innerHTML — defense in depth.

## 🟠 Bugi i potencjalne crashe

6. **Race condition w `setTheme`** — opakowane w try/catch, jasny komentarz
   wyjaśniający że bgR/bgTint mogą jeszcze nie istnieć przy pierwszym call.

7. **Fallback UI gdy dane się nie załadują** — funkcja `showDataLoadError`
   wyświetla po polsku co poszło nie tak i jak naprawić (uruchomić HTTP server).
   Wcześniej app pokazywał pusty SVG i nic nie mówił.

8. **Normalizacja `p[]`/`s[]`** po wczytaniu PEOPLE. Imported data bez tych
   pól crashowała `buildHourglass` z `Cannot read property 'includes' of undefined`.

## 🟡 localStorage / persistence

9. **`buildSavable()` strip `data:` URL** przed zapisem do localStorage.
   Wcześniej wgrany 8MB MP4 jako data URL przepełniał quotę i savePeople
   cicho ignorowało błąd — zmiany przepadały przy reloadzie.

10. **Lepszy komunikat błędu na QuotaExceededError** — po polsku, z radą.

11. **`loadVideoFile` limit zmniejszony z 8 MB do 2 MB** + komunikat że
    wczytany plik nie persistuje (tylko ścieżki względne się zapisują).

## 🟢 Drobne

12. **Usunięty martwy kod** — `updateGrainOld` (nigdzie nie wołane).

13. **Wydajność lang switcher** — używa `lastLayout` zamiast przeliczać
    `buildHourglass + calculateLayout` przy każdej zmianie języka
    (zmiana języka nie zmienia układu, tylko etykiety).

14. **Komentarze do stałych `L`** — wszystkie pola opisane.

15. **ARIA labels** — dla emoji-buttons w nagłówku (PDF, Drukuj, Okładka,
    Eksport, Reset, music toggle, music volume, lang switcher, nav buttons).

## Czego NIE zrobiłem

- Refaktoru `onclick` na `addEventListener` z event delegation — większa
  zmiana, escape jest równie bezpieczny, można zrobić w kolejnej wersji.
- Usunięcia `p/eliadora-data.js` — to oddzielny plik, `rm` przy deploy.
- Unit testów — zalecam Vitest na `isAncestor`, `applyParentLink`, `findDuplicate`,
  `ageSanityWarning`, `escAttr`.
- Migracji do ES modules — duża zmiana strukturalna.

## Weryfikacja

```
$ node --check check.js
✓ Składnia OK po wszystkich zmianach

$ node test-escAttr.js
✓ XSS injection
✓ normalne ID
✓ apostrof (O'Brien)
✓ HTML injection
✓ znaki specjalne (& " <)
✓ newline
6/6 passed
```
