# Fedy Do Task Command

Execute the next planned task from the tasks folder.

## Prerequisites

Before running this command, ensure:
1. A Fedy project exists (run `fedy-init` first if not)
2. A task plan exists in the `tasks/` folder (run `fedy-plan-task` first if not)
3. The task file has `- [ ] Not started` status

## Execution Steps

### Phase 1: Find Next Task

#### 1.1 Scan Tasks Folder
- Open the `tasks/` folder
- Find task files matching pattern `<number>.<task-name>.md`
- Sort by number (lowest first)
- Skip `0.task.md` (the initial placeholder)

#### 1.2 Find First Incomplete Task
- Read each task file in order
- Look for `## Status` section
- Find the first task with `- [ ] Not started`
- If no incomplete tasks found, inform the user:
  > "No pending task plans found. Run `fedy-plan-task` to plan the next task."

#### 1.3 Load the Task Plan
- Read the complete task file
- Parse the implementation plan:
  - Summary
  - Files to Modify/Create
  - Implementation Steps
  - Test Plan
  - Dependencies
  - Architecture Alignment

### Phase 2: Preparation

#### 2.1 Read the Architecture
- Open `architecture/high-level-design.md`
- Cross-reference with the task's Architecture Alignment section
- Ensure implementation will follow the design

#### 2.2 Check for Verification Hook
- Check if `hooks/verify-task.md` exists
- If it exists, read the verification steps
- If it does NOT exist, warn the user:
  > "No verify-task hook found. Run `fedy-add-hooks` to configure verification steps."
  > Continue without automated verification, or stop and let user add hooks first.

#### 2.3 Install Dependencies
If the task plan lists dependencies:
- Install required packages using the project's package manager
- Example: `npm install <package>` or `pip install <package>`

### Phase 3: Implementation

#### 3.1 Follow the Implementation Steps
- Execute each step from the task plan in order
- The task file contains detailed instructions for each step
- Follow the code patterns and snippets provided

#### 3.2 Follow Existing Patterns
- Match the coding style of existing files
- Use the same import patterns
- Follow the same file/folder structure

#### 3.3 Add Tests
Follow the Test Plan section from the task file:
- Create test files as specified
- Implement the key test cases listed
- Follow existing test patterns and naming conventions

#### 3.4 Add Logging
Add meaningful logs to help Fedy agents debug issues:
- Log at entry/exit points of important functions (e.g., API handlers, service methods)
- Log key decision points and branch conditions
- Log input parameters and return values for critical operations
- Log errors with context (what was being attempted, relevant IDs/data)
- Use appropriate log levels:
  - `error` - failures that need attention
  - `warn` - unexpected but handled situations
  - `info` - important business events (user actions, state changes)
  - `debug` - detailed technical information for troubleshooting
- Follow existing logging patterns in the codebase

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

#### 5.1 Update Task Status
Mark the task as complete in the task file:

```markdown
## Status
- [x] Completed
```

#### 5.2 Update plan.md
Mark the corresponding task in `plan.md` with `[x]`:

```markdown
- [x] The task that was just completed
```

#### 5.3 Prepare Commit
Ask user if approves and if yes stage and commit. If not, fix issues first.

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

#### 5.4 Summary
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
> Run `fedy-plan-task` to plan the next task, then `fedy-do-task` to execute it."

## Important Rules

1. **Follow the plan** - Execute exactly what's in the task file
2. **One task, one commit** - Never combine multiple tasks
3. **Tests are recommended** - Follow the Test Plan from the task file
4. **Hook verification must pass** - All steps in `hooks/verify-task.md` must pass
5. **Follow the design** - Never deviate from high-level-design.md
6. **Minimal changes** - Only change what's specified in the task plan
7. **No improvisation** - If the plan is unclear, ask the user, don't guess
8. **Add meaningful logs** - Log important operations so Fedy agents can easily diagnose issues

## Error Handling

### No Task Plans Found
> "No task plans found in tasks/. Run `fedy-plan-task` to create a plan for the next task."

### All Tasks Completed
> "All planned tasks are completed. Run `fedy-plan-task` to plan the next task from plan.md."

### No Verify Hook
> "No hooks/verify-task.md found. Run `fedy-add-hooks` to configure verification."

### Verification Failures
If hook steps fail after 3 fix attempts:
> "Unable to resolve verification errors. Please review manually:
> - [List of failing steps and errors]"

### Task Plan Unclear
If the implementation steps are ambiguous:
> "Task plan unclear at step X. Please clarify the following:
> - [Specific question about the plan]"
