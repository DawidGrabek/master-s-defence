
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

**Porównanie kontroli i odpowiedzialności:**
| Aspekt | IaaS | PaaS | SaaS |
|--------|------|------|------|
| **Kontrola** | Wysoka | Średnia | Niska |
| **Konfiguracja** | Pełna | Ograniczona | Minimalna |
| **Czas wdrożenia** | Długi | Średni | Krótki |
| **Koszty początkowe** | Wysokie | Średnie | Niskie |
| **Zarządzanie** | Klient | Współdzielone | Dostawca |

### 8.2. Programowanie full-stack w chmurze komputerowej - zasady i rozwiązania

**Architektura full-stack w chmurze:**

**Frontend (Client-side):**
- **Hosting**: AWS S3 + CloudFront, Azure Static Web Apps, Netlify
- **Frameworks**: React, Angular, Vue.js
- **Build tools**: Webpack, Vite, Parcel
- **CI/CD**: GitHub Actions, Azure DevOps, AWS CodePipeline

**Backend (Server-side):**
- **Compute**: AWS Lambda, Azure Functions, Google Cloud Functions
- **Containers**: AWS ECS/EKS, Azure Container Instances, Google GKE
- **Traditional servers**: AWS EC2, Azure VMs

**Database:**
- **Relacyjne**: AWS RDS, Azure SQL Database, Google Cloud SQL
- **NoSQL**: AWS DynamoDB, Azure Cosmos DB, Google Firestore
- **Cache**: AWS ElastiCache, Azure Cache for Redis

**Przykład architektury e-commerce:**
```
[React Frontend] → [API Gateway] → [Lambda Functions] → [RDS Database]
       ↓                                  ↓
[S3 + CloudFront]                  [SQS + SNS]
```

**Mikrousługi w chmurze:**
```yaml
# docker-compose.yml dla lokalnego rozwoju
version: '3.8'
services:
  user-service:
    build: ./user-service
    environment:
      - DB_HOST=postgres
      - REDIS_URL=redis:6379

  product-service:
    build: ./product-service
    environment:
      - DB_HOST=postgres

  api-gateway:
    build: ./api-gateway
    ports:
      - "3000:3000"
    depends_on:
      - user-service
      - product-service
```

**Wzorce projektowe dla chmury:**
- **API Gateway**: pojedynczy punkt wejścia
- **Circuit Breaker**: obsługa błędów serwisów
- **Event Sourcing**: zapisywanie zdarzeń zamiast stanu
- **CQRS**: oddzielenie odczytu od zapisu

### 8.3. Usługi chmur komputerowych dostępne dla programistów full-stack

**AWS (Amazon Web Services):**

**Compute:**
- **AWS Lambda**: serverless funkcje
- **EC2**: wirtualne maszyny
- **ECS/EKS**: kontenery i Kubernetes
- **Elastic Beanstalk**: PaaS dla aplikacji webowych

**Storage:**
- **S3**: object storage dla plików statycznych
- **EBS**: block storage dla EC2
- **EFS**: file system

**Database:**
- **RDS**: zarządzane bazy relacyjne
- **DynamoDB**: NoSQL database
- **ElastiCache**: in-memory cache

**Networking:**
- **CloudFront**: CDN
- **Route 53**: DNS
- **API Gateway**: zarządzanie API

**Azure (Microsoft):**

**Compute:**
- **Azure Functions**: serverless
- **App Service**: PaaS dla web apps
- **Container Instances**: kontenery
- **Virtual Machines**: IaaS

**Storage:**
- **Blob Storage**: object storage
- **Files**: managed file shares
- **Disk Storage**: block storage

**Database:**
- **Azure SQL Database**: zarządzany SQL Server
- **Cosmos DB**: globally distributed NoSQL
- **Cache for Redis**: in-memory cache

**Google Cloud Platform:**

**Compute:**
- **Cloud Functions**: serverless
- **App Engine**: PaaS
- **Kubernetes Engine**: managed Kubernetes
- **Compute Engine**: VMs

