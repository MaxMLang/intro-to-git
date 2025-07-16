# Git Guide for Data Analysis Projects

A comprehensive guide to using Git for data science and analysis projects, from basic concepts to advanced workflows.

## Overview

This guide covers everything you need to know about Git for data analysis:

- **Git Basics**: Understanding repositories, commits, and version control
- **Setup & Configuration**: Getting started with GitHub and local repositories
- **Daily Workflows**: Practical commands for everyday data analysis work
- **Branching Strategies**: Managing experiments and collaborative work
- **Best Practices**: What to track, what to ignore, and how to organize your projects
- **Troubleshooting**: Solutions for common issues in data science workflows

## Key Benefits

- **Reproducibility**: Track exactly which code version produced which results
- **Experimentation**: Try different approaches without losing working code
- **Collaboration**: Multiple analysts can work seamlessly on the same project
- **Documentation**: Every change documents your analytical journey
- **Backup & Recovery**: Never lose your analysis scripts or notebooks

## 📚 Quick Start

1. **Read the full guide**: [git-guide.md](git-guide.md)
2. **Set up your environment**: Follow the configuration section
3. **Start with basic workflows**: Practice the daily analysis workflow
4. **Explore branching**: Learn how to manage experiments and features

## 🔧 Essential Commands

```bash
# Daily workflow
git status                    # Check what's changed
git add <file>               # Stage changes
git commit -m "message"      # Save snapshot
git push                     # Share work
git pull                     # Get updates

# Branching for experiments
git checkout -b analysis/new-model
git merge analysis/new-model
```

## 📊 Understanding Git's Three File States

![Git Three File States](assets/visualisation_of_three_file_states.png)

*Credit: [NOAA EDAB - Getting Git](https://noaa-edab.github.io/presentations/20190411_Getting_Git_Hardison.html#11)*

This diagram illustrates the core Git workflow:
- **Working Directory**: Where you make changes to your files
- **Staging Area**: Where you prepare changes for commit
- **Repository**: Where your project history is permanently stored
- **Remote**: Cloud backup and collaboration hub (GitHub/GitLab)

## 📁 Project Structure

The guide includes templates for:
- `.gitignore` files optimized for data projects
- Branch naming conventions
- Commit message best practices
- Repository organization

## 🤝 Contributing

This guide is designed to be practical and actionable. If you have suggestions for improvements or additional workflows, feel free to contribute!

## 📚 Additional Resources

- **[NOAA EDAB - Getting Git](https://noaa-edab.github.io/presentations/20190411_Getting_Git_Hardison.html#11)**: Excellent presentation on Git fundamentals with clear visualizations