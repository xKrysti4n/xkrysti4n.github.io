# Fundamenty HTTP

## Podstawowe metody HTTP

### GET
- Używana do pobierania danych z serwera
- Parametry są przekazywane w URL
- Nie powinna modyfikować danych na serwerze
- Może być buforowana
- Ma ograniczenia długości URL

### POST
- Używana do wysyłania danych na serwer
- Dane są przesyłane w ciele żądania
- Może modyfikować dane na serwerze
- Nie jest buforowana
- Nie ma ograniczeń co do ilości przesyłanych danych
- Często używana do tworzenia nowych zasobów

### PUT
- Używana do aktualizacji istniejących zasobów
- Wymaga podania pełnych danych zasobu
- Jest idempotentna (wielokrotne wywołanie daje ten sam efekt)
- Często używana do aktualizacji całych zasobów

### DELETE
- Używana do usuwania zasobów
- Jest idempotentna
- Nie powinna zawierać ciała żądania
- Zwykle zwraca kod 204 (No Content) po pomyślnym usunięciu

### OPTIONS
- Używana do sprawdzenia dostępnych metod HTTP dla danego endpointu
- Zwraca listę obsługiwanych metod w nagłówku Allow
- Często używana w CORS (Cross-Origin Resource Sharing)

### HEAD
- Podobna do GET, ale zwraca tylko nagłówki odpowiedzi
- Nie zwraca ciała odpowiedzi
- Używana do sprawdzenia metadanych zasobu
- Przydatna do weryfikacji czy zasób istnieje

### Przykłady użycia

### Podstawowe przykłady
```http
GET /api/users
POST /api/users
PUT /api/users/123
DELETE /api/users/123
OPTIONS /api/users
HEAD /api/users/123
```

### Rozszerzone przykłady

#### GET - pobieranie danych
```http
# Pobieranie listy użytkowników z filtrowaniem
GET /api/users?role=admin&status=active

# Pobieranie konkretnego użytkownika
GET /api/users/123

# Pobieranie komentarzy do posta
GET /api/posts/456/comments

# Wyszukiwanie produktów
GET /api/products?category=electronics&minPrice=100&maxPrice=500
```

#### POST - tworzenie nowych zasobów
```http
# Tworzenie nowego użytkownika
POST /api/users
{
    "name": "Jan Kowalski",
    "email": "jan@example.com",
    "password": "haslo123"
}

# Dodawanie komentarza
POST /api/posts/456/comments
{
    "content": "Świetny artykuł!",
    "authorId": 789
}

# Logowanie użytkownika
POST /api/auth/login
{
    "email": "jan@example.com",
    "password": "haslo123"
}
```

#### PUT - aktualizacja zasobów
```http
# Aktualizacja danych użytkownika
PUT /api/users/123
{
    "name": "Jan Kowalski",
    "email": "jan.nowy@example.com",
    "phone": "123456789"
}

# Aktualizacja statusu zamówienia
PUT /api/orders/789
{
    "status": "shipped",
    "trackingNumber": "TRK123456"
}
```

#### DELETE - usuwanie zasobów
```http
# Usuwanie użytkownika
DELETE /api/users/123

# Usuwanie komentarza
DELETE /api/posts/456/comments/789

# Anulowanie zamówienia
DELETE /api/orders/789
```

#### OPTIONS - sprawdzanie dostępnych metod
```http
# Sprawdzenie dostępnych metod dla endpointu użytkowników
OPTIONS /api/users
# Odpowiedź zawiera nagłówek Allow: GET, POST, PUT, DELETE

# Sprawdzenie dostępnych metod dla konkretnego użytkownika
OPTIONS /api/users/123
# Odpowiedź zawiera nagłówek Allow: GET, PUT, DELETE
```

#### HEAD - sprawdzanie metadanych
```http
# Sprawdzenie czy użytkownik istnieje
HEAD /api/users/123

# Sprawdzenie czy plik został zmodyfikowany
HEAD /api/documents/report.pdf

# Sprawdzenie rozmiaru pliku
HEAD /api/downloads/large-file.zip
```

### Przykłady nagłówków odpowiedzi
```http
# Odpowiedź dla GET
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 1234
Cache-Control: max-age=3600

# Odpowiedź dla POST
HTTP/1.1 201 Created
Location: /api/users/123
Content-Type: application/json

# Odpowiedź dla DELETE
HTTP/1.1 204 No Content

# Odpowiedź dla OPTIONS
HTTP/1.1 200 OK
Allow: GET, POST, PUT, DELETE
```

