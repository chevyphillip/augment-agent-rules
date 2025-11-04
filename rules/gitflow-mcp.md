# GITFLOW & MCP TOOLS

## GITFLOW BRANCHING MODEL

### Primary Branches (Long-lived)

**`main`** - Production-ready code only. Always stable and deployable. Protected. NEVER commit directly.

**`develop`** - Integration branch for ongoing development. Base for all feature branches. Protected. NEVER commit directly.

### Supporting Branches (Short-lived)

**`feature/*`** - Format: `feature/<name>` or `feature/<issue>-<name>`. Branch from `develop`, merge to `develop`, delete after merge.

**`release/*`** - Format: `release/<version>` (SemVer). Branch from `develop`, merge to `main` AND `develop`, delete after merge. Bug fixes only.

**`hotfix/*`** - Format: `hotfix/<version>`. Branch from `main`, merge to `main` AND `develop`, delete after merge. Critical bugs only.

### Workflow Sequences

**Feature Development:**
```bash
git checkout develop && git pull origin develop
git checkout -b feature/new-feature
# ... develop and commit ...
git push origin feature/new-feature
# Create PR: develop ← feature/new-feature
# After merge: delete branch
```

**Release:**
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

**Hotfix:**
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

### Branch Protection

**`main`**: Require 2 PR approvals, status checks, signed commits, no force push, no deletions, restrict push access.

**`develop`**: Require 1 PR approval, status checks, no force push, no deletions.

### Pull Request Requirements

**Feature PRs** (to develop): Conventional commit title (`feat:`, `fix:`), 1+ reviewer, link issues, all tests pass.

**Release PRs** (to main): Title `Release v<version>`, changelog, tech lead approval, all quality gates pass.

**Hotfix PRs** (to main): Title `Hotfix v<version>: <desc>`, root cause analysis, urgent label, rollback plan.

### Merge Strategies

- **Feature → Develop**: Squash and merge (clean history)
- **Release → Main**: Create merge commit with `--no-ff` (preserve release history)
- **Release → Develop**: Create merge commit with `--no-ff`
- **Hotfix → Main**: Create merge commit with `--no-ff`
- **Hotfix → Develop**: Create merge commit with `--no-ff`

### Versioning and Tagging

**Semantic Versioning**: `MAJOR.MINOR.PATCH` (e.g., `1.2.3`)
- MAJOR: Breaking changes
- MINOR: New features (backward compatible)
- PATCH: Bug fixes (backward compatible)

**Tagging**: Always use annotated tags (`git tag -a v1.2.0 -m "Release version 1.2.0"`). Pre-release: `v1.2.0-alpha.1`, `v1.2.0-beta.1`, `v1.2.0-rc.1`.

### Commit Message Forma

