
---

## 9. Zaawansowane programowanie w Swift

### 9.1. Wzorzec projektowy Model-View-ViewModel (MVVM)

**Definicja MVVM:**
MVVM to wzorzec architektoniczny oddzielający logikę biznesową od interfejsu użytkownika przez wprowadzenie warstwy ViewModel.

**Komponenty MVVM:**

**Model:**
- Dane i logika biznesowa aplikacji
- Struktury danych, klasy encji, API calls
- Niezależny od UI

**View:**
- Interfejs użytkownika (SwiftUI/UIKit)
- Wyświetla dane i reaguje na interakcje użytkownika
- Obserwuje zmiany w ViewModel

**ViewModel:**
- Pośrednik między Model a View
- Zawiera logikę prezentacji
- Przygotowuje dane dla View
- Obsługuje akcje użytkownika

**Przykład implementacji MVVM w Swift:**

```swift
// Model
struct User {
    let id: Int
    let name: String
    let email: String
    let avatarURL: String?
}

// ViewModel
@MainActor
class UserListViewModel: ObservableObject {
    @Published private(set) var users: [User] = []
    @Published private(set) var isLoading = false
    @Published private(set) var errorMessage: String?

    func loadUsers() {
        Task {
            isLoading = true
            errorMessage = nil
            // Symulacja API call
            try await Task.sleep(for: .seconds(2))
            users = [
                User(id: 1, name: "Jan Kowalski", email: "jan@example.com", avatarURL: nil),
                User(id: 2, name: "Anna Nowak", email: "anna@example.com", avatarURL: nil)
            ]
            isLoading = false
        }
    }
}

// SwiftUI View
struct UserListView: View {
    @StateObject private var viewModel = UserListViewModel()

    var body: some View {
        NavigationView {
            Group {
                if viewModel.isLoading {
                    ProgressView("Ładowanie...")
                } else {
                    List(viewModel.users, id: \.id) { user in
                        VStack(alignment: .leading) {
                            Text(user.name).font(.headline)
                            Text(user.email).font(.caption)
                        }
                    }
                }
            }
            .navigationTitle("Użytkownicy")
            .onAppear {
                viewModel.loadUsers()
            }
        }
    }
}
```

**Zalety MVVM:**
- Separacja odpowiedzialności
- Lepszą testowalność
- Wielokrotne użycie ViewModel
- Reaktywność dzięki data binding

### 9.2. Typy generyczne w Swift

**Definicja:**
Generics pozwalają pisać elastyczny kod wielokrotnego użytku, który działa z różnymi typami przy zachowaniu bezpieczeństwa typów.

**Podstawowy przykład:**

```swift
// Bez generics - duplikacja kodu
struct IntStack {
    private var items: [Int] = []

    mutating func push(_ item: Int) {
        items.append(item)
    }

    mutating func pop() -> Int? {
        items.isEmpty ? nil : items.removeLast()
    }
}

// Z generics - kod wielokrotnego użytku
struct Stack<Element> {
    private var items: [Element] = []

    mutating func push(_ item: Element) {
        items.append(item)
    }

    mutating func pop() -> Element? {
        return items.isEmpty ? nil : items.removeLast()
    }

    var isEmpty: Bool {
        return items.isEmpty
    }

    var count: Int {
        return items.count
    }
}

// Użycie
var stringStack = Stack<String>()
stringStack.push("Hello")
stringStack.push("World")
print(stringStack.pop()) // Optional("World")
```

**Generic Functions:**

```swift
// Funkcja generyczna do zamiany wartości
func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
    let temporaryA = a
    a = b
    b = temporaryA
}

// Generic function z constraint
func findIndex<T: Equatable>(of valueToFind: T, in array: [T]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}
```

### 9.3. Zmienne funkcyjne w Swift

**Definicja:**
Funkcje jako obywatele pierwszej kategorii - można je przypisywać do zmiennych, przekazywać jako parametry i zwracać z innych funkcji.

**Podstawowe przykłady:**

```swift
// Przypisanie funkcji do zmiennej
func addTwoNumbers(a: Int, b: Int) -> Int {
    return a + b
}

// Zmienna funkcyjna
var mathFunction: (Int, Int) -> Int = addTwoNumbers
print(mathFunction(2, 3)) // 5

// Funkcje jako parametry
func performOperation(_ a: Int, _ b: Int, operation: (Int, Int) -> Int) -> Int {
    return operation(a, b)
}

let result = performOperation(4, 5, operation: addTwoNumbers)
```

