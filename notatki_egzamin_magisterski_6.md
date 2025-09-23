
---

## 13. Wzorce projektowe i techniki pisania czystego kodu

### 1. Konstrukcyjne wzorce projektowe (Creational Patterns)  
Służą do tworzenia obiektów w elastyczny i kontrolowany sposób.

- **Singleton** – gwarantuje istnienie tylko jednej instancji klasy.  
- **Factory Method** – definiuje interfejs do tworzenia obiektów, pozwalając podklasom zdecydować, który obiekt utworzyć.  
- **Abstract Factory** – dostarcza interfejs do tworzenia rodzin powiązanych obiektów bez określania ich klas.  
- **Builder** – oddziela konstrukcję złożonego obiektu od jego reprezentacji.  
- **Prototype** – tworzy nowe obiekty przez klonowanie istniejących.

### 2. Strukturalne wzorce projektowe (Structural Patterns)  
Pomagają organizować i łączyć klasy i obiekty w większe struktury.

- **Adapter** – pozwala współpracować niezgodnym interfejsom.  
- **Bridge** – oddziela abstrakcję od implementacji.  
- **Composite** – reprezentuje hierarchie obiektów w strukturze drzewiastej.  
- **Decorator** – dynamicznie dodaje nowe funkcjonalności obiektowi.  
- **Facade** – upraszcza interfejs złożonego systemu.  
- **Flyweight** – współdzieli obiekty w celu optymalizacji pamięci.  
- **Proxy** – zastępca obiektu, kontrolujący do niego dostęp.

### 3. Behawioralne wzorce projektowe (Behavioral Patterns)  
Definiują sposób komunikacji i odpowiedzialności między obiektami.

- **Chain of Responsibility** – przekazywanie żądań przez łańcuch obiektów.  
- **Command** – enkapsuluje żądanie jako obiekt.  
- **Iterator** – umożliwia sekwencyjny dostęp do elementów kolekcji.  
- **Mediator** – centralizuje komunikację między obiektami.  
- **Memento** – zapisuje i odtwarza stan obiektu.  
- **Observer** – mechanizm publish–subscribe.  
- **State** – zmienia zachowanie obiektu w zależności od stanu.  
- **Strategy** – definiuje rodzinę algorytmów wymiennych w czasie działania.  
- **Template Method** – szkielet algorytmu z możliwością nadpisania kroków.  
- **Visitor** – dodaje operacje do obiektów bez zmiany ich klas.

### 4. Proxy vs Facade  
- **Proxy** – pełni rolę pośrednika, kontrolując dostęp do obiektu (np. lazy loading, zabezpieczenia).  
- **Facade** – zapewnia uproszczony interfejs do złożonego systemu, ukrywając szczegóły implementacji.

### 5. Simple Factory vs Factory Method  
- **Simple Factory** – metoda statyczna zwracająca różne obiekty, brak polimorfizmu.  
- **Factory Method** – wzorzec, w którym podklasy decydują o tworzeniu obiektów (polimorficzne podejście).

### 6. Zastosowanie Flyweight  
Optymalizacja pamięci poprzez współdzielenie obiektów, np.:  
- znaki w edytorach tekstu,  
- kafelki w grach,  
- elementy GUI powtarzane wielokrotnie.

### 7. Zastosowania Chain of Responsibility  
- obsługa zdarzeń w GUI (np. przepływ kliknięcia przez hierarchię),  
- filtry w serwerach HTTP,  
- logowanie na różnych poziomach (DEBUG, INFO, ERROR).

### 8. Implementacje Singletona w Javie  
- Prywatny konstruktor + statyczna instancja,  
- Lazy initialization (opcjonalnie synchronized),  
- Holder idiom (statyczna wewnętrzna klasa),  
- Enum Singleton.

### 9. SOLID  
- **S** – Single Responsibility Principle – jedna klasa = jedna odpowiedzialność.  
- **O** – Open/Closed Principle – otwarte na rozszerzenia, zamknięte na modyfikacje.  
- **L** – Liskov Substitution Principle – podklasa może zastąpić klasę bazową.  
- **I** – Interface Segregation Principle – wiele małych interfejsów zamiast jednego dużego.  
- **D** – Dependency Inversion Principle – zależności od abstrakcji, nie od implementacji.

### 10. Komentarze w czystym kodzie  
Kod powinien być tak czytelny, żeby komentarze były minimalne.  
Komentarze służą wyjaśnianiu *dlaczego*, nigdy *co* robi kod.  
Nie mogą być zastępstwem złych nazw.

