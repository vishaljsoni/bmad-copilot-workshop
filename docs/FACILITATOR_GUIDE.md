# Facilitator Guide: BMad Method Workshop

## Pre-Workshop Checklist (1 Week Before)

### Technical Preparation

- [ ] **Dry run the entire workshop** yourself (complete all 4 exercises)
- [ ] **Test BMad installation** on your machine with clean state
- [ ] **Verify GitHub Copilot** is responding correctly to `@bmad` agents
- [ ] **Create backup artifacts** (PRD, Architecture files, Component specs)
- [ ] **Prepare demo repository** with completed to-do app
- [ ] **Download videos** locally in case of internet issues
  - BMad V6 Overview (12 min)
  - Complete Masterclass (42 min)

### Materials Preparation

- [ ] **Print Quick Reference Cards** (1 per participant)
- [ ] **Test screen sharing** with VS Code and Copilot Chat visible
- [ ] **Prepare breakout room assignments** (6-7 rooms, 4-6 people each)
- [ ] **Create shared folder** for distributing backup files
- [ ] **Set up feedback form** for post-workshop survey

### Communication

- [ ] **Send pre-workshop email** with setup instructions (see template below)
- [ ] **Confirm facilitator assignments** for each breakout room
- [ ] **Brief facilitators** on timing, exercises, and common issues
- [ ] **Test video conferencing** platform with breakout room features

---

## Workshop Timeline (2 Hours Total)

### Main Session: Introduction (0:00 - 0:15)

**Format:** All participants together

| Time | Activity | Notes |
|------|----------|-------|
| 0:00-0:03 | Welcome & Logistics | Introduce facilitators, confirm everyone has Copilot active |
| 0:03-0:08 | BMad Method Overview | 4-phase workflow, context engineering concept |
| 0:08-0:12 | Agent Roster Introduction | PM, Architect, Developer agents and their roles |
| 0:12-0:15 | Exercise 1 Demo | Live install `npx bmad-method install` |

**Key Talking Points:**
- "BMad solves the context problem: how do AI agents share understanding?"
- "Four phases: Analysis → Planning → Solutioning → Implementation"
- "Documents are the context layer between agents"
- "You'll build a real app in 90 minutes using only Copilot Chat"

---

### Breakout Session 1: Installation (0:15 - 0:25)

**Format:** 6-7 breakout rooms with facilitators

**Exercise 1: BMad Installation**

Participants will:
1. Create empty folder: `bmad-todo-workshop`
2. Run: `npx bmad-method install`
3. Select: "React + TypeScript + TailwindCSS"
4. Verify: `.bmad/` folder exists with agents and templates

**Facilitator Actions:**
- Monitor chat for error messages
- Check everyone sees "Installation complete" message
- Troubleshoot npm version issues (see Troubleshooting section)
- Share backup `.bmad/` folder if anyone fails installation

**Success Criteria:**
- `.bmad/` folder exists
- `agents/`, `prompts/`, `templates/` subfolders present
- GitHub Copilot recognizes `@bmad` when typed in Chat

**Common Issues:**
- Old npm version: `npm install -g npm@latest`
- Permission errors: Run terminal as Administrator
- Copilot not seeing agents: Restart VS Code

---

### Breakout Session 2: PRD Creation (0:25 - 0:50)

**Format:** 6-7 breakout rooms

**Exercise 2: Product Requirements Document**

Participants will:
1. Open Copilot Chat
2. Type: `@bmad /start-work`
3. Respond to PM agent questions:
   - Project name: "To-Do App"
   - Description: "Simple task manager with add, complete, delete"
   - Confirm scope: "Yes, start with core features"
4. Review generated `docs/prd.md`

**Facilitator Actions:**
- **Critical:** The PM agent asks MANY questions. Guide participants to:
  - **Keep answers concise** (1-2 sentences)
  - **Accept default suggestions** when offered
  - **Say "Yes, proceed" when asked for confirmation**
- Watch for "context pollution" (agent repeating questions)
  - Fix: Type `/clear` and restart with `@bmad /start-work`
- Share pre-built PRD if anyone falls >5 minutes behind

**Success Criteria:**
- `docs/prd.md` file created
- Contains: Project Overview, Core Features, User Stories
- User stories follow format: "As a [role], I want [action], so that [benefit]"