**Closures - domknięcia:**

```swift
let numbers = [1, 2, 3, 4, 5]

// Pełna składnia closure
let doubled = numbers.map({ (number: Int) -> Int in
    return number * 2
})

// Uproszczone składnie
let doubled2 = numbers.map { $0 * 2 }
```

### 9.4. Opcjonalność w Swift

**Optional Types:**

```swift
var optionalString: String? = "Hello"
var optionalNumber: Int? = nil

// Optional Binding
if let actualString = optionalString {
    print("String has value: \(actualString)")
}

// Guard statement
func processString(_ text: String?) {
    guard let text = text else {
        print("No text provided")
        return
    }
    print("Processing: \(text)")
}

// Nil-coalescing operator
let userName: String? = nil
let displayName = userName ?? "Anonymous"
```

**Unwrapping mechanizmy:**
1. **Optional binding** (if let, guard let) - bezpieczne
2. **Forced unwrapping** (!) - niebezpieczne
3. **Nil-coalescing** (??) - bezpieczne z domyślną wartością
4. **Optional chaining** (?.) - bezpieczne dla łańcucha wywołań

---

## 10. Zaawansowane programowanie aplikacji mobilnych na Android

### 10.1. Mechanizm końcowej lambdy (trailing lambda) w Kotlin

**Definicja:**
Jeśli ostatni parametr funkcji jest funkcją, można wywołać ją poza nawiasami funkcji jako trailing lambda.

**Przykłady:**

```kotlin
// Standardowe wywołanie
button.setOnClickListener({ view ->
    println("Button clicked!")
})

// Trailing lambda syntax
button.setOnClickListener { view ->
    println("Button clicked!")
}

// Jetpack Compose przykład
@Composable
fun MyButton() {
    Button(onClick = { println("Clicked") }) {
        Text("Click me")
    }
}
```

**Korzyści trailing lambda:**
- **Czytelność kodu**: bardziej naturalna składnia
- **Mniej nawiasów**: kod wygląda jak wbudowane konstrukcje
- **DSL creation**: umożliwia tworzenie języków domenowych
- **Compose integration**: doskonale współgra z Jetpack Compose

### 10.2. Rozwiązania redukujące kod w Kotlin

**1. Data Classes:**

```kotlin
// Tradycyjna klasa Java (verbose)
class PersonJava {
    private String name;
    private int age;
    // + konstruktor, gettery, settery, equals, hashCode, toString
}

// Data class w Kotlin (concise)
data class Person(var name: String, var age: Int)

// Automatycznie generuje wszystkie potrzebne metody
val person = Person("Jan", 30)
val olderPerson = person.copy(age = 31)
```

**2. Funkcje rozszerzające:**

```kotlin
// Rozszerzenie typu String
fun String.isEmail(): Boolean {
    return this.contains("@") && this.contains(".")
}

// Użycie
val email = "test@example.com"
if (email.isEmail()) {
    println("Valid email")
}
```

**3. Parametry nazwane i domyślne:**

```kotlin
fun createUser(
    name: String,
    email: String,
    age: Int = 18,
    isActive: Boolean = true
) {
    // implementacja
}

// Wywołania z parametrami domyślnymi
createUser("Jan", "jan@example.com")
createUser("Anna", "anna@example.com", age = 25)
```

### 10.3. Funkcje @Composable w Jetpack Compose

**Cechy funkcji @Composable:**
- Mogą wywoływać inne funkcje @Composable
- Są "idempotentne" - to samo wejście = to samo wyjście
- Mogą być wywoływane tylko z innych funkcji @Composable
- Zarządzają stanem automatycznie

**Przykład:**

```kotlin
@Composable
fun UserProfileCard(user: User) {
    Card(
        modifier = Modifier
            .fillMaxWidth()
            .padding(16.dp)
    ) {
        Column(modifier = Modifier.padding(16.dp)) {
            Text(
                text = user.name,
                style = MaterialTheme.typography.headlineSmall
            )
            Text(
                text = user.email,
                style = MaterialTheme.typography.bodyMedium
            )
        }
    }
}
```

