# GitHub Organization Setup Guide - Nkaizen Consulting Services

## Overview
This guide will help you create a secure GitHub Organization for Nkaizen Consulting Services with proper security settings, access controls, and best practices.

**Organization Name**: `nkaizen-consulting` or `nkaizen-services`

---

## Table of Contents
1. [Create GitHub Organization](#1-create-github-organization)
2. [Security Settings](#2-security-settings)
3. [Member Management](#3-member-management)
4. [Repository Settings](#4-repository-settings)
5. [Branch Protection Rules](#5-branch-protection-rules)
6. [Access Control](#6-access-control)
7. [Best Practices](#7-best-practices)

---

## 1. Create GitHub Organization

### Step 1: Login to GitHub
1. Go to: **https://github.com/**
2. Login with your account (or create one if needed)

### Step 2: Create Organization
1. Click your **profile picture** (top right)
2. Click **"Your organizations"**
3. Click **"New organization"** (green button)

### Step 3: Choose Plan
**Options**:
- **Free**: Unlimited public/private repos, basic features
- **Team** ($4/user/month): Advanced collaboration, required reviewers
- **Enterprise** ($21/user/month): SAML SSO, audit logs, advanced security

**Recommendation**: Start with **Free** plan, upgrade later if needed.

### Step 4: Set Up Organization
1. **Organization account name**: Enter `nkaizen-consulting` or `nkaizen-services`
   - Must be unique on GitHub
   - Use lowercase, hyphens allowed
   - Cannot be changed easily later

2. **Contact email**: Enter `info@nkps.ai`
   - This is for GitHub notifications

3. **This organization belongs to**: Select **"My personal account"** or **"A business or institution"**

4. Click **"Next"**

### Step 5: Add Members (Optional - Skip for Now)
1. You can add members later
2. Click **"Skip this step"** or **"Complete setup"**

### Step 6: Verify Email
1. Check `info@nkps.ai` inbox (Zoho Mail)
2. Click verification link from GitHub
3. Complete verification

---

## 2. Security Settings

### Step 1: Access Organization Settings
1. Go to your organization page: `https://github.com/nkaizen-consulting`
2. Click **"Settings"** tab

### Step 2: Enable Two-Factor Authentication (2FA)
**Critical Security Step!**

1. Go to **"Authentication security"** (left sidebar)
2. Enable **"Require two-factor authentication"**
3. Set grace period: **30 days** (gives members time to set up)
4. Click **"Save"**

**What this does**: All organization members must enable 2FA on their accounts.

### Step 3: Configure Base Permissions
1. Go to **"Member privileges"** (left sidebar)
2. **Base permissions**: Set to **"No permission"**
   - Members have no default access
   - Must be explicitly granted access to repos
3. **Repository creation**: Uncheck all boxes
   - Only admins can create repos
4. **Repository forking**: Uncheck **"Allow forking of private repositories"**
5. Click **"Save"**

### Step 4: Enable Security Features
1. Go to **"Code security and analysis"** (left sidebar)
2. Enable these features:

**Dependency graph**: ✅ Enable
- Shows project dependencies

**Dependabot alerts**: ✅ Enable
- Alerts for vulnerable dependencies

**Dependabot security updates**: ✅ Enable
- Automatic security patches

**Secret scanning**: ✅ Enable (if available)
- Detects committed secrets/tokens

**Push protection**: ✅ Enable (if available)
- Prevents pushing secrets

3. Click **"Enable all"** or enable individually

### Step 5: Configure Third-Party Access
1. Go to **"Third-party access"** (left sidebar)
2. **Third-party application access policy**: Select **"Access restriction enabled"**
3. This requires admin approval for OAuth apps

---

## 3. Member Management

### Step 1: Invite Members
1. Go to **"People"** tab in organization
2. Click **"Invite member"**
3. Enter email address or GitHub username
4. Select role:
   - **Owner**: Full control (use sparingly)
   - **Member**: Basic access (default)
5. Click **"Send invitation"**

### Step 2: Create Teams
Teams help organize members and manage permissions.

1. Go to **"Teams"** tab
2. Click **"New team"**

**Suggested Teams**:

**Team 1: Admins**
- **Team name**: `admins`
- **Description**: Organization administrators
- **Visibility**: Visible
- **Members**: Add yourself and trusted admins

**Team 2: Developers**
- **Team name**: `developers`
- **Description**: Development team
- **Visibility**: Visible
- **Members**: Add developers

**Team 3: Contractors**
- **Team name**: `contractors`
- **Description**: External contractors
- **Visibility**: Visible
- **Members**: Add contractors (temporary access)

### Step 3: Set Team Permissions
For each team:
1. Go to team page
2. Click **"Repositories"** tab
3. Add repositories and set permissions:
   - **Read**: View and clone
   - **Triage**: Read + manage issues
   - **Write**: Read + push to repo
   - **Maintain**: Write + manage repo settings
   - **Admin**: Full control

---

## 4. Repository Settings

### Step 1: Create Repository
1. Go to organization page
2. Click **"Repositories"** tab
3. Click **"New repository"**

### Step 2: Configure Repository
1. **Repository name**: e.g., `nk-website`, `client-project-1`
2. **Description**: Brief description
3. **Visibility**:
   - **Public**: Anyone can see (for open source)
   - **Private**: Only organization members (recommended for client work)
4. **Initialize repository**:
   - ✅ Add README
   - ✅ Add .gitignore (select template)
   - ✅ Choose license (if public)
5. Click **"Create repository"**

### Step 3: Repository Security Settings
For each repository:

1. Go to repository **"Settings"**
2. **General** section:
   - ❌ Disable **"Allow merge commits"** (optional - keeps history clean)
   - ✅ Enable **"Allow squash merging"**
   - ✅ Enable **"Allow rebase merging"**
   - ✅ Enable **"Automatically delete head branches"**

3. **Branches** section (see Branch Protection below)

4. **Collaborators and teams**:
   - Add teams with appropriate permissions
   - Don't add individual users (use teams instead)

---

## 5. Branch Protection Rules

**Critical for Security!** Prevents accidental or malicious changes to main branch.

### Step 1: Protect Main Branch
1. Go to repository **"Settings"**
2. Click **"Branches"** (left sidebar)
3. Click **"Add branch protection rule"**

### Step 2: Configure Protection Rules
**Branch name pattern**: `main` (or `master`)

**Enable these protections**:

✅ **Require a pull request before merging**
- ✅ Require approvals: **1** (or more for critical repos)
- ✅ Dismiss stale pull request approvals when new commits are pushed
- ✅ Require review from Code Owners (if using CODEOWNERS file)

✅ **Require status checks to pass before merging**
- Add CI/CD checks if configured (e.g., tests, linting)

✅ **Require conversation resolution before merging**
- All comments must be resolved

✅ **Require signed commits** (optional but recommended)
- Ensures commits are verified

✅ **Require linear history** (optional)
- Prevents merge commits

✅ **Include administrators**
- Even admins must follow these rules

✅ **Restrict who can push to matching branches**
- Only specific teams/users can push directly
- Add `admins` team only

✅ **Allow force pushes**: ❌ Disable
- Prevents rewriting history

✅ **Allow deletions**: ❌ Disable
- Prevents branch deletion

### Step 3: Save Rules
Click **"Create"** or **"Save changes"**

---

## 6. Access Control

### Step 1: Repository Access Levels

**Public Repositories**:
- Anyone can view and clone
- Only members can push/contribute

**Private Repositories**:
- Only organization members can view
- Access controlled by team permissions

### Step 2: Team-Based Access
**Best Practice**: Use teams, not individual users

**Example Access Structure**:

```
Repository: nk-website
├── admins team: Admin access
├── developers team: Write access
└── contractors team: Read access (or no access)

Repository: client-confidential-project
├── admins team: Admin access
├── project-team: Write access
└── others: No access
```

### Step 3: Outside Collaborators
For external contractors:

1. Go to **"People"** tab
2. Click **"Outside collaborators"**
3. Click **"Invite collaborator"**
4. Grant access to specific repositories only
5. Set expiration date if possible

**Remove access when project ends!**

---

## 7. Best Practices

### Security Best Practices

1. **Enable 2FA for all members** (required)
2. **Use SSH keys or Personal Access Tokens** (not passwords)
3. **Never commit secrets** (API keys, passwords, tokens)
4. **Use .gitignore** to exclude sensitive files
5. **Enable branch protection** on all important branches
6. **Review code before merging** (pull requests)
7. **Keep dependencies updated** (Dependabot)
8. **Audit access regularly** (remove inactive members)
9. **Use signed commits** (GPG keys)
10. **Enable secret scanning** (GitHub Advanced Security)

### Repository Best Practices

1. **Use descriptive names** (lowercase, hyphens)
2. **Add README.md** to every repo
3. **Use .gitignore** templates
4. **Add LICENSE** file (for public repos)
5. **Create CODEOWNERS** file (for code review assignments)
6. **Use issue templates** (for bug reports, features)
7. **Use pull request templates**
8. **Tag releases** (semantic versioning)
9. **Write good commit messages**
10. **Keep repos focused** (one project per repo)

### Team Management Best Practices

1. **Use teams, not individual access**
2. **Follow principle of least privilege** (minimum access needed)
3. **Remove members when they leave**
4. **Audit permissions quarterly**
5. **Use outside collaborators for contractors**
6. **Document team responsibilities**
7. **Set up team discussions** (for communication)

### Workflow Best Practices

1. **Use feature branches** (not direct commits to main)
2. **Create pull requests** for all changes
3. **Require code reviews** (at least 1 approval)
4. **Run CI/CD tests** before merging
5. **Squash commits** when merging (clean history)
6. **Delete branches** after merging
7. **Use semantic versioning** for releases
8. **Write changelog** for releases

---

## 8. Additional Security Features

### Enable GitHub Advanced Security (Paid)
If you upgrade to Team or Enterprise:

1. **Code scanning**: Automated security analysis
2. **Secret scanning**: Advanced secret detection
3. **Dependency review**: Review dependency changes in PRs
4. **Security advisories**: Private vulnerability reporting

### Set Up CODEOWNERS File
Create `.github/CODEOWNERS` in repository:

```
# Default owners for everything
* @nkaizen-consulting/admins

# Specific paths
/docs/ @nkaizen-consulting/developers
/src/ @nkaizen-consulting/developers
*.md @nkaizen-consulting/admins
```

### Set Up Issue Templates
Create `.github/ISSUE_TEMPLATE/bug_report.md`:

```markdown
---
name: Bug Report
about: Report a bug
title: '[BUG] '
labels: bug
assignees: ''
---

**Describe the bug**
A clear description of the bug.

**To Reproduce**
Steps to reproduce the behavior.

**Expected behavior**
What you expected to happen.

**Screenshots**
If applicable, add screenshots.
```

### Set Up Pull Request Template
Create `.github/PULL_REQUEST_TEMPLATE.md`:

```markdown
## Description
Brief description of changes.

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Comments added for complex code
- [ ] Documentation updated
- [ ] Tests added/updated
- [ ] All tests passing
```

---

## 9. Compliance & Audit

### Regular Audits
**Monthly**:
- Review member list
- Check repository access
- Review outside collaborators

**Quarterly**:
- Audit team permissions
- Review security settings
- Check for unused repositories
- Update security policies

### Audit Log
**Available on Team/Enterprise plans**:
1. Go to **"Settings"** → **"Audit log"**
2. Review all organization activities
3. Export logs for compliance

### Security Policies
Create `SECURITY.md` in repositories:

```markdown
# Security Policy

## Reporting a Vulnerability
Please report security vulnerabilities to: security@nkps.ai

## Supported Versions
| Version | Supported |
|---------|-----------|
| 1.x     | ✅        |
| < 1.0   | ❌        |
```

---

## 10. Quick Setup Checklist

### Initial Setup
- [ ] Create organization
- [ ] Verify email
- [ ] Enable 2FA requirement
- [ ] Set base permissions to "No permission"
- [ ] Enable security features (Dependabot, secret scanning)
- [ ] Restrict third-party access

### Repository Setup
- [ ] Create repository (private)
- [ ] Add README, .gitignore, LICENSE
- [ ] Enable branch protection on main
- [ ] Add teams with appropriate permissions
- [ ] Create CODEOWNERS file
- [ ] Add issue/PR templates

### Team Setup
- [ ] Create teams (admins, developers, contractors)
- [ ] Invite members
- [ ] Assign team permissions
- [ ] Document team responsibilities

### Security Verification
- [ ] All members have 2FA enabled
- [ ] Branch protection rules active
- [ ] No direct push to main branch
- [ ] Code review required for merges
- [ ] Dependabot alerts enabled
- [ ] Secret scanning enabled

---

## 11. Common Scenarios

### Scenario 1: Adding a New Developer
1. Invite to organization
2. Add to `developers` team
3. Grant team access to relevant repositories
4. Ensure they enable 2FA

### Scenario 2: Starting a New Project
1. Create private repository
2. Add README and .gitignore
3. Enable branch protection
4. Add relevant teams
5. Create initial branch structure

### Scenario 3: Working with Contractor
1. Invite as outside collaborator
2. Grant access to specific repository only
3. Set Read or Write permission (not Admin)
4. Remove access when project ends

### Scenario 4: Repository Got Compromised
1. Immediately revoke all access
2. Rotate all secrets/tokens
3. Review audit log
4. Force password reset for all members
5. Review all recent commits
6. Notify affected parties

---

## 12. Cost Breakdown

### Free Plan
- **Cost**: $0
- **Features**:
  - Unlimited public/private repos
  - Unlimited collaborators
  - 2,000 CI/CD minutes/month
  - 500MB package storage
  - Community support

### Team Plan
- **Cost**: $4/user/month
- **Additional Features**:
  - Required reviewers
  - Draft pull requests
  - Multiple PR reviewers
  - Code owners
  - 3,000 CI/CD minutes/month
  - 2GB package storage

### Enterprise Plan
- **Cost**: $21/user/month
- **Additional Features**:
  - SAML SSO
  - Audit log API
  - Advanced security
  - 50,000 CI/CD minutes/month
  - 50GB package storage
  - Priority support

**Recommendation**: Start with Free, upgrade to Team when you need advanced collaboration features.

---

## 13. Resources

### GitHub Documentation
- Organizations: https://docs.github.com/en/organizations
- Security: https://docs.github.com/en/code-security
- Best Practices: https://docs.github.com/en/repositories/creating-and-managing-repositories/best-practices-for-repositories

### Tools
- GitHub CLI: https://cli.github.com/
- GitHub Desktop: https://desktop.github.com/
- GitHub Mobile: iOS/Android app stores

### Support
- GitHub Support: https://support.github.com/
- Community Forum: https://github.community/
- Status: https://www.githubstatus.com/

---

## Summary

You've created a secure GitHub Organization with:
- ✅ Two-factor authentication required
- ✅ Restricted base permissions
- ✅ Branch protection rules
- ✅ Security scanning enabled
- ✅ Team-based access control
- ✅ Best practices implemented

**Next Steps**:
1. Create your first repository
2. Invite team members
3. Set up branch protection
4. Start collaborating securely!

---

**Documentation Created**: February 2026  
**Organization**: Nkaizen Consulting Services  
**Contact**: info@nkps.ai

---

**End of Guide**
