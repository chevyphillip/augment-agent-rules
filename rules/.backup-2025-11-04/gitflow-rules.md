---
type: "agent_requested"
description: "Enforces GitHub-specific GitFlow branching model with main/develop branches, feature/release/hotfix workflows, PR requirements, and merge strategies for versioned releases."
---

# GITFLOW WORKFLOW RULES FOR GITHUB

## BRANCH STRUCTURE

### Primary Branches (Long-lived)

**`main`** - Production-ready code only. Always stable and deployable. Protected branch. NEVER commit directly.

**`develop`** - Integration branch for ongoing development. Base for all feature branches. Protected branch. NEVER commit directly.

### Supporting Branches (Short-lived)

**`feature/*`** - Format: `feature/<name>` or `feature/<issue>-<name>`. Branch from `develop`, merge to `develop`, delete after merge.

**`release/*`** - Format: `release/<version>` (SemVer). Branch from `develop`, merge to `main` AND `develop`, delete after merge. Bug fixes only.

**`hotfix/*`** - Format: `hotfix/<version>`. Branch from `main`, merge to `main` AND `develop`, delete after merge. Critical bugs only.

## WORKFLOW SEQUENCES

### Feature Development

```bash
git checkout develop && git pull origin develop
git checkout -b feature/new-feature
# ... develop and commit ...
git push origin feature/new-feature
# Create PR: develop ← feature/new-feature
# After merge: delete branch
```

### Release

```bash
git checkout develop && git pull origin develop
git checkout -b release/1.2.0
# ... version bumps, changelog, bug fixes only ...
git push origin release/1.2.0
# Create PR: main ← release/1.2.0
# After merge to main:
git checkout main && git pull origin main
git tag -a v1.2.0 -m "Release version 1.2.0" && git push origin v1.2.0
git checkout develop && git merge release/1.2.0 && git push origin develop
git branch -d release/1.2.0 && git push origin --delete release/1.2.0
```

### Hotfix

```bash
git checkout main && git pull origin main
git checkout -b hotfix/1.2.1
# ... fix critical bug ...
git push origin hotfix/1.2.1
# Create PR: main ← hotfix/1.2.1 (mark URGENT)
# After merge to main:
git checkout main && git pull origin main
git tag -a v1.2.1 -m "Hotfix version 1.2.1" && git push origin v1.2.1
git checkout develop && git merge hotfix/1.2.1 && git push origin develop
git branch -d hotfix/1.2.1 && git push origin --delete hotfix/1.2.1
```

## BRANCH PROTECTION RULES

**`main`**: Require 2 PR approvals, status checks, signed commits, no force push, no deletions, restrict push access.

**`develop`**: Require 1 PR approval, status checks, no force push, no deletions.

## PULL REQUEST REQUIREMENTS

**Feature PRs** (to develop): Conventional commit title (`feat:`, `fix:`), 1+ reviewer, link issues, all tests pass.

**Release PRs** (to main): Title `Release v<version>`, changelog, tech lead approval, all quality gates pass.

**Hotfix PRs** (to main): Title `Hotfix v<version>: <desc>`, root cause analysis, urgent label, rollback plan.

## MERGE STRATEGIES

- **Feature → Develop**: Squash and merge (clean history)
- **Release → Main**: Create a merge commit with `--no-ff` (preserve release history)
- **Release → Develop**: Create a merge commit with `--no-ff`
- **Hotfix → Main**: Create a merge commit with `--no-ff`
- **Hotfix → Develop**: Create a merge commit with `--no-ff`

## VERSIONING AND TAGGING

**Semantic Versioning**: `MAJOR.MINOR.PATCH` (e.g., `1.2.3`)

- MAJOR: Breaking changes
- MINOR: New features (backward compatible)
- PATCH: Bug fixes (backward compatible)

**Tagging**: Always use annotated tags (`git tag -a v1.2.0 -m "Release version 1.2.0"`). Pre-release: `v1.2.0-alpha.1`, `v1.2.0-beta.1`, `v1.2.0-rc.1`.

## COMMIT MESSAGE CONVENTIONS

