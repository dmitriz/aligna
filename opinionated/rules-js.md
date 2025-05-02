# 🧭 Project Guidelines

This document outlines best practices for repository organization, script structure, configuration, and secure development workflows.

## 📁 Folder Structure

### Core Directories

- Use `scripts/` for maintenance scripts (setup, cleanup, deployment)
- Use `src/` for reusable, testable code modules
- Use `.secrets/` for configuration files containing sensitive data (never committed)
- Use `config/` for non-sensitive configuration files
- Use `docs/` for project documentation beyond the README
- Use `assets/` for static resources (images, fonts, etc.)

### Project Structure Options

- **Standard Project**: Use the core directories for single-project repositories
- **Monorepo**: For multi-project repositories, organize as follows:

```bash
monorepo/
├── packages/       # Shared libraries and modules
│   ├── package-a/
│   │   ├── src/
│   │   ├── tests/
│   │   └── package.json
│   └── package-b/
├── apps/           # Application projects
│   ├── web/
│   └── api/
└── package.json
```

### Naming Conventions

- Use `kebab-case` for directory and file names (e.g., `api-utils/`, `data-model.js`)
- Use consistent file suffixes to improve navigation:
  - Tests: `*.test.js`, `*.spec.js`
  - Types: `*.types.ts`, `*.d.ts`
  - Utilities: `*.util.js` (singular form, not plural)
- Limit directory nesting to 3 levels maximum to maintain code discoverability

### Naming and Style Conventions

> **Important Note**: This opinionated rule set, located in the `opinionated` folder, **explicitly requires snake_case** for all variable and function names. This is a deliberate and non-negotiable decision made to improve readability and consistency within this rule set. Strong language is used here because this aspect has been consistently marked by reviewers, and we want to avoid such comments at all costs for this particular opinionated folder. Reviewers are requested to refrain from commenting on this aspect and instead focus on other areas of the code.

- Use `UPPER_SNAKE_CASE` for constants and configuration values
- Use `snake_case` for variables, functions, and method names
- Adopt consistent naming patterns for similar operations:

```javascript
// Functions that create resources start with "create"
function create_user() {}
function create_database() {}

// Functions that retrieve resources start with "get"
function get_user() {}
function get_database() {}
```

### Testing Organization

- Co-locate tests with implementation files
- Use Jest's naming conventions consistently (`.test.js` or `.spec.js`)
- Organize end-to-end tests separately in a dedicated top-level `e2e/` directory

## 🔐 Secrets Management

### Storage Guidelines

- Store secrets in a dedicated `.secrets/` directory that is **never committed to source control**
- Implement multiple layers of protection beyond `.gitignore`:
  - Add pre-commit hooks with Husky to prevent accidental secret commits
  - Use git-secrets to detect high-entropy strings and sensitive patterns
  - Run regular audits of Git history for accidental exposures

### File Format and Templates

- Use JSON format for secrets files (e.g., `.secrets/github.json`)
- Provide a `.secrets.template.json` file that includes:

```json
{
  "apiKey": "[REQUIRED] Your API key from the developer portal",
  "clientSecret": "[REQUIRED] 64-character client secret",
  "webhook": {
    "token": "[OPTIONAL] Webhook verification token",
    "url": "[REQUIRED] https://your-webhook-endpoint.com"
  }
}
```

### Secret Access Patterns

- Create a dedicated secrets utility module with type-safe access methods

### Environment Integration

- For cloud environments, use platform-native secret management:
  - AWS: AWS Secrets Manager for rotation support, Parameter Store for cost-efficiency
  - Azure: Key Vault with managed identities for authentication
  - GCP: Secret Manager with IAM for fine-grained access control

### Secret Protection Best Practices

- Encrypt sensitive secrets at rest using a master key stored separately
- Rotate all credentials on defined schedules (60-90 days recommended)
- Use short-lived, automatically rotating tokens where supported (OIDC, JWT with short expiry)
- Document credential renewal procedures in your operations runbook

### Third-Party Secret Management

- For enhanced security, consider adopting:
  - HashiCorp Vault for enterprise-grade secret management with extensive API
  - Mozilla SOPS for Git-compatible encrypted secrets management
  - Doppler or Infisical for team-focused secrets management with access controls
  - dotenv-vault for simpler projects with encryption support

## ⚙️ Configuration

- Use `.js` files (e.g., `config.js`) for dynamic configuration where flexibility is needed
- Export all config constants using `module.exports`
- Use a simple, consistent structure for configuration objects

- Layer configuration by environment (development, test, production)
- Validate configuration on application startup to fail fast on missing values
- Use environment-specific configuration files for complex settings
- Consider `.yaml`/`.yml` for declarative configuration when it improves readability

## 🧪 Testing

### Testing Framework

- Use **Jest** for unit and integration tests
- Ensure Jest is included in `devDependencies` in your `package.json`:

```json
"devDependencies": {
  "jest": "^29.0.0"
}
```

- Avoid `describe` and use only `it` blocks to simplify test structure
- Write pure functions whenever possible to simplify testing

### Test Structure

- Organize tests to match the structure of the business logic
- Use clear and descriptive test names to improve readability
- Co-locate unit tests with implementation files for better discoverability
- Place integration and end-to-end tests in dedicated directories (e.g., `e2e/`)

- Organize tests with a clear pattern that matches business logic:

```javascript
// user_service.test.js
const user_service = require('./user_service');

it('creates a user with valid data', async () => {
  // Setup, action, assertions
});

it('rejects creation with invalid email', async () => {
  // Setup, action, assertions
});
```

