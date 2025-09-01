
---

## 13. Wzorce projektowe i techniki pisania czystego kodu

### 13.1. Wzorce konstrukcyjne (Creational Patterns)

**Definicja:**
Wzorce kreacyjne zajmują się procesem tworzenia obiektów. Zapewniają elastyczność w decydowaniu o tym, jakie obiekty mają zostać utworzone w danej sytuacji.

**Główne wzorce konstrukcyjne:**

**1. Singleton:**
```java
public class DatabaseConnection {
    private static DatabaseConnection instance;
    private String connectionString;

    private DatabaseConnection() {
        // Prywatny konstruktor
        this.connectionString = "jdbc:mysql://localhost:3306/mydb";
    }

    public static synchronized DatabaseConnection getInstance() {
        if (instance == null) {
            instance = new DatabaseConnection();
        }
        return instance;
    }

    public void executeQuery(String query) {
        System.out.println("Executing: " + query);
    }
}

// Użycie
DatabaseConnection db1 = DatabaseConnection.getInstance();
DatabaseConnection db2 = DatabaseConnection.getInstance();
// db1 == db2 (ten sam obiekt)
```

**Zastosowania Singleton:**
- Połączenia z bazą danych
- Konfiguracja aplikacji
- Cache
- Logowanie

**2. Factory Method:**
```java
// Interfejs produktu
interface Vehicle {
    void start();
    void stop();
}

// Konkretne produkty
class Car implements Vehicle {
    public void start() { System.out.println("Car started"); }
    public void stop() { System.out.println("Car stopped"); }
}

class Motorcycle implements Vehicle {
    public void start() { System.out.println("Motorcycle started"); }
    public void stop() { System.out.println("Motorcycle stopped"); }
}

// Abstrakcyjna fabryka
abstract class VehicleFactory {
    public abstract Vehicle createVehicle();

    // Template method
    public Vehicle orderVehicle() {
        Vehicle vehicle = createVehicle();
        vehicle.start();
        return vehicle;
    }
}

// Konkretne fabryki
class CarFactory extends VehicleFactory {
    @Override
    public Vehicle createVehicle() {
        return new Car();
    }
}

class MotorcycleFactory extends VehicleFactory {
    @Override
    public Vehicle createVehicle() {
        return new Motorcycle();
    }
}
```

**3. Builder Pattern:**
```java
public class Computer {
    private String CPU;
    private String RAM;
    private String storage;
    private String GPU;
    private boolean hasWiFi;

    private Computer(ComputerBuilder builder) {
        this.CPU = builder.CPU;
        this.RAM = builder.RAM;
        this.storage = builder.storage;
        this.GPU = builder.GPU;
        this.hasWiFi = builder.hasWiFi;
    }

    public static class ComputerBuilder {
        // Wymagane parametry
        private String CPU;
        private String RAM;

        // Opcjonalne parametry
        private String storage = "256GB SSD";
        private String GPU = "Integrated";
        private boolean hasWiFi = true;

        public ComputerBuilder(String CPU, String RAM) {
            this.CPU = CPU;
            this.RAM = RAM;
        }

        public ComputerBuilder storage(String storage) {
            this.storage = storage;
            return this;
        }

        public ComputerBuilder GPU(String GPU) {
            this.GPU = GPU;
            return this;
        }

        public ComputerBuilder hasWiFi(boolean hasWiFi) {
            this.hasWiFi = hasWiFi;
            return this;
        }

        public Computer build() {
            return new Computer(this);
        }
    }

    @Override
    public String toString() {
        return "Computer: " + CPU + ", " + RAM + ", " + storage + ", " + GPU + ", WiFi: " + hasWiFi;
    }
}

// Użycie
Computer gamingPC = new Computer.ComputerBuilder("Intel i9", "32GB")
    .storage("1TB SSD")
    .GPU("RTX 4080")
    .hasWiFi(true)
    .build();
```

### 13.2. Wzorce strukturalne (Structural Patterns)

