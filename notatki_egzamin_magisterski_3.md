
---

## 7. Programowanie aplikacji internetowych w JEE

### 7.1. Model aplikacji wielowarstwowej Java/Jakarta EE i podstawowe komponenty webowe

**Jakarta EE (dawniej Java EE):**
- Platforma do tworzenia rozproszonych aplikacji wielowarstwowych
- Przejęta przez Eclipse Foundation w 2018 roku (zmiana nazwy z Java EE)
- Zestaw specyfikacji dla aplikacji enterprise

**Architektura wielowarstwowa:**

**Warstwa klienta/prezentacji:**
- Przeglądarki webowe
- Aplikacje mobilne
- Rich client applications

**Warstwa webowa:**
- **Servlets**: komponenty Java obsługujące HTTP
- **JSP (JavaServer Pages)**: dynamiczne strony HTML z kodem Java
- **JSF (JavaServer Faces)**: framework MVC dla aplikacji webowych
- **JAX-RS**: usługi RESTful

**Warstwa biznesowa:**
- **EJB (Enterprise JavaBeans)**: komponenty biznesowe
- **CDI (Contexts and Dependency Injection)**: zarządzanie cyklem życia obiektów
- **Bean Validation**: walidacja danych

**Warstwa danych:**
- **JPA (Java Persistence API)**: mapowanie obiektowo-relacyjne
- **JDBC**: dostęp do baz danych
- **JTA (Java Transaction API)**: zarządzanie transakcjami

**Przykład serwletu:**
```java
@WebServlet("/hello")
public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, 
                        HttpServletResponse response) 
                        throws ServletException, IOException {
        response.getWriter().println("Hello World!");
    }
}
```

### 7.2. Podstawowe mechanizmy zaimplementowane w Spring (IoC, DI, AOP)

**IoC (Inversion of Control):**
- Zasada projektowa odwracająca kontrolę
- Framework zarządza cyklem życia obiektów
- Obiekty nie tworzą swoich zależności samodzielnie
- Spring IoC Container (ApplicationContext) zarządza bean'ami

**DI (Dependency Injection):**
- Wzorzec implementujący IoC
- Wstrzykiwanie zależności przez framework
- Typy DI w Spring:

**Constructor Injection (zalecane):**
```java
@Service
public class UserService {
    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

**Setter Injection:**
```java
@Service
public class UserService {
    private UserRepository userRepository;