**Format**: `<type>(<scope>): <subject>` where type is `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `chore`, `ci`, or `build`.

**Example**: `feat(auth): add OAuth2 authentication`

## DO'S AND DON'TS

### ✅ DO

1. **Always create feature branches from `develop`**
   - Ensures features are based on latest development code

2. **Use descriptive branch names**
   - Good: `feature/user-profile-page`, `hotfix/login-timeout`
   - Bad: `feature/fix`, `my-branch`, `test`

3. **Keep feature branches short-lived**
   - Merge within 1-2 weeks to avoid divergence
   - Rebase regularly from develop to stay current

4. **Write clear PR descriptions**
   - Explain what, why, and how
   - Include screenshots for UI changes
   - List testing steps

5. **Tag all releases on main**
   - Every merge to main should create a version tag
   - Use annotated tags with release notes

6. **Sync develop after hotfixes**
   - Always merge hotfix changes back to develop
   - Prevents regression in next release

7. **Use GitHub Actions for automation**
   - Automate testing, linting, security scans
   - Auto-deploy from main to production
   - Auto-deploy from develop to staging

8. **Keep main always deployable**
   - Main should reflect production state
   - Never merge broken code to main

9. **Document breaking changes**
   - Clearly mark breaking changes in PRs
   - Update CHANGELOG.md
   - Provide migration guides

10. **Use GitHub Projects for release planning**
    - Track features in release milestones
    - Link PRs to project boards
    - Monitor release progress

### ❌ DON'T

1. **NEVER commit directly to main or develop**
   - Always use pull requests
   - Enforce with branch protection rules

2. **NEVER merge without PR review**
   - Even for small changes
   - Maintains code quality and knowledge sharing

3. **NEVER force push to main or develop**
   - Destroys history and causes team issues
   - Use revert commits instead

4. **NEVER create feature branches from main**
   - Features should branch from develop
   - Main is for production code only

5. **NEVER merge develop into main directly**
   - Use release branches for controlled releases
   - Allows final testing and preparation

6. **NEVER leave stale branches**
   - Delete branches after merging
   - Clean up remote branches regularly

7. **NEVER skip CI/CD checks**
   - Wait for all tests to pass
   - Fix failing tests before merging

8. **NEVER merge with unresolved conflicts**
   - Resolve all conflicts locally
   - Test after conflict resolution

9. **NEVER use lightweight tags**
   - Always use annotated tags (`-a` flag)
   - Include release notes in tag message

10. **NEVER hotfix from develop**
    - Hotfixes must branch from main
    - Ensures fix applies to production code

11. **NEVER add new features in release branches**
    - Release branches are for bug fixes only
    - New features go in feature branches

12. **NEVER skip merging hotfix back to develop**
    - Prevents bug reappearing in next release
    - Critical for maintaining code integrity

## GITHUB-SPECIFIC BEST PRACTICES

### GitHub Actions Workflows

**Feature PR Workflow** (`.github/workflows/feature-pr.yml`):

```yaml
name: Feature PR Checks
on:
  pull_request:
    branches: [develop]
    types: [opened, synchronize, reopened]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      # Setup environment (adapt based on project)
      # For Node.js: setup-node, npm/yarn/pnpm/bun
      # For Python: setup-python, pip/uv/poetry
      # For Go: setup-go
      # For Rust: rust-toolchain

      - name: Install dependencies
        run: <package-manager-install-command>

      - name: Run linter
        run: <linter-command>

      - name: Run tests
        run: <test-command>

      - name: Security scan
        run: <security-scan-command>

      - name: Check code coverage
        run: <coverage-command>
```

**Release Workflow** (`.github/workflows/release.yml`):

```yaml
name: Release to Production
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Setup environment
        run: <setup-commands>

      - name: Build artifacts
        run: <build-command>

      - name: Run production tests
        run: <test-command>

      - name: Deploy to production
        run: <deploy-command>

      - name: Create GitHub Release
        uses: actions/create-release@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          tag_name: ${{ github.ref }}
          release_name: Release ${{ github.ref }}