**Storage:**
- **Cloud Storage**: object storage
- **Persistent Disk**: block storage

**Database:**
- **Cloud SQL**: zarządzane MySQL/PostgreSQL
- **Firestore**: NoSQL document database
- **Memorystore**: Redis cache

### 8.4. Szkielety programistyczne wykorzystywane do programowania full-stack

**Frontend Frameworks:**

**React (Meta/Facebook):**
```jsx
// Komponenty funkcyjne z hooks
import React, { useState, useEffect } from 'react';

function UserProfile({ userId }) {
    const [user, setUser] = useState(null);
    const [loading, setLoading] = useState(true);

    useEffect(() => {
        fetch(`/api/users/${userId}`)
            .then(res => res.json())
            .then(data => {
                setUser(data);
                setLoading(false);
            });
    }, [userId]);

    if (loading) return <div>Loading...</div>;

    return (
        <div>
            <h1>{user.name}</h1>
            <p>{user.email}</p>
        </div>
    );
}
```

**Angular (Google):**
```typescript
// Component z TypeScript
import { Component, OnInit } from '@angular/core';
import { UserService } from './user.service';

@Component({
  selector: 'app-user-profile',
  template: `
    <div *ngIf="loading">Loading...</div>
    <div *ngIf="!loading">
      <h1>{{ user.name }}</h1>
      <p>{{ user.email }}</p>
    </div>
  `
})
export class UserProfileComponent implements OnInit {
  user: any = null;
  loading = true;

  constructor(private userService: UserService) {}

  ngOnInit() {
    this.userService.getUser(this.userId)
      .subscribe(data => {
        this.user = data;
        this.loading = false;
      });
  }
}
```

**Vue.js:**
```vue
<template>
  <div>
    <div v-if="loading">Loading...</div>
    <div v-else>
      <h1>{{ user.name }}</h1>
      <p>{{ user.email }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      user: null,
      loading: true
    }
  },
  async mounted() {
    const response = await fetch(`/api/users/${this.userId}`);
    this.user = await response.json();
    this.loading = false;
  }
}
</script>
```

**Backend Frameworks:**

**Node.js + Express:**
```javascript
const express = require('express');
const app = express();

app.use(express.json());

// REST API endpoints
app.get('/api/users/:id', async (req, res) => {
    const user = await User.findById(req.params.id);
    res.json(user);
});

app.post('/api/users', async (req, res) => {
    const user = new User(req.body);
    await user.save();
    res.status(201).json(user);
});

app.listen(3000, () => {
    console.log('Server running on port 3000');
});
```

**Python + FastAPI:**
```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class User(BaseModel):
    name: str
    email: str

@app.get("/api/users/{user_id}")
async def get_user(user_id: int):
    # Pobranie z bazy danych
    return {"id": user_id, "name": "John Doe", "email": "john@example.com"}

@app.post("/api/users")
async def create_user(user: User):
    # Zapis do bazy danych
    return {"message": "User created", "user": user}
```

### 8.5. Programowanie full-stack aplikacji stanowych (stateful) w chmurze

**Charakterystyka aplikacji stanowych:**
- Przechowują stan użytkownika/sesji na serwerze
- Wymagają trwałego storage (bazy danych)
- Trudniejsze skalowanie (sticky sessions)
- Większe zużycie zasobów

**Architektura aplikacji stanowej:**
```
[Load Balancer with Sticky Sessions] 
           ↓
[App Server 1] [App Server 2] [App Server 3]
           ↓           ↓           ↓
    [Session Store - Redis Cluster]
           ↓
    [Database - PostgreSQL]
```

