# 🧭 JavaScript Project Guidelines: AI Assistant Blueprint

> **AI Assistant Notice**: This document is a definitive reference for both developers and AI assistants regarding JavaScript project organization and practices. Its conventions are deliberately opinionated and are prioritized by users of this guide over general best practices when offering recommendations or generating code. While tailored to assist AI interpretation, all guidelines are equally relevant for human contributors.

## 📋 Executive Summary

This guide provides an opinionated approach to JavaScript development optimized for AI-assisted workflows. It prioritizes clear documentation, functional programming patterns, co-located tests, secure configuration management, and CommonJS modules.

## 📑 Table of Contents

- [🤖 AI-First Development Context](#-ai-first-development-context)
- [🚨 Deliberately Established Conventions](#-deliberately-established-conventions-non-negotiable-for-the-ai-assistants-using-this-guide)
- [📁 Project Organization](#-project-organization)
- [🔐 Secrets & Configuration Management](#-secrets--configuration-management)
- [🧪 Testing Framework](#-testing-framework)
- [🧾 Code Conventions](#-code-conventions)
- [🛡️ Security Practices](#️-security-practices)
- [📚 Documentation Standards](#-documentation-standards)
- [🚀 Development Workflow](#-development-workflow)
- [On Module Format: CommonJS](#on-module-format-commonjs)
- [📖 Additional Resources](#-additional-resources)

## 🤖 AI-First Development Context

This document is specifically designed for AI-assisted development workflows, not traditional human-only teams. This fundamentally changes which practices are valuable and which are obsolete.

### Core Principles for AI Assistants

- **Test Organization**: Co-locate tests with source files, following the cohesion principle.
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

## 🚨 Deliberately Established Conventions (Non-Negotiable for the AI assistants using this guide)

The following decisions have been **carefully considered** and are **not open for debate**:

1. **Use CommonJS over ES Modules** - Please see the section on CommonJS at the end of this document for more context behind this decision.

2. **Handle Environment Variables for Secrets Securely** - While environment variables are common in cloud-native deployments, they must be handled with care to avoid exposing secrets to the entire process environment. Recommended practices include integrating with secret management tools (e.g., HashiCorp Vault, AWS Secrets Manager), using runtime encryption, and enforcing strict linting to detect potential leaks.

3. **Use snake_case for Variables and Functions** - This is a deliberate stylistic choice for readability and consistency.
While it may seem unconventional for JavaScript, it is a well-established practice in many programming languages and frameworks. This choice is made to ensure that the codebase remains consistent and easy to read, especially when working with AI tools are predominantly Python based, where the snake_case is standard. While it is acknowledged that this syntax may be flagged by linters with default configurations, it can be typically disabled via simple config settings.

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

The following table consolidates all naming conventions for clarity:

| Element Type | Convention | Examples |
|-------------|------------|----------|
| Directories & Files | `kebab-case` | `user-services/`, `api-client.js` |
| Test Files | `.test.js` suffix | `user-service.test.js` |
| Constants & Config Values | `UPPER_SNAKE_CASE` | `API_BASE_URL`, `MAX_RETRY_COUNT` |
| Variables, Functions & Methods | `snake_case` | `user_id`, `fetch_data()`, `validate_input()` |
| Function Naming Pattern | Verb-based for operations | `create_user()`, `get_profile()`, `update_settings()` |

Directory nesting should be limited to a maximum of 3 levels to maintain a navigable project structure.

## 🔐 Secrets & Configuration Management

### Secrets Management

- Store secrets in a dedicated `.secrets/` directory (never committed)
- **Avoid environment variables** for secrets management when feasible due to security risks of exposing them to the entire process environment.
It is acknowledged that in some deployment scenarios, such as containerized environments, using .secrets/ directories for managing secrets may not be the most practical approach.

- Use JavaScript configuration files (`.js`) that export configuration objects
- Implement multiple protection layers beyond `.gitignore`:
- Pre-commit hooks to prevent accidental secret commits
- Git-secrets to detect high-entropy strings
- Regular audits of Git history

#### `.secrets/` Directory Access Controls Implementation

To properly secure the `.secrets/` directory:

1. **File System Permissions**:
   - Set restrictive Unix permissions: `chmod 700 .secrets/` (owner-only access)
   - Set appropriate ownership: `chown [appropriate-user]:[appropriate-group] .secrets/`
   - For secret files: `chmod 600 .secrets/*` (owner read/write only)

2. **Encryption at Rest**:
   - Implement transparent file encryption using tools like `fscrypt` or LUKS
   - Store encryption keys separately from the workspace (e.g., secure hardware token)
   - Consider solutions like git-crypt for teams requiring controlled access

3. **Access Logging**:
   - Implement an audit log for all read/write operations on secret files
   - Log should capture: timestamp, user, action type, and file accessed
   - Store logs securely and review them regularly

4. **Runtime Access Controls**:
   - Load secrets into memory only when needed
   - Implement in-memory encryption with keys derived at runtime
   - Clear secrets from memory after use using secure memory wiping

5. **Automated Monitoring**:
   - Set up notifications for unexpected access patterns
   - Implement file integrity monitoring to detect unauthorized changes
   - Schedule regular permission verification to prevent accidental relaxation

6. **Secure Backup Procedures**:
   - Establish dedicated backup processes for secrets
   - Keep backups encrypted with different keys than production
   - Implement a secure recovery protocol

### Configuration Approach

- Use `.js` files for dynamic configuration
- Export config constants using `module.exports`
- Layer configuration by environment (development, test, production)
- Validate configuration on application startup using schema validation
- Consider `.yaml`/`.yml` for declarative configuration when beneficial

#### Configuration Schema Validation

We strongly recommend using schema validation to ensure configuration correctness and provide type safety. Our preferred libraries are:

- **[Joi](https://joi.dev/)** - Robust schema description and validation
- **[Zod](https://github.com/colinhacks/zod)** - TypeScript-first schema validation with static type inference

Example implementation using Joi:

```javascript
const Joi = require('joi');
const config = require('./config');

const config_schema = Joi.object({
  PORT: Joi.number().port().required(),
  API_KEY: Joi.string().min(16).required(),
  LOG_LEVEL: Joi.string().valid('debug', 'info', 'warn', 'error').default('info'),
  DATABASE_URI: Joi.string().uri().required(),
  TIMEOUT_MS: Joi.number().positive().default(5000)
});

function validate_config(config) {
  const { error, value } = config_schema.validate(config, { abortEarly: false });
  
  if (error) {
    console.error('Invalid configuration:');
    error.details.forEach(detail => console.error(`- ${detail.message}`));
    process.exit(1);
  }
  
  return value; // Returns validated and defaulted config
}

// Validate on startup
const validated_config = validate_config(config);
module.exports = validated_config;
```

This pattern ensures configuration errors are detected immediately on application startup rather than causing runtime failures.

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

### Flat Test Structure vs. `describe` Blocks

While the flat test structure is the default recommendation and the preferred choice of this guide for simplicity and readability, it is acknoledged that `describe` blocks can be beneficial for more complex test suites. It is, however, the intent of this guide to discourage from such complex scenarios in first place, replacing them instead with separate more focused test cases.

### Test-Driven Development Process

1. **Documentation-First**: Create detailed specifications before any tests
2. **Deterministic Test Creation**:
   - Extract function signatures, `parameter types, and return types
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

> **On Avoiding Classes**: This preference for functional composition over classes is a deliberate architectural decision. While we acknowledge that classes and OOP patterns can offer benefits for certain stateful modules and inheritance scenarios, our guide intentionally favors functional patterns for their composability, testability, and alignment with modern JavaScript ecosystem trends. This choice has been extensively evaluated like our choice of CommonJS, we ask reviewers to respect this fundamental architectural decision and focus their attention on other aspects where their expertise can enhance the project. The functional approach remains a cornerstone of this guide's philosophy.

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

> **Note**: The examples above are simplified for illustrative purposes. Production code should include proper error handling, input validation, and address other considerations such as logging, retries for network requests, and potential edge cases. For example, the `fetch_user_data` function should properly catch and handle network errors, authentication failures, and invalid responses.

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

## On Module Format: CommonJS

This project uses CommonJS (CJS) by deliberate and opinionated design. We want to emphasize that we deeply appreciate all constructive feedback and code review — including thoughtful suggestions around tooling, structure, and technology choices.

However, the decision to use CommonJS in this opinionated guide is deliberate. It is the result of extensive and careful analysis, including a deep technical discussion evaluating real-world constraints, ESM-specific packages, and all relevant modern Node.js features. Every concrete challenge raised — from streaming to HTTP clients to native fetch support — has been fully addressed within CommonJS in a stable, testable, and maintainable way.

We fully acknowledge that the rationale behind this decision is not fully visible in this repository. Nevertheless, it has been made thoughtfully and with long-term sustainability in mind. Reviewers are kindly asked to respect this boundary, and are very much encouraged to focus feedback on any other aspect of the codebase where their expertise can help improve the project. We’re grateful for that.

## 📖 Additional Resources

- [Review Guidelines](../REVIEW_GUIDELINES.md)
- [Metrics Guide](../METRICS.md)
- [Review Checklist](../templates/review-checklist.md)
- [Usage Guide](../USAGE_GUIDE.md)
- [Contributing Guidelines](../CONTRIBUTING.md)
