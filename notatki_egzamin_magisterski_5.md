
---

## Zaawansowane programowanie w Swift (10 pytań)

### 1. Omów wzorzec projektowy Model-View-ViewModel (MVVM)

**KRÓTKA ODPOWIEDŹ:**
MVVM to wzorzec architektoniczny, który dzieli aplikację na 3 warstwy: Model (dane i logika biznesowa), View (interfejs użytkownika) i ViewModel (pośrednik zarządzający stanem i logiką prezentacji). View obserwuje zmiany w ViewModel przez data binding, co zapewnia automatyczne aktualizacje UI.

**Szczegóły i kod:**
```swift
// Model - dane i logika biznesowa
struct User {
    let id: Int
    let name: String
    let email: String
}

// ViewModel - zarządzanie stanem i logika prezentacji
@MainActor
class UserListViewModel: ObservableObject {
    @Published private(set) var users: [User] = []
    @Published private(set) var isLoading = false

    func loadUsers() {
        isLoading = true
        // Logika ładowania danych
        isLoading = false
    }
}

// View - interfejs użytkownika obserwujący ViewModel
struct UserListView: View {
    @StateObject private var viewModel = UserListViewModel()

    var body: some View {
        List(viewModel.users, id: \.id) { user in
            Text(user.name)
        }
        .onAppear { viewModel.loadUsers() }
    }
}
```

### 2. Omów typy generyczne w Swift na wybranym przykładzie

**KRÓTKA ODPOWIEDŹ:**
Generics w Swift pozwalają napisać kod, który działa z różnymi typami danych przy zachowaniu bezpieczeństwa typów. Używamy placeholder'ów typu (np. `<T>`) które są zastępowane konkretnym typem podczas użycia. Umożliwia to tworzenie reużywalnych, type-safe komponentów.

**Przykład - Generic Stack:**
```swift
struct Stack<Element> {
    private var items: [Element] = []

    mutating func push(_ item: Element) {
        items.append(item)
    }

    mutating func pop() -> Element? {
        return items.popLast()
    }

    var isEmpty: Bool {
        return items.isEmpty
    }
}

// Użycie z różnymi typami
var stringStack = Stack<String>()
stringStack.push("Hello")

var intStack = Stack<Int>()
intStack.push(42)
```

### 3. Omów zmienne funkcyjne w Swift. Podaj przykład

**KRÓTKA ODPOWIEDŹ:**
W Swift funkcje są "first-class citizens" - można je przypisywać do zmiennych, przekazywać jako parametry i zwracać z innych funkcji. Umożliwia to functional programming i tworzenie elastycznych API. Zmienne funkcyjne mogą przechowywać zarówno nazwane funkcje jak i closures (lambda expressions).

**Przykłady:**
```swift
// Przypisanie funkcji do zmiennej
func addNumbers(_ a: Int, _ b: Int) -> Int {
    return a + b
}

var mathOperation: (Int, Int) -> Int = addNumbers
print(mathOperation(5, 3)) // 8

// Closure jako zmienna funkcyjna  
let multiply: (Int, Int) -> Int = { $0 * $1 }
mathOperation = multiply
print(mathOperation(5, 3)) // 15

// Funkcje jako parametry
func performOperation(_ a: Int, _ b: Int, operation: (Int, Int) -> Int) -> Int {
    return operation(a, b)
}

let result = performOperation(10, 5, operation: addNumbers)
```

### 4. Typ wyliczeniowy w Swift. Omów na wybranym przykładzie

**KRÓTKA ODPOWIEDŹ:**
Enum w Swift to typ, który definiuje grupę powiązanych wartości w type-safe sposób. Swift enums są bardzo potężne - mogą mieć associated values (dane przypisane do konkretnego case), raw values, metody i computed properties. Są często używane do modelowania stanu aplikacji i wariantów danych.

**Przykład z Associated Values:**
```swift
enum NetworkResponse {
    case success(Data)
    case failure(Error)
    case loading
}

enum TrafficLight {
    case red, yellow, green

    var duration: TimeInterval {
        switch self {
        case .red: return 30.0
        case .yellow: return 3.0
        case .green: return 25.0
        }
    }

    func next() -> TrafficLight {
        switch self {
        case .red: return .green
        case .yellow: return .red
        case .green: return .yellow
        }
    }
}

// Użycie
var light = TrafficLight.red
print(light.duration) // 30.0
light = light.next() // green
```

### 5. Rola i składnia protokołów w Swift

**KRÓTKA ODPOWIEDŹ:**
Protokoły w Swift definiują "kontrakt" - zestaw metod, właściwości i innych wymagań, które muszą zostać zaimplementowane przez adopting type (klasa, struktura, enum). Umożliwiają composition over inheritance, multiple protocol conformance i tworzenie abstrakcyjnych interfejsów. Protocol extensions pozwalają dodawać default implementations.