**Zarządzanie sesją w Redis:**
```javascript
// Node.js z express-session i Redis
const session = require('express-session');
const RedisStore = require('connect-redis')(session);
const redis = require('redis');

const client = redis.createClient({
    host: 'redis-cluster.cache.amazonaws.com',
    port: 6379
});

app.use(session({
    store: new RedisStore({ client: client }),
    secret: 'your-secret-key',
    resave: false,
    saveUninitialized: false,
    cookie: {
        secure: process.env.NODE_ENV === 'production',
        httpOnly: true,
        maxAge: 1000 * 60 * 60 * 24 // 24 godziny
    }
}));

// Endpoint używający sesji
app.post('/api/login', (req, res) => {
    // Weryfikacja użytkownika
    req.session.userId = user.id;
    req.session.save(() => {
        res.json({ message: 'Logged in successfully' });
    });
});
```

**Wzorce dla aplikacji stanowych:**
- **Session Affinity**: kierowanie użytkownika na ten sam serwer
- **Distributed Session Storage**: współdzielony magazyn sesji
- **Database-backed Sessions**: sesje w bazie danych

### 8.6. Programowanie full-stack aplikacji bezstanowych (stateless) w chmurze

**Charakterystyka aplikacji bezstanowych:**
- Każde żądanie jest niezależne
- Brak przechowywania stanu na serwerze
- Łatwiejsze skalowanie (load balancing)
- Wyższa fault tolerance

**JWT (JSON Web Tokens) jako alternatywa dla sesji:**
```javascript
// Generowanie JWT
const jwt = require('jsonwebtoken');

app.post('/api/login', async (req, res) => {
    const user = await authenticateUser(req.body);

    const token = jwt.sign(
        { 
            userId: user.id, 
            email: user.email,
            exp: Math.floor(Date.now() / 1000) + (60 * 60) // 1 godzina
        },
        process.env.JWT_SECRET
    );

    res.json({ token });
});

// Middleware weryfikujący JWT
const authenticateToken = (req, res, next) => {
    const token = req.headers['authorization']?.split(' ')[1];

    if (!token) {
        return res.status(401).json({ error: 'Access denied' });
    }

    try {
        const decoded = jwt.verify(token, process.env.JWT_SECRET);
        req.user = decoded;
        next();
    } catch (error) {
        res.status(403).json({ error: 'Invalid token' });
    }
};

// Chroniony endpoint
app.get('/api/profile', authenticateToken, (req, res) => {
    res.json({ userId: req.user.userId, email: req.user.email });
});
```

**Serverless Functions (AWS Lambda):**
```javascript
// Funkcja Lambda - każde wywołanie jest bezstanowe
exports.handler = async (event) => {
    const { httpMethod, path, body } = event;

    if (httpMethod === 'GET' && path === '/users') {
        const users = await getUsersFromDB();
        return {
            statusCode: 200,
            headers: {
                'Content-Type': 'application/json',
                'Access-Control-Allow-Origin': '*'
            },
            body: JSON.stringify(users)
        };
    }

    return {
        statusCode: 404,
        body: JSON.stringify({ error: 'Not Found' })
    };
};
```

### 8.7. System Kubernetes w programowaniu full-stack

**Kubernetes dla aplikacji full-stack:**

**Pod - podstawowa jednostka:**
```yaml
# frontend-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: frontend-app
  labels:
    app: frontend
spec:
  containers:
  - name: react-app
    image: nginx:alpine
    ports:
    - containerPort: 80
    volumeMounts:
    - name: app-volume
      mountPath: /usr/share/nginx/html
  volumes:
  - name: app-volume
    configMap:
      name: frontend-config
```

**Deployment - zarządzanie Podami:**
```yaml
# api-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: api
  template:
    metadata:
      labels:
        app: api
    spec:
      containers:
      - name: api
        image: myregistry/api:latest
        ports:
        - containerPort: 3000
        env:
        - name: DB_HOST
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: database-host
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: password
        resources:
          limits:
            memory: "512Mi"
            cpu: "500m"
          requests:
            memory: "256Mi"
            cpu: "250m"
```

