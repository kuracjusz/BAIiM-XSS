# Cross-Site Scripting (XSS) - laboratorium

## 🛠️ Wymagania wstępne

Do uruchomienia środowiska potrzebujesz zainstalowanego **Dockera** (np. Docker Desktop dla Windows/macOS lub Docker Engine dla Linux).

## 🚀 Uruchomienie środowiska

### 1. Damn Vulnerable Web Application (DVWA) – Zadania 1–4

```bash
docker run --rm -d -p 127.0.0.1:8080:80 --name dvwa vulnerables/web-dvwa
```

#### Konfiguracja wstępna DVWA:
1. Otwórz w przeglądarce adres: `http://127.0.0.1:8080`
2. Zaloguj się używając domyślnych danych:
   * **Username:** `admin`
   * **Password:** `password`
3. Przejdź na dół strony powitalnej i kliknij przycisk **Create / Reset Database**.
4. Poczekaj kilka sekund na przekierowanie do ekranu logowania i zaloguj się ponownie (`admin` / `password`).
5. W menu po lewej stronie przejdź do zakładki **DVWA Security** i upewnij się, że poziom trudności jest ustawiony na **Low**.