**Definicja:**
Wzorce strukturalne opisują sposoby łączenia klas i obiektów w większe struktury, zachowując elastyczność i efektywność.

**Główne wzorce strukturalne:**

**1. Adapter:**
```java
// Stary interfejs
class OldPrinter {
    public void oldPrint(String text) {
        System.out.println("Old printer: " + text);
    }
}

// Nowy interfejs
interface ModernPrinter {
    void print(String document);
    void printColor(String document);
}

// Adapter
class PrinterAdapter implements ModernPrinter {
    private OldPrinter oldPrinter;

    public PrinterAdapter(OldPrinter oldPrinter) {
        this.oldPrinter = oldPrinter;
    }

    @Override
    public void print(String document) {
        oldPrinter.oldPrint(document);
    }

    @Override
    public void printColor(String document) {
        oldPrinter.oldPrint("[COLOR] " + document);
    }
}
```

**2. Decorator:**
```java
// Komponent bazowy
interface Coffee {
    double cost();
    String description();
}

// Konkretny komponent
class SimpleCoffee implements Coffee {
    @Override
    public double cost() { return 2.0; }

    @Override
    public String description() { return "Simple coffee"; }
}

// Bazowy dekorator
abstract class CoffeeDecorator implements Coffee {
    protected Coffee coffee;

    public CoffeeDecorator(Coffee coffee) {
        this.coffee = coffee;
    }
}

// Konkretne dekoratory
class MilkDecorator extends CoffeeDecorator {
    public MilkDecorator(Coffee coffee) { super(coffee); }

    @Override
    public double cost() { return coffee.cost() + 0.5; }

    @Override
    public String description() { return coffee.description() + ", milk"; }
}

class SugarDecorator extends CoffeeDecorator {
    public SugarDecorator(Coffee coffee) { super(coffee); }

    @Override
    public double cost() { return coffee.cost() + 0.2; }

    @Override
    public String description() { return coffee.description() + ", sugar"; }
}

// Użycie
Coffee coffee = new SimpleCoffee();
coffee = new MilkDecorator(coffee);
coffee = new SugarDecorator(coffee);
System.out.println(coffee.description() + " costs $" + coffee.cost());
// "Simple coffee, milk, sugar costs $2.7"
```

**3. Facade:**
```java
// Złożone podsystemy
class CPU {
    public void start() { System.out.println("CPU started"); }
    public void execute() { System.out.println("CPU executing"); }
    public void stop() { System.out.println("CPU stopped"); }
}

class Memory {
    public void load() { System.out.println("Memory loaded"); }
    public void free() { System.out.println("Memory freed"); }
}

class HardDrive {
    public void read() { System.out.println("HDD reading"); }
    public void write() { System.out.println("HDD writing"); }
}

// Fasada
class ComputerFacade {
    private CPU cpu;
    private Memory memory;
    private HardDrive hardDrive;

    public ComputerFacade() {
        this.cpu = new CPU();
        this.memory = new Memory();
        this.hardDrive = new HardDrive();
    }

    public void startComputer() {
        System.out.println("Starting computer...");
        cpu.start();
        memory.load();
        hardDrive.read();
        cpu.execute();
        System.out.println("Computer started successfully!");
    }

    public void shutdownComputer() {
        System.out.println("Shutting down...");
        cpu.stop();
        memory.free();
        System.out.println("Computer shutdown complete!");
    }
}
```

### 13.3. Wzorce behawioralne (Behavioral Patterns)

**Definicja:**
Wzorce behawioralne zajmują się komunikacją między obiektami oraz podziałem obowiązków.

**Główne wzorce behawioralne:**

