# Agent Execution Protocol

This document defines **how to execute tasks in this project** — the workflow, skill usage, folder structure, and execution discipline. Any AI agent executing work in this project MUST read this first and follow the protocol.

---

## How to Use This File

1. Read the **Context Hierarchy** below before writing any code
2. Follow the **Design-First Feature Workflow** — spec first, visualize, clarify, approve, then build
3. Follow the **Project Structure Standards** for folder hierarchy
4. Identify which **domain** your task belongs to using the **Skill Mapping Table**
5. Load the right skill(s) via the `skill` tool before implementing
6. Reference **DESIGN.md** for component library selection — never default to a single library
7. Reference **Astryx (Meta design system)** for production-grade components and tokens
8. Run **Pre-Exit Checks** before marking any task complete

---

## Context Hierarchy (Read in Order)

1. `context/project-overview.md` — Product definition, goals, features, scope
2. `context/architecture.md` — System structure, boundaries, storage model, invariants
3. `context/ui-context.md` — Theme, colors, typography, component conventions
4. `context/code-standards.md` — Implementation rules and conventions
5. `context/ai-workflow-rules.md` — Development workflow, scoping rules, delivery approach
6. `context/progress-tracker.md` — Current phase, completed work, open questions, next steps

---

## Design-First Feature Workflow

**Before writing ANY code for a new feature, follow this exact sequence:**

### Step 1: Create Feature Spec
Create a feature specification in a `Feature_docs/<feature-name>/spec.md` file. This spec defines WHAT to build — user stories, requirements, success criteria.

### Step 2: Load Design Skills & Visualize
Load the relevant design skills to shape a design vision. Create a **visual wireframe / component tree diagram** (ASCII or detailed layout description) that shows the page structure, section hierarchy, spacing, and visual rhythm.

### Step 3: Ask Clarifying Questions
Ask the user clarifying questions about the design before proceeding.

### Step 4: Get User Approval
Present the spec + wireframe/visualization to the user for approval. **Do not write any code until the user explicitly approves.**

### Step 5: Implement
Only after approval, proceed with implementation: Plan → Tasks → Build.

---

## Project Structure Standards

### Frontend — Feature-First Architecture

Every Next.js project must follow this feature-first structure. Group code by business domain, not by file type.

```
src/
├── app/                          # Next.js App Router (routes, layouts, providers)
│   ├── layout.tsx                # Root layout (fonts, metadata, providers)
│   ├── page.tsx                  # Home page (thin composition layer)
│   ├── (auth)/                   # Route group for auth pages
│   │   ├── login/page.tsx
│   │   └── register/page.tsx
│   └── (dashboard)/
│       ├── layout.tsx
│       └── page.tsx
├── features/                     # Feature modules (self-contained, domain-driven)
│   ├── auth/                     # Example: Authentication feature
│   │   ├── api/                  # API calls, mutations, queries
│   │   ├── components/           # Feature-specific UI (LoginForm, AuthGuard)
│   │   ├── hooks/                # Feature-specific hooks (useAuth, useSession)
│   │   ├── types/                # Domain types (User, Session)
│   │   └── index.ts             # Public API — only exports from here
│   ├── billing/
│   │   ├── api/
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── types/
│   │   └── index.ts
│   └── dashboard/
│       ├── api/
│       ├── components/
│       ├── hooks/
│       ├── types/
│       └── index.ts
├── shared/                       # Business-agnostic reusable code
│   ├── ui/                       # Design system primitives (Button, Input, Modal)
│   ├── hooks/                    # Generic hooks (useDebounce, useMediaQuery)
│   ├── lib/                      # Pure utilities (formatDate, cn, validators)
│   ├── api/                      # HTTP client, interceptors
│   └── types/                    # Shared types
├── entities/                     # Domain models shared across features
│   ├── user/
│   │   ├── api/
│   │   ├── model/                # Types, schemas, invariants
│   │   └── index.ts
│   └── organization/
│       ├── api/
│       ├── model/
│       └── index.ts
├── lib/                          # Low-level infrastructure
│   ├── api-client.ts             # Axios / fetch client
│   ├── query-client.ts           # TanStack Query setup
│   └── logger.ts
├── config/                       # Runtime config, env vars, constants
│   └── index.ts
└── styles/                       # Global styles, design tokens
    ├── globals.css
    └── tokens.css
```

