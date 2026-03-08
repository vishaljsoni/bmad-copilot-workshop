# BMad Method Quick Reference Card

## 🛠️ Essential Commands

### Installation
```bash
npx bmad-method install
```

### BMad Agent Commands (in Copilot Chat)

| Command | Purpose | When to Use |
|---------|---------|-------------|
| `@bmad /help` | Show all commands | First step, or when stuck |
| `@bmad /start-work` | Start new project | Begin Exercise 2 (PRD) |
| `@bmad /arch` | Generate architecture | After PRD complete |
| `@bmad /start-story` | Implement user story | After Architecture complete |
| `@bmad /test` | Generate tests | After component created |
| `@bmad /review` | Code review | Before finalizing |

### Copilot Chat Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+Shift+I` (Win) / `Cmd+Shift+I` (Mac) | Open Copilot Chat |
| `/clear` | Clear chat history |
| `@workspace /help` | Test if Copilot is active |
| `@workspace /reindex` | Refresh file index |

---

## 🔄 The 4-Phase Workflow

### 1️⃣ Analysis Phase
**Command:** `@bmad /start-work`  
**Agent:** Product Manager (PM)  
**Output:** `docs/prd.md`  
**Contains:** Features, User Stories, Success Criteria

### 2️⃣ Planning Phase
**Command:** `@bmad /arch`  
**Agent:** Architect  
**Output:** `docs/architecture.md` + `docs/adrs/*.md`  
**Contains:** Component structure, Tech stack, Decision records

### 3️⃣ Solutioning Phase
**Command:** `@bmad /start-story`  
**Agent:** Developer (planning mode)  
**Output:** Component specifications  
**Contains:** Implementation plan, API design

### 4️⃣ Implementation Phase
**Command:** (Continue from step 3)  
**Agent:** Developer (coding mode)  
**Output:** Component files + tests  
**Contains:** Working code, unit tests

---

## 📁 File Structure

```
your-project/
├── .bmad/                  # BMad configuration
│   ├── agents/           # Agent definitions
│   ├── prompts/          # Agent prompts
│   └── templates/        # Code templates
├── docs/
│   ├── prd.md            # Product Requirements
│   ├── architecture.md   # Technical design
│   └── adrs/             # Architecture Decision Records
└── src/
    ├── components/       # Your components
    └── App.tsx           # Main app file
```

---

## 📝 Agent Question Tips

### Product Manager Agent
- **Keep answers short** (1-2 sentences)
- **Accept defaults** when suggested
- **Be specific** about core features
- **Avoid scope creep** (no login, databases, etc.)

### Architect Agent
- **Trust the suggestions** (they're based on best practices)
- **Choose simple options** (Context over Redux, LocalStorage over DB)
- **Ask for explanations** if confused

### Developer Agent
- **Pick clear component names** (AddTodoForm, not Form1)
- **Request validation** when needed
- **Ask for tests** to be included
- **Review code** before accepting

---

## ⚡ Speed Tips

### Faster PRD Creation
```
@bmad /start-work

# When asked for features, give complete list:
"Features: add tasks, mark complete, delete tasks, view list"

# When asked about users:
"Individual users tracking personal tasks"

# When asked about scope:
"Keep it simple - no auth or cloud sync"
```

### Faster Architecture
```
@bmad /arch

# Quick answers:
"Use React Context, LocalStorage, and TailwindCSS"
```

### Faster Story Implementation
```
@bmad /start-story

# Select story number: 1
# Component name: AddTodoForm
# Additional requirements: "Validate input, clear after submit, include tests"
```

---

## 🔴 Common Issues

### Issue: "@bmad not recognized"
**Fix:** Restart VS Code, then try `@bmad /help`

### Issue: PM agent loops same question
**Fix:** Type `/clear` then `@bmad /start-work`

### Issue: "Can't find PRD file"
**Fix:** Verify `docs/prd.md` exists, then `@workspace /reindex`

### Issue: TypeScript errors in generated code
**Fix:** Check if `npm run dev` works - if yes, ignore red squiggles

### Issue: Component doesn't render
**Fix:** Check `App.tsx` has import and `<ComponentName />` tag

---

## 🎯 Success Checklist

### After Exercise 1 (Installation)
- [ ] `.bmad/` folder exists
- [ ] `@bmad /help` shows commands

### After Exercise 2 (PRD)
- [ ] `docs/prd.md` file exists
- [ ] Contains 3+ user stories
- [ ] Stories follow: "As a [role], I want [feature] so that [benefit]"

### After Exercise 3 (Architecture)
- [ ] `docs/architecture.md` exists
- [ ] Contains component diagram
- [ ] At least 2 ADR files in `docs/adrs/`

### After Exercise 4 (Implementation)
- [ ] Component `.tsx` file created
- [ ] Test file `.test.tsx` created
- [ ] `npm run dev` works
- [ ] Component renders in browser

---

## 💡 Key Concepts

### Context Engineering
**Documents create shared understanding between agents.**

PM creates PRD → Architect reads PRD → Developer reads both

Better context = better code generation

### Architecture Decision Records (ADRs)
**Document important technical choices.**

Format:
- **Context:** Why did we need to decide?
- **Decision:** What did we choose?
- **Consequences:** What are the trade-offs?

### Agent Roles
- **PM:** Business requirements, user stories
- **Architect:** Technical design, component structure
- **Developer:** Code generation, implementation
- **QA:** Test generation, quality checks
- **DevOps:** Deployment, CI/CD (advanced)

---

## 🚀 Next Steps

### Complete Your App
```bash
# Implement remaining stories
@bmad /start-story  # Story 2: Mark complete
@bmad /start-story  # Story 3: Delete tasks
```

### Add Features
```bash
# 1. Update PRD with new feature
# 2. Update architecture: @bmad /arch
# 3. Create user story
# 4. Implement: @bmad /start-story
```

### Learn More
- Watch: [BMad Masterclass](https://www.youtube.com/watch?v=LorEJPrALcg) (42 min)
- Read: [BMad Docs](https://docs.bmad-method.org/)
- Join: BMad Discord community

---

## 📞 Emergency Help

**During Workshop:**
- Ask facilitator in breakout room
- Share screen for debugging
- Request backup files

**After Workshop:**
- Review recording
- Join BMad Discord
- Email workshop organizer

---

**Pro Tip:** Save this reference card - you'll use it for every BMad project!

**Remember:** The goal is learning the workflow, not perfect code. Even experienced devs hit issues. Keep practicing! 🚀