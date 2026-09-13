# Complete Commit Ledger

Total Commits Analyzed: 8

### [OK] b3a4c5d6 - add config loader
**Author:** craighckby <craighckby@example.com> | **Date:** Sun Sep 13 12:00:00 2026 +0000

```diff
diff --git a/config.ts b/config.ts
new file mode 100644
index 0000000..4444444
--- /dev/null
+++ b/config.ts
@@ -0,0 +1,2 @@
+import dotenv from 'dotenv';
+dotenv.config();

```

---

### [OK] f7e8d9c0 - document auth flow
**Author:** craighckby <craighckby@example.com> | **Date:** Sun Sep 13 11:30:00 2026 +0000

```diff
diff --git a/docs/AUTH.md b/docs/AUTH.md
new file mode 100644
index 0000000..9999999
--- /dev/null
+++ b/docs/AUTH.md
@@ -0,0 +1,3 @@
+# Auth Guide
+Bearer tokens required.


```

---

### [OK] 5e4e56be - fix: reorder middleware registration
**Author:** craighckby <craighckby@example.com> | **Date:** Sun Sep 13 11:00:00 2026 +0000

```diff
diff --git a/server.ts b/server.ts
index 2222222..3333333 100644
--- b/server.ts
+++ b/server.ts
@@ -3,3 +3,3 @@ const app.use(globalLogger);
+app.use(express.static('dist'));
+app.use(globalLogger);
 app.listen(3000);


```

---

### [WRONG] 234ab33d - attempt: try express middleware chain
**Author:** craighckby <craighckby@example.com> | **Date:** Sun Sep 13 10:30:00 2026 +0000
**Note:** Self-identified failure / WIP in commit subject ("attempt: try express middleware chain")

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

### [OK] a1b2c3d4 - add server scaffold
**Author:** craighckby <craighckby@example.com> | **Date:** Sun Sep 13 10:00:00 2026 +0000

```diff
diff --git a/server.ts b/server.ts
index 0000000..1111111 100644
--- /dev/null
+++ b/server.ts
@@ -0,0 +1,5 @@
+import express from 'express';
+const app = express();
+app.listen(3000);


```

---

### [OK] 7689035e - fix: extract jwt validation into middleware
**Author:** craighckby <craighckby@example.com> | **Date:** Sun Sep 13 09:10:00 2026 +0000

```diff
diff --git a/app.py b/app.py
index abcdef0..1234567 100644
--- a/app.py
+++ b/app.py
@@ -15,6 +15,4 @@ def handle_request(req):
-    if not req.headers.get("Authorization"):
-        raise Exception("Unauthorized")
+    @require_auth
+    def protected_route():
+        pass


```

---

### [WRONG] 34b1ec41 - wip: trying inline jwt validation
**Author:** craighckby <craighckby@example.com> | **Date:** Sun Sep 13 08:45:00 2026 +0000
**Note:** Self-identified failure / WIP in commit subject ("wip: trying inline jwt validation")

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

### [OK] 42f941a8 - add feature: auth middleware
**Author:** craighckby <craighckby@example.com> | **Date:** Sun Sep 13 08:30:00 2026 +0000

```diff
diff --git a/app.py b/app.py
index 1234567..89abcdef 100644
--- a/app.py
+++ b/app.py
@@ -10,3 +10,12 @@ def app():
+def verify_jwt(req):
+    token = req.headers.get("Authorization")
+    if not token:
+        return False
+    return True


```