### 11. Refactoring  
Proces poprawy jakości kodu bez zmiany jego funkcjonalności.  
Techniki: ekstrakcja metody, zmiana nazwy, przeniesienie klasy, usunięcie duplikacji.

### 12. Programowanie aspektowe (AOP)  
Technika wydzielania przekrojowych problemów (np. logowanie, bezpieczeństwo).  
W Javie: Spring AOP, AspectJ (np. adnotacje @Aspect, @Around).

### 13. Programowanie funkcyjne w Javie  
Styl bazujący na funkcjach jako wartościach: lambdy, Stream API, Optional, funkcje wyższego rzędu.  
Przykład: `list.stream().filter(x -> x > 10).map(x -> x*2).toList()`.

### 14. Zasady czystego kodu  
- Nazwy – mówiące, spójne, bez niezrozumiałych skrótów.  
- Długość funkcji – krótkie, realizujące jedno zadanie.  
- Argumenty funkcji – maksymalnie 3, najlepiej 0-2.

### 15. Abstrakcja w kodzie  
Ułatwia utrzymanie i rozwój (zmiany w implementacji nie wpływają na użytkowników).  
Pozwala na testowanie (mockowanie interfejsów).  
Zmniejsza sprzężenie między modułami.


---

## 14. Bariery w przestrzeni cyfrowej

### 14.1. Projektowanie uniwersalne - idea, przepisy prawne, zasady

**Definicja projektowania uniwersalnego:**
Projektowanie uniwersalne to koncepcja tworzenia produktów, środowisk i usług użytecznych dla wszystkich ludzi w jak największym zakresie, bez potrzeby specjalnej adaptacji czy dostosowań.

**Historia i podstawy prawne:**
- **1997**: Zespół architektów pod kierownictwem Rona Mace'a sformułował 7 zasad projektowania uniwersalnego
- **2006**: Konwencja ONZ o prawach osób niepełnosprawnych
- **2012**: Ratyfikacja Konwencji przez Polskę
- **2019**: Europejski Akt o Dostępności (European Accessibility Act)
- **2025**: Wejście w życie nowych wymagań WCAG 2.2

**Siedem zasad projektowania uniwersalnego:**

**1. Równoważne użytkowanie (Equitable Use):**
- Projekt jest użyteczny dla osób o różnych możliwościach
- **Przykład**: Automatyczne drzwi - wygodne dla wszystkich, niezbędne dla osób na wózkach

**2. Elastyczność użytkowania (Flexibility in Use):**
- Projekt akomoduje szeroki zakres indywidualnych preferencji
- **Przykład**: Schody z pochylnią - wybór sposobu przemieszczania

**3. Proste i intuicyjne użytkowanie (Simple and Intuitive Use):**
- Użytkowanie jest łatwe do zrozumienia niezależnie od doświadczenia
- **Przykład**: Ikony uniwersalne (symbol dostępności, zakaz palenia)

**4. Postrzegalna informacja (Perceptible Information):**
- Informacje są przekazywane skutecznie niezależnie od warunków otoczenia
- **Przykład**: Sygnały dźwiękowe i świetlne w komunikacji publicznej

**5. Tolerancja na błąd (Tolerance for Error):**
- Projekt minimalizuje ryzyko i konsekwencje przypadkowych działań
- **Przykład**: Funkcja "Cofnij" w oprogramowaniu, zabezpieczenia przed przypadkowym usunięciem

**6. Niski wysiłek fizyczny (Low Physical Effort):**
- Projekt może być użyty efektywnie z minimalną ilością wysiłku
- **Przykład**: Klamki dźwigniowe zamiast okrągłych

**7. Rozmiar i przestrzeń (Size and Space):**
- Odpowiednia wielkość i przestrzeń dla dostępu niezależnie od wzrostu czy mobilności
- **Przykład**: Szerokość korytarzy umożliwiająca przejazd wózkiem

### 14.2. Ergonomia interfejsów oprogramowania

**Definicja ergonomii interfejsów:**
Dziedzina zajmująca się projektowaniem interfejsów użytkownika w sposób optymalizujący interakcję między człowiekiem a systemem komputerowym.

**Obszary ergonomii interfejsów:**

**1. Ergonomia poznawcza:**
- Ograniczenia pamięci operacyjnej (7±2 elementy)
- Rozpoznawanie vs przywoływanie
- Mapowanie mentalne
- Przeciążenie informacyjne

**2. Ergonomia fizyczna:**
- Pozycja ciała przy pracy z komputerem
- Zmęczenie oczu (niebieskie światło)
- Syndrom cieśni nadgarstka
- Oświetlenie stanowiska pracy

