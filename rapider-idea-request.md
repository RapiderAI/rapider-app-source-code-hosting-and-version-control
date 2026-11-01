<!-- Rapider Official App SOW & Specification | App #101: Source Code Hosting & Version Control (source-control) | Domain: DEV -->

# #101 Source Code Hosting & Version Control (`source-control`) — Scope of Work (SOW)

- **App Number & Slug:** `#101` / `101-source-code-hosting-and-version-control`
- **Proposed Rapider App Key:** `source-control`
- **Suite / Domain Key:** `DEV` (Software Engineering & DevSecOps Suite — `Dev*` Canonical Prefix)
- **Representative Product (`P1`):** GitHub Enterprise
- **Combined Feature Catalog Reference:** [../combined-features/101-source-code-hosting-and-version-control-features.md](../combined-features/101-source-code-hosting-and-version-control-features.md)

---

## 1. Executive Outcome & Target Audience

`source-control` provides the enterprise Git repository hosting, code review diff studio, branch governance, speculative merge queues, supply-chain security scanning, and semantic release control plane for the Rapider `DEV` suite. Modern engineering teams—now shipping higher code volumes than ever alongside AI coding assistants—face three acute bottlenecks: review overload, race-condition `main` branch breakages, and accidental credential or CVE introduction. `source-control` resolves these through path-based `CODEOWNERS` review routing (`DevBranchRule`), speculative Merge Queues synced with `#102` (`ci-cd`), pre-receive secret push protection (`DevSecurityAdvisory`), bidirectional `APM` (`ApmIssue`) traceability, and grounded first-pass AI code review (`DevCodeReviewComment`).

- **SME Fast-Path Defaults:** Small engineering teams create private repositories from Golden Service Templates in seconds with pre-configured `main` branch protection, automatic `APM-123` commit-to-issue linking, push-protection secret scanning, and one-click `SemVer` changelog generation (`DevReleaseTag`).
- **Enterprise Progressive Disclosure:** Regulated engineering organizations enforce workspace-wide inherited compliance rulesets (mandatory signed commits, non-author Segregation-of-Duties approvals, mandatory linked `ApmIssue` change-control gates), speculative Merge Queues preventing broken `main`, CycloneDX/SPDX SBOM & license compliance gates, and DORA coding cycle-time telemetry exported to `#096`.
- **Measurable Success Outcomes:**
  - `100%` pre-receive interception of known secret formats before credentials ever reach Git commit history.
  - Zero broken `main` branch merges from conflicting parallel PRs when Merge Queue mode is enabled.
  - Complete end-to-end auditability from `APM` requirement (`ApmIssue`) -> `DevPullRequest` -> `DevCommitRef` -> `DevReleaseTag` -> `#102` production deployment.
- **Top 6 Global Benchmarks Synthesized:** `P1` GitHub Enterprise, `P2` GitLab, `P3` Atlassian Bitbucket, `P4` Microsoft Azure Repos, `P5` Harness Code Repository, and `P6` Gitea Enterprise.

---

## 2. In-Scope, Out-of-Scope & Suite Ownership Boundaries

### In-Scope
1. Git repository lifecycle (`DevRepository`), Golden Service Templates, Git LFS asset links (`Files`), cross-repo code search, `git blame`, and verified commit history (`DevCommitRef`).
2. Pull request authoring, side-by-side/unified diff viewer, inline review threads & committable suggestions (`DevPullRequest`, `DevCodeReviewComment`), `CODEOWNERS` routing via Home `WorkItem`s, and speculative Merge Queues.
3. Branch and tag protection rulesets (`DevBranchRule`) enforcing required approvers, Segregation-of-Duties self-approval bans, signed commits, linked `ApmIssue` checks, and `#102`/`#103` status gates.
4. Pre-receive secret push protection, lockfile SCA vulnerability advisories, SBOM exports, license checks (`DevSecurityAdvisory`), and annotated `SemVer` releases (`DevReleaseTag`).
5. Grounded AI PR walkthroughs and inline first-pass code review suggestions over `projectChannelMessage` / `DevCodeReviewComment`.

