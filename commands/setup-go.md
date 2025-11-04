---
description: Initialize a new Go project with minimal scaffolding
argument-hint: [project-name]
model: claude-haiku-4-5
---

# Set up a minimal Go project structure

1. Verify Go is installed (`go version`)
2. Validate project name is provided via $ARGUMENTS (prompt if missing)
3. Initialize Go module: `go mod init $ARGUMENTS`
4. Create `main.go` with Hello World boilerplate
5. Create `.gitignore` for Go projects
6. Verify setup by running `go run main.go`

## File Templates

### main.go

```go
package main

import "fmt"

func main() {
 fmt.Println("Hello, World!")
 fmt.Printf("Welcome to %s!\n", "$ARGUMENTS")
}
```

### .gitignore

```gitignore
# Binaries
*.exe
*.dll
*.so
*.dylib
*.test
*.out

# Dependencies
vendor/

# Build output
bin/

# IDE files
.idea/
.vscode/
.DS_Store

# Environment
.env
```