**1. Observer:**
```java
import java.util.*;

// Interfejs obserwatora
interface Observer {
    void update(String news);
}

// Interfejs subiektu
interface Subject {
    void registerObserver(Observer observer);
    void removeObserver(Observer observer);
    void notifyObservers();
}

// Konkretny subiekt
class NewsAgency implements Subject {
    private List<Observer> observers;
    private String latestNews;

    public NewsAgency() {
        this.observers = new ArrayList<>();
    }

    @Override
    public void registerObserver(Observer observer) {
        observers.add(observer);
    }

    @Override
    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }

    @Override
    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(latestNews);
        }
    }

    public void setNews(String news) {
        this.latestNews = news;
        notifyObservers();
    }
}

// Konkretni obserwatorzy
class NewsChannel implements Observer {
    private String channelName;

    public NewsChannel(String channelName) {
        this.channelName = channelName;
    }

    @Override
    public void update(String news) {
        System.out.println(channelName + " reporting: " + news);
    }
}

// Użycie
NewsAgency agency = new NewsAgency();
NewsChannel cnn = new NewsChannel("CNN");
NewsChannel bbc = new NewsChannel("BBC");

agency.registerObserver(cnn);
agency.registerObserver(bbc);

agency.setNews("Breaking: New technology released!");
// CNN reporting: Breaking: New technology released!
// BBC reporting: Breaking: New technology released!
```

**2. Strategy:**
```java
// Interfejs strategii
interface PaymentStrategy {
    void pay(double amount);
}

// Konkretne strategie
class CreditCardPayment implements PaymentStrategy {
    private String cardNumber;

    public CreditCardPayment(String cardNumber) {
        this.cardNumber = cardNumber;
    }

    @Override
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " using Credit Card: " + 
                         cardNumber.substring(cardNumber.length() - 4));
    }
}

class PayPalPayment implements PaymentStrategy {
    private String email;

    public PayPalPayment(String email) {
        this.email = email;
    }

    @Override
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " using PayPal: " + email);
    }
}

// Kontekst
class ShoppingCart {
    private PaymentStrategy paymentStrategy;

    public void setPaymentStrategy(PaymentStrategy paymentStrategy) {
        this.paymentStrategy = paymentStrategy;
    }

    public void checkout(double amount) {
        if (paymentStrategy == null) {
            System.out.println("Please select a payment method");
            return;
        }
        paymentStrategy.pay(amount);
    }
}

// Użycie
ShoppingCart cart = new ShoppingCart();

cart.setPaymentStrategy(new CreditCardPayment("1234-5678-9012-3456"));
cart.checkout(100.0);

cart.setPaymentStrategy(new PayPalPayment("user@example.com"));
cart.checkout(50.0);
```

**3. Command:**
```java
// Interfejs polecenia
interface Command {
    void execute();
    void undo();
}

// Odbiornik
class Light {
    private boolean isOn = false;

    public void turnOn() {
        isOn = true;
        System.out.println("Light is ON");
    }

    public void turnOff() {
        isOn = false;
        System.out.println("Light is OFF");
    }
}

// Konkretne polecenia
class LightOnCommand implements Command {
    private Light light;

    public LightOnCommand(Light light) {
        this.light = light;
    }

    @Override
    public void execute() {
        light.turnOn();
    }

    @Override
    public void undo() {
        light.turnOff();
    }
}

class LightOffCommand implements Command {
    private Light light;

    public LightOffCommand(Light light) {
        this.light = light;
    }

    @Override
    public void execute() {
        light.turnOff();
    }

    @Override
    public void undo() {
        light.turnOn();
    }
}

// Invoker
class RemoteControl {
    private Command command;
    private Command lastCommand;

    public void setCommand(Command command) {
        this.command = command;
    }

    public void pressButton() {
        command.execute();
        lastCommand = command;
    }

    public void pressUndo() {
        if (lastCommand != null) {
            lastCommand.undo();
        }
    }
}
```

### 13.4. Różnice między wzorcami Proxy a Facade

**Proxy Pattern:**
- **Cel**: Kontrola dostępu do obiektu
- **Relacja**: Jeden-do-jednego (proxy-obiekt docelowy)
- **Funkcja**: Zastępuje oryginalny obiekt, dodając kontrolę dostępu
- **Interfejs**: Taki sam jak obiekt docelowy

