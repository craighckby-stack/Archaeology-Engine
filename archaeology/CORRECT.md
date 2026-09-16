## Sun Sep 13 11:00:00 2026 +0000 -- fix: reorder middleware registration (`5e4e56be`)

**Pair ID:** 5e4e56be

**Author:** craighckby <craighckby@example.com>

**Files touched:**
- `server.ts`

**Commit message:**
```
fix: reorder middleware registration

Global logger broke static asset serving. Reordered static handler before logger interceptor.
```

**Diff:**
```diff
diff --git a/server.ts b/server.ts
index 2222222..3333333 100644
--- a/server.ts
+++ b/server.ts
@@ -2,5 +2,5 @@
 
 const app = express();
-app.use(globalLogger);
-app.use('/static', express.static('dist'));
+app.use('/static', express.static('dist'));
+app.use(globalLogger);
 app.listen(3000);
```

## Sun Sep 13 09:10:00 2026 +0000 -- fix: extract jwt validation into middleware (`7689035e`)

**Pair ID:** 7689035e

**Author:** craighckby <craighckby@example.com>

**Files touched:**
- `app.py`

**Commit message:**
```
fix: extract jwt validation into middleware

Stub middleware caused security bypass. Implemented real jwt.decode verification in decorator.
```

**Diff:**
```diff
diff --git a/app.py b/app.py
index 2222222..3333333 100644
--- a/app.py
+++ b/app.py
@@ -1,11 +1,19 @@
+import jwt
 from functools import wraps
 
 def require_auth(f):
     @wraps(f)
     def decorated(req, *args, **kwargs):
-        # TODO: implement real jwt verification
+        token = req.headers.get("Authorization", "")
+        if not token.startswith("Bearer "):
+            return {"error": "Unauthorized: Missing header"}, 401
+        try:
+            payload = jwt.decode(token.split(" ")[1], "secret", algorithms=["HS256"])
+            req.user = payload
+        except jwt.PyJWTError:
+            return {"error": "Unauthorized: Invalid signature"}, 401
         return f(req, *args, **kwargs)
     return decorated
 
 @require_auth
 def get_user_profile(req):
```