**Przykład:**
```swift
// Definicja protokołu
protocol Drawable {
    var area: Double { get }
    func draw()
    func description() -> String
}

// Protocol extension z default implementation
extension Drawable {
    func description() -> String {
        return "Figura o polu \(area)"
    }
}

// Implementacja w struct
struct Circle: Drawable {
    let radius: Double

    var area: Double {
        return Double.pi * radius * radius
    }

    func draw() {
        print("Rysowanie koła o promieniu \(radius)")
    }
}

// Protocol inheritance
protocol Named {
    var name: String { get }
}

protocol Person: Named {
    var age: Int { get }
    func introduce() -> String
}
```

### 6. Wyjaśnij pojęcie opcjonalności w Swift i mechanizmy unwrapping

**KRÓTKA ODPOWIEDŹ:**
Optionals w Swift reprezentują wartość, która może istnieć (ma wartość) lub nie istnieć (nil). Zapobiegają null pointer exceptions znanych z innych języków. Unwrapping to proces bezpiecznego wyciągnięcia wartości z optional. Swift oferuje kilka mechanizmów: optional binding (if let, guard let), nil-coalescing operator (??), optional chaining (?.) i forced unwrapping (!).

**Mechanizmy unwrapping:**
```swift
var optionalString: String? = "Hello"

// 1. Optional binding - bezpieczne
if let actualString = optionalString {
    print("String: \(actualString)")
}

// 2. Guard let - early exit
func processString(_ text: String?) {
    guard let text = text else {
        print("No text provided")
        return
    }
    print("Processing: \(text)")
}

// 3. Nil-coalescing operator - default value
let displayText = optionalString ?? "Default"

// 4. Optional chaining - safe navigation
let count = optionalString?.count

// 5. Forced unwrapping - niebezpieczne!
// let forced = optionalString! // Crash jeśli nil
```

### 7. Omów znaczenie rozszerzeń (extensions) w Swift

**KRÓTKA ODPOWIEDŹ:**
Extensions pozwalają dodawać nową funkcjonalność do istniejących typów (klasy, struktury, enums, protokoły) bez modyfikacji oryginalnego kodu. Można dodawać computed properties, metody, initializers, subscripts i conformance do protokołów. Bardzo przydatne do organizacji kodu i rozszerzania built-in types.

**Przykłady:**
```swift
// Extension dla typu wbudowanego
extension Int {
    var isEven: Bool {
        return self % 2 == 0
    }

    func squared() -> Int {
        return self * self
    }
}

// Użycie
let number = 4
print(number.isEven) // true
print(number.squared()) // 16

// Extension z protocol conformance
extension Int: CustomStringConvertible {
    public var description: String {
        return "Number: \(self)"
    }
}

// Extension z conditional conformance
extension Array where Element: Equatable {
    func removeDuplicates() -> [Element] {
        var result: [Element] = []
        for element in self {
            if !result.contains(element) {
                result.append(element)
            }
        }
        return result
    }
}
```

### 8. Wyjaśnij property observers w Swift

**KRÓTKA ODPOWIEDŹ:**
Property observers to mechanizm reagowania na zmiany wartości właściwości. `willSet` jest wywoływany przed ustawieniem nowej wartości (dostęp do `newValue`), a `didSet` po ustawieniu (dostęp do `oldValue`). Używane do walidacji danych, synchronizacji UI, logowania zmian i side effects.

**Przykład:**
```swift
class StepCounter {
    var totalSteps: Int = 0 {
        willSet {
            print("Ustawianie totalSteps na \(newValue)")
        }
        didSet {
            print("totalSteps zmieniono z \(oldValue) na \(totalSteps)")
            if totalSteps > oldValue {
                print("Dodano \(totalSteps - oldValue) kroków")
            }
            updateHealthKit()
        }
    }

    private func updateHealthKit() {
        // Synchronizacja z HealthKit
    }
}

// Przykład z walidacją
class User {
    var email: String = "" {
        didSet {
            if !isValidEmail(email) {
                email = oldValue // Przywrócenie poprzedniej wartości
                print("Niepoprawny email")
            }
        }
    }

    private func isValidEmail(_ email: String) -> Bool {
        return email.contains("@")
    }
}
```

### 9. Różnica między @StateObject a @ObservedObject

**KRÓTKA ODPOWIEDŹ:**
@StateObject tworzy i jest właścicielem ObservableObject - obiekt przetrwa rekomposycje view. @ObservedObject tylko obserwuje już istniejący obiekt utworzony gdzie indziej. StateObject używamy gdy view tworzy ViewModel, ObservedObject gdy otrzymujemy go z parent view lub dependency injection.

