# 📚 SAGA - Social Activity & Entertainment Aggregator

> A modern, full-stack platform for discovering, sharing, and engaging with books, movies, games, and creative content. Built with React, ASP.NET Core, and PostgreSQL.

[![Deployed on Vercel](https://img.shields.io/badge/Frontend-Vercel-000?style=flat&logo=vercel)](https://saga-hazel.vercel.app)
[![Backend on Render](https://img.shields.io/badge/Backend-Render-46E3B7?style=flat)](https://saga-backend-0r4u.onrender.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Open Source](https://img.shields.io/badge/Open-Source-brightgreen)](https://github.com)

---

## Table of Contents

- [Overview](#overview)
- [Screenshots](#screenshots)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
- [Setup & Installation](#setup--installation)
  - [Prerequisites](#prerequisites)
  - [Backend Setup](#backend-setup)
  - [Frontend Setup](#frontend-setup)
  - [Mobile (Capacitor/Android)](#mobile-capacitorandroid)
- [Environment Configuration](#environment-configuration)
- [Running Locally](#running-locally)
- [Deployment](#deployment)
- [Database](#database)
- [API Documentation](#api-documentation)
- [Project Structure](#project-structure)
- [Key Features Deep Dive](#key-features-deep-dive)
- [Security & Best Practices](#security--best-practices)
- [Contributing](#contributing)
- [Troubleshooting](#troubleshooting)
- [License](#license)

---

## Overview

SAGA is an all-in-one social platform designed for entertainment enthusiasts. Whether you're into books, movies, games, or anime, SAGA lets you:

- **Discover** new content with AI-powered semantic search
- **Share** your thoughts and reviews with the community
- **Organize** your library with smart collections
- **Connect** with like-minded users
- **Track** what you're reading, watching, and playing
- **Rate & Engage** with interactive community features

The platform is built using modern web technologies with a production-ready architecture supporting:

- 🔐 OAuth authentication via Google
- 📦 PostgreSQL database with EF Core migrations
- ⚡ Real-time notifications
- 🎨 Responsive, dark-themed UI
- 📱 Native Android app via Capacitor
- 🔍 AI-powered search with semantic understanding

---

## Screenshots

|              Login              |             Feed              |
| :-----------------------------: | :---------------------------: |
| ![Login](screenshots/login.png) | ![Feed](screenshots/feed.png) |

|          Discover & Filters           |          Content Detail           |
| :-----------------------------------: | :-------------------------------: |
| ![Discover](screenshots/discover.png) | ![Detail](screenshots/detail.png) |

|               Library               |               Profile               |
| :---------------------------------: | :---------------------------------: |
| ![Library](screenshots/library.png) | ![Profile](screenshots/profile.png) |

|               Other User Profile                |               Settings                |
| :---------------------------------------------: | :-----------------------------------: |
| ![Other Profile](screenshots/profile-other.png) | ![Settings](screenshots/settings.png) |

---

## Features

### Core Content Management

- **📖 Library Management**: Track books, movies, games, and anime across multiple lists
- **⭐ Rating System**: Rate content on a 1-5 star scale with detailed reviews
- **💬 Community Engagement**: Comment on reviews, reply to comments, like content
- **📝 Content Curation**: Create and manage multiple lists (reading lists, favorites, wish lists, etc.)

### Smart Discovery

- **🤖 AI-Powered Search**: Semantic search across titles, descriptions, and user reviews
- **🔗 External Data Integration**: Pull data from The Movie Database (TMDB), Google Books API, Open Library
- **📊 Trending Content**: See what the community is discussing
- **🎯 Personalized Recommendations**: Based on your library and activity

### Social Features

- **👥 User Profiles**: Customizable profiles with activity history
- **🔗 Follow System**: Keep track of users you want to follow
- **🚫 Block/Report**: Moderation tools for a safe community
- **📢 Real-time Notifications**: Get notified about likes, follows, and replies
- **💌 Messaging**: Direct message other users (future enhancement)

### Authentication & Security

- **🔐 OAuth 2.0 with Google**: Secure, passwordless authentication
- **🛡️ JWT Tokens**: Session management with automatic refresh
- **🔑 Password Reset**: Secure email-based password recovery
- **🚫 CORS & Rate Limiting**: API protection against abuse

### Mobile Experience

- **📱 Native Android App**: Built with Capacitor for native-feel experience
- **🎨 Responsive Design**: Works seamlessly on all screen sizes
- **💾 Offline Capable**: Basic functionality works offline with sync on reconnect
- **🔔 Push Notifications**: Real-time alerts on mobile

---

## Tech Stack

### Frontend

- **React 19** - UI framework
- **Vite 7.2.4** - Build tool (HMR, optimized bundles)
- **TypeScript** - Type-safe JavaScript
- **Tailwind CSS** - Utility-first styling
- **Mantine 8.3.9** - Component library
- **React Router 7.9** - Client-side routing
- **Axios** - HTTP client with interceptors
- **TanStack Query** - Data fetching & caching
- **React Hot Toast** - Toast notifications
- **Motion/Framer** - Smooth animations

### Backend

- **ASP.NET Core 8.0** - Web framework
- **Entity Framework Core 8.0** - ORM
- **PostgreSQL** - Primary database
- **Npgsql 8.0** - PostgreSQL adapter with enum mapping
- **JWT Bearer** - Token authentication
- **Swagger/Swashbuckle** - API documentation
- **Health Checks** - Monitoring endpoints

### External APIs

- **TMDB API** - Movies & TV shows data
- **Google Books API** - Book information
- **Open Library API** - Additional book data
- **Groq API** - AI-powered features
- **Local AI (Ollama)** - On-premise semantic search

### Deployment & Infrastructure

- **Render** - Backend hosting (Docker containers)
- **Vercel** - Frontend hosting & CDN
- **Neon** - PostgreSQL as a Service (with scaling)
- **Capacitor** - Native cross-platform wrapper
- **Docker** - Containerization for backend

---

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     Client Layer                             │
├──────────────────────────┬──────────────────────────────────┤
│  Web Browser (Vite)      │  Mobile (Capacitor/Android)      │
│  - React 19              │  - WebView wrapper                │
│  - TypeScript UI         │  - Native features via plugins    │
│  - Dark theme            │  - Same frontend code             │
└──────────────────────────┴──────────────────────────────────┘
                              ↓ (HTTPS/REST)
                         ┌────────────────┐
                         │ Load Balancer  │
                         └────────────────┘
                              ↓ (Port 443)
┌─────────────────────────────────────────────────────────────┐
│              API Gateway & Reverse Proxy (Render)            │
├─────────────────────────────────────────────────────────────┤
│  ASP.NET Core 8.0                                            │
│  - CORS handling                                             │
│  - Rate limiting                                             │
│  - JWT validation                                            │
│  - Request logging                                           │
└─────────────────────────────────────────────────────────────┘
            ↓ Controllers ↓ Services ↓ Middleware
┌─────────────────────────────────────────────────────────────┐
│              Business Logic & Data Access                    │
├─────────────────────────────────────────────────────────────┤
│  EF Core DbContext                                           │
│  - 20+ Entity Models                                         │
│  - Complex queries with LINQ                                │
│  - Relationship management                                   │
│  - Automatic migrations                                      │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│              Data Layer (PostgreSQL via Neon)                │
├─────────────────────────────────────────────────────────────┤
│  - 20+ Tables                                                │
│  - Triggers for automatic timestamps                         │
│  - Foreign key constraints                                   │
│  - Enum types (DBMS-level)                                   │
│  - Row-level indexes for performance                         │
│  - Automatic backups & point-in-time restore                │
└─────────────────────────────────────────────────────────────┘
```

### API Standards

- **REST** architecture with clear resource hierarchy
- **JSON** request/response format
- **JWT** Bearer tokens in Authorization header
- **Pagination** with limit/offset for large datasets
- **Error responses** with consistent error codes
- **CORS** enabled for web/mobile clients

---

## Setup & Installation

### Prerequisites

**System Requirements:**

- Node.js 18.x+ (for frontend)
- .NET SDK 8.0+ (for backend)
- PostgreSQL 14+ or Neon account (database)
- Android Studio (for mobile build)
- Git

**Required Accounts:**

- Google Cloud Console (for OAuth)
- Neon account (for database)
- TMDB API key (for movie data)
- Google Books API key (for book data)
- Groq API key (for AI features)

### Backend Setup

```bash
# 1. Navigate to backend
cd saga-backend

# 2. Restore NuGet packages
dotnet restore

# 3. Create .env file with database connection
echo "DATABASE_CONNECTION_STRING=postgresql://user:password@host/database" > .env

# 4. Apply database migrations
dotnet ef database update

# 5. (Optional) Run database seeding
dotnet run -- --seed

# 6. Start the development server
dotnet run

# Backend API will be available at: http://localhost:5054/api
# Swagger documentation: http://localhost:5054/swagger
```

**Environment Variables** (see `.env.example`):

```env
# Database
DATABASE_CONNECTION_STRING=postgresql://user:password@host:5432/sagadb

# JWT & Auth
JWT_SECRET=your-secret-key-min-32-chars
JWT_ISSUER=SagaPlatform
JWT_AUDIENCE=SagaUsers
JWT_EXPIRY_MINUTES=1440

# TMDB (Movies/TV)
TMDB_API_KEY=your-tmdb-api-key
TMDB_BEARER_TOKEN=your-tmdb-bearer-token

# Google Books
GOOGLE_BOOKS_API_KEY_1=your-key-1
GOOGLE_BOOKS_API_KEY_2=your-key-2

# AI Services
GROQ_API_KEY=your-groq-api-key
AI_SEMANTIC_SEARCH_URL=http://localhost:8000/v1/embeddings
LOCAL_AI_BASE_URL=http://localhost:11434
LOCAL_AI_MODEL=mistral

# CORS (production)
CORS_ALLOWED_ORIGINS=https://yourdomain.com,https://www.yourdomain.com

# Rate Limiting
RATE_LIMIT_REQUESTS=100
RATE_LIMIT_WINDOW_SECONDS=60
```

### Frontend Setup

```bash
# 1. Navigate to frontend
cd saga-frontend

# 2. Install dependencies
npm install

# 3. Create environment file
cp .env.example .env.local

# 4. Add your API endpoint and Neon Auth URL
# (See "Environment Configuration" section)

# 5. Start development server
npm run dev

# Frontend will be available at: http://localhost:5173
```

**Frontend Environment** (`.env.local`):

```env
# API Endpoint (backend)
VITE_API_URL=http://localhost:5054/api

# Neon Auth (authentication service)
VITE_NEON_AUTH_URL=https://your-auth-domain.neonauth.c-2.eu-central-1.aws.neon.tech/yourdb/auth
```

### Mobile (Capacitor/Android)

```bash
# 1. Build web assets
npm run build

# 2. Sync with Capacitor
npx cap sync android

# 3. Open Android Studio
npx cap open android

# 4. Configure in Android Studio:
#    - Set up Android SDK if needed
#    - Connect device or start emulator
#    - Build: Build > Make Project
#    - Run: Run > Run 'app'

# APK will be generated in: app/build/outputs/apk/debug/app-debug.apk
```

**Android Configuration** (`capacitor.config.ts`):

```typescript
const config: CapacitorConfig = {
  appId: "com.saga.app",
  appName: "Saga",
  webDir: "dist",
  bundledWebRuntime: false,
  server: {
    androidScheme: "https",
    allowNavigation: [
      "*.neon.tech", // Auth domain
      "accounts.google.com",
      "*.googleapis.com",
    ],
  },
};
```

---

## Environment Configuration

### Development vs Production

The application uses environment-based configuration:

**Development** (.env.local / .env.development.json)

```
VITE_API_URL=http://localhost:5054/api
VITE_NEON_AUTH_URL=http://localhost:3000/auth
CORS_DEVELOPMENT=true
```

**Production** (Vercel Dashboard Environment Variables)

```
VITE_API_URL=https://saga-backend-0r4u.onrender.com/api
VITE_NEON_AUTH_URL=https://ep-cold-glade-ag9bn1r6.neonauth.c-2.eu-central-1.aws.neon.tech/neondb/auth
```

**Backend Production** (Render Environment Variables)

- All `${VAR_NAME}` placeholders in `appsettings.json` are resolved
- CORS_ALLOWED_ORIGINS added for each frontend domain

### Sensitive Data Handling

✅ **DO:** Store in environment variables  
✅ **DO:** Use `.env` files locally (ignored by git)  
✅ **DO:** Use platform dashboard for production (Vercel, Render)

❌ **DON'T:** Commit `.env`, API keys, or secrets to git  
❌ **DON'T:** Hardcode configuration in source code  
❌ **DON'T:** Share credentials in pull requests or issues

---

## Running Locally

### Quick Start (Dev Mode)

```bash
# Terminal 1 - Backend
cd saga-backend
cp appsettings.Development.json    # Ensure development config exists
dotnet run
# Running on: https://localhost:7054, http://localhost:5054

# Terminal 2 - Frontend
cd saga-frontend
npm run dev
# Running on: http://localhost:5173

# Terminal 3 - Test with cURL
curl -X GET http://localhost:5054/api/health
```

### Testing the API

```bash
# Get all books
curl http://localhost:5054/api/icerik/kitaplar

# Search users (case-insensitive)
curl 'http://localhost:5054/api/kullanicilar/search?q=john'

# Create a review (requires auth token)
curl -X POST http://localhost:5054/api/yorumlar \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{"icerikId":1,"baslik":"Great book!","icerik":"..."}'

# Get yearly summary
curl 'http://localhost:5054/api/istatistikler/yillik-ozet?yil=2025' \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### Database Inspection

```bash
# Using psql (if PostgreSQL installed locally)
psql -h localhost -U postgres -d sagadb

# Common queries
SELECT * FROM "Kullanici" LIMIT 5;
SELECT * FROM "Icerik" WHERE "Tur" = 'Kitap';
SELECT * FROM "Yorum" ORDER BY "OlusturmaTarihi" DESC LIMIT 10;

# For Neon database (via pgAdmin or similar)
# Connection string: postgresql://user:password@host/database
```

---

## Deployment

### Frontend Deployment (Vercel)

**Option 1: Automatic (Recommended)**

1. Push to GitHub
2. Vercel auto-deploys on `main` branch
3. Environment variables in Vercel Dashboard

**Option 2: Manual**

```bash
npm run build
# Uploads dist/ folder to Vercel
vercel --prod
```

### Backend Deployment (Render)

**Automatic Deployment:**

1. Render connected to GitHub repository
2. Auto-deploys on push to `main`
3. Environment variables in Render Dashboard

**Manual Deployment:**

```bash
# Push to GitHub (Render polls your repo)
git add .
git commit -m "Deploy changes"
git push origin main

# OR use Render CLI
render deploy --service=saga-backend
```

### Database Deployment (Neon)

```bash
# Connect to production database
psql postgresql://user:password@host/database

# Verify migrations applied
SELECT * FROM "__EFMigrationsHistory";

# Backup production database
pg_dump postgresql://user:password@host/database > backup.sql
```

---

## Database

### Schema Overview

[Database Schema Diagram - See ARCHITECTURE.md for details]

**20+ Tables:**

- **User Management**: `Kullanici`, `KullaniciAyarlari`, `Takip`, `Engellenen`
- **Content**: `Icerik`, `Oyuncu`, `Aktivite`, `Liste`, `ListeIcerigi`
- **Social**: `Yorum`, `AktiviteYorum`, `Bildirim`, `YorumBegeni`
- **Engagement**: `Puanlama`, `IcerikFavori`, `KutuphaneDurumu`, `AktiviteBegeni`

### Migrations

```bash
# Create new migration
dotnet ef migrations add MigrationName

# Apply migrations
dotnet ef database update

# Rollback last migration
dotnet ef database update PreviousMigration
```

### Stored Procedures & Triggers

The database includes:

- **Timestamps**: Auto-updated `OlusturmaTarihi` and `GuncellemeTarihi`
- **Triggers**: Automatic activity logging
- **Enums**: `api_kaynak`, `icerik_turu`, `kullanici_rol`, etc.

---

## API Documentation

### Authentication Flow

```
1. User clicks "Login with Google"
   ↓
2. Redirected to Google OAuth consent
   ↓
3. Google redirects to: https://your-neon-auth-domain/auth/callback?code=...
   ↓
4. Neon Auth exchanges code for JWT token
   ↓
5. Frontend stores JWT in localStorage
   ↓
6. All subsequent API calls include: Authorization: Bearer {jwt_token}
```

### Key Endpoints

**User Management**

```
GET    /api/kullanicilar              # List users (paginated)
GET    /api/kullanicilar/:id          # Get user profile
GET    /api/kullanicilar/search?q=... # Search users (case-insensitive)
PUT    /api/kullanicilar/:id          # Update profile
DELETE /api/kullanicilar/:id          # Deactivate account
```

**Content (Books, Movies, Games)**

```
GET    /api/icerik                    # List content
GET    /api/icerik/:id                # Get content details
GET    /api/icerik/kitaplar           # List books
GET    /api/icerik/filmler            # List movies
GET    /api/icerik/oyunlar            # List games
POST   /api/icerik                    # Add new content (admin)
PUT    /api/icerik/:id                # Update content (admin)
```

**Reviews & Comments**

```
GET    /api/yorumlar/:icerikId        # Get reviews for content
POST   /api/yorumlar                  # Create review
PUT    /api/yorumlar/:id              # Edit review
DELETE /api/yorumlar/:id              # Delete review
POST   /api/yorumlar/:id/yorum        # Reply to review (comment)
POST   /api/yorumlar/:id/begeni       # Like review
```

**Lists & Organization**

```
GET    /api/listeler                  # Get user's lists
POST   /api/listeler                  # Create new list
PUT    /api/listeler/:id              # Edit list
DELETE /api/listeler/:id              # Delete list
POST   /api/listeler/:id/ekle         # Add content to list
DELETE /api/listeler/:id/sil/:icerikId # Remove from list
```

**Statistics & Insights**

```
GET    /api/istatistikler/yillik-ozet?yil=2025  # Yearly summary
GET    /api/istatistikler/ozetim                 # User stats
GET    /api/istatistikler/etkinlik               # Activity timeline
```

**Notifications**

```
GET    /api/bildirimler                          # Get notifications
POST   /api/bildirimler/:id/goster               # Mark as read
DELETE /api/bildirimler/:id                      # Delete notification
```

**Search & Discovery**

```
GET    /api/arama?q=query             # Full-text search
GET    /api/arama/anlam?q=query       # AI semantic search
GET    /api/populer                   # Trending content
GET    /api/onerililer                # Recommendations
```

### Error Codes

```
200 OK              - Request successful
201 Created         - Resource created
204 No Content      - Successful deletion
400 Bad Request     - Invalid input
401 Unauthorized    - Missing/invalid auth
403 Forbidden       - Insufficient permissions
404 Not Found       - Resource doesn't exist
409 Conflict        - Duplicate or constraint violation
422 Unprocessable   - Validation failed
429 Too Many        - Rate limit exceeded
500 Server Error    - Unexpected error
```

---

## Project Structure

```
sagaprod/
├── saga-backend/                    # ASP.NET Core API
│   ├── Program.cs                   # App configuration, DI, middleware
│   ├── Dockerfile                   # Container image for Render
│   ├── appsettings.json             # Production configuration template
│   ├── appsettings.Development.json # Development overrides
│   ├── neon_schema_clean.sql        # Database schema
│   │
│   ├── Controllers/                 # API endpoints
│   │   ├── AuthController.cs        # OAuth, login, password reset
│   │   ├── KullaniciController.cs   # User management, search
│   │   ├── IcerikController.cs      # Books, movies, games
│   │   ├── YorumController.cs       # Reviews, comments, likes
│   │   ├── ListeController.cs       # User lists, collections
│   │   ├── AktiviteController.cs    # Activity, timeline
│   │   ├── BildirimController.cs    # Notifications
│   │   ├── AiController.cs          # AI features, semantic search
│   │   └── ...more controllers
│   │
│   ├── Models/                      # Database entities
│   │   ├── Kullanici.cs             # User with profile data
│   │   ├── Icerik.cs                # Content (book, movie, game)
│   │   ├── Yorum.cs                 # Reviews with replies
│   │   ├── Liste.cs                 # User-created lists
│   │   ├── Aktivite.cs              # User activities
│   │   ├── Bildirim.cs              # Notifications
│   │   ├── Enum.cs                  # Enum types
│   │   └── ...20+ entity models
│   │
│   ├── Data/                        # Database context
│   │   └── SagaDbContext.cs         # EF Core DbContext
│   │
│   ├── Services/                    # Business logic
│   │   ├── TmdbService.cs           # TMDB API integration
│   │   ├── GoogleBooksService.cs    # Google Books API
│   │   ├── OpenLibraryService.cs    # Open Library API
│   │   ├── GroqService.cs           # Groq AI API
│   │   ├── LocalAiService.cs        # Local semantic search
│   │   ├── SemanticSearchService.cs # Search logic
│   │   ├── BildirimService.cs       # Notification logic
│   │   └── TokenService.cs          # JWT token generation
│   │
│   ├── Middleware/                  # Request/response handling
│   │   └── ExceptionHandlingMiddleware.cs
│   │
│   ├── Migrations/                  # EF Core migrations
│   │   ├── 20251125194947_AddYorumReplies.cs
│   │   └── ...migration files
│   │
│   ├── DTOs/                        # Data transfer objects
│   │   ├── KullaniciDtos.cs         # User DTOs
│   │   ├── IcerikDtos.cs            # Content DTOs
│   │   ├── YorumDtos.cs             # Review DTOs
│   │   └── ...more DTOs
│   │
│   └── Saga.Server.csproj           # Project file with dependencies
│
├── saga-frontend/                   # React + Vite client
│   ├── index.html                   # HTML entry point
│   ├── vite.config.ts               # Vite build configuration
│   ├── tailwind.config.js           # Tailwind CSS config
│   ├── tsconfig.json                # TypeScript configuration
│   ├── capacitor.config.ts          # Capacitor for Android
│   │
│   ├── src/
│   │   ├── main.tsx                 # React root
│   │   ├── App.tsx                  # App routing
│   │   ├── index.css                # Global styles
│   │   │
│   │   ├── pages/                   # Route pages
│   │   │   ├── glass/               # Main app pages (dark theme)
│   │   │   │   ├── HomePage.tsx     # Home/discover
│   │   │   │   ├── LoginPage.tsx    # OAuth login
│   │   │   │   ├── ProfilePage.tsx  # User profile
│   │   │   │   ├── ListePage.tsx    # Content lists
│   │   │   │   ├── SearchPage.tsx   # Search interface
│   │   │   │   └── YearlySummaryPage.tsx  # Wrapped/stats
│   │   │   │
│   │   │   └── components/          # Reusable UI components
│   │   │       ├── Header.tsx
│   │   │       ├── Navigation.tsx
│   │   │       ├── ContentCard.tsx
│   │   │       ├── ReviewList.tsx
│   │   │       └── ...more components
│   │   │
│   │   ├── context/                 # React Context state management
│   │   │   ├── AuthContext.tsx      # Auth state, token, user
│   │   │   ├── ThemeContext.tsx     # Dark/light theme
│   │   │   └── NotificationContext.tsx
│   │   │
│   │   ├── services/                # API & external services
│   │   │   ├── api.ts               # Axios instance with interceptors
│   │   │   ├── neonAuth.ts          # Neon Auth wrapper
│   │   │   ├── authService.ts       # Auth logic
│   │   │   └── contentService.ts
│   │   │
│   │   ├── hooks/                   # Custom React hooks
│   │   │   ├── useApi.ts            # Query wrapper
│   │   │   ├── useAuth.ts           # Auth hook
│   │   │   └── useLocalStorage.ts
│   │   │
│   │   └── types/                   # TypeScript type definitions
│   │       ├── models.ts            # Entity types
│   │       ├── api.ts               # API response types
│   │       └── forms.ts             # Form input types
│   │
│   ├── public/                      # Static assets
│   │   ├── favicon.svg              # Custom S icon with gradient
│   │   └── ...icons, images
│   │
│   ├── android/                     # Capacitor Android project
│   │   ├── app/
│   │   │   ├── src/main/res/        # Android resources
│   │   │   │   ├── drawable/        # Custom icons
│   │   │   │   ├── mipmap-*/        # App icons (all sizes)
│   │   │   │   └── values/          # Colors, strings
│   │   │   │
│   │   │   └── src/main/AndroidManifest.xml
│   │   │
│   │   └── build.gradle             # Gradle configuration
│   │
│   ├── .env.example                 # Environment template
│   ├── package.json                 # Dependencies, scripts
│   └── vercel.json                  # Vercel deployment config
│
├── sagaprod.sln                     # Visual Studio solution file
│
├── README.md                        # This file
├── ARCHITECTURE.md                  # Detailed architecture
├── AUDIT.md                         # Security & code review
├── CONTRIBUTING.md                  # Contribution guidelines
└── LICENSE                          # MIT License
```

---

## Key Features Deep Dive

### AI-Powered Semantic Search

The search system supports both keyword and semantic matching:

```typescript
// Keyword search (exact or substring match)
GET /api/arama?q=harry+potter
// Returns: Title/author/description containing "harry" OR "potter"

// Semantic search (meaning-based)
GET /api/arama/anlam?q=magic+school+adventure
// Uses AI to find semantically similar content
// Can match: "wizarding academy", "enchanted learning", etc.
```

**How it works:**

1. User's query is converted to vector embedding (via Groq or local Ollama)
2. Embedding is compared against indexed content embeddings
3. Results ranked by similarity score
4. Top matches returned sorted by relevance

### Real-time Notifications

```typescript
// Create event that triggers notification
POST /api/yorumlar (new review)
  → Automatically creates Bildirim records for:
    - Content creator
    - Other reviewers
    - Followers of reviewer

// Fetch notifications
GET /api/bildirimler
  → Returns paginated notifications with context
  → Includes: actor, action, target, timestamp
```

### Yearly Summary (Wrapped/Recap)

```typescript
// Get annual statistics
GET /api/istatistikler/yillik-ozet?yil=2025

// Response includes:
{
  topKitaplar: [],        // Most read books
  topYazarlar: [],        // Favorite authors
  topYorum: [],           // Most-liked reviews
  topEtkinlikler: [],     // Major activities
  istatistikler: {
    toplamKitap: 42,
    toplamReview: 18,
    toplamSaat: 200,      // Reading/watching hours
    toplamBegeni: 89
  }
}
```

---

## Security & Best Practices

### Authentication

- ✅ OAuth 2.0 via Google (no password storage)
- ✅ JWT tokens with 24-hour expiration
- ✅ Token refresh mechanism (transparent to user)
- ✅ Secure cookie flags (HttpOnly, SameSite)

### Authorization

- ✅ Role-based access control (RBAC)
- ✅ Resource-level permissions (own content)
- ✅ Admin-only endpoints protected
- ✅ Prevents data leakage across users

### API Security

- ✅ CORS restricted by environment
- ✅ Rate limiting (100 reqs/min per IP)
- ✅ Input validation on all endpoints
- ✅ SQL injection protected (EF Core parameterized queries)
- ✅ CSRF protection enabled
- ✅ Request body size limited (10 MB)

### Database Security

- ✅ All credentials in environment variables
- ✅ Connection string not in source code
- ✅ Automatic backups via Neon
- ✅ Encryption at rest (Neon default)
- ✅ Row-level security considerations

### Frontend Security

- ✅ No API keys stored in code
- ✅ Tokens only in memory (short-lived)
- ✅ Sensitive data not logged
- ✅ XSS protection via React's JSX escaping
- ✅ SVG icons sanitized

### GDPR & Privacy

- ✅ User data exportable
- ✅ Account deletion cascades to content ownership
- ✅ Privacy settings per content
- ✅ Block/report system for abuse

---

## Contributing

We welcome contributions! Please read [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

### Common Tasks

**Report a Bug**

```bash
# Create issue with reproduction steps
# Include: browser, OS, steps to reproduce, actual vs expected
```

**Add a Feature**

```bash
# 1. Create feature branch
git checkout -b feature/feature-name

# 2. Make changes and test locally
npm run dev    # Frontend
dotnet run     # Backend

# 3. Commit with clear message
git commit -m "Add feature: description"

# 4. Push and create pull request
git push origin feature/feature-name
```

**Improve Documentation**

```bash
# Edit .md files and submit PR
# Review for: clarity, accuracy, examples, links
```

---

## Troubleshooting

### Frontend Issues

**"White screen after login"**

- Clear localStorage: DevTools → Application → Clear Storage
- Check browser console for JWT token errors
- Verify `VITE_API_URL` in `.env.local`
- Restart dev server: `npm run dev`

**"API returns 403 or 401"**

- Expired JWT token → Refresh page
- Missing Authorization header → Check AuthContext
- Wrong Neon Auth domain → Verify `VITE_NEON_AUTH_URL`

**"OAuth redirect not working"**

- Neon Console → Auth → trusted origins: add your frontend URL
- Google Cloud Console → OAuth 2.0 credentials: add frontend domain
- Browser DevTools → Network: check auth callback URL

**"Styled components not loading"**

- Clear Vite cache: `rm -rf node_modules/.vite`
- Rebuild CSS: `npm run build`
- Verify Tailwind config includes `src/**/*.{js,jsx,ts,tsx}`

### Backend Issues

**"Database connection timeout"**

- Check `DATABASE_CONNECTION_STRING` in appsettings
- Verify Neon database is running (check Neon Console)
- Ping database: `psql connection-string`

**"EF Core migrations failing"**

- Check for syntax errors: `dotnet ef migrations validate`
- Rollback: `dotnet ef database update PreviousMigration`
- Recreate: `rm Migrations/*; dotnet ef migrations add InitialCreate`

**"CORS errors from frontend"**

- Check `AllowedOrigins` in Program.cs
- Verify `origin` matches exactly (including protocol/port)
- Add `https://localhost` for local https development

### Deployment Issues

**"Vercel deployment fails"**

- Check build log: Vercel Dashboard → Deployments → Logs
- Verify `package.json` scripts: `npm run build` must succeed locally
- Check environment variables in Vercel Dashboard

**"Backend on Render fails to start"**

- Check Render logs: Dashboard → Services → Logs
- Verify all environment variables are set
- Check Docker build: `docker build -t saga .`

---

## License

This project is licensed under the **MIT License** - see [LICENSE](LICENSE) file for details.

```
MIT License

Copyright (c) 2025 SAGA Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, and distribute the Software...
```

---

##📞 Support & Community

- **Issues**: [GitHub Issues](https://github.com/yourusername/saga/issues)
- **Discussions**: [GitHub Discussions](https://github.com/yourusername/saga/discussions)
- **Email**: contact@saga.app
- **Twitter**: [@SagaApp](https://twitter.com/sagaapp)

---

**Made with ❤️ by the SAGA Community**

⭐ Consider starring this project if you find it useful!
