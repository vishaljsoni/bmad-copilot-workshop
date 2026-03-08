# Troubleshooting Guide

Common issues and solutions for the BMad Method workshop.

---

## Installation Issues

### "npx: command not found"

**Cause:** Node.js/npm not installed or not in PATH

**Solutions:**
1. Install Node.js from https://nodejs.org/ (v20+)
2. Restart terminal after installation
3. Verify: `node --version` and `npm --version`

**Windows specific:**
- Restart VS Code and terminal
- Check Environment Variables → System → Path includes Node.js

**Mac specific:**
- If using Homebrew: `brew install node@20`
- Add to PATH: `echo 'export PATH="/usr/local/opt/node@20/bin:$PATH"' >> ~/.zshrc`

---

### "Permission denied" during installation

**Cause:** Insufficient permissions to create folders

**Solutions:**

**Windows:**
```bash
# Run terminal as Administrator
# Right-click PowerShell → Run as Administrator
npx bmad-method install
```

**Mac/Linux:**
```bash
# Option 1: Use sudo (not recommended)
sudo npx bmad-method install

# Option 2: Fix npm permissions (recommended)
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.zshrc
source ~/.zshrc
```

---

### "Installation hangs" or "No response"

**Cause:** Network issues or npm cache problems

**Solutions:**

1. **Check internet connection**
   - BMad downloads packages during install
   - Test: `ping npmjs.org`

2. **Clear npm cache**
   ```bash
   npm cache clean --force
   npx bmad-method install
   ```

3. **Cancel and retry**
   ```bash
   # Press Ctrl+C to cancel
   # Wait 5 seconds
   npx bmad-method install
   ```

4. **Use verbose logging**
   ```bash
   npx bmad-method install --verbose
   # Sends more detailed logs to help debug
   ```

---

### "Old npm version" warning

**Message:** `Your npm version is outdated. Please update to npm 9.0+`

**Solution:**
```bash
npm install -g npm@latest
node --version
npm --version  # Should show 9.x.x or higher
```

**If update fails:**
```bash
# Windows: Use Node.js installer (includes latest npm)
# Mac: brew upgrade node
# Linux: Use nvm (Node Version Manager)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
nvm install 20
nvm use 20
```

---

## GitHub Copilot Issues

### "@bmad not recognized" in Copilot Chat

**Cause:** Copilot hasn't indexed the `.bmad/` folder yet

**Solutions:**

1. **Restart VS Code**
   - Close and reopen VS Code
   - Wait 10 seconds for indexing
   - Try `@bmad /help`

2. **Reindex workspace**
   ```
   # In Copilot Chat:
   @workspace /reindex
   ```

3. **Verify .bmad folder exists**
   ```bash
   ls -la .bmad/
   # Should show: agents/, prompts/, templates/
   ```

4. **Reinstall BMad**
   ```bash
   rm -rf .bmad/
   npx bmad-method install
   ```

---

### "Copilot subscription not active"

**Cause:** GitHub Copilot license expired or not assigned

**Solutions:**

1. **Check subscription**
   - Go to https://github.com/settings/copilot
   - Verify subscription status is "Active"

2. **Reauthorize VS Code**
   - VS Code → Extensions → GitHub Copilot
   - Click "Sign out" then "Sign in"
   - Authorize with GitHub

3. **Contact workshop organizer**
   - They may have trial licenses available

---

### "Copilot gives generic responses" (not using BMad context)

**Cause:** Agent context not being read properly

**Solutions:**

1. **Use exact command syntax**
   - Correct: `@bmad /start-work`
   - Wrong: `@bmad start work` (missing slash)
   - Wrong: `bmad /start-work` (missing @)

2. **Clear chat and restart**
   ```
   /clear
   @bmad /start-work
   ```

3. **Verify agents are loaded**
   ```
   @bmad /help
   # Should show list of available commands
   ```

---

## Agent Workflow Issues

### PM Agent: "Keeps asking same question in a loop"