**3. Ergonomia organizacyjna:**
- Organizacja pracy z systemami informatycznymi
- Przerwy w pracy
- Rotacja zadań
- Szkolenia użytkowników

**Typy interfejsów i ich cechy ergonomiczne:**

**Interfejsy graficzne (GUI):**
```
Zalety:
+ Intuicyjność (metafora pulpitu)
+ Bezpośrednie manipulowanie obiektami
+ Natychmiastowa informacja zwrotna
+ Łatwość nauki

Wady:
- Wymagają sprawności manualnej
- Mogą być przeciążone informacyjnie
- Wymagają dobrej rozdzielczości ekranu
```

**Interfejsy głosowe (VUI):**
```
Zalety:
+ Hands-free operation
+ Naturalna forma komunikacji
+ Dostępność dla osób niewidomych
+ Multitasking

Wady:
- Problemy z rozpoznawaniem mowy
- Brak prywatności
- Trudności w hałaśliwym otoczeniu
- Ograniczenia językowe
```

**Interfejsy dotykowe (TUI):**
```
Zalety:
+ Bezpośrednia interakcja
+ Intuicyjne gesty
+ Mobilność
+ Łatwość nauki

Wady:
- Brak feedback'u haptycznego
- Problemy z precyzją
- Zmęczenie rąk (gorilla arm)
- Niedostępność dla niektórych niepełnosprawności
```

### 14.3. Użyteczność i dostępność interfejsu oprogramowania

**Użyteczność (Usability) według ISO 9241-11:**
Stopień, w jakim produkt może być używany przez określonych użytkowników w celu osiągnięcia określonych celów z efektywnością, skutecznością i satysfakcją w określonym kontekście użycia.

**Komponenty użyteczności (Jakob Nielsen):**
1. **Learnability**: łatwość nauki
2. **Efficiency**: efektywność użytkowania
3. **Memorability**: zapamiętywanie
4. **Errors**: częstość i konsekwencje błędów
5. **Satisfaction**: subiektywna satysfakcja

**Dostępność (Accessibility):**
Właściwość produktu pozwalająca na jego użytkowanie przez osoby o różnych możliwościach, w tym osoby z niepełnosprawnościami.

**Związek między użytecznością a dostępnością:**
```
Dostępność ⊂ Użyteczność

Dostępny produkt: może być użyty przez osoby z niepełnosprawnościami
Użyteczny produkt: jest efektywny, skuteczny i satysfakcjonujący dla wszystkich
```

**Przykłady poprawy dostępności wpływającej na użyteczność:**

**Alt text dla obrazów:**
```html
<!-- Korzyść dla osób niewidomych I wszystkich użytkowników gdy obrazy się nie wczytują -->
<img src="chart.png" alt="Sprzedaż wzrosła o 25% w Q4 2024">
```

**Opisowe linki:**
```html
<!-- Zamiast niejasnego "kliknij tutaj" -->
<a href="raport.pdf">Pobierz roczny raport finansowy (PDF, 2.3 MB)</a>
```

**Struktura nagłówków:**
```html
<!-- Logiczna hierarchia pomagająca wszystkim użytkownikom -->
<h1>Strona główna</h1>
  <h2>Aktualności</h2>
    <h3>Najnowsze wiadomości</h3>
  <h2>O firmie</h2>
    <h3>Historia</h3>
    <h3>Misja</h3>
```

### 14.4. Technologie wspomagające osoby z niepełnosprawnościami

**1. Dla osób niewidomych i słabowidzących:**

**Czytniki ekranu (Screen Readers):**
- **JAWS (Job Access With Speech)**: najczęściej używany na Windows
- **NVDA (NonVisual Desktop Access)**: darmowy dla Windows
- **VoiceOver**: wbudowany w macOS i iOS
- **TalkBack**: Android
- **Orca**: Linux

**Sposób działania czytników:**
```
1. Analiza struktury HTML/DOM
2. Konwersja tekstu na mowę (TTS)
3. Nawigacja przez elementy (nagłówki, linki, formularze)
4. Odczytywanie atrybutów dostępności (aria-labels, alt text)
```

**Klawiatury brajlowskie:**
- Wyświetlacze brajlowskie (20-80 komórek)
- Wprowadzanie tekstu w brajlu
- Nawigacja po stronie

**Lupki ekranowe:**
- ZoomText (Windows)
- Magnifier (Windows, macOS)
- Lupki hardware'owe

**2. Dla osób niesłyszących:**

**Tłumaczenia na język migowy:**
- Wideo z tłumaczem
- Awatary 3D (w rozwoju)

**Napisy i transkrypcje:**
- Closed captions (ukryte napisy)
- Otwarte napisy
- Transkrypcje na żywo

