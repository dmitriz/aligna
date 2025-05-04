# 🧭 Project Guidelines Template

> **Note**: This is an illustrative example document showing how project guidelines might be structured. It is provided for reference purposes only and is not intended to prescribe universal best practices.

This document illustrates common approaches to repository organization, project structure, configuration, and development workflows.

## 📁 Folder Structure

- Use `scripts/` for automation scripts (setup, cleanup, deployment).

- Place reusable, testable code modules in a source directory (e.g., `src/`, `lib/`).

- Store configuration files containing sensitive data in a secure, non-committed directory (e.g., `.secrets/`) and ensure it is added to `.gitignore`.

- Add a `docs/` directory for architectural diagrams, API references, and guides.

## 🔒 Secrets Management

- Store secrets in a dedicated directory that is **never committed to source control**.

- Include this directory in your version control ignore files.

- Provide template files with dummy values as a reference for required fields.

- Prefer secrets files for local development to reduce complexity.

- For production environments, use environment variables for runtime secrets.

- See also: [Detailed Secrets Management Guide](../opinionated/rules-js.md#-secrets--configuration-management) for comprehensive workflow and implementation details.

- WARNING: Never commit secrets to version control history. Consider using a dedicated secrets manager service for production environments.

## ⚙️ Configuration

- Use appropriate configuration files based on your language ecosystem.

- Consider structured formats like YAML or JSON for declarative configuration.

- Layer configuration by environment (development, test, production).

- Implement schema validation (e.g., with libraries like Joi/Zod) to catch configuration errors early in the application lifecycle.

## 🧪 Testing

- Use appropriate testing frameworks for your project.

- Keep tests organized either alongside implementation or in a dedicated test directory.

- Write pure functions whenever possible to simplify testing.

## 📦 Dependency Management

- Commit lockfiles to ensure consistent installations across environments.

- Regularly audit dependencies for security vulnerabilities.

- Address reported vulnerabilities promptly.

- Consider implementing security scanning in your CI pipeline.

## 🧾 Code Conventions

- All code modules should include appropriate documentation:
  - Purpose
  - Usage instructions
  - Dependencies

- Use consistent naming conventions for constants, variables, and functions.

- Prefer pure functions and isolate I/O logic where possible.

## 🛡️ Security Practices

- Never commit actual secrets. Use template files with placeholders instead.

- Avoid command-line arguments for sensitive values—prefer configuration files.

- Avoid unnecessary environment variables to prevent leakage. However, in scenarios where environment variables are necessary (e.g., containerized deployments or CI/CD pipelines), ensure they are securely managed and accessed only by authorized processes.

- Avoid environment variables for sensitive data except when truly necessary. Environment variables should be considered "necessary" only when:
  - Required by third-party services/platforms that don't support other methods
  - Needed in containerized/serverless environments where filesystem access is limited
  - Mandated by organizational policies or deployment platforms
  In all other scenarios, prefer more secure alternatives like dedicated secret managers.

- Consider implementing appropriate permission checks.

## 📚 Documentation

- Include a `README.md` at the project root with setup and usage instructions.

- Document important components and functionalities.

- Link to external references when applicable.

## 🗂 Version Control & Collaboration

- Use feature branches with a clear naming convention.

- Follow consistent commit message conventions (e.g., [Conventional Commits](https://www.conventionalcommits.org/) or as specified in your project's [CONTRIBUTING.md](../CONTRIBUTING.md)).

- Keep pull requests focused and small.

- Always review ignore files for sensitive paths.

## 🚫 Version Control Ignore Guidelines

Include appropriate patterns for:

- Secret directories

- Dependency directories

- Log files

- Environment files

- Build artifacts and caches

- Editor-specific files