### Out-of-Scope
1. Agile backlog boards, epics, sprint planning, and engineering bug/issue ticketing — owned by `APM` (`apm`, `planning`: `ApmProject`, `ApmEpic`, `ApmSprint`, `ApmIssue`). `source-control` links to `ApmIssue` rather than building a duplicate issue tracker!
2. Build container execution, test runners, artifact promotion, and deployment rollouts — owned by `#102 Continuous Integration / Deployment (CI/CD)` (`ci-cd`).
3. Interactive REST/GraphQL/gRPC client testing, mock servers, and synthetic API monitors — owned by `#103 API Development & Testing` (`api-testing`).

### Cross-Suite Ownership & Integration Contract

| Domain / App | Owns (Single Source of Truth) | Integration Contract with `source-control` (`#101`) |
| :--- | :--- | :--- |
| **`source-control` (`#101`)** | `DevRepository`, `DevCommitRef`, `DevPullRequest`, `DevCodeReviewComment`, `DevBranchRule`, `DevReleaseTag`, `DevSecurityAdvisory` | Publishes webhook/Kafka push, PR, merge-queue, and release events to `#102`, `#103`, `#098`, and `APM`. |
| **`APM` (`apm`, `planning`)** | `ApmProject`, `ApmEpic`, `ApmSprint`, `ApmIssue` | Smart commit/branch/PR parser extracts `APM-*` keys, validates `requireLinkedIssue` branch rules, and emits `dev.scm.pr.merged.v1` so `APM` transitions linked issues cleanly. |
| **`#102` CI/CD (`ci-cd`)** | `DevPipelineDefinition`, `DevPipelineRun`, `DevDeploymentRecord` | Triggers CI runs on PR head SHAs & Merge Queue batches; consumes `dev.cicd.pipeline_run.completed.v1` to unlock `DevBranchRule` status gates. |
| **`#103` API Testing (`api-testing`) & `#098` ETL** | `DevApiSpec`, `DevApiTestRun`, `DatTransformationModel` | Hosts versioned OpenAPI specs (`#103`) and SQL transformation models (`#098`); enforces API contract / SQL dry-run status checks on PRs. |
| **Rapider Platform Services** | Identity/RBAC, Secret Vault, Files/Drive, Home `WorkItem`s, `projectChannelMessage` | Stores LFS & release binaries in Drive (`Files`); routes PR reviews and critical CVEs/secret alerts to Home `WorkItem`s; stores PR conversations in `projectChannelMessage`. |

---

## 3. Primary Web Pages & Sidebar Navigation Architecture (12 Pages)

All visible sidebar sections enforce **`>= 3` visible items (`displayOnMenu: true`)**, while detail views, diff studios, and drawers set `displayOnMenu: false`.