**Porównanie:**
```swift
// @StateObject - właściciel obiektu
struct ContentView: View {
    @StateObject private var viewModel = CounterViewModel() // Tworzy i posiada

    var body: some View {
        VStack {
            Text("Count: \(viewModel.count)")
            ChildView(viewModel: viewModel) // Przekazuje do dziecka
        }
    }
}

// @ObservedObject - obserwator obiektu
struct ChildView: View {
    @ObservedObject var viewModel: CounterViewModel // Otrzymuje z parent

    var body: some View {
        Button("Increment") {
            viewModel.increment()
        }
    }
}

// Złe użycie - NIE ROBIĆ!
struct BadExample: View {
    @ObservedObject private var viewModel = CounterViewModel() // ZŁE!
    // Obiekt może być odtwarzany przy każdej rekomposycji
}
```

### 10. Różnice między strukturami a klasami w Swift

**KRÓTKA ODPOWIEDŹ:**
Główna różnica: struktury to value types (kopiowane przy przypisaniu), klasy to reference types (współdzielona instancja). Struktury nie obsługują inheritance, są szybsze, automatycznie thread-safe. Klasy obsługują inheritance, mają deinitializers, można sprawdzać identity (===). Struktury preferowane dla prostych danych, klasy dla złożonych obiektów wymagających inheritance.

**Porównanie:**
```swift
// STRUCT - value type
struct Point {
    var x: Int
    var y: Int
}

var point1 = Point(x: 10, y: 20)
var point2 = point1 // KOPIA!
point2.x = 30
print(point1.x) // 10 (nie zmienione)
print(point2.x) // 30 (zmienione)

// CLASS - reference type
class Person {
    var name: String
    init(name: String) { self.name = name }
}

let person1 = Person(name: "Jan")
let person2 = person1 // TA SAMA INSTANCJA!
person2.name = "Anna"
print(person1.name) // "Anna" (zmienione!)
print(person2.name) // "Anna" (zmienione!)

// Identity vs equality
print(person1 === person2) // true - ta sama instancja
```

---

## Zaawansowane programowanie na Android (15 pytań)

### 1. Mechanizm końcowej lambdy (trailing lambda) w Kotlin

**KRÓTKA ODPOWIEDŹ:**
Trailing lambda to składnia Kotlin pozwalająca wyciągnąć ostatni parametr typu lambda poza nawiasy funkcji. Jeśli lambda jest jedynym parametrem, nawiasy można całkowicie pominąć. Czyni kod bardziej czytelnym i podobnym do DSL, szczególnie w Jetpack Compose.

**Przykłady:**
```kotlin
// Standardowa składnia
button.setOnClickListener({ view -> println("Clicked") })

// Trailing lambda
button.setOnClickListener { view -> println("Clicked") }

// W Jetpack Compose
Button(
    onClick = { println("Clicked") }
) { // trailing lambda dla content
    Text("Click Me")
}

LazyColumn {  // trailing lambda dla content
    items(10) { index ->
        Text("Item $index")
    }
}
```

### 2. Rozwiązania redukujące kod w Kotlin

**KRÓTKA ODPOWIEDŹ:**
Kotlin oferuje kilka mechanizmów redukujących boilerplate: data classes (automatyczne equals/hashCode/toString), extension functions (dodawanie metod do istniejących typów), named parameters z default values (eliminuje builder pattern), destructuring declarations i operator overloading.

**Przykłady:**
```kotlin
// Data class vs tradycyjna klasa
data class Person(val name: String, val age: Int) // Automatycznie generuje wszystkie metody

// Extension functions
fun String.isEmail() = contains("@") && contains(".")
fun List<T>.second() = if (size >= 2) this[1] else null

// Named parameters z defaults
fun createUser(
    name: String,
    age: Int = 18,
    isActive: Boolean = true
) {}

createUser("Jan", age = 25) // named parameters

// Destructuring
val (name, age) = person
for ((key, value) in map) { }
```

### 3. Funkcje @Composable w Jetpack Compose

**KRÓTKA ODPOWIEDŹ:**
@Composable to adnotacja oznaczająca funkcje opisujące UI w deklaratywny sposób. Takie funkcje mogą być wywoływane tylko z innych @Composable, są idempotentne (te same parametry = ten sam wynik), mogą być wywoływane wielokrotnie i w różnej kolejności. Zarządzają stanem przez remember() i reagują na zmiany przez rekomposycję.

**Charakterystyki:**
```kotlin
@Composable
fun WelcomeScreen(userName: String) {
    // Może wywoływać inne @Composable
    Column {
        Text("Witaj, $userName!") // @Composable
        Button(onClick = { }) {   // @Composable
            Text("Rozpocznij")    // @Composable
        }
    }
}

// Stan lokalny w @Composable
@Composable
fun Counter() {
    var count by remember { mutableStateOf(0) }

    Button(onClick = { count++ }) {
        Text("Count: $count")
    }
}
```

### 4. Właściwości procesu rekompozycji

