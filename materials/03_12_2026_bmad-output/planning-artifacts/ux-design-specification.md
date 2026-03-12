---
stepsCompleted: ['step-01-init', 'step-02-discovery', 'step-03-core-experience', 'step-04-emotional-response', 'step-05-interaction-patterns', 'step-06-visual-design', 'step-07-complete']
inputDocuments: 
  - '/Users/sumil/Documents/repos/bmad-todo-workshop/_bmad-output/planning-artifacts/prd.md'
---

# UX Design Specification bmad-todo-workshop

**Author:** Sumil
**Date:** 2026-03-12T14:57:37.291Z

---

## Executive Summary

### Project Vision

bmad-todo-workshop is an intelligent task management web application designed to prevent deadline disasters through predictive analytics rather than nagging about what's already overdue. The core insight addresses "reminder blindness" - when traditional task apps create notification fatigue by alerting users to problems after it's too late to fix them. This app intervenes at the right moment with high-confidence predictions before problems cascade, creating a sense of relief and control rather than stress and overwhelm.

### Target Users

**Primary User:** Sumil - personal productivity tool for a developer/knowledge worker
**User Context:** Managing tasks with dependencies and deadlines where slips have cascading impacts
**User Pain:** "Did I forget something?" anxiety combined with reminder fatigue from traditional task apps
**Success Outcome:** 70% reduction in overdue tasks within 4 weeks of use

### Key Design Challenges

1. **Balancing Intelligence with Control**
   - The app makes predictions, but users need to feel in control, not managed by an AI
   - Show confidence levels without adding cognitive load
   - Maintain trust when predictions are occasionally wrong

2. **Progressive Disclosure vs. Simplicity**
   - Simple tasks must stay simple (title + deadline only)
   - Complex tasks need dependency tracking, progress updates, and rich context
   - Keep default experience clean while making power features discoverable

3. **Anxiety Reduction Through Design**
   - Avoid traditional red/urgent visual language that creates stress
   - "Focus Today/On Track/Blocked" segmentation needs calm, confident visual design
   - Warnings must feel helpful, not alarming

4. **Trust Building During Bootstrap**
   - First 2 weeks, predictions won't be accurate (need 10+ completed tasks for pattern recognition)
   - Set expectations and build trust during the learning period

### Design Opportunities

1. **The "Morning Relief" Experience**
   - Opening the app creates clarity, not overwhelm
   - Dashboard design shows what matters right now with visual hierarchy that guides attention without stress

2. **Intelligent Intervention Moments**
   - The "magic moment" when a prediction saves users from disaster
   - Present warnings to create gratitude, not frustration
   - Timely, accurate, actionable interventions

3. **Keyboard-First Power User Flow**
   - Ctrl+K quick-add enables fluid, flow-state experience
   - Minimal mouse dependency for core workflows
   - Progressive enhancement for power users

## Core User Experience

### Defining Experience

The core user experience centers on two primary interactions:

**Primary Action:** Morning dashboard check - opening the app to see "what needs my attention today" with immediate clarity through Focus Today/On Track/Blocked segmentation.

**Critical Interaction:** The intelligent warning moment - when the app surfaces a high-confidence prediction that prevents a deadline disaster. This interaction determines whether users trust and rely on the system or abandon it.

### Platform Strategy

**Primary Platform:** Web application (SPA)
- Desktop-first design (1280px+ optimized, responsive down to 768px)
- Keyboard-first interaction model for power users
- Modern evergreen browsers only (Chrome, Firefox, Safari, Edge)
- Client-side only in MVP (browser storage, no offline sync)

**Interaction Model:**
- Keyboard shortcuts as primary input method (Ctrl+K quick-add, arrow navigation)
- Progressive enhancement - works with mouse, optimized for keyboard
- Real-time UI updates within single session

### Effortless Interactions

**Zero-Friction Task Creation:**
- Ctrl+K shortcut accessible from anywhere in app
- Natural language deadline parsing ("tomorrow", "Friday", "March 15")
- 5-second task creation without breaking flow state
- Optional fields remain optional - no forced data entry

**Automatic Intelligence:**
- Pattern analysis runs in background without user intervention
- Predictions recalculate after task updates (< 500ms)
- Dependency risk detection triggers automatically when tasks are linked
- Bootstrap progress indicator shows path to prediction accuracy ("10 more completed tasks to unlock predictions")

**At-A-Glance Status:**
- Visual segmentation makes priorities obvious without reading
- Focus Today limited to 2-3 items maximum
- No scrolling required to see critical information

### Critical Success Moments

**1. The Grateful Warning (Make-or-Break Moment)**
- Timing: Intervention occurs 1-2 days before potential slip, not after
- Content: "Task X needs attention now - will slip and block 3 others by Friday"
- Presentation: Helpful tone, clear explanation, actionable guidance
- Outcome: User feels grateful for early warning, not annoyed by nagging

**2. Morning Relief Experience**
- Opening dashboard immediately provides clarity and confidence
- Calm, clean visual design reduces anxiety rather than adds to it
- User knows exactly what to focus on without decision paralysis

**3. First Accurate Prediction (Trust Threshold)**
- Occurs around week 2-3 after 10-15 completed tasks
- Prediction is demonstrably correct - validates the system
- User transitions from skeptical testing to confident reliance

### Experience Principles

1. **Intelligence Serves, Doesn't Control** - Predictions are suggestions from a helpful colleague, not commands from an overbearing manager. User maintains final control.