### 10.4. Proces rekompozycji

**Jak działa rekompozycja:**

```kotlin
@Composable
fun Counter() {
    // Stan lokalny - zmiana powoduje rekompozycję
    var count by remember { mutableStateOf(0) }

    Column {
        Text("Count: $count") // Rekompozycja gdy count się zmieni

        Button(onClick = { count++ }) {
            Text("Increment")
        }
    }
}
```

**Smart recomposition:**
- Tylko komponenty używające zmienionego stanu są rekompozowane
- Jetpack Compose optymalizuje rekompozycję automatycznie
- Stable types powodują mniej rekompozycji

### 10.5. State Hoisting (wyciąganie stanu)

**Definicja:**
Przenoszenie stanu z komponentu potomnego do rodzicielskiego w celu współdzielenia stanu.

**Przykład:**

```kotlin
// Przed State Hoisting
@Composable
fun CounterWithoutHoisting() {
    var count by remember { mutableStateOf(0) }

    Column {
        Text("Count: $count")
        Button(onClick = { count++ }) {
            Text("Increment")
        }
    }
}

// Po State Hoisting
@Composable
fun CounterWithHoisting(
    count: Int,
    onIncrement: () -> Unit
) {
    Column {
        Text("Count: $count")
        Button(onClick = onIncrement) {
            Text("Increment")
        }
    }
}

@Composable
fun ParentWithSharedState() {
    var count by remember { mutableStateOf(0) }

    Column {
        CounterWithHoisting(
            count = count,
            onIncrement = { count++ }
        )

        // Drugi komponent może też używać tego samego stanu
        CounterWithHoisting(
            count = count,
            onIncrement = { count += 2 }
        )
    }
}
```

**Zalety State Hoisting:**
- Współdzielenie stanu między komponentami
- Jednokierunkowy przepływ danych
- Lepszą testowalność
- Reużywalność komponentów

---

## 11. Zaawansowane programowanie w języku Python

### 11.1. Dekoratory funkcji

**Definicja:**
Dekorator to funkcja która "owija" inną funkcję, rozszerzając jej funkcjonalność bez modyfikacji kodu.

**Podstawowa składnia:**

```python
def moj_dekorator(func):
    def wrapper():
        print("Przed wywołaniem funkcji")
        result = func()
        print("Po wywołaniu funkcji")
        return result
    return wrapper

@moj_dekorator
def powitanie():
    print("Hello World!")

# Użycie
powitanie()
```

**Praktyczne przykłady:**

**Timer decorator:**
```python
import time
from functools import wraps

def measure_time(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        print(f"{func.__name__} wykonana w {end_time - start_time:.4f} sekund")
        return result
    return wrapper

@measure_time
def slow_function():
    time.sleep(1)
    return "Gotowe"
```

**Cache decorator:**
```python
def cache(func):
    cached_results = {}

    @wraps(func)
    def wrapper(*args, **kwargs):
        key = str(args) + str(sorted(kwargs.items()))

        if key in cached_results:
            print(f"Cache hit for {func.__name__}")
            return cached_results[key]

        result = func(*args, **kwargs)
        cached_results[key] = result
        return result

    return wrapper

@cache
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)
```

**Zastosowania dekoratorów:**
- **Logowanie**: zapisywanie informacji o wywołaniach
- **Autoryzacja**: sprawdzanie uprawnień dostępu
- **Cache**: przechowywanie wyników obliczeń
- **Retry**: ponawianie nieudanych operacji
- **Rate limiting**: ograniczanie częstości wywołań

### 11.2. Metaklasy

**Definicja:**
Metaklasa to "klasa klasy" - definiuje jak klasy są tworzone i jakie mają zachowanie.

**Podstawowy przykład:**

```python
class Meta(type):
    def __new__(cls, name, bases, dct):
        print(f"Tworzenie klasy: {name}")
        # Dodanie automatycznego timestampu
        import time
        dct['created_at'] = time.ctime()

        return super().__new__(cls, name, bases, dct)

class MyClass(metaclass=Meta):
    def __init__(self, value):
        self.value = value

# Tworzenie klasy: MyClass
obj = MyClass(42)
print(MyClass.created_at)
```

