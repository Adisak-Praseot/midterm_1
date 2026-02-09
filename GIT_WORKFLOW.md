# Git Workflow Guide

## ✅ Project Setup Complete

The full-stack project now has a complete Git workflow configured for collaboration and version control.

## Repository Structure

### Branches
- **master**: Main production branch with all initial setup
- **feature/add-express-backend-integration**: Feature branch for backend integration

### Current Status
```
* feature/add-express-backend-integration (HEAD)
  master
```

### Git Log
```
Commit: 0e267f5
Message: build: initial project setup with express backend and quasar frontend
Author: Developer <dev@example.com>

56 files changed:
- Backend Express server with all dependencies
- Quasar frontend with all components
- Docker configuration files
- Prisma database setup
- Environment configuration
```

## .gitignore Configuration

The project has a comprehensive `.gitignore` that excludes:

### Backend
- `backend/.env` - Environment variables (sensitive data)
- `backend/logs/` - Request logs
- `backend/node_modules/` - Dependencies
- `backend/dist/` - Build output
- `.prisma/` - Prisma cache

### Frontend
- `frontend/.env` - Environment variables
- `frontend/dist/` - Build output
- `frontend/.quasar/` - Quasar cache

### General
- `.env*` - All environment files
- `node_modules/` - Dependencies everywhere
- `logs/` - Log files
- `*.log` - Log files
- `.DS_Store` - macOS files
- `.vscode/` - IDE configuration
- `.idea/` - IDE configuration

### Currently Ignored Files
```
backend/.env
backend/logs/
backend/node_modules/
frontend/.env
frontend/.vscode/
```

## Git Workflow

### 1. **Feature Development Workflow**

Start feature development on a new branch:
```bash
# Switch to master and pull latest
git checkout master
git pull origin master

# Create feature branch
git checkout -b feature/add-express-backend-integration

# Make changes to backend and frontend
# ... Edit files ...

# Stage changes
git add backend/ frontend/src/pages/IndexPage.vue

# Commit with descriptive message
git commit -m "feat: add express backend api and integrate with quasar frontend"

# Push to remote
git push origin feature/add-express-backend-integration
```

### 2. **Commit Message Convention**

Following Conventional Commits:
```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `refactor`: Code refactoring
- `test`: Adding tests
- `docs`: Documentation
- `build`: Build system changes
- `chore`: Maintenance tasks

**Example:**
```
feat(backend): add express api endpoints

- Added /api/demo endpoint
- Configured CORS
- Added request logging

Closes #123
```

### 3. **Merging to Master**

When feature is complete and tested:

```bash
# Switch to master
git checkout master

# Pull latest changes
git pull origin master

# Merge feature branch
git merge feature/add-express-backend-integration

# Option 1: Regular merge (keeps full history)
git merge feature/add-express-backend-integration

# Option 2: Squash merge (clean history)
git merge --squash feature/add-express-backend-integration

# Delete feature branch after merge
git branch -d feature/add-express-backend-integration
git push origin --delete feature/add-express-backend-integration
```

### 4. **Pull Request Workflow (GitHub)**

If using GitHub:

1. **Create PR**
   - Push feature branch to remote
   - Open PR on GitHub
   - Add description of changes

2. **Code Review**
   - Team members review code
   - Request changes if needed
   - Approve when ready

3. **Merge to Master**
   - Squash and merge (recommended for clean history)
   - Or regular merge with rebase

4. **Delete Branch**
   - Delete remote branch after merge
   - Clean up local branch

## Best Practices

### 1. **Frequent Commits**
- Commit small, logical changes
- Write descriptive commit messages
- Test before committing

### 2. **Keep Branch Updated**
```bash
git fetch origin
git rebase origin/master
```

### 3. **Stashing Uncommitted Changes**
```bash
# Save uncommitted changes
git stash

# List stashes
git stash list

# Apply last stash
git stash pop

# Apply specific stash
git stash apply stash@{0}
```

### 4. **Viewing History**
```bash
# Oneline log
git log --oneline

# Graph view
git log --graph --oneline --all

# Recent commits
git log -5 --oneline

# Specific file history
git log -- backend/server.js
```

### 5. **Common Operations**

**Undo uncommitted changes:**
```bash
git checkout filename
git restore filename
```

**Undo last commit (keep changes):**
```bash
git reset --soft HEAD~1
```

**Undo last commit (discard changes):**
```bash
git reset --hard HEAD~1
```

**View changes before committing:**
```bash
git diff                    # Unstaged changes
git diff --staged           # Staged changes
git diff master..branch     # Difference between branches
```

## Files to Never Commit

⚠️ **These should always be in .gitignore:**

1. **.env files** - Contains API keys, database passwords, etc.
2. **node_modules/** - Install with `npm install`
3. **logs/** - Generated at runtime
4. **/dist or /build** - Generated build files
5. **IDE settings** - .vscode/, .idea/
6. **OS files** - .DS_Store, Thumbs.db
7. **Lock files** - Only in monorepos, usually committed

## Environment Variables (.env)

### Backend (.env)
```
DATABASE_URL="postgresql://..."
PORT=3000
NODE_ENV=development
```

### Frontend (.env)
```
API_URL=http://localhost:3000
```

**Never commit .env files!** Use `.env.example` as template:

### Backend (.env.example)
```
DATABASE_URL=postgresql://user:password@localhost/dbname
PORT=3000
NODE_ENV=development
```

### Frontend (.env.example)
```
API_URL=http://localhost:3000
```

## Remote Setup (GitHub/GitLab)

### First time push to remote:
```bash
git remote add origin https://github.com/username/repo.git
git branch -M main
git push -u origin main
```

### Subsequent pushes:
```bash
git push origin feature-branch
git push origin master
```

### Clone from remote:
```bash
git clone https://github.com/username/repo.git
cd repo
npm install  # in backend and frontend
```

## Troubleshooting

### **Lost commits?**
```bash
git reflog
git checkout <commit-hash>
```

### **Wrong branch pushed?**
```bash
git push origin +correct-branch:wrong-branch
```

### **Merge conflicts?**
1. Identify conflicted files: `git status`
2. Edit files and resolve conflicts
3. Stage resolved files: `git add file`
4. Complete merge: `git commit`

### **Need to revert public commit?**
```bash
git revert <commit-hash>
```

## Current Project Status

✅ **Completed:**
- Git repository initialized
- .gitignore configured for safety
- Initial commit with full project
- Feature branch created
- All source files tracked

🔄 **Next Steps:**
- Configure GitHub/GitLab remote
- Set up branch protection rules
- Configure CI/CD pipeline
- Enable PR reviews

## Integration with Current Workflow

### Files Tracked
```
✓ backend/
  - server.js
  - package.json
  - src/
  - prisma/
  ✗ .env (ignored)
  ✗ logs/ (ignored)
  ✗ node_modules/ (ignored)

✓ frontend/
  - src/pages/IndexPage.vue
  - src/boot/
  - package.json
  - quasar.config.js
  ✓ .env.example (tracked)
  ✗ .env (ignored)
  ✗ node_modules/ (ignored)

✓ docker-compose.yml
✓ .gitignore
✓ README.md
✓ FRONTEND_INTEGRATION.md
```

## Commands Reference

```bash
# Check status
git status

# View log
git log --oneline

# Create branch
git checkout -b feature/name

# Switch branch
git checkout branch-name

# Add files
git add file
git add .

# Commit
git commit -m "message"

# Push
git push origin branch-name

# Pull
git pull origin branch-name

# Merge
git merge branch-name

# Delete branch
git branch -d branch-name

# View branches
git branch -a

# View remote
git remote -v
```
