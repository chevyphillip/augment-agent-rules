---
type: "agent_requested"
description: "Reminds the LLM to prioritize available MCP tools for enhanced capabilities before falling back to standard tools."
---

# MCP TOOLS PRIORITY RULES

## CORE PRINCIPLE

**ALWAYS prioritize MCP (Model Context Protocol) tools over standard tools when available.** MCP tools provide enhanced, specialized capabilities that are more powerful and context-aware than generic alternatives.

## AVAILABLE MCP SERVERS

### 1. **mcpdocker** - Docker MCP Gateway
**Purpose**: Comprehensive documentation retrieval, code search, and web search capabilities

**Available Tools**:
- `ask_question_mcpdocker` - Ask questions about GitHub repositories
- `read_wiki_contents_mcpdocker` - View repository documentation
- `read_wiki_structure_mcpdocker` - Get documentation topics list
- `fetch_generic_documentation_mcpdocker` - Fetch docs for any GitHub repo
- `search_generic_documentation_mcpdocker` - Semantic search in repo docs
- `search_generic_code_mcpdocker` - Search code in GitHub repositories
- `fetch_generic_url_content_mcpdocker` - Fetch content from URLs
- `get-library-docs_mcpdocker` - Get up-to-date library documentation
- `resolve-library-id_mcpdocker` - Resolve package names to library IDs
- `match_common_libs_owner_repo_mapping_mcpdocker` - Match library names to repos
- `web_search_exa_exa` - Real-time web search with Exa AI
- `get_code_context_exa_exa` - Get context for programming tasks
- `convert_time_mcpdocker` - Convert time between timezones
- `get_current_time_mcpdocker` - Get current time in specific timezones
- `effect_docs_search_mcpdocker` - Search Effect documentation
- `get_effect_doc_mcpdocker` - Get Effect documentation by ID

**When to Use**:
- Researching libraries, frameworks, or APIs
- Finding documentation for dependencies
- Searching for code examples in GitHub repos
- Getting real-time information from the web
- Looking up programming patterns and best practices
- Time zone conversions and current time queries

### 2. **serena** - Advanced Code Intelligence
**Purpose**: Sophisticated codebase analysis, symbol manipulation, and code editing

**Available Tools**:
- `list_dir_serena` - List files and directories (with recursion)
- `find_file_serena` - Find files matching patterns
- `search_for_pattern_serena` - Flexible regex pattern search
- `get_symbols_overview_serena` - Get high-level file symbol overview
- `find_symbol_serena` - Find symbols by name path
- `find_referencing_symbols_serena` - Find references to symbols
- `replace_symbol_body_serena` - Replace symbol definitions
- `insert_after_symbol_serena` - Insert code after symbols
- `insert_before_symbol_serena` - Insert code before symbols
- `rename_symbol_serena` - Rename symbols across codebase
- `write_memory_serena` - Store project information
- `read_memory_serena` - Retrieve stored project information
- `list_memories_serena` - List available memories
- `delete_memory_serena` - Delete memory files
- `activate_project_serena` - Activate specific projects
- `get_current_config_serena` - Get agent configuration
- `check_onboarding_performed_serena` - Check onboarding status
- `onboarding_serena` - Perform project onboarding
- `think_about_collected_information_serena` - Reflect on gathered info
- `think_about_task_adherence_serena` - Verify task alignment
- `think_about_whether_you_are_done_serena` - Check completion status
- `initial_instructions_serena` - Get Serena instructions manual

**When to Use**:
- Symbol-level code refactoring (renaming, moving, replacing)
- Finding all references to a function, class, or variable
- Understanding code structure and dependencies
- Precise code insertions relative to existing symbols
- Cross-file symbol analysis
- Storing and retrieving project-specific knowledge
- Complex code navigation and manipulation

### 3. **openmemory** - Persistent Memory Management
**Purpose**: Long-term memory storage and retrieval across conversations

