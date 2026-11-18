## 📝 Zadania do wykonania

Dla każdego zadania udokumentuj:
* Użyty **payload** (kod wstrzyknięty w pole formularza lub adres URL).
* **Zrzut ekranu** potwierdzający wykonanie kodu JavaScript.
* Krótkie **wyjaśnienie**, dlaczego atak się powiódł (skorzystaj z przycisku **View Source** w prawym dolnym rogu DVWA, aby przeanalizować kod źródłowy PHP).

---

### Zadanie 1: Reflected XSS (Podstawy)
* **Moduł:** `XSS (Reflected)`
* **Poziom bezpieczeństwa:** `Low`
* **Cel:** Sprawdź, jak aplikacja przetwarza dane wpisane w pole formularza powitalnego. Wstrzyknij kod JavaScript, który wyświetli okno dialogowe z nazwą domeny (`document.domain`) lub zawartością ciasteczka (`document.cookie`).
* **Pytanie do refleksji:** Zwróć uwagę na pasek adresu przeglądarki po wysłaniu formularza. W jaki sposób atakujący mógłby wykorzystać tę podatność przeciwko innemu użytkownikowi?

<details>
<summary>💡 Kliknij, aby zobaczyć wskazówkę</summary>

Aplikacja wstawia wartość podaną w formularzu bezpośrednio do kodu HTML strony bez żadnego filtrowania. Spróbuj wpisać w pole tekstowe standardowy znacznik HTML służący do osadzania skryptów, np. `<script>alert(document.domain)</script>`.
</details>

<details>
<summary>🧠 Kliknij, aby zobaczyć odpowiedź do refleksji</summary>

Ponieważ dane z formularza są przesyłane metodą `GET` i stają się częścią adresu URL (parametr `?name=...`), złośliwy kod nie jest zapisywany na serwerze. Aby zaatakować ofiarę, przestępca musi przygotować spreparowany link zawierający złośliwy skrypt w parametrze URL i nakłonić zalogowanego użytkownika (np. poprzez e-mail phishingowy lub komunikator) do jego kliknięcia. Gdy ofiara otworzy link, jej przeglądarka wykona skrypt w kontekście jej własnej sesji.
</details>

---

### Zadanie 2: Stored XSS (Trwały XSS)
* **Moduł:** `XSS (Stored)`
* **Poziom bezpieczeństwa:** `Low`
* **Cel:** Dodaj wpis w księdze gości zawierający kod JavaScript. Po zapisaniu wpisu odśwież stronę (F5) i upewnij się, że skrypt uruchamia się automatycznie przy każdym jej załadowaniu.
* **Uwaga:** Po wykonaniu zadania wyczyść wpisy w bazie za pomocą przycisku **Clear Guestbook**.
* **Pytanie do refleksji:** Dlaczego *Stored XSS* jest uznawany za groźniejszy wariant podatności niż *Reflected XSS*?

<details>
<summary>💡 Kliknij, aby zobaczyć wskazówkę</summary>

Pole *Message* akceptuje dłuższe ciągi znaków i nie posiada żadnych filtrów po stronie serwera. Jeśli chcesz wstrzyknąć kod w pole *Name*, które ma ograniczenie długości po stronie przeglądarki, otwórz narzędzia deweloperskie (`F12`), kliknij prawym przyciskiem myszy na pole *Name* -> *Zbadaj (Inspect)* i zmień wartość atrybutu `maxlength="10"` na większą (np. `100`).
</details>

<details>
<summary>🧠 Kliknij, aby zobaczyć odpowiedź do refleksji</summary>

W przypadku *Stored XSS* złośliwy kod zostaje trwale zapisany w bazie danych aplikacji (np. jako komentarz, wpis na forum czy nazwa użytkownika). Atakujący nie musi wysyłać nikomu specjalnie spreparowanych linków – skrypt wykona się automatycznie u **każdego** użytkownika (w tym administratora), który po prostu odwiedzi zainfekowaną podstronę.
</details>

---

### Zadanie 3: Omijanie prostych filtrów (Blacklisting)
* **Moduł:** `XSS (Reflected)` lub `XSS (Stored)`
* **Poziom bezpieczeństwa:** `Medium`
* **Cel:** Zmień poziom trudności w zakładce **DVWA Security** na **Medium**. Spróbuj ponownie użyć prostego payloadu ze znacznikiem `<script>alert(1)</script>`. Zauważ, że aplikacja usuwa ten tag. Znajdź sposób na obejście zastosowanego zabezpieczenia i wywołaj okno `alert()`.
* **Pytanie do refleksji:** Dlaczego usuwanie konkretnych słów kluczowych (tzw. czarna lista / *blacklisting*) jest nieskuteczną metodą obrony przed XSS?

<details>
<summary>💡 Kliknij, aby zobaczyć wskazówkę</summary>

