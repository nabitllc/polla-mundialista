> Cloud/sandbox sessions (claude.ai/code): clone `github.com/michsaenz/Mich-Brain2` and follow its `BOOTSTRAP.md` before working. Local sessions ignore this - the global shim already covers it.

## Golden rules and do-not-touch

- **Every change follows Michael's golden rules:** `Mich-Brain2/Playbooks/Maintainability_and_Consistency.md` (design system, UI consistency, code structure, config, agent hygiene, releases, budgets, dependencies, hosting cost). Run the audit block of each section the change touches; any FAIL blocks the merge.
- **Do not touch:** `package-lock.json` except through npm; `node_modules/`.
