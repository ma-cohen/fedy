# Fedy Do Task Command

Execute the next pending task from the plan, implementing it with tests and verification.

## Prerequisites

Before running this command, ensure:
1. A Fedy project exists (run `fedy init` first if not)
2. The `plan.md` file has at least one pending task (`- [ ]`)
3. The `architecture/high-level-design.md` file exists and is populated

## Execution Steps

### Phase 1: Context Gathering

#### 1.1 Read the Plan
- Open `plan.md` and find the **first unchecked task** (`- [ ]`)
- If no pending tasks exist, inform the user:
  > "No pending tasks in plan.md. Run `plan-next` to add new tasks."

#### 1.2 Read the Architecture
- Open `architecture/high-level-design.md`
- Understand the system design, patterns, and constraints
- All implementation MUST adhere to this design

#### 1.3 Scan the Codebase
- Identify relevant existing files related to the task
- Note coding conventions, patterns, and styles in use
- Find existing tests to understand testing patterns

#### 1.4 Read Verification Hook
- Check if `hooks/verify-task.md` exists
- If it exists, read the verification steps defined there
- If it does NOT exist, warn the user:
  > "No verify-task hook found. Run `fedy add-hooks` to configure verification steps."
  > Continue without automated verification, or stop and let user add hooks first.

### Phase 2: Implementation Planning

#### 2.1 Break Down the Task
- Identify specific files to create or modify
- Determine the order of changes
- List any new dependencies needed

#### 2.2 Align with High-Level Design
- Verify the implementation approach matches the architecture
- Use the patterns and structures defined in the design
- If the task conflicts with the design, STOP and ask the user

#### 2.3 Plan Test Coverage
- Identify what tests are needed
- Follow existing test file naming conventions
- Plan for unit tests at minimum

### Phase 3: Implementation

#### 3.1 Make Code Changes
- Implement the task following project conventions
- Keep changes minimal and focused on the single task
- Add clear comments where logic is complex

#### 3.2 Follow Existing Patterns
- Match the coding style of existing files
- Use the same import patterns
- Follow the same file/folder structure

#### 3.3 Add Tests
If the project has a test framework:
- Write unit tests for new functionality
- Follow existing test patterns and naming conventions
- Aim for meaningful coverage, not just line coverage

Test file location patterns:
- `__tests__/` folder (Jest convention)
- `*.test.ts` / `*.spec.ts` alongside source files
- `tests/` folder at project root
- Follow whatever pattern already exists in the project

### Phase 4: Verification

Run verification steps defined in `hooks/verify-task.md`.

#### 4.1 Check for Hook
- If `hooks/verify-task.md` does NOT exist, skip automated verification
- Warn user and proceed to completion (or ask if they want to add hooks first)

#### 4.2 Execute Hook Steps
- Read `hooks/verify-task.md`
- Execute each step in order as defined in the hook
- Each step must pass (exit code 0) before proceeding to the next

#### 4.3 Fix Issues
- If any step fails, fix the issues
- Re-run the failing step
- Repeat until all steps pass

#### 4.4 Verification Complete
- All steps from `verify-task.md` passed
- Ready to proceed to completion

### Phase 5: Completion

#### 5.1 Update plan.md
Mark the completed task with `[x]`:

```markdown
- [x] The task that was just completed
```

#### 5.2 Prepare Commit
Ask user if approves and if yes if not fix and in future come back to this
Stage all changes and suggest a commit message:

```bash
git add .
git commit -m "<type>: <description>"
```

Commit message format:
- `feat:` - new feature
- `fix:` - bug fix
- `refactor:` - code refactoring
- `test:` - adding tests
- `docs:` - documentation
- `chore:` - maintenance

#### 5.3 Summary
Display what was accomplished:

> "Task completed: <task description>
> 
> Changes made:
> - Created/Modified: file1.ts
> - Created/Modified: file2.ts
> - Added tests: file1.test.ts
> 
> Verification (from hooks/verify-task.md):
> - Step 1: ✓
> - Step 2: ✓
> - Step 3: ✓
> 
> Changes committed: `<commit hash>`
> 
> Run `do-task` again for the next task."

## Important Rules

1. **One task, one commit** - Never combine multiple tasks
2. **Tests are recommended** - Add tests if the project has a test framework
3. **Hook verification must pass** - All steps in `hooks/verify-task.md` must pass
4. **Follow the design** - Never deviate from high-level-design.md
5. **Minimal changes** - Only change what's necessary for the task
6. **No new patterns** - Follow existing conventions, don't introduce new ones

## Error Handling

### No Pending Tasks
> "No pending tasks found. Run `plan-next` to generate new tasks."

### Empty High-Level Design
> "The high-level design is empty. Run `fedy create-high-level-design` first to define the architecture."

### No Verify Hook
> "No hooks/verify-task.md found. Run `fedy add-hooks` to configure verification."

### Verification Failures
If hook steps fail after 3 fix attempts:
> "Unable to resolve verification errors. Please review manually:
> - [List of failing steps and errors]"