**3. Dla osób z niepełnosprawnościami motorycznymi:**

**Klawiatury alternatywne:**
- Klawiatury jednoręczne
- Klawiatury ekranowe
- Eye tracking keyboards

**Urządzenia wskazujące:**
- Trackballe
- Joysticki
- Head mice (sterowanie ruchem głowy)

**Przełączniki (Switches):**
- Przełączniki pojedyncze/wielokrotne
- Sip-and-puff (dmuchanie/ssanie)
- Przełączniki nożne

**4. Dla osób z niepełnosprawnościami poznawczymi:**

**Wspomaganie czytania:**
- Text-to-speech
- Podświetlanie czytanego tekstu
- Uproszczony język

**Wspomaganie nawigacji:**
- Breadcrumbs (ścieżka nawigacji)
- Site maps
- Przewodniki krok po kroku

### 14.5. WCAG 2.1 - zasady, poziomy, weryfikacja

**Web Content Accessibility Guidelines 2.1:**
Międzynarodowy standard dostępności treści internetowych opracowany przez W3C.

**Cztery główne zasady WCAG (POUR):**

**1. Perceivable (Postrzegalność):**
Informacje i komponenty interfejsu muszą być prezentowane w sposób dostępny dla użytkowników.

**Wytyczne:**
- **1.1 Alternatywy tekstowe**: Zapewnij alternatywy tekstowe dla treści nietekstowych
- **1.2 Media czasowe**: Zapewnij alternatywy dla mediów czasowych
- **1.3 Adaptowalność**: Twórz treść w sposób umożliwiający prezentację w różnych formach
- **1.4 Rozróżnialność**: Ułatw użytkownikom postrzeganie treści

**Przykłady implementacji:**
```html
<!-- 1.1 Alt text -->
<img src="wykres.png" alt="Sprzedaż wzrosła z 100 do 150 jednostek">

<!-- 1.2 Napisy dla wideo -->
<video>
  <track kind="captions" src="napisy.vtt" srclang="pl" label="Polski">
</video>

<!-- 1.3 Semantyczny HTML -->
<main>
  <article>
    <h1>Tytuł artykułu</h1>
    <p>Treść artykułu...</p>
  </article>
</main>

<!-- 1.4 Odpowiedni kontrast (min 4.5:1 dla AA) -->
<style>
  .text {
    color: #000000; /* Czarny tekst */
    background-color: #ffffff; /* Białe tło */
    /* Kontrast: 21:1 - spełnia AAA */
  }
</style>
```

**2. Operable (Funkcjonalność):**
Komponenty interfejsu i nawigacja muszą być funkcjonalne.

**Wytyczne:**
- **2.1 Dostępność z klawiatury**: Całość funkcjonalności dostępna z klawiatury
- **2.2 Wystarczający czas**: Daj użytkownikom wystarczający czas na czytanie
- **2.3 Napady**: Nie projektuj treści mogących powodować napady
- **2.4 Nawigacja**: Pomóż użytkownikom w nawigacji i znajdowaniu treści
- **2.5 Modalności wejściowe**: Ułatw działanie różnymi sposobami

**Przykłady:**
```html
<!-- 2.1 Dostęp z klawiatury -->
<button tabindex="0" onclick="submit()">Wyślij</button>

<!-- 2.2 Timeout z opcją przedłużenia -->
<div role="alert">
  Sesja wygaśnie za 2 minuty. 
  <button onclick="extendSession()">Przedłuż sesję</button>
</div>

<!-- 2.4 Skip links -->
<a href="#main-content" class="skip-link">Przejdź do treści głównej</a>

<!-- 2.4 Nawigacja breadcrumb -->
<nav aria-label="Breadcrumb">
  <ol>
    <li><a href="/">Strona główna</a></li>
    <li><a href="/produkty">Produkty</a></li>
    <li aria-current="page">Laptop XYZ</li>
  </ol>
</nav>
```

**3. Understandable (Zrozumiałość):**
Informacje i obsługa interfejsu muszą być zrozumiałe.

**Wytyczne:**
- **3.1 Czytelność**: Uczyń treść tekstową czytelną i zrozumiałą
- **3.2 Przewidywalność**: Spraw, aby strony działały w przewidywalny sposób
- **3.3 Wspomaganie wprowadzania**: Pomóż użytkownikom unikać błędów

