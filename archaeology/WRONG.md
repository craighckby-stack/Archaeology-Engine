# Archaeological Anti-Patterns (WRONG.md)

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

## Sun Sep 13 08:30:00 2026 +0000 -- bug: add auth middleware stub (`42f941a8`)

**Pair ID:** 7689035e

**Author:** craighckby <craighckby@example.com>

**Files touched:**
- `app.py`

**Commit message:**
```
bug: add auth middleware stub

Added placeholder verify_jwt decorator that returns True unconditionally.
```

**Diff:**
```diff
diff --git a/app.py b/app.py
index 1111111..2222222 100644
--- a/app.py
+++ b/app.py
@@ -1,2 +1,11 @@
+from functools import wraps
+
+def require_auth(f):
+    @wraps(f)
+    def decorated(req, *args, **kwargs):
+        # TODO: implement real jwt verification
+        return f(req, *args, **kwargs)
+    return decorated
+
+@require_auth
 def get_user_profile(req):
     return {"user": "profile_data"}
```