    @Autowired
    public void setUserRepository(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

**Field Injection:**
```java
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
}
```

**AOP (Aspect-Oriented Programming):**
- Programowanie aspektowe dla cross-cutting concerns
- **Aspekty**: moduły enkapsulujące funkcjonalności przecinające
- **Pointcuts**: miejsca w kodzie gdzie aspekt jest aplikowany
- **Advice**: kod wykonywany w pointcut
- **Weaving**: łączenie aspektów z kodem

**Przykład AOP:**
```java
@Aspect
@Component
public class LoggingAspect {
    @Around("@annotation(Loggable)")
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        Object proceed = joinPoint.proceed();
        long executionTime = System.currentTimeMillis() - start;
        System.out.println(joinPoint.getSignature() + " executed in " + executionTime + "ms");
        return proceed;
    }
}
```

### 7.3. Stereotypy w Spring

**Definicja:** Adnotacje oznaczające role komponentów w aplikacji Spring.

**Hierarchia stereotypów:**
```
@Component (główny stereotyp)
├── @Service (warstwa biznesowa)
├── @Repository (warstwa danych)
└── @Controller (warstwa prezentacji)
```

**@Component:**
- Generyczny stereotyp dla komponentów zarządzanych przez Spring
- Automatyczne wykrywanie przez component scanning
- Bazowa adnotacja dla pozostałych stereotypów

**@Service:**
- Oznacza komponenty warstwy biznesowej/usług
- Miejsce na logikę biznesową aplikacji
- Przykład: obliczenia, transformacje, orkiestracja

**@Repository:**
- Komponenty warstwy dostępu do danych (DAO)
- Automatyczna translacja wyjątków bazodanowych
- Enkapsulacja logiki dostępu do danych

**@Controller:**
- Komponenty warstwy prezentacji w Spring MVC
- Obsługuje żądania HTTP
- Zwraca widoki lub dane (z @ResponseBody)

**Przykład użycia:**
```java
@Controller
@RequestMapping("/users")
public class UserController {
    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping("/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.findById(id);
    }
}

@Service
public class UserService {
    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    @Transactional
    public User createUser(User user) {
        return userRepository.save(user);
    }
}

@Repository
public class UserRepositoryImpl implements UserRepository {
    @PersistenceContext
    private EntityManager entityManager;

    public User findById(Long id) {
        return entityManager.find(User.class, id);
    }
}
```

### 7.4. Rola kontrolera w aplikacjach Spring Web MVC

**Kontroler w wzorcu MVC:**
- **Model**: dane i logika biznesowa
- **View**: prezentacja (JSP, Thymeleaf, JSON)
- **Controller**: obsługa żądań i orchestracja

**Główne zadania kontrolera:**
1. **Mapowanie żądań**: @RequestMapping i warianty
2. **Przetwarzanie danych wejściowych**: @RequestParam, @PathVariable
3. **Walidacja**: @Valid, BindingResult
4. **Wywołanie logiki biznesowej**: delegacja do serwisów
5. **Przygotowanie modelu**: ModelAndView, Model
6. **Zwrócenie odpowiedzi**: widok lub dane

**Podstawowe adnotacje kontrolera:**

**@Controller vs @RestController:**
```java
// Tradycyjny kontroler - zwraca widoki
@Controller
public class WebController {
    @RequestMapping("/home")
    public String home(Model model) {
        model.addAttribute("message", "Hello World");
        return "home"; // nazwa widoku JSP/Thymeleaf
    }
}

// REST kontroler - zwraca dane (JSON/XML)
@RestController
public class ApiController {
    @GetMapping("/api/users")
    public List<User> getUsers() {
        return userService.getAllUsers(); // automatycznie JSON
    }
}
```

**Mapowanie żądań:**
```java
@Controller
@RequestMapping("/products")
public class ProductController {

    @GetMapping // GET /products
    public String listProducts() { }

    @GetMapping("/{id}") // GET /products/123
    public String getProduct(@PathVariable Long id) { }

    @PostMapping // POST /products
    public String createProduct(@RequestBody Product product) { }

    @PutMapping("/{id}") // PUT /products/123
    public String updateProduct(@PathVariable Long id, @RequestBody Product product) { }

    @DeleteMapping("/{id}") // DELETE /products/123
    public String deleteProduct(@PathVariable Long id) { }
}
```

### 7.5. Niezbędne adnotacje stosowane w klasie encji w Spring

**JPA - podstawowe adnotacje encji:**

**@Entity i @Table:**
```java
@Entity
@Table(name = "users", schema = "public")
public class User {
    // definicja encji
}
```

**Klucz główny:**
```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
@Column(name = "user_id")
private Long id;

// Alternatywnie - UUID
@Id
@GeneratedValue(generator = "UUID")
@GenericGenerator(name = "UUID", strategy = "org.hibernate.id.UUIDGenerator")
@Column(name = "user_id", updatable = false, nullable = false)
private String id;
```

**Mapowanie kolumn:**
```java
@Column(name = "full_name", nullable = false, length = 100)
private String fullName;

@Column(name = "email", unique = true)
private String email;

@Column(name = "created_at", updatable = false)
@Temporal(TemporalType.TIMESTAMP)
private Date createdAt;

@Transient // nie mapowane do DB
private String temporaryField;
```

**Relacje między encjami:**
```java
@Entity
public class User {
    @Id
    private Long id;

    // Relacja jeden-do-wielu
    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    private List<Order> orders = new ArrayList<>();

    // Relacja wiele-do-jeden
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "department_id")
    private Department department;

    // Relacja jeden-do-jeden
    @OneToOne(cascade = CascadeType.ALL)
    @JoinColumn(name = "address_id", referencedColumnName = "id")
    private Address address;

    // Relacja wiele-do-wielu
    @ManyToMany
    @JoinTable(
        name = "user_roles",
        joinColumns = @JoinColumn(name = "user_id"),
        inverseJoinColumns = @JoinColumn(name = "role_id")
    )
    private Set<Role> roles = new HashSet<>();
}
```

### 7.6. Interfejs JpaRepository - wykorzystanie i metody

**Hierarchia repozytoriów Spring Data:**
```
Repository<T, ID>
└── CrudRepository<T, ID>
    └── PagingAndSortingRepository<T, ID>
        └── JpaRepository<T, ID>
```

**CrudRepository - podstawowe metody:**
- save(S entity)
- findById(ID id)
- findAll()
- count()
- deleteById(ID id)
- delete(T entity)

**JpaRepository - dodatkowe metody:**
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {

    // Dodatkowe metody JpaRepository:
    // flush() - natychmiastowe zapisanie do DB
    // saveAndFlush(T entity) - save + flush
    // deleteInBatch(Iterable<T> entities) - batch delete
    // getOne(ID id) - lazy loading reference

    // Query methods - automatyczne generowanie zapytań
    List<User> findByLastName(String lastName);
    List<User> findByEmailContaining(String email);
    Optional<User> findByEmail(String email);
    List<User> findByAgeGreaterThan(int age);
    List<User> findByDepartmentNameAndAgeGreaterThan(String deptName, int age);

    // Custom queries
    @Query("SELECT u FROM User u WHERE u.email = ?1")
    Optional<User> findByEmailAddress(String email);

    @Query(value = "SELECT * FROM users WHERE age > :age", nativeQuery = true)
    List<User> findUsersOlderThan(@Param("age") int age);

    @Modifying
    @Query("UPDATE User u SET u.active = false WHERE u.lastLogin < :date")
    int deactivateInactiveUsers(@Param("date") LocalDateTime date);
}
```

**Paginacja i sortowanie:**
```java
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;

    public Page<User> getUsers(int page, int size, String sortBy) {
        Pageable pageable = PageRequest.of(page, size, Sort.by(sortBy));
        return userRepository.findAll(pageable);
    }

    public List<User> getUsersSorted() {
        Sort sort = Sort.by(Sort.Direction.DESC, "createdAt")
                       .and(Sort.by(Sort.Direction.ASC, "lastName"));
        return userRepository.findAll(sort);
    }
}
```

### 7.7. Metody dostępu do baz danych w Spring - wzorce DAO i DTO

**Metody dostępu do danych:**

**1. Spring Data JPA (najczęściej używane):**
```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {
    List<Product> findByCategory(String category);
}
```

**2. JdbcTemplate:**
```java
@Repository
public class ProductDao {
    @Autowired
    private JdbcTemplate jdbcTemplate;