### Backend — Controller-Service-Repository Pattern

```
src/
├── controllers/                  # HTTP layer — request parsing, response formatting
│   ├── auth.controller.ts
│   ├── user.controller.ts
│   └── billing.controller.ts
├── services/                     # Business logic layer
│   ├── auth.service.ts
│   ├── user.service.ts
│   └── billing.service.ts
├── repositories/                 # Data access layer (DB queries, external APIs)
│   ├── user.repository.ts
│   ├── billing.repository.ts
│   └── subscription.repository.ts
├── middleware/                    # Express/Next.js middleware
│   ├── auth.middleware.ts
│   ├── rate-limit.middleware.ts
│   └── validation.middleware.ts
├── validators/                   # Request validation schemas
│   ├── auth.validator.ts
│   └── billing.validator.ts
├── types/                        # Shared backend types
├── config/                       # Config, env vars, DB connection
├── utils/                        # Pure helpers
└── index.ts                      # App entry point
```

### Dependency Direction
```
app/ (pages) → features/ → entities/ → shared/
app/ (pages) → features/ → shared/
features/ → shared/
features/ → entities/
entities/ → shared/

NEVER: features/ → features/ (cross-feature imports)
NEVER: shared/ → features/ or entities/ (shared must not know about business logic)
NEVER: entities/ → features/ (domain models don't depend on workflows)
```

### Key Rules
1. **Each feature is a self-contained module** — owns its API calls, components, hooks, types
2. **Public API via `index.ts`** — external code imports only from `features/auth`, never deep paths
3. **Co-locate by domain** — tests, styles, and types stay next to the code they belong to
4. **Promote to `shared/` only on second use** — avoid premature abstraction
5. **`entities/` for stable domain models** — User, Product, Organization (not features)
6. **`app/` pages are thin** — they compose, not contain business logic

---

## Skill Mapping Table

All skills are in `.agents/`:

| # | Domain | Skill | Location | When to Use |
|---|--------|-------|----------|-------------|
| 1 | **Design Fundamentals** | `design-basics` | `.agents/design-basics/` | Any visual decision: colors, typography, spacing, layout, accessibility |
| 2 | **Premium UI Polish** | `premium-design` | `.agents/premium-design/` | Making UI look premium/professional — typography, color, layout, tokens |
| 3 | **Performance Engineering** | `performance_engineering` | `.agents/performance_engineering/` | Optimizing performance, Core Web Vitals, Lighthouse scores |
| 4 | **UI/UX Checklist** | `ui-checklist` | `.agents/ui-checklist/` | Auditing components/pages for completeness |
| 5 | **Design Psychology** | `DESIGN-PSYCHOLOGY.md` | `DESIGN-PSYCHOLOGY.md` (root) | User describes UI vaguely, or before any feature design — understand psychology + systems thinking |
| 6 | **DESIGN.md** | Component Libraries | `DESIGN.md` (root) | Reference curated component libraries before writing UI code |
| 7+ | **Installed Skills** | Various | `.agents/skills/` | After running `python Skills.py` |

---

## Component Library Selection Protocol

