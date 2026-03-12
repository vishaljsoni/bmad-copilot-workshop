---
stepsCompleted: ['step-01-validate-prerequisites', 'step-02-design-epics', 'step-03-create-stories-in-progress']
inputDocuments:
  - '/Users/sumil/Documents/repos/bmad-todo-workshop/_bmad-output/planning-artifacts/prd.md'
  - '/Users/sumil/Documents/repos/bmad-todo-workshop/_bmad-output/planning-artifacts/architecture.md'
  - '/Users/sumil/Documents/repos/bmad-todo-workshop/_bmad-output/planning-artifacts/ux-design-specification.md'
---

# bmad-todo-workshop - Epic Breakdown

## Overview

This document provides the complete epic and story breakdown for bmad-todo-workshop, decomposing the requirements from the PRD, UX Design, and Architecture into implementable stories.

## Requirements Inventory

### Functional Requirements

**FR-1.1: Task Creation**
- System shall allow users to create tasks with title and deadline
- System shall support optional task description
- System shall provide keyboard shortcut (Ctrl+K) for quick task creation
- System shall parse natural language deadline input (e.g., "tomorrow", "Friday", "March 15")

**FR-1.2: Task Operations**
- System shall allow users to mark tasks as complete or incomplete
- System shall allow users to edit task details (title, deadline, description)
- System shall allow users to delete tasks
- System shall maintain task metadata (creation timestamp, completion timestamp)

**FR-1.3: Task Display**
- System shall display tasks in a list view
- System shall support sorting tasks by deadline
- System shall provide visual indicators for task status (not started, in progress, complete)

**FR-2.1: Dependency Definition**
- System shall allow users to mark a task as "blocked by" another task
- System shall support 1:1 blocking relationships in MVP
- System shall provide dropdown selection for dependency linking

**FR-2.2: Dependency Visualization**
- System shall display visual indicators when a task is blocked
- System shall show which task is blocking another task
- System shall highlight dependency chains in the UI

**FR-2.3: Dependency Risk Detection**
- System shall warn users when a blocking task is at risk
- System shall calculate impact of blocking task delays on dependent tasks

**FR-3.1: Status Management**
- System shall support three status states: not started, in progress, complete
- System shall allow users to update task status
- System shall automatically track status change timestamps

**FR-3.2: Completion Pattern Analysis**
- System shall store completion history for all tasks
- System shall track time between task creation and completion
- System shall maintain historical pattern data for prediction engine

**FR-4.1: Pattern Recognition**
- System shall analyze historical task completion patterns
- System shall calculate average completion time for similar tasks
- System shall identify user work patterns over time

**FR-4.2: Deadline Slip Prediction**
- System shall predict potential deadline slips based on progress patterns
- System shall provide confidence scoring for predictions
- System shall calculate predictions after minimum 10 completed tasks
- System shall achieve 70%+ prediction accuracy after bootstrap period

**FR-4.3: Impact Analysis**
- System shall identify tasks that will be impacted by deadline slips
- System shall calculate cascading effects of dependency chain failures
- System shall prioritize predictions by impact (number of dependent tasks affected)

**FR-5.1: Warning Generation**
- System shall generate deadline proximity alerts (3 days, 1 day, same day)
- System shall generate pattern-based slip predictions
- System shall generate dependency risk warnings

**FR-5.2: Context-Aware Timing**
- System shall limit warnings to maximum once per day per task
- System shall surface warnings when user opens the application
- System shall not generate push notifications in MVP

**FR-5.3: Warning Content**
- System shall explain why a warning is generated ("based on your pattern")
- System shall show impact of predicted slips ("blocks 3 other tasks")
- System shall provide actionable information in warnings

**FR-6.1: Dashboard**
- System shall provide dashboard with status segmentation (Focus Today, On Track, Blocked)
- System shall highlight at-risk tasks prominently
- System shall display intelligent alerts at top of dashboard

**FR-6.2: Keyboard Navigation**
- System shall provide keyboard shortcuts for all core functions
- System shall support Ctrl+K for quick-add
- System shall support arrow key navigation through task lists
- System shall provide focus indicators for interactive elements

**FR-6.3: Progressive Disclosure**
- System shall show minimal task details by default
- System shall provide access to full task details on demand
- System shall keep simple tasks simple (title + deadline only)

**FR-6.4: Real-time Updates**
- System shall recalculate predictions immediately after task changes
- System shall update UI within 500ms of prediction calculation
- System shall render task lists in under 1 second for up to 100 tasks

**FR-7.1: Data Persistence**
- System shall store task data in browser IndexedDB
- System shall store preferences in browser LocalStorage
- System shall persist data across browser sessions

**FR-7.2: Data Portability**
- System shall provide export functionality (JSON format)
- System shall provide import functionality (JSON format)
- System shall support backup and restore of all task data

