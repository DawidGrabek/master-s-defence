# Notatki na Egzamin Magisterski - Informatyka (II stopień)

## 1. Przygotowanie i publikowanie artykułów naukowych

### 1.1. Schemat IMRaD

**IMRaD** - skrót od Introduction, Methods, Results, and Discussion - standardowa struktura artykułów naukowych.

**Struktura w kształcie klepsydry:**
- **Introduction (Wprowadzenie)**: kontekst, stan wiedzy, cel badania, hipotezy
- **Methods & Materials (Metody i materiały)**: opis procedury badawczej, materiały, metody statystyczne
- **Results (Wyniki)**: prezentacja wyników bez interpretacji (tabele, wykresy)  
- **Discussion (Dyskusja)**: interpretacja wyników, ograniczenia, znaczenie dla dyscypliny

**Przykład zastosowania:** Artykuł o skuteczności nowego leku - wprowadzenie opisuje problem, metody przedstawiają protokół badania, wyniki pokazują dane statystyczne, dyskusja interpretuje znaczenie kliniczne.

### 1.2. Identyfikator ORCID

**ORCID** (Open Researcher and Contributor ID) - unikalny identyfikator naukowca składający się z 16 cyfr.

**Charakterystyka:**
- Format: https://orcid.org/0000-0002-1825-0097
- Eliminuje problemy z duplikacją nazwisk
- Rejestracja bezpłatna
- Integracja z bazami Scopus, Web of Science
- Wymagany przez wydawców (IEEE, Elsevier, Springer)

**Przykład:** Dr Jan Kowalski ma ORCID 0000-0002-1234-5678, co pozwala jednoznacznie zidentyfikować jego publikacje niezależnie od zmian nazwiska czy afiliacji.

### 1.3. Rodzaje artykułów naukowych

**Główne typy publikacji:**
- **Artykuły oryginalne**: prezentują nowe wyniki badań empirycznych
- **Artykuły przeglądowe (Review)**: synteza i analiza istniejącego piśmiennictwa
- **Komunikaty krótkie**: zwięzłe raporty z wstępnych wyników
- **Studia przypadków**: analiza konkretnych przypadków (głównie medycyna)
- **Metaanalizy**: statystyczna analiza wyników z wielu badań

**Przykład:** Artykuł o nowym algorytmie sortowania byłby artykułem oryginalnym, podczas gdy przegląd metod sortowania ostatniej dekady to artykuł przeglądowy.

### 1.4. Wskaźniki bibliometryczne. Lista Filadelfijska

**Główne wskaźniki:**
- **Impact Factor (IF)**: stosunek cytowań z 2 poprzednich lat do liczby artykułów
- **H-index**: h publikacji cytowanych h razy lub więcej  
- **CiteScore**: średnia cytowań z ostatnich 3 lat (Scopus)
- **SNIP, SJR**: wskaźniki znormalizowane z bazy Scopus

**Lista Filadelfijska:**
- Polska nazwa dla ISI Master Journal List
- Czasopisma ocenione przez Thomson Reuters
- NIE zawiera wartości Impact Factor (tylko dane bibliograficzne)
- Była podstawą polskiego systemu punktowego

**Przykład:** Czasopismo z IF=2,5 oznacza, że artykuły z ostatnich 2 lat były cytowane przeciętnie 2,5 raza.

### 1.5. Bazy danych bibliograficznych. Baza Scopus

**Baza Scopus (Elsevier):**
- Interdyscyplinarna baza abstraktów i cytowań
- Ponad 70 mln rekordów, 23 tys. czasopism
- Zakres chronologiczny: od 1960 roku
- Dostępna w Polsce w ramach licencji krajowej
- Narzędzia: SciVal do analiz bibliometrycznych

**Inne bazy:**
- **Web of Science**: konkurencyjna baza (Clarivate Analytics)
- **Google Scholar**: otwarty dostęp, szerszy zakres
- **PubMed**: specjalistyczna baza medyczna

**Przykład użycia:** Wyszukiwanie "machine learning AND healthcare" w Scopus zwróci artykuły z recenzowanych czasopism z informacjami o cytowaniach.

---

## 2. Zaawansowana eksploracja danych

