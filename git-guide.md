# Git Guide for Data Analysis Projects: From Basics to Branching

## Why Use Git for Data Analysis Projects?

### The Problem Git Solves for Data Scientists
Imagine working on a data analysis project where:
- You accidentally overwrite a working analysis script and can't recover the version that produced your results
- You want to try a different statistical approach but fear breaking your current analysis pipeline
- Team members work on the same Jupyter notebook and create conflicting versions
- Your results changed and you need to understand what happened between "last week's presentation" and "today's numbers"
- You need to track which version of your code produced which figures in your paper

**Git solves all these problems and ensures reproducible research.**

### Key Benefits for Data Analysis
- **Reproducibility**: Track exactly which code version produced which results
- **Experimentation**: Try different models or approaches without losing working code
- **Collaboration**: Multiple analysts can work on the same project seamlessly
- **Documentation**: Every change documents your analytical journey
- **Paper Trail**: Link specific results to specific code versions
- **Backup**: Never lose your analysis scripts or notebooks
- **Rollback**: Return to any previous working state when needed

## Understanding Local vs Remote Repositories

### What is a Repository?
A repository (or "repo") is your project folder enhanced with Git's version control capabilities. It contains:
- All your project files (scripts, notebooks, documentation)
- A hidden `.git` folder that stores the entire history of changes
- Configuration files that tell Git how to behave

### Local Repository: Your Personal Workspace
The **local repository** is the Git-tracked project on your computer. Think of it as your personal laboratory where you:
- Write and test code
- Run analyses
- Make commits (save snapshots)
- Create branches for experiments
- Have the complete project history available offline

**Location**: On your computer's hard drive (e.g., `C:\Users\YourName\Projects\data-analysis` or `/home/yourname/projects/data-analysis`)

### Remote Repository: The Shared Hub
The **remote repository** is a copy of your project hosted on a server (like GitHub, GitLab, or Bitbucket). Think of it as:
- A cloud backup of your entire project
- A collaboration hub where team members share work
- A public or private showcase of your analysis
- A way to sync work between multiple computers

**Location**: On a server (e.g., `https://github.com/yourusername/data-analysis`)

### The Relationship Between Local and Remote
```
Your Computer (Local)          Internet            GitHub/GitLab (Remote)
┌─────────────────┐           ┌─────────┐         ┌─────────────────┐
│  Local Repo     │  push →   │         │   →     │  Remote Repo    │
│  - Your edits   │           │         │         │  - Backup       │
│  - History      │           │         │         │  - Sharing      │
│  - Branches     │  ← pull   │         │   ←     │  - Collaboration│
└─────────────────┘           └─────────┘         └─────────────────┘
```

**Key Operations**:
- **Push**: Send your local commits to the remote repository
- **Pull**: Fetch and integrate changes from remote to local
- **Clone**: Create a local copy of a remote repository
- **Fetch**: Download remote changes without integrating them

## Setting Up GitHub as Your Remote Repository