```java
interface Image {
    void display();
}

class RealImage implements Image {
    private String filename;

    public RealImage(String filename) {
        this.filename = filename;
        loadFromDisk();
    }

    private void loadFromDisk() {
        System.out.println("Loading " + filename + " from disk");
    }

    @Override
    public void display() {
        System.out.println("Displaying " + filename);
    }
}

class ProxyImage implements Image {
    private RealImage realImage;
    private String filename;

    public ProxyImage(String filename) {
        this.filename = filename;
    }

    @Override
    public void display() {
        if (realImage == null) {
            realImage = new RealImage(filename);
        }
        realImage.display();
    }
}
```

**Facade Pattern:**
- **Cel**: Uproszczenie interfejsu do złożonego systemu
- **Relacja**: Jeden-do-wielu (fasada-podsystemy)
- **Funkcja**: Zapewnia prosty interfejs do skomplikowanych operacji
- **Interfejs**: Nowy, uproszczony

**Główne różnice:**
| Aspekt | Proxy | Facade |
|--------|--------|---------|
| **Liczba obiektów** | Jeden obiekt docelowy | Wiele podsystemów |
| **Cel** | Kontrola dostępu | Uproszczenie interfejsu |
| **Interfejs** | Identyczny | Nowy, prostszy |
| **Przykład** | Lazy loading obrazów | API do złożonego systemu |

### 13.5. Zasady SOLID

**S - Single Responsibility Principle (Zasada pojedynczej odpowiedzialności):**

**Definicja:** Klasa powinna mieć tylko jeden powód do zmiany.

```java
// ZŁAMANIE zasady - klasa ma wiele odpowiedzialności
class Employee {
    private String name;
    private double salary;

    public double calculatePay() { /* logika płacowa */ }
    public void save() { /* logika bazodanowa */ }
    public String generateReport() { /* logika raportów */ }
}

// PRZESTRZEGANIE zasady - podział odpowiedzialności
class Employee {
    private String name;
    private double salary;
    // gettery i settery
}

class PayrollService {
    public double calculatePay(Employee employee) { /* logika płacowa */ }
}

class EmployeeRepository {
    public void save(Employee employee) { /* logika bazodanowa */ }
}

class ReportGenerator {
    public String generateReport(Employee employee) { /* logika raportów */ }
}
```

**O - Open/Closed Principle (Zasada otwarte/zamknięte):**

**Definicja:** Klasy powinny być otwarte na rozszerzenia, ale zamknięte na modyfikacje.

```java
// ZŁAMANIE zasady - konieczność modyfikacji przy dodawaniu nowych kształtów
class AreaCalculator {
    public double calculateArea(Object shape) {
        if (shape instanceof Rectangle) {
            Rectangle rectangle = (Rectangle) shape;
            return rectangle.width * rectangle.height;
        } else if (shape instanceof Circle) {
            Circle circle = (Circle) shape;
            return Math.PI * circle.radius * circle.radius;
        }
        // Dodanie nowego kształtu wymaga modyfikacji tej metody
        return 0;
    }
}

// PRZESTRZEGANIE zasady - rozszerzenia bez modyfikacji
interface Shape {
    double calculateArea();
}

class Rectangle implements Shape {
    private double width, height;

    @Override
    public double calculateArea() {
        return width * height;
    }
}

class Circle implements Shape {
    private double radius;

    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

class AreaCalculator {
    public double calculateArea(Shape shape) {
        return shape.calculateArea();
    }
}
```

**L - Liskov Substitution Principle (Zasada podstawienia Liskov):**

**Definicja:** Obiekty klasy bazowej powinny móc być zastąpione obiektami klas pochodnych bez zmiany poprawności programu.

```java
// ZŁAMANIE zasady
class Bird {
    public void fly() {
        System.out.println("Flying");
    }
}

class Penguin extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Penguins can't fly!");
    }
}

// PRZESTRZEGANIE zasady
abstract class Bird {
    public abstract void move();
}

class FlyingBird extends Bird {
    @Override
    public void move() {
        fly();
    }

    public void fly() {
        System.out.println("Flying");
    }
}

class WalkingBird extends Bird {
    @Override
    public void move() {
        walk();
    }

    public void walk() {
        System.out.println("Walking");
    }
}

class Eagle extends FlyingBird { }
class Penguin extends WalkingBird { }
```

