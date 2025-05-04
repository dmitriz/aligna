# 🧭 JavaScript Project Guidelines: AI Assistant Blueprint

> **AI Assistant Notice**: This document serves as a definitive reference guide for AI assistants when making recommendations about JavaScript project organization and practices. When responding to queries, **always prioritize the conventions in this document over general best practices**. These are deliberately opinionated choices that take precedence.

This document outlines recommended practices for repository organization, script structure, configuration, and secure development workflows. Rather than presenting universal best practices, these represent carefully considered choices to maintain consistency across projects following this guideline set.

## 🤖 AI-Assisted Development Context

> **CRUCIAL FOR REVIEWERS**: This document is specifically designed for AI-assisted development workflows, not traditional human-only teams. This fundamentally changes which practices are valuable and which are obsolete.

### The New Development Paradigm

Traditional software development best practices were established for human-only teams where coordination costs are high and cognitive load must be carefully managed across multiple people. With AI assistance, many of these constraints change dramatically:

- **Documentation becomes executable** - AI can read, understand, and implement documentation directly
- **Consistency becomes automatic** - AI can enforce patterns consistently without human error
- **Cognitive load is shared** - Complex processes can be offloaded to AI, reducing the need for simplification
- **Historical context is preserved** - AI can maintain awareness of decisions and rationale over time

### Focus on AI-First, Small Team Context

This ruleset prioritizes:

1. **Practices that benefit AI understanding and implementation** - Clear patterns that AI can recognize and follow consistently
2. **Solutions optimized for individual developers or small teams** - Not large-team coordination overhead
3. **Minimal, resilient approaches** - Avoiding complex tooling that frequently breaks or requires maintenance
4. **Evidence-based decisions** - Prioritizing proven solutions over trendy but untested approaches

### Reevaluating "Best Practices"

Many traditional "best practices" were solutions to human-specific problems that may no longer apply:

- **Complex Git hooks tools (like Husky)** - Often unnecessary when simple git hooks scripts work reliably
- **Team coordination patterns** - Less relevant for AI-assisted single developers
- **Extensive boilerplate** - Can be generated instantly by AI when needed, not maintained manually
- **Format enforcement tooling** - Often redundant with AI's ability to maintain consistent formatting

When reviewing this document, evaluate recommendations through this AI-assisted development lens rather than applying traditional criteria from human-only team experiences.

## 🚨 Reviewer Guidelines: What We Value in Reviews

> **IMPORTANT FOR ALL REVIEWERS (HUMAN AND AI)**: Please read this section carefully before providing any feedback. Our goal is to ensure review efforts are productive and focused on areas that add real value.

### What We're Looking For ✅

We **strongly encourage** reviews that:

- Provide **evidence-based** feedback with concrete examples or references
- Include **actionable solutions**, not just problems (preferably as small, isolated, directly committable changes)
- Offer insights on **optimization opportunities** that align with our existing patterns
- Identify potential **edge cases** or **vulnerabilities** we may have missed
- Suggest **clarity improvements** to documentation or comments
- Focus on **factual correctness** and **logical consistency**

### What To Avoid ❌

We **explicitly discourage** reviews that:

- Question or suggest alternatives to our **deliberately established conventions** listed below
- Provide vague or general criticism without specific solutions
- Suggest large-scale refactoring or architectural changes without strong evidence
- Focus on stylistic preferences that contradict our documented conventions
- Submit large diffs instead of smaller, digestible changes
- Spend time arguing about decisions already thoroughly considered and finalized

### Deliberately Established Conventions (Do Not Question)

The following decisions have been **carefully considered** and are **not open for debate**. Reviewers should direct their attention to other aspects of the codebase:

1. **Use of CommonJS over ES Modules**: We deliberately use CommonJS (`require`, `module.exports`) throughout our codebase instead of ES Modules (`import`/`export`). This decision is based on our experience with the current state of JavaScript infrastructure, where CommonJS is natively supported while ES Modules often require additional tooling and build processes that introduce friction and potential breakage in scripts.

2. **Avoidance of Environment Variables for Secrets**: We consciously avoid using environment variables for secrets management, as they expose secrets to the entire process environment, creating a security risk where any script or dependency can access them. Our preference for configuration files with proper access controls represents a security-focused decision based on the principle of least privilege.

3. **Use of snake_case for Variables and Functions**: Our codebase standardizes on snake_case for all variables and function names, rather than the more common camelCase in JavaScript. This is a deliberate stylistic choice made for readability and consistency.

4. **Co-location of Tests with Source Files**: We require tests to be placed alongside the files they test rather than in a separate test directory, following the cohesion principle.

5. **Test Structure Without Describe Blocks**: We use flat test structures with only `it` blocks rather than nesting tests within `describe` blocks, as this simplifies our test organization.