    public List<Product> findByCategory(String category) {
        return jdbcTemplate.query(
            "SELECT * FROM products WHERE category = ?",
            new Object[]{category},
            (rs, rowNum) -> new Product(
                rs.getLong("id"),
                rs.getString("name"),
                rs.getString("category")
            )
        );
    }
}
```

**3. EntityManager (JPA):**
```java
@Repository
public class ProductDao {
    @PersistenceContext
    private EntityManager entityManager;

    public List<Product> findByCategory(String category) {
        return entityManager
            .createQuery("SELECT p FROM Product p WHERE p.category = :category", Product.class)
            .setParameter("category", category)
            .getResultList();
    }
}
```

**Wzorzec DAO (Data Access Object):**
- Enkapsuluje logikę dostępu do danych
- Separuje logikę biznesową od dostępu do danych
- Interfejs abstrakcyjny dla operacji CRUD

**Przykład DAO:**
```java
// Interfejs DAO
public interface UserDao {
    User findById(Long id);
    List<User> findAll();
    User save(User user);
    void delete(Long id);
}

// Implementacja DAO
@Repository
public class UserDaoImpl implements UserDao {
    @PersistenceContext
    private EntityManager entityManager;

    @Override
    public User findById(Long id) {
        return entityManager.find(User.class, id);
    }

