# CLAUDE.md — `drumbeat-web`

*Auto-loaded at the start of every agent session in this repo. `AGENTS.md` is a symlink to this file.*

**This repository is public.** Everything committed is world-readable, permanently, including anything
a later commit deletes. Both authors' email addresses are already in its history.

## What this repo is

A single-page landing site: plain HTML and one stylesheet, no framework, no build step, no
dependencies, and no JavaScript. That is the design, not a stage it is growing out of.

Several sections — customer logos, product graphics, a testimonial, pricing, a build log — are written
and styled but commented out in `index.html`, each carrying a note on what it needs before it can be
uncommented. Filling those in is not a design task. Note that a theme toggle was deliberately left
out: it needs a `<script>` tag, and the Definition of Ready below forbids one without a recorded
decision.

**It is not deployed yet.** GitHub Pages is not enabled — `gh api repos/sigaramendrum/drumbeat-web/pages`
returns 404, and `README.md` lists enabling it as a future step. Do not write instructions or checks
that assume a live site until that changes.

## The rule that matters most

**Nothing confidential goes in this repo.** Not in HTML, not in a comment, not in a commit message,
not in a filename, not in an issue title.

That includes: customer or pilot names, logos and quotes; pricing; roadmap; go-to-market plans;
internal metrics, revenue figures or targets; the architecture, stack or vendors of the product; and
competitive analysis. All of it belongs in the private planning repository instead.

Be careful with implication as well as statement. Copy naming a specific customer profile discloses
the target segment; a commented-out testimonial discloses a pilot; a commit message referencing an
internal decision discloses that the decision happened. A later commit does not undo any of it —
assume anything pushed is captured immediately.

## Guards, described honestly

Secret scanning runs in `.github/workflows/security.yml` and in **two** hooks: `.githooks/pre-commit`
scans the staged diff, and `.githooks/commit-msg` blocks internal references in the message itself.

**The commit-msg hook exists because CI structurally cannot do its job.** gitleaks scans commit
*content*; a commit *message* is never passed to it — verified in a scratch repository whose only
occurrence of a secret was in a subject line, which scanned clean at exit 0. A message is also the
worse of the two to get wrong: it cannot be edited after a push and it renders on the public commit
list. So for that class the hook is not a speed bump backed by CI; it is the only automated guard
there is, and nothing catches what it misses.

**Both hooks are speed bumps against a determined author** — `--no-verify` skips them, and a fresh
clone has neither until `git config core.hooksPath .githooks` is run. `README.md` and
`.githooks/pre-commit` name CI and GitHub's push protection as the controls that hold, which is right
for content and not available for messages.

**Merges made with GitHub's button bypass the message hook entirely.** The merge subject is generated
server-side, so a branch named for an internal ticket lands in history unhooked and unscannable.
Name branches as carefully as commits.

**The CI scan is weaker than those two files imply**, and `.github/workflows/security.yml` records why:
appending a path allowlist to `.gitleaks.toml` in the same pull request being scanned produced "no
leaks found", exit 0. A scanner whose configuration travels in the diff it inspects cannot be an
authority over that diff. Push protection is the one control here that does not travel in the diff.

Two consequences. Do not rely on a green scan as permission to push something borderline. And the hook
only runs if this clone has `core.hooksPath` set — check before your first commit, because a hook
present in the repository but not installed in your checkout protects nothing.

So: rely on push protection, not on a green scan.

## Source of truth

| Question | Authority |
|---|---|
| Whether something may be published | this file. If it is not clearly allowed, it is not allowed |
| Copy, brand, positioning | the private planning repository — approved there, not drafted here |
| Whether a dependency may be added | a recorded decision in the private planning repository |
| What the product actually does | the product repository, and almost none of it belongs on this page |

This repo is a **publication surface, not a source.** Nothing originates here except markup.

## Design principles

1. **Public by default means careful by default.** Assume every push is captured.
2. **No dependencies.** Zero third-party code in a public artefact is a feature. Each addition is a
   supply-chain decision and a privacy decision.
3. **No data collection without a recorded decision.** Analytics, remotely-hosted fonts and embeds all
   disclose visitor IP addresses to a third party.
4. **Boring and static beats clever.** No build step to break, no framework to patch.
5. **Say less than you could.** The page's job is credibility and a contact route.

## Definition of Ready (before starting a change)

- The copy is approved and sourced from the private planning repository, not drafted here.
- The change adds no dependency, script tag or remote asset — or a recorded decision permits it.
- You have confirmed which repository you are in.
- The pre-commit hook is installed in this checkout.

## Definition of Done (before pushing)

- `git diff --cached` read line by line, as a stranger would read it, including the commit message.
- Nothing from the confidentiality list above, by statement or by implication.
- Secret scanning green — read as one signal, not as clearance to publish.
- The page opens locally with no console errors and makes no external network requests.
- No new dependency, and `README.md` still describes the repo accurately.
