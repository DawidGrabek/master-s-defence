---

## 8. Programowanie full-stack w chmurze obliczeniowej

### 8.1. Poziomy chmur komputerowych wykorzystywane w programowaniu aplikacji

**Modele usług chmurowych:**

**IaaS (Infrastructure as a Service):**
- **Definicja**: Dostawca udostępnia podstawową infrastrukturę IT
- **Zakres**: wirtualne maszyny, storage, networking, load balancers
- **Odpowiedzialność klienta**: OS, middleware, runtime, aplikacje, dane
- **Odpowiedzialność dostawcy**: wirtualizacja, serwery, storage, networking
- **Przykłady**: AWS EC2, Azure Virtual Machines, Google Compute Engine
- **Użytkownicy**: administratorzy systemów, DevOps engineers

**PaaS (Platform as a Service):**
- **Definicja**: Platforma dla deweloperów do tworzenia i wdrażania aplikacji
- **Zakres**: środowisko wykonawcze, narzędzia deweloperskie, bazy danych
- **Odpowiedzialność klienta**: aplikacje i dane
- **Odpowiedzialność dostawcy**: runtime, middleware, OS, infrastruktura
- **Przykłady**: Google App Engine, Azure App Service, AWS Elastic Beanstalk
- **Użytkownicy**: programiści, zespoły deweloperskie

**SaaS (Software as a Service):**
- **Definicja**: Gotowe aplikacje dostępne przez Internet
- **Zakres**: kompletne rozwiązania biznesowe
- **Odpowiedzialność klienta**: tylko dane użytkownika
- **Odpowiedzialność dostawcy**: całość infrastruktury i aplikacji
- **Przykłady**: Google Workspace, Office 365, Salesforce, Dropbox
- **Użytkownicy**: użytkownicy końcowi

**FaaS (Function as a Service / serverless):**
- **Definicja**: Wykonywanie pojedynczych funkcji w odpowiedzi na zdarzenia, bez zarządzania serwerami
- **Przykłady**: AWS Lambda, Azure Functions, Google Cloud Functions

---

### 8.2. Programowanie full-stack w chmurze komputerowej, zasady i rozwiązania

Full-stack łączy frontend (UI), backend (logika, API) i bazę danych.

**Zasady:**
- **Skalowalność** – aplikacja działa w środowisku rozproszonym
- **Resilience** – odporność na awarie (load balancing, replikacja)
- **Security by design** – uwierzytelnianie, szyfrowanie, kontrola dostępu
- **CI/CD** – ciągła integracja i dostarczanie w oparciu o pipeline’y (GitHub Actions, GitLab CI, Jenkins)

---

### 8.3. Usługi chmur komputerowych dostępne dla programistów full-stack

- **Compute** – EC2, App Engine, Azure App Service
- **Storage** – S3, Google Cloud Storage, Azure Blob
- **Databases** – Relacyjne (Aurora, Cloud SQL, Azure SQL) i nierelacyjne (DynamoDB, Firestore, Cosmos DB)
- **Networking** – API Gateway, Load Balancer, CDN
- **DevOps** – Terraform, CloudFormation, Pulumi
- **Monitoring** – CloudWatch, Azure Monitor, Stackdriver
- **AI/ML** – gotowe modele (Vision API, Rekognition)

---

### 8.4. Szkielety programistyczne wykorzystywane w programowaniu full-stack

- **Frontend:** React, Angular, Vue.js, Svelte
- **Backend:** Express.js (Node.js), Spring Boot (Java), Django/Flask (Python), ASP.NET Core (C#)
- **Full-stack frameworks:** Next.js, Nuxt.js, Meteor, Remix
- **Mobilne:** React Native, Flutter

---

### 8.5. Programowanie full-stack aplikacji stanowych w chmurze komputerowej

- Aplikacja stanowa przechowuje stan sesji użytkownika (pamięć serwera lub baza)
- Wymaga sticky sessions (np. load balancer kieruje użytkownika na tę samą instancję)
- Przykłady: gry online, czaty, e-commerce z koszykiem w pamięci
- Wsparcie: Redis, Memcached, ElastiCache jako wspólne magazyny stanu

---

### 8.6. Programowanie full-stack aplikacji bezstanowych w chmurze komputerowej

- Aplikacja bezstanowa – klient każde żądanie traktuje niezależnie
- Typowe dla API REST i serverless (FaaS)
- Skalowanie łatwiejsze – każda instancja może obsłużyć dowolne żądanie
- Stan przechowuje się w JWT (JSON Web Token) lub bazie danych

---

### 8.7. System Kubernetes w programowaniu full-stack

- Kubernetes to system orkiestracji kontenerów (Docker, Podman)
- Umożliwia automatyczne skalowanie aplikacji, rolling updates, service discovery, persistent volumes (np. bazy danych)
- Full-stack w Kubernetes – frontend i backend jako oddzielne deploymenty komunikujące się przez serwisy

---

### 8.8. Powiązanie aplikacji z chmurową bazą danych – sposoby i rodzaje wtyczek

- Bezpośrednie połączenie – connection string (JDBC, ODBC, MongoDB driver)
- ORM – Hibernate (Java), Entity Framework (C#), Sequelize (Node.js)
- BaaS – np. Firebase, Supabase z gotowym SDK i API
- Connection pooling – HikariCP, PgBouncer do obsługi wielu połączeń

---

### 8.9. Zarządzanie użytkownikami i bezpieczeństwem w aplikacjach opracowywanych w chmurze komputerowej

- **Uwierzytelnianie:** OAuth 2.0, OpenID Connect, JWT
- **Autoryzacja:** RBAC (Role Based Access Control), ABAC (Attribute Based Control)
- **Bezpieczeństwo danych:** TLS, szyfrowanie danych at rest (np. KMS)
- **Zarządzanie tożsamością:** AWS Cognito, Azure AD B2C, Firebase Auth
- **DevSecOps:** skanowanie podatności, polityki least privilege

---

### 8.10. Integracja innych serwisów z aplikacją chmurową

- REST API i GraphQL – wymiana danych  
- Webhooki – powiadomienia push  
- Message brokers – Kafka, RabbitMQ, AWS SQS, Google Pub/Sub  
- Integracje SaaS – płatności (Stripe, PayPal), mailing (SendGrid), analityka (Google Analytics)  
- Service Mesh (Istio) – zarządzanie komunikacją mikroserwisów  

---
