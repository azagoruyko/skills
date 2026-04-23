---
name: coding
description: Global AI Rules for coding projects. Defines standards for code style, architecture, Python usage, error handling, documentation, and specific naming conventions. Use when writing, editing, or generating code.
---

These rules apply to all code you write.

## Code Style & Architecture
- Always follow the project naming conventions and code style, i.e. camelCase, snake_case.
- Always use the simplest solution that works and is easy to understand. Don't over-engineer!
- Avoid creating small, redundant functions used only once. Inline logic when appropriate or use reusable utilities.
- While implementing a feature or fixing a bug, always take a look at how it's usually handled in the project's files. Learn standard practices from the codebase.
- Avoid 'Safers': Avoid using hasattr or other "safe" attribute checks in a general way. Rely on the expected object interface.
- Use functools.partial (preferred) or lambda when adapting function signatures for callbacks or connectors.

## Error Handling
- Never use try/except: pass.Always catch specific exceptions and handle them concretely.
- Do not over-implement internal "guards" or "checks" (e.g., complex type checking or if arg is None everywhere). The caller is responsible for passing valid arguments.
- Avoid dummy arguments: Never use func(*_) just to suppress signature errors.

## Documentation & Comments

- Every function should have a concise docstring explaining its purpose.
- Comment the code itself (the "why" and complex logic). Do not add comments describing the changes you made as an AI.