### Testing Best Practices

- Write tests before fixing bugs to prevent regressions
- Maintain independent tests that don't rely on execution order
- Mock external dependencies and I/O operations for reliable unit tests
- Use snapshots sparingly and only for stable, deterministic output
- Implement integration tests for critical paths and API boundaries

## 📦 Dependency Management

### Version Control and Locking

- Always commit lockfiles (`package-lock.json`, `yarn.lock`) to ensure consistent installs across environments
- Run `npm audit` regularly to monitor for known vulnerabilities

### Security and Maintenance

- Run `npm audit` regularly to monitor for known vulnerabilities
- Address reported vulnerabilities by:
  - Updating or replacing affected packages promptly
  - Using `npm audit fix` when safe
  - Manually reviewing changes for critical updates
- Optionally define security policies (e.g., disallow high-severity vulnerabilities in CI)
- Configure automated dependency updates with dependabot or renovate
- Implement security scanning in your CI pipeline:

```yaml
# In .github/workflows/security.yml
jobs:
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
      - run: npm audit --audit-level=moderate
```

- Enforce security policies via a `.npmrc` file

### Dependency Selection Criteria

- Evaluate all third-party dependencies against these criteria:
  1. **Activity**: Regular releases and maintenance
  2. **Adoption**: Healthy download statistics and community usage
  3. **Minimalism**: Prefer smaller packages with fewer sub-dependencies
  4. **Security**: History of prompt security fixes
  5. **Documentation**: Comprehensive and up-to-date documentation
- Consider pinning transitive dependencies using package manager features

## 🧾 Script Conventions

- All scripts should begin with a structured comment header explaining:
  - Purpose
  - Usage instructions
  - Dependencies
  - Configuration file (e.g., `config.js`)
- Use `UPPER_SNAKE_CASE` for constants
- Adopt CommonJS (`require`, `module.exports`) throughout to avoid complexity and build issues
- Use `async/await` for asynchronous code to improve readability
- Use `console.error` for error messages and `console.log` for general output
- Avoid using `console.log` for debugging; use a proper logger if needed
- Prefer pure functions and isolate I/O logic

### File Structure and Documentation

- Begin each script with a comprehensive JSDoc header:

```javascript
/**
 * @fileoverview Database migration script to update user schemas
 * 
 * @usage node scripts/migrate-users.js
 * @requires ./config/database.js
 * @requires ./src/models/user.js
 */
```

- Place configuration imports at the top, followed by npm dependencies, then local imports

## 🛡️ Security Practices

- Never commit actual secrets. Use `.secrets.template.json` with placeholders instead
- Avoid shell script arguments unless strictly necessary—prefer configuration files
- Avoid unnecessary environment variables to prevent leakage via process environments
- Consider wrapping scripts with permission checks (e.g., read/write assertions)

### Input Validation and Sanitization

- Implement strict validation for all user inputs and API payloads
- Sanitize HTML content to prevent XSS attacks
- Use parameterized queries or ORMs to prevent SQL injection
- Validate file uploads by type, size, and content

### Authentication and Authorization

- Implement proper authentication with industry standards:
  - Use OAuth 2.0 or OpenID Connect for third-party authentication
  - Store only password hashes using bcrypt with appropriate work factors
  - Generate strong session tokens with sufficient entropy
- Enforce the principle of least privilege for all operations
- Implement role-based access control (RBAC) for complex applications

### Data Protection

- Never commit secrets to source code repositories
- Encrypt sensitive data at rest with strong algorithms
- Use HTTPS for all data in transit
- Implement rate limiting for authentication endpoints
- Consider container security for deployed applications

## 📚 Documentation

- Include a `README.md` at the project root with setup and usage instructions
- Document new scripts inline and in the README if used externally
- Link to GitHub issues or external references when applicable

### Project Documentation

- Create a comprehensive `README.md` at the project root that includes:
  - Project description and purpose
  - Installation instructions with prerequisites
  - Quick start guide with examples
  - Configuration options
  - Contributors section and contribution guidelines
  - License information
- Maintain additional documentation in the `docs/` directory organized by topic

### Code Documentation

- Use JSDoc comments for all exported functions, classes, and interfaces:

```javascript
/**
 * Processes a payment transaction
 * 
 * @param {Object} payment - Payment information
 * @param {string} payment.id - Unique payment identifier
 * @param {number} payment.amount - Payment amount in cents
 * @param {string} payment.currency - Three-letter currency code
 * @returns {Promise<Object>} Transaction result
 * @throws {ValidationError} If payment data is invalid
 * @throws {ProcessingError} If payment processing fails
 */
async function process_payment(payment) {
  // Implementation
}
```

- Generate API documentation using tools like JSDoc or TypeDoc
- Document non-obvious code sections with inline comments that explain "why" not "what"
- Include examples for complex functions or usage patterns

## 🗂 Git & Collaboration

- Use feature branches with a `feature/` or `fix/` prefix
- Follow commit message conventions (e.g., `feat:`, `fix:`, `docs:`)
- Keep PRs focused and small
- Always review `.gitignore` for sensitive paths

## 🚫 .gitignore Guidelines

- Include:
  - `.secrets/`
  - `secrets/`
  - `node_modules/`
  - `logs/`, `*.log`
  - `.env`, `.DS_Store`
  - `dist/` or `build/`
  - Lockfiles if intentionally excluded