    @Override
    @Transactional
    public User save(User user) {
        if (user.getId() == null) {
            entityManager.persist(user);
            return user;
        } else {
            return entityManager.merge(user);
        }
    }
}
```

**Wzorzec DTO (Data Transfer Object):**
- Obiekt przenoszący dane między warstwami
- Często używany do transferu danych przez API
- Może zawierać logikę walidacji i transformacji

**Przykład DTO:**
```java
// DTO dla API
public class UserDto {
    @NotBlank
    private String firstName;

    @NotBlank
    private String lastName;

    @Email
    private String email;

    private String departmentName;

    // konstruktory, gettery, settery

    // Metoda konwersji z encji
    public static UserDto from(User user) {
        UserDto dto = new UserDto();
        dto.setFirstName(user.getFirstName());
        dto.setLastName(user.getLastName());
        dto.setEmail(user.getEmail());
        dto.setDepartmentName(user.getDepartment().getName());
        return dto;
    }

    // Metoda konwersji do encji
    public User toEntity() {
        User user = new User();
        user.setFirstName(this.firstName);
        user.setLastName(this.lastName);
        user.setEmail(this.email);
        return user;
    }
}

// Użycie w kontrolerze
@RestController
@RequestMapping("/api/users")
public class UserController {

    @PostMapping
    public ResponseEntity<UserDto> createUser(@Valid @RequestBody UserDto userDto) {
        User user = userDto.toEntity();
        User savedUser = userService.save(user);
        return ResponseEntity.ok(UserDto.from(savedUser));
    }
}
```

### 7.8. Technologia odwzorowania obiektowo-relacyjnego (ORM)

**ORM (Object-Relational Mapping):**
- Technika mapowania obiektów na struktury relacyjne
- Eliminuje impedance mismatch między OOP a bazami relacyjnymi
- Automatyzuje operacje CRUD

**Główne zalety ORM:**
- Produktywność - mniej SQL-a do pisania
- Maintenance - zmiany schematu w jednym miejscu
- Vendor independence - niezależność od RDBMS
- Object-oriented approach - praca z obiektami zamiast tabelami

**JPA/Hibernate - główne mechanizmy:**

**Entity Lifecycle:**
```java
@Service
public class UserService {
    @PersistenceContext
    private EntityManager entityManager;

    @Transactional
    public void demonstrateLifecycle() {
        // 1. Transient - nowy obiekt
        User user = new User("John", "Doe");

        // 2. Persistent - zarządzany przez EntityManager
        entityManager.persist(user);
        user.setEmail("john@example.com"); // automatycznie zapisane

        // 3. Detached - po zamknięciu session/transaction
        entityManager.detach(user);

        // 4. Merge - ponowne dołączenie
        User managed = entityManager.merge(user);

        // 5. Removed - oznaczony do usunięcia
        entityManager.remove(managed);
    }
}
```

**Relacje w ORM:**

**@OneToMany i @ManyToOne - najefektywniejsze:**
```java
@Entity
public class Department {
    @Id
    private Long id;

    @OneToMany(mappedBy = "department", cascade = CascadeType.ALL)
    private List<Employee> employees = new ArrayList<>();

