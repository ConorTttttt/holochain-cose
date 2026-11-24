# Repository controls for `holochain-cose`

This repository will hold the standalone `holochain-cose` implementation (COSE signing and
verification for Holochain agreements). **No implementation exists yet.** Work begins with
task M0-001 (workspace bootstrap) only after its required approvals, signed with
human-controlled keys, have been verified by a protected dispatcher that coding agents cannot
modify.

This file lists the controls the repository is meant to have, and whether each one is
**verified active**. A control written down here is not active until the owner has confirmed
it in GitHub Settings and filled in the "Verified" column.

## Branch and review controls

| Control | Intended setting | How to apply | Verified (owner fills in: date, who) |
|---|---|---|---|
| Default branch | `main` | Created by the initial empty root commit | — |
| Ruleset on `main` | PR required. 1 approval, not by the last pusher. Code-owner review required. Stale approvals dismissed. Review threads resolved. Merge commits only, so reviewed commits are preserved. No force push. No deletion. **No bypass actors** | Settings → Rules → Rulesets → Import `docs/governance/main-ruleset.json` | NOT VERIFIED |
| CODEOWNERS | Owner and an independent reviewer on all paths, especially `.github/` and `docs/governance/` | `.github/CODEOWNERS`, plus adding the reviewer as a collaborator | NOT VERIFIED (reviewer not yet enrolled) |
| Independent reviewer account | A second human GitHub account with write access, distinct from the owner and from any coding-agent identity | Settings → Collaborators | NOT PRESENT |
| Coding-agent identity | Separate from the owner. Feature-branch write only, no admin | GitHub App or machine user | NOT PRESENT. Agents currently act under the owner's account |
| Private vulnerability reporting | Enabled (needed by `SECURITY.md`) | Settings → Code security → Private vulnerability reporting | NOT VERIFIED |

**Single-account caveat.** GitHub doesn't let a PR author approve their own PR. With one
collaborator and the ruleset above, PRs can't be merged without a bypass, and allowing a bypass
defeats the control. A second human reviewer account is therefore a prerequisite for enforcing
review, not an optional extra.

## GitHub Actions controls

| Control | Intended setting | Verified |
|---|---|---|
| Default `GITHUB_TOKEN` permissions | Read repository contents only (Settings → Actions → General) | NOT VERIFIED |
| Actions may create or approve PRs | Disabled | NOT VERIFIED |
| Fork PR workflows | Require approval for all outside contributors | NOT VERIFIED |
| Repository and organisation secrets | None. CI for untrusted changes must not need secrets | NOT VERIFIED (no workflows exist yet) |
| Allowed actions | Only those pinned by full commit SHA in the reviewed CI task (M0-007) | NOT VERIFIED |
| `pull_request_target` | Never used | Enforced by review of M0-007 |

The CI workflow itself is created by task M0-007, not by this governance baseline.

## Separation of duties

- **Development permissions.** Coding agents push to task feature branches and open PRs. They
  never merge, never hold reviewer signing keys, and cannot change rulesets, CODEOWNERS or the
  dispatcher.
- **Protected execution permissions.** The task dispatcher runs from reviewed code in a
  separate, private governance repository, in a protected environment that only the owner can
  approve. It verifies human-signed approvals bound to the exact plan commit, plan-lock digest,
  this repository and the exact implementation commit before a task may start.
- **Human approvals.** These are Ed25519 signatures from keys held by real, enrolled reviewers.
  A PASS file, a checkbox, or a GitHub approval from a shared account is not an approval.

## Reserved paths

The baseline deliberately contains no Cargo manifests, README, licenses, toolchain files, CI
workflows or source files. Those belong to tasks M0-001, M0-007 and later, which create them
under their reviewed file allowlists.
