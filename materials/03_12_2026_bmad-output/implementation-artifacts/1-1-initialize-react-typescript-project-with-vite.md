# Story 1.1: Initialize React + TypeScript Project with Vite

Status: blocked

**BLOCKED:** System npm installation malfunction detected. npm install repeatedly reports "audited 4 packages" despite package.json listing 17 dependencies. Vite and other dev dependencies not installing.

**Environment Issue:** Node v25.6.1, npm 11.9.0 - npm cache and fresh installs fail identically.

**Next Steps:** Requires system-level npm troubleshooting before story can proceed.

## Story

As a developer,
I want a React + TypeScript project scaffolded with Vite build tool,
so that I have a modern, fast development environment ready for feature implementation.

## Acceptance Criteria

**AC1:** React 18+ project with TypeScript is created using Vite
**Given** I am starting a new project
**When** I run the initialization command
**Then** a React 18+ project with TypeScript is created using Vite

**AC2:** TypeScript strict mode configuration
**And** the project includes proper tsconfig.json with strict mode enabled

**AC3:** Dev server runs successfully
**And** the dev server starts successfully on `npm run dev`

**AC4:** Production build works
**And** the project builds successfully with `npm run build`

**AC5:** Code quality tools configured
**And** ESLint and Prettier are configured for code quality

## Tasks / Subtasks

- [ ] **Task 1:** Initialize Vite React-TypeScript project (AC: AC1, AC2)
  - [ ] Run `npm create vite@latest . -- --template react-ts` in project root
  - [ ] Verify package.json includes React 18+, TypeScript, Vite dependencies
  - [ ] Review and update tsconfig.json for strict mode if not already set
  - [ ] Install dependencies with `npm install`

- [ ] **Task 2:** Configure ESLint for code quality (AC: AC5)
  - [ ] Install ESLint with TypeScript support: `npm install -D eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin`
  - [ ] Create .eslintrc.json with React + TypeScript rules
  - [ ] Add eslint-plugin-react and eslint-plugin-react-hooks
  - [ ] Add lint script to package.json: `"lint": "eslint . --ext ts,tsx --report-unused-disable-directives --max-warnings 0"`

- [ ] **Task 3:** Configure Prettier for consistent formatting (AC: AC5)
  - [ ] Install Prettier: `npm install -D prettier eslint-config-prettier eslint-plugin-prettier`
  - [ ] Create .prettierrc with project standards (2 spaces, single quotes, trailing commas)
  - [ ] Add format script to package.json: `"format": "prettier --write \"src/**/*.{ts,tsx,css}\""`
  - [ ] Create .prettierignore to exclude node_modules, dist, build

- [ ] **Task 4:** Verify dev server functionality (AC: AC3)
  - [ ] Run `npm run dev` and confirm server starts without errors
  - [ ] Verify localhost URL loads in browser
  - [ ] Confirm hot module replacement (HMR) works with a test edit

- [ ] **Task 5:** Verify production build (AC: AC4)
  - [ ] Run `npm run build` and confirm build completes successfully
  - [ ] Verify dist/ folder is generated with optimized assets
  - [ ] Check for build warnings or errors

- [ ] **Task 6:** Create basic project documentation
  - [ ] Create/update README.md with:
    - Project name: bmad-todo-workshop
    - Tech stack: React 18 + TypeScript + Vite
    - Setup instructions: `npm install`, `npm run dev`, `npm run build`
    - Available scripts
  - [ ] Add .gitignore if not already present (node_modules, dist, .env, etc.)

## Dev Notes

### Architecture Context

**From Architecture.md:**
- **Technology Stack Decision:** React with TypeScript (ADR-002)
- **Build Tool:** Vite chosen for fast HMR and optimized production builds
- **Code Quality:** ESLint and Prettier configured for code quality (Story 1.1 AC5)
- **TypeScript:** Strict mode enabled for type safety
- **Performance Target (NFR-P3):** Initial application load < 3 seconds on broadband

**Key Technical Requirements:**
- Modern evergreen browsers only (Chrome, Firefox, Safari, Edge)
- ES6+ JavaScript support required
- No legacy browser support (IE11)

### Project Structure Notes

**Standard Vite React-TS Structure:**
```
/bmad-todo-workshop
├── src/
│   ├── App.tsx
│   ├── App.css
│   ├── main.tsx
│   ├── index.css
│   └── vite-env.d.ts
├── public/
├── index.html
├── package.json
├── tsconfig.json
├── tsconfig.node.json
├── vite.config.ts
├── .eslintrc.json
├── .prettierrc
└── README.md
```

**Future Stories Will Add:**
- Story 1.2: Zustand state management
- Story 1.3: Tailwind CSS styling
- Story 1.4: Dexie.js for IndexedDB
- Story 1.5: LocalStorage wrapper

### Testing Standards

- **For this story:** Manual verification only (dev server, build, linting)
- **Future stories:** Unit tests with Vitest (as per Architecture)
- **No test files required** for this foundational setup story

### References

- [Source: _bmad-output/planning-artifacts/architecture.md#Technology Stack Decisions]
- [Source: _bmad-output/planning-artifacts/architecture.md#Build & Tooling]
- [Source: _bmad-output/planning-artifacts/prd.md#Non-Functional Requirements - NFR-P3]
- [Source: _bmad-output/planning-artifacts/epics.md#Epic 1: Story 1.1]

### Important Constraints

- **No database setup in this story** - comes in Story 1.4
- **No state management yet** - comes in Story 1.2
- **No styling framework** - comes in Story 1.3
- This story focuses solely on React + TypeScript + Vite scaffold with code quality tools

## Dev Agent Record

### Agent Model Used

_To be filled by dev agent_

### Debug Log References

_To be filled by dev agent during implementation_

### Completion Notes List

_To be filled by dev agent after each task completion_

### File List

_Files created/modified during implementation will be listed here_
