# 🧭 Project Guidelines

This document outlines best practices for repository organization, script structure, configuration, and secure development workflows.

## 📁 Folder Structure

- Use `scripts/` for all maintenance scripts (e.g., setup, cleanup).
- Use `src/` for reusable, testable code modules.
- Use `.secrets/` for configuration files containing sensitive data.

## 🔐 Secrets Management

- Secrets should be placed in the `.secrets/` directory.
- The directory must be included in `.gitignore`.
- Use JSON format for secrets files (e.g., `.secrets/github.json`).
- Provide a `.secrets.template.json` file with dummy values in the repository.
- Prefer secrets files over environment variables to reduce runtime complexity.

## ⚙️ Configuration

- Use `.js` files (e.g., `config.js`) for dynamic configuration where flexibility is needed.
- Consider `.yaml`/`.yml` for declarative configuration when preferred by tools or community.
- Export all config constants using `module.exports`.

## 🧪 Testing

- Use **Jest** for unit and integration tests.
- Keep tests next to the code they test (e.g., `src/__tests__/`).
- Avoid `describe` and use only `it` blocks to simplify tests structure.
- Write pure functions whenever possible to simplify testing.

## 📦 Dependency Management

- Commit lockfiles (`package-lock.json`, `yarn.lock`) to ensure consistent installs across environments.
- Run `npm audit` regularly to monitor for known vulnerabilities.
- Address reported vulnerabilities by:
  - Updating or replacing affected packages promptly.
  - Using `npm audit fix` when safe.
  - Manually reviewing changes for critical updates.
- Optionally define security policies (e.g., disallow high-severity vulnerabilities in CI).

## 🧾 Script Conventions

- All scripts should begin with a structured comment header explaining:
  - Purpose
  - Usage instructions
  - Dependencies
  - Configuration file (e.g., `config.js`)
- Use `UPPER_SNAKE_CASE` for constants.
- Adopt CommonJS (`require`, `module.exports`) throughout to avoid complexity and build issues.
- Use `async/await` for asynchronous code to improve readability.
- Use `console.error` for error messages and `console.log` for general output.
- Avoid using `console.log` for debugging; use a proper logger if needed.
- Prefer pure functions and isolate I/O logic.

## 🛡️ Security Practices

- Never commit actual secrets. Use `.secrets.template.json` with placeholders instead.
- Avoid shell script arguments unless strictly necessary—prefer configuration files.
- Avoid unnecessary environment variables to prevent leakage via process environments.
- Consider wrapping scripts with permission checks (e.g., read/write assertions).

## 📚 Documentation

- Include a `README.md` at the project root with setup and usage instructions.
- Document new scripts inline and in the README if used externally.
- Link to GitHub issues or external references when applicable.

## 🗂 Git & Collaboration

- Use feature branches with a `feature/` or `fix/` prefix.
- Follow commit message conventions (e.g., `feat:`, `fix:`, `docs:`).
- Keep PRs focused and small.
- Always review `.gitignore` for sensitive paths.

## 🚫 .gitignore Guidelines

- Include:
  - `.secrets/`
  - `secrets/`
  - `node_modules/`
  - `logs/`, `*.log`
  - `.env`, `.DS_Store`
  - `dist/` or `build/`
  - Lockfiles if intentionally excluded
