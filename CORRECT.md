## 2026-09-12T20:47:46Z -- eslint: fix bracket notation in language extensions (#334771) (`3879d0e8`)

**Pair ID:** 3879d0e8

**Author:** Dmitriy Vasyura <dmitriyvasyura@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
eslint: fix bracket notation in language extensions (#334771)

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

* eslint: fix bracket notation in language extensions

Replace identifier-safe bracket notation across language feature extensions and remove the completed group from the temporary allowlist.

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
+[eslint: fix bracket notation in language extensions 334771]
```

## 2026-09-12T20:33:23Z -- eslint: fix bracket notation in workbench services (#334774) (`b0240f60`)

**Pair ID:** b0240f60

**Author:** Dmitriy Vasyura <dmitriyvasyura@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
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
+[eslint: fix bracket notation in workbench services 334774]
```

## 2026-09-12T17:04:58Z -- Fix terminal-editor shift-drop on detached instances (#335945) (`9e9bd553`)

**Pair ID:** 9e9bd553

**Author:** Dmitriy Vasyura <dmitriyvasyura@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
Fix terminal-editor shift-drop on detached instances (#335945)

Dispose the drag-and-drop observer when a terminal detaches and prevent deferred initialization from targeting a stale container. Add regression coverage for both initialized and pending observers during terminal editor tab reuse.

Fixes #311164

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
Co-authored-by: Vlad Gerasimov vlad@vlad.studio
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
+[Fix terminal-editor shift-drop on detached instances 335945]
```

## 2026-09-12T00:50:29Z -- Fix permission bits in mock-policy-server file commands (#334382) (`298764bb`)

**Pair ID:** 298764bb

**Author:** joshspicer <joshspicer@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
Fix permission bits in mock-policy-server file commands (#334382)

* Agent Host changes for agents/mock-policy-server-permission-bits-fix

* Replace policy files atomically

Co-authored-by: joshspicer <23246594+joshspicer@users.noreply.github.com>

* Apply batched suggestions from code review

Co-authored-by: Copilot Autofix powered by AI <175728472+Copilot@users.noreply.github.com>

---------

Co-authored-by: copilot-swe-agent[bot] <198982749+Copilot@users.noreply.github.com>
Co-authored-by: Copilot Autofix powered by AI <175728472+Copilot@users.noreply.github.com>
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
+[Fix permission bits in mock-policy-server file commands 334382]
```

## 2026-09-11T18:37:08Z -- Revert "chat: make the Stop button red while a request is running" (#335831) (`7f86fa9a`)

**Pair ID:** 7f86fa9a

**Author:** Justin Chen <justinchen@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
Revert "chat: make the Stop button red while a request is running" (#335831)

Revert "chat: make the Stop button red while a request is running (#335579)"

This reverts commit ac4ca1181d249bb67fa7b77f236bf70dccbcb393.
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
+[Revert chat: make the Stop button red while a request is running 335831]
```

## 2026-09-11T16:48:24Z -- sessions: fix consecutive archive confetti animations (#335770) (`4c3757c8`)

**Pair ID:** 4c3757c8

**Author:** Megan Rogge <meganrogge@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
sessions: fix consecutive archive confetti animations (#335770)

* Fix consecutive click animations

Allow each click animation to own and clean up its overlay so rapid session archive actions are not silently dropped. Add regression coverage for consecutive confetti bursts.\n\nFixes #335283\n\nCo-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

* Resolve archive confetti setting on click

Keep archive action view items animation-ready and read the setting when clicked so virtualized rows do not retain stale behavior.\n\nCo-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
+[sessions: fix consecutive archive confetti animations 335770]
```

## 2026-09-11T14:45:17Z -- fix: centre the fork tooltip on the fork button (`03abc986`)

**Pair ID:** 03abc986

**Author:** Federico Brancasi <federicobrancasi@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
fix: centre the fork tooltip on the fork button

The tooltip's caret sat left of the fork button in the chat checkpoint
toolbar. Three independent causes, each verified by measurement in a
running build.

1. The decorative "·" separator was an in-flow ::before on the fork
action item, so the item's border box spanned dot + gap + button.
BaseActionViewItem anchors the managed hover to that element and
HoverWidget centres a pointer hover on its rect, putting the tooltip
3.8px left. The same box drives the click listeners, so the dot was
also a live target for Fork. Draw the separator in the item's left
gutter instead: absolutely positioned, reserved with margin-left, and
pointer-events: none. Net layout shift 0.44px.

2. HoverWidget positions the hover at target.center.x - clientWidth / 2
but placed the caret at Math.round(hoverWidth / 2), biasing it 0.5px
right whenever the hover's width is odd. Since the hover's own x is
fractional, the rounding could never align the caret to a device
pixel; it only broke the centring. Drop it on both axes.

3. hoverWidget.ts treats Constants.PointerSize as the caret's half
width, but the caret was a content-box square whose 1px borders
Chromium rounds down to whole device pixels under zoom, leaving a
5.86px box whose centre is 2.93px from its origin. Size it as a
border box so the box stays 6px and its centre stays exactly
PointerSize at any zoom.

Measured against each hover's real anchor element, every pointer tooltip
now centres within 0.011px, independent of width parity and zoom level.
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
+[fix: centre the fork tooltip on the fork button]
```

## 2026-09-11T14:20:59Z -- automations: fix: preserve dialog when dismissing suggestions (#335727) (`0f360d07`)

**Pair ID:** 0f360d07

**Author:** Ulugbek Abdullaev <ulugbekabdullaev@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
automations: fix: preserve dialog when dismissing suggestions (#335727)

Cancel active and pending prompt suggestions without closing the Automations dialog. Preserve popup priority, Escape press ownership, subsequent dismissal, and existing suggestion acceptance behavior.

Fixes #334869

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
+[automations: fix: preserve dialog when dismissing suggestions 335727]
```

## 2026-09-11T14:18:43Z -- Fixes #334429. (#335715) (`1fd3ffce`)

**Pair ID:** 1fd3ffce

**Author:** Henning Dieterichs <henningdieterichs@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
Fixes #334429. (#335715)

(better fix would be #335704 though)
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
+[Fixes 334429. 335715]
```

## 2026-09-11T14:03:57Z -- Fix auto model not being properly restored (#335706) (`e2eed440`)

**Pair ID:** e2eed440

**Author:** Logan Ramos <loganramos@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
Fix auto model not being properly restored (#335706)

* Fix auto model not being properly restored

* ADdress ccr comments
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
+[Fix auto model not being properly restored 335706]
```

## 2026-09-11T11:40:45Z -- Merge origin/main and resolve latest screenshot hash conflict (`4d5ad8f8`)

**Pair ID:** 4d5ad8f8

**Author:** copilot-swe-agent[bot] <copilotsweagentbot@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
Merge origin/main and resolve latest screenshot hash conflict

Co-authored-by: mrleemurray <25487940+mrleemurray@users.noreply.github.com>
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
+[Merge origin/main and resolve latest screenshot hash conflict]
```

## 2026-09-11T11:26:05Z -- Fix focus border for checked toggles (`80aef4d5`)

**Pair ID:** 80aef4d5

**Author:** mrleemurray <mrleemurray@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
Fix focus border for checked toggles

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

**Diff:**
```diff
diff --git a/vscode.ts b/vscode.ts
--- a/vscode.ts
+++ b/vscode.ts
@@ -0,0 +1,1 @@
+[Fix focus border for checked toggles]
```

## 2026-09-11T11:12:50Z -- sessions: Merge main and resolve test import conflict (`88f2307b`)

**Pair ID:** 88f2307b

**Author:** BeniBenj <benibenj@users.noreply.github.com>

**Files touched:**
- `vscode.ts`

**Commit message:**
```
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
+[sessions: Merge main and resolve test import conflict]
```

