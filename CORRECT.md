## Sun Sep 13 12:00:00 2026 +0000 -- add config loader (`b3a4c5d6`)

**Pair ID:** b3a4c5d6

**Author:** craighckby <craighckby@example.com>

**Files touched:**
- `config.ts`

**Commit message:**
```
add config loader

Environment variables parser with dotenv.
```

**Diff:**
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

## Sun Sep 13 11:30:00 2026 +0000 -- document auth flow (`f7e8d9c0`)

**Pair ID:** f7e8d9c0

**Author:** craighckby <craighckby@example.com>

**Files touched:**
- `docs/AUTH.md`

**Commit message:**
```
document auth flow

Added markdown guide for API consumers.
```

**Diff:**
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

## Sun Sep 13 11:00:00 2026 +0000 -- fix: reorder middleware registration (`5e4e56be`)

**Pair ID:** 5e4e56be

**Author:** craighckby <craighckby@example.com>

**Files touched:**
- `server.ts`

**Commit message:**
```
fix: reorder middleware registration

Global logger broke static asset serving. Reordered middleware before static handler.
```

**Diff:**
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

## Sun Sep 13 10:00:00 2026 +0000 -- add server scaffold (`a1b2c3d4`)

**Pair ID:** a1b2c3d4

**Author:** craighckby <craighckby@example.com>

**Files touched:**
- `server.ts`

**Commit message:**
```
add server scaffold

Express server setup with port binding.
```

**Diff:**
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

## Sun Sep 13 09:10:00 2026 +0000 -- fix: extract jwt validation into middleware (`7689035e`)

**Pair ID:** 7689035e

**Author:** craighckby <craighckby@example.com>

**Files touched:**
- `app.py`

**Commit message:**
```
fix: extract jwt validation into middleware

Inline check caused route duplication. Extracted into reusable middleware decorator.
```

**Diff:**
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

## Sun Sep 13 08:30:00 2026 +0000 -- add feature: auth middleware (`42f941a8`)

**Pair ID:** 42f941a8

**Author:** craighckby <craighckby@example.com>

**Files touched:**
- `app.py`

**Commit message:**
```
add feature: auth middleware

Initial implementation of JWT authorization header checking.
```

**Diff:**
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