**FR-7.3: Data Integrity**
- System shall ensure no data loss during normal operations
- System shall handle graceful degradation if prediction engine fails
- System shall validate data before persistence

### NonFunctional Requirements

**NFR-P1: UI Responsiveness**
- Task list shall render in under 1 second for up to 100 tasks
- UI interactions shall maintain 60 frames per second
- Task updates shall reflect in UI within 100ms

**NFR-P2: Prediction Engine Performance**
- Prediction calculations shall complete within 500ms
- Pattern analysis shall process incrementally to avoid blocking UI
- Bootstrap predictions shall be available after 10 completed tasks

**NFR-P3: Load Time**
- Initial application load shall complete within 3 seconds on broadband connection (10+ Mbps)
- Application shall use lazy loading for non-critical resources
- Application shall cache static assets for subsequent loads

**NFR-R1: Data Integrity**
- System shall ensure zero data loss during normal operations
- System shall persist task changes immediately to browser storage
- System shall validate data before writing to storage

**NFR-R2: Graceful Degradation**
- If prediction engine fails, system shall continue basic task management functions
- If storage quota exceeded, system shall warn user and provide export option
- System shall handle browser storage failures without crashing

**NFR-R3: Error Handling**
- System shall display user-friendly error messages for recoverable errors
- System shall log errors to browser console for debugging
- System shall prevent invalid state transitions (e.g., marking non-existent tasks complete)

**NFR-U1: Learnability**
- New user shall be able to create first task within 30 seconds
- Keyboard shortcuts shall be discoverable through UI hints
- System shall require zero external documentation for MVP features

**NFR-U2: Efficiency**
- Expert user shall be able to create task via keyboard in under 5 seconds
- System shall minimize clicks required for common operations
- Quick-add shall support natural language input without training

**NFR-U3: Visual Design**
- UI shall follow clean, modern design aesthetic
- Color scheme shall clearly differentiate task status (Focus/On Track/Blocked)
- At-risk tasks shall be visually prominent without being alarming

**NFR-C1: Browser Support**
- System shall function correctly on latest versions of Chrome, Firefox, Safari, and Edge
- System shall require ES6+ JavaScript support
- System shall not support Internet Explorer or legacy browsers

**NFR-C2: Device Support**
- System shall be optimized for desktop browsers (1280px+ viewport width)
- System shall be responsive down to tablet size (768px)
- System shall be functional but not optimized for mobile devices

**NFR-M1: Code Quality**
- Code shall follow consistent style guide
- Functions shall be focused and single-purpose
- Complex logic shall include inline comments explaining intent

**NFR-M2: Testability**
- Prediction engine logic shall be unit testable
- UI components shall be independently testable
- Data persistence layer shall be mockable for testing

### Additional Requirements

**From Architecture:**

- **Starter Template**: Architecture specifies React + TypeScript with Vite as build tool. This impacts Epic 1 Story 1 for project setup.
- **State Management**: Zustand with Immer for state management, impacts all UI components
- **Data Persistence**: IndexedDB via Dexie.js for task data, LocalStorage for preferences
- **Styling**: Tailwind CSS + Headless UI for styling and accessible components
- **Web Workers**: PredictionEngine must run in Web Worker thread to prevent UI blocking
- **Virtual Scrolling**: Use react-window for task lists >50 items for performance
- **Service Worker**: Consider for offline support (post-MVP)

**From UX Design:**