`<type>(<scope>): <subject>` where type is `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `chore`, `ci`, or `build`.

**Example**: `feat(auth): add OAuth2 authentication`

### Key Rules

**DO:**
1. Always create feature branches from `develop`
2. Use descriptive branch names (`feature/user-profile-page`, not `feature/fix`)
3. Keep feature branches short-lived (merge within 1-2 weeks)
4. Tag all releases on main
5. Sync develop after hotfixes
6. Keep main always deployable

**DON'T:**
1. NEVER commit directly to main or develop
2. NEVER merge without PR review
3. NEVER force push to main or develop
4. NEVER create feature branches from main
5. NEVER merge develop into main directly (use release branches)
6. NEVER add new features in release branches
7. NEVER skip merging hotfix back to develop

### Conflict Resolution

**Feature branch conflicts with develop:**
```bash
git checkout feature/my-feature
git fetch origin
git rebase origin/develop
# Resolve conflicts
git add .
git rebase --continue
git push --force-with-lease origin feature/my-feature
```

### Emergency Procedures

**Critical Production Bug:**
1. Create hotfix branch from main immediately
2. Fix and test locally
3. Create PR with "URGENT" label
4. Get expedited review (1 approver minimum)
5. Merge to main and deploy
6. Immediately merge to develop
7. Post-mortem within 48 hours

**Rollback:**
```bash
# Revert latest release if broken
git checkout main
git revert HEAD
git push origin main
```

## MCP TOOLS PRIORITY

### Core Principle

**ALWAYS prioritize MCP (Model Context Protocol) tools over standard tools when available.** MCP tools provide enhanced, specialized capabilities.

### Available MCP Servers

**mcpdocker** - Documentation retrieval, code search, web search
- `get-library-docs` - Get up-to-date library documentation
- `resolve-library-id` - Resolve package names to library IDs
- `search_generic_documentation` - Semantic search in repo docs
- `search_generic_code` - Search code in GitHub repositories
- `web_search_exa` - Real-time web search
- `effect_docs_search` - Search Effect documentation

**serena** - Advanced code intelligence
- `find_symbol` - Find symbols by name path
- `find_referencing_symbols` - Find references to symbols
- `replace_symbol_body` - Replace symbol definitions
- `insert_after_symbol` / `insert_before_symbol` - Insert code relative to symbols
- `rename_symbol` - Rename symbols across codebase
- `search_for_pattern` - Flexible regex pattern search
- `write_memory` / `read_memory` - Store/retrieve project information
- `list_dir` / `find_file` - File system operations

**openmemory** - Persistent memory managemen
- `add-memory` - Store new memories
- `search-memories` - Search through stored memories
- `list-memories` - List all memories

**exa** - Advanced web search & code contex
- `web_search_exa` - Real-time web search with content scraping
- `get_code_context_exa` - Get fresh context for APIs, libraries, SDKs

### Priority Order

**Documentation & Research:**
```
1. mcpdocker (get-library-docs, resolve-library-id, search_generic_documentation)
2. exa (get_code_context_exa, web_search_exa)
3. Standard web-search or codebase-retrieval
```

**Code Analysis & Editing:**
```
1. serena (find_symbol, find_referencing_symbols, replace_symbol_body)
2. Standard view, str-replace-editor
```

**Memory & Context:**
```
1. openmemory (search-memories at session start, add-memory when Chevy shares info)
2. serena (write_memory for project-specific knowledge)
3. Standard remember tool
```

**Web Search:**
```
1. exa (web_search_exa for programming-related queries)
2. mcpdocker (web_search_exa alternative)
3. Standard web-search
```

### Tool Selection Rules

**DO:**
1. Start sessions with `openmemory:search-memories`
2. Use `mcpdocker:resolve-library-id` → `mcpdocker:get-library-docs` for dependencies
3. Use serena for symbol-level operations (find, rename, replace)
4. Store learnings: `openmemory:add-memory` (Chevy's preferences), `serena:write_memory` (project patterns)
5. Use exa for fresh programming context
6. Leverage parallel tool execution for independent operations

**DON'T:**
1. Don't use standard tools when MCP alternatives exist
2. Don't skip memory search at session start
3. Don't use basic file operations when serena can do better
4. Don't forget to store learnings
5. Don't use outdated documentation (use `mcpdocker:get-library-docs` or `exa:get_code_context_exa`)
6. Don't rename symbols manually (use `serena:rename_symbol`)

### Tool Selection Decision Process

```
Need documentation?
├─ Library/API docs? → mcpdocker:get-library-docs
├─ GitHub repo docs? → mcpdocker:read_wiki_contents
├─ Code examples? → exa:get_code_context_exa
└─ General web search? → exa:web_search_exa

Need to edit code?
├─ Symbol-level changes? → serena:find_symbol + serena:replace_symbol_body
├─ Rename across files? → serena:rename_symbol
├─ Find references? → serena:find_referencing_symbols
└─ Simple edits? → str-replace-editor

Need to search code?
├─ By symbol name? → serena:find_symbol
├─ By pattern/regex? → serena:search_for_pattern
├─ In GitHub repo? → mcpdocker:search_generic_code
└─ Local files? → view with search_query_regex

Need to remember/recall?
├─ Chevy's preferences? → openmemory:add-memory / search-memories
├─ Project patterns? → serena:write_memory / read_memory
└─ Session context? → Standard remember tool
```

### Workflow Examples

**Researching a Library:**
1. `mcpdocker:resolve-library-id` (library name)
2. `mcpdocker:get-library-docs` (library ID, topic)
3. `exa:get_code_context_exa` (specific API usage examples)

**Refactoring a Function:**
1. `serena:find_symbol` (function name)
2. `serena:find_referencing_symbols` (find all callers)
3. `serena:replace_symbol_body` (update implementation)

**Starting a New Session:**
1. `openmemory:search-memories` (relevant keywords)
2. `serena:get_current_config` (check active project)
3. `serena:check_onboarding_performed`

### Performance Optimization

**Parallel Execution**: When operations are independent, call MCP tools in parallel.
✅ Call `serena:find_symbol` for 3 different symbols simultaneously
✅ Call `mcpdocker:get-library-docs` for multiple libraries at once
❌ Sequential calls when no dependencies exist

**Caching & Reuse**: MCP tools return rich context - analyze thoroughly before making additional calls. Store frequently accessed information in memories.

**Error Handling**: If MCP tool fails, explain failure and fall back to standard tools. Don't silently switch without informing Chevy (user).