```

**Common Package Manager Commands**:

- **npm**: `npm install`, `npm test`, `npm run lint`, `npm audit`
- **yarn**: `yarn install`, `yarn test`, `yarn lint`, `yarn audit`
- **pnpm**: `pnpm install`, `pnpm test`, `pnpm lint`, `pnpm audit`
- **bun**: `bun install`, `bun test`, `bun run lint`
- **pip**: `pip install -r requirements.txt`, `pytest`, `flake8`, `pip-audit`
- **uv**: `uv pip install -r requirements.txt`, `pytest`, `ruff check`
- **poetry**: `poetry install`, `poetry run pytest`, `poetry run ruff`
- **cargo**: `cargo build`, `cargo test`, `cargo clippy`, `cargo audit`
- **go**: `go mod download`, `go test ./...`, `golangci-lint run`

## AUTOMATION RECOMMENDATIONS

### Automated Branch Cleanup

- Use GitHub Actions to delete merged branches
- Set up Dependabot for dependency updates
- Configure auto-merge for approved PRs (with caution)

### Automated Versioning

- Use semantic-release or similar tools
- Auto-generate CHANGELOG.md from commits
- Auto-create GitHub releases from tags

### Automated Testing

- Run tests on every PR
- Require 80%+ code coverage
- Block merge if tests fail

### Automated Deployment

- Deploy develop → staging environment
- Deploy main → production environment
- Use blue-green or canary deployments

## CONFLICT RESOLUTION STRATEGY

### When Conflicts Occur

1. **Feature branch conflicts with develop**:

   ```bash
   git checkout feature/my-feature
   git fetch origin
   git rebase origin/develop
   # Resolve conflicts
   git add .
   git rebase --continue
   git push --force-with-lease origin feature/my-feature
   ```

2. **Release branch conflicts**:
   - Resolve in favor of release branch (it's closer to production)
   - Test thoroughly after resolution

3. **Hotfix conflicts with develop**:
   - Resolve carefully, may need to cherry-pick
   - Ensure hotfix changes are preserved

## TEAM COLLABORATION GUIDELINES

### Code Review Standards

- Review within 24 hours
- Provide constructive feedback
- Approve only if you'd deploy it yourself
- Test locally for complex changes

### Communication

- Comment on PRs for questions/suggestions
- Use GitHub Discussions for design decisions
- Tag relevant team members
- Update issue status when PR is created

### Release Planning

- Plan releases in 2-4 week sprints
- Freeze develop 1 week before release
- Conduct release retrospectives
- Document lessons learned

## EMERGENCY PROCEDURES

### Critical Production Bug

1. Create hotfix branch from main immediately
2. Fix and test locally
3. Create PR with "URGENT" label
4. Get expedited review (1 approver minimum)
5. Merge to main and deploy
6. Immediately merge to develop
7. Post-mortem within 48 hours

### Rollback Procedure

```bash
# If latest release is broken
git checkout main
git revert HEAD
git push origin main

# Or revert to specific tag
git checkout main
git reset --hard v1.2.0
git push --force origin main  # Use with extreme caution
```

## METRICS AND MONITORING

### Track These Metrics

- Average PR review time
- Number of hotfixes per release
- Code coverage percentage
- Deployment frequency
- Mean time to recovery (MTTR)
- Change failure rate

### GitHub Insights

- Use GitHub Insights to monitor:
  - PR merge frequency
  - Code review participation
  - Branch lifecycle duration
  - Contributor activity

## WHEN TO USE GITFLOW

### ✅ Use GitFlow When

- Managing versioned releases (e.g., v1.0, v2.0)
- Supporting multiple production versions
- Large team with parallel development
- Regulated industry requiring audit trails
- Complex release cycles with QA stages
- Enterprise software with scheduled releases

### ❌ Consider Alternatives When

- Small team (< 5 developers)
- Continuous deployment to production
- Simple web applications
- Rapid iteration required
- Startup/MVP development
- Mobile apps with single production version

### Alternatives to Consider

- **GitHub Flow**: Simpler, single main branch with feature branches
- **Trunk-Based Development**: Continuous integration to main with feature flags
- **GitLab Flow**: Hybrid approach with environment branches

## MIGRATION TO GITFLOW

### From GitHub Flow

1. Create develop branch from main
2. Update branch protection rules
3. Redirect all feature PRs to develop
4. Implement release branch process
5. Train team on new workflow

### From Trunk-Based

1. Create develop branch
2. Introduce feature branches
3. Implement PR review process
4. Add release branches for versioning
5. Gradually increase branch protection

## TROUBLESHOOTING COMMON ISSUES

### Issue: Develop diverged from main

**Solution**: Merge main into develop after each release/hotfix

### Issue: Feature branch too old

**Solution**: Regularly rebase from develop, or recreate branch

### Issue: Merge conflicts in release branch

**Solution**: Resolve in favor of release, test thoroughly

### Issue: Hotfix needed but develop has unreleased features

**Solution**: This is why hotfix branches from main, not develop

### Issue: Forgot to merge hotfix to develop

**Solution**: Cherry-pick the hotfix commits to develop immediately
