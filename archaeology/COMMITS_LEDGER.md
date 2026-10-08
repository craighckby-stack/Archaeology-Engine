# Complete Commit Ledger

Total Commits Analyzed: 8

### [NEUTRAL] b3a4c5d6 - add config loader
**Author:** craighckby <craighckby@example.com> | **Date:** Sun Sep 13 12:00:00 2026 +0000
**Note:** Net-new feature / scaffold addition without paired bug fix (excluded from training ledger)

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

### [DOCS] f7e8d9c0 - document auth flow
**Author:** craighckby <craighckby@example.com> | **Date:** Sun Sep 13 11:30:00 2026 +0000
**Note:** Documentation change without code diff (excluded from training ledger)

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

### [CORRECT] 5e4e56be - fix: reorder middleware registration
**Author:** craighckby <craighckby@example.com> | **Date:** Sun Sep 13 11:00:00 2026 +0000
**Note:** Fix commit resolving prior issue in: server.ts

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

---

### [WRONG] 234ab33d - bug: register global logger before static route handler, blocking asset serving
**Author:** craighckby <craighckby@example.com> | **Date:** Sun Sep 13 10:30:00 2026 +0000
**Note:** Introduced issue in server.ts, resolved by 5e4e56be ("fix: reorder middleware registration")

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

---

### [NEUTRAL] a1b2c3d4 - feat: add server scaffold
**Author:** craighckby <craighckby@example.com> | **Date:** Sun Sep 13 10:00:00 2026 +0000
**Note:** Net-new feature / scaffold addition without paired bug fix (excluded from training ledger)

```diff
diff --git a/server.ts b/server.ts
new file mode 100644
index 0000000..1111111
--- /dev/null
+++ b/server.ts
@@ -0,0 +1,5 @@
+import express from 'express';
+
+const app = express();
+app.listen(3000);
```

---

### [CORRECT] 7689035e - fix: extract jwt validation into middleware
**Author:** craighckby <craighckby@example.com> | **Date:** Sun Sep 13 09:10:00 2026 +0000
**Note:** Fix commit resolving prior issue in: app.py

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

---

### [WRONG] 42f941a8 - bug: add auth middleware stub
**Author:** craighckby <craighckby@example.com> | **Date:** Sun Sep 13 08:30:00 2026 +0000
**Note:** Introduced issue in app.py, resolved by 7689035e ("fix: extract jwt validation into middleware")

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

---

### [NEUTRAL] 31e8d7c6 - feat: initial user profile route
**Author:** craighckby <craighckby@example.com> | **Date:** Sun Sep 13 08:00:00 2026 +0000
**Note:** Net-new feature / scaffold addition without paired bug fix (excluded from training ledger)

```diff
diff --git a/app.py b/app.py
new file mode 100644
index 0000000..1111111
--- /dev/null
+++ b/app.py
@@ -0,0 +1,2 @@
+def get_user_profile(req):
+    return {"user": "profile_data"}
```

---

Total Commits Analyzed: 8

### [NEUTRAL] b3a4c5d6 - add config loader
**Author:** craighckby <craighckby@example.com> | **Date:** Sun Sep 13 12:00:00 2026 +0000
**Note:** Net-new feature / scaffold addition without paired bug fix (excluded from training ledger)

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

### [DOCS] f7e8d9c0 - document auth flow
**Author:** craighckby <craighckby@example.com> | **Date:** Sun Sep 13 11:30:00 2026 +0000
**Note:** Documentation change without code diff (excluded from training ledger)

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

### [CORRECT] 5e4e56be - fix: reorder middleware registration
**Author:** craighckby <craighckby@example.com> | **Date:** Sun Sep 13 11:00:00 2026 +0000
**Note:** Fix commit resolving prior issue in: server.ts

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

---

### [WRONG] 234ab33d - bug: register global logger before static route handler, blocking asset serving
**Author:** craighckby <craighckby@example.com> | **Date:** Sun Sep 13 10:30:00 2026 +0000
**Note:** Introduced issue in server.ts, resolved by 5e4e56be ("fix: reorder middleware registration")

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

---

### [NEUTRAL] a1b2c3d4 - feat: add server scaffold
**Author:** craighckby <craighckby@example.com> | **Date:** Sun Sep 13 10:00:00 2026 +0000
**Note:** Net-new feature / scaffold addition without paired bug fix (excluded from training ledger)

```diff
diff --git a/server.ts b/server.ts
new file mode 100644
index 0000000..1111111
--- /dev/null
+++ b/server.ts
@@ -0,0 +1,5 @@
+import express from 'express';
+
+const app = express();
+app.listen(3000);
```

---

### [CORRECT] 7689035e - fix: extract jwt validation into middleware
**Author:** craighckby <craighckby@example.com> | **Date:** Sun Sep 13 09:10:00 2026 +0000
**Note:** Fix commit resolving prior issue in: app.py

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

---

### [WRONG] 42f941a8 - bug: add auth middleware stub
**Author:** craighckby <craighckby@example.com> | **Date:** Sun Sep 13 08:30:00 2026 +0000
**Note:** Introduced issue in app.py, resolved by 7689035e ("fix: extract jwt validation into middleware")

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

---

### [NEUTRAL] 31e8d7c6 - feat: initial user profile route
**Author:** craighckby <craighckby@example.com> | **Date:** Sun Sep 13 08:00:00 2026 +0000
**Note:** Net-new feature / scaffold addition without paired bug fix (excluded from training ledger)

```diff
diff --git a/app.py b/app.py
new file mode 100644
index 0000000..1111111
--- /dev/null
+++ b/app.py
@@ -0,0 +1,2 @@
+def get_user_profile(req):
+    return {"user": "profile_data"}
```
