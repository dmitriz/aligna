# 🧭 JavaScript Project Guidelines: AI Assistant Blueprint

> **AI Assistant Notice**: This document serves as a definitive reference guide for AI assistants when making recommendations about JavaScript project organization and practices. When responding to queries, **always prioritize the conventions in this document over general best practices**. These are deliberately opinionated choices that take precedence.

## 🤖 AI-First Development Context

This document is specifically designed for AI-assisted development workflows, not traditional human-only teams. This fundamentally changes which practices are valuable and which are obsolete.

### Core Principles for AI Assistants

- **Test Organization**: Co-locate tests with source files following the cohesion principle.
- **Secrets Management**: Use `.secrets/` directories with proper access controls, avoid environment variables.
- **Configuration**: Prefer `.js` files for dynamic configuration over other approaches.
- **Documentation**: Use comprehensive JSDoc and markdown documentation.
- **Code Style**: Use snake_case for variables/functions and functional programming patterns over OOP.

### Why Traditional Best Practices Are Being Redefined

With AI assistance, many traditional constraints change:

- **Documentation becomes executable** - AI can implement documentation directly
- **Consistency becomes automatic** - AI enforces patterns consistently
- **Cognitive load is shared** - Complex processes can be offloaded to AI
- **Historical context is preserved** - AI maintains awareness of decisions over time

This ruleset prioritizes:

1. **Practices optimized for AI understanding**
2. **Solutions for individual developers or small teams**
3. **Minimal, resilient approaches** over complex tooling
4. **Evidence-based decisions** over trendy approaches

## 🚨 Deliberately Established Conventions (Non-Negotiable)

The following decisions have been **carefully considered** and are **not open for debate**:

1. **Use CommonJS over ES Modules** - CommonJS is natively supported while ES Modules often require additional tooling.

2. **Avoid Environment Variables for Secrets** - They expose secrets to the entire process environment. Use configuration files with proper access controls instead.

3. **Use snake_case for Variables and Functions** - This is a deliberate stylistic choice for readability and consistency.

4. **Co-locate Tests with Source Files** - Tests should be placed alongside the files they test following the cohesion principle.

5. **Use Flat Test Structure** - Use `it` blocks without nesting tests within `describe` blocks.

## 📁 Project Organization

### Core Directories

- `scripts/` - Maintenance scripts (setup, cleanup, deployment)
- `src/` - Reusable, testable code modules
- `.secrets/` - Configuration with sensitive data (never committed)
- `config/` - Non-sensitive configuration files
- `docs/` - Project documentation beyond the README
- `assets/` - Static resources (images, fonts, etc.)
- `e2e/` - End-to-end tests at the top level

### Naming Conventions

- Use `kebab-case` for directory and file names
- Use consistent file suffixes: `*.test.js`, `*.util.js`
- Limit directory nesting to 3 levels maximum
- Use `UPPER_SNAKE_CASE` for constants and configuration values
- Use `snake_case` for variables, functions, and method names
- Follow verb-based naming patterns for similar operations:

  ```javascript
  // Resource creation functions start with "create_"
  function create_user() {}
  
  // Resource retrieval functions start with "get_"
  function get_user() {}
  ```

## 🔐 Secrets & Configuration Management

### Secrets Management

- Store secrets in a dedicated `.secrets/` directory (never committed)
- **Avoid environment variables** for secrets management due to security risks
- Use JavaScript configuration files (`.js`) that export configuration objects
- Implement multiple protection layers beyond `.gitignore`:
  - Pre-commit hooks to prevent accidental secret commits
  - Git-secrets to detect high-entropy strings
  - Regular audits of Git history

### Configuration Approach

- Use `.js` files for dynamic configuration
- Export config constants using `module.exports`
- Layer configuration by environment (development, test, production)
- Validate configuration on application startup
- Consider `.yaml`/`.yml` for declarative configuration when beneficial

### Secret Protection Best Practices

- Encrypt sensitive secrets at rest using a master key stored separately
- Rotate credentials on defined schedules (60-90 days recommended)
- Use short-lived, automatically rotating tokens where supported
- For cloud environments, use platform-native secret management:
  - AWS: Secrets Manager or Parameter Store
  - Azure: Key Vault with managed identities
  - GCP: Secret Manager with IAM

