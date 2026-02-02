# quizzing-code

A Claude Code skill that quizzes you about your code changes before PR review. Helps catch issues, validate design decisions, and document your thinking.

## What it does

1. **Analyzes your branch diff** against main (or specified base)
2. **Asks probing questions** one at a time with relevant code snippets
3. **Tracks discoveries** categorized as Critical/Important/Minor
4. **Generates a summary** saved to `.claude/quiz-summaries/`

## Installation

```bash
git clone https://github.com/UniversalAvenue/quizzing-code.git ~/.claude/skills/quizzing-code
```

Restart Claude Code to discover the skill.

## Usage

```
/quizzing-code              # Quiz against main branch
/quizzing-code develop      # Quiz against develop branch
```

Or just say "quiz me on my code" in conversation.

## Example Session

````
You: /quizzing-code

Claude: I'll analyze your branch changes and quiz you on the implementation.

**Question 1/5** - Constraints

Looking at `migrations/add_user_role.sql:12-18`:
```sql
ALTER TABLE users
ADD COLUMN role_id UUID REFERENCES roles(id) ON DELETE SET NULL;
```

This uses `ON DELETE SET NULL`. What happens to users if their role is deleted?

You: They'd lose their role assignment... but we probably want to prevent
     deleting roles that are in use.

Claude: Exactly! Consider using ON DELETE RESTRICT instead, or adding a
        check before role deletion. Let me note that as a finding...

[continues with more questions]

Claude: Here's your summary:

# Code Quiz Summary: feat/add-user-roles
...
````

## Question Categories

The skill probes these areas:

- **Constraints & Invariants** - NULL handling, foreign keys, unique constraints
- **Pattern Consistency** - Does it match existing codebase patterns?
- **Edge Cases** - Empty inputs, concurrency, error conditions
- **Dependencies** - What depends on this? What does this depend on?
- **Naming & Design** - Why these names? Any duplication?

## Output

Summaries are saved to `.claude/quiz-summaries/YYYY-MM-DD-branch-name.md` with:

- Issues by severity (Critical/Important/Minor)
- Code references for each finding
- Validated design decisions
- Recommended action items

## License

MIT
