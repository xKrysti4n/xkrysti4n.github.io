# Metody HTTP

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

## Przykłady użycia

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

## Kody odpowiedzi

- 200 OK - pomyślne wykonanie żądania
- 201 Created - pomyślne utworzenie zasobu (POST)
- 204 No Content - pomyślne wykonanie bez zwracania danych (DELETE)
- 400 Bad Request - nieprawidłowe żądanie
- 401 Unauthorized - brak autoryzacji
- 403 Forbidden - brak uprawnień
- 404 Not Found - zasób nie istnieje
- 405 Method Not Allowed - metoda nie jest obsługiwana

## Zagrożenia bezpieczeństwa i potencjalne exploity

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