### 2.1. Metody identyfikacji obserwacji odstających

**Definicja:** Obserwacje odstające (outliers) to wartości znacząco różniące się od pozostałych danych.

**Główne podejścia:**
1. **Oparte na rozkładach**: Reguła 3-sigma (wartości poza μ ± 3σ)
2. **Oparte na odległościach**: Odległość Mahalanobisa, metoda IQR (Q1-1.5*IQR, Q3+1.5*IQR)
3. **Oparte na gęstości**: Local Outlier Factor (LOF), DBSCAN

**Przykład:** W zbiorze wynagrodzeń [2000, 2100, 2200, 2050, 50000], wartość 50000 to outlier wykryty metodą IQR.

### 2.2. Metody estymacji gęstości rozkładu prawdopodobieństwa

**Podejścia:**
1. **Parametryczne**: Założenie znanej postaci rozkładu (np. Gaussa), estymacja parametrów
2. **Nieparametryczne**: 
   - **Histogram**: podział na przedziały o równej szerokości
   - **Estymatory jądrowe**: f̂(x) = (1/nh) Σ K((x-xi)/h), gdzie K to funkcja jądra, h to parametr wygładzania

**Przykład:** Dla danych o wzroście studentów można założyć rozkład normalny (podejście parametryczne) lub użyć estymatora jądrowego z jądrem Gaussa.

### 2.3. ANOVA, MANOVA

**ANOVA (Analysis of Variance):**
- Porównuje średnie w ≥3 grupach
- Test F: F = MS_between / MS_within
- Typy: jednoczynnikowa, wieloczynnikowa, dla pomiarów powtarzanych

**MANOVA (Multivariate ANOVA):**
- Wielowymiarowa wersja ANOVA dla >1 zmiennej zależnej
- Nie wymaga założenia sferyczności
- Używana gdy zmienne zależne są skorelowane

**Przykład:** Porównanie skuteczności 3 metod nauczania na wyniki z matematyki (ANOVA) vs. na wyniki z matematyki i fizyki jednocześnie (MANOVA).

### 2.4. Modele regresji

**Regresja liniowa:**
- Model: y = β₀ + β₁x₁ + ... + βₖxₖ + ε
- Metoda najmniejszych kwadratów (MNK)

**Regresja nieliniowa:**
- Funkcje wykładnicze, logarytmiczne, trygonometryczne
- Algorytmy: Levenberg-Marquardt, Nelder-Mead
- Typy: nieliniowa względem zmiennych vs. parametrów

**Przykład:** Zależność między czasem nauki a wynikiem egzaminu może być liniowa, ale zależność między dawką leku a skutecznością często wymaga modelu nieliniowego.

### 2.5. Metody redukcji wymiaru i liczności próby

**PCA (Principal Component Analysis):**
- Transformacja do przestrzeni składowych głównych
- Składowe = kierunki największej wariancji
- Procent wyjaśnionej wariancji: (λ₁+...+λₖ)/(λ₁+...+λₚ) × 100%

**Inne metody:**
- **Analiza czynnikowa**: wykrywanie ukrytej struktury
- **Skalowanie wielowymiarowe**: zachowanie odległości
- **t-SNE**: wizualizacja nieliniowa

**Przykład:** Zredukowanie 100 cech produktów do 5 składowych głównych zachowując 95% wariancji danych.

### 2.6. Metody analizy skupień

**Hierarchiczne:**
- **Aglomeracyjne**: od pojedynczych obiektów do jednego skupienia
- **Dzielące**: od jednego skupienia do pojedynczych obiektów
- Wynik: dendrogram

**Niehierarchiczne:**
- **K-means**: minimalizacja wariancji wewnątrz skupień
- **DBSCAN**: oparte na gęstości punktów
- Wymagają podania liczby skupień (k-means)

**Przykład:** Grupowanie klientów na podstawie zachowań zakupowych - k-means dla 3 segmentów lub hierarchicznie bez z góry określonej liczby grup.

### 2.7. Filtry splotowe. Sieci konwolucyjne