**Cause:** Context pollution - agent lost track of conversation

**Solutions:**

1. **Clear and restart**
   ```
   /clear
   @bmad /start-work
   ```

2. **Give more direct answers**
   - Instead of: "I think maybe we could have..."
   - Say: "Yes, add those features"

3. **Use backup PRD**
   - Ask facilitator for pre-built `prd.md`
   - Copy to `docs/prd.md`
   - Continue to next exercise

---

### Architect Agent: "Can't find PRD file"

**Cause:** PRD file doesn't exist or is in wrong location

**Solutions:**

1. **Verify PRD exists**
   ```bash
   ls docs/prd.md
   # Should show: docs/prd.md
   ```

2. **Check file content**
   ```bash
   cat docs/prd.md
   # Should contain: Project Overview, Features, User Stories
   ```

3. **Reindex and retry**
   ```
   @workspace /reindex
   @bmad /arch
   ```

4. **Use backup PRD**
   - Copy pre-built PRD to `docs/prd.md`
   - Try `@bmad /arch` again

---

### Developer Agent: "Generated code has TypeScript errors"

**Cause:** Type mismatches or missing imports

**Solutions:**

1. **Check if code actually runs**
   ```bash
   npm run dev
   # If it works in browser, TS errors may be false positives
   ```

2. **Ask agent to fix**
   ```
   The AddTodoForm component has TypeScript errors. Can you fix them?
   ```

3. **Verify tsconfig.json exists**
   ```bash
   ls tsconfig.json
   # If missing, run: npx tsc --init
   ```

4. **Restart TypeScript server in VS Code**
   - Cmd/Ctrl + Shift + P
   - Type: "TypeScript: Restart TS Server"

---

### Developer Agent: "Component doesn't render in browser"

**Cause:** Missing import or incorrect component usage

**Solutions:**

1. **Check component is imported in App.tsx**
   ```typescript
   // Should see:
   import { AddTodoForm } from './components/AddTodoForm';
   ```

2. **Check component is rendered**
   ```typescript
   // Should see in return statement:
   <AddTodoForm onAddTodo={handleAddTodo} />
   ```

3. **Check browser console for errors**
   - Press F12 to open DevTools
   - Look in Console tab for error messages
   - Share errors with facilitator

4. **Verify npm run dev is running**
   ```bash
   # Should see:
   VITE v5.x.x  ready in XXX ms
   ➜  Local:   http://localhost:5173/
   ```

---

## Development Server Issues

### "npm run dev" fails with "command not found"

**Cause:** Dependencies not installed

**Solution:**
```bash
npm install
npm run dev
```

---

### "Port 5173 already in use"

**Cause:** Another dev server is running

**Solutions:**

1. **Kill existing process**
   ```bash
   # Find process using port 5173
   # Mac/Linux:
   lsof -ti:5173 | xargs kill -9
   
   # Windows:
   netstat -ano | findstr :5173
   taskkill /PID <PID> /F
   ```

2. **Use different port**
   ```bash
   npm run dev -- --port 5174
   ```

---

### "Module not found" errors

**Cause:** Missing dependencies or incorrect imports

**Solutions:**

1. **Reinstall dependencies**
   ```bash
   rm -rf node_modules package-lock.json
   npm install
   ```

2. **Check import paths**
   ```typescript
   // Correct:
   import { AddTodoForm } from './components/AddTodoForm';
   
   // Wrong:
   import { AddTodoForm } from './components/AddTodoForm.tsx'; // Don't include .tsx
   import { AddTodoForm } from 'components/AddTodoForm'; // Missing ./
   ```

3. **Install specific missing package**
   ```bash
   npm install <package-name>
   ```

---

## File System Issues

### "Cannot create folder" or "EACCES: permission denied"

**Cause:** File system permissions

**Solutions:**

**Windows:**
- Right-click project folder → Properties → Security
- Ensure your user has "Full Control"
- Run terminal as Administrator