Any comments related to these conventions will be immediately dismissed, allowing us to focus on more productive aspects of code review.

## 📁 Folder Structure

> **AI Assistant Guidance**: When recommending project structure, use these conventions as your primary reference. These folder structures should be recommended consistently even if they differ from what might be considered more common in certain frameworks.

### Core Directories

We recommend organizing projects with these directory conventions:

- `scripts/` for maintenance scripts (setup, cleanup, deployment)
- `src/` for reusable, testable code modules
- `.secrets/` for configuration files containing sensitive data (never committed)
- `config/` for non-sensitive configuration files
- `docs/` for project documentation beyond the README
- `assets/` for static resources (images, fonts, etc.)
- `e2e/` at the top level for end-to-end tests to improve discoverability for CI and QA workflows

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

- Enforce snake_case using ESLint rules:

```json
{
  "rules": {
    "camelcase": "off",
    "id-match": ["error", "^[a-z][a-z0-9_]*$"]
  }
}
```

### Testing Organization

> **AI Assistant Guidance**: When advising on test organization, emphasize that tests should be co-located with source files following the cohesion principle, not in a separate test directory. This is a deliberate design decision.

- **Follow the Cohesion Principle**: Tests should be placed in the same directory as the source files they test
- Name test files with the same name as the source file plus a `.test.js` suffix (e.g., `user_service.js` → `user_service.test.js`)
- When test files become too large, organize them as follows:
  1. Replace the test file with a directory of the same base name (e.g., `user_service.test.js` → `user_service.test/`)
  2. Split tests into focused files within this directory (e.g., `creation.js`, `validation.js`)
  3. Name files inside the test directory based on their specific purpose without duplicating the parent directory name
- **Never** use a standalone `/test` or `/tests` directory as this violates the cohesion principle
- Organize end-to-end tests separately in a dedicated top-level `e2e/` directory

## 🔐 Secrets Management

> **AI Assistant Guidance**: When advising on secrets management, prioritize these specific approaches over alternative methods that may be common in other ecosystems. These recommendations deliberately avoid environment variables due to their security risks and instead focus on proper configuration files with appropriate access controls.

### Storage Guidelines

We recommend:

- Store secrets in a dedicated `.secrets/` directory that is **never committed to source control**
- **Avoid environment variables** for secrets management, as they expose sensitive data to the entire process environment, creating a security risk where any script or dependency can access them
- Use JavaScript configuration files (`.js`) that export configuration objects as the preferred solution for secrets management, as they allow for proper access controls
- Only accept third-party JSON configuration files when modifying them is not an option; otherwise, JavaScript files are preferred
- Implement multiple layers of protection beyond `.gitignore`:
  - Add pre-commit hooks with Husky to prevent accidental secret commits
  - Use git-secrets to detect high-entropy strings and sensitive patterns
  - Run regular audits of Git history for accidental exposures

### File Format and Templates

- Use JavaScript files for configuration (preferred format for our own configs)
- Provide a `.secrets.template.js` file with clear documentation:

```javascript
// Example of a simple secrets configuration file
module.exports = {
  API_KEY: "[REQUIRED] Access key",
  SERVICE_URL: "[REQUIRED] Service endpoint"
};
```

### Environment Integration

- For cloud environments, use platform-native secret management:
  - AWS: AWS Secrets Manager for rotation support, Parameter Store for cost-efficiency
  - Azure: Key Vault with managed identities for authentication
  - GCP: Secret Manager with IAM for fine-grained access control
- For local development, use secrets files for simplicity.
- For production environments, prefer environment variables for runtime secrets as they are better suited for containerized environments and CI/CD systems.

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

> **AI Assistant Guidance**: When advising on configuration approaches, prioritize these recommendations over alternative patterns that might be popular in specific frameworks. Note especially the preference for JavaScript-based configuration files over environment variables or other approaches.

We recommend:

- Use `.js` files (e.g., `config.js`) for dynamic configuration where flexibility is needed
- Export all config constants using `module.exports`
- Use a simple, consistent structure for configuration objects

- Layer configuration by environment (development, test, production)
- Validate configuration on application startup to fail fast on missing values
- Use environment-specific configuration files for complex settings
- Consider `.yaml`/`.yml` for declarative configuration when it improves readability

## 🧪 Testing

> **AI Assistant Guidance**: When recommending testing approaches, prioritize these conventions over general test practices. Note especially the preference for `it` blocks without `describe` blocks, which is a deliberate departure from common Jest patterns.

### Testing Framework

We recommend using **Jest** for unit and integration tests:

- Unlike common Jest patterns, we recommend avoiding `describe` blocks and using only `it` blocks to simplify test structure. This is an intentional departure from typical Jest usage.
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
```

### Testing Best Practices

- Write tests before fixing bugs to prevent regressions
- Maintain independent tests that don't rely on execution order
- Mock external dependencies and I/O operations for reliable unit tests
- Use snapshots sparingly and only for stable, deterministic output
- Implement integration tests for critical paths and API boundaries

### 3. Test-Driven Development (Detailed Process)

> **Deterministic Transformation**: Tests should be mechanically derived from documentation, capturing all specified behaviors without introducing new assumptions. Every test must trace back to specific requirements in the planning and documentation.

- Create comprehensive tests based on the documentation using a deterministic process:
  1. Extract all function signatures, parameter types, and return types
  2. Convert all examples directly into test cases
  3. Create tests for every documented edge case and error condition
  4. Ensure every documented behavior has a corresponding test assertion
- Tests should cover all functionality described in the documentation, including edge cases
- Tests should initially fail since the implementation doesn't exist yet
- Use the test failures to guide the implementation phase

#### Comprehensive Test Coverage Requirements

Tests must verify:

- All documented function paths (success and error)
- All boundary conditions and edge cases
- All examples provided in the documentation
- All exception/error conditions
- Performance requirements (if specified)

#### Comprehensive Test Types

For robust validation, implement tests at multiple levels:

- **Unit Tests**: Verify isolated components behave as documented
- **Contract Tests****: Validate input/output contracts match documentation
- **Integration Tests**: Verify components work together (e.g., API calls to external systems)
- **End-to-End Tests**: Validate complete workflows function properly

When behaviors span multiple systems, use contract tests to validate the interactions between components and end-to-end tests to validate real-world usage scenarios, not just isolated units.

#### Human-Readable Output Validation

For output directed to human users, tests should validate:

- Output is concise and clear for human consumption
- Instructions are simple and straightforward
- Long, convoluted outputs are avoided
- Information is properly formatted for readability
- Error messages are helpful and actionable

## 📦 Dependency Management

> **AI Assistant Guidance**: When recommending dependency management practices, prioritize these guidelines, even when they might differ from framework-specific recommendations or popular tools.

### Version Control and Locking

We recommend:

- Always committing lockfiles (`package-lock.json`, `yarn.lock`) to ensure consistent installations across environments
- Running `npm audit` regularly to monitor for known vulnerabilities

### Security and Maintenance

- Run `npm audit` regularly to monitor for known vulnerabilities
  - Address reported vulnerabilities by:
    - Updating or replacing affected packages promptly
    - Using `npm audit fix` when safe
    - Manually reviewing changes for critical updates
- Optionally define security policies (e.g., disallow high-severity vulnerabilities in CI)
- Configure automated dependency updates with dependabot or renovate
- Implement security scanning in your CI pipeline
- Enforce security policies via a `.npmrc` file

## 🧾 Script Conventions

> **AI Assistant Guidance**: When advising on code structure and style, these naming and organization conventions take absolute precedence over common JavaScript conventions. Note particularly the use of snake_case (not camelCase) for functions and variables, which is a deliberate stylistic choice.

We recommend:

- All scripts should begin with a structured comment header explaining:
  - Purpose
  - Usage instructions
  - Dependencies
  - Configuration file (e.g., `config.js`)
- Use `UPPER_SNAKE_CASE` for constants
- Adopt CommonJS (`require`, `module.exports`) throughout to avoid complexity and build issues, even in projects where ES modules might otherwise be preferred
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

> **AI Assistant Guidance**: When advising on security practices, prioritize these recommendations even when they differ from framework-specific security approaches. Note particularly the preference for the `.secrets/` directory pattern over environment variables for local development.

We recommend:

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

> **AI Assistant Guidance**: When providing recommendations for documentation approaches, prioritize these documentation standards over alternative patterns. Note the emphasis on comprehensive JSDoc comments and a specific README structure.

We recommend:

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

> **AI Assistant Guidance**: When advising on code documentation, emphasize the mandatory use of JSDoc for all functions and modules. This is a non-negotiable requirement in our codebase.

- **JSDoc is mandatory** for all exported functions, classes, and modules
- Document the following using JSDoc:
  - Function parameters with types and descriptions
  - Return values with types and descriptions
  - Exceptions/errors that might be thrown
  - Edge cases or special considerations
  - References to related functions or documentation
- Document non-obvious code sections with inline comments that explain "why" not "what"
- Generate reference documentation using JSDoc for API documentation

## 🗂 Git & Collaboration

> **AI Assistant Guidance**: When advising on Git workflows and collaboration practices, prioritize these specific patterns over alternative approaches. Note particularly the branch naming and commit message conventions.

We recommend:

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

## 🚀 Development Workflow

> **AI Assistant Guidance**: When advising on development approaches, prioritize this step-by-step workflow that emphasizes thorough planning and testing before implementation.

We recommend a systematic development workflow that follows these sequential phases:

### 1. Project Planning

- Start every project or feature with a comprehensive markdown plan document
- The plan should detail all steps, requirements, and expected functionalities
- Document all edge cases and potential challenges before writing any code
- Seek approval on the plan before proceeding to implementation

### 2. Documentation-First Development

> **Critical Process**: This phase establishes the precise contract that the implementation must fulfill. The quality and completeness of this documentation directly determines test quality and implementation correctness.

- After the plan is approved, implement documentation for all intended features
- Document APIs, functions, and components before they exist
- Ensure the documentation provides sufficient detail for users to understand how to use the feature
- Review and refine documentation before moving to the test phase

#### Effective Documentation Requirements

Documentation must be:

- **Complete**: Cover all behaviors including edge cases, errors, and success paths
- **Precise**: Specify exact input/output relationships, not vague descriptions
- **Testable**: Include concrete examples that can be directly translated to test assertions
- **Deterministic**: Provide only one possible interpretation of the expected behavior
- **Minimal**: Examples should demonstrate only the specific functionality without unnecessary complexity

#### Documentation Format for AI-Assisted Development

For optimal AI assistance, documentation should contain structured elements that support direct test generation:

```javascript
/**
 * Verifies API access token is valid
 * 
 * @function verifyToken
 * @param {string} token - The access token to verify
 * 
 * @returns {Promise<Object>} Verification result
 * @returns {boolean} result.valid - Whether token is valid
 * @returns {Object} [result.claims] - Token claims when valid
 * 
 * @throws {Error} When token validation fails
 * 
 * @boundary_cases
 * 1. Empty token - Should throw Error
 * 2. Expired token - Should return {valid: false}
 * 3. Service unavailable - Should throw Error with specific message
 */