**Singleton pattern z metaklasą:**

```python
class Singleton(type):
    _instances = {}

    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super().__call__(*args, **kwargs)
        return cls._instances[cls]

class Database(metaclass=Singleton):
    def __init__(self):
        self.connection = "Database connection"

# Test
db1 = Database()
db2 = Database()
print(db1 is db2)  # True - ten sam obiekt
```

**Zastosowania metaklas:**
- **Singleton pattern**: jeden obiekt na klasę
- **Registry pattern**: automatyczna rejestracja klas
- **ORM**: mapowanie obiektowo-relacyjne
- **Validation**: sprawdzanie poprawności definicji klas

### 11.3. Obsługa wyjątków try-except

**Podstawowa składnia:**

```python
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Nie można dzielić przez zero!")
except Exception as e:
    print(f"Wystąpił błąd: {e}")
else:
    print("Operacja zakończona sukcesem")
finally:
    print("Finalizacja operacji")
```

**Niestandardowe wyjątki:**

```python
class CustomError(Exception):
    def __init__(self, message: str, error_code: int = None):
        self.message = message
        self.error_code = error_code
        super().__init__(self.message)

def safe_divide(a: float, b: float):
    try:
        if b == 0:
            raise CustomError("Division by zero", error_code=1001)
        return a / b
    except CustomError as e:
        print(f"Error {e.error_code}: {e.message}")
        return None
```

### 11.4. Klasy abstrakcyjne

**Definicja z ABC:**

```python
from abc import ABC, abstractmethod

class Animal(ABC):
    def __init__(self, name: str):
        self.name = name

    @abstractmethod
    def make_sound(self) -> str:
        pass

    @abstractmethod
    def move(self) -> str:
        pass

    # Konkretna metoda
    def introduce(self) -> str:
        return f"I'm {self.name}"

class Dog(Animal):
    def make_sound(self) -> str:
        return "Woof!"

    def move(self) -> str:
        return "Running on four legs"

# animal = Animal("Generic")  # TypeError - nie można utworzyć instancji
dog = Dog("Buddy")
print(dog.introduce())  # I'm Buddy
print(dog.make_sound()) # Woof!
```

**Template method pattern:**

```python
from abc import ABC, abstractmethod

class DataProcessor(ABC):
    def process_data(self, data):
        validated_data = self.validate_data(data)
        processed_data = self.transform_data(validated_data)
        self.save_data(processed_data)
        return processed_data

    def validate_data(self, data):
        return [item for item in data if item is not None]

    @abstractmethod
    def transform_data(self, data):
        pass

    @abstractmethod
    def save_data(self, data):
        pass

class NumberProcessor(DataProcessor):
    def transform_data(self, data):
        return [int(item) ** 2 for item in data if str(item).isdigit()]

    def save_data(self, data):
        print(f"Saving numbers to file: {data}")
```

---

## 12. Internet Rzeczy

### 12.1. Zagadnienie Internetu Rzeczy

**Definicja IoT:**
Internet Rzeczy (Internet of Things) to ekosystem wzajemnie połączonych urządzeń fizycznych wyposażonych w czujniki, oprogramowanie i łączność sieciową, które mogą zbierać i wymieniać dane.

**Główne cechy IoT:**
- **Łączność**: połączenie z internetem lub siecią lokalną
- **Czujniki i aktuatory**: zbieranie danych i oddziaływanie na środowisko
- **Autonomiczność**: działanie bez bezpośredniej interwencji człowieka
- **Identyfikowalność**: unikalne identyfikatory urządzeń
- **Inteligencja**: podejmowanie decyzji na podstawie danych

**Architektura IoT:**

```
[Warstwa Percepcji]     →    [Warstwa Sieciowa]     →    [Warstwa Aplikacji]
Czujniki, aktuatory          Protokoły, gateway           Analityka, UI
RFID, GPS, kamery           WiFi, Bluetooth, LoRa         Dashboard, alerty
Mikrokontrolery             Cellular, Ethernet            Machine Learning
```

**Przykłady zastosowań:**
- **Smart Home**: inteligentne termostaty, oświetlenie, kamery
- **Smart City**: monitoring parkingów, oświetlenie uliczne, jakość powietrza
- **Przemysł 4.0**: monitoring maszyn, predictive maintenance
- **Rolnictwo**: monitoring gleby, automatyczne nawadnianie
- **Healthcare**: monitoring zdrowia, urządzenia medyczne

