# Pre-Workshop Setup Instructions

## ⏱️ Time Required: 15 minutes

**Complete this setup BEFORE the workshop to maximize hands-on learning time!**

---

## Step 1: Install Node.js (5 minutes)

### Why?
BMad framework requires Node.js to run.

### Instructions

**1. Download Node.js v20+**
- Visit: https://nodejs.org/
- Click: "Download LTS" (green button)
- Version should be 20.x.x or higher

**2. Install**
- Run the downloaded installer
- Follow default options
- **Windows:** Allow installer to add to PATH
- **Mac:** May need to enter password

**3. Verify Installation**

Open a **new** terminal window and run:

```bash
node --version
```

**Expected output:** `v20.11.0` (or higher)

If you see a version number, you're good! ✅

### Troubleshooting

**"command not found"**
- **Windows:** Restart computer, then try again
- **Mac:** Check if using Homebrew - run `brew install node@20`
- **Linux:** Use nvm (see nvm setup guide)

---

## Step 2: Verify GitHub Copilot (3 minutes)

### Why?
BMad agents run through GitHub Copilot Chat in VS Code.

### Instructions

**1. Open VS Code**

**2. Open Copilot Chat**
- **Windows:** Press `Ctrl+Shift+I`
- **Mac:** Press `Cmd+Shift+I`
- **Or:** Click chat icon in sidebar

**3. Test Copilot**

Type in the chat:
```
@workspace /help
```

**Expected:** You should see a response from Copilot listing available commands.

If you see a response, you're good! ✅

### Troubleshooting

**"GitHub Copilot is not active"**
1. Go to https://github.com/settings/copilot
2. Verify subscription status shows "Active"
3. If not active, contact workshop organizer

**"Sign in required"**
1. Click "Sign in" in VS Code
2. Authorize with your GitHub account
3. Try `@workspace /help` again

**Still not working?**
- Restart VS Code
- Check Extensions panel → GitHub Copilot is enabled
- Contact workshop organizer for trial license

---

## Step 3: Check Disk Space (1 minute)

### Why?
BMad installation downloads agent files and dependencies (~500MB).

### Instructions

**Check free space:**

**Windows:**
1. Open File Explorer
2. Click "This PC"
3. Check free space on C: drive
4. Need: 500MB+ free

**Mac:**
1. Apple menu → About This Mac
2. Click "Storage" tab
3. Need: 500MB+ free

**Linux:**
```bash
df -h
```

If you have >500MB free, you're good! ✅

---

## Step 4: Create Project Folder (2 minutes)

### Why?
We'll install BMad fresh during the workshop.

### Instructions

**1. Open Terminal/Command Prompt**

**2. Navigate to where you keep code projects**
```bash
cd ~/Documents  # Mac/Linux
cd C:\Users\YourName\Documents  # Windows
```

**3. Create workshop folder**
```bash
mkdir bmad-todo-workshop
```

**4. Verify it exists**
```bash
ls bmad-todo-workshop  # Mac/Linux
dir bmad-todo-workshop  # Windows
```

Folder should be empty. ✅

**⚠️ Important:** Do NOT install BMad yet - we'll do this together during the workshop!

---

## Step 5: Optional Pre-Reading (30 minutes)

### Why?
Familiarize yourself with BMad concepts before the workshop.

### Materials

**Video (12 minutes):**
- [BMad V6 Overview](https://www.youtube.com/watch?v=4VPoGSeI2sw)
- Quick introduction to BMad framework
- Shows 4-phase workflow

**Documentation (15 minutes skim):**
- [BMad README](https://github.com/bmad-code-org/BMAD-METHOD)
- Focus on: "What is BMad?" and "Quick Start" sections
- Don't worry about understanding everything!

**Note:** These are **optional** - we'll cover core concepts during the workshop. But pre-reading helps you get more from the exercises.

---

## ✅ Final Checklist

Before the workshop, verify:

```bash
# 1. Node.js installed
node --version
# Expected: v20.x.x or higher

# 2. npm installed (comes with Node.js)
npm --version
# Expected: 9.x.x or higher
```

In VS Code Copilot Chat:
```
@workspace /help
# Expected: Response listing commands
```

File system:
- [ ] Empty folder created: `bmad-todo-workshop`
- [ ] 500MB+ disk space free
- [ ] VS Code installed and working

**All checks passed? You're ready for the workshop! 🎉**

---

## What to Bring to Workshop

- 💻 **Laptop** with VS Code installed
- 🌐 **Stable internet** (we'll be downloading packages)
- ❓ **Questions** and curiosity!
- ☕ **Optional:** Coffee/tea - it's a 2-hour session

---

## Workshop Day Checklist

**30 minutes before start:**

1. **Test video/audio**
   - Join meeting early
   - Verify mic and camera work
   - Check screen sharing works

2. **Open tools**
   - VS Code with empty `bmad-todo-workshop` folder
   - Terminal window
   - Copilot Chat panel

3. **Close unnecessary apps**
   - Free up memory
   - Reduce distractions

4. **Have backup plan**
   - Mobile hotspot if wifi is unreliable
   - Tablet/phone for viewing shared screens if laptop has issues

---

## Still Have Issues?

### Before Workshop
- **Email:** [Workshop Organizer Email]
- **Discord:** [Discord Link]
- **Office Hours:** [Times if available]

### During Workshop
- **Ask in chat** - facilitators are monitoring
- **Request backup files** if setup fails
- **Pair with neighbor** who's ahead

**Remember:** We have backup plans! Even if setup fails, you can still learn the workflow by following along.

---

## System Requirements

### Minimum
- **OS:** Windows 10+, macOS 10.15+, or Linux (Ubuntu 20.04+)
- **RAM:** 4GB
- **Disk:** 2GB free
- **Internet:** Stable broadband connection
- **VS Code:** Version 1.80+

### Recommended
- **RAM:** 8GB+
- **Disk:** 5GB free (for comfort)
- **Internet:** 10+ Mbps download
- **Screen:** 1920x1080 or higher (to see code + chat + browser)

---

## Advanced Setup (Optional)

### For Faster Installation

If you want to pre-download some packages:

```bash
cd bmad-todo-workshop
npm init -y
npm install react react-dom typescript @types/react @types/react-dom
```

**Note:** This is NOT required - just speeds up Exercise 4 slightly.

### For Offline Usage

If internet might be unstable:

1. Download Node.js installer as backup
2. Clone BMad repo locally: `git clone https://github.com/bmad-code-org/BMAD-METHOD`
3. Ask facilitator for offline install instructions

---

**Questions about setup?** Don't hesitate to reach out before the workshop!

**See you soon! 🚀**