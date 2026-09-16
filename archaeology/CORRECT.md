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

---

<!-- CAE Append Session: 2026-09-16T12:52:32.183Z -->

## Sun, 13 Sep 2026 22:15:41 +1000 -- Introduced UI state for managing commit selection and limits. Added server-side logic to handle appending and merging log content for structured files like CORRECT.md and WRONG.md. (`ee62a33d`)

**Pair ID:** ee62a33d

**Author:** Unknown

**Files touched:**
- `server.ts`
- `src/App.tsx`

**Commit message:**
```
Introduced UI state for managing commit selection and limits. Added server-side logic to handle appending and merging log content for structured files like CORRECT.md and WRONG.md.
```

**Diff:**
```diff
---
 server.ts   |  107 +++++-
 src/App.tsx | 1002 +++++++++++++++++++++++++++++++++++++++++++--------
 2 files changed, 959 insertions(+), 150 deletions(-)

diff --git a/server.ts b/server.ts
index c0fb9cf..c7b339a 100644
--- a/server.ts
+++ b/server.ts
@@ -938,8 +938,8 @@ app.post("/api/github/history", async (req, res) => {
     // Clean repoFullName in case a full GitHub URL was passed
     repoFullName = repoFullName.trim().replace(/^https?:\/\/github\.com\//i, '').replace(/\.git$/i, '').replace(/^\/+|\/+$/g, '');
 
-    const shouldFetchAll = fetchAll === true || limit === 'all' || Number(limit) >= 100 || !limit;
-    const maxCommits = shouldFetchAll ? 200 : Math.max(1, Number(limit) || 50);
+    const shouldFetchAll = fetchAll === true || limit === 'all' || !limit;
+    const maxCommits = shouldFetchAll ? 1000 : Math.max(1, Math.min(2000, Number(limit) || 200));
 
     let parsedCommits: Array<{ sha: string, author: string, date: string, message: string }> = [];
 
@@ -1107,9 +1107,87 @@ app.post("/api/github/history", async (req, res) => {
   }
 });
 
+function mergeAndAppendContent(existing: string, incoming: string, filename: string): string {
+  if (!existing || !existing.trim()) return incoming;
+  if (!incoming || !incoming.trim()) return existing;
+
+  const baseName = filename.split('/').pop() || filename;
+
+  if (baseName === 'CORRECT.md' || baseName === 'WRONG.md') {
+    let cleanedIncoming = incoming.trim();
+    // Strip duplicate top-level title header if existing already has one
+    const titleMatch = cleanedIncoming.match(/^#\s+[^\n]+\n+(?:>[^\n]+\n+)?/);
+    if (titleMatch && existing.includes('# ')) {
+      cleanedIncoming = cleanedIncoming.substring(titleMatch[0].length).trim();
+    }
+    
+    // Extract incoming entries by header and filter out entries whose hashes already exist in the file
+    const incomingEntries = cleanedIncoming.split(/(?=^##\s+)/gm).filter(b => b.trim());
+    const newEntries = incomingEntries.filter(entry => {
+      const hashMatch = entry.match(/\(`?([0-9a-fA-F]{6,40})`?\)/);
+      if (hashMatch) {
+        return !existing.includes(hashMatch[1]);
+      }
+      return true;
+    });
+
+    if (newEntries.length === 0) {
+      return existing; // All entries already present in the file
+    }
+
+    const appendChunk = newEntries.join('\n\n');
+    return `${existing.trimEnd()}\n\n---\n\n<!-- CAE Append Session: ${new Date().toISOString()} -->\n\n${appendChunk}\n`;
+  }
+
+  if (baseName === 'stuff.md') {
+    let cleanedIncoming = incoming.trim();
+    const titleMatch = cleanedIncoming.match(/^#\s+[^\n]+\n+(?:>[^\n]+\n+)?/);
+    if (titleMatch && existing.includes('# ')) {
+      cleanedIncoming = cleanedIncoming.substring(titleMatch[0].length).trim();
+    }
+    return `${existing.trimEnd()}\n\n---\n\n## Appended Analysis Stream (${new Date().toISOString().replace('T', ' ').substring(0, 19)})\n\n${cleanedIncoming}\n`;
+  }
+
+  if (baseName === 'COMMITS_LEDGER.md') {
+    let cleanedIncoming = incoming.trim();
+    const titleMatch = cleanedIncoming.match(/^#\s+Complete Commit Ledger\n+/);
+    if (titleMatch && existing.includes('# Complete Commit Ledger')) {
+      cleanedIncoming = cleanedIncoming.substring(titleMatch[0].length).trim();
+    }
+    return `${existing.trimEnd()}\n\n---\n\n${cleanedIncoming}\n`;
+  }
+
+  if (baseName.endsWith('.json')) {
+    try {
+      const existingObj = JSON.parse(existing);
+      const incomingObj = JSON.parse(incoming);
+      if (Array.isArray(existingObj) && Array.isArray(incomingObj)) {
+        return JSON.stringify([...existingObj, ...incomingObj], null, 2);
+      }
+      return JSON.stringify({
+        ...existingObj,
+        ...incomingObj,
+        runs: [
+          ...(Array.isArray(existingObj.runs) ? existingObj.runs : [existingObj]),
+          incomingObj
+        ],
+        lastAppendedAt: new Date().toISOString()
+      }, null, 2);
+    } catch {
+      return `${existing.trimEnd()}\n\n${incoming}\n`;
+    }
+  }
+
+  if (baseName.endsWith('.txt') || baseName === 'RAW_GIT_LOG.txt') {
+    return `${existing.trimEnd()}\n\n${incoming}\n`;
+  }
+
+  return `${existing.trimEnd()}\n\n---\n\n${incoming}\n`;
+}
+
 app.post("/api/github/push", async (req, res) => {
   try {
-    const { token, repoName, isPrivate, targetFolder, files } = req.body;
+    const { token, repoName, isPrivate, targetFolder, files, appendOnly = true } = req.body;
     if (!token) return res.status(400).json({ error: "No token provided" });
 
     const headers = {
@@ -1170,20 +1248,33 @@ app.post("/api/github/push", async (req, res) => {
       const filePath = targetFolder ? `${targetFolder}/${f.path}` : f.path;
       const fileUrl = `https://api.github.com/repos/${owner}/${repoName}/contents/${filePath}`;
       
-      // Check for existing file SHA to allow overwriting
-      let sha = undefined;
+      // Check for existing file SHA & content to allow appending instead of overwriting
+      let sha: string | undefined = undefined;
+      let existingContent = "";
       const checkRes = await fetch(fileUrl, { headers });
       if (checkRes.ok) {
         const fileData = await checkRes.json();
         sha = fileData.sha;
+        if (fileData.content) {
+          try {
+            existingContent = Buffer.from(fileData.content, fileData.encoding || 'base64').toString('utf-8');
+          } catch (e) {
+            existingContent = "";
+          }
+        }
       }
 
-      const contentBase64 = Buffer.from(f.content || '').toString('base64');
+      // Add to file (append) if file exists and appendOnly mode is active
+      const finalContent = (existingContent && appendOnly)
+        ? mergeAndAppendContent(existingContent, f.content || '', f.path)
+        : (f.content || '');
+
+      const contentBase64 = Buffer.from(finalContent).toString('base64');
       const uploadRes = await fetch(fileUrl, {
         method: "PUT",
         headers,
         body: JSON.stringify({
-          message: `CAE: Update ${f.path}`,
+          message: sha ? `CAE: Append to ${f.path}` : `CAE: Create ${f.path}`,
           content: contentBase64,
           sha
         })
@@ -1192,7 +1283,7 @@ app.post("/api/github/push", async (req, res) => {
       if (uploadRes.ok) {
         results.push({
           path: f.path,
-          status: sha ? "updated" : "created",
+          status: sha ? "appended" : "created",
           url: `${repoUrl}/blob/main/${filePath}`
         });
       } else {
diff --git a/src/App.tsx b/src/App.tsx
index a89b5d2..70d0d26 100644
--- a/src/App.tsx
+++ b/src/App.tsx
@@ -1,11 +1,11 @@
-import React, { useState, useEffect } from 'react';
+import React, { useState, useEffect, useMemo } from 'react';
 import JSZip from 'jszip';
 import { 
   GitCommit, CheckCircle2, XCircle, Brain, FileCode, Search, 
   Download, RefreshCw, Layers, Terminal, Sparkles, Copy, Check, Plus, Tag, X,
   Github, UploadCloud, Lock, Globe, ExternalLink, ShieldCheck, AlertCircle, FolderGit2,
   Settings, Key, Cpu, User, Save, Eye, EyeOff, FolderDown, Archive, Star,
-  Timer, Hourglass, Gauge, Zap
+  Timer, Hourglass, Gauge, Zap, CheckSquare, Square, ListFilter, Sliders, CheckCheck
 } from 'lucide-react';
 import { BarChart, Bar, XAxis, YAxis, Tooltip, ResponsiveContainer } from 'recharts';
 import { CommitRecord, CommitStats } from './types';
@@ -29,6 +29,19 @@ export default function App() {
   const [selectedThemeFilter, setSelectedThemeFilter] = useState<string | null>(null);
   const [copiedTab, setCopiedTab] = useState<string | null>(null);
   const [showPasteModal, setShowPasteModal] = useState<boolean>(false);
+  const [appendLogToCurrent, setAppendLogToCurrent] = useState<boolean>(false);
+
+  // Commit Selection & Limit State
+  const [selectedCommitHashes, setSelectedCommitHashes] = useState<string[]>([]);
+  const [commitSelectLimit, setCommitSelectLimit] = useState<number | 'all'>('all');
+
+  // Architectural Hotspot limit
+  const [hotspotsLimit, setHotspotsLimit] = useState<number | 'all'>(6);
+
+  // Append entry to markdown file state
+  const [showAppendModal, setShowAppendModal] = useState<boolean>(false);
+  const [appendEntryFile, setAppendEntryFile] = useState<'correct' | 'wrong' | 'stuff'>('correct');
+  const [appendEntryText, setAppendEntryText] = useState<string>('');
 
   // Settings Panel State
   const [isSettingsPanelOpen, setIsSettingsPanelOpen] = useState<boolean>(false);
@@ -69,12 +82,34 @@ export default function App() {
   const [loadingRepos, setLoadingRepos] = useState<boolean>(false);
   const [repoLoadError, setRepoLoadError] = useState<string | null>(null);
   const [selectedRepoFullNames, setSelectedRepoFullNames] = useState<string[]>([]);
+  const [repoSelectLimit, setRepoSelectLimit] = useState<number | 'all'>('all');
+  const [batchAnalyzing, setBatchAnalyzing] = useState<boolean>(false);
+  const [batchProgress, setBatchProgress] = useState<{ current: number; total: number; repoName: string } | null>(null);
 
   const [repoName, setRepoName] = useState<string>('Archaeology-Engine');
   const [isPrivateRepo, setIsPrivateRepo] = useState<boolean>(false);
   const [targetFolder, setTargetFolder] = useState<string>('archaeology');
   const [pushScope, setPushScope] = useState<'deliverables' | 'full_app'>('deliverables');
 
+  // Push Files Selection State with granular variable limits
+  const [selectedPushFiles, setSelectedPushFiles] = useState<{
+    correct: boolean;
+    wrong: boolean;
+    stuff: boolean;
+    ledger: boolean;
+    rawLog: boolean;
+    summary: boolean;
+    readme: boolean;
+  }>({
+    correct: true,
+    wrong: true,
+    stuff: true,
+    ledger: true,
+    rawLog: true,
+    summary: true,
+    readme: true,
+  });
+
   const [pushingToGitHub, setPushingToGitHub] = useState<boolean>(false);
   const [pushSuccess, setPushSuccess] = useState<{
     repoUrl: string;
@@ -179,6 +214,60 @@ export default function App() {
     }
   };
 
+  const matchesRepoSearch = (repo: any, filterText: string) => {
+    if (!filterText || !filterText.trim()) return true;
+    const q = filterText.toLowerCase().trim();
+    return (
+      (repo.name && repo.name.toLowerCase().includes(q)) ||
+      (repo.full_name && repo.full_name.toLowerCase().includes(q)) ||
+      (repo.description && repo.description.toLowerCase().includes(q))
+    );
+  };
+
+  const handleSelectAllPushFiles = (mode: 'all' | 'core_md' | 'logs' | 'none') => {
+    if (mode === 'all') {
+      setSelectedPushFiles({
+        correct: true,
+        wrong: true,
+        stuff: true,
+        ledger: true,
+        rawLog: true,
+        summary: true,
+        readme: true,
+      });
+    } else if (mode === 'core_md') {
+      setSelectedPushFiles({
+        correct: true,
+        wrong: true,
+        stuff: true,
+        ledger: false,
+        rawLog: false,
+        summary: false,
+        readme: true,
+      });
+    } else if (mode === 'logs') {
+      setSelectedPushFiles({
+        correct: false,
+        wrong: false,
+        stuff: false,
+        ledger: true,
+        rawLog: true,
+        summary: true,
+        readme: false,
+      });
+    } else {
+      setSelectedPushFiles({
+        correct: false,
+        wrong: false,
+        stuff: false,
+        ledger: false,
+        rawLog: false,
+        summary: false,
+        readme: false,
+      });
+    }
+  };
+
   const handleGitHubPush = async () => {
     if (!githubToken.trim()) {
       setTokenError('Personal Access Token is required');
@@ -191,22 +280,45 @@ export default function App() {
     setPushSuccess(null);
     cooldown.startCooldown('push_github');
 
-    let filesToPush: Array<{ path: string; content: string }> = [
-      { path: 'CORRECT.md', content: correctMd },
-      { path: 'WRONG.md', content: wrongMd },
-      { path: 'stuff.md', content: stuffMd },
-      { path: 'COMMITS_LEDGER.md', content: `# Complete Commit Ledger\n\nTotal Commits Analyzed: ${commits.length}\n\n` + commits.map(c => `### [${c.verdict}] ${c.shortHash} - ${c.subject}\n**Author:** ${c.author} | **Date:** ${c.date}\n${c.reason ? `**Note:** ${c.reason}\n` : ''}\n\`\`\`diff\n${c.diff}\n\`\`\`\n`).join('\n---\n\n') },
-      { path: 'RAW_GIT_LOG.txt', content: rawLogInput || 'No raw git log captured' },
-    ];
+    let filesToPush: Array<{ path: string; content: string }> = [];
 
-    if (pushScope === 'full_app') {
+    if (selectedPushFiles.correct) {
+      filesToPush.push({ path: 'CORRECT.md', content: correctMd });
+    }
+    if (selectedPushFiles.wrong) {
+      filesToPush.push({ path: 'WRONG.md', content: wrongMd });
+    }
+    if (selectedPushFiles.stuff) {
+      filesToPush.push({ path: 'stuff.md', content: stuffMd });
+    }
+    if (selectedPushFiles.ledger) {
+      filesToPush.push({
+        path: 'COMMITS_LEDGER.md',
+        content: `# Complete Commit Ledger\n\nTotal Commits Analyzed: ${commits.length}\n\n` + commits.map(c => `### [${c.verdict}] ${c.shortHash} - ${c.subject}\n**Author:** ${c.author} | **Date:** ${c.date}\n${c.reason ? `**Note:** ${c.reason}\n` : ''}\n\`\`\`diff\n${c.diff}\n\`\`\`\n`).join('\n---\n\n')
+      });
+    }
+    if (selectedPushFiles.rawLog) {
+      filesToPush.push({ path: 'RAW_GIT_LOG.txt', content: rawLogInput || 'No raw git log captured' });
+    }
+    if (selectedPushFiles.readme || pushScope === 'full_app') {
+      filesToPush.push({
+        path: 'README.md',
+        content: `# Archaeology Engine Deliverables\n\nGenerated by Commit Archaeology Engine (CAE).\n\n## Included Deliverables in this Folder\n- \`CORRECT.md\` - Success ledger with all confirmed clean commits (newest first)\n- \`WRONG.md\` - Failure and recovery ledger with paired fix commits (newest first)\n- \`stuff.md\` - Gemini intelligence deep archaeological report\n- \`COMMITS_LEDGER.md\` - Complete per-commit breakdown and unified diffs\n- \`RAW_GIT_LOG.txt\` - Complete extracted git history\n`
+      });
+    }
+    if (selectedPushFiles.summary || pushScope === 'full_app') {
       filesToPush.push(
-        { path: 'README.md', content: `# Archaeology Engine Deliverables\n\nGenerated by Commit Archaeology Engine (CAE).\n\n## Included Deliverables in this Folder\n- \`CORRECT.md\` - Success ledger with all confirmed clean commits (newest first)\n- \`WRONG.md\` - Failure and recovery ledger with paired fix commits (newest first)\n- \`stuff.md\` - Gemini intelligence deep archaeological report\n- \`COMMITS_LEDGER.md\` - Complete per-commit breakdown and unified diffs\n- \`RAW_GIT_LOG.txt\` - Complete extracted git history\n` },
         { path: 'SUMMARY.json', content: JSON.stringify({ stats, totalCommits: commits.length, timestamp: new Date().toISOString() }, null, 2) },
         { path: 'metadata.json', content: JSON.stringify({ name: "Archaeology Engine Deliverables", description: "Commit Archaeology Engine deliverables" }, null, 2) }
       );
     }
 
+    if (filesToPush.length === 0) {
+      setPushError('Please select at least one file variable to push.');
+      setPushingToGitHub(false);
+      return;
+    }
+
     try {
       const res = await fetch('/api/github/push', {
         method: 'POST',
@@ -217,6 +329,7 @@ export default function App() {
           isPrivate: isPrivateRepo,
           targetFolder: targetFolder.trim(),
           files: filesToPush,
+          appendOnly: true,
         }),
       });
 
@@ -233,6 +346,22 @@ export default function App() {
     }
   };
 
+  const handleAppendToMarkdown = (targetFile: 'correct' | 'wrong' | 'stuff', contentToAdd: string) => {
+    if (!contentToAdd || !contentToAdd.trim()) return;
+    const timestamp = new Date().toISOString().replace('T', ' ').substring(0, 19);
+    const entryHeader = `\n\n---\n\n## [Appended Record - ${timestamp}]\n\n`;
+    
+    if (targetFile === 'correct') {
+      setCorrectMd(prev => `${prev.trimEnd()}${entryHeader}${contentToAdd.trim()}\n`);
+    } else if (targetFile === 'wrong') {
+      setWrongMd(prev => `${prev.trimEnd()}${entryHeader}${contentToAdd.trim()}\n`);
+    } else {
+      setStuffMd(prev => `${prev.trimEnd()}${entryHeader}${contentToAdd.trim()}\n`);
+    }
+    setShowAppendModal(false);
+    setAppendEntryText('');
+  };
+
   const loadSampleRepo = async () => {
     if (cooldown.isCooling('load_sample')) return;
     setLoading(true);
@@ -264,17 +393,25 @@ export default function App() {
     }
   };
 
-  const runAnalysis = async (overrideLog?: string) => {
+  const runAnalysis = async (overrideLog?: string, shouldAppend = false) => {
     if (cooldown.isCooling('run_analysis')) return;
     setAnalyzing(true);
     setShowPasteModal(false);
     cooldown.startCooldown('run_analysis');
     try {
+      let finalLogText = overrideLog || rawLogInput;
+      if (shouldAppend && rawLogInput.trim() && overrideLog && overrideLog !== rawLogInput) {
+        finalLogText = `${rawLogInput.trim()}\n\n${overrideLog.trim()}`;
+        setRawLogInput(finalLogText);
+      } else if (overrideLog) {
+        setRawLogInput(overrideLog);
+      }
+
       const res = await fetch('/api/analyze', {
         method: 'POST',
         headers: { 'Content-Type': 'application/json' },
         body: JSON.stringify({ 
-          gitLogText: overrideLog || rawLogInput,
+          gitLogText: finalLogText,
           userGeminiApiKey: geminiApiKey,
           userModel: selectedModel,
         })
@@ -299,6 +436,68 @@ export default function App() {
     }
   };
 
+  const fetchAndAnalyzeBatchRepos = async (repoNames: string[]) => {
+    if (!repoNames || repoNames.length === 0) {
+      alert("Please select at least one repository to analyze.");
+      return;
+    }
+    if (cooldown.isCooling('fetch_repo')) {
+      alert(`Cooldown Active: Please wait ${cooldown.getRemainingSeconds('fetch_repo')}s before scanning again.`);
+      return;
+    }
+    setBatchAnalyzing(true);
+    setFetchingHistory(true);
+    setLoading(true);
+    setShowRepoModal(false);
+    cooldown.startCooldown('fetch_repo');
+    cooldown.startCooldown('quick_scan');
+
+    try {
+      let combinedLog = '';
+      let successfulRepos = 0;
+
+      for (let i = 0; i < repoNames.length; i++) {
+        const rName = repoNames[i].trim().replace(/^https?:\/\/github\.com\//i, '').replace(/\.git$/i, '').replace(/^\/+|\/+$/g, '');
+        setBatchProgress({ current: i + 1, total: repoNames.length, repoName: rName });
+        
+        try {
+          const res = await fetch('/api/github/history', {
+            method: 'POST',
+            headers: { 'Content-Type': 'application/json' },
+            body: JSON.stringify({ 
+              token: githubToken.trim(), 
+              repoFullName: rName, 
+              limit: commitLimit, 
+              fetchAll: analyzeEntireHistory 
+            }),
+          });
+          const data = await res.json();
+          if (res.ok && data.rawLogText) {
+            combinedLog += (combinedLog ? '\n\n' : '') + `=== REPOSITORY: ${rName} ===\n` + data.rawLogText;
+            successfulRepos++;
+          }
+        } catch (subErr) {
+          console.error(`Failed to fetch repo ${rName}:`, subErr);
+        }
+      }
+
+      if (combinedLog) {
+        setRawLogInput(combinedLog);
+        await runAnalysis(combinedLog);
+      } else {
+        alert("Failed to retrieve git history for the selected repositories.");
+      }
+    } catch (err: any) {
+      console.error("Batch archaeology error:", err);
+      alert("Error during multi-repository batch analysis.");
+    } finally {
+      setBatchAnalyzing(false);
+      setBatchProgress(null);
+      setFetchingHistory(false);
+      setLoading(false);
+    }
+  };
+
   const fetchAndAnalyzeRepo = async (repoFullName: string) => {
     if (!repoFullName || !repoFullName.trim()) {
       alert("Please specify a valid repository name (e.g., owner/repo or full GitHub URL).");
@@ -441,12 +640,68 @@ export default function App() {
     );
   });
 
-  const fileChartData = stats?.fileCounts 
-    ? Object.entries(stats.fileCounts)
-        .map(([file, count]) => ({ file: file.split('/').pop() || file, fullName: file, count: Number(count) }))
-        .sort((a, b) => b.count - a.count)
-        .slice(0, 6)
-    : [];
+  const handleToggleCommitSelection = (hash: string) => {
+    setSelectedCommitHashes(prev => 
+      prev.includes(hash) ? prev.filter(h => h !== hash) : [...prev, hash]
+    );
+  };
+
+  const handleSelectAllCommits = (limitOrFilter?: number | 'all' | 'correct' | 'wrong' | 'clear') => {
+    if (limitOrFilter === 'clear') {
+      setSelectedCommitHashes([]);
+      return;
+    }
+    
+    let target = filteredCommits;
+    if (limitOrFilter === 'correct') {
+      target = target.filter(c => c.verdict === 'CORRECT');
+    } else if (limitOrFilter === 'wrong') {
+      target = target.filter(c => c.verdict === 'WRONG');
+    } else if (typeof limitOrFilter === 'number' && limitOrFilter > 0) {
+      target = target.slice(0, limitOrFilter);
+    }
+
+    const targetHashes = target.map(c => c.hash);
+    const allSelected = targetHashes.length > 0 && targetHashes.every(h => selectedCommitHashes.includes(h));
+
+    if (allSelected && (!limitOrFilter || limitOrFilter === 'all')) {
+      setSelectedCommitHashes(prev => prev.filter(h => !targetHashes.includes(h)));
+    } else {
+      setSelectedCommitHashes(prev => Array.from(new Set([...prev, ...targetHashes])));
+    }
+  };
+
+  const focusAnalyzeSelectedCommits = async () => {
+    if (selectedCommitHashes.length === 0) {
+      alert("Please select at least one commit to analyze.");
+      return;
+    }
+    const selected = commits.filter(c => selectedCommitHashes.includes(c.hash));
+    const extractedLog = selected.map(c => 
+      `commit ${c.hash}\nAuthor: ${c.author}\nDate: ${c.date}\n\n    ${c.subject}\n\n${c.body ? `    ${c.body}\n\n` : ''}${c.diff}`
+    ).join('\n\n');
+    
+    await runAnalysis(extractedLog);
+  };
+
+  const copySelectedCommitsMarkdown = () => {
+    if (selectedCommitHashes.length === 0) return;
+    const selected = commits.filter(c => selectedCommitHashes.includes(c.hash));
+    const text = `# Selected Commits (${selected.length})\n\n` + 
+      selected.map(c => `### [${c.verdict}] ${c.shortHash} - ${c.subject}\n**Author:** ${c.author} | **Date:** ${c.date}\n${c.reason ? `**Verdict Reason:** ${c.reason}\n` : ''}\n\`\`\`diff\n${c.diff}\n\`\`\`\n`).join('\n---\n\n');
+    navigator.clipboard.writeText(text);
+    copyToClipboard(text, 'selected_commits');
+  };
+
+  const fileChartData = useMemo(() => {
+    if (!stats?.fileCounts) return [];
+    const entries = Object.entries(stats.fileCounts)
+      .map(([file, count]) => ({ file: file.split('/').pop() || file, fullName: file, count: Number(count) }))
+      .sort((a, b) => b.count - a.count);
+    
+    if (hotspotsLimit === 'all') return entries;
+    return entries.slice(0, typeof hotspotsLimit === 'number' ? hotspotsLimit : 6);
+  }, [stats?.fileCounts, hotspotsLimit]);
 
   return (
     <div className="min-h-screen bg-black text-neutral-100 flex flex-col font-sans antialiased selection:bg-blue-500 selection:text-white relative overflow-x-hidden">
@@ -809,10 +1064,29 @@ export default function App() {
                 <div className="grid grid-cols-1 lg:grid-cols-12 gap-6">
                   {/* Top Touched Files Chart */}
                   <div className="lg:col-span-5 bg-neutral-900/80 p-6 rounded-2xl border border-neutral-800/80 shadow-sm flex flex-col">
-                    <h3 className="text-xs font-bold font-mono uppercase tracking-wider text-neutral-300 mb-4 flex items-center gap-2">
-                      <FileCode className="w-4 h-4 text-blue-400" />
-                      Architectural Hotspots
-                    </h3>
+                    <div className="flex items-center justify-between mb-4">
+                      <h3 className="text-xs font-bold font-mono uppercase tracking-wider text-neutral-300 flex items-center gap-2">
+                        <FileCode className="w-4 h-4 text-blue-400" />
+                        <span>Architectural Hotspots</span>
+                      </h3>
+                      {/* Hotspots Limit Controls */}
+                      <div className="flex items-center space-x-1">
+                        {[5, 10, 20, 'all'].map((limitVal) => (
+                          <button
+                            key={String(limitVal)}
+                            type="button"
+                            onClick={() => setHotspotsLimit(limitVal as any)}
+                            className={`px-2 py-0.5 rounded text-[10px] font-mono transition-colors ${
+                              hotspotsLimit === limitVal
+                                ? 'bg-blue-600 text-white font-bold'
+                                : 'bg-neutral-800 text-neutral-400 hover:text-white'
+                            }`}
+                          >
+                            {limitVal === 'all' ? 'All' : `Top ${limitVal}`}
+                          </button>
+                        ))}
+                      </div>
+                    </div>
                     <div className="h-60 w-full">
                       <ResponsiveContainer width="100%" height="100%">
                         <BarChart data={fileChartData} layout="vertical" margin={{ left: -10, right: 10 }}>
@@ -842,11 +1116,13 @@ export default function App() {
                             onClick={() => {
                               setSelectedThemeFilter(null);
                               setSearchQuery('');
-                              setActiveTab('commits');
+                              handleSelectAllCommits('all');
+                              setActiveTab('search');
                             }}
-                            className="inline-flex items-center space-x-1 text-[11px] font-mono text-neutral-400 hover:text-white px-2 py-0.5 rounded-lg bg-neutral-800 border border-neutral-700"
+                            className="inline-flex items-center space-x-1 text-[11px] font-mono text-neutral-300 hover:text-white px-2.5 py-1 rounded-lg bg-neutral-800 hover:bg-neutral-700 border border-neutral-700 transition-colors"
                           >
-                            <span>Select All Commits</span>
+                            <CheckSquare className="w-3 h-3 text-blue-400" />
+                            <span>Select All Commits ({commits.length})</span>
                           </button>
                           {selectedThemeFilter && (
                             <button
@@ -995,6 +1271,16 @@ export default function App() {
                   </div>
 
                   <div className="flex items-center space-x-2">
+                    <button
+                      onClick={() => {
+                        setAppendEntryFile(activeTab);
+                        setShowAppendModal(true);
+                      }}
+                      className="inline-flex items-center space-x-1.5 bg-emerald-500/10 hover:bg-emerald-500/20 text-emerald-300 border border-emerald-500/30 text-xs font-mono px-3.5 py-2 rounded-xl transition-all"
+                    >
+                      <Plus className="w-3.5 h-3.5" />
+                      <span>Add / Append to File</span>
+                    </button>
                     <button
                       onClick={() => setShowGitHubModal(true)}
                       className="inline-flex items-center space-x-1.5 bg-blue-500/10 hover:bg-blue-500/20 text-blue-300 border border-blue-500/30 text-xs font-mono px-3.5 py-2 rounded-xl transition-all"
@@ -1028,9 +1314,12 @@ export default function App() {
             {/* Search & Filter Tab */}
             {activeTab === 'search' && (
               <div className="space-y-6">
-                <div className="bg-neutral-900/90 p-6 rounded-2xl border border-neutral-800 shadow-sm space-y-3">
+                <div className="bg-neutral-900/90 p-6 rounded-2xl border border-neutral-800 shadow-sm space-y-4">
                   <div className="flex items-center justify-between">
-                    <h2 className="text-xs font-bold font-mono uppercase tracking-wider text-neutral-300">Search & Filter Commit History</h2>
+                    <h2 className="text-xs font-bold font-mono uppercase tracking-wider text-neutral-300 flex items-center gap-2">
+                      <ListFilter className="w-4 h-4 text-blue-400" />
+                      <span>Search & Filter Commit History</span>
+                    </h2>
                     {(searchQuery || selectedThemeFilter) && (
                       <button
                         onClick={() => {
@@ -1056,49 +1345,160 @@ export default function App() {
                       className="w-full pl-11 pr-4 py-3 rounded-xl border border-neutral-800 bg-black text-xs font-mono text-neutral-200 placeholder-neutral-600 focus:outline-none focus:border-blue-500 focus:ring-1 focus:ring-blue-500/50"
                     />
                   </div>
+
+                  {/* Select All Limits for Commits */}
+                  <div className="flex flex-wrap items-center justify-between gap-2 pt-2 border-t border-neutral-800/60">
+                    <div className="flex flex-wrap items-center gap-1.5">
+                      <span className="text-[11px] font-mono text-neutral-400 flex items-center gap-1 mr-1">
+                        <Sliders className="w-3 h-3 text-blue-400" />
+                        Select Limits:
+                      </span>
+                      <button
+                        type="button"
+                        onClick={() => handleSelectAllCommits('all')}
+                        className="px-2.5 py-1 bg-neutral-800 hover:bg-neutral-700 text-neutral-200 text-xs font-mono rounded-lg border border-neutral-700 transition-colors flex items-center gap-1"
+                      >
+                        <CheckSquare className="w-3 h-3 text-blue-400" />
+                        <span>Select All ({filteredCommits.length})</span>
+                      </button>
+                      {[10, 25, 50, 100].map((num) => (
+                        <button
+                          key={num}
+                          type="button"
+                          onClick={() => handleSelectAllCommits(num)}
+                          className="px-2 py-1 bg-neutral-900 hover:bg-neutral-800 text-neutral-300 text-xs font-mono rounded-lg border border-neutral-800 hover:border-neutral-700 transition-colors"
+                        >
+                          Top {num}
+                        </button>
+                      ))}
+                      <button
+                        type="button"
+                        onClick={() => handleSelectAllCommits('correct')}
+                        className="px-2 py-1 bg-emerald-950/30 hover:bg-emerald-900/40 text-emerald-300 text-xs font-mono rounded-lg border border-emerald-500/30 transition-colors"
+                      >
+                        Correct Only
+                      </button>
+                      <button
+                        type="button"
+                        onClick={() => handleSelectAllCommits('wrong')}
+                        className="px-2 py-1 bg-rose-950/30 hover:bg-rose-900/40 text-rose-300 text-xs font-mono rounded-lg border border-rose-500/30 transition-colors"
+                      >
+                        Wrong Only
+                      </button>
+                    </div>
+
+                    {selectedCommitHashes.length > 0 && (
+                      <button
+                        type="button"
+                        onClick={() => handleSelectAllCommits('clear')}
+                        className="text-xs font-mono text-neutral-400 hover:text-white px-2 py-1 rounded bg-neutral-800/80"
+                      >
+                        Clear Selection ({selectedCommitHashes.length})
+                      </button>
+                    )}
+                  </div>
                 </div>
 
+                {/* Batch Action Toolbar when commits are selected */}
+                {selectedCommitHashes.length > 0 && (
+                  <div className="sticky top-20 z-20 p-4 rounded-2xl bg-blue-950/90 backdrop-blur-md border border-blue-500/40 shadow-lg flex flex-col sm:flex-row items-center justify-between gap-3 animate-in fade-in">
+                    <div className="flex items-center gap-2">
+                      <div className="w-6 h-6 rounded-lg bg-blue-500/20 flex items-center justify-center text-blue-300">
+                        <CheckCheck className="w-3.5 h-3.5" />
+                      </div>
+                      <span className="text-xs font-mono text-blue-200 font-semibold">
+                        {selectedCommitHashes.length} commits selected
+                      </span>
+                    </div>
+
+                    <div className="flex flex-wrap items-center gap-2">
+                      <button
+                        type="button"
+                        onClick={focusAnalyzeSelectedCommits}
+                        disabled={analyzing}
+                        className="px-3.5 py-1.5 bg-blue-600 hover:bg-blue-500 disabled:opacity-50 text-white text-xs font-semibold rounded-xl transition-all shadow-sm flex items-center gap-1.5"
+                      >
+                        <Brain className="w-3.5 h-3.5" />
+                        <span>Focus Analyze Selected ({selectedCommitHashes.length})</span>
+                      </button>
+
+                      <button
+                        type="button"
+                        onClick={copySelectedCommitsMarkdown}
+                        className="px-3 py-1.5 bg-neutral-800 hover:bg-neutral-700 text-neutral-200 text-xs font-mono rounded-xl border border-neutral-700 transition-all flex items-center gap-1.5"
+                      >
+                        {copiedTab === 'selected_commits' ? <Check className="w-3.5 h-3.5 text-emerald-400" /> : <Copy className="w-3.5 h-3.5 text-neutral-400" />}
+                        <span>Copy Markdown</span>
+                      </button>
+
+                      <button
+                        type="button"
+                        onClick={() => setSelectedCommitHashes([])}
+                        className="text-xs font-mono text-neutral-400 hover:text-white px-2 py-1"
+                      >
+                        Deselect
+                      </button>
+                    </div>
+                  </div>
+                )}
+
                 <div className="space-y-3">
                   {filteredCommits.length === 0 ? (
                     <div className="text-center py-16 bg-neutral-900/40 rounded-2xl border border-neutral-800/80 text-neutral-500 text-xs font-mono">
                       No commits match current search filter criteria.
                     </div>
                   ) : (
-                    filteredCommits.map((c) => (
-                      <div key={c.hash} className="bg-neutral-900/80 p-5 rounded-2xl border border-neutral-800/80 space-y-3 hover:border-neutral-700 transition-all shadow-sm">
-                        <div className="flex items-center justify-between">
-                          <div className="flex items-center space-x-2.5">
-                            <span className={`px-2.5 py-0.5 rounded-full text-[10px] font-mono font-bold uppercase ${
-                              c.verdict === 'OK' ? 'bg-emerald-500/10 text-emerald-400 border border-emerald-500/20' : 'bg-rose-500/10 text-rose-400 border border-rose-500/20'
-                            }`}>
-                              {c.verdict}
-                            </span>
-                            <span className="font-mono text-xs text-blue-400 font-bold">{c.shortHash}</span>
-                            <span className="text-xs text-neutral-500">• {c.date}</span>
+                    filteredCommits.map((c) => {
+                      const isSelected = selectedCommitHashes.includes(c.hash);
+                      return (
+                        <div 
+                          key={c.hash} 
+                          className={`p-5 rounded-2xl border transition-all shadow-sm space-y-3 ${
+                            isSelected 
+                              ? 'bg-blue-950/20 border-blue-500/50' 
+                              : 'bg-neutral-900/80 border-neutral-800/80 hover:border-neutral-700'
+                          }`}
+                        >
+                          <div className="flex items-center justify-between">
+                            <div className="flex items-center space-x-2.5">
+                              <input
+                                type="checkbox"
+                                checked={isSelected}
+                                onChange={() => handleToggleCommitSelection(c.hash)}
+                                className="w-4 h-4 rounded border-neutral-700 bg-neutral-900 text-blue-600 focus:ring-blue-500 cursor-pointer"
+                              />
+                              <span className={`px-2.5 py-0.5 rounded-full text-[10px] font-mono font-bold uppercase ${
+                                c.verdict === 'OK' ? 'bg-emerald-500/10 text-emerald-400 border border-emerald-500/20' : 'bg-rose-500/10 text-rose-400 border border-rose-500/20'
+                              }`}>
+                                {c.verdict}
+                              </span>
+                              <span className="font-mono text-xs text-blue-400 font-bold">{c.shortHash}</span>
+                              <span className="text-xs text-neutral-500">• {c.date}</span>
+                            </div>
+                            {c.fixedBy && (
+                              <span className="text-[11px] font-mono text-blue-300 bg-blue-500/10 px-2.5 py-1 rounded-lg border border-blue-500/20">
+                                Fixed by: {c.fixedBy}
+                              </span>
+                            )}
                           </div>
-                          {c.fixedBy && (
-                            <span className="text-[11px] font-mono text-blue-300 bg-blue-500/10 px-2.5 py-1 rounded-lg border border-blue-500/20">
-                              Fixed by: {c.fixedBy}
-                            </span>
-                          )}
-                        </div>
 
-                        <h3 className="text-sm font-semibold text-neutral-200">{c.subject}</h3>
-                        {c.reason && (
-                          <p className="text-xs text-rose-300 bg-rose-500/10 p-3 rounded-xl border border-rose-500/20 font-mono">
-                            Reason: {c.reason}
-                          </p>
-                        )}
+                          <h3 className="text-sm font-semibold text-neutral-200">{c.subject}</h3>
+                          {c.reason && (
+                            <p className="text-xs text-rose-300 bg-rose-500/10 p-3 rounded-xl border border-rose-500/20 font-mono">
+                              Reason: {c.reason}
+                            </p>
+                          )}
 
-                        <div className="flex flex-wrap gap-1.5 pt-1">
-                          {c.files.map((f: string) => (
-                            <span key={f} className="text-[11px] bg-black text-neutral-400 px-2.5 py-1 rounded-lg font-mono border border-neutral-800">
-                              {f}
-                            </span>
-                          ))}
+                          <div className="flex flex-wrap gap-1.5 pt-1">
+                            {c.files.map((f: string) => (
+                              <span key={f} className="text-[11px] bg-black text-neutral-400 px-2.5 py-1 rounded-lg font-mono border border-neutral-800">
+                                {f}
+                              </span>
+                            ))}
+                          </div>
                         </div>
-                      </div>
-                    ))
+                      );
+                    })
                   )}
                 </div>
               </div>
@@ -1326,6 +1726,94 @@ export default function App() {
                   </div>
                 </div>
 
+                {/* Variable Selection Limits Configuration */}
+                <div className="space-y-4">
+                  <div className="flex items-center gap-2 text-blue-400 pb-2 border-b border-neutral-800/50">
+                    <Sliders className="w-4 h-4" />
+                    <h3 className="text-xs font-bold font-mono uppercase tracking-widest">Select All Limits for Variables</h3>
+                  </div>
+
+                  <div className="space-y-4 bg-black p-3.5 rounded-xl border border-neutral-800">
+                    {/* Hotspot Display Limit */}
+                    <div className="space-y-1.5">
+                      <label className="flex items-center justify-between text-xs font-semibold text-neutral-300">
+                        <span>Architectural Hotspots Display Limit</span>
+                        <span className="text-purple-400 font-mono text-[11px] font-bold">
+                          {hotspotsLimit === 'all' ? 'All Files' : `Top ${hotspotsLimit} Files`}
+                        </span>
+                      </label>
+                      <div className="flex items-center gap-1.5">
+                        {[5, 10, 20, 'all'].map((lim) => (
+                          <button
+                            key={String(lim)}
+                            type="button"
+                            onClick={() => setHotspotsLimit(lim as any)}
+                            className={`flex-1 py-1.5 rounded-lg text-xs font-mono border transition-all ${
+                              hotspotsLimit === lim
+                                ? 'bg-purple-600/30 text-purple-300 border-purple-500/50 font-bold'
+                                : 'bg-neutral-900 text-neutral-400 border-neutral-800 hover:text-white'
+                            }`}
+                          >
+                            {lim === 'all' ? 'All' : `Top ${lim}`}
+                          </button>
+                        ))}
+                      </div>
+                    </div>
+
+                    {/* Commit Select All Default Limit */}
+                    <div className="space-y-1.5 pt-2 border-t border-neutral-900">
+                      <label className="flex items-center justify-between text-xs font-semibold text-neutral-300">
+                        <span>Commit Selection Batch Limit</span>
+                        <span className="text-blue-400 font-mono text-[11px] font-bold">
+                          {commitSelectLimit === 'all' ? 'All Commits' : `Top ${commitSelectLimit} Commits`}
+                        </span>
+                      </label>
+                      <div className="flex items-center gap-1.5">
+                        {[10, 25, 50, 100, 'all'].map((lim) => (
+                          <button
+                            key={String(lim)}
+                            type="button"
+                            onClick={() => setCommitSelectLimit(lim as any)}
+                            className={`flex-1 py-1.5 rounded-lg text-xs font-mono border transition-all ${
+                              commitSelectLimit === lim
+                                ? 'bg-blue-600/30 text-blue-300 border-blue-500/50 font-bold'
+                                : 'bg-neutral-900 text-neutral-400 border-neutral-800 hover:text-white'
+                            }`}
+                          >
+                            {lim === 'all' ? 'All' : lim}
+                          </button>
+                        ))}
+                      </div>
+                    </div>
+
+                    {/* Repository Batch Limit */}
+                    <div className="space-y-1.5 pt-2 border-t border-neutral-900">
+                      <label className="flex items-center justify-between text-xs font-semibold text-neutral-300">
+                        <span>Repository Selection Limit</span>
+                        <span className="text-emerald-400 font-mono text-[11px] font-bold">
+                          {repoSelectLimit === 'all' ? 'All Repos' : `Top ${repoSelectLimit} Repos`}
+                        </span>
+                      </label>
+                      <div className="flex items-center gap-1.5">
+                        {[5, 10, 25, 50, 'all'].map((lim) => (
+                          <button
+                            key={String(lim)}
+                            type="button"
+                            onClick={() => setRepoSelectLimit(lim as any)}
+                            className={`flex-1 py-1.5 rounded-lg text-xs font-mono border transition-all ${
+                              repoSelectLimit === lim
+                                ? 'bg-emerald-600/30 text-emerald-300 border-emerald-500/50 font-bold'
+                                : 'bg-neutral-900 text-neutral-400 border-neutral-800 hover:text-white'
+                            }`}
+                          >
+                            {lim === 'all' ? 'All' : lim}
+                          </button>
+                        ))}
+                      </div>
+                    </div>
+                  </div>
+                </div>
+
                 {/* Gemini API Preferences */}
                 <div className="space-y-4">
                   <div className="flex items-center gap-2 text-purple-400 pb-2 border-b border-neutral-800/50">
@@ -1584,30 +2072,111 @@ export default function App() {
                 </div>
               </div>
 
-              {/* Scope Selection */}
+              {/* Scope Selection & Granular File Variable Selection */}
               <div>
-                <label className="text-[11px] font-semibold text-neutral-300 block mb-1">Complete Files to Place in Folder</label>
-                <div className="space-y-2 bg-black p-3.5 rounded-xl border border-neutral-800 text-xs font-mono text-neutral-300">
-                  <label className="flex items-center space-x-2 cursor-pointer">
+                <div className="flex items-center justify-between mb-2">
+                  <label className="text-[11px] font-semibold text-neutral-300">Select Deliverable File Variables</label>
+                  <div className="flex items-center space-x-1">
+                    <button
+                      type="button"
+                      onClick={() => handleSelectAllPushFiles('all')}
+                      className="text-[10px] font-mono px-2 py-0.5 rounded bg-neutral-800 text-blue-300 hover:text-white border border-neutral-700"
+                    >
+                      Select All Files
+                    </button>
+                    <button
+                      type="button"
+                      onClick={() => handleSelectAllPushFiles('core_md')}
+                      className="text-[10px] font-mono px-2 py-0.5 rounded bg-neutral-800 text-neutral-300 hover:text-white border border-neutral-700"
+                    >
+                      Core 3 (.md)
+                    </button>
+                    <button
+                      type="button"
+                      onClick={() => handleSelectAllPushFiles('logs')}
+                      className="text-[10px] font-mono px-2 py-0.5 rounded bg-neutral-800 text-neutral-300 hover:text-white border border-neutral-700"
+                    >
+                      Logs Only
+                    </button>
+                    <button
+                      type="button"
+                      onClick={() => handleSelectAllPushFiles('none')}
+                      className="text-[10px] font-mono px-2 py-0.5 rounded bg-neutral-800 text-neutral-400 hover:text-white border border-neutral-700"
+                    >
+                      Deselect
+                    </button>
+                  </div>
+                </div>
+
+                <div className="grid grid-cols-1 sm:grid-cols-2 gap-2 bg-black p-3.5 rounded-xl border border-neutral-800 text-xs font-mono">
+                  <label className="flex items-center space-x-2 cursor-pointer p-1 rounded hover:bg-neutral-900">
+                    <input
+                      type="checkbox"
+                      checked={selectedPushFiles.correct}
+                      onChange={(e) => setSelectedPushFiles(p => ({ ...p, correct: e.target.checked }))}
+                      className="w-3.5 h-3.5 rounded accent-emerald-500"
+                    />
+                    <span className="text-emerald-400">CORRECT.md</span>
+                  </label>
+                  <label className="flex items-center space-x-2 cursor-pointer p-1 rounded hover:bg-neutral-900">
                     <input
-                      type="radio"
-                      name="pushScope"
-                      checked={pushScope === 'deliverables'}
-                      onChange={() => setPushScope('deliverables')}
-                      className="accent-blue-500"
+                      type="checkbox"
+                      checked={selectedPushFiles.wrong}
+                      onChange={(e) => setSelectedPushFiles(p => ({ ...p, wrong: e.target.checked }))}
+                      className="w-3.5 h-3.5 rounded accent-rose-500"
                     />
-                    <span>All Core Files (<code className="text-emerald-400">CORRECT.md</code>, <code className="text-rose-400">WRONG.md</code>, <code className="text-purple-400">stuff.md</code>, <code className="text-blue-400">COMMITS_LEDGER.md</code>, <code className="text-neutral-400">RAW_GIT_LOG.txt</code>)</span>
+                    <span className="text-rose-400">WRONG.md</span>
                   </label>
-                  <label className="flex items-center space-x-2 cursor-pointer">
+                  <label className="flex items-center space-x-2 cursor-pointer p-1 rounded hover:bg-neutral-900">
                     <input
-                      type="radio"
-                      name="pushScope"
-                      checked={pushScope === 'full_app'}
-                      onChange={() => setPushScope('full_app')}
-                      className="accent-blue-500"
+                      type="checkbox"
+                      checked={selectedPushFiles.stuff}
+                      onChange={(e) => setSelectedPushFiles(p => ({ ...p, stuff: e.target.checked }))}
+                      className="w-3.5 h-3.5 rounded accent-purple-500"
                     />
-                    <span>Complete Bundle + README.md & SUMMARY.json Metrics</span>
+                    <span className="text-purple-400">stuff.md</span>
                   </label>
+                  <label className="flex items-center space-x-2 cursor-pointer p-1 rounded hover:bg-neutral-900">
+                    <input
+                      type="checkbox"
+                      checked={selectedPushFiles.ledger}
+                      onChange={(e) => setSelectedPushFiles(p => ({ ...p, ledger: e.target.checked }))}
+                      className="w-3.5 h-3.5 rounded accent-blue-500"
+                    />
+                    <span className="text-blue-400">COMMITS_LEDGER.md</span>
+                  </label>
+                  <label className="flex items-center space-x-2 cursor-pointer p-1 rounded hover:bg-neutral-900">
+                    <input
+                      type="checkbox"
+                      checked={selectedPushFiles.rawLog}
+                      onChange={(e) => setSelectedPushFiles(p => ({ ...p, rawLog: e.target.checked }))}
+                      className="w-3.5 h-3.5 rounded accent-neutral-500"
+                    />
+                    <span className="text-neutral-300">RAW_GIT_LOG.txt</span>
+                  </label>
+                  <label className="flex items-center space-x-2 cursor-pointer p-1 rounded hover:bg-neutral-900">
+                    <input
+                      type="checkbox"
+                      checked={selectedPushFiles.summary}
+                      onChange={(e) => setSelectedPushFiles(p => ({ ...p, summary: e.target.checked }))}
+                      className="w-3.5 h-3.5 rounded accent-amber-500"
+                    />
+                    <span className="text-amber-400">SUMMARY.json & README</span>
+                  </label>
+                </div>
+              </div>
+
+              {/* Append-Only Protection Badge */}
+              <div className="bg-emerald-500/10 border border-emerald-500/30 rounded-xl p-3.5 flex items-start gap-2.5 text-xs font-mono">
+                <ShieldCheck className="w-4 h-4 text-emerald-400 shrink-0 mt-0.5" />
+                <div className="space-y-1">
+                  <div className="flex items-center gap-2">
+                    <span className="font-bold text-emerald-300">Mode: Add to File Only (Append Mode)</span>
+                    <span className="text-[9px] bg-emerald-500/20 text-emerald-300 px-1.5 py-0.5 rounded uppercase font-bold">Active</span>
+                  </div>
+                  <p className="text-[11px] text-neutral-400 font-sans">
+                    Existing repository deliverables are never overwritten. New commits and analysis records will be added to the end of existing files preserving all history.
+                  </p>
                 </div>
               </div>
             </div>
@@ -1625,7 +2194,7 @@ export default function App() {
                 <div className="flex items-center justify-between text-emerald-300 font-bold">
                   <span className="flex items-center gap-1.5">
                     <CheckCircle2 className="w-4 h-4 text-emerald-400" />
-                    Successfully created & pushed to GitHub!
+                    Successfully pushed to GitHub!
                   </span>
                   <a
                     href={pushSuccess.repoUrl}
@@ -1640,7 +2209,13 @@ export default function App() {
                 <div className="space-y-1 font-mono text-[11px] text-neutral-300 pt-1">
                   {pushSuccess.results.map(r => (
                     <div key={r.path} className="flex items-center justify-between">
-                      <span>✓ {r.path}</span>
+                      <span className="flex items-center gap-1.5">
+                        <span className="text-emerald-400">✓</span>
+                        <span>{r.path}</span>
+                        <span className="text-[9px] px-1.5 py-0.2 rounded bg-neutral-800 text-neutral-400">
+                          {r.status === 'appended' ? 'Appended to existing file' : 'Created new file'}
+                        </span>
+                      </span>
                       <a href={r.url} target="_blank" rel="noopener noreferrer" className="text-neutral-400 hover:text-neutral-200">
                         view file
                       </a>
@@ -1669,18 +2244,18 @@ export default function App() {
                   <CooldownButtonContent
                     isCooling={true}
                     remainingSeconds={cooldown.getRemainingSeconds('push_github')}
-                    idleText="Create Repo & Push Files"
+                    idleText="Push (Add to Files)"
                     coolingText="Push Cooldown"
                   />
                 ) : pushingToGitHub ? (
                   <>
                     <RefreshCw className="w-3.5 h-3.5 animate-spin" />
-                    <span>Creating & Pushing...</span>
+                    <span>Pushing & Adding to Files...</span>
                   </>
                 ) : (
                   <>
                     <UploadCloud className="w-3.5 h-3.5" />
-                    <span>Create Repo & Push Files</span>
+                    <span>Push (Add to Files Only)</span>
                   </>
                 )}
               </button>
@@ -1718,28 +2293,118 @@ export default function App() {
               className="w-full font-mono text-xs p-4 rounded-xl border border-neutral-800 bg-black text-neutral-200 placeholder-neutral-600 focus:outline-none focus:border-blue-500 focus:ring-1 focus:ring-blue-500/50"
             />
 
-            <div className="flex justify-end space-x-3">
+            <div className="flex items-center justify-between pt-1">
+              <label className="flex items-center space-x-2 text-xs font-mono text-neutral-300 cursor-pointer">
+                <input
+                  type="checkbox"
+                  checked={appendLogToCurrent}
+                  onChange={(e) => setAppendLogToCurrent(e.target.checked)}
+                  className="rounded border-neutral-700 bg-black text-blue-500 focus:ring-0"
+                />
+                <span>Add / Append to existing ledger (don't overwrite)</span>
+              </label>
+
+              <div className="flex space-x-3">
+                <button
+                  onClick={() => setShowPasteModal(false)}
+                  className="px-4 py-2 rounded-xl text-xs font-medium text-neutral-400 hover:bg-neutral-800 transition-all"
+                >
+                  Cancel
+                </button>
+                <button
+                  onClick={() => runAnalysis(undefined, appendLogToCurrent)}
+                  disabled={cooldown.isCooling('run_analysis')}
+                  className="px-5 py-2.5 rounded-xl bg-white hover:bg-neutral-200 text-xs font-semibold text-black transition-all shadow-sm disabled:opacity-50"
+                >
+                  {cooldown.isCooling('run_analysis') ? (
+                    <CooldownButtonContent
+                      isCooling={true}
+                      remainingSeconds={cooldown.getRemainingSeconds('run_analysis')}
+                      idleText="Run Archaeology Analysis"
+                      coolingText="Analysis Cooldown"
+                    />
+                  ) : (
+                    <span>{appendLogToCurrent ? 'Add to File & Analyze' : 'Run Archaeology Analysis'}</span>
+                  )}
+                </button>
+              </div>
+            </div>
+          </div>
+        </div>
+      )}
+
+      {/* Manual Append to File Dialog */}
+      {showAppendModal && (
+        <div className="fixed inset-0 z-50 bg-black/80 backdrop-blur-md flex items-center justify-center p-4">
+          <div className="bg-neutral-900 rounded-2xl max-w-xl w-full p-6 sm:p-8 shadow-sm border border-neutral-800 space-y-4">
+            <div className="flex items-center justify-between">
+              <h3 className="text-sm font-bold text-white flex items-center gap-2">
+                <Plus className="w-4 h-4 text-emerald-400" />
+                Add Record to {appendEntryFile === 'correct' ? 'CORRECT.md' : appendEntryFile === 'wrong' ? 'WRONG.md' : 'stuff.md'}
+              </h3>
+              <button 
+                onClick={() => setShowAppendModal(false)}
+                className="text-neutral-400 hover:text-neutral-200 text-sm font-semibold"
+              >
+                ✕
+              </button>
+            </div>
+            
+            <p className="text-xs text-neutral-400">
+              Append a new commit record, fix notes, or architectural observation. This strictly adds to the file without overwriting any existing lines.
+            </p>
+
+            <div className="flex items-center space-x-2">
+              <button
+                type="button"
+                onClick={() => setAppendEntryFile('correct')}
+                className={`flex-1 py-1.5 rounded-lg text-xs font-mono border transition-all ${
+                  appendEntryFile === 'correct' ? 'bg-emerald-500/20 text-emerald-300 border-emerald-500/40' : 'bg-black text-neutral-400 border-neutral-800'
+                }`}
+              >
+                CORRECT.md
+              </button>
+              <button
+                type="button"
+                onClick={() => setAppendEntryFile('wrong')}
+                className={`flex-1 py-1.5 rounded-lg text-xs font-mono border transition-all ${
+                  appendEntryFile === 'wrong' ? 'bg-rose-500/20 text-rose-300 border-rose-500/40' : 'bg-black text-neutral-400 border-neutral-800'
+                }`}
+              >
+                WRONG.md
+              </button>
+              <button
+                type="button"
+                onClick={() => setAppendEntryFile('stuff')}
+                className={`flex-1 py-1.5 rounded-lg text-xs font-mono border transition-all ${
+                  appendEntryFile === 'stuff' ? 'bg-purple-500/20 text-purple-300 border-purple-500/40' : 'bg-black text-neutral-400 border-neutral-800'
+                }`}
+              >
+                stuff.md
+              </button>
+            </div>
+
+            <textarea
+              rows={8}
+              value={appendEntryText}
+              onChange={(e) => setAppendEntryText(e.target.value)}
+              placeholder="### Commit / Observation Record&#10;**Author:** ...&#10;**Notes:** ...&#10;&#10;```diff&#10;+ added lines&#10;```"
+              className="w-full font-mono text-xs p-4 rounded-xl border border-neutral-800 bg-black text-neutral-200 placeholder-neutral-600 focus:outline-none focus:border-blue-500 focus:ring-1 focus:ring-blue-500/50"
+            />
+
+            <div className="flex justify-end space-x-3 pt-2">
               <button
-                onClick={() => setShowPasteModal(false)}
+                onClick={() => setShowAppendModal(false)}
                 className="px-4 py-2 rounded-xl text-xs font-medium text-neutral-400 hover:bg-neutral-800 transition-all"
               >
                 Cancel
               </button>
               <button
-                onClick={() => runAnalysis()}
-                disabled={cooldown.isCooling('run_analysis')}
+                onClick={() => handleAppendToMarkdown(appendEntryFile, appendEntryText)}
+                disabled={!appendEntryText.trim()}
                 className="px-5 py-2.5 rounded-xl bg-white hover:bg-neutral-200 text-xs font-semibold text-black transition-all shadow-sm disabled:opacity-50"
               >
-                {cooldown.isCooling('run_analysis') ? (
-                  <CooldownButtonContent
-                    isCooling={true}
-                    remainingSeconds={cooldown.getRemainingSeconds('run_analysis')}
-                    idleText="Run Archaeology Analysis"
-                    coolingText="Analysis Cooldown"
-                  />
-                ) : (
-                  <span>Run Archaeology Analysis</span>
-                )}
+                Add to File Only
               </button>
             </div>
           </div>
@@ -1898,58 +2563,98 @@ export default function App() {
               </div>
             </div>
 
-            {/* Filter and Select All controls */}
-            <div className="flex items-center space-x-2 shrink-0">
-              <div className="relative flex-1">
-                <Search className="w-3.5 h-3.5 absolute left-3 top-2.5 text-neutral-500" />
-                <input
-                  type="text"
-                  placeholder="Filter loaded repositories..."
-                  value={repoSearchFilter}
-                  onChange={(e) => setRepoSearchFilter(e.target.value)}
-                  className="w-full bg-black text-xs font-mono pl-8 pr-3 py-1.5 rounded-xl border border-neutral-800 text-neutral-200 placeholder-neutral-600 focus:outline-none focus:border-blue-500"
-                />
+            {/* Filter and Select All controls with variable limits */}
+            <div className="space-y-2 shrink-0">
+              <div className="flex items-center space-x-2">
+                <div className="relative flex-1">
+                  <Search className="w-3.5 h-3.5 absolute left-3 top-2.5 text-neutral-500" />
+                  <input
+                    type="text"
+                    placeholder="Filter loaded repositories..."
+                    value={repoSearchFilter}
+                    onChange={(e) => setRepoSearchFilter(e.target.value)}
+                    className="w-full bg-black text-xs font-mono pl-8 pr-3 py-1.5 rounded-xl border border-neutral-800 text-neutral-200 placeholder-neutral-600 focus:outline-none focus:border-blue-500"
+                  />
+                </div>
+
+                <span className="text-[11px] font-mono text-neutral-400 shrink-0">
+                  {userRepos.filter(r => !repoSearchFilter || r.name.toLowerCase().includes(repoSearchFilter.toLowerCase()) || r.full_name.toLowerCase().includes(repoSearchFilter.toLowerCase())).length} repos
+                </span>
               </div>
 
-              {/* Select All Button */}
+              {/* Select All Limit Controls */}
               {userRepos.length > 0 && (
-                <button
-                  type="button"
-                  onClick={() => {
-                    const filtered = userRepos.filter(r => !repoSearchFilter || r.name.toLowerCase().includes(repoSearchFilter.toLowerCase()) || r.full_name.toLowerCase().includes(repoSearchFilter.toLowerCase()));
-                    const allFilteredNames = filtered.map(r => r.full_name);
-                    const isAllSelected = allFilteredNames.length > 0 && allFilteredNames.every(name => selectedRepoFullNames.includes(name));
-                    if (isAllSelected) {
-                      setSelectedRepoFullNames(prev => prev.filter(name => !allFilteredNames.includes(name)));
-                    } else {
-                      setSelectedRepoFullNames(prev => Array.from(new Set([...prev, ...allFilteredNames])));
-                    }
-                  }}
-                  className="px-3 py-1.5 bg-neutral-800 hover:bg-neutral-700 text-[11px] font-mono font-semibold text-neutral-200 rounded-xl transition-all border border-neutral-700 shrink-0 flex items-center gap-1.5"
-                >
-                  <CheckCircle2 className="w-3.5 h-3.5 text-blue-400" />
-                  <span>
-                    {(() => {
-                      const filtered = userRepos.filter(r => !repoSearchFilter || r.name.toLowerCase().includes(repoSearchFilter.toLowerCase()) || r.full_name.toLowerCase().includes(repoSearchFilter.toLowerCase()));
-                      const allFilteredNames = filtered.map(r => r.full_name);
-                      const isAllSelected = allFilteredNames.length > 0 && allFilteredNames.every(name => selectedRepoFullNames.includes(name));
-                      return isAllSelected ? 'Deselect All' : `Select All (${filtered.length})`;
-                    })()}
-                  </span>
-                </button>
-              )}
+                <div className="flex flex-wrap items-center justify-between gap-1.5 pt-1 border-t border-neutral-800/60">
+                  <div className="flex flex-wrap items-center gap-1.5">
+                    <span className="text-[11px] font-mono text-neutral-400 flex items-center gap-1 mr-1">
+                      <Sliders className="w-3 h-3 text-blue-400" />
+                      Select Limits:
+                    </span>
+                    <button
+                      type="button"
+                      onClick={() => {
+                        const filtered = userRepos.filter(r => !repoSearchFilter || r.name.toLowerCase().includes(repoSearchFilter.toLowerCase()) || r.full_name.toLowerCase().includes(repoSearchFilter.toLowerCase()));
+                        const allFilteredNames = filtered.map(r => r.full_name);
+                        const isAllSelected = allFilteredNames.length > 0 && allFilteredNames.every(name => selectedRepoFullNames.includes(name));
+                        if (isAllSelected) {
+                          setSelectedRepoFullNames(prev => prev.filter(name => !allFilteredNames.includes(name)));
+                        } else {
+                          setSelectedRepoFullNames(prev => Array.from(new Set([...prev, ...allFilteredNames])));
+                        }
+                      }}
+                      className="px-2.5 py-1 bg-neutral-800 hover:bg-neutral-700 text-[11px] font-mono font-semibold text-neutral-200 rounded-lg transition-all border border-neutral-700 shrink-0 flex items-center gap-1"
+                    >
+                      <CheckCircle2 className="w-3 h-3 text-blue-400" />
+                      <span>
+                        {(() => {
+                          const filtered = userRepos.filter(r => !repoSearchFilter || r.name.toLowerCase().includes(repoSearchFilter.toLowerCase()) || r.full_name.toLowerCase().includes(repoSearchFilter.toLowerCase()));
+                          const allFilteredNames = filtered.map(r => r.full_name);
+                          const isAllSelected = allFilteredNames.length > 0 && allFilteredNames.every(name => selectedRepoFullNames.includes(name));
+                          return isAllSelected ? 'Deselect All' : `Select All (${filtered.length})`;
+                        })()}
+                      </span>
+                    </button>
 
-              <span className="text-[11px] font-mono text-neutral-400 shrink-0">
-                {userRepos.filter(r => !repoSearchFilter || r.name.toLowerCase().includes(repoSearchFilter.toLowerCase()) || r.full_name.toLowerCase().includes(repoSearchFilter.toLowerCase())).length} repos
-              </span>
+                    {[5, 10, 25].map((limitCount) => (
+                      <button
+                        key={limitCount}
+                        type="button"
+                        onClick={() => {
+                          const filtered = userRepos.filter(r => !repoSearchFilter || r.name.toLowerCase().includes(repoSearchFilter.toLowerCase()) || r.full_name.toLowerCase().includes(repoSearchFilter.toLowerCase()));
+                          const topNames = filtered.slice(0, limitCount).map(r => r.full_name);
+                          setSelectedRepoFullNames(topNames);
+                        }}
+                        className="px-2 py-1 bg-neutral-900 hover:bg-neutral-800 text-neutral-300 text-[11px] font-mono rounded-lg border border-neutral-800 hover:border-neutral-700 transition-colors"
+                      >
+                        Top {limitCount}
+                      </button>
+                    ))}
+                  </div>
+
+                  {selectedRepoFullNames.length > 0 && (
+                    <button
+                      type="button"
+                      onClick={() => setSelectedRepoFullNames([])}
+                      className="text-[11px] font-mono text-neutral-400 hover:text-white px-2 py-1 rounded bg-neutral-800/80"
+                    >
+                      Clear ({selectedRepoFullNames.length})
+                    </button>
+                  )}
+                </div>
+              )}
             </div>
 
             {/* Batch Action Bar if any selected */}
             {selectedRepoFullNames.length > 0 && (
-              <div className="p-2.5 rounded-xl bg-blue-950/40 border border-blue-500/30 flex items-center justify-between shrink-0">
-                <span className="text-xs font-mono text-blue-300">
-                  {selectedRepoFullNames.length} {selectedRepoFullNames.length === 1 ? 'repository' : 'repositories'} selected
-                </span>
+              <div className="p-3 rounded-xl bg-blue-950/60 border border-blue-500/40 flex flex-col sm:flex-row items-center justify-between gap-2 shrink-0 animate-in fade-in">
+                <div className="flex items-center gap-2">
+                  <div className="w-5 h-5 rounded-md bg-blue-500/20 flex items-center justify-center text-blue-300">
+                    <CheckCheck className="w-3 h-3" />
+                  </div>
+                  <span className="text-xs font-mono text-blue-200 font-semibold">
+                    {selectedRepoFullNames.length} {selectedRepoFullNames.length === 1 ? 'repository' : 'repositories'} selected
+                  </span>
+                </div>
                 <div className="flex items-center space-x-2">
                   <button
                     type="button"
@@ -1961,14 +2666,27 @@ export default function App() {
                   <button
                     type="button"
                     onClick={() => {
-                      if (selectedRepoFullNames.length > 0) {
+                      if (selectedRepoFullNames.length === 1) {
                         fetchAndAnalyzeRepo(selectedRepoFullNames[0]);
+                      } else if (selectedRepoFullNames.length > 1) {
+                        fetchAndAnalyzeBatchRepos(selectedRepoFullNames);
                       }
                     }}
                     disabled={fetchingHistory || analyzing || cooldown.isCooling('fetch_repo')}
-                    className="px-3 py-1 bg-blue-600 hover:bg-blue-500 text-white text-xs font-semibold rounded-lg transition-all disabled:opacity-50"
+                    className="px-3.5 py-1.5 bg-blue-600 hover:bg-blue-500 text-white text-xs font-semibold rounded-lg transition-all shadow-sm disabled:opacity-50 flex items-center gap-1.5"
                   >
-                    {cooldown.isCooling('fetch_repo') ? `Cooldown (${cooldown.getRemainingSeconds('fetch_repo')}s)` : fetchingHistory ? 'Fetching...' : `Analyze Selected (${selectedRepoFullNames[0]})`}
+                    {cooldown.isCooling('fetch_repo') ? (
+                      `Cooldown (${cooldown.getRemainingSeconds('fetch_repo')}s)`
+                    ) : fetchingHistory ? (
+                      <>
+                        <RefreshCw className="w-3 h-3 animate-spin" />
+                        <span>Fetching & Aggregating...</span>
+                      </>
+                    ) : selectedRepoFullNames.length === 1 ? (
+                      `Analyze (${selectedRepoFullNames[0]})`
+                    ) : (
+                      `Batch Analyze All ${selectedRepoFullNames.length} Repos`
+                    )}
                   </button>
                 </div>
               </div>
```