### 12.2. Magistrala, interfejs, protokół - różnice

**Magistrala (Bus):**
- **Definicja**: fizyczne medium komunikacyjne łączące wiele urządzeń
- **Charakterystyka**: wspólny kanał transmisji danych
- **Przykłady**: I2C bus, SPI bus, CAN bus, USB
- **Zalety**: prosty routing, niski koszt
- **Wady**: ograniczona prędkość, konflikty dostępu

**Interfejs:**
- **Definicja**: fizyczny lub logiczny punkt połączenia między urządzeniami
- **Charakterystyka**: definiuje sposób wymiany informacji
- **Typy**: fizyczne (GPIO, UART, SPI) i logiczne (API)
- **Przykłady**: RS232, USB, Ethernet, GPIO

**Protokół:**
- **Definicja**: zestaw reguł i procedur komunikacji
- **Charakterystyka**: określa format, kolejność i znaczenie wiadomości
- **Warstwy**: fizyczna, łącza danych, sieciowa, aplikacyjna
- **Przykłady**: HTTP, MQTT, CoAP, TCP/IP

### 12.3. Mikrokontroler vs Mikroprocesor

**Mikroprocesor (MPU):**
- **Definicja**: jednostka centralna (CPU) bez wbudowanych peryferii
- **Architektura**: CPU + cache + interfejsy zewnętrzne
- **Pamięć**: zewnętrzna RAM i ROM
- **Moc**: wysoka (GHz), wysokie zużycie energii
- **Zastosowania**: komputery, serwery, smartfony
- **Przykłady**: Intel x86, ARM Cortex-A

**Mikrokontroler (MCU):**
- **Definicja**: zintegrowany system na chipie (SoC)
- **Architektura**: CPU + RAM + Flash + peryferia na jednym chipie
- **Pamięć**: wbudowana (KB do MB)
- **Moc**: niska (MHz), niskie zużycie energii
- **Zastosowania**: IoT, systemy wbudowane, automotive
- **Przykłady**: Arduino (ATmega), ESP32, STM32

**Porównanie:**

| Aspekt | Mikroprocesor | Mikrokontroler |
|--------|---------------|----------------|
| **Integracja** | CPU + interfejsy | All-in-one SoC |
| **Koszt** | $50-1000+ | $1-50 |
| **Energia** | 5-100W | 0.001-5W |
| **Złożoność** | Wysoka | Niska |
| **Real-time** | Ograniczone | Bardzo dobre |

### 12.4. CISC vs RISC

**CISC (Complex Instruction Set Computer):**
- **Filozofia**: jedna instrukcja = złożona operacja
- **Instrukcje**: duży zestaw (200-400), zmienna długość
- **Cykle**: wiele cykli na instrukcję
- **Zalety**: kompaktowy kod, łatwiejszy kompiler
- **Wady**: skomplikowane dekodowanie, wysokie zużycie energii
- **Przykłady**: Intel x86, x64

**RISC (Reduced Instruction Set Computer):**
- **Filozofia**: proste instrukcje, szybkie wykonanie
- **Instrukcje**: mały zestów (50-100), stała długość
- **Cykle**: mało cykli na instrukcję (1-2)
- **Zalety**: prosty pipeline, niskie zużycie energii
- **Wady**: dłuższy kod, bardziej złożony kompiler
- **Przykłady**: ARM, MIPS, RISC-V

**W kontekście IoT:**
- **RISC (ARM)**: dominuje w urządzeniach IoT ze względu na niskie zużycie energii
- **CISC (x86)**: używany w bramkach przemysłowych i edge serverach

### 12.5. UART vs USART

**UART (Universal Asynchronous Receiver-Transmitter):**
- **Tryb**: tylko asynchroniczny
- **Linie**: RX, TX, GND
- **Synchronizacja**: start/stop bity
- **Prędkość**: standardowe baudy (9600, 115200)

**USART (Universal Synchronous/Asynchronous Receiver-Transmitter):**
- **Tryb**: synchroniczny i asynchroniczny
- **Linie**: RX, TX, CLK (w trybie sync), GND
- **Synchronizacja**: zegar + start/stop bity
- **Prędkość**: wyższa w trybie synchronicznym