**CNN (Convolutional Neural Networks):**
- **Warstwy konwolucyjne**: filtry wykrywające cechy lokalne
- **Operacja splotu**: (I * K)(i,j) = ΣΣ I(i+m,j+n)K(m,n)  
- **Pooling**: redukcja rozmiaru (max pooling, average pooling)
- **Funkcja aktywacji**: ReLU, Softmax

**Zastosowania:** rozpoznawanie obrazów, widzenie komputerowe, przetwarzanie sygnałów

**Przykład:** CNN do rozpoznawania cyfr - pierwsza warstwa wykrywa krawędzie, kolejne bardziej złożone wzory, ostatnie całe cyfry.

### 2.8. Metody uczenia maszynowego

**Uczenie nadzorowane:**
- **Klasyfikacja**: SVM, drzewa decyzyjne, k-NN, Naive Bayes, lasy losowe
- **Regresja**: regresja liniowa, wielomianowa, drzewa regresyjne

**Uczenie nienadzorowane:**
- **Grupowanie**: k-means, hierarchiczne
- **Redukcja wymiaru**: PCA, t-SNE
- **Reguły asocjacyjne**: Apriori, FP-Growth

**Przykład:** Klasyfikacja e-maili jako spam (nadzorowana) vs. grupowanie klientów bez znanych kategorii (nienadzorowana).

### 2.9. Ocena jakości modeli klasyfikacyjnych

**Macierz pomyłek (Confusion Matrix):**
- TP (True Positive), TN (True Negative), FP (False Positive), FN (False Negative)

**Metryki:**
- **Accuracy**: (TP+TN)/(TP+TN+FP+FN)
- **Precision**: TP/(TP+FP) - precyzja
- **Recall/Sensitivity**: TP/(TP+FN) - czułość  
- **Specificity**: TN/(TN+FP) - swoistość
- **F1-score**: 2×(Precision×Recall)/(Precision+Recall)
- **AUC-ROC**: pole pod krzywą ROC

**Przykład:** Model wykrywający choroby - wysoka czułość ważna (wykryć wszystkich chorych), ale też precyzja (nie niepokojić zdrowych).

### 2.10. Ocena jakości modeli regresyjnych i prognozujących

**Metryki błędów:**
- **MSE**: (1/n)Σ(yi - ŷi)² - średni błąd kwadratowy
- **RMSE**: √MSE - w tych samych jednostkach co zmienna
- **MAE**: (1/n)Σ|yi - ŷi| - średni błąd bezwzględny
- **MAPE**: średni błąd procentowy bezwzględny

**Metryki dopasowania:**
- **R²**: współczynnik determinacji (0-1, gdzie 1 = idealne dopasowanie)
- **Adjusted R²**: skorygowany o liczbę zmiennych

**Przykład:** Model przewidujący ceny mieszkań z RMSE=50000 PLN oznacza średni błąd ±50k PLN. R²=0.85 oznacza wyjaśnienie 85% wariancji cen.

---

## 3. Metody wnioskowania wielokryterialnego

### 3.1. Główne zadania normalizacji wartości kryteriów optymalizacji

**Cele normalizacji:**
- Sprowadzenie różnych kryteriów do porównywalnej skali
- Eliminacja wpływu różnych jednostek miary  
- Zapewnienie równego wpływu wszystkich kryteriów

**Metody normalizacji:**
- Liniowa: xi' = (xi - min)/(max - min)
- Min-max do przedziału [a,b]: xi' = a + (xi - min)(b-a)/(max - min)
- Z-score: xi' = (xi - μ)/σ

**Przykład:** Przy wyborze samochodu mamy cenę (PLN), zużycie (l/100km), moc (KM) - bez normalizacji cena dominowałaby ze względu na wielkość wartości.

### 3.2. Metoda leksykograficzna

**Zasada:** Hierarchiczne uporządkowanie kryteriów według ważności i optymalizacja kolejno.

**Procedura:**
1. Uszeregowanie kryteriów według preferencji (1 = najważniejsze)
2. Optymalizacja względem najważniejszego kryterium  
3. Optymalizacja względem kolejnego z dozwoloną tolerancją ε dla poprzednich
4. Kontynuacja dla wszystkich kryteriów