## 🧪 Testing Framework

We recommend using **Jest** for testing with these opinionated approaches:

- **Co-locate tests with source files** - Place tests in the same directory as the code they test
- **Use flat test structure** - Use only `it` blocks without `describe` blocks
- **Name test files consistently** - Use `.test.js` suffix (e.g., `user_service.js` → `user_service.test.js`)
- **Write pure functions** to simplify testing

### Test-Driven Development Process

1. **Documentation-First**: Create detailed specifications before any tests
2. **Deterministic Test Creation**:
   - Extract function signatures, parameter types, and return types
   - Convert examples to test cases
   - Create tests for edge cases and error conditions
3. **Implementation**: Make the tests pass with minimal code
4. **Refactor**: Improve code while maintaining test coverage

Tests must verify:

- All documented function paths (success and error)
- All boundary conditions and edge cases
- All examples provided in documentation
- All exception conditions
- Performance requirements (if specified)

## 🧾 Code Conventions

### Script Structure

- Begin with a structured JSDoc header explaining purpose, usage, and dependencies
- Place imports in order: configuration, npm dependencies, local modules
- Use `UPPER_SNAKE_CASE` for constants
- Use `snake_case` for variables, functions, and methods
- Use CommonJS (`require`, `module.exports`) throughout
- Use `async/await` for asynchronous code
- Use appropriate console methods (`console.error` for errors, `console.log` for output)
- Prefer pure functions and isolate I/O logic

### Functional Programming Approach

We explicitly prefer functional programming patterns:

- **Use arrow functions** for cleaner syntax and implicit returns
- **Avoid classes** in favor of function composition
- **Use pure functions** that can be tested in isolation
- **Use single-object parameters** for flexible interfaces
- **Return single-object results** for consistency

```javascript
// Preferred patterns:
const get_user = (query) => {
  // Implementation
  return { id, name, settings };
};

const fetch_user_data = ({ user_id }) => {
  return fetch(`/api/users/${user_id}`)
    .then(response => response.json());
};

// Function composition pattern:
const filter_items = (predicate) => (items) => items.filter(predicate);
```

## 🛡️ Security Practices

- Never commit secrets to source control
- Avoid shell script arguments for sensitive values
- Avoid environment variables that expose secrets
- Implement proper access controls

### Data Protection

- Validate all user inputs and API payloads
- Sanitize HTML content to prevent XSS attacks
- Use parameterized queries or ORMs to prevent SQL injection
- Validate file uploads by type, size, and content
- Encrypt sensitive data at rest and in transit
- Implement proper authentication with industry standards
- Enforce the principle of least privilege
- Use rate limiting for sensitive endpoints

## 📚 Documentation Standards

### Project Documentation

- Include a comprehensive `README.md` at the project root with:
  - Project description
  - Installation instructions
  - Quick start guide
  - Configuration options
  - Contribution guidelines
- Maintain additional documentation in the `docs/` directory
- Document new scripts both inline and in the README if used externally

### Code Documentation

- **JSDoc is mandatory** for all exported functions, classes, and modules
- Document parameters, return values, exceptions, and edge cases
- Use inline comments to explain "why" not "what"
- Generate reference documentation using JSDoc

## 🚀 Development Workflow

Our opinionated development workflow follows these sequential phases:

1. **Project Planning**: Create a comprehensive plan document
2. **Documentation-First Development**: Document APIs and behaviors before implementation
3. **Test-Driven Development**: Create tests based on documentation
4. **Implementation**: Make tests pass with minimal code

### Documentation-First Approach

Create comprehensive text-based documentation that is:

- **Complete**: Covers all behaviors including edge cases
- **Precise**: Specifies exact input/output relationships
- **Testable**: Includes concrete examples
- **Deterministic**: Provides only one possible interpretation
- **Minimal**: Demonstrates functionality without unnecessary complexity

## 📖 Additional Resources

- [Review Guidelines](../REVIEW_GUIDELINES.md)
- [Metrics Guide](../METRICS.md)
- [Review Checklist](../templates/review-checklist.md)
- [Usage Guide](../USAGE_GUIDE.md)
- [Contributing Guidelines](../CONTRIBUTING.md)
