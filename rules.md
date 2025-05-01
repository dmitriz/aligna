# Aligna Project Guidelines

## 🧾 Git Structure & Workflow

- All configuration files live under a `config/` folder when relevant.
- Sensitive data like tokens should be stored in `.secrets/` and **must be** gitignored.
- Scripts for automation or maintenance go in the `scripts/` folder — not reusable modules.
- All constants should be declared explicitly at the top of each script, using `UPPER_SNAKE_CASE`.

## 🔐 Secrets Management

- Secrets should be placed in `.secrets/` directory.
- The directory must be included in `.gitignore`.
- Use JSON format for secrets files (e.g., `.secrets/github.json`).
- Prefer secrets files over environment variables to reduce runtime complexity.
  
## ⚙️ Config Files

- Prefer `.js` config files over `.json` for flexibility (e.g., comments, dynamic values).
- Name clearly by project or purpose, e.g. `health.config.js`.

## 🔧 Script Conventions

- Scripts should be self-contained and use **CommonJS** (`require`, `module.exports`).
- Use **Jest** for testing scripts.
- Main function should be minimal — delegate to smaller pure helper functions.
- Avoid use of shell arguments — prefer constants or config imports.

## 📦 Package & Dependencies

- Avoid unnecessary dependencies.
- Native modules (`fs`, `path`, `https`) are preferred unless a clear benefit exists.
- If used, list all dependencies and their purpose in README.

## 🧪 Testing & Quality

- Scripts should be testable — separate IO from logic.
- Use `jest` for unit tests.
- Validate input where practical (especially external API calls).
  
## 📁 File Naming & Structure

- Use clear, lowercase hyphen-separated file names: `create-labels.js`, `health.config.js`.
- All scripts should begin with structured comment header explaining:
  - Purpose
  - Required inputs
  - Output
  - Example usage

## 💬 Coding Style

- Use `snake_case` for functions.
- Use `UPPER_SNAKE_CASE` for constants.
- Use CommonJS (`require`, `module.exports`) throughout.
- Prefer explicit over clever.
- Wrap long lines to improve readability.

## 📄 .gitignore Guidelines

- Ensure `.secrets/` and all sensitive or machine-generated files are included.
- Example:

```gitignore
# Secrets
.secrets/

# Logs
*.log

# IDE
.vscode/
.idea/

# Node
node_modules/