**Przykład:** Wybór mieszkania: 1) lokalizacja, 2) cena, 3) powierzchnia. Najpierw wybieramy najlepszą dzielnicę, potem najtańsze z dozwoloną odległością od centrum, na końcu największe.

### 3.3. Wyznaczanie wag ważności kryteriów w metodzie AHP

**AHP (Analytic Hierarchy Process) - metoda Saaty'ego:**

**Skala porównań (1-9):**
- 1: równe znaczenie
- 3: słaba przewaga  
- 5: silna przewaga
- 7: bardzo silna przewaga
- 9: ekstremalna przewaga

**Procedura:**
1. Porównania parami wszystkich kryteriów
2. Utworzenie macierzy preferencji A gdzie aij = ważność i względem j
3. Obliczenie wektora własnego odpowiadającego największej wartości własnej
4. Normalizacja - suma wag = 1

**Przykład:** Porównując kryteria A, B, C: A jest 3 razy ważniejsze od B, B równe C, to wagi będą w przybliżeniu [0.6, 0.2, 0.2].

### 3.4. Warianty należące do zbioru optymalnych w sensie Pareto

**Definicja:** Rozwiązanie x* jest Pareto-optymalne jeśli nie istnieje inne rozwiązanie x lepsze we wszystkich kryteriach i równe w pozostałych.

**Charakterystyka:**
- Zbiór rozwiązań niezdominowanych
- Front Pareto = obraz zbioru w przestrzeni kryteriów  
- Każde rozwiązanie Pareto może być optymalne przy odpowiednich preferencjach

**Przykład:** Wybór samochodu - model A (drogi, ekonomiczny), model B (tani, żarłoczny). Oba są Pareto-optymalne bo nie ma modelu tańszego i bardziej ekonomicznego jednocześnie.

### 3.5. Metoda Blina

**Charakterystyka:** Jedna z metod wielokryterialnego podejmowania decyzji służąca do wyboru rozwiązania ze zbioru Pareto.

**Zasada działania:**
- Wykorzystuje informacje o preferencjach decydenta
- Buduje funkcję użyteczności agregującą wszystkie kryteria
- Znajduje rozwiązanie maksymalizujące tę funkcję

**Zastosowanie:** Gdy decydent może określić swoje preferencje między kryteriami, ale potrzebuje pomocy w znalezieniu optymalnego kompromisu.

**Przykład:** W planowaniu inwestycji gdzie mamy kryteria: zysk, ryzyko, płynność - metoda Blina pomaga znaleźć portfel optymalny przy danych preferencjach ryzyka.

---

## 4. Programowanie aplikacji w języku PL/SQL

### 4.1. Budowa bloku PL/SQL

**Struktura bloku:**
```sql
DECLARE
    -- Sekcja deklaracyjna (opcjonalna)
    -- zmienne, kursory, wyjątki
BEGIN  
    -- Sekcja wykonywalna (obowiązkowa)
    -- instrukcje SQL i PL/SQL
EXCEPTION
    -- Sekcja obsługi wyjątków (opcjonalna)
    -- obsługa błędów
END;
```

**Rodzaje bloków:**
- **Anonimowe**: bez nazwy, kompilowane przy każdym wykonaniu
- **Procedury i funkcje**: nazwane, kompilowane raz
- **Pakiety**: grupy procedur i funkcji

**Przykład:**
```sql
DECLARE
    v_liczba NUMBER := 10;
BEGIN
    DBMS_OUTPUT.PUT_LINE('Wartość: ' || v_liczba);
EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Błąd: ' || SQLERRM);
END;
```

### 4.2. Kursory i ich wykorzystanie

**Definicja:** Kursor to obszar roboczy do przechowywania wyników zapytania SELECT.

**Operacje na kursorach:**
1. **DECLARE**: deklaracja kursora
2. **OPEN**: otwarcie i wykonanie zapytania  
3. **FETCH**: pobranie wiersza
4. **CLOSE**: zamknięcie kursora

**Atrybuty kursora:**
- %FOUND, %NOTFOUND: czy pobrano wiersz
- %ROWCOUNT: liczba pobranych wierszy
- %ISOPEN: czy kursor jest otwarty

