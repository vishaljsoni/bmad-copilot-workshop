---
stepsCompleted: ['step-01-init', 'step-02-context', 'step-03-technology-stack', 'step-04-system-architecture', 'step-05-data-model', 'step-06-performance', 'step-07-security', 'step-08-deployment', 'step-09-testing', 'step-10-adrs', 'step-11-complete']
inputDocuments:
  - '/Users/sumil/Documents/repos/bmad-todo-workshop/_bmad-output/planning-artifacts/prd.md'
  - '/Users/sumil/Documents/repos/bmad-todo-workshop/_bmad-output/planning-artifacts/ux-design-specification.md'
workflowType: 'architecture'
project_name: 'bmad-todo-workshop'
user_name: 'Sumil'
date: '2026-03-12T15:06:27.716Z'
---

# Architecture Decision Document

_This document builds collaboratively through step-by-step discovery. Sections are appended as we work through each architectural decision together._

## Project Context Analysis

### Requirements Overview

**Functional Requirements:**

The system requires 23 functional capabilities across 7 domains:

1. **Task Management** (FR-1.1 - FR-1.3): Core CRUD operations with metadata tracking, natural language deadline parsing, and keyboard-driven quick-add (Ctrl+K)

2. **Dependency Tracking** (FR-2.1 - FR-2.3): 1:1 blocking relationships in MVP with visual indicators and dependency risk detection

3. **Progress & Status Tracking** (FR-3.1 - FR-3.2): Three-state tracking (not started/in progress/complete) with timestamp capture for pattern analysis

4. **Predictive Analytics** (FR-4.1 - FR-4.3): Pattern recognition on historical data, deadline slip prediction with confidence scoring, cascading impact analysis across dependency chains

5. **Intelligent Notifications** (FR-5.1 - FR-5.3): Context-aware warnings (max once/day per task), high-signal predictions with explanatory content

6. **User Interface & Interaction** (FR-6.1 - FR-6.4): Three-column dashboard (Focus Today/On Track/Blocked), keyboard navigation, progressive disclosure, real-time UI updates

7. **Data Management** (FR-7.1 - FR-7.3): Browser-based persistence (IndexedDB + LocalStorage), export/import for portability, zero data loss with validation

**Non-Functional Requirements:**

Critical NFRs that drive architectural decisions:

- **Performance**: Task list render < 1s (100 tasks), prediction calc < 500ms, UI updates < 100ms, initial load < 3s
- **Reliability**: Zero data loss, immediate persistence, graceful degradation if prediction engine fails
- **Usability**: 30s to first task creation, keyboard shortcuts for all core functions, zero external documentation needed
- **Compatibility**: Modern evergreen browsers only, desktop-first (1280px+), responsive to 768px
- **Maintainability**: Unit testable prediction logic, independently testable UI components

**Scale & Complexity:**

- **Primary domain**: Full-stack SPA (frontend-heavy with client-side intelligence)
- **Complexity level**: Medium - MVP well-scoped but intelligent features add algorithmic complexity
- **Estimated architectural components**: 
  - UI Layer: Dashboard, Quick-Add, Task List, Warning Banner
  - Intelligence Layer: Pattern Analysis Engine, Prediction Engine, Dependency Resolver
  - Data Layer: Storage Abstraction, State Management, Export/Import
  - Approximately 8-12 major components

### Technical Constraints & Dependencies

**Hard Constraints:**
- **No backend server** in MVP - entirely client-side
- **Browser storage only** - IndexedDB for task data, LocalStorage for preferences
- **Single user** - no authentication, no multi-tenant concerns
- **Modern browsers** - ES6+, no IE11, no polyfills
- **Bootstrap period** - Requires 10 completed tasks minimum for prediction accuracy

**UX-Driven Technical Requirements:**
- **Keyboard-first interaction** - Command palette (Ctrl+K), arrow navigation
- **Real-time prediction updates** - Must recalculate on every task change
- **Progressive disclosure** - UI complexity revealed on demand
- **Calm confidence visual design** - No red alerts, amber for focus, soft greens for success
- **Three-column responsive layout** - Segment-based dashboard architecture

### Cross-Cutting Concerns Identified