| # | Route | Page Title | Sidebar Section | Menu Visible | Key Components & UX |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | `scm-overview` | Developer Engineering Hub | `Code & Reviews` | `true` (Order 1) | Assigned PR reviews ribbon, open PRs authored, DORA review cycle-time KPIs, critical security advisory banner, `#activeFilterTagsBar`. |
| 2 | `repositories` | Repositories | `Code & Reviews` | `true` (Order 2) | `findSWR` directory of `DevRepository` records with visibility/template/language filter pills, `#activeFilterTagsBar` + `Clear all`, clickable repo slug, icon+text `Edit`, icon-only `Delete`. |
| 3 | `pull-requests` | Pull Requests | `Code & Reviews` | `true` (Order 3) | Cross-repo `DevPullRequest` queue (`OPEN`, `DRAFT`, `QUEUED`, `MERGED`, `CLOSED`), CI check badges, risk score pill, clickable PR title, icon+text `Edit`, icon-only `Delete`. |
| 4 | `merge-queue` | Speculative Merge Queue | `Code & Reviews` | `true` (Order 4) | Live ordered queue of `QUEUED` PRs per target branch, cumulative `#102` CI batch status, estimated time-to-merge, and `Eject from Queue` action. |
| 5 | `code-search` | Cross-Repo Code Search | `Commits & Releases` | `true` (Order 5) | Symbol, path, and regex search studio across authorized `DevRepository` trees with syntax-highlighted context snippets and jump-to-line links. |
| 6 | `commit-ledger` | Commits & Traceability | `Commits & Releases` | `true` (Order 6) | Searchable `DevCommitRef` history showing verified GPG/SSH badges, author, linked `ApmIssue` chips, and `#102` deployment environment badges. |
| 7 | `releases-tags` | Releases & Changelogs | `Commits & Releases` | `true` (Order 7) | `DevReleaseTag` catalog (`SemVer`), auto-compiled markdown changelogs, SHA-256 verified Drive asset links, and `"Draft New Release"` launcher. |
| 8 | `branch-rulesets` | Branch & CODEOWNERS Rules | `Governance & Security` | `true` (Order 8) | Workspace and repo `DevBranchRule` table showing protected globs (`main`, `release/*`), required approvers, SoD self-approval ban, and required `#102/#103` checks. |
| 9 | `security-advisories` | Secret & Dependency Security | `Governance & Security` | `true` (Order 9) | `DevSecurityAdvisory` triage view (`<rapider-kanban>` + synchronized table) for push-blocked secrets, SCA CVEs (`Decimal128` CVSS), and license alerts. |
| 10 | `sbom-compliance` | SBOM & Change Audit | `Governance & Security` | `true` (Order 10) | CycloneDX/SPDX package inventory exporter and SOX/SOC 2 PR Segregation-of-Duties compliance report (`100%` linked `ApmIssue` & independent approver check). |
| 11 | `repo-code-browser` | Repository Tree & Blame View | *(Hidden)* | `false` | Branch/tag selector (`#/repo-code-browser?id=...`), directory tree, file syntax viewer + `git blame` toggle, `README.md` renderer, and `Back to Repositories`. |
| 12 | `pull-request-detail` | Pull Request Diff & Review Studio | *(Hidden)* | `false` | Tabbed PR studio (`#/pull-request-detail?id=...`): Conversation (`projectChannelMessage` + AI Walkthrough), Files Changed (split/unified diff + inline `DevCodeReviewComment`s), Checks (`#102/#103`), Merge/Queue button, plus sliding `pr-rule-drawer` (`displayOnMenu: false`) for PR/Rule creation & editing. |

---

## 4. Canonical Data Models (`Dev*` — 7 Models)

All models strictly omit `projectId`, store zero raw leaked secrets or mirror passwords, use platform UUID `id` + optimistic `version`, and use `Decimal128` for CVSS & quota metrics.