**Przykład:**
```sql
DECLARE
    CURSOR c_emp IS SELECT name, salary FROM employees;
    v_name VARCHAR2(50);
    v_sal NUMBER;
BEGIN
    OPEN c_emp;
    LOOP
        FETCH c_emp INTO v_name, v_sal;
        EXIT WHEN c_emp%NOTFOUND;
        DBMS_OUTPUT.PUT_LINE(v_name || ': ' || v_sal);
    END LOOP;
    CLOSE c_emp;
END;
```

### 4.3. Pętla kursora

**Cursor FOR LOOP:** automatycznie otwiera, pobiera i zamyka kursor.

**Zalety:**
- Automatyczna deklaracja zmiennej typu %ROWTYPE
- Automatyczne otwarcie i zamknięcie
- Automatyczne pobieranie kolejnych wierszy
- Brak konieczności sprawdzania %NOTFOUND

**Przykład:**
```sql
DECLARE
    CURSOR c_emp IS SELECT * FROM employees WHERE dept_id = 10;
BEGIN
    FOR emp_rec IN c_emp LOOP
        DBMS_OUTPUT.PUT_LINE(emp_rec.first_name || ' ' || emp_rec.last_name);
        -- emp_rec automatycznie typu employees%ROWTYPE
    END LOOP;
END;
```

### 4.4. Wyjątki w języku PL/SQL

**Rodzaje wyjątków:**
- **Predefiniowane systemowe**: NO_DATA_FOUND, TOO_MANY_ROWS, ZERO_DIVIDE
- **Użytkownika**: definiowane przez programistę

**Obsługa wyjątków:**
```sql
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        -- obsługa braku danych
    WHEN TOO_MANY_ROWS THEN  
        -- obsługa wielu wierszy
    WHEN OTHERS THEN
        -- obsługa pozostałych wyjątków
```

**Przykład:**
```sql
DECLARE
    v_count NUMBER;
    e_too_many EXCEPTION;
BEGIN
    SELECT COUNT(*) INTO v_count FROM employees;
    IF v_count > 100 THEN
        RAISE e_too_many;
    END IF;
EXCEPTION
    WHEN e_too_many THEN
        DBMS_OUTPUT.PUT_LINE('Za dużo pracowników');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Błąd: ' || SQLERRM);
END;
```

### 4.5. Funkcje i procedury PL/SQL

**Procedura:**
```sql
CREATE OR REPLACE PROCEDURE proc_name(param1 IN VARCHAR2, param2 OUT NUMBER)
IS
    v_local NUMBER;
BEGIN
    -- kod procedury
    param2 := 100;
END;
```

**Funkcja:**
```sql
CREATE OR REPLACE FUNCTION func_name(param1 NUMBER) RETURN VARCHAR2
IS
    v_result VARCHAR2(50);
BEGIN
    -- kod funkcji
    RETURN v_result;
END;
```

**Tryby parametrów:**
- **IN**: tylko do odczytu (domyślny)
- **OUT**: tylko do zapisu
- **IN OUT**: do odczytu i zapisu

### 4.6. Procedury i funkcje składowane

**Charakterystyka:**
- Przechowywane w bazie danych
- Kompilowane raz przy tworzeniu
- Udostępniane innym użytkownikom
- Lepsze bezpieczeństwo i wydajność

**Zastosowania:**
- Logika biznesowa w bazie
- Kontrola dostępu do danych
- Optymalizacja wydajności
- Wielokrotne wykorzystanie kodu

**Przykład:**
```sql
CREATE OR REPLACE FUNCTION get_employee_bonus(emp_id NUMBER) 
RETURN NUMBER
IS
    v_salary NUMBER;
    v_bonus NUMBER;
BEGIN
    SELECT salary INTO v_salary 
    FROM employees WHERE employee_id = emp_id;

    v_bonus := v_salary * 0.1;
    RETURN v_bonus;
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        RETURN 0;
END;
```

### 4.7. Kolekcje w PL/SQL

**Rodzaje kolekcji:**
1. **Tablice asocjacyjne** (INDEX BY): sparse, indeks typu VARCHAR2 lub INTEGER
2. **Nested Tables**: gęste, mogą być przechowywane w tabeli
3. **VARRAYs**: o stałym maksymalnym rozmiarze