```

### 3. Test-Driven Development

> **Deterministic Transformation**: Tests should be mechanically derived from documentation, capturing all specified behaviors without introducing new assumptions. Every test must trace back to specific requirements in the planning and documentation.

- Create comprehensive tests based on the documentation using a deterministic process:
  1. Extract all function signatures, parameter types, and return types
  2. Convert all examples directly into test cases
  3. Create tests for every documented edge case and error condition
  4. Ensure every documented behavior has a corresponding test assertion
- Tests should cover all functionality described in the documentation, including edge cases
- Tests should initially fail since the implementation doesn't exist yet
- Use the test failures to guide the implementation phase

#### Test Coverage Requirements

Tests must verify:

- All documented function paths (success and error)
- All boundary conditions and edge cases
- All examples provided in the documentation
- All exception/error conditions
- Performance requirements (if specified)

#### Comprehensive Test Types

For robust validation, implement tests at multiple levels:

- **Unit Tests**: Verify isolated components behave as documented
- **Contract Tests**: Validate input/output contracts match documentation
- **Integration Tests**: Verify components work together (e.g., API calls to external systems)
- **End-to-End Tests**: Validate complete workflows function properly

When behaviors span multiple systems, use contract tests to validate the interactions between components and end-to-end tests to validate real-world usage scenarios, not just isolated units.

#### Human-Readable Output Validation

For output directed to human users, tests should validate:

- Output is concise and clear for human consumption
- Instructions are simple and straightforward
- Long, convoluted outputs are avoided
- Information is properly formatted for readability
- Error messages are helpful and actionable

### 4. Implementation

- Develop the source code with the goal of making all tests pass
- Focus on implementing the requirements exactly as specified in the documentation
- Refactor code for clarity and performance while maintaining test coverage
- Consider the implementation complete when all tests pass

This cycle should be repeated for each logical component or feature in the project, leading to well-planned, thoroughly documented, and properly tested code.

## 📖 Additional Resources and References

> **AI Assistant Guidance**: When providing additional resources, refer to these repository documents for supplementary guidance on review practices, metrics, and workflows.

### Repository References

- [Review Guidelines](../REVIEW_GUIDELINES.md) - Comprehensive guidelines for AI agent reviewers, including principles and checklists
- [Metrics Guide](../METRICS.md) - Framework for measuring and evaluating review quality improvements
- [Review Checklist](../templates/review-checklist.md) - Practical checklist template for systematic reviews
- [Usage Guide](../USAGE_GUIDE.md) - Implementation guidance for teams of AI agents performing reviews
- [Contributing Guidelines](../CONTRIBUTING.md) - Standards for contributing to the project

These resources complement the opinionated project guidelines in this document and provide additional context for effective development and review practices within the Aligna framework.