**KRÓTKA ODPOWIEDŹ:**
Rekompozycja to automatyczne ponowne wywołanie @Composable funkcji gdy zmieni się stan, od którego zależą. Jest "smart" - rekompozuje tylko części, które faktycznie zależą od zmienionego stanu. Może wystąpić wielokrotnie, w różnej kolejności, a nawet równolegle. Optymalizowana przez skipping (pomijanie) niezmiennych części.

**Przykład smart recomposition:**
```kotlin
@Composable
fun SmartRecomposition() {
    var count1 by remember { mutableStateOf(0) }
    var count2 by remember { mutableStateOf(0) }

    Column {
        Text("Count1: $count1") // Rekompozycja tylko przy zmianie count1
        Text("Count2: $count2") // Rekompozycja tylko przy zmianie count2
        Text("Static text")      // Nigdy nie rekompozowane

        Button(onClick = { count1++ }) { Text("Increment 1") }
        Button(onClick = { count2++ }) { Text("Increment 2") }
    }
}
```

### 5. Cykl życia elementów UI w kompozycji

**KRÓTKA ODPOWIEDŹ:**
Elementy UI w Compose mają 3 fazy: Enter Composition (pojawienie się w UI tree), Recomposition (aktualizacje przy zmianie stanu) i Leave Composition (usunięcie z UI tree). Cykl życia kontrolowany przez: conditional rendering, key changes, navigation, LazyList viewport. LaunchedEffect dla enter, DisposableEffect dla cleanup przy leave.

**Przykład lifecycle:**
```kotlin
@Composable
fun ComponentWithLifecycle() {
    // ENTER COMPOSITION
    LaunchedEffect(Unit) {
        println("Component entered composition")
        // Setup, start coroutines, subscribe to data
    }

    // RECOMPOSITION - każda pomyślna rekompozycja
    SideEffect {
        println("Component recomposed")
    }

    // LEAVE COMPOSITION
    DisposableEffect(Unit) {
        val resource = setupResource()
        onDispose {
            println("Component leaving composition")
            resource.cleanup()
        }
    }

    Text("Hello World")
}
```

### 6. Mechanizm wyciągania stanu (state hoisting)

**KRÓTKA ODPOWIEDŹ:**
State hoisting to przeniesienie stanu z komponentu potomnego do wspólnego przodka, aby umożliwić współdzielenie stanu między komponentami. Komponent przekazuje stan w dół jako parametry, a wydarzenia w górę jako callback functions. Zapewnia single source of truth i unidirectional data flow.

**Przykład:**
```kotlin
// PRZED hoisting - stateful component
@Composable
fun StatefulCounter() {
    var count by remember { mutableStateOf(0) }

    Button(onClick = { count++ }) {
        Text("Count: $count")
    }
}

// PO hoisting - stateless component  
@Composable
fun StatelessCounter(
    count: Int,
    onIncrement: () -> Unit
) {
    Button(onClick = onIncrement) {
        Text("Count: $count")
    }
}

// Parent z hoisted state
@Composable
fun ParentWithSharedState() {
    var count by remember { mutableStateOf(0) }

    Column {
        StatelessCounter(count, { count++ })
        StatelessCounter(count, { count += 2 })
        Text("Total clicks: $count")
    }
}
```

### 7. Stan w remember(), klasach i ViewModel

**KRÓTKA ODPOWIEDŹ:**
remember() przechowuje stan lokalny - przetrwa rekomposycję ale nie configuration changes. Klasy z remember() pozwalają na bardziej złożony stan ale nadal lokalne. ViewModel przetrwa configuration changes i navigation, najlepsze dla business logic i shared state. Wybór zależy od scope i lifecycle wymagań stanu.

**Porównanie:**
```kotlin
// remember() - stan lokalny
@Composable
fun RememberState() {
    var count by remember { mutableStateOf(0) }
    // Przetrwa rekomposycję, NIE przetrwa rotation
}

// Klasa z remember()
class CounterState {
    var count by mutableStateOf(0)
    fun increment() { count++ }
}

@Composable  
fun ClassState() {
    val state = remember { CounterState() }
    // Bardziej strukturalny, ale nadal lokalny
}

// ViewModel
class CounterViewModel : ViewModel() {
    var count by mutableStateOf(0)
        private set
    fun increment() { count++ }
}

@Composable
fun ViewModelState(viewModel: CounterViewModel = viewModel()) {
    // Przetrwa configuration changes i navigation
}
```

### 8. Podstawowe komponenty Material Design

**KRÓTKA ODPOWIEDŹ:**
Material Design w Compose oferuje komponenty zgodne z Material Design guidelines: Button (contained/outlined/text), TextField (filled/outlined), Card (elevated surfaces), FAB (floating action button), TopAppBar, BottomNavigation. Każdy komponent ma predefined styles, colors i behavior zgodne ze specyfikacją Material Design.

