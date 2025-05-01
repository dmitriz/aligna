# 🧩 Aligna Guidelines

These conventions align practices across repositories for consistent automation, scripting, and project structure. They serve as a foundation for contributors and maintainers.

---

## 📁 Folder & Project Structure

- Keep root-level files minimal: `README.md`, `rules.md`, `.gitignore`, `.secrets/`, and `/scripts`.
- Maintenance scripts live in `/scripts`.
- Secrets should be placed in the `.secrets/` directory.
- The directory must be excluded via `.gitignore`.

---

## 🌿 Git Workflow

- Use conventional branch prefixes: `feature/`, `fix/`, `refactor/`, `chore/`.
- Commit messages should follow [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/):  
  Example: `feat(labels): add color-coded status labels`

---

## ⚙️ Configuration Format

- Prefer `.js` files over `.json` for config, for comments and flexibility.
- Optionally support `.yaml` or `.yml` if required by tooling or CI/CD pipelines.

---

## 📦 Dependency Management

- Commit lockfiles (`package-lock.json`, `yarn.lock`) to ensure consistent installs.
- Run `npm audit` regularly to monitor vulnerabilities.

---

## 🧪 Testing & Coding Conventions

- Use **Jest** for unit and integration testing.
- All scripts should begin with a structured comment header explaining:
  - Purpose and usage
  - Expected input/output
  - Any required configuration

- Use `UPPER_SNAKE_CASE` for constants.
- Adopt CommonJS (`require`, `module.exports`) throughout.
- Maintain concise, reusable functions with clear names.
- Prefer single-responsibility functions.

---

## 🧹 .gitignore Example

```plaintext
# Secrets
.secrets/

# Logs
logs/
*.log

# IDE and system files
.idea/
.DS_Store

# Node modules
node_modules/

# Build artifacts
dist/

# Environment config
.env
.env.*
```
