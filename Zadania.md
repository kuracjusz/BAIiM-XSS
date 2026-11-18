## 📝 Zadania do wykonania

### Zadanie 1: Reflected XSS (Podstawy)
* **Moduł:** `XSS (Reflected)`
* **Poziom bezpieczeństwa:** `Low`
* **Cel:** Sprawdź, jak aplikacja przetwarza dane wpisane w pole formularza powitalnego. Wstrzyknij kod JavaScript, który wyświetli okno dialogowe z nazwą domeny (`document.domain`) lub zawartością ciasteczka (`document.cookie`).
* **Pytanie kontrolne:** Zwróć uwagę na pasek adresu przeglądarki po wysłaniu formularza. W jaki sposób atakujący mógłby wykorzystać tę podatność przeciwko innemu użytkownikowi?

### Zadanie 2: Stored XSS (Trwały XSS)
* **Moduł:** `XSS (Stored)`
* **Poziom bezpieczeństwa:** `Low`
* **Cel:** Dodaj wpis w księdze gości zawierający kod JavaScript. Po zapisaniu wpisu odśwież stronę (F5) i upewnij się, że skrypt uruchamia się automatycznie przy każdym jej załadowaniu.
* **Wskazówka:** Jeśli pole *Name* ogranicza długość wpisywanego tekstu po stronie przeglądarki (atrybut `maxlength`), sprawdź, czy możesz zmienić to ograniczenie w narzędziach deweloperskich przeglądarki (F12 -> *Inspect*) lub wykorzystać pole *Message*.
* **Uwaga:** Po wykonaniu zadania wyczyść wpisy w bazie za pomocą przycisku **Clear Guestbook**.

### Zadanie 3: Omijanie prostych filtrów (Blacklisting)
* **Moduł:** `XSS (Reflected)` lub `XSS (Stored)`
* **Poziom bezpieczeństwa:** `Medium`
* **Cel:** Zmień poziom trudności w zakładce **DVWA Security** na **Medium**. Spróbuj ponownie użyć prostego payloadu ze znacznikiem `<script>alert(1)</script>`. Zauważ, że aplikacja usuwa ten tag. Znajdź sposób na obejście zastosowanego zabezpieczenia i wywołaj okno `alert()`.
* **Wskazówka:** Zastanów się, czy filtr sprawdza wielkość liter, czy usuwa ciąg znaków tylko jednokrotnie, albo czy możesz wywołać JavaScript za pomocą innego znacznika HTML i jego zdarzeń (np. `<img>`, `<svg>`, `<body>`). Porównaj swoje wnioski z kodem w *View Source*.

### Zadanie 4: DOM-based XSS
* **Moduł:** `XSS (DOM)`
* **Poziom bezpieczeństwa:** `Low` (opcjonalnie również `Medium`)
* **Cel:** Przeanalizuj działanie formularza wyboru języka. Zwróć uwagę na to, jak zmienia się adres URL po kliknięciu przycisku *Select* oraz w jaki sposób kod JavaScript na stronie odczytuje wybrany język i generuje listę rozwijaną. Wstrzyknij własny kod JavaScript poprzez modyfikację parametru w pasku adresu przeglądarki.
* **Pytanie kontrolne:** Czym różni się wykonanie tego ataku od klasycznego *Reflected XSS* z Zadania 1 z perspektywy ruchu sieciowego wysyłanego do serwera?

### Zadanie 5: Zaawansowane omijanie filtrów i walidacji
* **Moduł:** `XSS (Reflected)` **oraz** `XSS (DOM)`
* **Poziom bezpieczeństwa:** `High`
* **Cel:** Zmień poziom trudności w zakładce **DVWA Security** na **High** i wykonaj dwie próby obejścia silniejszych zabezpieczeń:
  1. **W `XSS (Reflected)`:** Aplikacja używa wyrażeń regularnych do wycinania wszelkich wariantów znacznika `<script>` (niezależnie od wielkości liter i zagnieżdżeń). Wykorzystaj wektor oparty o inne tagi HTML i atrybuty zdarzeń (np. `onerror`, `onload`), aby uruchomić kod JavaScript.
  2. **W `XSS (DOM)`:** Aplikacja wprowadza białą listę (whitelist) dozwolonych języków po stronie serwera/skryptu. Zastanów się, która część adresu URL **nie jest** wysyłana na serwer i jak możesz ją wykorzystać do wstrzyknięcia kodu po stronie przeglądarki (podpowiedź: symbol `#` – fragment identifier).

### Zadanie 6 (Analiza): Jak skutecznie bronić się przed XSS?
* **Moduł:** Dowolny moduł XSS
* **Poziom bezpieczeństwa:** `Impossible`
* **Cel:** Przełącz poziom trudności na **Impossible**, kliknij przycisk **View Source** (oraz **Compare** w oknie podglądu kodu), a następnie porównaj kod poziomu `High` z poziomem `Impossible`.
* **Do opisania w sprawozdaniu:**
  * Jakie konkretne funkcje w PHP/JavaScript zostały użyte na poziomie `Impossible`, aby całkowicie wyeliminować podatność XSS?
  * Dlaczego podejście oparte na czarnych listach (poziomy `Medium` i `High`) jest nieskuteczne w porównaniu z kodowaniem wyjścia (ang. *Output Encoding*)?