**Service - ekspozycja aplikacji:**
```yaml
# api-service.yaml
apiVersion: v1
kind: Service
metadata:
  name: api-service
spec:
  selector:
    app: api
  ports:
  - protocol: TCP
    port: 80
    targetPort: 3000
  type: LoadBalancer

---
# database-service.yaml
apiVersion: v1
kind: Service
metadata:
  name: database-service
spec:
  selector:
    app: database
  ports:
  - protocol: TCP
    port: 5432
    targetPort: 5432
  type: ClusterIP
```

**Ingress - routing HTTP:**
```yaml
# ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: app-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: myapp.example.com
    http:
      paths:
      - path: /api
        pathType: Prefix
        backend:
          service:
            name: api-service
            port:
              number: 80
      - path: /
        pathType: Prefix
        backend:
          service:
            name: frontend-service
            port:
              number: 80
```

**ConfigMap i Secret:**
```yaml
# configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  database-host: "postgres-service"
  api-url: "https://api.myapp.com"
  log-level: "info"

---
# secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  username: cG9zdGdyZXM=  # base64 encoded 'postgres'
  password: bXlwYXNzd29yZA==  # base64 encoded 'mypassword'
```

### 8.8. Powiązanie aplikacji z chmurową bazą danych

**AWS RDS Integration:**
```javascript
// Node.js z AWS RDS
const mysql = require('mysql2/promise');

const connection = mysql.createConnection({
    host: 'mydb.cluster-123.us-east-1.rds.amazonaws.com',
    user: process.env.DB_USER,
    password: process.env.DB_PASSWORD,
    database: 'myapp',
    ssl: {
        rejectUnauthorized: true
    },
    acquireTimeout: 60000,
    timeout: 60000
});

// Connection pooling
const pool = mysql.createPool({
    host: process.env.DB_HOST,
    user: process.env.DB_USER,
    password: process.env.DB_PASSWORD,
    database: process.env.DB_NAME,
    waitForConnections: true,
    connectionLimit: 10,
    queueLimit: 0
});

// Użycie w API
app.get('/api/users', async (req, res) => {
    try {
        const [rows] = await pool.execute('SELECT id, name, email FROM users');
        res.json(rows);
    } catch (error) {
        console.error('Database error:', error);
        res.status(500).json({ error: 'Database connection failed' });
    }
});
```

**Azure SQL Database z tożsamościami zarządzanymi:**
```javascript
// Node.js z Azure SQL bez haseł
const sql = require('mssql');
const { DefaultAzureCredential } = require('@azure/identity');

const credential = new DefaultAzureCredential();

const config = {
    server: 'myserver.database.windows.net',
    database: 'mydatabase',
    authentication: {
        type: 'azure-active-directory-msi-app-service'
    },
    options: {
        encrypt: true
    }
};

async function queryDatabase() {
    try {
        await sql.connect(config);
        const result = await sql.query`SELECT * FROM users`;
        return result.recordset;
    } catch (err) {
        console.error('SQL error', err);
        throw err;
    }
}
```

**ORM/ODM dla chmury:**
```javascript
// Prisma ORM z PostgreSQL w chmurze
// schema.prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id       Int     @id @default(autoincrement())
  email    String  @unique
  name     String?
  posts    Post[]
  profile  Profile?
}

model Post {
  id        Int     @id @default(autoincrement())
  title     String
  content   String?
  published Boolean @default(false)
  author    User    @relation(fields: [authorId], references: [id])
  authorId  Int
}

// Użycie w aplikacji
const { PrismaClient } = require('@prisma/client');
const prisma = new PrismaClient();

app.get('/api/users', async (req, res) => {
    const users = await prisma.user.findMany({
        include: {
            posts: true,
            profile: true
        }
    });
    res.json(users);
});
```

### 8.9. Zarządzanie użytkownikami i bezpieczeństwem w aplikacjach chmurowych

**Identity and Access Management (IAM):**

