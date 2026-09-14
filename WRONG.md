## Sun Sep 13 10:30:00 2026 +0000 -- attempt: try express middleware chain (`234ab33d`)

**Pair ID:** unknown

**Author:** craighckby <craighckby@example.com>

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

## Sun Sep 13 08:45:00 2026 +0000 -- wip: trying inline jwt validation (`34b1ec41`)

**Pair ID:** unknown

**Author:** craighckby <craighckby@example.com>

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

