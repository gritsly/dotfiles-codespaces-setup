# Engineering Rules

## Worktrees
- Create all worktrees under `./worktrees/`.
- Name each worktree directory `<repository>_<branch>`, for example `./worktrees/example_my-branch`.
- These requirements override broader workspace examples that use another path or only the branch name.

## Think Before Acting
- State assumptions explicitly before changing code. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- Do not claim correctness you have not verified.

## Simplicity and Scope
- Solve the problem asked. No adjacent fixes, no speculative features.
- Minimum code that solves the problem. If 200 lines could be 50, rewrite it.
- No abstractions for single-use code, no unrequested flexibility or error handling.
- Ask: "Would a senior engineer call this overcomplicated?" If yes, simplify.

## Surgical Changes
- Touch only what the task requires. Match existing style.
- Don't refactor, reformat, or "improve" unrelated code.
- Remove imports/functions/variables YOUR changes orphaned; leave pre-existing dead code unless asked.
- Mention unrelated issues you notice - don't silently fix them.

## Goal-Driven Execution
- Transform tasks into verifiable goals before starting:
  - "Fix the bug" -> write a test that reproduces it, then make it pass.
- For multi-step tasks, state a brief plan: `[Step] -> verify: [check]`.
- Strong success criteria let you loop independently. Weak criteria require constant clarification.

## Secrets and Destructive Actions
- Confirm before destructive actions or any change involving secrets, credentials, or key handling.
- Never commit or log secrets, API keys, account identifiers, credentials, or PII.

## External Systems
- Never assume an API, database, or external service call succeeded.
- If a result is ambiguous, verify actual state with another read or fail explicitly.
- Do not invent a narrative about what probably happened and act on it as confirmed.
- Read provider documentation before concluding an endpoint or capability doesn't exist.

Anti-patterns to avoid:
- assuming a failed cancel means an order filled or expired
- treating ambiguous error codes as one specific outcome
- swallowing integration failures and continuing as if success is established

## Documentation and Plans
- Update docs in the same session when behavior or architecture changes.
- Do not mark roadmap phases complete without explicit user confirmation.
- Follow project or workspace instructions for plan locations. If none exist, write implementation plans under `.codex/plans/` and create the directory if absent.
- Plans must be executable and decision-complete: build steps, verification steps, constraints.

## Scaffolding
Scaffolding means project workflow assets: skills, subagents, hooks, and plans under `.codex/`, plus tool compatibility symlinks/config in `.claude/` and `.agents/`.

- Propose scaffolding when you judge it useful - recurring workflows, noisy tasks, deterministic safety checks, or anything that would otherwise bloat always-loaded instructions.
- Don't create scaffolding silently; propose it and explain the value.
- If you propose it as part of a solution, implement it in the same session unless the user objects.
- For cross-tool repos: keep shared logic in `.codex/skills/`, `.codex/agents/`, `.codex/hooks/`, and `.codex/plans/`; use symlinks such as `.claude/*` and `.agents/skills` only as tool compatibility layers.
- Zed discovers project skills only from `.agents/skills`; prefer `.agents/skills -> ../.codex/skills` over duplicate skill files.
- First-time checks: Codex sees `.codex` config/skills/hooks, Claude sees symlinked skills/agents/hooks, and Zed sees skills after trusting the worktree.

## Context Economy
- Keep always-loaded instructions compact and cross-project.
- Move reusable workflows into skills instead of growing this file.
- Prefer narrow validation targets and explicit stop conditions over broad "make everything pass" goals.

## Review Standard
Ask before finishing:
- What assumptions am I making?
- What would break this?
- What would a tired maintainer misunderstand?

Use ASD-STE100 Simplified Technical English (STE) for all responses.