**AWS Cognito:**
```javascript
// Frontend - logowanie z AWS Cognito
import { CognitoUserPool, CognitoUser, AuthenticationDetails } from 'amazon-cognito-identity-js';

const userPool = new CognitoUserPool({
    UserPoolId: 'us-east-1_EXAMPLE',
    ClientId: 'your-client-id'
});

const authenticateUser = (username, password) => {
    return new Promise((resolve, reject) => {
        const user = new CognitoUser({ Username: username, Pool: userPool });
        const authDetails = new AuthenticationDetails({ Username: username, Password: password });

        user.authenticateUser(authDetails, {
            onSuccess: (result) => {
                const token = result.getAccessToken().getJwtToken();
                resolve(token);
            },
            onFailure: (err) => reject(err)
        });
    });
};

// Backend - weryfikacja tokenu
const jwt = require('jsonwebtoken');
const jwksClient = require('jwks-rsa');

const client = jwksClient({
    jwksUri: 'https://cognito-idp.us-east-1.amazonaws.com/us-east-1_EXAMPLE/.well-known/jwks.json'
});

const verifyToken = async (token) => {
    const getKey = (header, callback) => {
        client.getSigningKey(header.kid, (err, key) => {
            const signingKey = key.publicKey || key.rsaPublicKey;
            callback(null, signingKey);
        });
    };

    return new Promise((resolve, reject) => {
        jwt.verify(token, getKey, { algorithms: ['RS256'] }, (err, decoded) => {
            if (err) reject(err);
            else resolve(decoded);
        });
    });
};
```

**Azure Active Directory B2C:**
```javascript
// MSAL.js dla Azure AD B2C
import { PublicClientApplication } from '@azure/msal-browser';

const msalConfig = {
    auth: {
        clientId: 'your-client-id',
        authority: 'https://yourtenant.b2clogin.com/yourtenant.onmicrosoft.com/B2C_1_signupin',
        redirectUri: 'http://localhost:3000'
    }
};

const msalInstance = new PublicClientApplication(msalConfig);

const loginRequest = {
    scopes: ['openid', 'profile', 'https://yourtenant.onmicrosoft.com/api/read']
};

// Logowanie
const handleLogin = async () => {
    try {
        const response = await msalInstance.loginPopup(loginRequest);
        console.log('Login successful:', response);
    } catch (error) {
        console.error('Login failed:', error);
    }
};
```

**Role-Based Access Control (RBAC):**
```javascript
// Middleware RBAC
const authorize = (roles) => {
    return (req, res, next) => {
        const userRoles = req.user.roles; // z JWT token

        const hasPermission = roles.some(role => userRoles.includes(role));

        if (!hasPermission) {
            return res.status(403).json({ error: 'Insufficient permissions' });
        }

        next();
    };
};

// Użycie w endpointach
app.get('/api/admin/users', authenticate, authorize(['admin']), (req, res) => {
    // Tylko administratorzy mogą uzyskać dostęp
});

app.put('/api/posts/:id', authenticate, authorize(['author', 'editor']), (req, res) => {
    // Autorzy i edytorzy mogą edytować posty
});
```

### 8.10. Integracja innych serwisów z aplikacją chmurową

**API Gateway Pattern:**
```javascript
// Express.js jako API Gateway
const express = require('express');
const httpProxy = require('http-proxy-middleware');
const rateLimit = require('express-rate-limit');

const app = express();

// Rate limiting
const limiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minut
    max: 100, // maksymalnie 100 żądań
    message: 'Too many requests from this IP'
});

app.use(limiter);

// Routing do mikrousług
app.use('/api/users', httpProxy({
    target: 'http://user-service:3001',
    changeOrigin: true,
    pathRewrite: { '^/api/users': '' }
}));

app.use('/api/products', httpProxy({
    target: 'http://product-service:3002',
    changeOrigin: true,
    pathRewrite: { '^/api/products': '' }
}));

app.use('/api/orders', httpProxy({
    target: 'http://order-service:3003',
    changeOrigin: true,
    pathRewrite: { '^/api/orders': '' }
}));
```

