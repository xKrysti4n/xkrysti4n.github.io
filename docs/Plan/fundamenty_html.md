
# 🧱**Absolutne podstawy web i HTTP**

📅 *Czas: 5–7 dni przy codziennym tempie nauki (~1–2h dziennie)*

🎯 *Cel: Zrozumieć fundamenty komunikacji w internecie, czyli jak działa HTTP, requesty, cookies, sesje, CORS itd.*

---

## 📘 **1. Jak działa internet i HTTP**

### 🔍 Co musisz zrozumieć:

* Co to jest **HTTP / HTTPS**
* Czym jest **request** i **response**
* Czym są **metody HTTP**: `GET`, `POST`, `PUT`, `DELETE`, `OPTIONS`, `HEAD`
* Co to są **nagłówki HTTP** i co robią (np. `User-Agent`, `Host`, `Cookie`, `Referer`, `Origin`, `Authorization`)
* Co to jest **status code**: `200`, `301`, `302`, `403`, `404`, `500`, `401`...

### 🎓 Materiały do nauki:

* 📺 [Computerphile – HTTP Explained](https://www.youtube.com/watch?v=iYM2zFP3Zn0) *(świetny start!)*
* 📘 [MDN Web Docs – HTTP overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)
* 🧪 Zrób:
  W przeglądarce → PPM → „Zbadaj” → zakładka **Sieć / Network**
  Odśwież stronę i zobacz, jak wygląda request. Przeanalizuj:

  * URL
  * Method
  * Headers
  * Response

---

## 🍪 **2. Cookies, sesje i tokeny**

### 🔍 Co musisz wiedzieć:

* Co to są **ciasteczka** (cookies)
* Jak działają **sesje** — `JSESSIONID`, `PHPSESSID`, `ASP.NET_SessionId`
* Czym różni się sesja od **JWT tokena** (JSON Web Token)
* Czym są **Secure**, **HttpOnly**, **SameSite**, **Domain**, **Path** w cookies
* Jak wygląda przykład:

  ```
  Set-Cookie: sessionid=abc123; HttpOnly; Secure; SameSite=Strict
  ```

### 🎓 Materiały:

* 📘 [MDN – Cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)
* 📘 [Auth0 Blog – Cookies vs Tokens](https://auth0.com/docs/secure/tokens/cookies-vs-tokens)
* 📺 [JWT explained (YouTube)](https://www.youtube.com/watch?v=7Q17ubqLfaM)

### 🧪 Zrób:

* Otwórz konsolę sieci → sprawdź cookies przy logowaniu np. do forum / bloga
* Wejdź na stronę, która używa JWT (np. reqres.in), i zobacz jak wygląda token

---

## 🔐 **3. Same Origin Policy i CORS**

### 🔍 Co musisz wiedzieć:

* Czym jest **Origin** w przeglądarce
* Dlaczego nie można robić requestów JS do innego hosta (SOP)
* Co to jest **CORS** – Cross-Origin Resource Sharing
* Jak wygląda nagłówek `Access-Control-Allow-Origin`

### 🎓 Materiały:

* 📘 [MDN – Same-origin policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)
* 📘 [MDN – CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)
* 📺 [YouTube – CORS in 100 seconds](https://www.youtube.com/watch?v=Ka8vG5miErE)

### 🧪 Zrób:

* Użyj `curl` albo Burp Repeater i wyślij request z podrobionym `Origin` → zobacz czy działa
* Przykład:

  ```http
  GET / HTTP/1.1
  Host: target.com
  Origin: evil.com
  ```

---

🗂️**4. Zrób notatki / writeup z etapu 1**

📁 Folder w MkDocs lub Logseq: `web_basics/`

Przykład struktury:

```
web_basics/
├── http_methods.md
├── cookies_vs_sessions.md
├── jwt.md
├── headers.md
├── cors.md
```

### 🎯 W każdej notatce:

* 🔹 definicja
* 🔹 realny przykład (np. z Burpa / przeglądarki)
* 🔹 czemu to ważne w kontekście testowania

---

✅ **Checklist na koniec etapu:**

| Umiejętność                                       | ✔️/❌ |
| ------------------------------------------------- | ---- |
| Wiesz co to GET vs POST                           |      |
| Wiesz jak wygląda pełny HTTP request i response   |      |
| Rozumiesz cookies, sesje, tokeny                  |      |
| Wiesz czym jest SOP i CORS                        |      |
| Potrafisz przechwycić request i go przeanalizować |      |

---