**Expected PRD Content:**
```markdown
# To-Do App - Product Requirements Document

## Project Overview
Simple task management application allowing users to create, complete, and delete tasks.

## Core Features
- Add new tasks
- Mark tasks as complete
- Delete tasks
- View task list

## User Stories
1. As a user, I want to add tasks so that I can track what I need to do
2. As a user, I want to mark tasks complete so that I can see my progress
3. As a user, I want to delete tasks so that I can remove items I no longer need
```

**Timing Risk:** This exercise can run long. If >30 minutes:
- **Stop new PRD work**
- **Distribute pre-built PRD** to everyone
- **Move to Exercise 3**

---

### Breakout Session 3: Architecture Design (0:50 - 1:15)

**Format:** 6-7 breakout rooms

**Exercise 3: Technical Architecture**

Participants will:
1. Type: `@bmad /arch`
2. Architect agent reads PRD and asks clarifying questions:
   - State management: "Yes, use React Context"
   - Styling: "Yes, TailwindCSS is fine"
   - Data persistence: "Local storage for now"
3. Review generated `docs/architecture.md` and `docs/adrs/`

**Facilitator Actions:**
- Explain ADRs (Architecture Decision Records) concept
- Point out key sections: Component Structure, Data Flow, Tech Stack
- Verify everyone has both `architecture.md` and at least 1 ADR file
- Share pre-built architecture if anyone is blocked

**Success Criteria:**
- `docs/architecture.md` exists with component diagrams
- `docs/adrs/` folder contains decision records
- Participants understand the component hierarchy

**Expected Architecture:**
```markdown
# Architecture

## Component Structure
```
App
├── TodoList
│   ├── TodoItem
│   └── TodoItem
└── AddTodoForm
```

## State Management
- React Context for global todo state
- Local state for form inputs

## Data Flow
1. User adds task via AddTodoForm
2. Context updates todo array
3. TodoList re-renders with new item
```

---

### Breakout Session 4: Build First Story (1:15 - 1:45)

**Format:** 6-7 breakout rooms

**Exercise 4: Implement First User Story**

Participants will:
1. Type: `@bmad /start-story`
2. Developer agent shows story list, select: "Add new tasks"
3. Agent asks implementation questions:
   - Component name: "AddTodoForm"
   - Validation: "Yes, prevent empty tasks"
   - Submit behavior: "Clear input after add"
4. Agent generates:
   - `src/components/AddTodoForm.tsx`
   - `src/components/AddTodoForm.test.tsx`
   - Updated `src/App.tsx`
5. Test the component:
   ```bash
   npm install
   npm run dev
   ```

**Facilitator Actions:**
- **Most important exercise** - this is where "magic" happens
- Walk through generated code:
  - State management
  - Event handlers
  - TypeScript types
  - TailwindCSS styling
- Explain: "The agent read PRD + Architecture to generate this"
- Help debug if component doesn't render
- Emphasize: "Context engineering = better code generation"

**Success Criteria:**
- Component file created with proper TypeScript types
- Test file generated
- Component renders in browser
- Can type in input and click "Add" button

**Expected Component Code:**
```typescript
// src/components/AddTodoForm.tsx
import React, { useState } from 'react';

interface AddTodoFormProps {
  onAddTodo: (text: string) => void;
}

export const AddTodoForm: React.FC<AddTodoFormProps> = ({ onAddTodo }) => {
  const [text, setText] = useState('');

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    if (text.trim()) {
      onAddTodo(text);
      setText('');
    }
  };

  return (
    <form onSubmit={handleSubmit} className="mb-4">
      <input
        type="text"
        value={text}
        onChange={(e) => setText(e.target.value)}
        placeholder="Add a new task..."
        className="border rounded px-4 py-2 w-full"
      />
      <button type="submit" className="mt-2 bg-blue-500 text-white px-4 py-2 rounded">
        Add Task
      </button>
    </form>
  );
};
```

**Common Issues:**
- "npm run dev fails": Check Node.js version
- "Component doesn't appear": Verify import in App.tsx
- "TypeScript errors": Agent might need to regenerate types

---

### Main Session: Wrap-Up (1:45 - 2:00)

**Format:** All participants together

| Time | Activity | Notes |
|------|----------|-------|
| 1:45-1:50 | Demo Completed App | Screen share working to-do app with all features |
| 1:50-1:55 | Key Takeaways | Reinforce 4-phase workflow, context engineering |
| 1:55-2:00 | Q&A & Next Steps | Share resources, encourage continued practice |