**Mac/Linux:**
```bash
# Change ownership of project folder
sudo chown -R $USER:$USER .

# Or work in home directory
cd ~
mkdir bmad-todo-workshop
cd bmad-todo-workshop
```

---

### "Files not appearing" in VS Code explorer

**Cause:** VS Code not refreshing file tree

**Solutions:**

1. **Manual refresh**
   - Click refresh icon in Explorer pane
   - Or: Cmd/Ctrl + R

2. **Restart VS Code**
   - Close and reopen
   - Files should appear

3. **Check .gitignore**
   ```bash
   cat .gitignore
   # Make sure it's not hiding your files
   ```

---

## General Tips

### "I'm falling behind the group"

**Don't panic! Here's what to do:**

1. **Signal your facilitator** in chat
2. **Use backup artifacts**:
   - Skip to completed PRD
   - Skip to completed Architecture
   - Jump to Exercise 4 with pre-built docs
3. **Follow along** with facilitator's screen share
4. **Try on your own** after workshop with recordings

---

### "My output looks different from the demo"

**This is normal!** AI agents generate variations.

**What matters:**
- Does PRD have user stories? ✅
- Does Architecture have component structure? ✅
- Does component render in browser? ✅

Exact code/wording differences are fine.

---

### "Agent gives unexpected response"

**Strategies:**

1. **Rephrase your request**
   ```
   # Instead of: "Make it better"
   # Try: "Add validation to prevent empty tasks"
   ```

2. **Be more specific**
   ```
   # Instead of: "Create the component"
   # Try: "Create AddTodoForm component with TypeScript and TailwindCSS"
   ```

3. **Reference the docs**
   ```
   "Based on the PRD in docs/prd.md, create the add task feature"
   ```

4. **Clear and start fresh**
   ```
   /clear
   @bmad /start-story
   ```

---

## Still Stuck?

### During Workshop

1. **Ask in breakout room chat** - facilitator is monitoring
2. **Share your screen** with facilitator
3. **Pair with a neighbor** who's ahead
4. **Use backup files** to catch up

### After Workshop

1. **Review workshop recording**
2. **Join BMad Discord** - community is helpful
3. **Open GitHub issue** on BMad repo
4. **Email workshop organizer**

---

## Emergency Backup Plan

**If nothing works and you can't continue:**

1. **Switch to observer mode**
   - Watch facilitator's screen
   - Take notes on the workflow
   - Try at home with fresh install

2. **Get backup materials**
   - Completed PRD
   - Completed Architecture
   - Completed components
   - Full project ZIP file

3. **Schedule 1-on-1 help**
   - Office hours after workshop
   - Debug your specific setup

---

## Logging Issues for Future Workshops

**Help us improve! If you hit an issue:**

1. **Take screenshots**
2. **Copy error messages**
3. **Note your OS and Node.js version**
4. **Share in feedback survey**

**Common info to include:**
- OS: Windows 11 / macOS Sonoma / Ubuntu 22.04
- Node.js: `node --version`
- npm: `npm --version`
- VS Code: Help → About
- Error message: Full text from terminal

---

## Quick Diagnostic Checklist

Before asking for help, verify:

```bash
# 1. Node.js installed and correct version
node --version
# Expected: v20.x.x or higher

# 2. npm works
npm --version
# Expected: 9.x.x or higher

# 3. BMad folder exists
ls -la .bmad/
# Expected: agents/, prompts/, templates/

# 4. Copilot is active
# In VS Code Copilot Chat: @workspace /help
# Expected: Response from Copilot

# 5. PRD file exists (after Exercise 2)
ls docs/prd.md
# Expected: docs/prd.md

# 6. Dependencies installed (after Exercise 4)
ls node_modules/
# Expected: Many folders (react, typescript, etc.)
```

**Share this checklist output with your facilitator for faster debugging!**

---

**Remember:** Technical issues happen. The goal is learning the workflow, not having a perfect setup. Even if you hit problems, you're gaining valuable experience with AI-driven development!