# Project Name

> [Short tagline — 1 sentence describing what this project does]

## Tech Stack

[Replace with your tech stack]

- **Framework**: Next.js 16 + TypeScript
- **Styling**: Tailwind CSS v4 + shadcn/ui
- **Animation**: Motion (Framer Motion), GSAP
- **UI Library**: Mixed from DESIGN.md — Astryx, Animata, Skipper UI, etc.
- **Package Manager**: npm or pnpm

## Getting Started

```bash
# Install dependencies
npm install

# 1. Install skills (AI agent design/development skills)
python Skills.py

# 2. Install Spec Kit (SDLC workflow commands — run AFTER Skills.py)
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git@latest
specify init .

# Run development server
npm run dev
```

## Project Structure

```
├── .agents/         # AI agent skills (embedded + installed)
│   ├── design-basics/
│   ├── premium-design/
│   ├── performance_engineering/
│   └── ui-checklist/
├── context/         # Project context files (fill these in)
├── src/             # Application source code (feature-first)
│   ├── app/         # Next.js App Router
│   ├── features/    # Feature modules
│   ├── shared/      # Shared UI, hooks, utilities
│   └── entities/    # Domain models
├── Agent.md         # Agent execution protocol
├── DESIGN.md        # Curated component library references
├── Skills.py        # Skill installer
└── README.md        # This file
```

## Design Resources

- **DESIGN.md** contains the curated list of allowed component libraries and design tools
- **Astryx (Meta)** is the primary reference for production-grade components
- Premium animated libraries: Animata, Cult UI, Skipper UI, React Bits Pro, etc.

## Skills

Skills are in `.agents/` — run `python Skills.py` to install AI agent design and development skills. See `.agents/AGENTS.md` for details.

**After running Skills.py, you MUST also run specify to install the SDLC workflow commands:**

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git@latest
specify init .
```

This unlocks the `/speckit.*` commands (speckit.specify, speckit.plan, speckit.tasks, etc.).

## Important Rules

- Never default to a single UI library — always reference DESIGN.md
- Never use Mantine, Chakra UI, MUI, Ant Design
- Follow feature-first folder structure (Agent.md)
- Never use `npx install --force` or `npm install --force`

## License

[License type]