| Model Name | Key Fields & Types | Relationships & External Refs | Invariants & Lifecycle |
| :--- | :--- | :--- | :--- |
| **`DevRepository`** | `id: UUID`, `slug: string`, `name: string`, `description: string`, `visibility: enum(PRIVATE, INTERNAL)`, `defaultBranch: string`, `isTemplate: boolean`, `templateRepoId: UUID?`, `allowedMergeStrategies: enum(MERGE_COMMIT, SQUASH, REBASE_LINEAR)[]`, `enableMergeQueue: boolean`, `secretPushProtection: boolean`, `mirrorVaultSecretRef: string?`, `storageUsedMb: Decimal128`, `apmProjectId: UUID?`, `ownerDepartmentId: UUID?`, `status: enum(ACTIVE, ARCHIVED)`, `version: int` | `apmProjectId -> ApmProject.id` (`APM`); `ownerDepartmentId -> HrDepartment.id`. | `slug` unique per workspace. `ARCHIVED` repositories become strictly read-only (pushes, PR merges, and rule deletions rejected). |
| **`DevBranchRule`** | `id: UUID`, `repositoryId: UUID?`, `scopeLevel: enum(WORKSPACE_INHERITED, REPOSITORY)`, `branchPattern: string`, `minApprovals: int`, `blockAuthorSelfApproval: boolean`, `dismissStaleReviewsOnPush: boolean`, `requireCodeOwnersReview: boolean`, `codeOwnersRulesJson: {pathGlob, reviewerGroupOrUserIds}[]`, `requiredStatusChecks: string[]`, `requireLinkedApmIssue: boolean`, `requireSignedCommits: boolean`, `blockForcePush: boolean`, `isEnabled: boolean`, `version: int` | `repositoryId -> DevRepository.id` (nullable when `WORKSPACE_INHERITED`). | Repository-level rules can tighten (`minApprovals >= inherited`) but NEVER weaken an active `WORKSPACE_INHERITED` `DevBranchRule`. |
| **`DevCommitRef`** | `id: UUID`, `repositoryId: UUID`, `commitSha: string`, `parentShas: string[]`, `branchName: string`, `authorUserId: UUID?`, `authorEmail: string`, `messageHeadline: string`, `messageBody: string?`, `signatureVerified: boolean`, `linkedApmIssueIds: UUID[]`, `filesChangedCount: int`, `additions: int`, `deletions: int`, `committedAt: datetime` | `repositoryId -> DevRepository.id`; `linkedApmIssueIds -> ApmIssue.id[]` (`APM`). | Immutable append-only Git commit metadata indexed on `(repositoryId, commitSha)`. |
| **`DevPullRequest`** | `id: UUID`, `repositoryId: UUID`, `prNumber: int`, `title: string`, `descriptionMarkdown: string`, `sourceBranch: string`, `targetBranch: string`, `headCommitSha: string`, `baseCommitSha: string`, `status: enum(DRAFT, OPEN, QUEUED, MERGED, CLOSED)`, `riskLevel: enum(LOW, MEDIUM, HIGH)`, `authorUserId: UUID`, `requiredReviewerIds: UUID[]`, `approvedByUserIds: UUID[]`, `unresolvedThreadCount: int`, `checksStatus: enum(PENDING, PASSED, FAILED)`, `linkedApmIssueIds: UUID[]`, `mergeQueuePosition: int?`, `mergedByUserId: UUID?`, `mergedAt: datetime?`, `discussionChannelId: UUID?`, `activeWorkItemIds: UUID[]`, `version: int` | `repositoryId -> DevRepository.id`; `linkedApmIssueIds -> ApmIssue.id[]`; `discussionChannelId -> projectChannelMessage`; `activeWorkItemIds -> Platform WorkItem.id[]`. | `(repositoryId, prNumber)` monotonically increasing & unique. Transition to `MERGED` or `QUEUED` is deterministically blocked unless all matching `DevBranchRule` invariants pass (`approvedByUserIds` excluding `authorUserId` when `blockAuthorSelfApproval: true`, `unresolvedThreadCount === 0`, `checksStatus === 'PASSED'`, and `linkedApmIssueIds.length >= 1` when required). |
| **`DevCodeReviewComment`** | `id: UUID`, `pullRequestId: UUID`, `commitSha: string`, `filePath: string`, `startLine: int`, `endLine: int`, `authorType: enum(HUMAN, GROUNDED_AI)`, `authorUserId: UUID?`, `reviewDecision: enum(COMMENT, APPROVE, REQUEST_CHANGES)?`, `bodyMarkdown: string`, `suggestedCodePatch: string?`, `threadStatus: enum(OPEN, RESOLVED, OUTDATED)`, `resolvedByUserId: UUID?`, `version: int` | `pullRequestId -> DevPullRequest.id`. | `authorType === 'GROUNDED_AI'` is strictly prohibited from submitting `reviewDecision: 'APPROVE'`; only authenticated `HUMAN` reviewers count toward `minApprovals`. |
| **`DevReleaseTag`** | `id: UUID`, `repositoryId: UUID`, `tagName: string`, `targetCommitSha: string`, `title: string`, `changelogMarkdown: string`, `isPrerelease: boolean`, `assetFileIds: UUID[]`, `assetChecksumsJson: {fileId, sha256}[]`, `publishedByUserId: UUID`, `publishedAt: datetime`, `version: int` | `repositoryId -> DevRepository.id`; `assetFileIds -> Platform Drive File.id[]`. | `(repositoryId, tagName)` immutable once published (cannot repoint a published release tag SHA without creating a new `SemVer` patch tag). |
| **`DevSecurityAdvisory`** | `id: UUID`, `repositoryId: UUID`, `pullRequestId: UUID?`, `advisoryType: enum(SECRET_PUSH_BLOCKED, SECRET_HISTORICAL, DEPENDENCY_CVE, LICENSE_VIOLATION)`, `severity: enum(LOW, MEDIUM, HIGH, CRITICAL)`, `cvssScore: Decimal128?`, `packageOrDetectorName: string`, `cveOrRuleId: string`, `filePath: string`, `maskedFingerprint: string`, `remediationVersion: string?`, `status: enum(OPEN, IN_REMEDIATION, FIXED, ACCEPTED_RISK, FALSE_POSITIVE)`, `activeWorkItemId: UUID?`, `version: int` | `repositoryId -> DevRepository.id`; `pullRequestId -> DevPullRequest.id`; `activeWorkItemId -> Platform WorkItem.id`. | **Zero-secret leak invariant:** `maskedFingerprint` stores only redacted prefix/hash (e.g., `AKIA****...7F2Q`), NEVER the raw detected secret string. `ACCEPTED_RISK` requires `Security Admin` role + justification reason. |

