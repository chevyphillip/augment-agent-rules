---
type: "agent_requested"
description: "Optimizations for context management, memory usage, tool selection, and state tracking across conversations."
---

# CONTEXT ENGINEERING OPTIMIZATIONS

## CORE PRINCIPLE

**Effective context management is critical for high-quality assistance.** Proper memory usage, tool selection, and state tracking ensure continuity, efficiency, and accuracy across conversations.

## MEMORY MANAGEMENT

### Session Start Protocol

**ALWAYS start every session with memory search:**

```
1. Call openmemory:search-memories with relevant keywords
2. Review user preferences and project conventions
3. Check for related previous work or decisions
4. Identify stored patterns about codebase/workflow
```

### Memory Search Best Practices

**Query with specific, relevant keywords:**
- Project name or technology stack
- Feature area or component name
- User's name or role
- Specific tools or frameworks in use
- Problem domain or task type

**Examples:**
```
Good: "React authentication patterns user preferences"
Good: "Python testing conventions project structure"
Good: "GitFlow branch naming conventions"

Bad: "help"
Bad: "code"
Bad: "previous work"
```

### What to Store in Memory

**User Preferences:**
- Coding style preferences (tabs vs spaces, naming conventions)
- Preferred tools and frameworks
- Communication preferences (verbosity, format)
- Workflow preferences (testing approach, review process)

**Project Structure:**
- Directory organization patterns
- Module/package naming conventions
- File organization principles
- Architecture decisions

**Patterns and Conventions:**
- Common code patterns in the project
- Testing strategies and patterns
- Error handling approaches
- Documentation standards

**Decisions and Rationale:**
- Why certain approaches were chosen
- Trade-offs that were considered
- Rejected alternatives and reasons
- Important constraints or requirements

### Memory Storage Strategy

**Use `openmemory:add-memory` when:**
- User explicitly shares a preference
- User asks you to remember something
- You discover an important project convention
- User corrects your understanding
- A significant decision is made

**Use `serena:write_memory` for:**
- Project-specific technical patterns
- Codebase structure and organization
- Module relationships and dependencies
- Build/test/deploy procedures

**Format for storing memories:**
```
Concise, actionable, specific:
✅ "User prefers functional components over class components in React"
✅ "Project uses pytest with fixtures in conftest.py"
✅ "Feature branches should be named: feature/<issue-number>-<description>"

Avoid vague or generic:
❌ "User likes React"
❌ "Project uses testing"
❌ "Use good branch names"
```

## TOOL USAGE PATTERNS

### Tool Availability Verification

**Before proposing solutions:**

1. **Check MCP server status**
   - Verify which MCP servers are available
   - Check if required tools are enabled
   - Understand tool capabilities and limitations

2. **Verify tool availability**
   - Don't assume tools are available
   - Check tool documentation if uncertain
   - Test with safe operations first

3. **Choose appropriate tools**
   - Use MCP tools when available (see mcp-tools-priority.md)
   - Use Desktop Commander for file operations when available
   - Fall back to bash/shell only when necessary

### Tool Selection Decision Process

```
1. Is there an MCP tool for this? → Use MCP tool (highest priority)
2. Is Desktop Commander available? → Use for file operations
3. Is there a specialized tool? → Use specialized tool
4. Fall back to standard tools → Use with caution
```

### Parallel Tool Execution

**Maximize efficiency with parallel calls:**

```
✅ GOOD: Read 3 files simultaneously
view(file1) + view(file2) + view(file3) in parallel

✅ GOOD: Search multiple memories at once
search-memories(query1) + search-memories(query2) in parallel

✅ GOOD: Multiple independent codebase retrievals
codebase-retrieval(topic1) + codebase-retrieval(topic2) in parallel

❌ BAD: Sequential calls when no dependencies
view(file1) → wait → view(file2) → wait → view(file3)
```

**When to use parallel execution:**
- Reading multiple files
- Searching multiple topics
- Independent codebase retrievals
- Multiple memory searches
- Fetching multiple documentation sources

**When NOT to use parallel execution:**
- Operations have dependencies
- Second operation needs result from first
- Order matters for correctness
- Risk of race conditions