1. **State Management**: Complex UI state across dashboard segments, task details, predictions, and bootstrap progress tracking

2. **Data Persistence Strategy**: Hybrid LocalStorage (preferences) + IndexedDB (task data/history) with immediate write-through

3. **Prediction Engine Architecture**: Client-side pattern recognition that processes incrementally without blocking UI

4. **Keyboard Navigation System**: Global shortcuts and focus management across progressive disclosure

5. **Error Handling & Graceful Degradation**: Prediction failures must not break core task management

6. **Performance Optimization**: Efficient rendering for 100-task lists, optimistic UI updates, lazy loading strategies

## Technology Stack Decisions

### Frontend Framework

**Decision: React with TypeScript**

**Rationale:**
- Component-based architecture aligns with progressive disclosure pattern
- Strong TypeScript support for complex state management and prediction engine
- Virtual DOM optimizes performance for frequent re-renders (real-time predictions)
- Mature ecosystem for state management (Redux/Zustand), routing, testing
- Hooks enable clean separation of business logic (prediction engine) from UI

**Alternatives Considered:**
- Vue: Excellent but React ecosystem better for complex state patterns
- Svelte: Performance benefits but less mature tooling for TypeScript + testing

### State Management

**Decision: Zustand with Immer**

**Rationale:**
- Lightweight (avoiding Redux complexity for single-user app)
- Built-in TypeScript support
- Immer enables immutable updates for time-travel debugging
- Simple pub/sub for prediction engine to notify UI components
- Middleware support for localStorage persistence sync

### Data Persistence

**Decision: IndexedDB via Dexie.js**

**Rationale:**
- Dexie provides clean async/await API over raw IndexedDB
- Built-in TypeScript definitions
- Query capabilities for pattern analysis (filter completed tasks by date range)
- Transactions for data integrity
- Observable queries for reactive updates
- LocalStorage for simple preferences (last dashboard view, dismissed warnings)

### Styling & UI Components

**Decision: Tailwind CSS + Headless UI**

