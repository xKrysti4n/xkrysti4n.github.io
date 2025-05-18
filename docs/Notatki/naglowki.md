title:📦 Nagłówki HTTP
---

## 📦 Czym są nagłówki HTTP?

Nagłówki HTTP to **dodatkowe dane przesyłane w żądaniu (request)** lub odpowiedzi (response). Służą np. do identyfikacji klienta, autoryzacji, przesyłania ciasteczek, itp.

---

## 🔑 Kluczowe nagłówki HTTP

### 🧠 `User-Agent`

* Opisuje **przeglądarkę lub klienta**, który wysyła żądanie.
* Przykład:

  ```
  User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) Chrome/124.0
  ```
* Można go łatwo **spoofować**, np. przy crawlingu albo testach botów.

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

* Wskazuje, **do jakiego hosta** kierowane jest żądanie.
* Przykład:

  ```
  Host: login.mojadomena.pl
  ```
* Umożliwia serwerowi rozróżnić, do której domeny klient się odwołuje (ważne przy **virtual hosting**).

🔍 W pentestingu: testujesz **Host Header Injection**, np. przekierowania zależne od `Host`.

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

* Przesyła **ciasteczka** (np. sesyjne, identyfikacyjne).
* Przykład:

  ```
  Cookie: sessionid=abc123; is_admin=true
  ```

🔍 W pentestingu:

* Testujesz **przejęcie sesji** (Session Fixation / Session Hijacking),
* Manipulujesz ciastkami, np. `is_admin=true`.

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

* Pokazuje **skąd przyszło żądanie**, czyli URL strony, z której użytkownik kliknął link.
* Przykład:

  ```
  Referer: https://example.com/login
  ```

🔍 W pentestingu:

* Możesz sprawdzać, czy aplikacja **ufa Refererowi**, np. w przypadku „tylko użytkownicy z naszej strony mogą...”.
* Przydaje się do testowania **CSRF**, jeśli Referer jest jedynym zabezpieczeniem.

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

* Pokazuje **domenę źródłową** bez ścieżki (bardziej zaufany niż Referer).
* Przykład:

  ```
  Origin: https://mojastrona.pl
  ```

🔍 W pentestingu:

* Bardzo ważny przy **CORS (Cross-Origin Resource Sharing)**!
* Jeśli Origin jest źle sprawdzany → **CORS misconfiguration** → można kraść dane z cudzych sesji.

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

* Przesyła **token lub dane logowania**.
* Przykład (Basic Auth):

  ```
  Authorization: Basic dXNlcjpwYXNz
  ```
* Przykład (Bearer token):

  ```
  Authorization: Bearer eyJhbGciOi...
  ```

🔍 W pentestingu:

* Testujesz czy tokeny są przewidywalne,
* Czy można używać tokenu z innym użytkownikiem,
* Czy można zadziałać bez nagłówka i uzyskać dostęp (Broken Access Control).

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

## 🔥 Cheat sheet

| Nagłówek        | Co robi?                                | Pentesting zastosowanie           |
| --------------- | --------------------------------------- | --------------------------------- |
| `User-Agent`    | Identyfikuje klienta (np. przeglądarkę) | Bypassy, bot detection, spoofing  |
| `Host`          | Do jakiej domeny jest żądanie           | Host Header Injection             |
| `Cookie`        | Przesyła dane sesji                     | Session Hijack / Manipulacja      |
| `Referer`       | Z jakiej strony przyszedł użytkownik    | CSRF, redirecty, sprawdzanie flow |
| `Origin`        | Z jakiej domeny pochodzi przeglądarka   | CORS exploity                     |
| `Authorization` | Dane logowania/token                    | Auth bypass, token replay, brute  |