### Kody odpowiedzi

- 200 OK - pomyślne wykonanie żądania
- 201 Created - pomyślne utworzenie zasobu (POST)
- 204 No Content - pomyślne wykonanie bez zwracania danych (DELETE)
- 400 Bad Request - nieprawidłowe żądanie
- 401 Unauthorized - brak autoryzacji
- 403 Forbidden - brak uprawnień
- 404 Not Found - zasób nie istnieje
- 405 Method Not Allowed - metoda nie jest obsługiwana

### Zagrożenia bezpieczeństwa i potencjalne exploity

### GET - Potencjalne exploity
```http
# Przykład exploita XSS poprzez parametry GET
GET /api/search?q=<script>alert('XSS')</script>

# Przykład exploita SQL Injection
GET /api/users?id=1' OR '1'='1

# Przykład exploita Path Traversal
GET /api/files?path=../../../etc/passwd
```

### POST - Potencjalne exploity
```http
# Przykład exploita CSRF
POST /api/transfer
{
    "amount": 1000,
    "to": "attacker_account"
}

# Przykład exploita JSON Injection
POST /api/users
{
    "name": {"$gt": ""},
    "role": "admin"
}

# Przykład exploita XXE (XML External Entity)
POST /api/upload
Content-Type: application/xml
<?xml version="1.0"?>
<!DOCTYPE data [
    <!ENTITY file SYSTEM "file:///etc/passwd">
]>
<data>&file;</data>
```

### PUT - Potencjalne exploity
```http
# Przykład exploita poprzez nadpisanie plików
PUT /api/files/config.json
{
    "admin": true,
    "permissions": "all"
}

# Przykład exploita poprzez modyfikację uprawnień
PUT /api/users/123/permissions
{
    "role": "admin",
    "access_level": "superuser"
}
```

### DELETE - Potencjalne exploity
```http
# Przykład exploita poprzez masowe usuwanie
DELETE /api/users?confirm=false

# Przykład exploita poprzez usuwanie systemowych plików
DELETE /api/files?path=/var/www/html/index.php
```

### Bezpieczne praktyki

1. Walidacja danych wejściowych
   - Sprawdzanie typów danych
   - Sanityzacja parametrów
   - Walidacja długości i formatu

2. Autoryzacja i uwierzytelnianie
   - Używanie tokenów JWT
   - Implementacja CORS
   - Rate limiting

3. Zabezpieczenia przed popularnymi atakami
   - CSRF tokens
   - Content Security Policy (CSP)
   - XSS filters
   - SQL injection prevention

4. Logowanie i monitorowanie
   - Logowanie podejrzanych żądań
   - Monitorowanie nietypowych wzorców
   - Alerty bezpieczeństwa



---

## Nagłówki HTTP

Nagłówki HTTP to **dodatkowe dane przesyłane w żądaniu (request)** lub odpowiedzi (response). Służą np. do identyfikacji klienta, autoryzacji, przesyłania ciasteczek, itp.

---

###  Kluczowe nagłówki HTTP

### 🧠 `User-Agent`

Opisuje przeglądarkę lub klienta, który wysyła żądanie.
Przykład:
```
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) Chrome/124.0
```
Można go łatwo spoofować, np. przy crawlingu albo testach botów.