    // Helper methods dla bi-directional relationship
    public void addEmployee(Employee employee) {
        employees.add(employee);
        employee.setDepartment(this);
    }
}

@Entity
public class Employee {
    @Id
    private Long id;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "department_id")
    private Department department;
}
```

**Zaawansowane mapowania:**
```java
@Entity
@Inheritance(strategy = InheritanceType.JOINED) // strategia dziedziczenia
public abstract class Person {
    @Id
    @GeneratedValue
    private Long id;

    private String name;
}

@Entity
@Table(name = "employees")
public class Employee extends Person {
    private String position;

    @Embedded // wbudowany obiekt
    private Address address;

    @ElementCollection // kolekcja typów podstawowych
    @CollectionTable(name = "employee_skills")
    private Set<String> skills = new HashSet<>();
}

@Embeddable // klasa wbudowana
public class Address {
    private String street;
    private String city;
    private String zipCode;
}
```

### 7.9. Mechanizmy bezpieczeństwa w Spring Boot

**Spring Security - główne komponenty:**

**1. Authentication (uwierzytelnianie):**
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(authz -> authz
                .requestMatchers("/api/public/**").permitAll()
                .requestMatchers("/api/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .httpBasic(withDefaults())
            .formLogin(withDefaults())
            .jwt(withDefaults());

        return http.build();
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

**2. UserDetailsService:**
```java
@Service
public class CustomUserDetailsService implements UserDetailsService {

    @Autowired
    private UserRepository userRepository;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        User user = userRepository.findByUsername(username)
            .orElseThrow(() -> new UsernameNotFoundException("User not found"));

        return org.springframework.security.core.userdetails.User.builder()
            .username(user.getUsername())
            .password(user.getPassword())
            .authorities(user.getRoles().stream()
                .map(role -> new SimpleGrantedAuthority("ROLE_" + role.getName()))
                .collect(Collectors.toList()))
            .build();
    }
}
```

**3. Method-level Security:**
```java
@Configuration
@EnableMethodSecurity(prePostEnabled = true)
public class MethodSecurityConfig {
}

@Service
public class OrderService {

    @PreAuthorize("hasRole('ADMIN')")
    public void deleteOrder(Long orderId) {
        // tylko admin może usuwać
    }

    @PreAuthorize("@orderService.isOwner(#orderId, authentication.name)")
    public Order getOrder(Long orderId) {
        // tylko właściciel może zobaczyć
    }

    @PostAuthorize("returnObject.userId == authentication.name")
    public Order processOrder(Order order) {
        // sprawdzenie po wykonaniu
        return order;
    }
}
```

**4. JWT Authentication:**
```java
@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {

    @Override
    protected void doFilterInternal(HttpServletRequest request, 
                                   HttpServletResponse response, 
                                   FilterChain filterChain) throws ServletException, IOException {

        String token = getTokenFromRequest(request);

        if (token != null && jwtUtil.validateToken(token)) {
            String username = jwtUtil.getUsernameFromToken(token);
            UserDetails userDetails = userDetailsService.loadUserByUsername(username);

            UsernamePasswordAuthenticationToken authToken = 
                new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());

            SecurityContextHolder.getContext().setAuthentication(authToken);
        }

        filterChain.doFilter(request, response);
    }
}
```

### 7.10. Wzorzec REST w Spring Boot

**REST (Representational State Transfer):**
- Architekturalny styl dla usług webowych
- Bezstanowy protokół HTTP
- Reprezentacje zasobów (JSON, XML)
- Standardowe metody HTTP

**Zasady REST:**
1. **Stateless**: każde żądanie jest niezależne
2. **Cacheable**: odpowiedzi mogą być cache'owane
3. **Uniform Interface**: spójny interfejs
4. **Layered System**: architektura warstwowa
5. **Client-Server**: rozdzielenie klienta i serwera

**REST Controller w Spring Boot:**
```java
@RestController
@RequestMapping("/api/v1/products")
@Validated
public class ProductController {

    private final ProductService productService;

    public ProductController(ProductService productService) {
        this.productService = productService;
    }

