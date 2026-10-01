---
name: use-reviewers
description: Launch independent code reviewers for PR assessment. Use this skill to trigger Dr. Alice Chen (Correctness & Type Safety) and Marcus Rodriguez (Security & Performance) to independently review changes and post verdicts. Reviewers provide domain-specific feedback—Alice catches type errors and correctness issues, Marcus identifies security risks and performance bottlenecks. When reviewers post "Changes Requested", fix pertinent issues (bugs, security gaps, test gaps) and respond in PR threads; skip style nits. Re-request review after fixes. Both must post "Approval Recommended ✓" before merge.
---

# Use Reviewers Skill

## When to Use

- **After PR Creation**: Launch the two reviewers yourself as parallel subagents (Agent tool, one call each, same message). No hooks or settings are involved. Each subagent gets the PR diff, its profile below, and the repo's CLAUDE.md conventions (ruff-only, English-only, tabs kept, TDD), and returns its findings and verdict as text; the launching agent then posts or relays them
- **For Code Quality**: Get specialized feedback before merge
- **Security Reviews**: Marcus targets injection risks, encoding, performance
- **Type Safety**: Alice focuses on correctness, null checks, type errors

## Reviewer Profiles

### Dr. Alice Chen — Correctness & Type Safety
- Finds: Type mismatches, missing imports, logic bugs, validation gaps, test coverage
- Focus: Compile-time safety, null pointer checks, logic correctness
- Verdict: "Approval Recommended ✓" or "Changes Requested ✗"

### Marcus Rodriguez — Security & Performance
- Finds: Injection risks, encoding issues, DOS vectors, N+1 queries, inefficiency
- Focus: Security vulnerabilities, performance bottlenecks, input validation
- Verdict: "Approval Recommended ✓" or "Changes Requested ✗"

### Optional 3rd Reviewers (for large/complex PRs)
- **Dr. Priya Patel** (Architecture & Design): API consistency, module coupling
- **Evan Brooks** (Frontend/UX): Component API, state, accessibility
- **Sam Okoro** (Backend/Data): Database queries, caching, API design

## How It Works

1. PR Created → the main agent launches the reviewers as subagents
2. Two reviewers launch in parallel (Alice + Marcus)
3. Each independently examines changes
4. Both post findings in PR comments
5. Developer assesses findings:
   - **Real bugs** (security, type errors, perf) → Fix
   - **Style nits** (naming, formatting) → Explain
6. Re-request review after fixes
7. Repeat until both post "Approval Recommended ✓"

## Pertinence Assessment

### Fix These ✓ (Real Issues)
- Type mismatches, missing imports
- Security vulnerabilities (injection, encoding, auth)
- Performance bottlenecks (N+1 queries, wasteful computation)
- Logic bugs, missing validation
- Test coverage gaps

### Explain These ✗ (Not Real Issues)
- Style preferences (naming, formatting)
- Refactoring suggestions without bugs
- "Could use X library" suggestions
- Whitespace/indentation preferences
- Comment improvements (separate task)

## Approval Criteria

**Merge only when:**
- ✓ Both reviewers post "Approval Recommended"
- ✓ All tests pass
- ✓ No merge conflicts
- ✓ All pertinent issues addressed

## Example Workflow

(diagrama: PR Created → Alice y Marcus piden cambios → fixes en commits separados
 → respuesta en el hilo → re-request → ambos aprueban → MERGE con CI verde)

## Configuration

Adjust the prompts you pass to the subagents to:
- Change the default reviewer pair (Evan+Alice for frontend, Sam+Marcus for backend)
- Add a 3rd reviewer for large PRs (Dr. Priya Patel)
- Customize reviewer focus areas

See EXAMPLES.md for real-world scenarios.
