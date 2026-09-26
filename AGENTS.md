# Flowbite React

## Map and setup

- `packages/ui/` contains the published React components, themes, helpers, and integration CLI; `packages/cli/` contains the project-creation CLI. Keep public exports and consumer compatibility aligned.
- `apps/storybook/` exercises components; `apps/web/` is the Next.js documentation site. `turbo.json` coordinates workspace tasks; `.changeset/` records release changes.
- Use Bun with the committed `bun.lock`: `bun install --frozen-lockfile`. The root manifest pins Bun 1.3.11, while `.github/actions/setup/action.yml` still installs 1.3.1 plus Node 24. Report that mismatch if reproducing CI; don't silently rewrite the toolchain.
- `bun run dev:ui`, `bun run dev:storybook`, and `bun run dev:web` target the relevant workspace. UI installation prepares generated metadata/plugin CSS; edit their source generators rather than generated output.

## Checks

CI runs `bun run format:check`, `bun run lint`, `bun run typecheck`, `bun run test:coverage`, and `bun run build`. Scope local checks through Turbo filters where appropriate, then satisfy the relevant CI gates. `test:coverage` is the one-shot Vitest suite; UI CLI/helper changes also need `bun test scripts src/cli src/helpers` from `packages/ui/`. The ordinary `test` script enters Vitest watch mode.

Inspect changed component behavior in Storybook, including keyboard operation and relevant themes/sizes; a successful build alone doesn't verify appearance. Avoid `bun run clean`: its `git clean -xdf` deletes ignored and untracked work. Version/release tasks mutate package metadata or publish, so they need the corresponding task authorization.

## Finishing work

Follow the nearby implementation and keep changes within the requested scope. Carry authorized changes through the relevant checks, fixing failures caused by the change. For a bug, reproduce the affected behavior and add a focused regression check when useful. Make routine reversible choices without another approval; ask only when missing information materially changes correctness, scope, or authorization, and name the exact source of any blocking rule. Report what changed, checks actually run, and concrete unverified behavior; repeat checks when new edits or evidence warrant it.