## STATE TRACKING

### Explicit State Awareness

**Maintain clear awareness of:**

1. **Current Task Phase**
   - Analysis, Planning, Implementation, or Validation?
   - What phase are we in right now?
   - What's the next phase?

2. **Completed Work**
   - What has been validated and confirmed?
   - What changes have been made?
   - What decisions have been finalized?

3. **Pending Work**
   - What remains to be done?
   - What's blocked or waiting?
   - What needs user input?

4. **Context and Constraints**
   - What are the requirements?
   - What are the limitations?
   - What are the risks?

### State Tracking Template

```
CURRENT STATE:
├─ Phase: [Analysis/Planning/Implementation/Validation]
├─ Completed: [List of validated steps]
├─ Pending: [List of remaining steps]
├─ Blockers: [What's preventing progress]
└─ Context: [Relevant information]

NEXT STEPS:
1. [Immediate next action]
2. [Following action]
3. [Subsequent action]
```

### Handoff Points

**Clear transitions between phases:**

```
Analysis → Planning
├─ State: "Analysis complete, ready to plan"
├─ Handoff: Present findings and propose approach
└─ Wait: Get user approval before planning

Planning → Implementation
├─ State: "Plan approved, ready to implement"
├─ Handoff: Confirm plan and begin execution
└─ Wait: Get user confirmation before changes

Implementation → Validation
├─ State: "Implementation complete, ready to validate"
├─ Handoff: Present changes for review
└─ Wait: Get user validation before finalizing
```

### Long Conversation Management

**For extended interactions:**

1. **Periodic State Summaries**
   - Every 10-15 exchanges, summarize current state
   - Reconfirm goals and success criteria
   - Check for scope drift

2. **Context Preservation**
   - Store important decisions in memory
   - Reference previous decisions explicitly
   - Maintain thread of reasoning

3. **Scope Validation**
   - Regularly check: "Are we still solving the original problem?"
   - Identify mission creep early
   - Redirect to original intent if needed

4. **Energy Management**
   - Break large tasks into manageable chunks
   - Create natural stopping points
   - Allow user to pause and resume

## CONTEXT OPTIMIZATION STRATEGIES

### Information Gathering

**Efficient context gathering:**

1. **Start broad, then narrow**
   - Begin with high-level understanding
   - Drill down into specifics as needed
   - Don't gather unnecessary detail

2. **Use appropriate tools**
   - codebase-retrieval for broad searches
   - view for specific files
   - serena:find_symbol for precise lookups

3. **Parallel information gathering**
   - Gather multiple pieces of context simultaneously
   - Don't wait for one before requesting another
   - Synthesize after gathering

### Context Reuse

**Avoid redundant context gathering:**

1. **Remember what you've learned**
   - Don't re-read files unnecessarily
   - Reference previous findings
   - Build on existing knowledge

2. **Store reusable context**
   - Save project patterns in memory
   - Document discovered conventions
   - Record architectural decisions

3. **Leverage memories**
   - Check memories before gathering new context
   - Update memories with new learnings
   - Use memories to guide context gathering

## SUMMARY CHECKLIST

### Every Session Start
- [ ] Called `openmemory:search-memories` with relevant keywords
- [ ] Reviewed user preferences and project context
- [ ] Checked for related previous work
- [ ] Identified relevant patterns and conventions

### Before Proposing Solutions
- [ ] Verified MCP server availability
- [ ] Checked tool capabilities
- [ ] Selected appropriate tools
- [ ] Planned for parallel execution where possible

### During Task Execution
- [ ] Tracking current phase explicitly
- [ ] Maintaining list of completed work
- [ ] Tracking pending work and blockers
- [ ] Preserving context across exchanges

### After Significant Work
- [ ] Stored user preferences in memory
- [ ] Documented project patterns
- [ ] Recorded important decisions
- [ ] Updated state tracking

### Long Conversations
- [ ] Periodically summarized state
- [ ] Reconfirmed goals and criteria
- [ ] Checked for scope drift
- [ ] Preserved context in memory