**I - Interface Segregation Principle (Zasada segregacji interfejsów):**

**Definicja:** Klienci nie powinni być zmuszani do zależności od interfejsów, których nie używają.

```java
// ZŁAMANIE zasady - "tłusty" interfejs
interface Worker {
    void work();
    void eat();
    void sleep();
}

class HumanWorker implements Worker {
    @Override
    public void work() { System.out.println("Working"); }
    @Override
    public void eat() { System.out.println("Eating"); }
    @Override
    public void sleep() { System.out.println("Sleeping"); }
}

class RobotWorker implements Worker {
    @Override
    public void work() { System.out.println("Working"); }
    @Override
    public void eat() { /* Robot nie je - martwa metoda */ }
    @Override
    public void sleep() { /* Robot nie śpi - martwa metoda */ }
}

// PRZESTRZEGANIE zasady - rozdzielone interfejsy
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

interface Sleepable {
    void sleep();
}

class HumanWorker implements Workable, Eatable, Sleepable {
    @Override
    public void work() { System.out.println("Working"); }
    @Override
    public void eat() { System.out.println("Eating"); }
    @Override
    public void sleep() { System.out.println("Sleeping"); }
}

class RobotWorker implements Workable {
    @Override
    public void work() { System.out.println("Working"); }
}
```

**D - Dependency Inversion Principle (Zasada odwrócenia zależności):**

**Definicja:** Moduły wysokiego poziomu nie powinny zależeć od modułów niskiego poziomu. Oba powinny zależeć od abstrakcji.

```java
// ZŁAMANIE zasady - zależność od konkretnej implementacji
class MySQLDatabase {
    public void save(String data) {
        System.out.println("Saving to MySQL: " + data);
    }
}

class OrderService {
    private MySQLDatabase database = new MySQLDatabase(); // Sztywna zależność

    public void processOrder(String orderData) {
        database.save(orderData);
    }
}

// PRZESTRZEGANIE zasady - zależność od abstrakcji
interface Database {
    void save(String data);
}

class MySQLDatabase implements Database {
    @Override
    public void save(String data) {
        System.out.println("Saving to MySQL: " + data);
    }
}

class MongoDatabase implements Database {
    @Override
    public void save(String data) {
        System.out.println("Saving to MongoDB: " + data);
    }
}

class OrderService {
    private Database database;

    public OrderService(Database database) { // Dependency Injection
        this.database = database;
    }

    public void processOrder(String orderData) {
        database.save(orderData);
    }
}

// Użycie
OrderService mysqlService = new OrderService(new MySQLDatabase());
OrderService mongoService = new OrderService(new MongoDatabase());
```

### 13.6. Inne zasady Clean Code