---

## 5. Core End-to-End User Journeys (6 Journeys)

1. **Journey 1 — Pre-Receive Secret Push Protection & Advisory Remediation:**
   - A developer attempts to push a commit containing an accidental API token to `feature/APM-208-billing-webhook`.
   - `source-control`'s pre-receive scanner (`SCM-019`) blocks the push synchronously before the blob enters Git history, logs a `SECRET_PUSH_BLOCKED` `DevSecurityAdvisory` with a redacted `maskedFingerprint` (`sk_live_****...91ab`), and guides the developer to bind a Platform Secret Vault reference instead.
2. **Journey 2 — Pull Request Authoring, AI First-Pass Review & `CODEOWNERS` Home `WorkItem`:**
   - Opening a `DevPullRequest` from `feature/APM-208-billing-webhook` -> `main` automatically links `ApmIssue` `APM-208`, triggers `#102` CI and `#103` API contract checks, posts an AI Walkthrough (`SCM-025`) in `projectChannelMessage`, and adds an inline `GROUNDED_AI` `DevCodeReviewComment` flagging a float field that should use `Decimal128` (`SCM-026`).
   - Matching `CODEOWNERS` (`/billing/**`) assigns the Principal Fintech Engineer and creates a Rapider Home `WorkItem`.
   - The author clicks `"Commit Suggestion"` on the inline `Decimal128` patch; `#102` CI turns `PASSED`, the human reviewer clicks `Approve` (resolving all threads and closing the `WorkItem`).
3. **Journey 3 — Speculative Merge Queue Protecting `main`:**
   - Rather than direct merging while 3 other PRs are landing on `main`, the engineer clicks `"Enqueue to Merge"` (`SCM-011`).
   - `merge-queue` constructs a speculative cumulative ref (`main + PR #41 + PR #42`), runs `#102` validation, fast-forwards both cleanly into `main`, emits `dev.scm.pr.merged.v1` (auto-transitioning `APM-208` in `APM`), and deletes the head branch.
4. **Journey 4 — Click-Through Repo/PR Navigation & Back-State Restoration:**
   - On `pull-requests`, an Engineering Manager filters by `Risk Level: "HIGH"` and `Status: "OPEN"` (`#activeFilterTagsBar` displays both chips + `[Clear all]`), clicks PR `#42` title to inspect `#/pull-request-detail?id=...`, reviews the diff, and clicks `"Back to Pull Requests"`, restoring both active filter chips seamlessly.
5. **Journey 5 — Dependency CVE Kanban Triage & Auto-Bump PR:**
   - A new `CRITICAL` (`cvssScore: 9.8`) CVE is published affecting a transitive lockfile package. `security-advisories` (`<rapider-kanban>` + table) surfaces the `DEPENDENCY_CVE` card, opens a Security Home `WorkItem`, and clicks `"Generate Upgrade PR"` (`SCM-022`), opening a pre-tested `DevPullRequest` bumping the lockfile to `remediationVersion`.
6. **Journey 6 — Immutable `SemVer` Release & CI/CD Deployment Handoff:**
   - From `releases-tags`, a Release Manager drafts `v3.2.0`, auto-compiling categorized notes from merged PRs and `ApmIssue`s (`SCM-021`), publishes the immutable `DevReleaseTag`, and triggers `#102` (`ci-cd`) staging-to-production deployment while placing a release marker on `#097` (`product-analytics`).

---

## 6. Consolidated Feature Priorities & Work Packages (`P0`, `P1`, `P2`)

