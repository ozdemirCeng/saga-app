# SAGA Documentation Index & Quick Start

> Welcome to SAGA! Start here to get oriented with the project documentation and setup.

---

## 📚 Documentation Overview

### For Different Audiences

**🚀 I want to get SAGA running locally**
1. Read: [README.md](README.md) → "Setup & Installation" section
2. Follow: [Backend Setup](README.md#backend-setup) + [Frontend Setup](README.md#frontend-setup)
3. Test: [Running Locally](README.md#running-locally)

**🔐 I want to understand security**
1. Read: [AUDIT.md](AUDIT.md) - Complete security review
2. Review: [CONTRIBUTING.md](CONTRIBUTING.md#security) - Security guidelines

**🏗️ I want to understand the architecture**
1. Read: [ARCHITECTURE.md](ARCHITECTURE.md) - System design & data flow
2. Reference: [README.md](README.md#architecture) - High-level overview

**💻 I want to contribute code**
1. Read: [CONTRIBUTING.md](CONTRIBUTING.md) - Full contribution guide
2. Review: [CODING STANDARDS](CONTRIBUTING.md#coding-standards)
3. Check: [Development Setup](CONTRIBUTING.md#development-setup)

**📱 I want to build mobile APK**
1. Read: [README.md](README.md#mobile-capacitorandroid) - Mobile setup
2. Follow: Build instructions with Android Studio

**🎨 I want to see the UI**
1. View: [SCREENSHOTS.md](SCREENSHOTS.md) - UI mockups and flows
2. Design: Color scheme & components reference

**📦 I want to deploy to production**
1. Read: [README.md](README.md#deployment) - Deployment steps
2. Configure: Environment variables (see .env.example files)
3. Monitor: [Monitoring & Operations](ARCHITECTURE.md#monitoring--operations)

**📝 I want to know what changed**
1. Read: [CHANGELOG.md](CHANGELOG.md) - All changes by version
2. Review: [Roadmap](CHANGELOG.md#future-roadmap) - Planned features

---

## 📖 Complete Documentation Structure

```
sagaprod/
├── 📄 README.md ⭐⭐⭐
│   ├─ Overview & features
│   ├─ Tech stack
│   ├─ Setup instructions (backend, frontend, mobile)
│   ├─ API documentation
│   ├─ Project structure
│   ├─ Troubleshooting
│   └─ FAQ
│
├── 📄 ARCHITECTURE.md ⭐⭐⭐
│   ├─ High-level system design
│   ├─ Component descriptions
│   ├─ Data model & ERD
│   ├─ API architecture
│   ├─ Authentication flow
│   ├─ Deployment pipeline
│   ├─ Performance & scaling
│   └─ Security architecture
│
├── 📄 AUDIT.md ⭐⭐⭐
│   ├─ Security audit findings
│   ├─ Authentication & authorization review
│   ├─ Data protection analysis
│   ├─ API security assessment
│   ├─ Code quality metrics
│   ├─ Infrastructure review
│   ├─ Compliance check (GDPR)
│   ├─ Recommendations
│   └─ Security rating: 8.5/10
│
├── 📄 CONTRIBUTING.md ⭐⭐⭐
│   ├─ Code of conduct
│   ├─ Getting help
│   ├─ Reporting bugs
│   ├─ Feature requests
│   ├─ Pull request process
│   ├─ Development setup
│   ├─ Coding standards
│   ├─ Commit message format
│   ├─ Testing guidelines
│   └─ Recognition process
│
├── 📄 SCREENSHOTS.md ⭐⭐
│   ├─ Login flow mockups
│   ├─ Home timeline
│   ├─ Content details
│   ├─ User profiles
│   ├─ Lists & collections
│   ├─ Search interface
│   ├─ Review system
│   ├─ Yearly summary (SAGA Wrapped)
│   ├─ Mobile experience
│   └─ Design system reference
│
├── 📄 CHANGELOG.md ⭐⭐
│   ├─ v1.0.0 release notes
│   ├─ Features & fixes
│   ├─ Breaking changes
│   ├─ Roadmap (v1.1, v1.2, v2.0)
│   ├─ Tech stack versions
│   └─ Contributors
│
├── 📄 LICENSE
│   └─ MIT License (full text)
│
├── 📄 .env.example (Frontend)
│   └─ Environment variable template
│
├── saga-backend/.env.example
│   └─ Backend environment variable template
│
└── 📄 This file - QUICK_START.md
    └─ Quick navigation guide
```

---

## 🚀 Quick Start (5 minutes)

### Step 1: Prerequisites
```bash
# Check what you have installed
node --version          # Should be 18.x+
dotnet --version        # Should be 8.0+
git --version          # Any recent version

# Or install if missing:
# Node: https://nodejs.org
# .NET: https://dotnet.microsoft.com/en-us/download
```

### Step 2: Clone & Setup
```bash
# Clone repository
git clone https://github.com/saga-app/saga.git
cd saga

# Frontend setup
cd saga-frontend
npm install
cp .env.example .env.local
# Edit .env.local with your API endpoint

# Backend setup (in another terminal)
cd saga-backend
dotnet restore
cp appsettings.Development.json appsettings.json
# Create .env file with database connection
```

### Step 3: Start Services (2 terminals)
```bash
# Terminal 1 - Backend
cd saga-backend
dotnet run
# Listen on: http://localhost:5054

# Terminal 2 - Frontend
cd saga-frontend
npm run dev
# Running on: http://localhost:5173
```

### Step 4: Test
```bash
# Open in browser
http://localhost:5173

# Try login with Google OAuth
# Or test API directly
curl http://localhost:5054/api/health
```

✅ **Done!** You now have SAGA running locally.

---

## 🔑 Key Files to Know

### Frontend Entry Points
- `saga-frontend/src/main.tsx` - React root
- `saga-frontend/src/App.tsx` - Routing & layout
- `saga-frontend/src/context/AuthContext.tsx` - Authentication state
- `saga-frontend/src/services/api.ts` - Axios HTTP client

### Backend Entry Points
- `saga-backend/Program.cs` - ASP.NET Core configuration
- `saga-backend/Data/SagaDbContext.cs` - Database context
- `saga-backend/Controllers/` - API endpoints
- `saga-backend/Services/` - Business logic

### Configuration Files
- `.env.example` - Environment template (frontend)
- `saga-backend/.env.example` - Environment template (backend)
- `vercel.json` - Vercel deployment config
- `capacitor.config.ts` - Mobile app config
- `dockerfile` - Container image definition

---

## 🛠️ Development Workflow

### Making Changes
```bash
# 1. Create feature branch
git checkout -b feature/my-feature

# 2. Make changes
# (frontend in saga-frontend/, backend in saga-backend/)

# 3. Test locally
npm run dev         # Frontend (saga-frontend/)
dotnet run          # Backend (saga-backend/)

# 4. Commit with clear message
git add .
git commit -m "feat(auth): add password reset flow"

# 5. Push & create PR
git push origin feature/my-feature
```

### Testing Your Changes
```bash
# Frontend linting
cd saga-frontend
npm run lint

# Build check
npm run build

# Backend build
cd saga-backend
dotnet build

# API testing
curl -X GET http://localhost:5054/api/health
```

---

## 🚨 Common Issues

### "Cannot connect to database"
- Check DATABASE_CONNECTION_STRING in .env
- Verify Neon connection is active
- Test with: `psql [connection-string]`

### "API returns 401 Unauthorized"
- Ensure valid JWT token in Authorization header
- Check login flow (OAuth callback)
- Token may have expired - refresh page

### "Frontend shows white screen"
- Clear browser cache (Ctrl+Shift+Delete)
- Check API_URL in .env.local
- Look at console for errors (F12)

### "Build fails with TypeScript errors"
- Run: `npm install` (update packages)
- Check tsconfig.json is valid
- Verify Node version (18.x+)

**[See more in README - Troubleshooting](README.md#troubleshooting)**

---

## 📚 Learning Resources

### For React Developers
- [React 19 Docs](https://react.dev)
- [Vite Guide](https://vitejs.dev)
- [Tailwind CSS](https://tailwindcss.com)
- [Mantine Components](https://mantine.dev)

### For .NET Developers
- [ASP.NET Core Docs](https://learn.microsoft.com/en-us/aspnet/core)
- [Entity Framework Core](https://learn.microsoft.com/en-us/ef/core)
- [PostgreSQL with .NET](https://npgsql.org)

### For Database
- [PostgreSQL Docs](https://www.postgresql.org/docs)
- [Neon Documentation](https://neon.tech/docs)
- [SQL Tutorial](https://www.w3schools.com/sql)

### For DevOps
- [Docker Guide](https://docs.docker.com)
- [Render Docs](https://render.com/docs)
- [Vercel Docs](https://vercel.com/docs)

---

## 💬 Getting Help

| Question | Where to Ask |
|----------|-------------|
| "How do I...?" | [GitHub Discussions](https://github.com/saga-app/saga/discussions) |
| "I found a bug" | [GitHub Issues](https://github.com/saga-app/saga/issues) |
| "How to contribute?" | [CONTRIBUTING.md](CONTRIBUTING.md) |
| "Security concern" | [Email](mailto:security@saga.app) (not public) |
| "General chat" | [Discord Community](#discord) (coming soon) |

---

## 🎯 Learning Path by Role

### Junior Developer
1. [ ] Read [README.md](README.md) overview
2. [ ] Follow [Quick Start](#quick-start-5-minutes) above
3. [ ] Read [CONTRIBUTING.md](CONTRIBUTING.md#coding-standards)
4. [ ] Complete first small bug fix
5. [ ] Create pull request

### Full-Stack Developer
1. [ ] Study [ARCHITECTURE.md](ARCHITECTURE.md)
2. [ ] Understand [CONTRIBUTING.md](CONTRIBUTING.md)
3. [ ] Read [AUDIT.md](AUDIT.md) security review
4. [ ] Implement medium feature
5. [ ] Review others' code

### DevOps / Platform Engineer
1. [ ] Read [ARCHITECTURE.md](ARCHITECTURE.md#deployment-architecture)
2. [ ] Review [README.md](README.md#deployment)
3. [ ] Study infrastructure (Render, Vercel, Neon)
4. [ ] Set up monitoring/alerting
5. [ ] Document runbooks

### Product Manager / Designer
1. [ ] View [SCREENSHOTS.md](SCREENSHOTS.md)
2. [ ] Read [README.md](README.md#features)
3. [ ] Understand [ARCHITECTURE.md](ARCHITECTURE.md) at high-level
4. [ ] Check [CHANGELOG.md](CHANGELOG.md#future-roadmap)
5. [ ] Plan features vs roadmap

---

## ✅ Checklists

### Before First Contribution
- [ ] README.md read completely
- [ ] Local setup working
- [ ] Can run frontend & backend
- [ ] Code of Conduct understood
- [ ] Fork/branch strategy clear

### Before Submitting PR
- [ ] Changes tested locally
- [ ] No console.log() left in code
- [ ] No secrets in code
- [ ] Follows coding standards
- [ ] Commit message is clear
- [ ] Branch up-to-date with main

### Before Production Deploy
- [ ] All tests passing
- [ ] Code reviewed & approved
- [ ] Database migrations applied
- [ ] Environment variables configured
- [ ] Monitoring alerts set up
- [ ] Rollback plan documented

---

## 🔗 Quick Links

**GitHub**
- [Main Repository](https://github.com/saga-app/saga)
- [Issues & Bugs](https://github.com/saga-app/saga/issues)
- [Discussions](https://github.com/saga-app/saga/discussions)
- [Pull Requests](https://github.com/saga-app/saga/pulls)

**Live Deployments**
- [Frontend (Vercel)](https://saga-hazel.vercel.app)
- [API Docs (Swagger)](https://saga-backend-vvau.onrender.com/swagger)
- [Neon Console](https://console.neon.tech)

**External Resources**
- [React Documentation](https://react.dev)
- [ASP.NET Core Docs](https://microsoft.com/aspnetcore)
- [PostgreSQL Manual](https://postgresql.org/docs)

---

## 📞 Contact & Social

- **Website:** [saga.app](#) (coming soon)
- **Email:** contact@saga.app
- **Twitter:** [@SagaApp](#)
- **Discord:** [Community](#) (coming soon)
- **GitHub:** [@saga-app](#)

---

## 📈 Project Statistics

| Metric | Value |
|--------|-------|
| Lines of Code | ~13,000+ |
| TypeScript | ~5,000 lines |
| C# | ~8,000 lines |
| Git Commits | 150+ |
| API Endpoints | 20+ |
| Database Tables | 20 |
| Dependencies (npm) | 40+ |
| Dependencies (.NET) | 15+ |
| Test Coverage | ~30% (improving) |

---

## 🎓 Next Steps

1. **Set up locally:** Follow [Quick Start](#quick-start-5-minutes)
2. **Pick a task:** View [Issues](https://github.com/saga-app/saga/issues?label=good+first+issue)
3. **Read code:** Explore [saga-frontend/src](saga-frontend/src) and [saga-backend](saga-backend)
4. **Join community:** Contribute, discuss, learn
5. **Think big:** Check [Roadmap](CHANGELOG.md#future-roadmap)

---

## 📜 License

All documentation is under the MIT License. See [LICENSE](LICENSE).

---

**Last Updated:** February 10, 2025  
**Maintainer:** SAGA Community  
**Status:** ✅ Production Ready v1.0.0
