## Sun Sep 13 10:30:00 2026 +0000 -- bug: register global logger before static route handler, blocking asset serving (`234ab33d`)

**Pair ID:** 5e4e56be

**Author:** craighckby <craighckby@example.com>

**Files touched:**
- `server.ts`

**Commit message:**
```
bug: register global logger before static route handler, blocking asset serving

Registered global request logger ahead of static file serving, intercepting and breaking static asset delivery.
```

**Diff:**
```diff
diff --git a/server.ts b/server.ts
index 1111111..2222222 100644
--- a/server.ts
+++ b/server.ts
@@ -1,4 +1,6 @@
 import express from 'express';
 const app = express();
+app.use(globalLogger);
+app.use('/static', express.static('dist'));
 app.listen(3000);
```

## Sun Sep 13 08:30:00 2026 +0000 -- add feature: auth middleware (`42f941a8`)

**Pair ID:** 7689035e

**Author:** craighckby <craighckby@example.com>

**Files touched:**
- `app.py`

**Commit message:**
```
add feature: auth middleware

Initial inline JWT authorization header checking inside route handlers.
```

**Diff:**
```diff
diff --git a/app.py b/app.py
index 1234567..89abcdef 100644
--- a/app.py
+++ b/app.py
@@ -10,7 +10,14 @@ def get_user_profile(req):
+    # inline token check
+    token = req.headers.get("Authorization", "")
+    if not token.startswith("Bearer "):
+        return {"error": "Unauthorized"}, 401
+    raw_token = token.split(" ")[1] if " " in token else ""
+    if not raw_token:
+        return {"error": "Invalid token"}, 401
     return {"user": "profile_data"}
```

