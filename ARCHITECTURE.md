# SAGA Architecture & Technical Design

**Document Version:** 1.0.0  
**Last Updated:** February 2025  
**Status:** Production Ready

---

## Table of Contents

1. [High-Level Architecture](#high-level-architecture)
2. [Technology Stack](#technology-stack)
3. [System Components](#system-components)
4. [Data Model](#data-model)
5. [API Architecture](#api-architecture)
6. [Authentication & Authorization](#authentication--authorization)
7. [Service Architecture](#service-architecture)
8. [Deployment Architecture](#deployment-architecture)
9. [Performance & Scaling](#performance--scaling)
10. [Security Architecture](#security-architecture)
11. [Monitoring & Operations](#monitoring--operations)

---

## High-Level Architecture

```
┌────────────────────────────────────────────────────────────────────┐
│                         CLIENT LAYER                               │
├──────────────────────────┬───────────────────────────────┬─────────┤
│ Web Browser              │ Mobile (WebView)              │ Desktop │
│ - React 19               │ - Capacitor                   │ Electron│
│ - Progressive Web App    │ - Android Native Features     │ (future)│
│ - Dark Theme             │ - iOS via Vercel Bridge       │         │
│ - Responsive Design      │ - Same Frontend Code          │         │
└──────────────────────────┴───────────────────────────────┴─────────┘
                           ↓ (HTTPS/REST)
                    ┌──────────────────┐
                    │  Load Balancer   │
                    │   (Render Edge)  │
                    └──────────────────┘
                              ↓
┌────────────────────────────────────────────────────────────────────┐
│                    API GATEWAY LAYER (Render)                      │
│  - CORS handling                                                   │
│  - Rate limiting (100 req/min)                                     │
│  - Request validation                                              │
│  - Error handling & logging                                        │
└────────────────────────────────────────────────────────────────────┘
                              ↓
┌────────────────────────────────────────────────────────────────────┐
│                  APPLICATION LAYER (ASP.NET Core)                  │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │ Controllers (API Endpoints)                                 │  │
│  │ - AuthController      (OAuth, JWT, Sessions)                │  │
│  │ - KullaniciController (User CRUD, Search)                   │  │
│  │ - IcerikController    (Books, Movies, Games)                │  │
│  │ - YorumController     (Reviews, Comments, Ratings)          │  │
│  │ - ListeController     (Collections, Lists)                  │  │
│  │ - AktiviteController  (Activity Feed, Timeline)             │  │
│  │ - BildirimController  (Notifications)                       │  │
│  │ - AiController        (Search, Recommendations)             │  │
│  └─────────────────────────────────────────────────────────────┘  │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │ Services (Business Logic)                                   │  │
│  │ - TokenService           (JWT generation & validation)      │  │
│  │ - TmdbService            (Movie/TV data)                    │  │
│  │ - GoogleBooksService     (Book metadata)                    │  │
│  │ - OpenLibraryService     (Book data)                        │  │
│  │ - GroqService            (AI semantic search)               │  │
│  │ - LocalAiService         (Vector embeddings)                │  │
│  │ - SemanticSearchService  (Search logic)                     │  │
│  │ - BildirimService        (Notification generation)          │  │
│  │ - ExternalApiServices    (Aggregated API calls)             │  │
│  └─────────────────────────────────────────────────────────────┘  │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │ Middleware                                                  │  │
│  │ - ExceptionHandlingMiddleware (Global error handling)       │  │
│  │ - JWT Authentication         (Token validation)            │  │
│  │ - CORS                        (Cross-origin requests)       │  │
│  │ - Rate Limiting               (DDoS protection)             │  │
│  │ - Request Logging             (Audit trail)                 │  │
│  └─────────────────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────────────────┘
                              ↓
┌────────────────────────────────────────────────────────────────────┐
│                   DATA ACCESS LAYER (EF Core)                      │
│  - Entity Framework Core 8.0                                       │
│  - LINQ to SQL translations                                        │
│  - Automatic migrations                                            │
│  - Change tracking & caching                                       │
│  - Connection pooling (Npgsql)                                     │
└────────────────────────────────────────────────────────────────────┘
                              ↓
┌────────────────────────────────────────────────────────────────────┐
│            DATABASE LAYER (Neon PostgreSQL Serverless)             │
│  - 20+ tables (normalized relational schema)                       │
│  - Triggers (auto timestamps, cascades)                            │
│  - Enum types (DBMS-level type safety)                             │
│  - Foreign key constraints                                         │
│  - Indexes (performance optimization)                              │
│  - Row-level security (future)                                     │
│  - Automatic backups & PITR (Point-In-Time Restore)               │
└────────────────────────────────────────────────────────────────────┘
```

---

## Technology Stack

### Client-Side

| Layer | Technology | Version | Purpose |
|-------|-----------|---------|---------|
| **Framework** | React | 19.2.0 | UI library |
| **Build Tool** | Vite | 7.2.4 | Fast bundler & dev server |
| **Language** | TypeScript | 5.9.3 | Type safety |
| **UI Library** | Mantine | 8.3.9 | Component library |
| **Styling** | Tailwind CSS | 3.4.18 | Utility-first CSS |
| **Routing** | React Router | 7.9.6 | Client-side routing |
| **State Management** | Context API + Custom | - | Auth, Theme, local state |
| **HTTP Client** | Axios | 1.13.2 | API requests |
| **Data Fetching** | TanStack Query | 5.90.10 | Caching & sync |
| **Animations** | Motion | 12.31.0 | Smooth transitions |
| **Notifications** | React Hot Toast | 2.6.0 | User feedback |
| **Icons** | Tabler Icons | 3.35.0 | SVG icon set |
| **Mobile** | Capacitor | 8.0.2 | Native Android wrapper |
| **Deployment** | Vercel | - | Hosting & CDN |

### Server-Side

| Layer | Technology | Version | Purpose |
|-------|-----------|---------|---------|
| **Framework** | ASP.NET Core | 8.0 | Web API framework |
| **Language** | C# | 12.0 | Server-side logic |
| **ORM** | Entity Framework Core | 8.0.0 | Database abstraction |
| **Database Driver** | Npgsql | 8.0.11 | PostgreSQL adapter |
| **Authentication** | JWT Bearer | 8.0.0 | Token-based auth |
| **API Documentation** | Swashbuckle | 6.6.2 | Swagger UI |
| **Health Checks** | AspNetCore.HealthChecks | 9.0.0 | Service monitoring |
| **JSON Serialization** | Newtonsoft.Json | 13.0.4 | JSON handling |
| **Containerization** | Docker | Latest | Container images |
| **Deployment** | Render | - | Managed hosting |

### Database

| Component | Technology | Version | Details |
|-----------|-----------|---------|---------|
| **Engine** | PostgreSQL | 14.0+ | Relational DBMS |
| **Hosting** | Neon | Latest | Serverless PaaS |
| **Features** | PostgreSQL Extensions | - | UUID, JSON, Enum types |
| **Backups** | Automated | Daily | Point-in-time restore |
| **Connection Pooling** | pgBouncer | Latest | Connection management |

### External Services

| Service | Purpose | API Type | Rate Limit |
|---------|---------|----------|-----------|
| **TMDB** | Movie/TV data | REST | 40 req/10s |
| **Google Books** | Book metadata | REST | 100k req/day |
| **Open Library** | Book data | REST | None |
| **Groq** | AI embeddings | REST | 10k req/day (free) |
| **Ollama** (optional) | Local semantic search | REST | Unlimited |

---

## System Components

### 1. Frontend (React Application)

**Responsibilities:**
- User interface rendering
- Client-side routing
- Authentication UI
- Form handling & validation
- Local state management
- API communication

**Key Directories:**
```
src/
├── pages/              # Route pages
│   └── glass/         # Main app pages
├── components/        # Reusable UI components
├── context/           # Global state (Auth, Theme)
├── services/          # API clients & external services
├── hooks/             # Custom React hooks
├── types/             # TypeScript definitions
└── utils/             # Helper functions
```

**State Management:**
- **Authentication:** `AuthContext` (user, token, loading)
- **Theme:** `ThemeContext` (dark/light mode)
- **Notifications:** `Toast` (react-hot-toast)
- **Data Caching:** `TanStack Query` (user profiles, content)

### 2. API Gateway (ASP.NET Core)

**Responsibilities:**
- HTTP request handling
- Route mapping
- Request validation
- Authentication/authorization
- Error handling
- CORS management
- Rate limiting
- Response formatting

**Controllers (20+ endpoints):**
- `AuthController` - OAuth, JWT, password reset
- `KullaniciController` - Users, search, profiles
- `IcerikController` - Content CRUD
- `YorumController` - Reviews, comments, ratings
- `ListeController` - User collections
- `AktiviteController` - Activity & timeline
- `BildirimController` - Notifications
- `AiController` - Semantic search, recommendations

### 3. Business Logic Layer (Services)

**Core Services:**
```
TokenService
├─ GenerateJwtToken(userId)
└─ ValidateToken(token)

TmdbService
├─ SearchMovies(query)
├─ GetMovieDetails(id)
└─ SearchTvShows(query)

GoogleBooksService
├─ SearchBooks(query)
├─ GetBookDetails(isbn)
└─ GetBooksByAuthor(author)

GroqService
├─ GenerateEmbedding(text)
├─ SemanticSearch(query)
└─ GenerateDescription(prompt)

BildirimService
├─ CreateNotification(actor, action, target)
├─ SendNotifications(userIds)
└─ GetNotificationsForUser(userId)
```

### 4. Data Access Layer (EF Core)

**Responsibilities:**
- Entity mapping
- Query execution
- Relationship management
- Migration execution
- Connection pooling

**DbContext:**
```csharp
public class SagaDbContext : DbContext
{
    public DbSet<Kullanici> Users { get; set; }
    public DbSet<Icerik> Content { get; set; }
    public DbSet<Yorum> Reviews { get; set; }
    // ... 17 more DbSets
}
```

### 5. Database (PostgreSQL/Neon)

**Key Features:**
- Normalized schema (3NF)
- Enum types (api_kaynak, icerik_turu, etc.)
- Triggers (auto timestamps)
- Foreign keys (referential integrity)
- Indexes (query performance)

---

## Data Model

### Entity Relationship Diagram (Simplified)

```
┌──────────────┐
│  Kullanici   │ (User)
├──────────────┤
│ Id           │
│ Eposta       │
│ KullaniciAdi │
│ ... 15 more  │
└──────────────┘
      │
      ├─→ Takip (Follow relationship)
      ├─→ Engellenen (Block relationship)
      ├─→ KullaniciAyarlari (Settings)
      ├─→ Icerik (Created content)
      ├─→ Yorum (Reviews)
      ├─→ Liste (Collections)
      ├─→ Puanlama (Ratings)
      └─→ Bildirim (Notifications)

┌──────────────┐
│   Icerik     │ (Content)
├──────────────┤
│ Id           │
│ Baslik       │
│ Aciklama     │
│ Tur (enum)   │
│ ... 12 more  │
└──────────────┘
      │
      ├─→ Yorum (Reviews)
      ├─→ Puanlama (Ratings)
      ├─→ IcerikFavori (Favorite)
      ├─→ KutuphaneDurumu (Status)
      ├─→ ListeIcerigi (In lists)
      ├─→ Oyuncu (Cast/People)
      ├─→ Aktivite (Activity)
      └─→ AktiviteBegeni (Like)

┌──────────────┐
│   Yorum      │ (Review/Comment)
├──────────────┤
│ Id           │
│ IcerikId     │
│ KullaniciId  │
│ Baslik       │
│ Icerik       │
│ Puan         │
│ ParentId     │ (For replies)
│ ... 8 more   │
└──────────────┘
      │
      ├─→ YorumBegeni (Like)
      └─→ Yorum (Replies)

┌──────────────┐
│   Liste      │ (Collection)
├──────────────┤
│ Id           │
│ Baslik       │
│ Aciklama     │
│ Tur (enum)   │
│ KullaniciId  │
│ ... 10 more  │
└──────────────┘
      │
      └─→ ListeIcerigi (Contents in list)

┌──────────────┐
│   Bildirim   │ (Notification)
├──────────────┤
│ Id           │
│ EksiyenId    │
│ SujeId       │
│ Turu (enum)  │
│ Okundu       │
│ ... 6 more   │
└──────────────┘
```

### Tables (20 entities)

**User Management:**
1. `Kullanici` - User profile
2. `KullaniciAyarlari` - User settings & preferences
3. `Takip` - Follow relationships
4. `Engellenen` - Block relationships

**Content:**
5. `Icerik` - Books, movies, games, anime
6. `Oyuncu` - Cast/directors/authors
7. `IcerikOyuncu` - Association table

**Social & Engagement:**
8. `Yorum` - Reviews & comments (hierarchical)
9. `YorumBegeni` - Review likes
10. `AktiviteBegeni` - Content likes
11. `IcerikFavori` - Favorites
12. `KutuphaneDurumu` - Library status (want-to-read, reading, read)

**Collections & Organization:**
13. `Liste` - User-created lists
14. `ListeIcerigi` - Contents in lists
15. `Puanlama` - Star ratings

**Activity & Notifications:**
16. `Aktivite` - User activities (for feed)
17. `AktiviteYorum` - Comments on activities
18. `Bildirim` - Notifications
19. `AktiviteYorumBegeni` - Likes on activity comments

**System:**
20. `KullaniciAyarlari` (duplicated above) or other system tables

---

## API Architecture

### REST Conventions

**Base URL:** `https://api.saga.app` (or `http://localhost:5054/api`)

**Resource Naming:**
```
GET    /api/kullanicilar              # List users
GET    /api/kullanicilar/:id          # Get user
POST   /api/kullanicilar              # Create user
PUT    /api/kullanicilar/:id          # Update user
DELETE /api/kullanicilar/:id          # Delete user
```

**Query Parameters:**
```
?limit=10&offset=0              # Pagination
?sort=createdAt&order=desc      # Sorting
?filter=status:active           # Filtering
?expand=reviews,creator         # Eagerly load relations
```

**Response Format:**
```json
{
  "success": true,
  "data": { /* resource data */ },
  "error": null,
  "pagination": {
    "limit": 10,
    "offset": 0,
    "total": 42,
    "pages": 5
  }
}
```

**Error Response:**
```json
{
  "success": false,
  "data": null,
  "error": {
    "code": "INVALID_INPUT",
    "message": "Email is required",
    "details": { "field": "email" }
  }
}
```

### Authentication Flow

```
1. User clicks "Login with Google"
   ↓
2. Frontend redirects to: https://neonauth.../oauth/authorize
   Parameters: client_id, redirect_uri, state, scope
   ↓
3. Google OAuth consent screen appears
   ↓
4. User grants permission
   ↓
5. Google redirects to: https://neonauth.../auth/callback?code=XXX&state=YYY
   ↓
6. Neon Auth exchanges code for JWT token
   ↓
7. Neon Auth redirects to: https://frontend.app/auth/callback?token=XXX
   ↓
8. Frontend stores token in memory (AuthContext)
   ↓
9. All subsequent requests include: Authorization: Bearer {token}
   ↓
10. Backend validates JWT token before processing
```

### Rate Limiting Strategy

```
Default: 100 requests / 60 seconds per IP

├─ Sliding window with 8 segments
├─ Each segment = 7.5 seconds
├─ When limit hit: 429 Too Many Requests
└─ Queue up to 2 requests during limit window
```

**Future Enhancement:**
- Auth endpoints: 5 requests / 15 minutes (brute force protection)
- Password reset: 3 requests / 1 hour (spam prevention)
- File uploads: 10 requests / 1 hour (resource protection)

---

## Authentication & Authorization

### JWT Token Structure

```
Header.Payload.Signature

Header: {
  "alg": "HS256",
  "typ": "JWT"
}

Payload: {
  "iss": "SagaPlatform",
  "aud": "SagaUsers",
  "sub": "user_id",
  "email": "user@example.com",
  "role": "User",
  "iat": 1739183600,
  "exp": 1739270000
}

Signature: HMACSHA256(
  base64UrlEncode(header) + "." + base64UrlEncode(payload),
  secret
)
```

### Role-Based Access Control (RBAC)

```
User Roles:
├─ SuperAdmin
│  └─ Can: Everything
│
├─ Admin
│  ├─ Can: Moderate content, manage users, create announcements
│  └─ Cannot: Delete database, access financials
│
├─ Editor
│  ├─ Can: Suggest content edits, report issues
│  └─ Cannot: Modify published content, manage users
│
├─ User
│  ├─ Can: Create reviews, lists, follow others
│  └─ Cannot: Modify other users' content
│
└─ Restricted
   ├─ Can: Read-only access (limited visibility)
   └─ Cannot: Create content (temp banned)
```

**Implementation:**
```csharp
[Authorize(Roles = "Admin,SuperAdmin")]
[HttpPost("api/icerik")]
public async Task<IActionResult> CreateContent([FromBody] IcerikCreateDto dto)
{
    // Only admins can create content
}
```

---

## Service Architecture

### External API Integration Pattern

```
Frontend Request
    ↓
Backend Controller
    ↓
Service Layer (IExternalApiService implementation)
    ├─ Validation
    ├─ Circuit Breaker logic
    ├─ Caching
    └─ Error handling
    ↓
External API (TMDB, Google Books, etc.)
    ↓
Response Transformation
    ↓
Frontend Response
```

### Semantic Search Service

```
User Query: "magic school adventure"
    ↓
1. Query Vector Embedding (via Groq or Ollama)
   "magic school adventure" → [0.234, -0.145, ... 1536 dimensions]
    ↓
2. Vector Similarity Search (PostgreSQL with pgvector extension)
   Find content with similar embeddings
    ↓
3. Rank by Relevance Score
   Results sorted by cosine similarity
    ↓
4. Return top-k matches to frontend
```

### Notification Service

```
Event Triggered (e.g., User creates review)
    ↓
BildirimService.CreateNotification()
    ├─ Actor: reviewer
    ├─ Action: "created_review"
    ├─ Target: content_id
    └─ Recipients: [content_creator, previous_reviewers, followers]
    ↓
Create Bildirim records in database
    ↓
(Optional) Send push notifications via FCM
    ↓
Recipients see in notification center
```

---

## Deployment Architecture

### Frontend Deployment Pipeline

```
Developer
    ↓
Push to GitHub (main branch)
    ↓
Vercel Webhook Triggered
    ↓
1. Install dependencies: npm ci
2. Build: npm run build (tsc + vite)
3. Run tests (future)
4. Deploy dist/ to Vercel CDN
    ↓
Distributed Globally (100+ Edge Locations)
    ↓
User Browser
```

### Backend Deployment Pipeline

```
Developer
    ↓
Push to GitHub (main branch)
    ↓
Render Webhook Triggered
    ↓
1. Pull source code
2. Install dependencies: dotnet restore
3. Build app: dotnet build
4. Build Docker image: docker build
5. Push to Render registry
6. Spin down old instance
7. Start new instance from image
    ↓
Health Check: /api/health
    ↓
Route traffic to new instance
    ↓
Ready for requests
```

### Database Deployment

```
EF Core Migration Created Locally
    ↓
Test on development database
    ↓
Commit migration to GitHub
    ↓
Application startup auto-applies migrations
    ↓
Neon Database Updated
```

### Environment Promotion

```
Local Development
    ↓ (push to dev branch)
Staging (preview deployments)
    ↓ (PR merged to main)
Production
    ↓ (manual release)
Mobile App (via App Store/Play Store)
```

---

## Performance & Scaling

### Frontend Optimization

**Build Optimization:**
```
Vite build results:
├─ index.js: 233.5 KB (gzipped ~70 KB)
├─ vendor bundle: 145 KB (gzipped ~45 KB)
└─ CSS: 32 KB (gzipped ~8 KB)
Total: ~123 KB gzipped (excellent)
```

**Runtime Optimization:**
- Code splitting (lazy loaded routes)
- Image optimization (responsive images)
- CSS-in-JS minification (Emotion)
- Tree shaking unused code

**Browser Caching:**
- CSS/JS: Immutable (cache forever)
- HTML: No cache (fresh on each load)
- Static assets: 1 year TTL

### Backend Optimization

**Database Query Optimization:**
- Indexes on frequently queried columns
- EXPLAIN ANALYZE used for slow queries
- Pagination (never fetch all rows)
- Eager loading of relations (prevent N+1)

**Caching Strategy:**
- HTTP caching (ETag, Last-Modified)
- Database query caching (EF Core)
- Distributed cache (Redis - optional)
- CDN edge caching (Vercel)

**Connection Pooling:**
- Npgsql: Default pool 10-30 connections
- Neon: Integrated pgBouncer (1000+ concurrent)

### Scaling Considerations

**Horizontal Scaling:**
```
Load Balancer
   ↓
┌──────────────┬──────────────┬──────────────┐
│ Instance 1   │ Instance 2   │ Instance 3   │
│ (Render)     │ (Render)     │ (Render)     │
└──────────────┴──────────────┴──────────────┘
   ↓
Shared Database
(Neon handles scaling)
```

**Vertical Scaling:**
- Render: Upgrade plan from Free → Standard → Pro
- Neon: Auto-scales compute credits

---

## Security Architecture

### Defense in Depth

```
              User
              ↓
         1. HTTPS/TLS
         ↓
    2. CORS validation
    ↓
3. Rate limiting
    ↓
4. JWT validation
    ↓
5. Authorization checks
    ↓
6. Input validation
    ↓
7. SQL injection prevention (EF Core)
    ↓
8. Database encryption
    ↓
         Protected Resources
```

### Security Layers

1. **Transport Security:** TLS 1.2+ (HTTPS everywhere)
2. **Perimeter Security:** CORS, rate limiting, WAF (via Render)
3. **Authentication:** OAuth 2.0, JWT tokens
4. **Authorization:** Role-based access control
5. **Input Security:** Type validation, sanitization
6. **Data Security:** Database encryption, secrets in environment
7. **Audit:** Logging without sensitive data

---

## Monitoring & Operations

### Health Checks

```
GET /api/health
{
  "status": "healthy",
  "database": "connected",
  "externalApis": {
    "tmdb": "ok",
    "googleBooks": "ok",
    "groq": "ok"
  },
  "uptime": "24 days"
}
```

### Logging Strategy

```
Level   | When                      | Destination
--------|---------------------------|------------------
ERROR   | Exceptions, failures      | Console + Logs
WARN    | Rate limits, slow queries  | Console + Logs
INFO    | Auth events, API calls    | Logs
DEBUG   | Variable values, flow     | Logs (dev only)
```

**Log Aggregation:**
- Render: Built-in log dashboard
- Vercel: Function logs in dashboard
- Neon: Query logs in console

### Monitoring & Alerting

**Recommended Setup (future):**
- Grafana (visualization)
- Prometheus (metrics collection)
- Datadog (APM)
- Sentry (error tracking)

---

## Future Architecture Enhancements

1. **GraphQL API:** Alongside REST for flexibility
2. **Microservices:** Separate services for search, notifications
3. **Message Queue:** Kafka/RabbitMQ for async tasks
4. **Search Engine:** Elasticsearch for fast full-text search
5. **Caching Layer:** Redis for session & query caching
6. **CDN:** CloudFlare for edge caching
7. **Real-time Updates:** WebSockets for live feed
8. **Admin Dashboard:** Separate admin portal

---

## Conclusion

SAGA's architecture is designed for:
- ✅ **Reliability:** No single point of failure
- ✅ **Scalability:** Horizontal scaling when needed
- ✅ **Maintainability:** Clear separation of concerns
- ✅ **Security:** Defense in depth approach
- ✅ **Performance:** Optimized at every layer
- ✅ **Extensibility:** Easy to add new features

---

**For questions about architecture, please see CONTRIBUTING.md or create an issue.**