**Przykład:**
```sql
DECLARE
    TYPE emp_array IS TABLE OF VARCHAR2(50) INDEX BY BINARY_INTEGER;
    v_employees emp_array;
BEGIN
    v_employees(1) := 'Jan Kowalski';
    v_employees(2) := 'Anna Nowak';

    FOR i IN 1..v_employees.COUNT LOOP
        DBMS_OUTPUT.PUT_LINE(v_employees(i));
    END LOOP;
END;
```

### 4.8. Wyzwalacze bazy danych

**Definicja:** Specjalne procedury automatycznie wykonywane w odpowiedzi na wydarzenia w bazie danych.

**Typy wyzwalaczy:**
- **BEFORE/AFTER**: przed/po operacji
- **INSERT/UPDATE/DELETE**: typ operacji
- **FOR EACH ROW**: wierszowe vs. instrukcyjne

**Przykład:**
```sql
CREATE OR REPLACE TRIGGER trg_audit_employees
    AFTER UPDATE ON employees
    FOR EACH ROW
BEGIN
    INSERT INTO employee_audit(emp_id, old_salary, new_salary, change_date)
    VALUES (:OLD.employee_id, :OLD.salary, :NEW.salary, SYSDATE);
END;
```

### 4.9. Pakiety PL/SQL

**Struktura pakietu:**
- **Specyfikacja**: interfejs publiczny (typy, zmienne, procedury, funkcje)
- **Ciało**: implementacja

**Przykład:**
```sql
-- Specyfikacja
CREATE OR REPLACE PACKAGE pkg_employee AS
    TYPE emp_rec IS RECORD (
        emp_id NUMBER,
        name VARCHAR2(100)
    );

    FUNCTION get_employee_name(p_id NUMBER) RETURN VARCHAR2;
    PROCEDURE update_salary(p_id NUMBER, p_new_sal NUMBER);
END pkg_employee;

-- Ciało pakietu
CREATE OR REPLACE PACKAGE BODY pkg_employee AS
    FUNCTION get_employee_name(p_id NUMBER) RETURN VARCHAR2 IS
        v_name VARCHAR2(100);
    BEGIN
        SELECT first_name || ' ' || last_name INTO v_name
        FROM employees WHERE employee_id = p_id;
        RETURN v_name;
    END;

    PROCEDURE update_salary(p_id NUMBER, p_new_sal NUMBER) IS
    BEGIN
        UPDATE employees 
        SET salary = p_new_sal 
        WHERE employee_id = p_id;
    END;
END pkg_employee;
```

### 4.10. Dynamiczny SQL i PL/SQL

**Zastosowania:**
- Budowanie zapytań w runtime
- Nazwy tabel/kolumn znane dopiero w runtime
- DDL w PL/SQL (CREATE, DROP)

**EXECUTE IMMEDIATE:**
```sql
DECLARE
    v_sql VARCHAR2(1000);
    v_table_name VARCHAR2(30) := 'employees';
    v_count NUMBER;
BEGIN
    v_sql := 'SELECT COUNT(*) FROM ' || v_table_name;
    EXECUTE IMMEDIATE v_sql INTO v_count;
    DBMS_OUTPUT.PUT_LINE('Liczba wierszy: ' || v_count);

    -- DDL
    EXECUTE IMMEDIATE 'CREATE TABLE temp_table (id NUMBER)';
END;
```

**REF CURSOR z dynamicznym SQL:**
```sql
DECLARE
    TYPE ref_cur IS REF CURSOR;
    v_cursor ref_cur;
    v_emp_name VARCHAR2(100);
    v_sql VARCHAR2(500);
BEGIN
    v_sql := 'SELECT first_name || '' '' || last_name FROM employees WHERE department_id = :dept_id';
    OPEN v_cursor FOR v_sql USING 10;

    LOOP
        FETCH v_cursor INTO v_emp_name;
        EXIT WHEN v_cursor%NOTFOUND;
        DBMS_OUTPUT.PUT_LINE(v_emp_name);
    END LOOP;
    CLOSE v_cursor;
END;
```
