title: OWASP 10

# CSRF (Cross-Site Request Forgery) – Kompletny Przewodnik

##CSRF

CSRF to atak polegający na **wymuszeniu na zalogowanym użytkowniku wykonania nieautoryzowanego żądania do aplikacji webowej**, w której jest aktualnie uwierzytelniony.

* Przykład: ofiara klikając link lub odwiedzając stronę atakującego, nieświadomie zmienia swój email w banku.
* Przeglądarka automatycznie dołącza ciasteczka sesyjne (np. sesja, token uwierzytelniający), co umożliwia atak.

---

### Czy CSRF jest w OWASP Top 10?

* **Był w OWASP Top 10 do 2017 roku**, jako osobna kategoria (np. A8: CSRF).
* **Od 2017 roku usunięty jako osobny punkt**, bo:

  * Frameworki webowe wprowadziły ochronę domyślną (tokeny CSRF).
  * Przeglądarki dodały `SameSite` do ciasteczek.
  * Liczba podatnych aplikacji spadła.
* Nadal jest traktowany jako część:

  * **A05:2021 Security Misconfiguration**
  * **A01:2021 Broken Access Control**

---

### Jak działa atak CSRF?

1. Użytkownik jest zalogowany w podatnej aplikacji (np. banku).
2. Odwiedza złośliwą stronę, reklamę lub klika link.
3. Złośliwa strona wysyła automatyczne żądanie (POST/PUT/DELETE) do aplikacji ofiary.
4. Przeglądarka dołącza ciasteczka sesji ofiary.
5. Serwer akceptuje żądanie bez dodatkowej weryfikacji → akcja zostaje wykonana.

---

### Token CSRF – co to jest?

* To losowy, unikalny token generowany przez serwer dla sesji użytkownika.
* Wysyłany do klienta i musi zostać odesłany z każdym żądaniem zmieniającym stan.
* Serwer sprawdza, czy token jest poprawny i należy do sesji użytkownika.
* Brak lub błędny token oznacza odrzucenie żądania → ochrona przed CSRF.

---

### Jak testować podatność na CSRF?

### Krok 1: Znajdź podatne żądania

* Szukaj w Burp Suite lub przeglądarce:

  * Żądań POST, PUT, DELETE zmieniających dane.
  * Braku tokena CSRF w formularzu lub nagłówkach.
  * Obecności ciasteczka sesji w żądaniu.

### Krok 2: Stwórz prosty exploit HTML (PoC)

```html
<form action="https://oficjalna-strona.com/zmien_email" method="POST">
  <input type="hidden" name="email" value="atakuj@zly.pl" />
  <input type="submit" value="Kliknij mnie" />
</form>

<script>
  document.forms[0].submit();
</script>
```

* Zapisz jako `csrf.html` i otwórz lokalnie.
* Jeśli serwer nie ma ochrony → zmiana zostanie wykonana automatycznie.

### Krok 3: Sprawdź, czy serwer wymaga tokena

* Jeśli bez tokena żądanie jest odrzucane — dobra ochrona.
* Jeśli serwer akceptuje — jest podatny.

### Krok 4: Sprawdź dodatkowo nagłówki

* Niektóre serwery próbują chronić się sprawdzając `Origin` lub `Referer`.
* Ta metoda jest mniej bezpieczna i może być obejścia.

---

### Dodatkowe rzeczy do sprawdzenia

* **SameSite cookie** — czy jest ustawione na `Strict` lub `Lax`?
  Jeśli tak — znacznie zmniejsza ryzyko CSRF.
* Testuj różne metody HTTP, bo CSRF może działać na PUT, DELETE, itp.
* Użyj rozszerzeń Burp Suite (np. CSRF Scanner) do automatycznego wykrywania.

---

### Podsumowanie

* CSRF to wciąż realne zagrożenie, szczególnie w starszych i customowych aplikacjach.
* Najlepszą ochroną jest stosowanie **tokenów CSRF** oraz **SameSite cookies**.
* Testowanie polega na wysyłaniu żądań bez tokena i obserwowaniu reakcji serwera.
* CSRF nie jest już osobną kategorią w OWASP Top 10, ale jest wciąż ważnym problemem.

---
