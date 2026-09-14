## 2026-09-13T18:06:20Z -- Retry failed sanity extension installs (#335995) (`1e8e852c`)

**Pair ID:** 7fe7e982

**Author:** Dmitriy Vasyura <dmitriyvasyura@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
Retry failed sanity extension installs (#335995)

test: retry failed sanity extension installs

Scope extension installation to the intended result and retry as soon as the install action returns or a Marketplace error appears.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
+[Retry failed sanity extension installs 335995]
```

## 2026-09-12T17:05:57Z -- workbench: avoid synchronous activity bar size measurement (#335537) (`3addbda6`)

**Pair ID:** 3879d0e8

**Author:** Dmitriy Vasyura <dmitriyvasyura@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
workbench: avoid synchronous activity bar size measurement (#335537)

Compute global activity height from rendered actions and the existing CSS sizing inputs instead of reading clientHeight during layout. Recalculate overflow when Accounts visibility changes and cover compact, Modern UI, and horizontal layouts.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
+[workbench: avoid synchronous activity bar size measurement 335537]
```

## 2026-09-12T13:57:50Z -- update json/html services (#335483) (`041d1b66`)

**Pair ID:** 9e9bd553

**Author:** Martin Aeschlimann <martinaeschlimann@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
update json/html services (#335483)

* update json/html services

* fix: update TextDocument import from 'vscode-languageserver' to 'vscode-json-languageservice'

---------

Co-authored-by: Dmitriy Vasyura <dmitriv@microsoft.com>
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
+[update json/html services 335483]
```

## 2026-09-12T00:39:27Z -- sessions: add rich GitHub reference previews (#335583) (`4d3755c0`)

**Pair ID:** 298764bb

**Author:** Cherry Wang <cherrywang@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
sessions: add rich GitHub reference previews (#335583)

* sessions: add rich GitHub reference hovers

Reuse the existing issue and pull request hover components for dropdown details, with compact metadata, explicit spacing ownership, and bounded readable descriptions. Integrate the pill, row action, detail link, and branch-copy controls into one accessible keyboard flow.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

* sessions: refine GitHub reference hover metadata

Use existing relative-time, title, spacing, and popup patterns consistently across standalone and collection references. Bound long titles, keep transient content concise, and make HTML hover padding ownership explicit.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

* sessions: tighten GitHub hover title spacing

Use the existing 8px SessionSummaryHover rhythm between the title and description in both standalone and collection cards.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

* sessions: balance GitHub hover row spacing

Let the card root own a uniform 8px gap between metadata, title, description, and branch rows instead of splitting relationship spacing across child padding.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

* sessions: align GitHub hover content hierarchy

Follow GitHub hover cards for provenance, title and ID, state, context, routing, and author order while preserving VS Code styling and the bounded transient content budget.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

* sessions: keep GitHub reference IDs with titles

Group the final visible title word with the linked reference ID, bound pathological titles safely, and allow narrow cards to wrap without clipping focusable content.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

* sessions: address GitHub hover review feedback

Use measured submenu height for positioning and reveal the full bounded GitHub title when keyboard focus reaches its linked reference.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

* hover: align footer action icons and labels

Render shared hover actions as centered inline flex rows so codicons and text use the same vertical center.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

* sessions: align GitHub reference metadata

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

* sessions: hide decorative PR branch arrow

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

* sessions: address remaining Copilot review feedback

- actionList: Shift+Tab from a hover panel now returns to the last
non-removal toolbar action instead of a trailing Remove control,
covered by a new multi-action regression test.
- pullRequestHover: stop branch-pill clicks from bubbling to the
ActionList row so copying a branch no longer clears list focus.
- chatView: extract the Shift+Tab-to-pills predicate into a testable
helper and add regression tests for modifier filtering and
preventDefault/stopPropagation cancellation; align the accessibility
help text with the Shift+Tab shortcut.
- githubPRFetcher: cover the closed_at -> closedAt mapping with a
dedicated fixture and assertion.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

* sessions: show CI status in pull request hovers

Surface the shared overall checks state alongside PR lifecycle metadata, including draft pull requests, while keeping no-check and completed PR cards quiet. Preserve the same status in single-pill and collection-row accessibility descriptions.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

* sessions: align pending checks with GitHub

Use the familiar static pending dot and aggregate 'Checks pending' label rather than a sync metaphor that implies active execution or refresh.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

* sessions: fix hover reverse navigation

Traverse rich-hover controls and row actions symmetrically with Shift+Tab, and preserve GitHub's distinct Duplicate issue terminology.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

---------

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
+[sessions: add rich GitHub reference previews 335583]
```

## 2026-09-11T21:01:30Z -- Agent Host: Improve Dev Container failure diagnostics (#335766) (`8bf89deb`)

**Pair ID:** 49247edb

**Author:** Christof Marti <christofmarti@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
Agent Host: Improve Dev Container failure diagnostics (#335766)

* Agent Host: Improve Dev Container failure diagnostics

Run Dev Container CLI commands with debug logging and reveal the workspace-specific output channel when setup fails.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

* Agent Host: Address Dev Container diagnostics review

Apply debug logging to relay startup and recognize serialized cancellation errors before revealing setup output.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

---------

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
+[Agent Host: Improve Dev Container failure diagnostics 335766]
```

## 2026-09-11T20:53:36Z -- notebook: revert declarative built-in type registration (#335880) (`a871cb8d`)

**Pair ID:** 1d79f867

**Author:** Dmitriy Vasyura <dmitriyvasyura@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
notebook: revert declarative built-in type registration (#335880)

Reverts the changes from #331911.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
+[notebook: revert declarative built-in type registration 335880]
```

## 2026-09-11T18:32:41Z -- base: make confetti more fun (#335807) (`db6ec195`)

**Pair ID:** 7f86fa9a

**Author:** Megan Rogge <meganrogge@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
base: make confetti more fun (#335807)
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
+[base: make confetti more fun 335807]
```

## 2026-09-11T16:25:42Z -- Update comment metadata fixture baselines (`a74d3a14`)

**Pair ID:** 4c3757c8

**Author:** mrleemurray <mrleemurray@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
Update comment metadata fixture baselines

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
+[Update comment metadata fixture baselines]
```

## 2026-09-11T14:39:02Z -- Refactor multi-diff editor variants (`f751ef9c`)

**Pair ID:** 03abc986

**Author:** Henning Dieterichs <henningdieterichs@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
Refactor multi-diff editor variants

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
+[Refactor multi-diff editor variants]
```

## 2026-09-11T14:11:03Z -- update codenotify (#335730) (`b9ec98e3`)

**Pair ID:** 0f360d07

**Author:** Ulugbek Abdullaev <ulugbekabdullaev@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
update codenotify (#335730)
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
+[update codenotify 335730]
```

## 2026-09-11T14:02:37Z -- Add multi-diff editor visual fixture matrix (`924b262f`)

**Pair ID:** e2eed440

**Author:** Henning Dieterichs <henningdieterichs@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
Add multi-diff editor visual fixture matrix

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
+[Add multi-diff editor visual fixture matrix]
```

## 2026-09-11T11:36:27Z -- Differentiate terminal bright colors in light themes (`3623f170`)

**Pair ID:** 4d5ad8f8

**Author:** mrleemurray <mrleemurray@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
Differentiate terminal bright colors in light themes

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
+[Differentiate terminal bright colors in light themes]
```

## 2026-09-11T11:02:46Z -- Allow leaving PR comments in agents (#335648) (`0dbebd2c`)

**Pair ID:** 80aef4d5

**Author:** Alex Ross <alexross@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
Allow leaving PR comments in agents (#335648)

* Allow leaving PR comments in agents

* Wording

* CCR and CI fixes
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
+[Allow leaving PR comments in agents 335648]
```

## 2026-09-12T20:33:23Z -- [Pre-image] Prior state for b0240f60 (`b0240f60-pre`)

**Pair ID:** b0240f60

**Author:** Dmitriy Vasyura <dmitriyvasyura@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
Pre-image reconstructed from fix commit b0240f60 (no prior introducing commit in log history):
eslint: fix bracket notation in workbench services (#334774)
* eslint: enable no bracket notation rule

Enable code-no-bracket-notation-for-identifiers for JavaScript and TypeScript files while grandfathering the 509 files with existing violations in a CODEOWNERS-gated allowlist.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

* eslint: update bracket allowlist owners

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

* eslint: group bracket notation exclusions

Organize the existing baseline by feature area so cleanup can be tracked and assigned without changing the excluded file set.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

* eslint: test no bracket notation rule

Add RuleTester coverage for valid accesses, diagnostics, and autofix edge cases. Preserve escaped string-literal property names by checking their raw source before reporting.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

* eslint: fix bracket notation in workbench services

Replace identifier-safe bracket access in workbench UI and service files, remove the cleaned group from the rule allowlist, and expose protected seams for strongly typed default-account tests.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

---------

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
```

## 2026-09-11T14:18:43Z -- [Pre-image] Prior state for 1fd3ffce (`1fd3ffce-pre`)

**Pair ID:** 1fd3ffce

**Author:** Henning Dieterichs <henningdieterichs@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
Pre-image reconstructed from fix commit 1fd3ffce (no prior introducing commit in log history):
Fixes #334429. (#335715)
(better fix would be #335704 though)
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
```

## 2026-09-11T11:12:50Z -- [Pre-image] Prior state for 88f2307b (`88f2307b-pre`)

**Pair ID:** 88f2307b

**Author:** BeniBenj <benibenj@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
Pre-image reconstructed from fix commit 88f2307b (no prior introducing commit in log history):
sessions: Merge main and resolve test import conflict
Preserve upstream empty-group coverage alongside the collapsed-section status tests and screenshot baselines.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
```