### Step 1: Create a GitHub Account
1. Go to [github.com](https://github.com)
2. Click "Sign up"
3. Choose a username (e.g., `yourname` or `yourname-analytics`)
4. Verify your email address

### Step 2: Create a New Repository on GitHub
1. Click the "+" icon in the top right corner
2. Select "New repository"
3. Fill in the details:
   - **Repository name**: `customer-churn-analysis` (use descriptive names)
   - **Description**: "Predictive model for customer churn using machine learning"
   - **Public/Private**: Choose based on your needs
   - **Initialize with README**: Usually check this
   - **Add .gitignore**: Choose "Python" or "R" template
   - **License**: Choose if making public (MIT is common)

### Step 3: Connect Local Repository to GitHub

#### Option A: Starting Fresh (Recommended for New Projects)
```bash
# 1. Clone the repository from GitHub to your local machine
git clone https://github.com/yourusername/customer-churn-analysis.git

# 2. Navigate into the project folder
cd customer-churn-analysis

# 3. Verify the remote connection
git remote -v
# You should see:
# origin  https://github.com/yourusername/customer-churn-analysis.git (fetch)
# origin  https://github.com/yourusername/customer-churn-analysis.git (push)
```

#### Option B: Connecting Existing Local Project to GitHub
```bash
# 1. Navigate to your existing project
cd /path/to/your/analysis-project

# 2. Initialize Git if not already done
git init

# 3. Add all your files to Git
git add .
git commit -m "Initial commit: Add existing analysis files"

# 4. Add GitHub as remote repository
git remote add origin https://github.com/yourusername/analysis-project.git

# 5. Push your code to GitHub
git push -u origin main
# The -u flag sets upstream tracking, so future pushes can just use 'git push'
```

### Step 4: Authentication Setup

#### For HTTPS (Easier for Beginners)
When you push for the first time, Git will ask for your GitHub username and password. Since 2021, GitHub requires a Personal Access Token instead of your password:

1. Go to GitHub → Settings → Developer settings → Personal access tokens
2. Click "Generate new token (classic)"
3. Give it a name like "Git CLI"
4. Select scopes (at minimum, check "repo")
5. Generate token and copy it
6. Use this token as your password when Git prompts you

#### For SSH (More Secure, One-Time Setup)
```bash
# 1. Generate SSH key
ssh-keygen -t ed25519 -C "your.email@example.com"
# Press Enter to accept default location
# Optionally add a passphrase

# 2. Start SSH agent
eval "$(ssh-agent -s)"

# 3. Add SSH key to agent
ssh-add ~/.ssh/id_ed25519

# 4. Copy public key
cat ~/.ssh/id_ed25519.pub
# Copy the output

# 5. Add to GitHub:
# - Go to GitHub → Settings → SSH and GPG keys
# - Click "New SSH key"
# - Paste your key and save

# 6. Test connection
ssh -T git@github.com
# Should see: "Hi username! You've successfully authenticated..."

# 7. Change remote URL to SSH
git remote set-url origin git@github.com:yourusername/repo-name.git
```

## Core Git Concepts for Data Analysis

### The Three States of Git Explained

![Git Three File States](./assets/visualisation_of_three_file_states.png)

*Credit: [NOAA EDAB - Getting Git](https://noaa-edab.github.io/presentations/20190411_Getting_Git_Hardison.html#11)*

1. **Working Directory**: Where you actively edit files
   - This is where you write Python/R scripts, modify Jupyter notebooks
   - Files here might be in any state: modified, new, deleted
   - Example: You're updating `data_cleaning.py` with new preprocessing steps

2. **Staging Area (Index)**: Your "commit preparation zone"
   - Where you organize which changes will go into the next commit
   - Allows you to commit only specific files or even parts of files
   - Example: Stage only `analysis.R` and `results/figures/plot1.png`, not the large data files

3. **Repository (History)**: The permanent record
   - Contains all committed snapshots of your project
   - Each commit has a unique ID (hash) like `a3f5c2b`
   - Example: Commit with message "Add logistic regression model for churn prediction"

### Key Terms Explained
- **Commit**: A permanent snapshot of your project
  - Like saving a version with a timestamp and description
  - Example: "Fixed data leakage in cross-validation setup"
  
- **Stage**: Selecting which changes to include in a commit
  - Like choosing which files to include in a zip archive
  - Allows logical grouping of related changes
  
- **Branch**: An independent line of development
  - Like having multiple versions of your analysis running in parallel
  - Example: `main` branch has stable analysis, `experiment/neural-net` tries deep learning
  
- **Merge**: Combining changes from different branches
  - Like integrating your experimental analysis back into the main project
  - Git automatically combines changes when possible
  
- **Checkout**: Switching between branches or versions
  - Like opening different versions of your project
  - Can also restore individual files to previous states

## First-Time Git Configuration

```bash
# Set your identity (used in commit history)
git config --global user.name "Your Full Name"
git config --global user.email "your.email@example.com"

# Set default branch name to 'main' (modern standard)
git config --global init.defaultBranch main

# Set your preferred text editor
git config --global core.editor "code --wait"   # VS Code
# git config --global core.editor "nano"        # Nano (simple)
# git config --global core.editor "vim"         # Vim (advanced)

# Helpful settings for data analysis
git config --global core.autocrlf input  # Handle line endings properly
git config --global pull.rebase false    # Use merge (not rebase) when pulling

# View your configuration
git config --list
```

## Basic Git Workflow for Data Analysis

### Starting a New Analysis Project

#### Option 1: Initialize Git in Existing Project
```bash
# Navigate to your project folder
cd /path/to/customer-segmentation-analysis

# Initialize Git repository
git init
# This creates a hidden .git folder that tracks all changes

# Create a .gitignore file for data analysis
echo "# Data files
data/raw/
data/processed/large_files/
*.csv
*.xlsx
*.sav
*.dta

# Python
__pycache__/
*.py[cod]
.ipynb_checkpoints/
venv/
env/

# R
.Rhistory
.RData
.Rproj.user/

# Output files
results/temp/
*.log

# OS files
.DS_Store
Thumbs.db" > .gitignore

# Make your first commit
git add .
git commit -m "Initial commit: Project structure and analysis scripts"
```

#### Option 2: Clone Existing Repository
```bash
# Clone a repository (creates a new folder)
git clone https://github.com/yourusername/covid-impact-analysis.git

# Navigate into the cloned project
cd covid-impact-analysis

# You're ready to work!
```

### The Daily Analysis Workflow Explained

```bash
# 1. Start your day: Check project status
git status
# Shows:
# - Which branch you're on
# - Modified files (in red)
# - Staged files (in green)
# - Untracked new files

# 2. Review what changed in your scripts
git diff                    # All changes in working directory
git diff analysis.py        # Changes in specific file
git diff --stat            # Summary of changes
# This shows exact lines added/removed

# 3. Stage your changes selectively
git add analysis.py         # Stage specific file
git add src/*.py           # Stage all Python files in src/
git add -p analysis.py     # Stage specific parts of a file interactively
git add .                  # Stage everything (use carefully!)

# 4. Commit with descriptive message
git commit -m "Add robust standard errors to regression models"
# Good messages explain WHY, not just WHAT

# 5. Push to remote repository (backup + share)
git push origin main
# Sends your commits to GitHub/GitLab

# 6. Pull latest changes (start of day or before pushing)
git pull origin main
# Downloads and merges teammates' changes
```

### Detailed Example: A Day in the Life of a Data Analyst

```bash
# Morning: Start working on customer churn analysis
cd ~/projects/customer-churn
git pull origin main                      # Get latest changes

# Work on your analysis
# Edit churn_analysis.py, create new visualizations

git status
# Output:
# modified:   churn_analysis.py
# modified:   notebooks/exploratory_analysis.ipynb
# untracked:  figures/churn_by_segment.png

# Review changes
git diff churn_analysis.py               # See what changed

# Stage and commit logical units
git add churn_analysis.py
git commit -m "Add customer lifetime value calculation to churn model"

git add figures/churn_by_segment.png
git commit -m "Add visualization: churn rates by customer segment"

git add notebooks/exploratory_analysis.ipynb
git commit -m "Update EDA notebook with new feature engineering"

# Push all commits
git push origin main
```

### Viewing Project History

```bash
# View commit history (multiple ways)
git log                         # Full details
git log --oneline              # Compact view
git log --oneline -10          # Last 10 commits
git log --graph                # Show branch structure
git log --author="Sarah"       # Commits by specific person
git log --grep="regression"    # Commits mentioning "regression"
git log -- analysis.py         # History of specific file

# View specific commit details
git show a3f5c2b               # Show what changed in commit
git show a3f5c2b:analysis.py   # Show file contents at that commit

# Find when something changed
git blame analysis.py          # Show who changed each line when
```

## Managing Changes in Your Analysis

### Before Committing: Undoing Local Changes
```bash
# Discard changes in working directory (CAREFUL - loses work!)
git checkout -- analysis.py    # Reset single file to last commit
git checkout -- .             # Reset all files to last commit

# Unstage files (keep changes in working directory)
git reset HEAD analysis.py    # Unstage single file
git reset HEAD               # Unstage all files

# Clean untracked files (CAREFUL!)
git clean -n                 # Preview what would be deleted
git clean -f                 # Delete untracked files
git clean -fd                # Delete untracked files and directories
```

### After Committing: Fixing Mistakes
```bash
# Undo last commit, keep changes staged
git reset --soft HEAD~1
# Use when: You committed too early and want to add more changes

# Undo last commit, keep changes unstaged
git reset HEAD~1
# Use when: You want to reorganize into different commits

# Undo last commit, discard all changes (DANGEROUS!)
git reset --hard HEAD~1
# Use when: The commit was completely wrong and you want to start over

# Create new commit that undoes previous commit
git revert HEAD
# Use when: You already pushed and need to undo publicly

# Fix last commit message
git commit --amend -m "Better commit message"

# Add forgotten file to last commit
git add forgotten_notebook.ipynb
git commit --amend --no-edit
```

## Branching for Data Analysis Experiments

### Why Branches Matter for Analysis
- **Try Different Approaches**: Test various models without breaking working code
- **Collaborate Safely**: Team members work on different analyses simultaneously
- **Peer Review**: Review code and results before merging
- **Experimentation**: Try risky changes in isolation
- **Feature Development**: Build new analysis features separately

### Branch Naming for Data Projects
- `main`: Stable, reviewed analysis code
- `develop`: Integration branch for ongoing work
- `analysis/description`: Analysis tasks (e.g., `analysis/customer-segmentation`)
- `model/description`: Model development (e.g., `model/random-forest`)
- `feature/description`: New features (e.g., `feature/data-pipeline`)
- `bugfix/description`: Fixing issues (e.g., `bugfix/missing-data-handling`)
- `experiment/description`: Experimental work (e.g., `experiment/deep-learning`)

### Branching Workflow for Analysis

```bash
# 1. Create branch for new analysis
git checkout main                        # Start from main
git pull origin main                     # Get latest version
git checkout -b analysis/time-series     # Create and switch to new branch

# 2. Work on your analysis
# ... edit time_series_analysis.py ...
# ... create new notebooks ...
# ... generate visualizations ...

git add src/time_series_analysis.py
git commit -m "Implement ARIMA model for sales forecasting"

git add notebooks/time_series_eda.ipynb
git commit -m "Add exploratory analysis of seasonal patterns"

# 3. Keep branch updated with main (important for long-running analyses)
git checkout main
git pull origin main
git checkout analysis/time-series
git merge main                          # Incorporate latest changes
# Resolve any conflicts if they exist

# 4. Push branch to remote
git push origin analysis/time-series

# 5. Create Pull Request on GitHub/GitLab for review
# Team can review code, results, and methodology

# 6. After approval, merge to main
git checkout main
git merge analysis/time-series
git push origin main

# 7. Clean up
git branch -d analysis/time-series      # Delete local branch
git push origin --delete analysis/time-series  # Delete remote branch
```

### Real-World Example: A/B Test Analysis

```bash
# Create branch for A/B test analysis
git checkout -b analysis/email-campaign-ab-test

# Develop analysis
# Create: ab_test_analysis.py, power_analysis.R, results_visualization.ipynb

# Commit your work progressively
git add src/power_analysis.R
git commit -m "Add statistical power analysis for sample size determination"

git add src/ab_test_analysis.py
git commit -m "Implement A/B test with bootstrap confidence intervals"

git add notebooks/results_visualization.ipynb
git commit -m "Create visualizations of test results with significance tests"

# Push for review
git push origin analysis/email-campaign-ab-test

# After review and approval, merge
git checkout main
git merge analysis/email-campaign-ab-test
```

## Collaborative Workflows for Data Teams

### Handling Merge Conflicts in Analysis Code

When multiple people edit the same file:

```bash
# Attempting to merge shows conflict
git merge teammate-branch
# Output: Automatic merge failed; fix conflicts and commit

# Check which files have conflicts
git status
# Shows: both modified:   src/data_preprocessing.py

# Open conflicted file, you'll see:
<<<<<<< HEAD
# Your version
def clean_data(df):
    df = df.dropna(subset=['customer_id'])
    return df
=======
# Their version
def clean_data(df):
    df = df.dropna(subset=['customer_id', 'purchase_date'])
    return df
>>>>>>> teammate-branch

# Edit file to resolve (keep both constraints):
def clean_data(df):
    df = df.dropna(subset=['customer_id', 'purchase_date'])
    return df

# Mark as resolved
git add src/data_preprocessing.py
git commit -m "Merge: Combine data cleaning approaches"
```

### Pull Request Best Practices for Analysis

1. **Create Descriptive PRs**:
   - Title: "Add customer segmentation analysis using K-means"
   - Description: Include:
     - Analysis objective
     - Methodology summary
     - Key findings
     - Links to result visualizations

2. **Review Checklist**:
   - Code runs without errors
   - Data sources are documented
   - Results are reproducible
   - Statistical assumptions are checked
   - Visualizations are clear

## Best Practices for Data Analysis Projects

### Commit Messages for Analysis
```bash
# Good commit messages for data projects
git commit -m "Fix data leakage in train-test split"
git commit -m "Add cross-validation to model selection process"
git commit -m "Update feature engineering: add lag variables"
git commit -m "Refactor data pipeline for memory efficiency"
git commit -m "Add unit tests for data transformation functions"

# Bad commit messages
git commit -m "Update"
git commit -m "Fix"
git commit -m "New analysis"
```

### What to Track in Data Projects

✅ **DO track:**
- Analysis scripts (`.py`, `.R`, `.jl`)
- Jupyter notebooks (`.ipynb`)
- Documentation (`.md`, `.txt`)
- Configuration files (`.json`, `.yaml`)
- Small reference data (`lookup_tables.csv`)
- Requirements files (`requirements.txt`, `renv.lock`)
- Project reports (`.tex`, `.Rmd`)
- Figure generation scripts

❌ **DON'T track:**
- Large datasets (use `.gitignore`)
- Generated outputs (PDFs, PNGs - unless final versions)
- Credentials/API keys (use environment variables)
- Temporary files
- Cache files (`.RData`, `__pycache__`)
- Personal IDE settings

### Data-Specific .gitignore
```gitignore
# Data files
data/raw/
data/processed/
data/external/
*.csv
*.xlsx
*.parquet
*.feather
*.sav
*.dta
*.h5
*.hdf5

# Large model files
models/*.pkl
models/*.joblib
*.h5
*.pt
*.pth

# Python
__pycache__/
*.py[cod]
*$py.class
.Python
env/
venv/
.venv
.ipynb_checkpoints/
.pytest_cache/
htmlcov/
.coverage
*.egg-info/

# R
.Rhistory
.RData
.Rproj.user/
*.Rproj

# Outputs (track only final versions)
results/temp/
results/scratch/
figures/drafts/
*.log

# Credentials
.env
config/secrets.yaml
credentials.json

# OS
.DS_Store
Thumbs.db
*~

# IDE
.vscode/
.idea/
*.swp
```

## Common Data Analysis Workflows

### Exploratory Analysis Workflow
```bash
# Create branch for exploration
git checkout -b analysis/explore-customer-behavior

# As you explore, commit meaningful checkpoints
git add notebooks/initial_exploration.ipynb
git commit -m "Initial EDA: distributions and missing data patterns"

git add src/data_quality_checks.py
git commit -m "Add automated data quality validation functions"

# Share findings
git push origin analysis/explore-customer-behavior
```

### Model Development Workflow
```bash
# Branch for model development
git checkout -b model/gradient-boosting

# Track model iterations
git add src/models/baseline_model.py
git commit -m "Implement baseline logistic regression"

git add src/models/gb_model.py
git commit -m "Add gradient boosting with hyperparameter tuning"

git add results/model_comparison.csv
git commit -m "Add model performance comparison results"
```

### Production Pipeline Workflow
```bash
# Branch for productionizing
git checkout -b feature/automated-pipeline

# Build robust pipeline
git add src/pipeline/data_ingestion.py
git commit -m "Add automated data ingestion with validation"

git add src/pipeline/feature_engineering.py
git commit -m "Modularize feature engineering steps"

git add tests/test_pipeline.py
git commit -m "Add unit tests for pipeline components"
```

## Troubleshooting Common Issues

### Large File Problems
```bash
# If you accidentally committed a large file
git rm --cached data/large_dataset.csv
git commit -m "Remove large dataset from tracking"
echo "data/large_dataset.csv" >> .gitignore
git add .gitignore
git commit -m "Update gitignore to exclude large datasets"

# For files already in history, use BFG Repo Cleaner or git-filter-branch
```

### Jupyter Notebook Conflicts
```bash
# Notebooks are hard to merge, so:
# 1. Use nbdime for better notebook diffs
pip install nbdime
nbdime config-git --enable

# 2. Or clear outputs before committing
jupyter nbconvert --clear-output --inplace notebook.ipynb
git add notebook.ipynb
git commit -m "Add analysis notebook (outputs cleared)"
```

### Credentials in Code
```bash
# If you accidentally committed credentials
# 1. Remove from code immediately
# 2. Change the credentials
# 3. Use environment variables instead:

# In .env file (don't commit this!)
DATABASE_URL=postgresql://user:pass@host:5432/db
API_KEY=your_secret_key

# In Python:
import os
from dotenv import load_dotenv
load_dotenv()
db_url = os.getenv('DATABASE_URL')
```

## Quick Reference for Data Analysis

| Command | Description | When to Use |
|---------|-------------|-------------|
| `git init` | Initialize repository | Starting new project |
| `git clone <url>` | Clone repository | Joining existing project |
| `git status` | Check current state | Frequently! |
| `git add <file>` | Stage changes | Before committing |
| `git commit -m "msg"` | Save snapshot | Completed logical unit |
| `git push` | Upload to remote | Share work/backup |
| `git pull` | Download updates | Start of day/before push |
| `git branch` | List branches | Check available branches |
| `git checkout -b <name>` | New branch | Starting new analysis |
| `git merge <branch>` | Combine branches | Integrate completed work |
| `git log --oneline` | View history | Review project evolution |
| `git diff` | See changes | Before committing |
| `git stash` | Temporarily save work | Switching context quickly |