### `P0` Foundation Work Packages
- **`WP-P0-1` Repositories, Code Browser, Blame & Smart `APM` Links (`SCM-001`, `SCM-002`, `SCM-003`):** Build `scm-overview`, `repositories`, `repo-code-browser`, and `commit-ledger` with LFS Drive links, GPG/SSH verification badges, and `APM-*` commit/branch linking (`Developer`).
- **`WP-P0-2` Pull Request Studio, Inline Reviews & `CODEOWNERS` `WorkItem`s (`SCM-007`, `SCM-008`, `SCM-009`, `SCM-010`):** Build `pull-requests` and `pull-request-detail` with split/unified syntax diffs, committable code suggestions, thread resolution gates, and Home `WorkItem` reviewer routing (`Developer` / `Reviewer`).
- **`WP-P0-3` Branch Protection & CI/API Status Gates (`SCM-014`, `SCM-015`):** Build `branch-rulesets` blocking force-pushes and gating merges on `#102` and `#103` commit status checks (`Repo Maintainer`).
- **`WP-P0-4` Pre-Receive Secret Blocking, Lockfile SCA & Releases (`SCM-019`, `SCM-020`, `SCM-021`):** Build `security-advisories` (`<rapider-kanban>` + table) and `releases-tags` with zero-plaintext secret push blocking, `Decimal128` CVSS SCA alerts, and `SemVer` changelog releases (`AppSec` / `Release Manager`).

### `P1` Scale & Enterprise Governance Work Packages
- **`WP-P1-1` Templates, Code Search, Mirrors, Merge Queue & Revert/Cherry-Pick (`SCM-004`, `SCM-005`, `SCM-006`, `SCM-011`, `SCM-012`):** Build `code-search`, `merge-queue`, Golden Service Templates, vault-backed Git mirrors, and 1-click revert/cherry-pick PRs (`Staff Engineer`).
- **`WP-P1-2` SOX/SOC 2 Change Gates, SoD Self-Approval Ban, SBOM & DORA Metrics (`SCM-016`, `SCM-017`, `SCM-018`, `SCM-022`, `SCM-023`, `SCM-024`):** Build `sbom-compliance`, workspace-inherited rulesets, mandatory `ApmIssue` gates, auto-bump PRs, license checks, and `#096` DORA telemetry (`Engineering Director` / `Compliance`).

### `P2` Adjacent & Grounded AI Work Packages
- **`WP-P2-1` AI PR Walkthrough, First-Pass Code Review & PR Risk Badge (`SCM-013`, `SCM-025`, `SCM-026`):** Automated PR blast-radius scoring, `projectChannelMessage` walkthrough summaries, and non-approving inline AI bug/security review comments (`All Developers`).
- **`WP-P2-2` Explain-This-File Assistant & Release Notes Formatter (`SCM-027`, `SCM-028`):** Commit/issue-grounded file architecture Q&A and customer-ready release note drafting (`Developer` / `Product Manager`).

---

## 7. Roles, Permissions & Field-Level Governance

| Role | Permitted Actions | Restricted / Field-Level Boundaries |
| :--- | :--- | :--- |
| **`Repo Reader / Auditor`** | Browse permitted `DevRepository` code, commits, PR diffs, `sbom-compliance` reports, and releases. | Cannot push branches, post merge approvals, or view `mirrorVaultSecretRef` metadata. |
| **`Developer / Contributor`** | Clone/push feature branches, open `DevPullRequest`s, post `DevCodeReviewComment`s, review peer PRs (where not author). | Cannot push directly or force-push to protected branches, cannot self-approve own PR when `blockAuthorSelfApproval: true`, and cannot bypass secret push protection. |
| **`Code Owner / Senior Reviewer`** | Submit authoritative `APPROVE` / `REQUEST_CHANGES` reviews on owned `CODEOWNERS` paths, enqueue passing PRs to `merge-queue`, draft `DevReleaseTag`s. | Cannot weaken `WORKSPACE_INHERITED` `DevBranchRule`s or accept `CRITICAL` `DevSecurityAdvisory` risks unilaterally. |
| **`Repository Maintainer`** | Configure repo settings, `CODEOWNERS`, templates, and stricter repo-level `DevBranchRule`s; archive obsolete repos. | Bound by workspace compliance baselines; cannot delete immutable `DevCommitRef` audit history or published `DevReleaseTag`s. |
| **`AppSec & SCM Administrator`** | Author `WORKSPACE_INHERITED` `DevBranchRule`s, manage secret detector rules, adjudicate `ACCEPTED_RISK` on `DevSecurityAdvisory` items with audit reason. | Break-glass emergency merges require dual-control/justification and trigger immediate compliance audit notifications. |

---

