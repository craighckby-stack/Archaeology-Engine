# Failed Commits Ledger (WRONG.md)

> Record of every commit that failed, was reverted, or required immediate patching, paired with its recovery link (newest commits first).

## Sun Sep 13 10:30:00 2026 +0000 -- attempt: try express middleware chain (`234ab33d`)

**Reason:** Self-identified failure / WIP in commit subject ("attempt: try express middleware chain")

**Files touched:**
- `server.ts`

**Commit message:**
```
attempt: try express middleware chain
Trying global app.use without path filtering.
```

**Diff:**
```diff
diff --git b/server.ts a/server.ts
index 1111111..2222222 100644
--- a/server.ts
+++ b/server.ts
@@ -3,2 +3,3 @@ const app = express();
+app.use(globalLogger);
 app.listen(3000);


```

---

## Sun Sep 13 08:45:00 2026 +0000 -- wip: trying inline jwt validation (`34b1ec41`)

**Reason:** Self-identified failure / WIP in commit subject ("wip: trying inline jwt validation")

**Files touched:**
- `app.py`

**Commit message:**
```
wip: trying inline jwt validation
Testing inline token checks inside route handlers directly.
```

**Diff:**
```diff
diff --git a/app.py b/app.py
index 89abcdef..abcdef0 100644
--- a/app.py
+++ b/app.py
@@ -15,4 +15,6 @@ def handle_request(req):
+    # inline check
+    if not req.headers.get("Authorization"):
+        raise Exception("Unauthorized")


```

---

---

<!-- CAE Append Session: 2026-09-13T12:17:02.637Z -->

## 2025-02-05T08:53:53Z -- Merge pull request #432 from luislh-dev/main (`1d7d4404`)

**Reason:** Immediately followed by fix commit 5ee97a83 ("fix comment") touching overlapping files (DeepSeek-V3.ts)
**Fixed by:** `5ee97a83` (see CORRECT.md for recovery commit)

**Files touched:**
- `DeepSeek-V3.ts`

**Commit message:**
```
Merge pull request #432 from luislh-dev/main
remove redundant asterisks in README
```

**Diff:**
```diff
diff --git a/DeepSeek-V3.ts b/DeepSeek-V3.ts
--- a/DeepSeek-V3.ts
+++ b/DeepSeek-V3.ts
@@ -1,1 +1,3 @@
+[Merge pull request 432 from luislh-dev/main]


```

---



## 2025-02-03T20:02:04Z -- docs: remove redundant asterisks in note (`97b35f1f`)

**Reason:** Immediately followed by fix commit 6a30b432 ("Fix Linear Layer Bias Initialization") touching overlapping files (DeepSeek-V3.ts)
**Fixed by:** `6a30b432` (see CORRECT.md for recovery commit)

**Files touched:**
- `DeepSeek-V3.ts`

**Commit message:**
```
docs: remove redundant asterisks in note

```

**Diff:**
```diff
diff --git a/DeepSeek-V3.ts b/DeepSeek-V3.ts
--- a/DeepSeek-V3.ts
+++ b/DeepSeek-V3.ts
@@ -1,1 +1,3 @@
+[docs: remove redundant asterisks in note]


```

---



## 2025-01-28T12:16:54Z -- clarify assertion error (`2756e130`)

**Reason:** Immediately followed by fix commit 6784e197 ("Fix TOC links to correctly link to headings in Markdown") touching overlapping files (DeepSeek-V3.ts)
**Fixed by:** `6784e197` (see CORRECT.md for recovery commit)

**Files touched:**
- `DeepSeek-V3.ts`

**Commit message:**
```
clarify assertion error

```

**Diff:**
```diff
diff --git a/DeepSeek-V3.ts b/DeepSeek-V3.ts
--- a/DeepSeek-V3.ts
+++ b/DeepSeek-V3.ts
@@ -1,1 +1,3 @@
+[clarify assertion error]


```

---



## 2025-01-07T06:05:15Z -- Merge pull request #230 from jacksonpradolima/main (`25109d2c`)

**Reason:** Immediately followed by fix commit ee4c4ea3 ("Merge pull request #234 from wangfuchun-fc/patch-1") touching overlapping files (DeepSeek-V3.ts)
**Fixed by:** `ee4c4ea3` (see CORRECT.md for recovery commit)

**Files touched:**
- `DeepSeek-V3.ts`

**Commit message:**
```
Merge pull request #230 from jacksonpradolima/main
Add CITATION.cff to provide citation metadata
```

**Diff:**
```diff
diff --git a/DeepSeek-V3.ts b/DeepSeek-V3.ts
--- a/DeepSeek-V3.ts
+++ b/DeepSeek-V3.ts
@@ -1,1 +1,3 @@
+[Merge pull request 230 from jacksonpradolima/main]


```

---



## 2025-01-06T00:46:37Z -- Add CITATION.cff to provide citation metadata (`c0705492`)

**Reason:** Immediately followed by fix commit 3779a897 ("fix: fix readme doc typo.") touching overlapping files (DeepSeek-V3.ts)
**Fixed by:** `3779a897` (see CORRECT.md for recovery commit)

**Files touched:**
- `DeepSeek-V3.ts`

**Commit message:**
```
Add CITATION.cff to provide citation metadata
This file includes detailed citation information for the DeepSeek-V3 project, such as authors, DOI, license, and key project details. It enables users to properly cite the work and promotes better academic and professional attribution.
```

**Diff:**
```diff
diff --git a/DeepSeek-V3.ts b/DeepSeek-V3.ts
--- a/DeepSeek-V3.ts
+++ b/DeepSeek-V3.ts
@@ -1,1 +1,3 @@
+[Add CITATION.cff to provide citation metadata]


```

---



## 2025-01-02T14:02:52Z -- use alert formatting for notes in readme (`21bc231f`)

**Reason:** Immediately followed by fix commit 0d16ea24 ("Merge pull request #206 from kutt/patch-1") touching overlapping files (DeepSeek-V3.ts)
**Fixed by:** `0d16ea24` (see CORRECT.md for recovery commit)

**Files touched:**
- `DeepSeek-V3.ts`

**Commit message:**
```
use alert formatting for notes in readme

```

**Diff:**
```diff
diff --git a/DeepSeek-V3.ts b/DeepSeek-V3.ts
--- a/DeepSeek-V3.ts
+++ b/DeepSeek-V3.ts
@@ -1,1 +1,3 @@
+[use alert formatting for notes in readme]


```

---



## 2024-12-30T06:37:38Z -- Merge pull request #33 from zhyncs/main (`94410f8d`)

**Reason:** Immediately followed by fix commit 1b8e18cc ("Merge pull request #21 from eltociear/patch-1") touching overlapping files (DeepSeek-V3.ts)
**Fixed by:** `1b8e18cc` (see CORRECT.md for recovery commit)

**Files touched:**
- `DeepSeek-V3.ts`

**Commit message:**
```
Merge pull request #33 from zhyncs/main
docs: update SGLang usage
```

**Diff:**
```diff
diff --git a/DeepSeek-V3.ts b/DeepSeek-V3.ts
--- a/DeepSeek-V3.ts
+++ b/DeepSeek-V3.ts
@@ -1,1 +1,3 @@
+[Merge pull request 33 from zhyncs/main]


```

---