**Key Takeaways to Emphasize:**
1. **Documents create shared context** between agents
2. **Structured workflow** beats ad-hoc prompting
3. **Each agent has a role**: PM plans, Architect designs, Developer builds
4. **Context engineering >> Prompt engineering** for complex projects
5. **BMad scales** - same workflow for larger applications

**Next Steps for Participants:**
- Complete remaining user stories (mark complete, delete)
- Try BMad on their own project
- Join BMad community Discord
- Watch full masterclass video (42 min)

---

## Facilitator Tips

### Timing Management

**Exercise 2 (PRD) is the biggest timing risk** because the PM agent is conversational.

**If running behind:**
- At 0:40, check progress
- If <50% have PRD, **distribute pre-built PRD to everyone**
- Say: "Let's move together to Architecture so we can finish the build"

**Buffer time built into Exercise 4:**
- Actual coding: 20 minutes
- Built-in slack: 10 minutes for installs, debugging

### Breakout Room Management

**Facilitator Roles:**
- **Monitor** chat for questions
- **Respond** quickly to errors
- **Check in** with quiet participants
- **Share screen** to demonstrate if needed
- **Distribute backup files** to anyone blocked

**Communication:**
- Use main channel for urgent announcements
- Pin important links in each room
- Have co-facilitator monitor main room chat

### Backup Plans

**If installation fails entirely:**
- Share pre-configured `.bmad/` folder
- Participants can copy into their project
- Restart VS Code to recognize agents

**If agent isn't responding:**
1. Check Copilot subscription is active
2. Restart VS Code
3. Type `@workspace /help` to verify Copilot is working
4. If still broken, pair participant with neighbor

**If >30% of room is stuck:**
- **Switch to demo mode**
- Screen share your working example
- Walk through the output documents
- Participants can try on their own after workshop

---

## Pre-Workshop Email Template

**Subject:** BMad Method Workshop - Pre-Event Setup (Action Required)

---

Hi [Participant Name],

Thanks for registering! To maximize hands-on learning time, please complete this **15-minute setup** before [Workshop Date].

### Required Setup

**1. Install Node.js v20+**
- Download: https://nodejs.org/
- Verify: `node --version` (should show v20.x.x+)

**2. Verify GitHub Copilot**
- Open VS Code
- Open Copilot Chat (Ctrl+Shift+I or Cmd+Shift+I)
- Type: `@workspace /help`
- You should see a response

**3. Create Workshop Folder**
```bash
mkdir bmad-todo-workshop
cd bmad-todo-workshop
```

**Don't install BMad yet** - we'll do this together!

### Optional: Prep Reading (30 min)

- Watch: [BMad V6 Overview](https://www.youtube.com/watch?v=4VPoGSeI2sw) (12 min)
- Skim: [BMad README](https://github.com/bmad-code-org/BMAD-METHOD)

### Workshop Details

**Date:** [Date]  
**Time:** [Time] ([Timezone])  
**Duration:** 2 hours  
**Link:** [Meeting Link]

You'll receive:
- Breakout room assignments at start
- All materials via shared folder
- Backup files if setup issues arise

### Questions?

Reply to this email or join our Discord: [Link]

---

Looking forward to seeing you!

Best,  
[Your Name]

---

## Post-Workshop Follow-Up

**Send within 24 hours:**

1. **Thank you email** with:
   - Link to workshop recording
   - GitHub repo with completed app
   - Feedback survey
   - Office hours schedule

2. **Resources:**
   - BMad Discord community invite
   - Advanced tutorials
   - Recommended next projects

3. **Feedback Collection:**
   - What worked well?
   - What was confusing?
   - Timing feedback
   - Technical difficulties

---

## Success Metrics

**Minimum Success:**
- 70% complete Exercise 1 (installation)
- 60% complete Exercise 2 (PRD)
- 50% reach Exercise 4 (building code)

**Good Success:**
- 90% complete installation
- 80% have PRD and Architecture
- 70% generate working component

**Excellent Success:**
- 95%+ complete all exercises
- Participants build 2+ components
- Positive feedback on survey

---

## Emergency Contacts

**Technical Support:**
- BMad Discord: [Link]
- GitHub Issues: https://github.com/bmad-code-org/BMAD-METHOD/issues

**Facilitator Coordination:**
- Slack channel: #bmad-workshop
- Emergency contact: [Phone]

---

Good luck with your workshop! 🚀