**Przykłady komponentów:**
```kotlin
@Composable
fun MaterialComponents() {
    var text by remember { mutableStateOf("") }

    Column {
        // Buttons
        Button(onClick = { }) { Text("Contained") }
        OutlinedButton(onClick = { }) { Text("Outlined") }
        TextButton(onClick = { }) { Text("Text") }

        // TextFields
        TextField(
            value = text,
            onValueChange = { text = it },
            label = { Text("Label") }
        )

        OutlinedTextField(
            value = text,
            onValueChange = { text = it },
            label = { Text("Outlined") }
        )

        // Card
        Card(
            elevation = CardDefaults.cardElevation(6.dp)
        ) {
            Text("Card content", modifier = Modifier.padding(16.dp))
        }

        // FAB
        FloatingActionButton(onClick = { }) {
            Icon(Icons.Default.Add, contentDescription = "Add")
        }
    }
}
```

### 9. Podstawowe zasady Jetpack Compose

**KRÓTKA ODPOWIEDŹ:**
Compose opiera się na dwóch kluczowych zasadach: Single Source of Truth (jeden punkt przechowywania każdego stanu) i Unidirectional Data Flow (dane płyną w dół, eventy w górę). Stan pochodzi z jednego miejsca, przekazywany jako parametry do komponentów, które reagują na zmiany przez callback functions. Zapewnia przewidywalność i łatwość debugowania.

**Implementacja zasad:**
```kotlin
// Single Source of Truth
@Composable
fun TodoApp() {
    var todos by remember { mutableStateOf(listOf<Todo>()) }

    // todos jest jedynym źródłem prawdy
    Column {
        TodoList(
            todos = todos,                    // Data flows DOWN
            onTodoToggle = { id ->           // Events flow UP
                todos = todos.map { 
                    if (it.id == id) it.copy(completed = !it.completed)
                    else it 
                }
            }
        )

        AddTodoButton(
            onAddTodo = { newTodo ->         // Events flow UP
                todos = todos + newTodo
            }
        )
    }
}

@Composable
fun TodoList(
    todos: List<Todo>,                       // Data parameter
    onTodoToggle: (String) -> Unit          // Event callback
) {
    LazyColumn {
        items(todos) { todo ->
            TodoItem(
                todo = todo,                 // Data flows DOWN
                onToggle = { onTodoToggle(todo.id) } // Events flow UP
            )
        }
    }
}
```

### 10. Architektura współczesnej aplikacji Android

**KRÓTKA ODPOWIEDŹ:**
Zalecana architektura składa się z 3 warstw: UI Layer (Activities, Fragments, Composables + ViewModels), Domain Layer (Use Cases, business logic) i Data Layer (Repositories, data sources). Każda warstwa ma jasno określone odpowiedzialności. Dane płyną z Data przez Domain do UI, a user actions w przeciwną stronę.

**Architektura:**
```
┌─────────────────────────────────────┐
│           UI Layer                  │ ← Composables, Activities, ViewModels
├─────────────────────────────────────┤
│         Domain Layer                │ ← Use Cases, Business Logic  
├─────────────────────────────────────┤
│          Data Layer                 │ ← Repositories, API, Database
└─────────────────────────────────────┘
```

**Implementacja:**
```kotlin
// Data Layer
class UserRepository @Inject constructor(
    private val apiService: UserApiService,
    private val userDao: UserDao
) {
    suspend fun getUsers(): Flow<List<User>> = flow {
        emit(userDao.getAllUsers())
        val remoteUsers = apiService.getUsers()
        userDao.insertUsers(remoteUsers)
        emit(remoteUsers)
    }
}

// Domain Layer  
class GetUsersUseCase @Inject constructor(
    private val repository: UserRepository
) {
    suspend operator fun invoke(): Flow<List<User>> = repository.getUsers()
}

// UI Layer
@HiltViewModel
class UserViewModel @Inject constructor(
    private val getUsersUseCase: GetUsersUseCase
) : ViewModel() {

    private val _uiState = MutableStateFlow<UiState>(UiState.Loading)
    val uiState: StateFlow<UiState> = _uiState.asStateFlow()

    fun loadUsers() {
        viewModelScope.launch {
            getUsersUseCase().collect { users ->
                _uiState.value = UiState.Success(users)
            }
        }
    }
}
```

### 11-15. Pozostałe pytania Android

**11. Najlepsze praktyki:** SOLID principles, Dependency Injection (Hilt), Testing (Unit/Integration/UI), Offline-first approach, Reactive programming (Flow), Performance optimization.

**12. Właściciel stanu logiki biznesowej:** ViewModel - przetrwa configuration changes, zarządza business logic, używa StateFlow/LiveData, komunikuje się z Domain/Data layers.

**13. Właściciel stanu UI:** Compose state (remember) - lokalne stany UI jak expanded/selected/loading, krótki lifecycle związany z komponentem.