## 8. Technical APIs, Guarded Commands, Events & `requiredWorkflows`

### Guarded Commands
1. `dev.scm.evaluatePreReceivePush({ repositoryId, branchName, commits, idempotencyKey, correlationId })` — Synchronously enforces `DevBranchRule` (force-push ban, signed commit check) and pre-receive secret push protection before accepting refs.
2. `dev.scm.submitPullRequestReview({ pullRequestId, decision, inlineComments, expectedVersion, idempotencyKey, correlationId })` — Enforces `authorType === 'HUMAN'` and `reviewerId !== authorUserId` (when SoD enabled), updates `approvedByUserIds`, and resolves reviewer Home `WorkItem`s.
3. `dev.scm.enqueueOrMergePullRequest({ pullRequestId, strategy, expectedVersion, idempotencyKey, correlationId })` — Deterministically verifies all `DevBranchRule` invariants (`checksStatus === 'PASSED'`, `unresolvedThreadCount === 0`, approvals, `ApmIssue` link) before merging or placing in `merge-queue`.
4. `dev.scm.publishReleaseTag({ repositoryId, tagName, targetCommitSha, changelogMarkdown, assetFileIds, idempotencyKey, correlationId })` — Computes SHA-256 asset digests and creates immutable `DevReleaseTag`.
5. `dev.scm.adjudicateSecurityAdvisory({ advisoryId, targetStatus, justification, expectedVersion, idempotencyKey, correlationId })` — Updates `DevSecurityAdvisory` state and resolves linked Home `WorkItem`s.

### Domain Events Published & Consumed (Kafka)
- **Published:** `dev.scm.push.accepted.v1`, `dev.scm.pull_request.opened.v1`, `dev.scm.pull_request.merged.v1`, `dev.scm.merge_queue.batch_ready.v1`, `dev.scm.release.published.v1`, `dev.scm.security_advisory.detected.v1`.
- **Consumed:** `dev.cicd.pipeline_run.completed.v1` (from `#102` to update `checksStatus` and advance `merge-queue`), `dev.api.test_run.completed.v1` (from `#103` for contract status gates), `apm.issue.updated.v1` (from `APM`).

### `requiredWorkflows`
- `scmPullRequestReviewRoutingWorkflow` — Evaluates `CODEOWNERS` diff matches and creates/updates Reviewer Home `WorkItem`s.
- `scmSpeculativeMergeQueueWorkflow` — Coordinates batched speculative `#102` CI runs, bisects/ejects failing PRs, and fast-forwards passing refs.
- `scmDependencyScaAndSbomWorkflow` — Parses lockfiles on push/CVE feed updates, generates CycloneDX/SPDX SBOMs, and escalates `HIGH/CRITICAL` advisories to Home `WorkItem`s.

### Grounded Rapider AI Governance Rules
- AI code review (`SCM-026`) is strictly advisory (`authorType: 'GROUNDED_AI'`), **CANNOT** call `APPROVE` or `enqueueOrMergePullRequest`, never ingests vault secrets into prompt context, and cites exact diff file/line locations and repository coding rules.

---

## 9. Security, Privacy, Compliance & Non-Functional Requirements

- **SOX / SOC 2 Type II / ISO 27001 / SLSA Supply-Chain Compliance:** Enforces Segregation of Duties (`blockAuthorSelfApproval`), mandatory change-ticket linkage (`requireLinkedApmIssue`), cryptographic GPG/SSH commit signature verification, and verifiable SBOM + SHA-256 release asset provenance.
- **Zero-Secret Persistence Guarantee:** Pre-receive blocked secrets and historical secret findings NEVER persist raw credentials in `DevSecurityAdvisory`, logs, or `projectChannelMessage` threads (`maskedFingerprint` only); upstream Git mirror tokens use `mirrorVaultSecretRef` exclusively.
- **Precision & Host Tenancy:** `Decimal128` enforced on `cvssScore` and `storageUsedMb`; zero `projectId` fields on models.
- **Accessibility & Responsive UX:** `<rapider-kanban>` on `security-advisories` syncs live with its table view; accessible keyboard diff navigation (`n`/`p` file/hunk jump), screen-reader +/- line prefix labels (not color-only green/red), `#ffffff !important` white text on primary/danger buttons across `390px`, `768px`, and `1440px`.

---