🔍 W pentestingu: sprawdzasz, czy backend rozróżnia klientów (np. „dostęp tylko dla Safari").

Przykłady:
1. Zmiana User-Agent na starszą wersję przeglądarki:
```
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) Chrome/80.0
```
2. Spoofing na boty wyszukiwarek:
```
User-Agent: Googlebot/2.1
```
3. Testowanie dostępu tylko dla mobilnych:
```
User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 14_0 like Mac OS X)
```

---

### 🧠 `Host`

Wskazuje, do jakiego hosta kierowane jest żądanie.
Przykład:
```
Host: login.mojadomena.pl
```
Umożliwia serwerowi rozróżnić, do której domeny klient się odwołuje (ważne przy virtual hosting).

🔍 W pentestingu: testujesz Host Header Injection, np. przekierowania zależne od `Host`.

Przykłady:
1. Testowanie przekierowań:
```
Host: evil.com
```
2. Cache poisoning:
```
Host: example.com
X-Forwarded-Host: evil.com
```
3. Bypass WAF:
```
Host: 127.0.0.1
```

---

### 🍪 `Cookie`

Przesyła ciasteczka (np. sesyjne, identyfikacyjne).
Przykład:
```
Cookie: sessionid=abc123; is_admin=true
```

🔍 W pentestingu: testujesz przejęcie sesji (Session Fixation / Session Hijacking), manipulujesz ciastkami, np. `is_admin=true`.

Przykłady:
1. Session Fixation:
```
Cookie: PHPSESSID=123456789
```
2. Manipulacja uprawnieniami:
```
Cookie: role=admin; is_admin=1; access_level=superuser
```
3. XSS w ciasteczkach:
```
Cookie: user=<script>alert(1)</script>
```

---

### 🧠 `Referer` *(z literówką z historycznych powodów)*

Pokazuje skąd przyszło żądanie, czyli URL strony, z której użytkownik kliknął link.
Przykład:
```
Referer: https://example.com/login
```

🔍 W pentestingu: możesz sprawdzać, czy aplikacja ufa Refererowi, np. w przypadku „tylko użytkownicy z naszej strony mogą...". Przydaje się do testowania CSRF, jeśli Referer jest jedynym zabezpieczeniem.

Przykłady:
1. Bypass sprawdzania Referer:
```
Referer: https://trusted-site.com
```
2. CSRF exploit:
```
Referer: https://evil.com
```
3. Empty Referer:
```
Referer: 
```

---

### 🧠 `Origin`

Pokazuje domenę źródłową bez ścieżki (bardziej zaufany niż Referer).
Przykład:
```
Origin: https://mojastrona.pl
```

🔍 W pentestingu: bardzo ważny przy CORS (Cross-Origin Resource Sharing)! Jeśli Origin jest źle sprawdzany → CORS misconfiguration → można kraść dane z cudzych sesji.

Przykłady:
1. CORS bypass:
```
Origin: https://evil.com
```
2. Null Origin:
```
Origin: null
```
3. Spoofing zaufanej domeny:
```
Origin: https://trusted-site.com
```

---

### 🧠`Authorization`

Przesyła token lub dane logowania.
Przykład (Basic Auth):
```
Authorization: Basic dXNlcjpwYXNz
```
Przykład (Bearer token):
```
Authorization: Bearer eyJhbGciOi...
```

🔍 W pentestingu: testujesz czy tokeny są przewidywalne, czy można używać tokenu z innym użytkownikiem, czy można zadziałać bez nagłówka i uzyskać dostęp (Broken Access Control).

Przykłady:
1. Token replay:
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```
2. Basic Auth bypass:
```
Authorization: Basic YWRtaW46YWRtaW4=
```
3. JWT manipulation:
```
Authorization: Bearer eyJhbGciOiJub25lIn0.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.
```

### Cheat sheet

| Nagłówek        | Co robi?                                | Pentesting zastosowanie           |
| --------------- | --------------------------------------- | --------------------------------- |
| `User-Agent`    | Identyfikuje klienta (np. przeglądarkę) | Bypassy, bot detection, spoofing  |
| `Host`          | Do jakiej domeny jest żądanie           | Host Header Injection             |
| `Cookie`        | Przesyła dane sesji                     | Session Hijack / Manipulacja      |
| `Referer`       | Z jakiej strony przyszedł użytkownik    | CSRF, redirecty, sprawdzanie flow |
| `Origin`        | Z jakiej domeny pochodzi przeglądarka   | CORS exploity                     |
| `Authorization` | Dane logowania/token                    | Auth bypass, token replay, brute  |


---
Okej, robimy porządne **wprowadzenie do cookies, sesji i tokenów**, jakiego potrzebuje każdy kto wchodzi w **web pentesting** 🔐
Będzie zrozumiale, ale dokładnie – bez owijania w bawełnę 😈

---

## Cookies, Sesje i Tokeny

---

###  Co to są cookies?

* Cookies to **małe dane** (key-value) wysyłane przez serwer do przeglądarki.
* Przeglądarka **automatycznie je dołącza** do każdego requestu do tej samej domeny.

**Przykład:**

```
Set-Cookie: sessionid=abc123; HttpOnly; Secure; Path=/; SameSite=Lax
```

---

###  Co może być w cookies?

| Klucz                     | Opis                                                    |
| ------------------------- | ------------------------------------------------------- |
| `sessionid`               | ID Twojej sesji (często losowe)                         |
| `auth_token`, `jwt`       | Token JWT lub inny do autoryzacji                       |
| `isAdmin=1`, `user=admin` | Czasem aplikacje trzymają role w cookies (złe praktyki) |
| `lang=en`, `theme=dark`   | Preferencje użytkownika                                 |
| `csrftoken=XYZ`           | Token anty-CSRF                                         |

---

###  Atrybuty cookies (bardzo ważne!)

| Atrybut       | Znaczenie                                   | Bezpieczeństwo                       |
| ------------- | ------------------------------------------- | ------------------------------------ |
| `HttpOnly`    | Nie można odczytać przez JS                 | 🔒 Chroni przed XSS kradnącym cookie |
| `Secure`      | Wysyłane tylko przez HTTPS                  | 🔒 Chroni przed sniffingiem na HTTP  |
| `SameSite`    | Ogranicza wysyłanie do "tej samej strony"   | 🔒 Chroni przed CSRF                 |
| `Path=/admin` | Ogranicza użycie do konkretnego patha       | 👀 Czasem pozwala coś obejść         |
| `Domain`      | Umożliwia współdzielenie między subdomenami | 🔥 Ryzykowne przy wielu subdomenach  |

---

### Czym jest sesja?

* **Stan po stronie serwera** – np. kto jesteś, czy jesteś zalogowany.
* Identyfikator sesji (np. `sessionid=abc123`) trzymany jest w **cookie**.
* Backend wie, że `abc123` = np. użytkownik `adam`, rola `admin`.

**Uwaga:** Jeśli możesz zgadnąć / ukraść `sessionid` → **przejąłeś konto** (Session Hijacking).

---

###  Tokeny autoryzacyjne (np. JWT)

Zamiast sesji na serwerze można używać **tokenów** – np. **JWT (JSON Web Token)**.

**Przykład tokena JWT:**

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhZG1pbiI6dHJ1ZX0.signature
```

Składa się z 3 części:

* **Header** – info o algorytmie (`alg`, `typ`)
* **Payload** – dane użytkownika (`admin`, `id`, `email`)
* **Signature** – podpis cyfrowy

**Zaleta**: Nie trzeba trzymać nic po stronie serwera
**Ryzyko**: Jeśli JWT jest źle skonfigurowany, można go podrobić.

---

### Co atakujemy?

| Co widzisz?               | Co możesz zrobić?                                                                                                |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| Cookie `sessionid=XYZ`    | 🔸 Próbuj przejąć (XSS, MitM, bruteforce ID)<br>🔸 Próbuj go zmienić                                             |
| Cookie `isAdmin=true`     | 🔸 Podmień na `false` / `1` / `0` i testuj dostęp                                                                |
| JWT token w Authorization | 🔸 Zmień payload (`admin=true`) i zobacz czy działa<br>🔸 Sprawdź alg=none<br>🔸 Spróbuj brute-force HMAC klucza |
| Brak `HttpOnly`           | 🔥 XSS → document.cookie = kradzież sesji                                                                        |
| Brak `SameSite`           | 🔥 CSRF możliwy                                                                                                  |
| Zły `Domain`              | 🔥 Można przejąć subdomenę i ukraść cookies                                                                      |

---

###  Jak testować w Burpie?

* Zobacz zakładkę **Cookies** w requestach i odpowiedziach
* Sprawdzaj:

  * Czy `Set-Cookie` ustawia bezpieczne flagi?
  * Czy możesz ręcznie edytować cookie i zyskać dostęp?
  * Czy możesz przejąć sesję?
* JWT można łatwo rozkodować (np. [jwt.io](https://jwt.io)) i fuzzować payloady

---

###  Klasyczne ataki:

| Atak                     | Opis                                                 |
| ------------------------ | ---------------------------------------------------- |
| **Session Fixation**     | Wymuszasz sesję na ofierze, a potem ją przejmujesz   |
| **Session Hijacking**    | Kradniesz cudze cookies (np. przez XSS)              |
| **JWT alg=none**         | JWT bez podpisu = każdy może wygenerować token       |
| **Privilege Escalation** | Masz token usera → zmieniasz payload na `admin=true` |
| **CSRF**                 | Wysyłasz request ofiary z jej cookies bez jej wiedzy |

---