**14. Warstwa danych:** Repository pattern, data sources (Remote/Local), caching strategy, offline-first, Room database, Retrofit API calls.

**15. Warstwa domeny:** Use Cases enkapsulujące business logic, platform-independent, pure Kotlin bez Android dependencies, single responsibility per use case.

---

## Zaawansowane programowanie w Python (10 pytań)

### 1. Dekoratory funkcji

**KRÓTKA ODPOWIEDŹ:**
Dekoratory to funkcje, które modyfikują lub rozszerzają zachowanie innych funkcji bez zmiany ich kodu. Używają składni @decorator_name przed definicją funkcji. Pozwalają na separation of concerns - można dodać funkcjonalności jak logowanie, timing, cache, authorization bez modyfikacji oryginalnej funkcji.

**Przykłady:**
```python
def timer_decorator(func):
    import time
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"{func.__name__} wykonała się w {end - start:.4f}s")
        return result
    return wrapper

@timer_decorator
def slow_function():
    import time
    time.sleep(1)
    return "Done"
```

### 2. Metaklasy

**KRÓTKA ODPOWIEDŹ:**
Metaklasy to "klasy klas" - definiują jak klasy są tworzone i jakie mają zachowanie. Metaclass kontroluje proces tworzenia klasy poprzez __new__ i __init__. Używane do implementacji wzorców jak Singleton, automatycznej rejestracji klas, walidacji definicji klas czy ORM frameworks.

**Przykład Singleton:**
```python
class SingletonMeta(type):
    _instances = {}

    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super().__call__(*args, **kwargs)
        return cls._instances[cls]

class Database(metaclass=SingletonMeta):
    def __init__(self):
        self.connected = False
```

### 3. Mechanizm obsługi wyjątków try-except

**KRÓTKA ODPOWIEDŹ:**
Try-except to mechanizm obsługi błędów w Pythonie. Try zawiera kod, który może wywołać wyjątek, except łapie określone typy wyjątków, else wykonuje się gdy nie było wyjątku, finally zawsze się wykonuje (cleanup). Pozwala na graceful error handling zamiast crash aplikacji.

**Składnia:**
```python
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Nie można dzielić przez zero")
except ValueError as e:
    print(f"Błąd wartości: {e}")
else:
    print("Kod wykonany bez błędów")
finally:
    print("Zawsze wykonywane")
```

### 4. Klasy abstrakcyjne

**KRÓTKA ODPOWIEDŹ:**
Klasy abstrakcyjne to klasy, których nie można bezpośrednio instancjować. Zawierają metody abstrakcyjne (bez implementacji) które muszą być zaimplementowane w podklasach. Używane do definiowania interfejsów i wymuszania określonego API w hierarchii klas. W Pythonie realizowane przez moduł abc (Abstract Base Classes).

**Przykład:**
```python
from abc import ABC, abstractmethod

class Animal(ABC):
    @abstractmethod
    def make_sound(self):
        pass

    def sleep(self):  # Metoda konkretna
        print("Zzz...")

class Dog(Animal):
    def make_sound(self):
        return "Woof!"
```

### 5. Metody specjalne (__magic methods__)

**KRÓTKA ODPOWIEDŹ:**
Metody specjalne (magic/dunder methods) to metody o nazwach rozpoczynających i kończących się podwójnym podkreśleniem. Definiują zachowanie obiektów z wbudowanymi operatorami Pythona (jak +, ==, len(), str()). Umożliwiają tworzenie klas, które zachowują się jak built-in types.

**Przykład klasy Vector:**
```python
class Vector:
    def __init__(self, x, y):
        self.x, self.y = x, y

    def __str__(self):           # str(obj)
        return f"Vector({self.x}, {self.y})"

    def __add__(self, other):    # obj1 + obj2  
        return Vector(self.x + other.x, self.y + other.y)

    def __len__(self):           # len(obj)
        return int((self.x**2 + self.y**2)**0.5)
```

### 6. Funkcje lambda

**KRÓTKA ODPOWIEDŹ:**
Lambda to anonimowe funkcje jednolinijkowe w Pythonie. Składnia: lambda argumenty: wyrażenie. Używane głównie z funkcjami wyższego rzędu jak map(), filter(), sort(). Są ograniczone do jednego wyrażenia, nie mogą zawierać statements jak print czy assignment.

**Przykłady:**
```python
# Podstawowe lambda
square = lambda x: x ** 2
add = lambda x, y: x + y

# Z funkcjami built-in
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x**2, numbers))
evens = list(filter(lambda x: x % 2 == 0, numbers))

# W sortowaniu
students = [{'name': 'Jan', 'grade': 85}, {'name': 'Anna', 'grade': 92}]
sorted_by_grade = sorted(students, key=lambda s: s['grade'])
```

### 7. Funkcja mapująca (map)