**Event-Driven Architecture z AWS SQS/SNS:**
```javascript
// Producer - wysyłanie wiadomości do SQS
const AWS = require('aws-sdk');
const sqs = new AWS.SQS();

const sendMessage = async (queueUrl, message) => {
    const params = {
        QueueUrl: queueUrl,
        MessageBody: JSON.stringify(message),
        MessageAttributes: {
            'event-type': {
                DataType: 'String',
                StringValue: 'user-registered'
            }
        }
    };

    try {
        const result = await sqs.sendMessage(params).promise();
        console.log('Message sent:', result.MessageId);
    } catch (error) {
        console.error('Error sending message:', error);
    }
};

// Consumer - odbieranie wiadomości
const processMessages = async () => {
    const params = {
        QueueUrl: process.env.QUEUE_URL,
        MaxNumberOfMessages: 10,
        WaitTimeSeconds: 20
    };

    try {
        const data = await sqs.receiveMessage(params).promise();

        if (data.Messages) {
            for (const message of data.Messages) {
                const body = JSON.parse(message.Body);
                console.log('Processing message:', body);

                // Przetwarzanie wiadomości
                await handleUserRegistration(body);

                // Usunięcie z kolejki po przetworzeniu
                await sqs.deleteMessage({
                    QueueUrl: process.env.QUEUE_URL,
                    ReceiptHandle: message.ReceiptHandle
                }).promise();
            }
        }
    } catch (error) {
        console.error('Error processing messages:', error);
    }
};
```

**Integracja z zewnętrznymi API:**
```javascript
// Integracja z API płatności (Stripe)
const stripe = require('stripe')(process.env.STRIPE_SECRET_KEY);

app.post('/api/process-payment', async (req, res) => {
    const { amount, currency, paymentMethodId } = req.body;

    try {
        const paymentIntent = await stripe.paymentIntents.create({
            amount,
            currency,
            payment_method: paymentMethodId,
            confirmation_method: 'manual',
            confirm: true,
            return_url: 'https://myapp.com/payment/complete'
        });

        if (paymentIntent.status === 'succeeded') {
            // Aktualizacja zamówienia w bazie
            await updateOrderStatus(req.body.orderId, 'paid');

            // Wysłanie emaila potwierdzającego
            await sendConfirmationEmail(req.body.userEmail);
        }

        res.json({
            success: true,
            paymentIntent: {
                id: paymentIntent.id,
                status: paymentIntent.status
            }
        });
    } catch (error) {
        console.error('Payment error:', error);
        res.status(400).json({ error: error.message });
    }
});

// Integracja z usługą email (SendGrid)
const sgMail = require('@sendgrid/mail');
sgMail.setApiKey(process.env.SENDGRID_API_KEY);

const sendConfirmationEmail = async (email) => {
    const msg = {
        to: email,
        from: 'noreply@myapp.com',
        subject: 'Order Confirmation',
        text: 'Thank you for your order!',
        html: '<strong>Thank you for your order!</strong>'
    };

    try {
        await sgMail.send(msg);
        console.log('Email sent successfully');
    } catch (error) {
        console.error('Email error:', error);
    }
};
```

**Monitoring i Logging:**
```javascript
// Integracja z CloudWatch (AWS)
const cloudwatch = new AWS.CloudWatch();

const logMetric = async (metricName, value) => {
    const params = {
        Namespace: 'MyApp/API',
        MetricData: [{
            MetricName: metricName,
            Value: value,
            Unit: 'Count',
            Timestamp: new Date()
        }]
    };

    try {
        await cloudwatch.putMetricData(params).promise();
    } catch (error) {
        console.error('CloudWatch error:', error);
    }
};

// Middleware do logowania metryk
app.use((req, res, next) => {
    const start = Date.now();

    res.on('finish', () => {
        const duration = Date.now() - start;
        logMetric('ResponseTime', duration);
        logMetric('RequestCount', 1);

        if (res.statusCode >= 400) {
            logMetric('ErrorCount', 1);
        }
    });

    next();
});
```