## 10. Deterministic Acceptance Criteria (15 Testable Criteria)

1. **AC-01 (Sidebar Minimum-3 Rule):** Visible sidebar sections (`Code & Reviews`: 4 items, `Commits & Releases`: 3 items, `Governance & Security`: 3 items) each contain `>= 3` `displayOnMenu: true` pages; `repo-code-browser`, `pull-request-detail`, and `pr-rule-drawer` set `displayOnMenu: false`.
2. **AC-02 (Rapider Kanban & Multi-View Sync):** `security-advisories` renders `<rapider-kanban>` synchronized with its table view; updating an advisory status optimistically refreshes both views immediately.
3. **AC-03 (SWR Clean Queries & Defensive Soft-Delete):** All 10 sidebar routes load via `findSWR`/`countSWR` with zero `isDeleted` client where-clauses and apply in-memory `item.isDeleted !== true` filtering.
4. **AC-04 (Table Actions & Real Click-Through):** Zero `"View"` buttons exist in `Actions` columns; clicking a repo slug on `repositories` navigates to `#/repo-code-browser?id=...` and clicking a PR title on `pull-requests` navigates to `#/pull-request-detail?id=...` without `<base href>` 404 diversion; `Edit` uses icon+text and `Delete` uses an icon-only button with `#rapiderConfirmOkBtn` modal confirmation.
5. **AC-05 (Active Filter Tags & Clear All):** Filtering `pull-requests` or `security-advisories` by status, risk level, or severity renders `#activeFilterTagsBar` with HTML-escaped dismissible chips and `[Clear all]`.
6. **AC-06 (Drawer Lifecycle & `finally` Reset):** Creating/editing a PR or `DevBranchRule` in `pr-rule-drawer` resets button state inside `finally`, clears SWR caches, updates parent lists optimistically, and closes the drawer via `closePageDrawer()`.
7. **AC-07 (Pre-Receive Secret Push Blocking & Masking):** Pushing a commit containing a simulated AWS/API secret pattern is synchronously rejected (`422 Secret Push Blocked`), and the resulting `DevSecurityAdvisory` stores only a redacted `maskedFingerprint` with zero plaintext secret leakage.
8. **AC-08 (Segregation-of-Duties Self-Approval Ban):** When `DevBranchRule.blockAuthorSelfApproval === true`, an `APPROVE` call by `pullRequest.authorUserId` is deterministically rejected (`403 Self-Approval Prohibited`), and `GROUNDED_AI` comments never increment `approvedByUserIds`.
9. **AC-09 (Branch Rule Inheritance Guard):** Attempting to save a repository-level `DevBranchRule` with `minApprovals: 1` when an active `WORKSPACE_INHERITED` rule requires `minApprovals: 2` fails validation (`422 Cannot Weaken Inherited Ruleset`).
10. **AC-10 (Mandatory Status Check & Thread Resolution Gate):** Calling `dev.scm.enqueueOrMergePullRequest` while `checksStatus !== 'PASSED'`, `unresolvedThreadCount > 0`, or `linkedApmIssueIds` is empty (when `requireLinkedApmIssue: true`) fails deterministically (`409 Merge Preconditions Unmet`).
11. **AC-11 (Archived Repository Immutability):** Transitioning a `DevRepository` to `ARCHIVED` deterministically blocks new pushes, PR merges, and release mutations while keeping code browsing read-accessible.
12. **AC-12 (Published Release Tag Immutability):** Attempting to overwrite `targetCommitSha` on an already published `DevReleaseTag` is rejected (`409 Immutable Release Tag`).
13. **AC-13 (`APM` Cross-Suite Traceability):** Merging a PR referencing `APM-208` emits `dev.scm.pull_request.merged.v1` containing `ApmIssue` UUIDs without duplicating issue records inside `source-control`.
14. **AC-14 (Back Navigation State Restoration):** Navigating from `pull-requests` (filtered by `Risk Level: "HIGH"`) into `pull-request-detail` and clicking `#btnGoBack` restores `pull-requests` with `Risk Level: "HIGH"` intact.
15. **AC-15 (Theme & Contrast Compliance):** All routes pass light/dark `validate_theme` checks with `#ffffff !important` white text on primary/danger buttons and zero `window.alert`/`confirm`/`prompt` dialogs.
