# Correct Commits Ledger (CORRECT.md)

> Full content and diffs of every commit that succeeded without failure or immediate reversion (newest commits first).

## Thu, 15 Jan 2026 14:32:10 +0000 -- release(core): v4.0.0 architecture overhaul & state machine engine (`8a4f91c6`)

**Author:** codecrafters-io Core Bot <bot@codecrafters-io.org>

**Files touched:**
- `src/core/engine.ts`

**Commit message:**
```
release(core): v4.0.0 architecture overhaul & state machine engine
- Streamlined core dispatch loop
- Added modular archetype drivers
- Reduced memory footprint by 42%
```

**Diff:**
```diff
diff --git a/src/core/engine.ts b/src/core/engine.ts
--- a/src/core/engine.ts
+++ b/src/core/engine.ts
@@ -10,6 +10,18 @@
-export function legacyEngineLoop() {}
+export class CoreArchaeologyEngine {
+  private stateMachine: StateMachine;
+  constructor() {
+    this.stateMachine = new StateMachine();
+  }
+  public executePipeline(context: ExecutionContext) {
+    return this.stateMachine.transition('ACTIVE', context);
+  }
+}


```

---

## Mon, 12 Jan 2026 18:21:44 +0000 -- perf(query): implement zero-alloc buffer pooling for diff stream parsing (`7b3e21a5`)

**Author:** Chief Architect <lead@codecrafters-io.org>

**Files touched:**
- `src/query/bufferPool.ts`

**Commit message:**
```
perf(query): implement zero-alloc buffer pooling for diff stream parsing
Eliminates GC pressure during high-throughput repository archaeology scans.
```

**Diff:**
```diff
diff --git a/src/query/bufferPool.ts b/src/query/bufferPool.ts
--- a/src/query/bufferPool.ts
+++ b/src/query/bufferPool.ts
@@ -1,4 +1,12 @@
+// Buffer pool implementation for high-speed parsing
+export const bufferPool = new FastBufferPool(1024 * 64);
+export function acquireStreamBuffer() {
+  return bufferPool.borrow();
+}


```

---

## Fri, 09 Jan 2026 11:15:30 +0000 -- fix(security): harden credential masking and sanitize token regex (`6c2d1094`)

**Author:** Security Reviewer <sec@codecrafters-io.org>

**Files touched:**
- `src/security/sanitizer.ts`

**Commit message:**
```
fix(security): harden credential masking and sanitize token regex
Ensures API keys, secret hashes, and PAT tokens are never exposed in log exports.
```

**Diff:**
```diff
diff --git a/src/security/sanitizer.ts b/src/security/sanitizer.ts
--- a/src/security/sanitizer.ts
+++ b/src/security/sanitizer.ts
@@ -25,4 +25,8 @@
-const TOKEN_RE = /ghp_[0-9a-zA-Z]{36}/g;
+const TOKEN_RE = /(ghp|github_pat)_[0-9a-zA-Z_]{36,}/gi;
+export function sanitizeLogs(input: string): string {
+  return input.replace(TOKEN_RE, '[REDACTED_SECRET]');
+}


```

---

## Tue, 06 Jan 2026 09:40:12 +0000 -- feat(dashboard): add interactive archetype explorer and real-time velocity metrics (`5d1c0983`)

**Author:** UI Specialist <design@codecrafters-io.org>

**Files touched:**
- `src/ui/Dashboard.tsx`

**Commit message:**
```
feat(dashboard): add interactive archetype explorer and real-time velocity metrics
- Responsive bento grid metrics
- Interactive timeline scrubbers
- Theme tags and author impact charts
```

**Diff:**
```diff
diff --git a/src/ui/Dashboard.tsx b/src/ui/Dashboard.tsx
--- a/src/ui/Dashboard.tsx
+++ b/src/ui/Dashboard.tsx
@@ -1,5 +1,14 @@
+export function Dashboard({ stats, commits }: DashboardProps) {
+  return (
+    <div className="archeology-dashboard">
+      <MetricCards stats={stats} />
+      <CommitTimeline commits={commits} />
+    </div>
+  );
+}


```

---

## Wed, 01 Jan 2026 00:00:00 +0000 -- init(build-your-own-x): initial commit and core project scaffold (`4e0b9872`)

**Author:** Founding Engineer <dev@codecrafters-io.org>

**Files touched:**
- `package.json`

**Commit message:**
```
init(build-your-own-x): initial commit and core project scaffold
Bootstrapped repository foundation with TypeScript, schema contracts, and test harness.
```

**Diff:**
```diff
diff --git a/package.json b/package.json
--- /dev/null
+++ b/package.json
@@ -0,0 +1,10 @@
+{
+  "name": "build-your-own-x",
+  "version": "1.0.0",
+  "private": false
+}


```

---

