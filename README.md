# BMad Method Workshop: GitHub Copilot Advanced

> A 2-hour guided workshop teaching the BMad Method framework for AI-driven development using GitHub Copilot

## 🎯 Workshop Overview

**Duration:** 2 hours  
**Format:** Interactive workshop with breakout rooms  
**Level:** Intermediate to Advanced  
**Tool:** GitHub Copilot (VS Code)

### What You'll Learn

- **The BMad Method Framework**: 4-phase structured approach to AI-driven development
- **Context Engineering**: How to build shared context between AI agents
- **Agent Orchestration**: Using Product Manager, Architect, and Developer agents
- **Hands-On Practice**: Build a complete to-do application from PRD to working code

### What You'll Build

A complete to-do application including:
- Product Requirements Document (PRD)
- Technical Architecture with Decision Records (ADRs)
- Component specifications
- Working React components with TypeScript

## 📋 Prerequisites

### Required

- **Node.js v20+** - [Download here](https://nodejs.org/)
- **VS Code** with **GitHub Copilot** extension active
- **Basic JavaScript/React knowledge**
- **500MB free disk space**

### Verify Your Setup

```bash
# Check Node.js version
node --version  # Should show v20.x.x or higher

# Verify Copilot is active
# In VS Code: Open Copilot Chat (Ctrl+Shift+I or Cmd+Shift+I)
# You should see the Copilot Chat panel open and ready
```

## 🚀 Workshop Agenda

### Part 1: Introduction (15 min)
- BMad Method overview
- 4-phase workflow explanation
- Context vs. Prompt Engineering

### Part 2: Hands-On Exercises (90 min)
**Exercise 1:** BMad Installation & Setup (10 min)  
**Exercise 2:** PRD Creation with PM Agent (25 min)  
**Exercise 3:** Architecture Design with Architect Agent (25 min)  
**Exercise 4:** Build First Story with Developer Agent (30 min)

### Part 3: Wrap-Up (15 min)
- Q&A
- Next steps
- Additional resources

## 📁 Repository Structure

```
bmad-copilot-workshop/
├── README.md                           # This file
├── docs/
│   ├── FACILITATOR_GUIDE.md           # Complete workshop delivery guide
│   ├── PARTICIPANT_GUIDE.md           # Step-by-step exercises for participants
│   └── TROUBLESHOOTING.md             # Common issues and solutions
├── materials/
│   ├── slides/                        # PowerPoint presentation
│   ├── backup-artifacts/              # Pre-built PRD, Architecture files
│   └── QUICK_REFERENCE.md            # Quick reference card
└── examples/
    └── completed-todo-app/            # Finished application for reference
```

## 🎓 For Facilitators

**Start here:** [Facilitator Guide](docs/FACILITATOR_GUIDE.md)

Key documents:
- Complete timing breakdown
- Exercise solutions
- Common issues and solutions
- Pre-workshop checklist

## 👥 For Participants

**Start here:** [Participant Guide](docs/PARTICIPANT_GUIDE.md)

Includes:
- Step-by-step exercise instructions
- Expected agent responses
- Verification checkpoints
- Extension challenges

## 🔧 Quick Start (Participants)

1. **Clone this repository**
   ```bash
   git clone https://github.com/vishaljsoni/bmad-copilot-workshop.git
   cd bmad-copilot-workshop
   ```

2. **Create your workshop folder**
   ```bash
   mkdir my-todo-app
   cd my-todo-app
   ```

3. **Install BMad Method**
   ```bash
   npx bmad-method install
   # When prompted, select: BMad Method
   ```

   After installation you'll see two key folders:
   - `_bmad/` — BMad configuration and agents
   - `_bmad-output/` — Where all generated artifacts are saved

4. **Follow along with the workshop!**

## 📚 Additional Resources

- [BMad Method Official Repo](https://github.com/bmad-code-org/BMAD-METHOD)
- [BMad Method Documentation](https://docs.bmad-method.org/)
- [Video: BMad V6 Overview](https://www.youtube.com/watch?v=4VPoGSeI2sw) (12 min)
- [Video: Complete Masterclass](https://www.youtube.com/watch?v=LorEJPrALcg) (42 min)

## 🤝 Contributing

Found an issue or have suggestions? Please open an issue or submit a pull request!

## 📄 License

MIT License - feel free to use these materials for your own workshops!

## 👤 Author

Created by Vishal Soni for GitHub Copilot training workshops

---

**Questions?** Open an issue or reach out on LinkedIn!