**Przykłady:**
```html
<!-- 3.1 Język dokumentu -->
<html lang="pl">

<!-- 3.1 Trudne słowa z objaśnieniami -->
<p>To jest <dfn>akronim</dfn> - skrót utworzony z pierwszych liter.</p>

<!-- 3.2 Konsystentna nawigacja -->
<nav>
  <ul>
    <li><a href="/">Home</a></li>
    <li><a href="/about">O nas</a></li>
    <li><a href="/contact">Kontakt</a></li>
  </ul>
</nav>

<!-- 3.3 Walidacja formularza -->
<form>
  <label for="email">Email (wymagane):</label>
  <input type="email" id="email" required aria-describedby="email-error">
  <div id="email-error" role="alert">
    Wprowadź poprawny adres email
  </div>
</form>
```

**4. Robust (Solidność):**
Treść musi być solidna - poprawnie interpretowana przez różne technologie.

**Wytyczne:**
- **4.1 Kompatybilność**: Maksymalizuj kompatybilność z technologiami wspomagającymi

**Przykłady:**
```html
<!-- 4.1 Poprawny HTML -->
<!DOCTYPE html>
<html lang="pl">
<head>
  <meta charset="UTF-8">
  <title>Tytuł strony</title>
</head>
<body>
  <!-- Poprawnie zagnieżdżone elementy -->
  <main>
    <h1>Nagłówek</h1>
    <p>Paragraf treści</p>
  </main>
</body>
</html>

<!-- ARIA labels dla screen readerów -->
<button aria-label="Zamknij okno dialogowe" onclick="closeDialog()">
  <span aria-hidden="true">×</span>
</button>

<!-- Status updates -->
<div role="status" aria-live="polite" id="status">
  Formularz został wysłany pomyślnie
</div>
```

**Poziomy zgodności WCAG:**

**Poziom A (minimum):**
- Podstawowe wymagania dostępności
- 25 kryteriów sukcesu
- **Przykład**: Alt text dla obrazów, dostęp z klawiatury

**Poziom AA (standard):**
- Usuwa główne bariery dostępności
- 38 kryteriów (A + 13 dodatkowych)
- **Wymagany prawnie** w UE i Polsce
- **Przykład**: Kontrast 4.5:1, możliwość powiększenia do 200%

**Poziom AAA (zaawansowany):**
- Najwyższy poziom dostępności
- 61 kryteriów (A + AA + 23 dodatkowe)
- Trudny do osiągnięcia dla całych witryn
- **Przykład**: Kontrast 7:1, język migowy dla wszystkich materiałów audio

**Metody weryfikacji WCAG:**

**1. Automatyczne narzędzia:**
- **axe DevTools**: rozszerzenie do przeglądarek
- **WAVE**: Web Accessibility Evaluation Tool
- **Lighthouse**: wbudowane w Chrome DevTools
- **Pa11y**: narzędzie command line

**2. Manualne testowanie:**
- Nawigacja tylko klawiaturą (Tab, Shift+Tab, Enter, spacja)
- Test z wyłączonymi obrazami
- Test z wyłączonym CSS
- Test z wyłączonym JavaScript (graceful degradation)

**3. Testowanie z użytkownikami:**
- Testy z czytnikami ekranu
- Testy z użytkownikami z niepełnosprawnościami
- Sesje z technologiami wspomagającymi

**4. Narzędzia do testowania:**
```bash
# Pa11y - command line accessibility testing
npm install -g pa11y
pa11y https://example.com

# axe-core - programmatyczne testowanie
npm install axe-core
```

```javascript
// Testowanie z axe-core
const axe = require('axe-core');

axe.run(document, {
  tags: ['wcag2a', 'wcag2aa']
}, (err, results) => {
  if (err) throw err;
  console.log(results.violations);
});
```

**Przykład raportu z audytu dostępności:**
```
Strona: https://example.com/login
Poziom docelowy: AA
Data audytu: 2024-01-15

Krytyczne błędy:
1. Brak alt text dla 5 obrazów (WCAG 1.1.1)
2. Niewystarczający kontrast: 3.1:1, wymagane 4.5:1 (WCAG 1.4.3)
3. Brak dostępu z klawiatury do menu (WCAG 2.1.1)

Ostrzeżenia:
1. Brak nagłówków strukturyzujących treść (WCAG 1.3.1)
2. Linki o tej samej nazwie prowadzące do różnych miejsc (WCAG 2.4.4)

Zgodność: 23/38 kryteriów AA (60%)
```

**Korzyści z implementacji WCAG:**
- **Prawne**: zgodność z regulacjami
- **Biznesowe**: większa grupa użytkowników (15-20% populacji)
- **SEO**: lepsza struktura = wyższe pozycje w wyszukiwarkach
- **UX**: lepsze doświadczenie dla wszystkich użytkowników
- **Techniczne**: czystszy, bardziej semantyczny kod
