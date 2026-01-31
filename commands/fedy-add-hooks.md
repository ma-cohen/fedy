# Fedy Add Hooks Command

Add verification hooks to customize how tasks are validated.

## Prerequisites

Before running this command, ensure:
1. A Fedy project exists (run `fedy-init` first if not)

## Supported Hooks

| Hook | File | Description |
|------|------|-------------|
| `verify-task` | `hooks/verify-task.md` | Runs after task implementation to verify changes |

## Execution Steps

### 1. Create Hooks Folder

If it doesn't exist, create:
```
fedy/<projectName>-fedy/hooks/
```

### 2. Ask User for Hook Configuration

Prompt the user:
> "What verification steps should run after each task? (e.g., run tests, lint, type-check, build)"

### 3. Create verify-task.md

Based on user input, create `hooks/verify-task.md` with the following structure:

```markdown
# Verify Task Hook

Run these verification steps after completing a task.

## Steps

<!-- Each step runs in order. All must pass before task is marked complete. -->

1. **Step Name**
   ```bash
   command to run
   ```
   - Success: exit code 0
   - On failure: fix issues and retry

2. **Step Name**
   ```bash
   another command
   ```

## Notes

- All steps must pass before the task can be marked complete
- If a step fails, fix the issues and re-run verification
```


## Output

After creating the hook, display:

> "Created hooks/verify-task.md
> 
> Verification steps:
> 1. Step 1 name
> 2. Step 2 name
> ...
> 
> These steps will run after each `fedy-do-task` completion."

## Final Structure

After adding hooks:

```
fedy/<projectName>-fedy/
├── plan.md
├── hooks/
│   └── verify-task.md
├── tasks/
│   └── 0.task.md
└── architecture/
    └── high-level-design.md
```

## Updating Hooks

To modify verification steps, either:
- Run `fedy-add-hooks` again (will ask to overwrite)
- Edit `hooks/verify-task.md` directly