**KRÓTKA ODPOWIEDŹ:**
Map() aplikuje podaną funkcję do każdego elementu iterable i zwraca iterator z wynikami. Składnia: map(function, iterable). Jest lazy (oceniane na żądanie), więc trzeba przekonwertować do listy aby zobaczyć wyniki. Alternatywa to list comprehension, która często jest bardziej czytelna.

**Przykłady:**
```python
# Podstawowe użycie
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x**2, numbers))  # [1, 4, 9, 16, 25]

# Z funkcją
def celsius_to_fahrenheit(c):
    return (c * 9/5) + 32

temps_c = [0, 20, 30]
temps_f = list(map(celsius_to_fahrenheit, temps_c))  # [32.0, 68.0, 86.0]

# Z wieloma iterables
result = list(map(lambda x, y: x + y, [1, 2, 3], [4, 5, 6]))  # [5, 7, 9]
```

### 8. *args i **kwargs

**KRÓTKA ODPOWIEDŹ:**
*args zbiera dodatkowe argumenty pozycyjne do tuple, **kwargs zbiera argumenty nazwane do dictionary. Pozwalają na tworzenie funkcji o zmiennej liczbie argumentów. *args musi być przed **kwargs. Używane do tworzenia elastycznych API i przekazywania argumentów między funkcjami.

**Przykład:**
```python
def flexible_function(*args, **kwargs):
    print("Pozycyjne:", args)      # tuple
    print("Nazwane:", kwargs)      # dict

flexible_function(1, 2, 3, name="Jan", age=30)
# Pozycyjne: (1, 2, 3)
# Nazwane: {'name': 'Jan', 'age': 30}

# Rozpakowywanie
def greet(name, age, city):
    print(f"Cześć {name}, {age} lat, {city}")

person_tuple = ("Anna", 25, "Kraków")
person_dict = {"name": "Piotr", "age": 30, "city": "Gdańsk"}

greet(*person_tuple)    # Rozpakowanie tuple
greet(**person_dict)    # Rozpakowanie dict
```

### 9. List comprehension

**KRÓTKA ODPOWIEDŹ:**
List comprehension to zwięzły sposób tworzenia list w Pythonie. Składnia: [wyrażenie for element in iterable if warunek]. Jest szybsze i bardziej czytelne niż tradycyjne pętle for. Można tworzyć również dict comprehension i set comprehension. Alternatywa dla map() i filter().

**Przykłady:**
```python
# Podstawowe użycie
squares = [x**2 for x in range(10)]  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# Z warunkiem
even_squares = [x**2 for x in range(10) if x % 2 == 0]  # [0, 4, 16, 36, 64]

# Zagnieżdżone
matrix = [[i*j for j in range(3)] for i in range(3)]

# Flatten
nested = [[1, 2], [3, 4], [5, 6]]
flattened = [item for sublist in nested for item in sublist]  # [1, 2, 3, 4, 5, 6]

# Dict i set comprehension
squares_dict = {x: x**2 for x in range(5)}  # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
unique_lengths = {len(word) for word in ["hello", "world"]}  # {5}
```

### 10. Metoda __init__

**KRÓTKA ODPOWIEDŹ:**
__init__ to konstruktor klasy w Pythonie, automatycznie wywoływany przy tworzeniu nowej instancji. Pierwszy parametr to zawsze self (referencja do tworzonego obiektu). Służy do inicjalizacji atrybutów instancji, walidacji parametrów i wykonania setup operations. W inheritance można wywołać super().__init__() aby uruchomić konstruktor klasy nadrzędnej.

**Przykład:**
```python
class Person:
    def __init__(self, name, age, email=None):
        # Inicjalizacja atrybutów
        self.name = name
        self.age = age
        self.email = email

        # Walidacja
        if age < 0:
            raise ValueError("Wiek nie może być ujemny")

        print(f"Utworzono osobę: {name}")

# Inheritance
class Employee(Person):
    def __init__(self, name, age, employee_id, salary):
        super().__init__(name, age)  # Wywołanie konstruktora parent
        self.employee_id = employee_id
        self.salary = salary

person = Person("Jan", 30)
employee = Employee("Anna", 25, "EMP001", 5000)
```

---

## Internet Rzeczy (10 pytań)

### 1. Zagadnienie Internetu Rzeczy

**KRÓTKA ODPOWIEDŹ:**
Internet Rzeczy (IoT) to ekosystem połączonych urządzeń fizycznych wyposażonych w sensory, oprogramowanie i łączność sieciową, które zbierają i wymieniają dane automatycznie. Składa się z: urządzeń końcowych (sensory, aktuatory), komunikacji (WiFi, Bluetooth, LoRa), przetwarzania danych (edge/cloud) i aplikacji. Umożliwia monitoring, automatyzację i optymalizację procesów w real-time.

### 2. Magistrala vs interfejs vs protokół