2. **Effortless by Default, Powerful on Demand** - Simple tasks stay simple (Ctrl+K, title, deadline). Complex features available but never forced through progressive disclosure.

3. **Calm Confidence Over Urgent Anxiety** - Visual design creates clarity and relief, avoiding red alerts or panic-inducing UI patterns.

4. **Gratitude-Inducing Interventions** - Warnings are timely, accurate, actionable, and prevent disasters before they cascade.

5. **Trust Through Transparency** - Show prediction reasoning, confidence levels, and impact. Set clear expectations during bootstrap learning period.

## Emotional Response & Visual Design

### Target Emotional Response

**Primary Emotions:**
- **Relief** - Morning dashboard check creates sense of "I've got this"
- **Confidence** - Trust in intelligent predictions reduces decision anxiety
- **Gratitude** - Timely warnings feel helpful, not intrusive
- **Control** - User maintains agency despite intelligent assistance

**Avoid These Emotions:**
- Stress/anxiety from traditional red-alert urgent UI
- Overwhelm from too many notifications or options
- Distrust from opaque or inaccurate predictions
- Powerlessness from feeling managed by AI

### Visual Design Direction

**Design Language: Calm Confidence**

**Color Strategy:**
- **Primary**: Cool blues/teals for calm, trusted intelligence
- **Success/On Track**: Soft greens, not aggressive
- **Focus Today**: Warm amber/gold for attention without alarm
- **Blocked**: Neutral grays, not red
- **Avoid**: Red for warnings (use amber with high contrast)

**Typography:**
- Clean, modern sans-serif (Inter, SF Pro, or similar)
- Clear hierarchy without bold overuse
- Generous whitespace for breathing room

**Visual Patterns:**
- Segmented dashboard with clear boundaries
- Subtle depth through shadows, not heavy borders
- Progressive disclosure - reveal complexity on demand
- Icon language for status at a glance

### Interaction Patterns

**Task Creation:**
- Ctrl+K command palette overlay (centered, 600px max width)
- Natural language input with inline preview
- Minimal confirmation - instant creation

**Dashboard Layout:**
- Three-column segmentation: Focus Today | On Track | Blocked
- Sticky intelligent warning banner at top when needed
- Collapsible sections for space management

**Intelligent Warnings:**
- Non-modal notifications (top banner, dismissible)
- Explanation expandable on click
- Show confidence indicator subtly (dot color/icon)
- One warning per task maximum per day

**Task Detail View:**
- Inline expansion (no modals for simple edits)
- Progressive disclosure for dependencies, notes
- Quick actions accessible via keyboard shortcuts

**Bootstrap Experience:**
- Progress indicator showing path to predictions
- Educational tooltips during first week
- Celebrate milestone at 10 completed tasks

## Component Specifications

### Dashboard Segments

**Focus Today:**
- Maximum 2-3 items
- Warm amber background (#FFF4E6 or similar)
- Sort by urgency + dependency impact
- Show deadline proximity

**On Track:**
- Soft green indicator (#E8F5E9 or similar)
- Collapsed by default with count
- Expandable to see full list

**Blocked:**
- Gray/neutral tone (#F5F5F5)
- Show blocking task name
- Visual connection to blocker

### Intelligent Warning Banner

**Structure:**
- Top of dashboard, full width
- Amber background, not red
- Dismissible with 'x' (remembers for 24h)
- Icon + Title + Explanation + Action

**Example:**
```
⚠️ Task needs attention: "Client demo prep"
Current progress suggests 2 more days needed, deadline Friday.
Blocks: "Schedule demo call", "Create demo data"
[View Task] [Adjust Deadline]
```

### Quick-Add Modal (Ctrl+K)

**Structure:**
- Centered overlay (600px width)
- Single input field with natural language parsing
- Preview of parsed deadline below input
- Optional: Recent tasks suggestion dropdown

**Keyboard Flow:**
- Type task name and deadline
- Enter to create
- Escape to dismiss

### Task List Item

**Compact View:**
- Checkbox | Title | Deadline | Status indicator
- Hover reveals quick actions (edit, delete, dependency)
- Click expands inline for details

**Expanded View:**
- All metadata visible
- Dependency selector dropdown
- Progress status toggle
- Notes field (if added)

## Implementation Notes

**Accessibility:**
- Keyboard navigation for all actions
- Focus indicators on interactive elements
- Semantic HTML structure
- ARIA labels for screen readers (post-MVP)

**Performance:**
- Render < 1 second for 100 tasks
- Real-time updates < 500ms
- Optimistic UI updates for instant feedback

**Responsive Breakpoints:**
- Desktop: 1280px+ (optimal)
- Tablet: 768-1279px (functional)
- Mobile: < 768px (basic support)

## Design Delivery

**Deliverables for Architecture/Development:**
1. Dashboard wireframes (3-column layout)
2. Component states (default, hover, active, disabled)
3. Color palette with accessibility contrast ratios
4. Typography scale and usage guidelines
5. Interaction flow diagrams for key moments
6. Bootstrap experience progression

**Design Decisions Summary:**
- Calm confidence over urgent anxiety
- Progressive disclosure for complexity
- Keyboard-first power user optimization
- Gratitude-inducing intelligent interventions
- Trust through transparency and education

<!-- UX design content will be appended sequentially through collaborative workflow steps -->
