---
name: llm-codebase-context
description: Inspect a software repository and create or improve a compact, accurate context document that helps coding agents understand the project, stack, architecture, modules, routes, data models, conventions, workflows, and commands without reading every file. Use when creating AGENTS.md, CLAUDE.md, GEMINI.md, or another LLM-facing codebase guide.
---

# LLM Codebase Context Builder

## Mission

Create a high-quality LLM context file. It is not merely a README: it is a compressed knowledge base that gives a coding agent enough verified context to navigate a codebase without reading every file.

The document should answer six questions:

1. What is this project?
2. What technologies are used?
3. How is the project organized?
4. What does each part do?
5. What conventions should be followed?
6. How is the project installed, run, tested, built, and deployed?

Describe the actual repository. Do not invent architecture, routes, dependencies, conventions, or commands to fill gaps. If something cannot be established, mark it as unknown or not found.

## Workflow

### 1. Inspect before writing

- Read existing agent instructions and project documentation first, including any existing `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, and README files.
- Inspect package manifests, lockfiles, build configuration, environment examples, CI configuration, and test configuration.
- Inspect the source tree before documenting it.
- Follow imports and references into important modules to establish real responsibilities and relationships.
- Prefer evidence from implementation and executable configuration over assumptions based on folder names.
- Do not dump the entire repository into the context file. Select the information that most reduces an agent's need to rediscover the codebase.
- Do not expose secrets. Document environment variable names and purposes, never their secret values.

### 2. Write from global to local

Organize the context from project-wide identity and technology into architecture, modules, and actionable development guidance.

Include only sections that apply to this repository. Frontend-specific design sections are optional for backend-only projects; backend-specific API and data sections are optional for frontend-only projects.

## Recommended document structure

### 1. Project identity

Document:
- Project name
- One-sentence summary
- Purpose and intended users
- Primary goals
- Major features

### 2. Technology stack

Group technologies by role and explain what each does in this project, rather than merely listing dependencies.

Consider:
- Framework and language
- Runtime
- Package manager
- Database and data-access layer
- Authentication
- State management
- Networking
- UI framework
- Testing
- Build, deployment, and tooling

For example, do not merely write `zustand` and `axios`. Explain which state the project stores with Zustand and what Axios communicates with.

### 3. Design system (frontend only)

When relevant, document:
- Theme and visual intent
- Color palette and the purpose of each color
- Spacing and layout conventions
- Typography
- Border radius
- Animation philosophy
- Icon conventions
- Accessibility principles

Record actual values and conventions only when verified in source files. Explain intent as well as values where evidence supports it.

### 4. Architecture overview

Explain the system conceptually and how the main layers communicate.

Possible backend layers:
- API
- Controllers or route handlers
- Services
- Repositories or data-access modules
- Database
- External services

Possible frontend layers:
- App router or pages
- Components
- Hooks
- Services
- State management
- Utilities
- Assets

Use the architecture that actually exists; do not impose a template that conflicts with the code.

### 5. Folder structure

Include a concise tree of meaningful directories and files. Omit generated files, dependencies, and unimportant detail. The tree should help an agent quickly decide where new code belongs.

### 6. Module documentation

Organize modules by feature or responsibility rather than alphabetically. For each important module or folder, document:
- Purpose
- Responsibilities
- Key files
- Relationships and dependencies
- Important usage notes or examples

Describe responsibilities, not every implementation detail.

### 7. Route documentation

For web applications and APIs, document routes that can be verified from source:
- Path and HTTP method, where applicable
- Purpose
- Main components or handlers
- Authentication requirements, if known
- Important navigation or downstream calls

Do not infer routes from filenames alone. For large route sets, summarize by feature and link to the source files.

### 8. Component catalog

For frontend projects, group components by feature. For each important component, document:
- Purpose
- Inputs or props
- Outputs or events
- Dependencies
- State ownership
- Relevant constraints

Avoid a long, low-value alphabetical inventory.

### 9. Data models

Document important entities, fields, relationships, validation rules, and where each model is used. Distinguish schema facts from inferred business meaning.

### 10. API documentation

For backend or full-stack projects, document important endpoints and integrations. Where verifiable, include:
- Method and path
- Purpose
- Request shape
- Response shape
- Errors
- Authentication requirements

Do not invent request or response schemas when they are not established in code.

### 11. State management

Explain where state lives and how it flows:
- Local component state
- Global client state
- Server/cache state
- Persistent storage
- Session or authentication state

Only include mechanisms actually present in the project.

### 12. Business workflows

Describe important end-to-end flows as ordered steps or concise diagrams. For example, a quiz workflow might select a subject, load questions, submit answers, calculate a score, and save an attempt—but include such a flow only if the repository supports it.

### 13. External integrations

For each important external service, state its purpose, where it is called, and which configuration is needed. Do not reveal credentials.

### 14. Development workflow

Provide verified commands for:
- Install
- Run locally
- Build
- Lint
- Format
- Test
- Type-check
- Deploy, if applicable

Use commands from manifests, scripts, CI, and project docs. Clearly mark commands that could not be verified.

### 15. Coding conventions

Record conventions observed in the codebase, such as:
- Naming
- Component and module patterns
- Import organization
- Type usage
- Error handling
- Logging
- Testing
- Styling
- Dependency injection

Separate established conventions from recommendations. Do not turn a preference into a project rule without evidence.

### 16. Project rules for agents

Capture practical rules that prevent accidental inconsistency. Verify them against the repository where possible. Examples:
- Search for an existing component before creating a new one.
- Check existing hooks and services before duplicating functionality.
- Keep business logic out of presentation code when that matches the architecture.
- Avoid adding dependencies without justification.
- Preserve existing naming and design conventions.
- When uncertain, preserve current architecture instead of inventing a new pattern.

### 17. Known limitations and TODOs

Document known unfinished work, constraints, and gaps only when supported by TODOs, issues, tests, or project documentation. Avoid interpreting every incomplete-looking implementation as an acknowledged TODO.

### 18. Glossary

Define project-specific terms, acronyms, and domain concepts when they help an unfamiliar agent understand the codebase.

### 19. Quick navigation

Provide task-oriented pointers, for example:
- Authentication → relevant module and key files
- Profile UI → relevant components and routes
- Quiz logic → relevant feature and data modules

Use real paths that exist in the repository.

### 20. Agent instructions

End with concise, actionable instructions for future coding agents. Include only rules that fit the actual project and the user's stated requirements.

## Quality and maintenance rules

- Be accurate and concise: optimize for useful context per token, not maximum length.
- Preserve distinctions between verified facts, reasonable inferences, and unknowns.
- Prefer links to source files for detailed implementation over copying large code excerpts.
- Avoid duplicating a README or every source file.
- Do not include secrets, tokens, credentials, or private environment values.
- Use exact commands only when supported by repository configuration or documentation.
- Do not claim a test, build, or command was run unless it actually was.
- If an existing context file is present, update it in place conceptually: preserve useful facts, verify stale claims, and avoid unnecessary churn.
- If the repository is large, prioritize the top-level architecture, core workflows, critical modules, and common development tasks, then add a quick-navigation index.
- Keep the final file easy to scan with meaningful headings, tables when useful, and concise bullets.

## Suggested output location

Use an existing agent-context file if the repository already has one. Otherwise, recommend a suitable location such as `AGENTS.md` or `docs/llm-codebase-context.md`, based on the project's existing conventions and the target agent tooling.

## Completion report

After writing or proposing the context file, report:
- Which file was created or updated
- Which repository areas were inspected
- Which important details remain unknown or need human confirmation
- Whether any commands were actually run to verify the documented workflow