    // GET /api/v1/products
    @GetMapping
    public ResponseEntity<Page<Product>> getAllProducts(
            @RequestParam(defaultValue = "0") int page,
            @RequestParam(defaultValue = "10") int size,
            @RequestParam(defaultValue = "id") String sortBy) {

        Pageable pageable = PageRequest.of(page, size, Sort.by(sortBy));
        Page<Product> products = productService.findAll(pageable);
        return ResponseEntity.ok(products);
    }

    // GET /api/v1/products/123
    @GetMapping("/{id}")
    public ResponseEntity<Product> getProduct(@PathVariable Long id) {
        return productService.findById(id)
            .map(ResponseEntity::ok)
            .orElse(ResponseEntity.notFound().build());
    }

    // POST /api/v1/products
    @PostMapping
    public ResponseEntity<Product> createProduct(@Valid @RequestBody ProductDto productDto) {
        Product product = productDto.toEntity();
        Product savedProduct = productService.save(product);

        URI location = ServletUriComponentsBuilder
            .fromCurrentRequest()
            .path("/{id}")
            .buildAndExpand(savedProduct.getId())
            .toUri();

        return ResponseEntity.created(location).body(savedProduct);
    }

    // PUT /api/v1/products/123
    @PutMapping("/{id}")
    public ResponseEntity<Product> updateProduct(@PathVariable Long id, 
                                                @Valid @RequestBody ProductDto productDto) {
        if (!productService.existsById(id)) {
            return ResponseEntity.notFound().build();
        }

        Product product = productDto.toEntity();
        product.setId(id);
        Product updatedProduct = productService.save(product);
        return ResponseEntity.ok(updatedProduct);
    }

    // DELETE /api/v1/products/123
    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteProduct(@PathVariable Long id) {
        if (!productService.existsById(id)) {
            return ResponseEntity.notFound().build();
        }

        productService.deleteById(id);
        return ResponseEntity.noContent().build();
    }

    // PATCH /api/v1/products/123/status
    @PatchMapping("/{id}/status")
    public ResponseEntity<Product> updateProductStatus(@PathVariable Long id, 
                                                      @RequestBody Map<String, String> status) {
        return productService.updateStatus(id, status.get("status"))
            .map(ResponseEntity::ok)
            .orElse(ResponseEntity.notFound().build());
    }
}
```

**Obsługa błędów w REST:**
```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(EntityNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleEntityNotFound(EntityNotFoundException ex) {
        ErrorResponse error = new ErrorResponse(
            HttpStatus.NOT_FOUND.value(),
            "Resource not found",
            ex.getMessage(),
            Instant.now()
        );
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
    }

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ValidationErrorResponse> handleValidation(
            MethodArgumentNotValidException ex) {

        Map<String, String> errors = ex.getBindingResult()
            .getFieldErrors()
            .stream()
            .collect(Collectors.toMap(
                FieldError::getField,
                FieldError::getDefaultMessage
            ));

        ValidationErrorResponse response = new ValidationErrorResponse(
            HttpStatus.BAD_REQUEST.value(),
            "Validation failed",
            errors,
            Instant.now()
        );

        return ResponseEntity.badRequest().body(response);
    }
}
```

**HATEOAS (Hypermedia as the Engine of Application State):**
```java
@RestController
public class BookController {

    @GetMapping("/books/{id}")
    public ResponseEntity<EntityModel<Book>> getBook(@PathVariable Long id) {
        Book book = bookService.findById(id);

        EntityModel<Book> bookModel = EntityModel.of(book)
            .add(linkTo(methodOn(BookController.class).getBook(id)).withSelfRel())
            .add(linkTo(methodOn(BookController.class).getAllBooks()).withRel("books"))
            .add(linkTo(methodOn(AuthorController.class).getAuthor(book.getAuthorId())).withRel("author"));

        return ResponseEntity.ok(bookModel);
    }
}
```