**Available Tools**:
- `add-memory_openmemory` - Store new memories
- `search-memories_openmemory` - Search through stored memories
- `list-memories_openmemory` - List all memories
- `delete-all-memories_openmemory` - Clear all memories

**When to Use**:
- User shares preferences, project conventions, or important context
- User asks you to remember something
- Beginning of new conversation (search for relevant memories)
- User asks about previous conversations or stored information
- Storing learned patterns about the codebase or user workflow

### 4. **exa** - Advanced Web Search & Code Context
**Purpose**: High-quality web search and programming context retrieval

**Available Tools**:
- `web_search_exa_exa` - Real-time web search with content scraping
- `get_code_context_exa_exa` - Get fresh context for APIs, libraries, SDKs

**When to Use**:
- Researching current best practices or recent changes
- Finding examples for specific libraries or frameworks
- Getting up-to-date API documentation
- Searching for solutions to programming problems
- Finding fresh code examples and patterns

## PRIORITY ORDER

### 1. **Documentation & Research**
```
FIRST:  mcpdocker (get-library-docs, resolve-library-id, search_generic_documentation)
SECOND: exa (get_code_context_exa_exa, web_search_exa_exa)
THIRD:  Standard web-search or codebase-retrieval
```

### 2. **Code Analysis & Editing**
```
FIRST:  serena (find_symbol, find_referencing_symbols, replace_symbol_body)
SECOND: Standard view, str-replace-editor
```

### 3. **Memory & Context**
```
FIRST:  openmemory (search-memories at session start, add-memory when user shares info)
SECOND: serena (write_memory for project-specific knowledge)
THIRD:  Standard remember tool
```

### 4. **Web Search**
```
FIRST:  exa (web_search_exa_exa for programming-related queries)
SECOND: mcpdocker (web_search_exa_exa alternative)
THIRD:  Standard web-search
```

## DO'S AND DON'TS

### ✅ DO

1. **Start sessions with memory search**
   ```
   openmemory:search-memories → Check for user preferences and project context
   ```

2. **Use library docs tools for dependencies**
   ```
   mcpdocker:resolve-library-id → mcpdocker:get-library-docs
   ```

3. **Use serena for symbol-level operations**
   ```
   serena:find_symbol → serena:find_referencing_symbols → serena:replace_symbol_body
   ```

4. **Store important learnings**
   ```
   openmemory:add-memory (user preferences, conventions)
   serena:write_memory (project-specific patterns)
   ```

5. **Use exa for fresh programming context**
   ```
   exa:get_code_context_exa_exa for API documentation and examples
   ```

6. **Leverage parallel tool execution**
   ```
   Call multiple MCP tools simultaneously when operations are independent
   ```

7. **Use serena's thinking tools**
   ```
   serena:think_about_collected_information_serena after gathering data
   serena:think_about_task_adherence_serena before major edits
   serena:think_about_whether_you_are_done_serena before completing tasks
   ```

### ❌ DON'T

1. **DON'T use standard tools when MCP alternatives exist**
   - Bad: `web-search` for library docs
   - Good: `mcpdocker:get-library-docs`

2. **DON'T skip memory search at session start**
   - Always call `openmemory:search-memories` with relevant keywords

3. **DON'T use basic file operations when serena can do better**
   - Bad: `view` + manual search for function
   - Good: `serena:find_symbol` with name path

4. **DON'T forget to store learnings**
   - User shares preference → `openmemory:add-memory`
   - Discover project pattern → `serena:write_memory`

5. **DON'T use outdated documentation**
   - Bad: Relying on training data for library APIs
   - Good: `mcpdocker:get-library-docs` or `exa:get_code_context_exa_exa`

6. **DON'T rename symbols manually**
   - Bad: `str-replace-editor` across multiple files
   - Good: `serena:rename_symbol` (handles all references)

7. **DON'T ignore serena's reflection tools**
   - These tools help maintain quality and task alignment

## WORKFLOW EXAMPLES