**KRÓTKA ODPOWIEDŹ:**
Magistrala to fizyczne medium komunikacyjne łączące wiele urządzeń (np. I2C, SPI). Interfejs to punkt połączenia między urządzeniami definiujący sposób wymiany sygnałów (np. UART, GPIO). Protokół to zestaw reguł komunikacji definiujący format i znaczenie wiadomości (np. HTTP, MQTT). Magistrala=fizyka, Interfejs=fizyka/logika, Protokół=logika.

### 3. Mikrokontroler vs mikroprocesor

**KRÓTKA ODPOWIEDŹ:**
Mikrokontroler (MCU) to kompletny system na jednym chipie zawierający CPU, RAM, Flash, I/O i peryferia. Niskie zużycie energii, dedykowany do konkretnych zadań, tani. Mikroprocesor (MPU) to tylko CPU wymagający zewnętrznych komponentów. Wyższa moc obliczeniowa, może uruchamiać OS, droższy. IoT przeważnie używa MCU ze względu na energooszczędność.

### 4. CISC (Complex Instruction Set Computing)

**KRÓTKA ODPOWIEDŹ:**
CISC to architektura procesora z bogatym zestawem złożonych instrukcji (100-300+). Jedna instrukcja może wykonać złożoną operację, instrukcje mają różną długość, używa microcode do dekodowania. Zalety: kompaktowy kod, łatwiejszy assembly. Wady: wolniejsze dekodowanie, większe zużycie energii. Przykład: x86 architecture.

### 5. RISC (Reduced Instruction Set Computing)

**KRÓTKA ODPOWIEDŹ:**
RISC to architektura z uproszczonym zestawem instrukcji (30-50). Wszystkie instrukcje tej samej długości, jedna instrukcja = jedna operacja, load/store architektura. Zalety: szybsze wykonanie, niższe zużycie energii, prostsze projektowanie. Wady: większe programy. Przykład: ARM architecture. Dominuje w IoT ze względu na energy efficiency.

### 6. UART vs USART

**KRÓTKA ODPOWIEDŹ:**
UART (Universal Asynchronous Receiver-Transmitter) obsługuje tylko komunikację asynchroniczną z liniami TX, RX, GND. Synchronizacja przez start/stop bity. USART (Universal Synchronous/Asynchronous Receiver-Transmitter) obsługuje oba tryby + dodatkową linię zegara (CLK) w trybie synchronicznym. USART ma wyższą prędkość w trybie sync ale większą złożoność.

### 7. Pojemność pasożytnicza

**KRÓTKA ODPOWIEDŹ:**
Pojemność pasożytnicza to niechciana pojemność elektryczna między przewodnikami w obwodzie powstająca z bliskiej odległości i dużej powierzchni. Powoduje spowolnienie sygnałów (większa stała RC), crosstalk między sygnałami, EMI i zwiększone zużycie energii. Minimalizuje się przez zwiększenie odległości ścieżek, ground plane i kontrolę impedancji.

### 8. Cechy sensora inteligentnego

**KRÓTKA ODPOWIEDŹ:**
Sensor inteligentny ma wbudowany mikroprocesor do lokalnego przetwarzania danych, auto-kalibrację, komunikację cyfrową (I2C/SPI), samodiagnostykę, konfigurowalność parametrów i często AI/ML capabilities. W przeciwieństwie do prostego sensora nie tylko mierzy ale również analizuje, filtruje i podejmuje decyzje lokalnie, redukując obciążenie sieci i poprawiając responsywność systemu.

### 9. Aktuatory

**KRÓTKA ODPOWIEDŹ:**
Aktuatory to urządzenia przekształcające sygnały elektryczne na działania mechaniczne/fizyczne. Typy: silniki elektryczne (DC, stepper, servo), elektrozawory, przekaźniki, pneumatyczne/hydrauliczne. W IoT sterowane przez PWM, cyfrowe sygnały lub protokoły komunikacyjne. Umożliwiają systemom IoT wpływanie na środowisko fizyczne - nie tylko monitoring ale również kontrolę.

### 10. PWM (Pulse Width Modulation)

**KRÓTKA ODPOWIEDŹ:**
PWM to technika kontroli mocy przez zmianę duty cycle (współczynnika wypełnienia) sygnału prostokątnego o stałej częstotliwości. Średnie napięcie = duty_cycle × napięcie_zasilania. Używane do kontroli jasności LED, prędkości silników, generowania dźwięku, sterowania serwomechanizmami. Bardzo energooszczędne bo przełącza tylko między 0 a 100% bez strat na rezystancji.

**Przykład sterowania:**
```c
// PWM dla kontroli jasności LED
void set_led_brightness(int brightness_percent) {
    int duty_cycle = (brightness_percent * 255) / 100;
    analogWrite(LED_PIN, duty_cycle);  // 0-255 na Arduino
}
```

