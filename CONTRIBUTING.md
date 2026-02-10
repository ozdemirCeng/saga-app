# Contributing to SAGA

Thank you for your interest in contributing to SAGA! This document provides guidelines and instructions for contributing to the project.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Help](#getting-help)
- [Reporting Bugs](#reporting-bugs)
- [Suggesting Features](#suggesting-features)
- [Pull Request Process](#pull-request-process)
- [Development Setup](#development-setup)
- [Coding Standards](#coding-standards)
- [Commit Messages](#commit-messages)
- [Testing Guidelines](#testing-guidelines)
- [Documentation](#documentation)

---

## Code of Conduct

### Our Pledge

We are committed to providing a welcoming and inspiring community for all. Please read and respect our [Code of Conduct](CODE_OF_CONDUCT.md).

**Expected Behavior:**
- Use welcoming and inclusive language
- Be respectful of differing opinions and experiences
- Accept constructive criticism gracefully
- Focus on what is best for the community
- Show empathy towards other community members

**Unacceptable Behavior:**
- Harassment or discrimination
- Trolling, insulting/derogatory comments
- Private/public attacks
- Publishing others' private information
- Other conduct considered inappropriate

---

## Getting Help

- **Questions about usage:** [GitHub Discussions](https://github.com/saga-app/saga/discussions)
- **Documentation issues:** [GitHub Issues](https://github.com/saga-app/saga/issues)
- **Live chat:** [Discord Community](https://discord.gg/saga)
- **Email:** contact@saga.app

---

## Reporting Bugs

### Before Submitting a Bug Report

1. **Search existing issues** - Check if the bug has already been reported
2. **Check documentation** - Ensure it's not a known limitation
3. **Try latest version** - Bug may already be fixed
4. **Reproduce reliably** - Try to reproduce the issue consistently

### How to Submit a Bug Report

Create an issue with the following information:

**Title:** Clear, descriptive summary of the issue

**Description:**
```markdown
## Describe the Bug
[Clear description of what the bug is]

## Steps to Reproduce
1. [First step]
2. [Second step]
3. [Expected behavior]
4. [Actual behavior]

## Environment
- OS: [Windows/macOS/Linux]
- Browser: [Chrome/Firefox/Safari] version X
- Node version: [e.g., 18.x]
- .NET version: [e.g., 8.0]

## Additional Context
[Screenshots, error logs, or other relevant information]

## Error Message
```
[Paste any error messages or stack traces]
```
```

---

## Suggesting Features

### Before Submitting a Feature Request

1. **Search existing issues** - Check if feature already suggested
2. **Check roadmap** - May be planned
3. **Consider scope** - Does it fit SAGA's goals?

### How to Submit a Feature Request

Create an issue with:

**Title:** Clear description of the feature

**Description:**
```markdown
## Feature Request
[Brief, clear description of the desired feature]

## Problem It Solves
[Use case and why this would be beneficial]

## Proposed Solution
[How you imagine the feature working]

## Alternatives Considered
[Other approaches or features that might solve the problem]

## Additional Context
[Screenshots, design mockups, or links]
```

---

## Pull Request Process

### Before Starting

1. **Fork the repository** - Create your own copy
2. **Create feature branch** - `git checkout -b feature/my-feature`
3. **Check existing PRs** - Avoid duplicate work
4. **Discuss major changes** - Open issue first for approval

### Development Workflow

```bash
# 1. Clone your fork
git clone https://github.com/YOUR_USERNAME/saga.git
cd saga

# 2. Add upstream remote
git remote add upstream https://github.com/saga-app/saga.git

# 3. Create feature branch
git checkout -b feature/short-description

# 4. Make changes (see development setup below)
# ... write code ...

# 5. Commit with clear messages (see commit guidelines)
git add .
git commit -m "Add feature: clear description"

# 6. Keep updated with main
git fetch upstream
git rebase upstream/main

# 7. Push to your fork
git push origin feature/short-description

# 8. Create Pull Request on GitHub
```

### PR Checklist

Before submitting your PR, ensure:

- [ ] Code follows project style guidelines
- [ ] Changes are well-commented, especially complex logic
- [ ] Tests added for new functionality
- [ ] Existing tests still pass
- [ ] Documentation updated (README, comments, architecture)
- [ ] No console.log() or debugging code left
- [ ] No secrets or API keys in code
- [ ] Branch is updated with latest `main`
- [ ] Commit messages are clear and descriptive
- [ ] PR description explains changes clearly

### PR Description Template

```markdown
## Description
[Brief description of changes]

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation

## Related Issue
Closes #[issue number]

## Testing
[How to test these changes]

## Screenshots
[If applicable, add screenshots]

## Environment
Frontend / Backend / Both

## Breaking Changes
[Any breaking changes? How to migrate?]

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review of own code
- [ ] Comments added for complex logic
- [ ] Tests added
- [ ] All tests pass locally
- [ ] No new warnings generated
```

### Review Process

1. **Automated Checks:** GitHub Actions runs tests and linting
2. **Code Review:** Maintainers review for:
   - Code quality
   - Security issues
   - Performance concerns
   - Documentation
3. **Changes Requested:** Address feedback and update PR
4. **Approval & Merge:** PR merged when approved

---

## Development Setup

### Frontend Development

```bash
cd saga-frontend

# Install dependencies
npm install

# Create environment file
cp .env.example .env.local

# Edit with your values
nano .env.local

# Start dev server (with HMR)
npm run dev

# Run linting
npm run lint

# Build for production
npm run build
```

**Development Server:** http://localhost:5173

### Backend Development

```bash
cd saga-backend

# Restore packages
dotnet restore

# Create .env file
cp appsettings.Development.json appsettings.Local.json

# Start dev server
dotnet run

# Run with watch mode
dotnet watch run

# View API docs
# http://localhost:5054/swagger
```

**API Base URL:** http://localhost:5054/api

### Database Setup

```bash
# Option 1: Use Neon (recommended for development)
# 1. Sign up at neon.tech
# 2. Create project
# 3. Get connection string
# 4. Set DATABASE_CONNECTION_STRING in .env

# Option 2: Local PostgreSQL
# 1. Install PostgreSQL
# 2. Create database: createdb sagadb
# 3. Set DATABASE_CONNECTION_STRING=postgresql://user:password@localhost/sagadb

# Apply migrations
cd saga-backend
dotnet ef database update
```

---

## Coding Standards

### TypeScript / React

**File Structure:**
```bash
src/
├── pages/           # Page components
├── components/      # Reusable components
├── hooks/           # Custom hooks
├── context/         # Context providers
├── services/        # API and business logic
├── types/           # Type definitions
├── utils/           # Utility functions
└── styles/          # Global styles
```

**Naming Conventions:**
```typescript
// Components: PascalCase
export const UserProfile = () => {};

// Variables/functions: camelCase
const userName = user.name;

// Constants: UPPER_SNAKE_CASE
const API_TIMEOUT = 5000;

// Interfaces/Types: PascalCase with I prefix (optional)
interface IUser {
  id: string;
  name: string;
}
```

**Code Style:**
```typescript
// ✅ Good
const users = await fetchUsers({ limit: 10 });
const filtered = users.filter(u => !u.deleted);

// ❌ Bad
const users = await fetchUsers({limit:10});
const filtered = users.filter(function(u) {return !u.deleted;});
```

**React Patterns:**
```typescript
// ✅ Good: Hooks, functional components
export const UserList = () => {
  const [users, setUsers] = useState<IUser[]>([]);

  useEffect(() => {
    loadUsers();
  }, []);

  return (
    <div>
      {users.map(user => <UserCard key={user.id} user={user} />)}
    </div>
  );
};

// ❌ Bad: Class components, complex logic in render
class UserList extends React.Component {
  render() {
    return <div>...</div>;
  }
}
```

### C# / ASP.NET Core

**File Structure:**
```bash
Controllers/          # API endpoints
Models/               # EF entities
DTOs/                 # Data transfer objects
Services/             # Business logic
Data/                 # DbContext
Middleware/           # Request/response handlers
Migrations/           # EF migrations
```

**Naming Conventions:**
```csharp
// Classes: PascalCase
public class UserService { }

// Methods: PascalCase
public async Task<User> GetUserAsync(int id) { }

// Parameters: camelCase
public void SetName(string userName) { }

// Enums: PascalCase members
public enum UserRole { Admin, Editor, User }

// Constants: UPPER_SNAKE_CASE
public const int MAX_LOGIN_ATTEMPTS = 5;
```

**Code Style:**
```csharp
// ✅ Good: Clear, readable
public async Task<IActionResult> GetUser(int id)
{
    var user = await _context.Users.FindAsync(id);
    if (user == null)
        return NotFound();
    
    return Ok(_mapper.Map<UserDto>(user));
}

// ❌ Bad: Condensed, hard to read
public async Task<IActionResult> GetUser(int id) =>
    (await _context.Users.FindAsync(id)) switch {
        null => NotFound(),
        var u => Ok(_mapper.Map<UserDto>(u))
    };
```

**Async/Await:**
```csharp
// ✅ Good: Properly async
public async Task<List<User>> GetUsers()
{
    return await _context.Users.ToListAsync();
}

// ❌ Bad: Synchronous over async
public List<User> GetUsers()
{
    return _context.Users.Result;  // Causes deadlocks
}
```

### SQL / Database

**Migration Naming:**
```bash
# Format: {timestamp}_{Description}.cs
# Example: 20250210_AddUserBio.cs
dotnet ef migrations add AddUserBio
```

**Query Style:**
```csharp
// ✅ Good: Parameterized (via LINQ)
var users = await _context.Users
    .Where(u => u.Name == searchTerm)
    .ToListAsync();

// ❌ Bad: Raw SQL (SQL injection risk)
var users = await _context.Users
    .FromSqlInterpolated(@$"SELECT * FROM Users WHERE Name = {searchTerm}")
    .ToListAsync();
```

---

## Commit Messages

Follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
type(scope): subject

body

footer
```

**Types:**
- `feat:` New feature
- `fix:` Bug fix
- `refactor:` Code restructure without behavior change
- `style:` Formatting, missing semicolons, etc.
- `test:` Adding tests
- `docs:` Documentation changes
- `ci:` CI/CD configuration
- `chore:` Dependency updates, etc.

**Examples:**

```bash
# Good commits
git commit -m "feat(auth): add password reset flow"
git commit -m "fix(search): case-insensitive user search"
git commit -m "docs(readme): update setup instructions"
git commit -m "refactor(api): extract notification logic to service"
git commit -m "test(auth): add JWT validation tests"

# Bad commits
git commit -m "updates"
git commit -m "fixed stuff"
git commit -m "WIP"
```

---

## Testing Guidelines

### Frontend Testing

```typescript
// Example test with React Testing Library
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import LoginPage from './LoginPage';

describe('LoginPage', () => {
  it('should display login form', () => {
    render(<LoginPage />);
    expect(screen.getByText('Login')).toBeInTheDocument();
  });

  it('should submit form with email and password', async () => {
    const user = userEvent.setup();
    render(<LoginPage />);
    
    await user.type(screen.getByLabelText('Email'), 'test@example.com');
    await user.click(screen.getByRole('button', { name: 'Login' }));
    
    // Assert successful login
  });
});
```

### Backend Testing

```csharp
// Example test with xUnit
public class UserServiceTests
{
    private readonly IUserService _service;
    
    public UserServiceTests()
    {
        _service = new UserService();
    }

    [Fact]
    public async Task GetUser_WithValidId_ReturnsUser()
    {
        // Arrange
        var userId = 1;
        
        // Act
        var user = await _service.GetUserAsync(userId);
        
        // Assert
        Assert.NotNull(user);
        Assert.Equal(userId, user.Id);
    }
}
```

**Testing Requirements:**
- New features should include tests
- Bug fixes should include test case for the bug
- Code coverage target: >80% for business logic
- All tests must pass before PR merge

---

## Documentation

### Code Comments

```typescript
// ✅ Good: Explains WHY, not WHAT
// User emails are case-insensitive in our system
const normalizedEmail = email.toLowerCase();

// ❌ Bad: Explains obvious code
// Convert email to lowercase
const normalizedEmail = email.toLowerCase();
```

### Function Documentation

```typescript
/**
 * Fetches user profile from API
 * @param userId - The user's ID
 * @returns Promise resolving to user profile
 * @throws Error if user not found
 */
export async function fetchUserProfile(userId: string): Promise<IUser> {
  // ...
}
```

### README Updates

When adding features, update:
- Feature list in main README
- Setup instructions if needed
- API documentation if new endpoints
- Architecture diagram if structure changes

### Architecture Documentation

Update [ARCHITECTURE.md](ARCHITECTURE.md) when:
- Adding new service/module
- Changing data flow
- Modifying database schema
- Updating deployment strategy

---

## Release Process

Only maintainers can create releases, but you can suggest:

1. **Prepare Release Candidate**
   - Bump version in package.json and .csproj
   - Update CHANGELOG.md
   - Create release notes

2. **Tag Release**
   ```bash
   git tag -a v1.0.0 -m "Release 1.0.0"
   git push origin v1.0.0
   ```

3. **Create GitHub Release**
   - Attach release notes
   - Highlight breaking changes
   - Thank contributors

---

## Support

- **Questions:** Use GitHub Discussions
- **Bug Reports:** Open GitHub Issue (see template)
- **Security:** Email security@saga.app (don't use public issues)
- **Commercial Support:** contact@saga.app

---

## Recognition

Contributors will be:
- Listed in [CONTRIBUTORS.md](CONTRIBUTORS.md)
- Thanked in release notes
- Featured on website (with permission)

---

## License

By contributing, you agree that your contributions will be licensed under the same MIT License as the project.

---

**Thank you for contributing to SAGA! 🎉**
