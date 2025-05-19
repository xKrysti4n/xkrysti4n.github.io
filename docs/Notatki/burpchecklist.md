
## 🧠 **Burp Suite Exploitation Checklist**

### 🗂 Nagłówki HTTP:

| Co widzisz?                 | Co możesz przetestować?                                                                                       |
| --------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `Authorization: Bearer xyz` | 🔸 Zmień token na cudzy<br>🔸 Usuń token całkiem<br>🔸 Token JWT? → sprawdź alg=none, brute force klucza      |
| `Cookie: session=XYZ`       | 🔸 Podmień `session` (Insecure Direct Object Reference)<br>🔸 Dodaj `admin=true`, `isAdmin=1` itp.            |
| `Referer: ...`              | 🔸 Czy aplikacja wymaga konkretnego referera?<br>🔸 Czy można obejść CSRF albo redirect?                      |
| `Origin: evil.com`          | 🔸 Sprawdź odpowiedź CORS: `Access-Control-Allow-Origin: *`<br>🔸 Czy akceptuje nieautoryzowaną domenę?       |
| `Host: yourdomain.com`      | 🔸 Spróbuj `Host Header Injection`<br>🔸 Zmień hosta i sprawdź przekierowania / linki w mailach               |
| `User-Agent: ...`           | 🔸 Spoofowanie klienta (np. Googlebot = większy dostęp)<br>🔸 Fuzzuj — czasem różna logika dla mobilki / bota |

---

### 🔐 **Autoryzacja i sesje:**

| Co widzisz?                             | Co możesz przetestować?                                                                   |
| --------------------------------------- | ----------------------------------------------------------------------------------------- |
| ID w URL lub body (np. `/profile?id=5`) | 🔸 Zmień ID → IDOR test<br>🔸 Sprawdź, czy możesz wyświetlać cudze dane                   |
| Różne poziomy kont (user/admin)         | 🔸 Zaloguj się jako user → fuzzuj admin endpointy<br>🔸 Dodaj `X-User-Role: admin` itp.   |
| Panel admina (`/admin`)                 | 🔸 Sprawdź, czy masz dostęp bez uprawnień<br>🔸 Czasem jest ukryty tylko przez brak linka |

---

### 📤 **Formularze i payloady:**

| Co widzisz?                           | Co możesz przetestować?                                                                                    |
| ------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| Formularz logowania / rejestracji     | 🔸 Bruteforce (jeśli brak rate-limit)<br>🔸 SQLi w polach login/pass<br>🔸 Bypass loginów (np. `admin'--`) |
| Pole `email` lub `username`           | 🔸 XSS<br>🔸 SQLi<br>🔸 Enumeracja użytkowników (np. różne odpowiedzi: „email nie istnieje”)               |
| Pola typu `comment`, `message`, `bio` | 🔸 Stored XSS / Reflected XSS<br>🔸 Command Injection, jeśli trafia do shella                              |

---

### 💬 **Odpowiedzi serwera:**

| Co widzisz?                    | Co możesz przetestować?                                                                                  |
| ------------------------------ | -------------------------------------------------------------------------------------------------------- |
| `Set-Cookie: HttpOnly; Secure` | 🔸 Czy są flagi zabezpieczające?<br>🔸 Brak `HttpOnly` = możliwy XSS cookie theft                        |
| `Server: Apache/2.4.1`         | 🔸 Wersja → sprawdź CVE<br>🔸 Banner grabbing może pomóc w fingerprintingu                               |
| `X-Powered-By: Express`        | 🔸 Może wskazywać na Node.js → inne wektory ataku<br>🔸 Możesz testować podatności zależne od frameworka |

---

### 🧪 **Techniki testowe** (w Burpie):

* **Repeater** → ręczne testy, zmieniasz wartości i patrzysz na odpowiedzi
* **Intruder** → automatyczne fuzzowanie (np. ID, tokenów, payloadów)
* **Logger++ / Logger4** → logujesz wszystkie requesty/responsy
* **Comparer** → porównuj odpowiedzi (np. admin vs user)

---

## 📥 Chcesz więcej?

Mogę dorzucić konkretne payloady (XSS, SQLi, XXE, etc), checklistę CORS, CSRF, fuzzowania, bypassów auth...
Powiedz tylko, czy chcesz wersję do Logseq / Obsidian z checkboxami ✅📝

Albo — daj link do konkretnego labu, a zrobimy checklistę pod ten target 🛠️