**Różnice:**

| Aspekt | UART | USART |
|--------|------|-------|
| **Tryb synchroniczny** | Nie | Tak |
| **Linia zegara** | Nie | Tak |
| **Prędkość** | Ograniczona | Wyższa |
| **Złożoność** | Prosty | Bardziej złożony |

### 12.6. Sensor inteligentny - cechy

**Definicja:**
Sensor inteligentny to urządzenie, które nie tylko zbiera dane, ale również je przetwarza lokalnie, komunikuje się z innymi urządzeniami i może podejmować autonomiczne decyzje.

**Główne cechy:**

**1. Przetwarzanie lokalne:**
- Wbudowany mikroprocesor/mikrokontroler
- Filtrowanie i analiza danych
- Kalibracja automatyczna

**2. Komunikacja sieciowa:**
- Wbudowane interfejsy (WiFi, Bluetooth, LoRa)
- Możliwość wysyłania do chmury
- Komunikacja mesh

**3. Autonomiczność:**
- Podejmowanie akcji bez interwencji
- Automatyczne zarządzanie energią
- Self-configuration

**4. Adaptowalność:**
- Zmiana parametrów działania
- Uczenie się wzorców
- Dostosowanie do warunków

**5. Bezpieczeństwo:**
- Szyfrowanie komunikacji
- Autentykacja urządzenia
- Secure boot

**Przykład implementacji (Arduino/ESP32):**

```cpp
#include <WiFi.h>
#include <DHT.h>
#include <PubSubClient.h>

class SmartTemperatureSensor {
private:
    DHT dht;
    WiFiClient wifiClient;
    PubSubClient mqttClient;

    float lastTemp;
    float tempThreshold;
    bool anomalyDetected;

public:
    SmartTemperatureSensor(int pin, int type) : 
        dht(pin, type), 
        mqttClient(wifiClient) {}

    void init() {
        dht.begin();
        connectToWiFi();
        connectToMQTT();
    }

    void loop() {
        if (millis() % 5000 == 0) {  // Co 5 sekund
            readAndProcessData();
        }
    }

private:
    void readAndProcessData() {
        float temp = dht.readTemperature();

        if (isnan(temp)) {
            Serial.println("Sensor error");
            return;
        }

        // Detekcja anomalii
        if (detectAnomaly(temp)) {
            handleAnomaly(temp);
        }

        // Wysyłanie danych
        sendData(temp);
        lastTemp = temp;
    }

    bool detectAnomaly(float currentTemp) {
        // Nagła zmiana temperatury
        if (abs(currentTemp - lastTemp) > 5.0) {
            return true;
        }

        // Przekroczenie progu
        if (currentTemp > tempThreshold) {
            return true;
        }

        return false;
    }

    void handleAnomaly(float temp) {
        anomalyDetected = true;

        // Natychmiastowe powiadomienie
        String alertMsg = "{"
            "\"sensor_id\": \"TEMP_001\","
            "\"type\": \"ANOMALY\","
            "\"temperature\": " + String(temp) + ","
            "\"threshold\": " + String(tempThreshold) +
        "}";

        mqttClient.publish("sensors/alerts", alertMsg.c_str());
        Serial.println("ANOMALY DETECTED!");
    }

    void sendData(float temp) {
        String dataMsg = "{"
            "\"sensor_id\": \"TEMP_001\","
            "\"temperature\": " + String(temp) + ","
            "\"humidity\": " + String(dht.readHumidity()) + ","
            "\"anomaly\": " + String(anomalyDetected) +
        "}";

        mqttClient.publish("sensors/data", dataMsg.c_str());
        anomalyDetected = false;
    }
};
```

**Cechy tego inteligentnego sensora:**
1. **Lokalne przetwarzanie**: filtrowanie, kalibracja
2. **Detekcja anomalii**: automatyczne wykrywanie nietypowych wartości
3. **Komunikacja sieciowa**: WiFi + MQTT
4. **Autonomiczność**: samodzielne podejmowanie decyzji
5. **Adaptowalność**: możliwość zdalnej konfiguracji

To różni sensor inteligentny od tradycyjnego, który tylko mierzy i przekazuje surowe dane.