Podejrzyj kod źródłowy przyciskiem **View Source**. Zauważysz funkcję `str_replace( '<script>', '', $_GET[ 'name' ] )`. Ma ona trzy słabości, z których każdą możesz wykorzystać:
1. Rozróżnia wielkość liter (spróbuj `<Script>alert(1)</Script>`).
2. Usuwa ciąg znaków tylko jednokrotnie, nie rekurencyjnie (spróbuj `<scr<script>ipt>alert(1)</script>`).
3. Blokuje tylko tag `<script>`, ale pozwala na użycie innych znaczników HTML ze zdarzeniami JavaScript, np. `<img src=x onerror=alert(1)>`.
</details>

<details>
<summary>🧠 Kliknij, aby zobaczyć odpowiedź do refleksji</summary>

Język HTML i przeglądarki internetowe oferują dziesiątki sposobów na wykonanie kodu JavaScript (różne znaczniki, atrybuty zdarzeń `onload`/`onerror`/`onmouseover`, kodowanie encjami HTML czy nietypowe formatowanie). Próba przewidzenia i zablokowania wszystkich możliwych kombinacji zawsze kończy się przeoczeniem jakiegoś wektora ataku.
</details>

---

### Zadanie 4: DOM-based XSS
* **Moduł:** `XSS (DOM)`
* **Poziom bezpieczeństwa:** `Low`
* **Cel:** Przeanalizuj działanie formularza wyboru języka. Zwróć uwagę na to, jak zmienia się adres URL po kliknięciu przycisku *Select* oraz w jaki sposób kod JavaScript na stronie odczytuje wybrany język i generuje listę rozwijaną. Wstrzyknij własny kod JavaScript poprzez modyfikację parametru w pasku adresu przeglądarki.
* **Pytanie do refleksji:** Czym różni się wykonanie tego ataku od klasycznego *Reflected XSS* z Zadania 1 z perspektywy serwera i systemów ochronnych (np. WAF)?

<details>
<summary>💡 Kliknij, aby zobaczyć wskazówkę</summary>

Zbadaj źródło strony (`Ctrl+U` lub `F12`), aby zobaczyć skrypt odpowiedzialny za generowanie listy `<select>`. Kod pobiera wartość z `document.location.href` (po fragmencie `default=`) i wstrzykuje ją bezpośrednio do DOM przez `document.write()`. Aby uciec z bieżącego tagu `<option>` i `<select>`, spróbuj przekazać w URL parametr zakończony zamknięciem tych tagów, np.:
`?default=English</option></select><script>alert(1)</script>`
</details>

<details>
<summary>🧠 Kliknij, aby zobaczyć odpowiedź do refleksji</summary>

W *DOM-based XSS* serwer w ogóle nie uczestniczy w przetwarzaniu złośliwego ciągu – zwraca identyczny, statyczny kod HTML/JS, a sam błąd leży wyłącznie w kodzie JavaScript wykonywanym lokalnie w przeglądarce ofiary. Oznacza to, że filtry po stronie backendu (np. PHP) nie mają szans zadziałać, a w przypadku użycia symbolu `#` złośliwy payload nie trafia nawet w logi serwera HTTP.
</details>

---

### Zadanie 5: Zaawansowane omijanie filtrów i walidacji
* **Moduł:** `XSS (Reflected)` **oraz** `XSS (DOM)`
* **Poziom bezpieczeństwa:** `High`
* **Cel:** Zmień poziom trudności w zakładce **DVWA Security** na **High** i wykonaj dwie próby obejścia silniejszych zabezpieczeń:
  1. **W `XSS (Reflected)`:** Aplikacja używa wyrażeń regularnych do wycinania wszelkich wariantów znacznika `<script>`. Wykorzystaj wektor oparty o inne tagi HTML, aby uruchomić kod JavaScript.
  2. **W `XSS (DOM)`:** Aplikacja sprawdza po stronie serwera, czy wybrany język znajduje się na liście dozwolonych wartości. Wykorzystaj fragment adresu URL (`#`), aby ominąć walidację serwerową i wstrzyknąć kod po stronie przeglądarki.
* **Pytanie do refleksji:** Dlaczego fragment adresu URL po znaku `#` pozwala ominąć walidację przeprowadzaną przez skrypt PHP na serwerze?

<details>
<summary>💡 Kliknij, aby zobaczyć wskazówkę</summary>

1. **Reflected XSS (High):** Wyrażenie regularne `preg_replace( '/<(.*)s(.*)c(.*)r(.*)i(.*)p(.*)t/i', ... )` blokuje tylko słowo `script`. Użyj innego znacznika HTML z atrybutem zdarzenia, np.:
   `<img src=x onerror=alert(1)>` lub `<svg onload=alert(1)>`
2. **DOM XSS (High):** Przeglądarka nigdy nie wysyła części URL znajdującej się po znaku `#` (fragment identifier) w żądaniu HTTP do serwera. Dzięki temu serwer widzi poprawny parametr `?default=English`, przepuszcza żądanie, natomiast skrypt JavaScript w przeglądarce odczytuje cały `document.location.href` (razem z tym, co jest po `#`). Spróbuj adresu:
   `?default=English#</option></select><script>alert(1)</script>`
</details>

<details>
<summary>🧠 Kliknij, aby zobaczyć odpowiedź do refleksji</summary>