**DRY (Don't Repeat Yourself):**
```java
// ZŁAMANIE zasady DRY
public class UserValidator {
    public boolean validateRegistration(String email, String password) {
        if (email == null || email.trim().isEmpty()) {
            return false;
        }
        if (!email.contains("@") || !email.contains(".")) {
            return false;
        }
        if (password == null || password.length() < 8) {
            return false;
        }
        return true;
    }

    public boolean validateLogin(String email, String password) {
        if (email == null || email.trim().isEmpty()) {
            return false;
        }
        if (!email.contains("@") || !email.contains(".")) {
            return false;
        }
        if (password == null || password.length() < 8) {
            return false;
        }
        return true;
    }
}

// PRZESTRZEGANIE zasady DRY
public class UserValidator {
    public boolean validateRegistration(String email, String password) {
        return validateEmail(email) && validatePassword(password);
    }

    public boolean validateLogin(String email, String password) {
        return validateEmail(email) && validatePassword(password);
    }

    private boolean validateEmail(String email) {
        return email != null && !email.trim().isEmpty() && 
               email.contains("@") && email.contains(".");
    }

    private boolean validatePassword(String password) {
        return password != null && password.length() >= 8;
    }
}
```

**KISS (Keep It Simple, Stupid):**
```java
// ZŁAMANIE zasady KISS - zbyt skomplikowane
public class NumberProcessor {
    public boolean isEvenUsingComplexLogic(int number) {
        return ((number & 1) == 0) ? true : false;
    }
}

// PRZESTRZEGANIE zasady KISS - proste rozwiązanie
public class NumberProcessor {
    public boolean isEven(int number) {
        return number % 2 == 0;
    }
}
```

**YAGNI (You Aren't Gonna Need It):**
```java
// ZŁAMANIE zasady YAGNI - implementacja funkcji "na zapas"
public class User {
    private String name;
    private String email;
    private String phoneNumber; // Dodane "na wypadek gdyby"
    private String address;     // Dodane "na wypadek gdyby"
    private Date lastLoginTime; // Dodane "na wypadek gdyby"
    private List<String> preferences; // Dodane "na wypadek gdyby"

    // Konstruktor tylko z wymaganymi polami
    public User(String name, String email) {
        this.name = name;
        this.email = email;
        this.phoneNumber = "";
        this.address = "";
        this.preferences = new ArrayList<>();
    }

    // Metody dla wszystkich pól, nawet nieużywanych
}

// PRZESTRZEGANIE zasady YAGNI - tylko potrzebne funkcje
public class User {
    private String name;
    private String email;

    public User(String name, String email) {
        this.name = name;
        this.email = email;
    }

    // Tylko potrzebne metody
    public String getName() { return name; }
    public String getEmail() { return email; }
}
```

### 13.7. Techniki refaktoryzacji

**Extract Method:**
```java
// PRZED refaktoryzacją
public void printOwing() {
    printBanner();

    // Obliczenie outstanding
    Enumeration e = orders.elements();
    double outstanding = 0.0;
    while (e.hasMoreElements()) {
        Order each = (Order) e.nextElement();
        outstanding += each.getAmount();
    }

    // Drukowanie szczegółów
    System.out.println("name: " + name);
    System.out.println("amount: " + outstanding);
}

// PO refaktoryzacji
public void printOwing() {
    printBanner();
    double outstanding = getOutstanding();
    printDetails(outstanding);
}

private double getOutstanding() {
    Enumeration e = orders.elements();
    double outstanding = 0.0;
    while (e.hasMoreElements()) {
        Order each = (Order) e.nextElement();
        outstanding += each.getAmount();
    }
    return outstanding;
}

private void printDetails(double outstanding) {
    System.out.println("name: " + name);
    System.out.println("amount: " + outstanding);
}
```

**Extract Class:**
```java
// PRZED refaktoryzacją - klasa z wieloma odpowiedzialnościami
public class Person {
    private String name;
    private String officeAreaCode;
    private String officeNumber;

    public String getTelephoneNumber() {
        return ("(" + officeAreaCode + ") " + officeNumber);
    }

    public String getOfficeAreaCode() { return officeAreaCode; }
    public void setOfficeAreaCode(String arg) { officeAreaCode = arg; }
    public String getOfficeNumber() { return officeNumber; }
    public void setOfficeNumber(String arg) { officeNumber = arg; }
}

// PO refaktoryzacji - wydzielona klasa TelephoneNumber
public class Person {
    private String name;
    private TelephoneNumber officeTelephone = new TelephoneNumber();

    public String getTelephoneNumber() {
        return officeTelephone.getTelephoneNumber();
    }

    public TelephoneNumber getOfficeTelephone() {
        return officeTelephone;
    }
}

public class TelephoneNumber {
    private String areaCode;
    private String number;

    public String getTelephoneNumber() {
        return ("(" + areaCode + ") " + number);
    }

    public String getAreaCode() { return areaCode; }
    public void setAreaCode(String arg) { areaCode = arg; }
    public String getNumber() { return number; }
    public void setNumber(String arg) { number = arg; }
}
```

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