**Rationale:**
- Utility-first approach speeds development
- Easy to implement "calm confidence" color system (#FFF4E6 amber, #E8F5E9 green, etc.)
- Headless UI provides accessible keyboard navigation primitives
- PurgeCSS keeps bundle small
- Responsive breakpoints align with UX spec (1280px, 768px)

### Build & Tooling

**Decision: Vite**

**Rationale:**
- Fast HMR for development iteration
- ES modules native, tree-shaking by default
- TypeScript support out of box
- Optimized production builds
- Static hosting compatible (GitHub Pages, Netlify, Vercel)

## System Architecture

### High-Level Architecture Pattern

**Pattern: Client-Side MVC with Intelligent Agent**

```
┌─────────────────────────────────────────────────┐
│                   Browser                       │
│                                                 │
│  ┌──────────────────────────────────────────┐ │
│  │         Presentation Layer               │ │
│  │  (React Components + Tailwind)           │ │
│  │                                          │ │
│  │  Dashboard | Quick-Add | Task Detail    │ │
│  │  Warning Banner | Bootstrap Progress    │ │
│  └────────────┬─────────────────────────────┘ │
│               │                                 │
│  ┌────────────▼─────────────────────────────┐ │
│  │       Application Layer                  │ │
│  │  (Zustand State Management)              │ │
│  │                                          │ │
│  │  Task State | UI State | Predictions    │ │
│  └───┬──────────────────────────────┬───────┘ │
│      │                              │         │
│  ┌───▼──────────┐          ┌───────▼──────┐ │
│  │ Intelligence │          │ Data Layer   │ │
│  │ Engine       │          │              │ │
│  │              │          │ Dexie/IDB    │ │
│  │ Pattern      │◄─────────│ LocalStorage │ │
│  │ Analyzer     │          │              │ │
│  │ Predictor    │          │ Export/Import│ │
│  │ Dep Resolver │          │              │ │
│  └──────────────┘          └──────────────┘ │
│                                                 │
└─────────────────────────────────────────────────┘
```

### Component Architecture

**UI Components (Presentation Layer):**

1. **Dashboard** - Three-column layout container
   - FocusTodayPanel
   - OnTrackPanel (collapsible)
   - BlockedPanel
   
2. **QuickAddModal** - Ctrl+K command palette
   - NaturalLanguageInput
   - DeadlineParser
   - TaskPreview

3. **TaskList** - Virtualized list for performance
   - TaskListItem (compact view)
   - TaskDetailExpanded (inline expansion)
   - QuickActions (edit, delete, dependency)

4. **WarningBanner** - Intelligent alert display
   - PredictionMessage
   - ConfidenceIndicator
   - ActionButtons

5. **BootstrapProgress** - First-time user guidance
   - ProgressRing
   - MilestoneIndicator
   - EducationalTooltips

**Intelligence Layer:**

1. **PatternAnalyzer**
   - Analyzes completion history
   - Calculates average completion time per task type
   - Identifies work patterns (time of day, day of week)
   - Outputs: Pattern summary for prediction engine

2. **PredictionEngine**
   - Consumes pattern data + current task progress
   - Calculates slip probability with confidence score
   - Runs incrementally (Web Worker for heavy computation)
   - Outputs: Predictions array with confidence levels

3. **DependencyResolver**
   - Builds dependency graph from task relationships
   - Detects circular dependencies
   - Calculates impact cascade (what fails if X slips)
   - Outputs: Impact analysis for warnings

4. **WarningGenerator**
   - Filters predictions (high confidence + high impact only)
   - Enforces once-per-day-per-task rule
   - Generates human-readable warning text
   - Outputs: Warning objects for UI display

**Data Layer:**

1. **TaskRepository**
   - CRUD operations for tasks
   - Query interface for pattern analyzer
   - Transaction management for data integrity
   - Schema: `{id, title, deadline, status, createdAt, completedAt, blockedBy, progress}`

2. **HistoryRepository**
   - Stores completion events for pattern analysis
   - Append-only log of state changes
   - Schema: `{taskId, timestamp, event, metadata}`

3. **PreferencesRepository**
   - LocalStorage wrapper for UI preferences
   - Dismissed warnings tracking (24h expiry)
   - Last dashboard state
   - Schema: `{dismissedWarnings[], lastView, theme}`

## Data Model

### Core Entities

**Task**
```typescript
interface Task {
  id: string;                    // UUID
  title: string;                 // User-provided
  deadline: Date;                // Parsed from natural language
  description?: string;          // Optional
  status: 'not_started' | 'in_progress' | 'complete';
  createdAt: Date;               // Auto-generated
  completedAt?: Date;            // When marked complete
  blockedBy?: string;            // Task ID (1:1 in MVP)
  metadata: {
    lastModified: Date;
    estimatedDuration?: number;  // ML-derived, optional
  };
}
```

**CompletionEvent** (for pattern analysis)
```typescript
interface CompletionEvent {
  id: string;
  taskId: string;
  taskTitle: string;             // Denormalized for analysis
  startedAt: Date;
  completedAt: Date;
  durationDays: number;          // Derived
  wasLate: boolean;              // Missed deadline?
  metadata: {
    dayOfWeek: number;
    timeOfDay: 'morning' | 'afternoon' | 'evening';
  };
}
```

**Prediction**
```typescript
interface Prediction {
  taskId: string;
  type: 'deadline_slip' | 'dependency_risk' | 'deadline_proximity';
  confidence: number;            // 0-1
  message: string;               // Human-readable
  impact: {
    blockedTasks: string[];      // Task IDs affected
    severity: 'low' | 'medium' | 'high';
  };
  generatedAt: Date;
  expiresAt: Date;              // 24h from generation
}
```

**Preferences**
```typescript
interface Preferences {
  dismissedWarnings: Map<string, Date>;  // taskId -> dismissedAt
  dashboardView: 'default' | 'compact';
  lastBootstrapStep: number;
  completedTaskCount: number;    // For bootstrap progress
}
```

### Data Flow Patterns

**Task Creation Flow:**
1. User triggers Ctrl+K → QuickAddModal
2. Parse natural language deadline → Date object
3. Create Task entity → TaskRepository.create()
4. IndexedDB transaction writes atomically
5. Zustand state update → UI re-renders
6. Optimistic UI (show immediately, rollback on error)

**Prediction Generation Flow:**
1. Task state change detected (Zustand middleware)
2. Debounced trigger (500ms) → PredictionEngine.calculate()
3. PatternAnalyzer queries HistoryRepository
4. Pattern data + current state → ML algorithm
5. Predictions filtered by confidence threshold (>0.7)
6. DependencyResolver adds impact analysis
7. WarningGenerator creates UI-ready warnings
8. Zustand predictions state updated → Banner renders

**Data Persistence Flow:**
1. User action modifies state (Zustand action)
2. Middleware intercepts state change
3. Dexie transaction writes to IndexedDB
4. Write-through pattern (UI updates immediately)
5. Error handling: Rollback UI state on write failure
6. Export: Serialize Zustand state → JSON blob

## Performance Strategy

### Rendering Optimization

**Virtual Scrolling:**
- Use `react-window` for task lists >50 items
- Render only visible items + buffer
- Maintains 60fps scroll performance

**Memoization:**
- React.memo on TaskListItem (prevent unnecessary re-renders)
- useMemo for expensive calculations (dependency graphs)
- useCallback for event handlers passed as props

**Code Splitting:**
- Lazy load QuickAddModal (only on Ctrl+K)
- Lazy load TaskDetailExpanded (only on click)
- Separate bundle for PredictionEngine (Web Worker)

### Computation Optimization

**Web Workers:**
- PatternAnalyzer runs in Worker thread
- Prevents UI blocking during heavy computation
- Communication via postMessage with structured cloning

**Incremental Processing:**
- Prediction engine processes in chunks (10 tasks at a time)
- Yields to main thread between chunks
- Progressive results (show partial predictions)

**Caching:**
- Memoize pattern analysis results (5min TTL)
- Cache dependency graph until task relationships change
- LocalStorage cache for recent predictions

### Loading Strategy

**Initial Load:**
- Critical CSS inline
- Progressive hydration (render static shell first)
- Lazy load prediction engine after first paint
- IndexedDB read async, show skeleton UI

**Asset Optimization:**
- Tree-shake unused code (Vite rollup)
- Minify production bundle
- Gzip compression on static host
- Target <500KB initial bundle

## Security & Data Privacy

### Data Security (Client-Side)

**No Backend = Reduced Attack Surface:**
- No API endpoints to secure
- No server-side vulnerabilities
- No network data transmission (local only)

**Browser Storage Security:**
- IndexedDB isolated per origin (same-origin policy)
- LocalStorage XSS protection (CSP headers)
- No sensitive data stored (personal task info only)

**Export Security:**
- JSON export includes schema version
- Import validates schema before loading
- Sanitize user input (task titles, descriptions)

### Privacy Considerations

**Single User, Local Data:**
- No analytics, no tracking, no telemetry
- All data stays on user's device
- Export feature for backup/migration only

**Future Considerations (Post-MVP):**
- If cloud sync added: E2E encryption required
- If analytics added: Privacy-preserving aggregation only

## Deployment Strategy

### Static Hosting (MVP)

**Hosting Options:**
- GitHub Pages (free, simple)
- Netlify (free tier, auto-deploy)
- Vercel (free tier, optimized for Vite)

**Build Process:**
1. `npm run build` → production bundle
2. Vite optimizes assets (minify, tree-shake)
3. Output to `/dist` directory
4. Deploy via git push (automated)

**No Server Required:**
- HTML + CSS + JS served statically
- Service Worker for offline (post-MVP)
- No environment variables needed

### Browser Compatibility

**Target:**
- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

**Feature Detection:**
- Check IndexedDB availability at startup
- Graceful fallback to LocalStorage if needed
- Display compatibility warning for old browsers

## Testing Strategy

### Unit Testing

**Test Framework: Vitest**
- Fast, Vite-native test runner
- TypeScript support built-in
- Snapshot testing for components

**Coverage Targets:**
- PredictionEngine: 90%+ (critical business logic)
- DependencyResolver: 85%+ (complex algorithm)
- Data repositories: 80%+
- UI components: 60%+ (integration tests more valuable)

**Key Test Suites:**
- Pattern analysis correctness
- Prediction accuracy simulation
- Dependency graph edge cases
- Data persistence integrity

### Integration Testing

**Test Framework: Testing Library**
- User-centric testing approach
- Keyboard interaction testing
- State management flow testing

**Critical Flows to Test:**
- Task creation → persistence → prediction
- Dependency linking → impact calculation
- Warning display → dismissal → 24h re-show
- Bootstrap progress tracking

### E2E Testing (Post-MVP)

**Consider: Playwright**
- Cross-browser testing
- Keyboard navigation flows
- IndexedDB state verification
- Export/import round-trip

## Architecture Decision Records (ADRs)

### ADR-001: Client-Side Only Architecture

**Status:** Accepted

**Context:** MVP requires fast development with minimal complexity

**Decision:** Build entirely client-side with browser storage, no backend

**Consequences:**
- ✅ Faster development (no API, no deployment complexity)
- ✅ Zero hosting costs
- ✅ No authentication needed
- ❌ No cross-device sync (acceptable for MVP)
- ❌ Limited to browser storage limits (~50MB typical)

### ADR-002: React + TypeScript

**Status:** Accepted

**Context:** Need strong typing for complex state and prediction logic

**Decision:** Use React with TypeScript, not JavaScript

**Consequences:**
- ✅ Type safety prevents runtime errors in prediction engine
- ✅ Better IDE support and refactoring
- ✅ Self-documenting interfaces
- ❌ Slightly longer build times (acceptable trade-off)

### ADR-003: Zustand over Redux

**Status:** Accepted

**Context:** Redux adds significant boilerplate for single-user app

**Decision:** Use Zustand for simpler state management

**Consequences:**
- ✅ Less boilerplate (actions, reducers, selectors)
- ✅ Still testable and type-safe
- ✅ Middleware for persistence
- ❌ Less mature ecosystem than Redux (acceptable for MVP)

### ADR-004: Pattern Recognition in Browser

**Status:** Accepted

**Context:** No backend means ML must run client-side

**Decision:** Implement simple pattern recognition algorithm in JavaScript

**Consequences:**
- ✅ No server costs
- ✅ Privacy-preserving (data never leaves device)
- ❌ Limited to simple algorithms (no deep learning)
- ❌ Performance constrained by browser (mitigated with Web Workers)

## Implementation Guidance for AI Agents

### Critical Implementation Rules

1. **Zero Data Loss:** Every state mutation must write-through to IndexedDB before UI confirmation
2. **Prediction Performance:** Pattern analysis must complete in <500ms or yield to main thread
3. **Keyboard First:** All features accessible via keyboard, mouse optional
4. **Progressive Disclosure:** Simple tasks never require complex UI, reveal on demand only
5. **Graceful Degradation:** If prediction engine fails, core task management still works

### Component Implementation Order

**Phase 1: Core Task Management**
1. TaskRepository + IndexedDB setup
2. Basic Task CRUD (no predictions yet)
3. Simple task list UI
4. LocalStorage preferences

**Phase 2: Intelligence Layer**
5. HistoryRepository + completion tracking
6. PatternAnalyzer with mock data
7. Basic PredictionEngine (simple heuristics)
8. Warning display UI

**Phase 3: Advanced Features**
9. Dependency tracking (1:1 blocking)
10. Dashboard segmentation (Focus/On Track/Blocked)
11. QuickAdd modal with Ctrl+K
12. Bootstrap progress tracking

**Phase 4: Polish**
13. Keyboard navigation system
14. Progressive disclosure refinement
15. Performance optimization (virtualization)
16. Export/import functionality

### Testing Requirements for AI Implementation

Each component must include:
- Unit tests for business logic
- Integration tests for data flow
- Accessibility tests for keyboard navigation
- Performance tests for rendering benchmarks

### Success Criteria for Implementation

- [ ] All 23 functional requirements implemented
- [ ] All NFRs met (performance, reliability, usability)
- [ ] Zero data loss under normal operation
- [ ] 70%+ prediction accuracy after bootstrap period
- [ ] Keyboard navigation for all core functions
- [ ] <1s task list render for 100 tasks
- [ ] <500ms prediction calculation