- **Color Palette**: Warm amber (#FFF4E6) for Focus Today, soft green (#E8F5E9) for On Track, neutral gray (#F5F5F5) for Blocked
- **Typography**: Clean sans-serif (Inter, SF Pro) with clear hierarchy
- **Dashboard Layout**: Three-column segmentation with collapsible sections
- **Warning Banner**: Non-modal, amber background, dismissible with 24h memory
- **Quick-Add Modal**: Centered overlay (600px width) with inline preview
- **Bootstrap Progress**: Progress indicator showing path to 10 completed tasks
- **Accessibility**: Focus indicators, semantic HTML, keyboard navigation for all actions

### FR Coverage Map

FR-1.1: Epic 2 - Task Creation
FR-1.2: Epic 2 - Task Operations  
FR-1.3: Epic 2 - Task Display
FR-2.1: Epic 4 - Dependency Definition
FR-2.2: Epic 4 - Dependency Visualization
FR-2.3: Epic 6 - Dependency Risk Detection
FR-3.1: Epic 2 - Status Management
FR-3.2: Epic 5 - Completion Pattern Analysis
FR-4.1: Epic 5 - Pattern Recognition
FR-4.2: Epic 6 - Deadline Slip Prediction
FR-4.3: Epic 6 - Impact Analysis
FR-5.1: Epic 6 - Warning Generation
FR-5.2: Epic 6 - Context-Aware Timing
FR-5.3: Epic 6 - Warning Content
FR-6.1: Epic 3 - Dashboard
FR-6.2: Epic 7 - Keyboard Navigation
FR-6.3: Epic 2 - Progressive Disclosure
FR-6.4: Epic 3 - Real-time Updates
FR-7.1: Epic 1 - Data Persistence
FR-7.2: Epic 8 - Data Portability
FR-7.3: Epic 2 - Data Integrity

## Epic List

### Epic 1: Project Foundation & Data Layer
Development environment is set up with data persistence and basic infrastructure ready for feature development.
**FRs covered:** FR-7.1, Architecture starter template requirement

### Epic 2: Core Task Management
Users can create, view, edit, delete, and complete tasks with a clean interface.
**FRs covered:** FR-1.1, FR-1.2, FR-1.3, FR-3.1, FR-6.3, FR-7.3

### Epic 3: Intelligent Dashboard & Segmentation
Users see their tasks organized into Focus Today, On Track, and Blocked segments with clear visual hierarchy.
**FRs covered:** FR-6.1, FR-6.4

### Epic 4: Dependency Tracking
Users can link tasks as dependencies and see visual indicators when tasks are blocked.
**FRs covered:** FR-2.1, FR-2.2

### Epic 5: Pattern Analysis & History
System learns from user's task completion patterns to enable future predictions.
**FRs covered:** FR-3.2, FR-4.1

### Epic 6: Predictive Intelligence
Users receive intelligent warnings about potential deadline slips and dependency risks before problems cascade.
**FRs covered:** FR-4.2, FR-4.3, FR-5.1, FR-5.2, FR-5.3, FR-2.3

### Epic 7: Keyboard Navigation & Accessibility
Expert users can navigate and operate the entire app via keyboard without touching the mouse.
**FRs covered:** FR-6.2

### Epic 8: Data Portability & Backup
Users can export and import their task data for backup or migration purposes.
**FRs covered:** FR-7.2

## Epic 1: Project Foundation & Data Layer

Development environment is set up with data persistence and basic infrastructure ready for feature development.

### Story 1.1: Initialize React + TypeScript Project with Vite

As a developer,
I want a React + TypeScript project scaffolded with Vite build tool,
So that I have a modern, fast development environment ready for feature implementation.

**Acceptance Criteria:**

**Given** I am starting a new project
**When** I run the initialization command
**Then** a React 18+ project with TypeScript is created using Vite
**And** the project includes proper tsconfig.json with strict mode enabled
**And** the dev server starts successfully on `npm run dev`
**And** the project builds successfully with `npm run build`
**And** ESLint and Prettier are configured for code quality

### Story 1.2: Set Up State Management with Zustand

As a developer,
I want Zustand state management configured with TypeScript support,
So that I can manage application state efficiently across components.

**Acceptance Criteria:**

**Given** the React + TypeScript project is initialized
**When** I install and configure Zustand with Immer middleware
**Then** a store structure is created with TypeScript interfaces
**And** the store includes a basic example slice (e.g., app settings)
**And** middleware for localStorage persistence is configured
**And** the store is accessible via custom hooks with proper typing
**And** development tools (Redux DevTools extension) are enabled for debugging

### Story 1.3: Configure Tailwind CSS and Base Styles

As a developer,
I want Tailwind CSS configured with the project's color palette,
So that I can build the UI with the approved design system.

**Acceptance Criteria:**

**Given** the React project is set up
**When** I install and configure Tailwind CSS
**Then** Tailwind is integrated with Vite's build process
**And** tailwind.config.js includes custom colors (amber #FFF4E6, green #E8F5E9, gray #F5F5F5)
**And** typography configuration uses Inter or SF Pro font family
**And** base styles are applied globally (CSS reset, font imports)
**And** a simple test component renders with Tailwind classes successfully
**And** PurgeCSS is enabled for production builds

### Story 1.4: Implement IndexedDB with Dexie for Task Storage

As a developer,
I want IndexedDB set up with Dexie.js for task data persistence,
So that task data can be stored and retrieved efficiently in the browser.

**Acceptance Criteria:**

**Given** the project foundation is established
**When** I install Dexie.js and create the database schema
**Then** a Dexie database is initialized with a "tasks" table
**And** the tasks table schema includes: id, title, deadline, description, status, createdAt, completedAt, blockedBy
**And** a TaskRepository class provides CRUD methods (create, read, update, delete, list)
**And** all methods use async/await with proper TypeScript typing
**And** database initialization happens on app startup
**And** a simple test successfully creates and retrieves a task from IndexedDB

### Story 1.5: Implement LocalStorage Wrapper for Preferences

As a developer,
I want a LocalStorage abstraction for user preferences,
So that simple settings can be persisted without IndexedDB overhead.

**Acceptance Criteria:**

**Given** the storage layer architecture is defined
**When** I create a PreferencesRepository class
**Then** the class provides methods for get/set/remove operations
**And** methods handle JSON serialization/deserialization automatically
**And** TypeScript interfaces define the Preferences schema (dismissedWarnings, dashboardView, lastBootstrapStep, completedTaskCount)
**And** default values are returned when keys don't exist
**And** error handling prevents crashes if localStorage is unavailable (private browsing mode)
**And** a simple test successfully stores and retrieves preferences

## Epic 2: Core Task Management

Users can create, view, edit, delete, and complete tasks with a clean interface.

### Story 2.1: Create Task Entity and Basic Task List Display

As a user,
I want to see a list of my tasks displayed on the screen,
So that I can view all my tasks at a glance.

**Acceptance Criteria:**

**Given** the data layer is set up with IndexedDB
**When** I open the application
**Then** a task list component renders on the page
**And** any existing tasks are fetched from IndexedDB and displayed
**And** each task shows: title, deadline date, and status indicator
**And** tasks are sorted by deadline (earliest first)
**And** an empty state message displays when no tasks exist
**And** the list renders in under 1 second for up to 100 tasks (NFR-P1)
**And** tasks persist across browser sessions (FR-7.1)

### Story 2.2: Implement Quick-Add Modal with Ctrl+K Shortcut

As a user,
I want to press Ctrl+K to quickly add a new task,
So that I can create tasks without breaking my workflow.

**Acceptance Criteria:**

**Given** the application is running
**When** I press Ctrl+K (or Cmd+K on Mac)
**Then** a centered modal overlay appears (600px width per UX spec)
**And** the modal contains a single input field with focus
**And** the input placeholder shows "Task name - deadline" as guidance
**And** pressing Escape closes the modal without creating a task
**And** clicking outside the modal closes it without creating a task
**And** the modal follows UX design specifications (centered, clean styling)

### Story 2.3: Add Natural Language Deadline Parsing

As a user,
I want to type deadlines like "tomorrow" or "Friday" in the quick-add,
So that I can create tasks naturally without date pickers.

**Acceptance Criteria:**

**Given** the quick-add modal is open
**When** I type "Buy groceries - tomorrow" and press Enter
**Then** a task is created with title "Buy groceries"
**And** the deadline is set to tomorrow's date
**And** the task appears in the task list immediately (optimistic UI)
**And** the task is persisted to IndexedDB
**And** the modal closes after successful creation
**And** natural language parsing supports: "today", "tomorrow", "Friday", "March 15", "next Monday"
**And** invalid or missing deadlines show an inline error message
**And** task creation completes in under 5 seconds (NFR-U2)

### Story 2.4: Implement Task Status Management (Not Started/In Progress/Complete)

As a user,
I want to update the status of my tasks,
So that I can track progress on tasks before completing them.

**Acceptance Criteria:**

**Given** tasks exist in the task list
**When** I click on a task's status indicator
**Then** a status dropdown appears with three options: "Not Started", "In Progress", "Complete"
**And** selecting a status updates the task immediately
**And** the status change is persisted to IndexedDB with a timestamp (FR-3.1)
**And** the UI updates within 100ms of the status change (NFR-P1)
**And** visual indicators clearly differentiate the three states
**And** completed tasks show a completion timestamp
**And** status transitions are validated (prevent invalid state changes per NFR-R3)

### Story 2.5: Add Task Edit and Delete Functionality

As a user,
I want to edit task details or delete tasks I no longer need,
So that I can keep my task list accurate and up to date.

**Acceptance Criteria:**

**Given** a task exists in the task list
**When** I click on the task title
**Then** the task expands inline to show edit fields (progressive disclosure per FR-6.3)
**And** I can edit the title, deadline, and optional description
**And** changes are saved when I click "Save" or press Enter
**And** changes persist to IndexedDB immediately (NFR-R1)
**And** clicking "Delete" shows a confirmation prompt
**And** confirming deletion removes the task from IndexedDB and the UI
**And** the task list collapses back to compact view after save or cancel
**And** validation prevents saving empty task titles

### Story 2.6: Implement Task Completion Toggle

As a user,
I want to mark tasks as complete with a single click,
So that I can quickly update my task list as I finish work.

**Acceptance Criteria:**

**Given** a task exists with status "In Progress" or "Not Started"
**When** I click the checkbox next to the task
**Then** the task status changes to "Complete"
**And** a completedAt timestamp is recorded in IndexedDB (FR-1.2)
**And** the task visually indicates completion (strikethrough, muted color)
**And** clicking the checkbox again toggles the task back to "In Progress"
**And** the completion state persists across browser sessions
**And** completed tasks remain visible in the list (not hidden)
**And** the UI update happens within 100ms (NFR-P1)