### Example 1: Researching a Library
```
1. mcpdocker:resolve-library-id (library name)
2. mcpdocker:get-library-docs (library ID, topic)
3. exa:get_code_context_exa_exa (specific API usage examples)
```

### Example 2: Refactoring a Function
```
1. serena:find_symbol (function name)
2. serena:find_referencing_symbols (find all callers)
3. serena:replace_symbol_body (update implementation)
4. serena:think_about_collected_information_serena
```

### Example 3: Starting a New Session
```
1. openmemory:search-memories (relevant keywords)
2. serena:get_current_config_serena (check active project)
3. serena:check_onboarding_performed_serena
```

### Example 4: Finding Code Examples
```
1. exa:get_code_context_exa_exa (specific API or pattern)
2. mcpdocker:search_generic_code_mcpdocker (GitHub repo search)
3. mcpdocker:ask_question_mcpdocker (specific repo questions)
```

## TOOL SELECTION DECISION TREE

```
Need documentation?
├─ Library/API docs? → mcpdocker:get-library-docs
├─ GitHub repo docs? → mcpdocker:read_wiki_contents_mcpdocker
├─ Code examples? → exa:get_code_context_exa_exa
└─ General web search? → exa:web_search_exa_exa

Need to edit code?
├─ Symbol-level changes? → serena:find_symbol + serena:replace_symbol_body
├─ Rename across files? → serena:rename_symbol
├─ Find references? → serena:find_referencing_symbols
└─ Simple edits? → str-replace-editor

Need to search code?
├─ By symbol name? → serena:find_symbol
├─ By pattern/regex? → serena:search_for_pattern_serena
├─ In GitHub repo? → mcpdocker:search_generic_code_mcpdocker
└─ Local files? → view with search_query_regex

Need to remember/recall?
├─ User preferences? → openmemory:add-memory / search-memories
├─ Project patterns? → serena:write_memory / read_memory
└─ Session context? → Standard remember tool
```

## PERFORMANCE OPTIMIZATION

### Parallel Execution
When operations are independent, call MCP tools in parallel:
```
✅ GOOD: Call serena:find_symbol for 3 different symbols simultaneously
✅ GOOD: Call mcpdocker:get-library-docs for multiple libraries at once
❌ BAD: Sequential calls when no dependencies exist
```

### Caching & Reuse
- MCP tools often return rich context - analyze thoroughly before making additional calls
- Store frequently accessed information in memories
- Use serena's memory system for project-specific knowledge

### Error Handling
- If MCP tool fails, explain the failure and fall back to standard tools
- Don't silently switch to standard tools without informing the user
- Verify MCP server availability if tools consistently fail

## INTEGRATION WITH EXISTING RULES

### Works With GitFlow Rules
- Use `serena:rename_symbol` when refactoring during feature development
- Store branch naming conventions in `openmemory`
- Use `mcpdocker` to research libraries before adding dependencies

### Works With Main Agent Rules
- MCP tools enhance the SEARCH phase of the cognitive protocol
- Use serena's thinking tools as part of VALIDATE phase
- Memory tools support context engineering optimizations

## SUMMARY CHECKLIST

### Every Session Start
- [ ] Call `openmemory:search-memories` with relevant keywords
- [ ] Check `serena:get_current_config_serena` if working with projects
- [ ] Review available MCP tools for the current task

### Before Documentation Lookup
- [ ] Try `mcpdocker:get-library-docs` first
- [ ] Use `exa:get_code_context_exa_exa` for fresh examples
- [ ] Fall back to standard tools only if MCP fails

### Before Code Editing
- [ ] Use `serena:find_symbol` to locate code
- [ ] Use `serena:find_referencing_symbols` to check impact
- [ ] Use serena's editing tools for symbol-level changes

### After Learning Something New
- [ ] Store user preferences in `openmemory:add-memory`
- [ ] Store project patterns in `serena:write_memory`
- [ ] Update mental model of available tools and capabilities