1. **Always reference DESIGN.md** before choosing a component library
2. When the user describes a UI element in vague/layman terms, **use namethatui.com** (https://namethatui.com/) to translate their description into the correct component name
3. **Check Astryx (Meta) first** for standard UI patterns — buttons, forms, tables, dialogs, nav
4. For **animated/premium sections** (hero, pricing, FAQ): Animata, Cult UI, Skipper UI, React Bits Pro, Aceternity, MagicUI
5. For **utility components** (tabs, accordions, tooltips): COSS UI, HeroUI, or Astryx
6. **Never default to HeroUI** — it's one option among many, not the default
7. **Never use Mantine, Chakra, MUI, Ant Design** — these are not allowed
8. **Prefer CLI-installable or copy-paste** — shadcn registry, direct source. Own the code.
9. **Mix libraries** — a hero from one, pricing from another, forms from Astryx

---

## Astryx (Meta) Design System — Primary Reference

URL: https://astryx.atmeta.com/docs/getting-started

Astryx is Meta's design system. It provides:

- **200+ production-grade components** — Button, Dialog, Table, Form fields, Navigation, Layout, Toast, etc.
- **Full design token system** — colors, spacing, typography, elevation, motion, shape
- **7 themes** — neutral, butter, chocolate, gothic, matcha, stone, y2k
- **Page templates** — full layouts and page shells
- **AI-specific components** — Chat Composer, Command Palette, Tokenized Text, Streaming Text
- **StyleX integration** — atomic CSS-in-JS for custom styling
- **CLI tool** — `npx @astryxdesign/cli init` to set up agent docs

**Always check Astryx first for any standard UI component before pulling from other libraries.**

---

## Design Psychology Protocol

Read `DESIGN-PSYCHOLOGY.md` for deep design knowledge. Use these resources before making design decisions:

### NameThatUI — Translate Vague User Descriptions
- **URL:** https://namethatui.com/
- **When:** The user says "the thing that does X" or describes a UI element in layman terms
- **What it does:** Translates vague descriptions into exact component names, ARIA roles, and HTML elements
- **Do this:** Go to the site, describe what the user said, find the real name, then build that component
- **Example:** User says "the dark layer behind the popup" → you find "Scrim (Backdrop)" → you build a `<dialog>` with `::backdrop`

### Product Design Psychology — Design Products People Love
- **URL:** https://productdesignpsychology.com/
- **When:** Before ANY feature design — especially forms, checkout flows, navigation, and onboarding
- **What it gives you:** 40 chapters on cognitive biases, user psychology, and organizational dynamics
- **Key principles to always apply:**
  - "Nobody Remembers Your UI" — design for recognition, not recall
  - "Design the Last Moment First" — start with the user's goal
  - "Fake Progress Is Real Motivation" — show progress, celebrate completions
  - "More Options Make Users Quit" — fewer choices = more conversions
  - "Layout Speaks Before You Do" — visual hierarchy communicates priority
  - "Your UI Is Exhausting" — minimize cognitive load at every step

### DesignSystems.com — Industry Best Practices
- **URL:** https://www.designsystems.com/
- **When:** Setting up design tokens, component architecture, typography, icons, accessibility
- **What it gives you:** Guides from Figma + case studies from Spotify, Atlassian, GitHub, Salesforce
- **Reference for:** Typography systems, grid/layout foundations, iconography, design token strategy

---

## Pre-Exit Checks (Before Marking ANY Task Complete)

1. Lint passes (e.g., `npm run lint`)
2. Typecheck passes (e.g., `npm run typecheck`)
3. Build passes (e.g., `npm run build`)
4. All animations respect `@media (prefers-reduced-motion: reduce)`
5. Only `transform` + `opacity` are animated — no layout properties
6. No hardcoded colors — all use CSS custom properties from `ui-context.md`
7. `progress-tracker.md` is updated with the completed work
8. Folder structure follows the feature-first convention (no flat `components/` dumping ground)

---

## Git Discipline

1. **Branch naming**: `NNN-feature-name` (sequential) or `YYYYMMDD-HHMMSS-feature-name` (timestamp)
2. **Commits**: Use conventional commits — `type(scope): description`
3. **Never commit** to `main`/`master` directly — always use feature branches
4. **Commit messages**: `feat(scope): description`, `fix(scope): description`, etc.

---

## Important Rules

- **Never jump to coding.** Always follow the Design-First Feature Workflow
- **Never default to a single component library.** Mix and match from DESIGN.md
- **Before any feature design, read DESIGN-PSYCHOLOGY.md** — understand user psychology and cognitive biases first
- **When the user describes UI vaguely, use namethatui.com** to translate to exact component names
- **Never use `npx install --force` or `npm install --force`** — resolve dependency conflicts properly
- **Never create flat `components/` folders** — always organize by feature
- **After running Skills.py, MUST install specify:** `uv tool install specify-cli --from git+https://github.com/github/spec-kit.git@latest` then `specify init .` — this unlocks speckit.* SDLC commands
- Load skills before writing code. Do not guess design decisions.
- Update `progress-tracker.md` after every meaningful change.
- If unsure about a design decision, ask the user.