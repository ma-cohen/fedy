# Fedy Plan Next Command

Generate the next 2-3 actionable tasks for the project plan.

## Prerequisites

Before running this command, ensure:
1. A Fedy project exists (run `fedy-init` first if not)
2. The `plan.md` file exists in the project root
3. The `architecture/high-level-design.md` file exists

## Execution Steps

### 1. Gather Context

Read the following files to understand the current state:

1. **plan.md** - Review existing tasks and their completion status
2. **architecture/high-level-design.md** - Understand the system architecture and goals
3. **Codebase** - Scan relevant source files to understand what's already implemented

### 2. Analyze Current State

Determine:
- What tasks are already completed (marked with `[x]`)
- What tasks are pending (marked with `[ ]`)
- What's the next logical step based on the high-level design
- What dependencies exist between components

### 3. Generate Next Tasks

Create **2-3 new tasks** following these rules:

#### Task Requirements

Each task MUST be:
- **One-liner**: A single, clear sentence
- **Atomic**: Does exactly ONE thing
- **Commit-sized**: Results in a single, understandable commit
- **Self-contained**: Can be completed independently (minimal dependencies)
- **Verifiable**: Has a clear "done" state

#### Task Format

```markdown
- [ ] <verb> <what> <where/context>
```

#### Good Examples

```markdown
- [ ] Create User model with email and password fields
- [ ] Add login endpoint to auth controller
- [ ] Write unit tests for password validation
```

#### Bad Examples (Too Big)

```markdown
- [ ] Implement user authentication system  <!-- Too broad, multiple commits -->
- [ ] Build the frontend                     <!-- Way too vague -->
- [ ] Add all API endpoints                  <!-- Multiple features -->
```

### 4. Update plan.md

Append the new tasks to the existing `plan.md` file, maintaining the checklist format:

```markdown
# Plan

<!-- Add one task per line. Complete them one by one. -->

- [x] Completed task 1
- [x] Completed task 2
- [ ] Existing pending task
- [ ] New task 1          <!-- Added by plan-next -->
- [ ] New task 2          <!-- Added by plan-next -->
```

## Output

After updating the plan, display:

> "Added X new tasks to plan.md:
> 
> - [ ] Task 1 description
> - [ ] Task 2 description


## Notes

- Only add tasks that make sense given the current project state
- Prioritize foundational work before features that depend on it
- Consider the natural order: models → services → controllers → UI
- If the high-level design is empty, suggest running `fedy-create-high-level-design` first
