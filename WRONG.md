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

---

<!-- CAE Append Session: 2026-09-13T12:29:08.103Z -->

## 2021-03-11T15:29:15Z -- Remove no-op pylint disable comments. (`01ef9a62`)

**Reason:** Immediately followed by fix commit c9cdbb4b ("Fix sonnet/v2/src/nets/dnc:util_test_gpu test") touching overlapping files (sonnet.ts)
**Fixed by:** `c9cdbb4b` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove no-op pylint disable comments.
PiperOrigin-RevId: 362287115
Change-Id: Ib5efc634f54914055d10b914f7aac70d386edf7e
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove no-op pylint disable comments.]


```

---



## 2020-10-08T10:01:11Z -- Merge pull request #181 from tirkarthi:fix-collections (`5a465696`)

**Reason:** Immediately followed by fix commit eb06599a ("Fix the checkpoint_test after fix for bug b/168905859") touching overlapping files (sonnet.ts)
**Fixed by:** `eb06599a` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Merge pull request #181 from tirkarthi:fix-collections
PiperOrigin-RevId: 336048465
Change-Id: I93e579d4e9832aa9ca66a68fac04ea88183970ee
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Merge pull request 181 from tirkarthi:fix-collections]


```

---



## 2020-10-01T13:24:02Z -- Merge pull request #190 from deepmind:dependabot/pip/tensorflow-2.2.1 (`af462020`)

**Reason:** Immediately followed by fix commit 5a465696 ("Merge pull request #181 from tirkarthi:fix-collections") touching overlapping files (sonnet.ts)
**Fixed by:** `5a465696` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Merge pull request #190 from deepmind:dependabot/pip/tensorflow-2.2.1
PiperOrigin-RevId: 334799980
Change-Id: Ie453fa1ca5cf42d06c5d1bfdd952952cee518011
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Merge pull request 190 from deepmind:dependabot/pip/tensorflow-2.2.1]


```

---



## 2020-04-17T12:53:37Z -- Update distributed training notes. (`a06702c6`)

**Reason:** Immediately followed by fix commit 46333904 ("Merge pull request #168 from ialong:patch-1") touching overlapping files (sonnet.ts)
**Fixed by:** `46333904` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update distributed training notes.
PiperOrigin-RevId: 307028907
Change-Id: I5697668bf11f0037d0b580b79973f6904bad4222
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update distributed training notes.]


```

---



## 2020-04-03T18:07:54Z -- Remove `%tensorflow_version` and `from __future__` (`941f7b33`)

**Reason:** Immediately followed by fix commit 58d9a274 ("fix type of one hot indicators in vqvae.py") touching overlapping files (sonnet.ts)
**Fixed by:** `58d9a274` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove `%tensorflow_version` and `from __future__`
PiperOrigin-RevId: 304652978
Change-Id: I4e46902b49b71a98cd3ebc13fd5eb20e4c076dbf
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove tensorflow_version and from __future__]


```

---



## 2020-04-02T12:48:58Z -- Support shape changing inputs (`f28eb90f`)

**Reason:** Immediately followed by fix commit 3b8f9d4c ("Fix for change to conv_transpose") touching overlapping files (sonnet.ts)
**Fixed by:** `3b8f9d4c` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Support shape changing inputs
PiperOrigin-RevId: 304382117
Change-Id: I8b2f86adc492365c3327770aa69db01c62c1aebc
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Support shape changing inputs]


```

---



## 2020-03-09T14:43:46Z -- Merge pull request #164 from hanbyul-kim:hotfix/resolve_doc_warnings (`7a70496b`)

**Reason:** Self-identified failure / WIP in commit subject ("Merge pull request #164 from hanbyul-kim:hotfix/resolve_doc_warnings")

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Merge pull request #164 from hanbyul-kim:hotfix/resolve_doc_warnings
PiperOrigin-RevId: 299838560
Change-Id: I8698eed36a2b122fdfc11a83a92925da8ae5a2fb
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Merge pull request 164 from hanbyul-kim:hotfix/resolve_doc_warnings]


```

---



## 2020-02-27T14:12:47Z -- Internal change (`3debabb3`)

**Reason:** Immediately followed by fix commit 733d6bbb ("Fix typing of cifar10_convnet after previous change") touching overlapping files (sonnet.ts)
**Fixed by:** `733d6bbb` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal change
PiperOrigin-RevId: 297579572
Change-Id: Id57c06efd031bae6449ae02f4eb361e5ddacde38
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal change]


```

---



## 2020-02-26T13:35:21Z -- Split a line into two and remove indent (`8574df2f`)

**Reason:** Immediately followed by fix commit c8fbd100 ("Fix indentation error") touching overlapping files (sonnet.ts)
**Fixed by:** `c8fbd100` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Split a line into two and remove indent

```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Split a line into two and remove indent]


```

---



## 2020-02-26T12:32:43Z -- In preparation for cl/296449699, disabling some pytype checks that fail after (`d0cc5780`)

**Reason:** Immediately followed by fix commit 623a88d4 ("Fix code block to use ::") touching overlapping files (sonnet.ts)
**Fixed by:** `623a88d4` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
In preparation for cl/296449699, disabling some pytype checks that fail after
this CL is submitted.

The root cause for these failures is that currently tensorflow.compat.v1 maps
to Any and effectively there isn't any type checking. cl/296449699 enables
this type checking for tensorflow.compat.v1 and as a result exposes some bugs /
problems with the type checking code that got checked in previously.

PiperOrigin-RevId: 297331371
Change-Id: I7bb4ccb53ef6aebcae69e809ce245b1f02882ea9
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[In preparation for cl/296449699 disabling some pytype checks that fail after]


```

---



## 2020-02-25T14:46:58Z -- Fix broken code block (`2095e032`)

**Reason:** Self-identified failure / WIP in commit subject ("Fix broken code block")

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix broken code block

```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix broken code block]


```

---



## 2020-02-25T14:31:31Z -- Fix unexpected indent of docstring (`dab662d4`)

**Reason:** Immediately followed by fix commit 2095e032 ("Fix broken code block") touching overlapping files (sonnet.ts)
**Fixed by:** `2095e032` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix unexpected indent of docstring

```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix unexpected indent of docstring]


```

---



## 2020-02-24T13:30:51Z -- DepthwiseConv2D is now accessible via snt.DepthwiseConv2D (`3732f195`)

**Reason:** Immediately followed by fix commit dab662d4 ("Fix unexpected indent of docstring") touching overlapping files (sonnet.ts)
**Fixed by:** `dab662d4` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
DepthwiseConv2D is now accessible via snt.DepthwiseConv2D
PiperOrigin-RevId: 296870777
Change-Id: Ie1ac45cfbb8cee5b004ddffe34568044405245dc
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[DepthwiseConv2D is now accessible via snt.DepthwiseConv2D]


```

---



## 2020-02-20T19:37:05Z -- CPU count on macos (`4cbbe29c`)

**Reason:** Immediately followed by fix commit d4f38d4b ("Fixed a few formatting quirks in recurrent docs") touching overlapping files (sonnet.ts)
**Fixed by:** `d4f38d4b` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
CPU count on macos

```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[CPU count on macos]


```

---



## 2020-01-22T20:12:17Z -- Expose `snt.{merge,split}_leading_dims`. (`c616bbd1`)

**Reason:** Immediately followed by fix commit 82049c26 ("Fixed vqvae_example.ipynb to be compatible with tf v2") touching overlapping files (sonnet.ts)
**Fixed by:** `82049c26` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Expose `snt.{merge,split}_leading_dims`.
PiperOrigin-RevId: 291003968
Change-Id: I7678d4aa15bb08650203afe0e21da9141c96a574
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Expose snt.mergesplit_leading_dims.]


```

---



## 2020-01-15T14:40:14Z -- Don't run stateful mixed precision example in doctest. (`d8d16523`)

**Reason:** Immediately followed by fix commit 25f040a2 ("Fix bug in _rnn_step, by swapping the prev_state with state.") touching overlapping files (sonnet.ts)
**Fixed by:** `25f040a2` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Don't run stateful mixed precision example in doctest.
PiperOrigin-RevId: 289848542
Change-Id: I66743dd05dccdac8a8b063b73995601a6c430652
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Dont run stateful mixed precision example in doctest.]


```

---



## 2019-12-06T21:57:57Z -- Throw an explicit error if user call TPUStrategy experimental_run_v2 in eager mode with a python function. (`1928424e`)

**Reason:** Immediately followed by fix commit 84817e56 ("Fix or ignore type errors generated by the next release of pytype.") touching overlapping files (sonnet.ts)
**Fixed by:** `84817e56` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Throw an explicit error if user call TPUStrategy experimental_run_v2 in eager mode with a python function.
PiperOrigin-RevId: 284256438
Change-Id: Iad99f48dab9787d1a53fb3e4815a357dbf147cdc
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Throw an explicit error if user call TPUStrategy experimental_run_v2 in eager mode with a python function.]


```

---



## 2019-11-06T15:01:27Z -- Ensure all builtin modules create and use parameters in a consistent order. (`0fcfe870`)

**Reason:** Immediately followed by fix commit 50413fe1 ("Minor fixes.") touching overlapping files (sonnet.ts)
**Fixed by:** `50413fe1` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Ensure all builtin modules create and use parameters in a consistent order.
Currently our optimizers rely on parameters being presented in a consistent
order between calls to `opt.apply(..)`. This is because we use lists (because of
checkpointing) to store momentum terms in Adam/Momentum etc.

PiperOrigin-RevId: 278849811
Change-Id: I442681c9adcbcd08abce4a41fec3e1a8dd5a91a3
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Ensure all builtin modules create and use parameters in a consistent order.]


```

---



## 2019-11-03T22:18:49Z -- Move code for snt.distribute into src/distribute. (`43908bb2`)

**Reason:** Immediately followed by fix commit 605ca701 ("Fix bug in Conv*Transpose weight initialization") touching overlapping files (sonnet.ts)
**Fixed by:** `605ca701` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Move code for snt.distribute into src/distribute.
PiperOrigin-RevId: 278255130
Change-Id: Ieeb64c6089c0c8ffc62135f991d8418c10514f4c
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Move code for snt.distribute into src/distribute.]


```

---



## 2019-10-24T08:42:58Z -- Minor update to make nesterov formula more readable. (`a31c623c`)

**Reason:** Immediately followed by fix commit feb7f7d9 ("Increase tolerance to fix test flakiness.") touching overlapping files (sonnet.ts)
**Fixed by:** `feb7f7d9` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Minor update to make nesterov formula more readable.
PiperOrigin-RevId: 276441106
Change-Id: Icb3b7029341412b7cfbcfcc14dbbbb4be67adfd7
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Minor update to make nesterov formula more readable.]


```

---



## 2019-10-15T08:41:17Z -- Add test for nested mixed_precision scopes. (`0f14ee72`)

**Reason:** Immediately followed by fix commit 7fcf1888 ("Use sync-on-read variables for TPU strategy now these have been fixed") touching overlapping files (sonnet.ts)
**Fixed by:** `7fcf1888` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add test for nested mixed_precision scopes.
PiperOrigin-RevId: 274754476
Change-Id: I0f34464666a4dea7ee2e5757a8d9452f0e575a1b
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add test for nested mixed_precision scopes.]


```

---



## 2019-10-11T13:18:32Z -- Add option to set channels_per_group_list on ResNet. (`4cdd392a`)

**Reason:** Immediately followed by fix commit 3d0fffe2 ("Fixed cases where tf.TensorShape was constructed with float dimensions") touching overlapping files (sonnet.ts)
**Fixed by:** `3d0fffe2` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add option to set channels_per_group_list on ResNet.
PiperOrigin-RevId: 274159409
Change-Id: If31b33c236c5153aa1ad65b87293dfbca44674a7
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add option to set channels_per_group_list on ResNet.]


```

---



## 2019-10-09T09:10:45Z -- Run ResNet tests with v1 and v2. (`adae0b2e`)

**Reason:** Immediately followed by fix commit 275af721 ("Ensure all test in Sonnet are run when test.sh is called and fixes to enable this") touching overlapping files (sonnet.ts)
**Fixed by:** `275af721` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Run ResNet tests with v1 and v2.
PiperOrigin-RevId: 273703386
Change-Id: I0338b99808988d4c0b4a136e263c0eb3b6f36668
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Run ResNet tests with v1 and v2.]


```

---



## 2019-10-01T13:34:14Z -- snt.dynamic_unroll now accepts inputs with dynamic number of steps (`e0b3e3b8`)

**Reason:** Immediately followed by fix commit 220c87d7 ("Fix ResNet50 docstring.") touching overlapping files (sonnet.ts)
**Fixed by:** `220c87d7` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
snt.dynamic_unroll now accepts inputs with dynamic number of steps
PiperOrigin-RevId: 272196444
Change-Id: Ia2efe53de5ba96fdee0ed85e49c659f7cce6b056
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[snt.dynamic_unroll now accepts inputs with dynamic number of steps]


```

---



## 2019-09-19T15:45:59Z -- Add more type annotations throughout. (`e097a667`)

**Reason:** Immediately followed by fix commit 0ef0a79e ("Fix reset of vector valued moving_average.") touching overlapping files (sonnet.ts)
**Fixed by:** `0ef0a79e` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add more type annotations throughout.
PiperOrigin-RevId: 270049002
Change-Id: Icd469b45b7d8cac855cbe125e31811be64850968
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add more type annotations throughout.]


```

---



## 2019-09-12T19:52:44Z -- Fixed colab link on Readme (`54c67eee`)

**Reason:** Immediately followed by fix commit 6d8200f2 ("Fixed typo") touching overlapping files (sonnet.ts)
**Fixed by:** `6d8200f2` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fixed colab link on Readme

```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fixed colab link on Readme]


```

---



## 2019-09-12T14:52:12Z -- Fold py3_tags into tags and remove python_version. (`32e333f7`)

**Reason:** Immediately followed by fix commit 54c67eee ("Fixed colab link on Readme") touching overlapping files (sonnet.ts)
**Fixed by:** `54c67eee` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fold py3_tags into tags and remove python_version.
PiperOrigin-RevId: 268684200
Change-Id: Iec17440ecc68a14124b950f27b9c7d46724f36e8
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fold py3_tags into tags and remove python_version.]


```

---



## 2019-09-11T15:30:45Z -- Put example training scripts and colab notebooks in a single folder. (`2ccd6a13`)

**Reason:** Immediately followed by fix commit b6b3af10 ("Fixed python version on mlp notebook") touching overlapping files (sonnet.ts)
**Fixed by:** `b6b3af10` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Put example training scripts and colab notebooks in a single folder.
PiperOrigin-RevId: 268463868
Change-Id: I5131866a651515bf4b7b777adc442dff80c09750
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Put example training scripts and colab notebooks in a single folder.]


```

---



## 2019-07-31T15:54:50Z -- Export UnrolledRNN and UnrolledLSTM (`20c31bc0`)

**Reason:** Immediately followed by fix commit c6895efb ("Fixed a bug in UnrollTest.testVariableLengthRange") touching overlapping files (sonnet.ts)
**Fixed by:** `c6895efb` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Export UnrolledRNN and UnrolledLSTM
PiperOrigin-RevId: 260935285
Change-Id: Ib811f7b213c7ac560f0b9e70a3f685f98df4e7bd
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Export UnrolledRNN and UnrolledLSTM]


```

---



## 2019-07-22T12:10:52Z -- Add an Optimizer base class. (`53e84bfd`)

**Reason:** Immediately followed by fix commit fa5e406e ("Small fix to docstring.") touching overlapping files (sonnet.ts)
**Fixed by:** `fa5e406e` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add an Optimizer base class.
PiperOrigin-RevId: 259305455
Change-Id: I397034afa3cb0adc540ec1a527c0c0809f95743e
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add an Optimizer base class.]


```

---



## 2019-07-15T14:51:48Z -- Move test parameterization to class level. (`fd31df39`)

**Reason:** Immediately followed by fix commit da28ab54 ("Fix `BatchNorm` constructor.") touching overlapping files (sonnet.ts)
**Fixed by:** `da28ab54` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Move test parameterization to class level.
PiperOrigin-RevId: 258161214
Change-Id: I17ef86e3eb2a1fd3a0005eef61e385669c56ee06
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Move test parameterization to class level.]


```

---



## 2019-07-10T16:11:01Z -- Remove AxisNorm from the API replacing with LayerNorm which now has the equivalent API taking no arguments as the axis to normalize aren't clear so we want to avoid mistakes. (`401eaf09`)

**Reason:** Immediately followed by fix commit 9cdfc977 ("Fix instance norm to normalize over the spatial dimensions only instead of channels") touching overlapping files (sonnet.ts)
**Fixed by:** `9cdfc977` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove AxisNorm from the API replacing with LayerNorm which now has the equivalent API taking no arguments as the axis to normalize aren't clear so we want to avoid mistakes.
The docstring provides some details for the correct arguments to use to get the most common behavior.

Also change eps to 1e-5

PiperOrigin-RevId: 257416418
Change-Id: Ief671b9403813f109344d8d48d5e58e0c6c2084e
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove AxisNorm from the API replacing with LayerNorm which now has the equivalent API taking no arguments as the axis to normalize arent clear so we want to avoid mistakes.]


```

---



## 2019-07-05T10:34:18Z -- Remove TODOs that are done (thanks @malcolmreynolds)! (`afeff5eb`)

**Reason:** Immediately followed by fix commit da4f2a9e ("Fix Momentum when using tf.function.") touching overlapping files (sonnet.ts)
**Fixed by:** `da4f2a9e` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove TODOs that are done (thanks @malcolmreynolds)!
PiperOrigin-RevId: 256649110
Change-Id: I6233b76885364e10eb765594605a9a7a0183b265
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove TODOs that are done thanks malcolmreynolds]


```

---



## 2019-07-04T17:23:15Z -- Expose VQ VAE modules to as snt.nets.VectorQuantizer{EMA} (`9578680a`)

**Reason:** Immediately followed by fix commit 3fd97a0b ("Fix SGD when using tf.function.") touching overlapping files (sonnet.ts)
**Fixed by:** `3fd97a0b` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Expose VQ VAE modules to as snt.nets.VectorQuantizer{EMA}
Added goldens.

Also fixed checkpoint_test & saved_model_test to support nested output.

PiperOrigin-RevId: 256566131
Change-Id: Ic26fe1785741ebac9b4a6a790c3585a64fd2f882
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Expose VQ VAE modules to as snt.nets.VectorQuantizerEMA]


```

---



## 2019-07-04T12:11:50Z -- Fix broken centered mode in RMSProp and add tests. (`6df9ee23`)

**Reason:** Self-identified failure / WIP in commit subject ("Fix broken centered mode in RMSProp and add tests.")

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix broken centered mode in RMSProp and add tests.
PiperOrigin-RevId: 256535597
Change-Id: I00f58289a75683b3eb83159546e86f070bab3dea
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix broken centered mode in RMSProp and add tests.]


```

---



## 2019-07-04T11:42:20Z -- Moved VQ-VAE to Sonnet 2. (`67f03692`)

**Reason:** Immediately followed by fix commit 6df9ee23 ("Fix broken centered mode in RMSProp and add tests.") touching overlapping files (sonnet.ts)
**Fixed by:** `6df9ee23` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Moved VQ-VAE to Sonnet 2.
Implements model from https://arxiv.org/abs/1711.00937 - original TF 1 code written Aaron van den Oord.

PiperOrigin-RevId: 256532556
Change-Id: Icea20719b2dab8d1f85b3b1dfe4d5657a89ab9ba
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Moved VQ-VAE to Sonnet 2.]


```

---



## 2019-06-27T10:08:51Z -- Allow TpuReplicator in Sonnet optimizers. (`436dc684`)

**Reason:** Immediately followed by fix commit dbd585cc ("Tiny fix to MNIST example.") touching overlapping files (sonnet.ts)
**Fixed by:** `dbd585cc` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Allow TpuReplicator in Sonnet optimizers.
PiperOrigin-RevId: 255365125
Change-Id: I403c8018cc301d2273538ab9d035357f38ae0bb7
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Allow TpuReplicator in Sonnet optimizers.]


```

---



## 2019-06-24T09:10:08Z -- Docstring changes for convolutional layers (`3d2fa231`)

**Reason:** Immediately followed by fix commit 578b65bc ("Docstring fixes for GroupNorm") touching overlapping files (sonnet.ts)
**Fixed by:** `578b65bc` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Docstring changes for convolutional layers
PiperOrigin-RevId: 254715194
Change-Id: I68e08397b07f15f8fccf12f94c1c34b2a208bdd1
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Docstring changes for convolutional layers]


```

---



## 2019-06-20T08:52:58Z -- Pin estimator version since dev2019062000 is broken. (`cc392e07`)

**Reason:** Self-identified failure / WIP in commit subject ("Pin estimator version since dev2019062000 is broken.")

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Pin estimator version since dev2019062000 is broken.
tf-nightly-2.0-preview does not pin a release of estimator and the release at
head is broken. Sonnet does not import or use estimator, however when using
tf.function autograph forces the estimator module to be loaded (as part of
`_fix_linecache_record`) which triggers an issue with loading:

ModuleNotFoundError: No module named 'tensorflow_core.estimator'
PiperOrigin-RevId: 254155508
Change-Id: I42f92d702eb32c406acc7a849b59916172becc87
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Pin estimator version since dev2019062000 is broken.]


```

---



## 2019-06-17T12:02:03Z -- Remove workaround for broken ReplicaLocalVariable cross-replica read_value. (`1f3f060c`)

**Reason:** Self-identified failure / WIP in commit subject ("Remove workaround for broken ReplicaLocalVariable cross-replica read_value.")

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove workaround for broken ReplicaLocalVariable cross-replica read_value.
PiperOrigin-RevId: 253558060
Change-Id: Ia9a37ef474a7a59dc025d7b5a8fb45b78187a648
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove workaround for broken ReplicaLocalVariable cross-replica read_value.]


```

---



## 2019-06-14T11:44:15Z -- Remove workaround for broken ReplicaLocalVariable cross-replica assign_add/assign_sub. (`ab162f99`)

**Reason:** Self-identified failure / WIP in commit subject ("Remove workaround for broken ReplicaLocalVariable cross-replica assign_add/assign_sub.")

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove workaround for broken ReplicaLocalVariable cross-replica assign_add/assign_sub.
PiperOrigin-RevId: 253206984
Change-Id: Idfd09acd52f85afff90e3421bc1f9edfa76d5184
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove workaround for broken ReplicaLocalVariable cross-replica assign_add/assign_sub.]


```

---



## 2019-06-04T18:27:36Z -- Switched to sphinxcontrib-bibtex for references (`279a03e4`)

**Reason:** Immediately followed by fix commit d78d5dd3 ("Create trainable ReplicaLocalVariable by default even after upcoming TF bugfix.") touching overlapping files (sonnet.ts)
**Fixed by:** `d78d5dd3` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Switched to sphinxcontrib-bibtex for references
No more copy-pasted citations, yay!

PiperOrigin-RevId: 251479873
Change-Id: I8d7e9decda1bca053247e5d7c8347bdad11cd18e
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Switched to sphinxcontrib-bibtex for references]


```

---



## 2019-05-30T10:25:46Z -- Added an explicit master_doc (`7d43294f`)

**Reason:** Immediately followed by fix commit 74db9bd4 ("Fix docstring typo.") touching overlapping files (sonnet.ts)
**Fixed by:** `74db9bd4` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Added an explicit master_doc
Sphinx 2.0 defaults it to 'contents' whereas we use 'index'.

PiperOrigin-RevId: 250659221
Change-Id: I3adc694fb83da073247976cb319f58639e64cc02
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Added an explicit master_doc]


```

---



## 2019-05-23T17:02:53Z -- Use snt_py_library BUILD rule. (`480682a1`)

**Reason:** Immediately followed by fix commit 81427016 ("Copy over docs from ConvND{,Transpose} to fixed spatial dims versions.") touching overlapping files (sonnet.ts)
**Fixed by:** `81427016` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Use snt_py_library BUILD rule.
PiperOrigin-RevId: 249664278
Change-Id: I5e1f52b76a10a80ef455d86fbea51a837da7a51d
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Use snt_py_library BUILD rule.]


```

---



## 2019-05-23T09:25:53Z -- Add contributing read me. (`d97e4568`)

**Reason:** Immediately followed by fix commit 25b98bcc ("Fix to ensure that the correct axis is being used in Batch Norm") touching overlapping files (sonnet.ts)
**Fixed by:** `25b98bcc` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add contributing read me.
PiperOrigin-RevId: 249606664
Change-Id: I90b46e1907e66cde4f49eb6573beaef06c6539e9
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add contributing read me.]


```

---



## 2019-05-21T09:53:42Z -- [Automated] Update package documentation. (`17517d51`)

**Reason:** Immediately followed by fix commit 5f7bca5c ("Merge pull request #130 from deepmind:tomhennigan-patch-1") touching overlapping files (sonnet.ts)
**Fixed by:** `5f7bca5c` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
[Automated] Update package documentation.
PiperOrigin-RevId: 249216394
Change-Id: Ifcbc9a2a0d031cc7a2baf930a40825af5d285efe
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[[Automated] Update package documentation.]


```

---



## 2019-05-20T14:55:38Z -- Removed a new line in test.sh (`071ec1e1`)

**Reason:** Immediately followed by fix commit a29e5801 ("Merge pull request #129 from superbobry:patch-1") touching overlapping files (sonnet.ts)
**Fixed by:** `a29e5801` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Removed a new line in test.sh

```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Removed a new line in test.sh]


```

---



## 2019-05-17T10:38:13Z -- Pin TensorFlow nightly version to make integration tests hermetic. (`32569f20`)

**Reason:** Immediately followed by fix commit c220de5c ("Indent documentation to fix formatting in rendered output.") touching overlapping files (sonnet.ts)
**Fixed by:** `c220de5c` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Pin TensorFlow nightly version to make integration tests hermetic.
We'll bump this ~weekly to keep track of TF2 progress.

PiperOrigin-RevId: 248693469
Change-Id: Id2861f9f4df8a27c1784c26c0769ebdb61d46922
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Pin TensorFlow nightly version to make integration tests hermetic.]


```

---



## 2019-05-17T10:22:47Z -- Revert to legacy tf.where for now. (`294f9fef`)

**Reason:** Self-identified failure / WIP in commit subject ("Revert to legacy tf.where for now.")

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Revert to legacy tf.where for now.
PiperOrigin-RevId: 248692249
Change-Id: Ie38dc745556382af4e5570dcb112815d40172942
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Revert to legacy tf.where for now.]


```

---



## 2019-05-13T11:02:54Z -- Run test.sh in a virtualenv and set minimum versions for requirements. (`ec3afffc`)

**Reason:** Immediately followed by fix commit ac19d93b ("Fix build rules") touching overlapping files (sonnet.ts)
**Fixed by:** `ac19d93b` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Run test.sh in a virtualenv and set minimum versions for requirements.
PiperOrigin-RevId: 247909259
Change-Id: I21a43bb5808568c3caa03fdd5f2ef0d97c898d84
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Run test.sh in a virtualenv and set minimum versions for requirements.]


```

---



## 2019-04-01T16:50:48Z -- Add deprecation warning for broken layer norm in ConvLSTM (`2380d9cf`)

**Reason:** Self-identified failure / WIP in commit subject ("Add deprecation warning for broken layer norm in ConvLSTM")

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add deprecation warning for broken layer norm in ConvLSTM
Currently it has a reshape before normalizing so scale and offset are created for product(spatial_dims) * channels instead of just channels

PiperOrigin-RevId: 241343128
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add deprecation warning for broken layer norm in ConvLSTM]


```

---



## 2019-01-24T11:12:46Z -- Remove Pandoc and Sphinx configuration files. (`11e00f79`)

**Reason:** Immediately followed by fix commit e478c60a ("Fix headers in installation instructions.") touching overlapping files (sonnet.ts)
**Fixed by:** `e478c60a` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove Pandoc and Sphinx configuration files.
PiperOrigin-RevId: 230690314
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove Pandoc and Sphinx configuration files.]


```

---



## 2018-12-18T12:28:16Z -- Allow for kwargs to be forwarded to sub-modules in DeepRNN. (`59956470`)

**Reason:** Immediately followed by fix commit 39e817be ("FIX: setup.py.tmpl referenced wrong package name") touching overlapping files (sonnet.ts)
**Fixed by:** `39e817be` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Allow for kwargs to be forwarded to sub-modules in DeepRNN.
Useful for propagating 'is_training'.

PiperOrigin-RevId: 225976178
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Allow for kwargs to be forwarded to sub-modules in DeepRNN.]


```

---



## 2018-11-30T16:53:21Z -- Support non-2D recurrent state in pondering RNN. (Still relies on leading dimension being the batch dimension.) (`a64805b7`)

**Reason:** Immediately followed by fix commit 0c8a946c ("1. Fix the sequence reading order for backward unroll. The order should be") touching overlapping files (sonnet.ts)
**Fixed by:** `0c8a946c` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Support non-2D recurrent state in pondering RNN. (Still relies on leading dimension being the batch dimension.)
PiperOrigin-RevId: 223522357
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Support non-2D recurrent state in pondering RNN. Still relies on leading dimension being the batch dimension.]


```

---



## 2018-11-12T14:02:43Z -- Support FULL, CAUSAL and REVERSE_CAUSAL padding options for _ConvND and its subclasses. Also support using different padding settings for {depth,} height and width dimensions in Conv{2,3}D. (`4e6f863a`)

**Reason:** Immediately followed by fix commit f0ae8ac5 ("Backwards-compatibility fix for _ConvND.padding, following cl/221079656 which introduced different padding types per dimension.") touching overlapping files (sonnet.ts)
**Fixed by:** `f0ae8ac5` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Support FULL, CAUSAL and REVERSE_CAUSAL padding options for _ConvND and its subclasses. Also support using different padding settings for {depth,} height and width dimensions in Conv{2,3}D.
This is implemented using a tf.pad where necessary before the convolution op.

Deprecates snt.CausalConv1D which is now achievable via Conv1D(..., padding=CAUSAL).

PiperOrigin-RevId: 221079656
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Support FULL CAUSAL and REVERSE_CAUSAL padding options for _ConvND and its subclasses. Also support using different padding settings for depth height and width dimensions in Conv23D.]


```

---



## 2018-10-30T11:03:24Z -- Internal change. (`55c72143`)

**Reason:** Immediately followed by fix commit 729aedf0 ("A test erroneously suggested use_batch_norm accepts iterables of booleans. Fix") touching overlapping files (sonnet.ts)
**Fixed by:** `729aedf0` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal change.
PiperOrigin-RevId: 219275912
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal change.]


```

---



## 2018-09-07T14:31:24Z -- Increase size of convnet_test and dilation_test from small to medium. (`f4da53a1`)

**Reason:** Immediately followed by fix commit e8bd52b7 ("Fix for n-th farthest task RMC example. Corrects index reference to object.") touching overlapping files (sonnet.ts)
**Fixed by:** `e8bd52b7` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Increase size of convnet_test and dilation_test from small to medium.
PiperOrigin-RevId: 211973944
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Increase size of convnet_test and dilation_test from small to medium.]


```

---



## 2018-08-31T10:19:14Z -- Removes the reference to snt.SkipConnectionCore. (`0bdd9c3b`)

**Reason:** Immediately followed by fix commit 6680867b ("Fix docstring") touching overlapping files (sonnet.ts)
**Fixed by:** `6680867b` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Removes the reference to snt.SkipConnectionCore.
PiperOrigin-RevId: 211061744
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Removes the reference to snt.SkipConnectionCore.]


```

---



## 2018-08-24T11:46:14Z -- Sonnet embed: print warning about using default initializer. (`c776d06d`)

**Reason:** Immediately followed by fix commit 535ccdb2 ("Fix docstring typo") touching overlapping files (sonnet.ts)
**Fixed by:** `535ccdb2` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet embed: print warning about using default initializer.
Eventually we will switch to using a default initializer of stddev=1.

PiperOrigin-RevId: 210082974
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet embed: print warning about using default initializer.]


```

---



## 2018-06-07T15:00:26Z -- Make dataset_nth_farthest python3 compatible. (`1eac5c69`)

**Reason:** Immediately followed by fix commit ef9146d5 ("Fix dependencies in examples/BUILD - SciPy was missing.") touching overlapping files (sonnet.ts)
**Fixed by:** `ef9146d5` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make dataset_nth_farthest python3 compatible.
PiperOrigin-RevId: 199635116
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make dataset_nth_farthest python3 compatible.]


```

---



## 2018-06-07T12:52:07Z -- Adds demo for Relational Memory Core for "nth farthest" task from paper. (`9fe0a35d`)

**Reason:** Immediately followed by fix commit 220832e1 ("Fix rmc_nth_farthest.ipynb") touching overlapping files (sonnet.ts)
**Fixed by:** `220832e1` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Adds demo for Relational Memory Core for "nth farthest" task from paper.
PiperOrigin-RevId: 199622123
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Adds demo for Relational Memory Core for nth farthest task from paper.]


```

---



## 2018-06-04T13:34:19Z -- Make relational_memory python3 compatible. (`3dd3d330`)

**Reason:** Immediately followed by fix commit 05e21095 ("Fix error message in DeepRNN.") touching overlapping files (sonnet.ts)
**Fixed by:** `05e21095` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make relational_memory python3 compatible.
PiperOrigin-RevId: 199124668
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make relational_memory python3 compatible.]


```

---



## 2018-03-08T17:15:11Z -- Support the root scope (an empty string) in get_variable_scope. (`e3311726`)

**Reason:** Immediately followed by fix commit 80d47843 ("Fix typo in DeepRNN docs.") touching overlapping files (sonnet.ts)
**Fixed by:** `80d47843` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Support the root scope (an empty string) in get_variable_scope.
PiperOrigin-RevId: 188341067
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Support the root scope an empty string in get_variable_scope.]


```

---



## 2018-02-13T10:04:36Z -- Fix comment. (`c75bca45`)

**Reason:** Immediately followed by fix commit f9e38596 ("Fix an error message which was incorrect.") touching overlapping files (sonnet.ts)
**Fixed by:** `f9e38596` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix comment.
PiperOrigin-RevId: 185501790
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix comment.]


```

---



## 2018-02-12T17:36:54Z -- Internal change. (`07374827`)

**Reason:** Immediately followed by fix commit c75bca45 ("Fix comment.") touching overlapping files (sonnet.ts)
**Fixed by:** `c75bca45` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Internal change.
PiperOrigin-RevId: 185389700
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Internal change.]


```

---



## 2018-01-29T17:58:15Z -- Update changelog (`b23489a2`)

**Reason:** Immediately followed by fix commit 551d6528 ("Fix Python3 incompatibilities in new tests and methods.") touching overlapping files (sonnet.ts)
**Fixed by:** `551d6528` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update changelog
PiperOrigin-RevId: 183680938
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update changelog]


```

---



## 2018-01-25T17:25:35Z -- Refactor out bias construction and application. (`132924d0`)

**Reason:** Immediately followed by fix commit 1d2d99e5 ("Remove fixed seed dependency in gated_rnn_test.LSTMTest.testRecurrentDropout") touching overlapping files (sonnet.ts)
**Fixed by:** `1d2d99e5` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Refactor out bias construction and application.
Refactor out the construction of the bias variable and its application to the output of all Convolution modules.

PiperOrigin-RevId: 183249262
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Refactor out bias construction and application.]


```

---



## 2018-01-22T14:09:01Z -- 1. Start supporting TensorShapes for the output_shape input for Conv*DTranspose classes. (`de769df6`)

**Reason:** Immediately followed by fix commit 2a8e6e9f ("Fix typo.") touching overlapping files (sonnet.ts)
**Fixed by:** `2a8e6e9f` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
1. Start supporting TensorShapes for the output_shape input for Conv*DTranspose classes.
2. Support the use of tf.Dimension objects instead of integers for dimensions.
3. Add tests to make sure output_shape inference works correctly.

PiperOrigin-RevId: 182768071
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[1. Start supporting TensorShapes for the output_shape input for ConvDTranspose classes.]


```

---



## 2018-01-10T15:51:51Z -- Sonnet version update produced on Monday, 8. January 2018 (`1d2171d5`)

**Reason:** Immediately followed by fix commit 3a21b8d0 ("Remove dependency on fixed seed for testZoneout.") touching overlapping files (sonnet.ts)
**Fixed by:** `3a21b8d0` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet version update produced on Monday, 8. January 2018
PiperOrigin-RevId: 181464426
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet version update produced on Monday 8. January 2018]


```

---



## 2017-11-02T11:03:50Z -- Sonnet initial implementation for eager mode. (`e1ef6d50`)

**Reason:** Reverted by commit dcf8c90c ("Revert initial implementation for eager mode.")
**Fixed by:** `dcf8c90c` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet initial implementation for eager mode.
PiperOrigin-RevId: 174308021
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet initial implementation for eager mode.]


```

---



## 2017-10-09T21:23:31Z -- Make `base_info._is_iterable` safer by only returning True for supported types: list, tuple and dict. (`316e453e`)

**Reason:** Immediately followed by fix commit d80a554e ("Fixed typo in doc string.") touching overlapping files (sonnet.ts)
**Fixed by:** `d80a554e` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make `base_info._is_iterable` safer by only returning True for supported types: list, tuple and dict.
PiperOrigin-RevId: 171586466
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make base_info._is_iterable safer by only returning True for supported types: list tuple and dict.]


```

---



## 2017-10-04T10:46:41Z -- Added Sonnet `ModuleInfo` to the "sonnet" graph collection. This allows to keep track of which modules generated which connected sub-graphs. This information is serialised and available when loading a meta-graph-def. This can be used, for instance, to visualise the TensorFlow graph from a Sonnet perspective. (`a4167044`)

**Reason:** Immediately followed by fix commit 84f4c4e7 ("Decorating the _build function with memoize is breaking the connected_subgraph code. This CL fixes this issue. Note sure if this use-case is valid in the first place but let's have this discussion later!") touching overlapping files (sonnet.ts)
**Fixed by:** `84f4c4e7` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Added Sonnet `ModuleInfo` to the "sonnet" graph collection. This allows to keep track of which modules generated which connected sub-graphs. This information is serialised and available when loading a meta-graph-def. This can be used, for instance, to visualise the TensorFlow graph from a Sonnet perspective.
PiperOrigin-RevId: 170989708
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Added Sonnet ModuleInfo to the sonnet graph collection. This allows to keep track of which modules generated which connected sub-graphs. This information is serialised and available when loading a meta-graph-def. This can be used for instance to visualise the TensorFlow graph from a Sonnet perspective.]


```

---



## 2017-09-27T09:00:49Z -- Add docfix to open source doc. (`b323508e`)

**Reason:** Immediately followed by fix commit 9743f766 ("Fix stride property on Conv2D for NCHW inputs.") touching overlapping files (sonnet.ts)
**Fixed by:** `9743f766` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Add docfix to open source doc.
PiperOrigin-RevId: 170170288
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Add docfix to open source doc.]


```

---



## 2017-08-21T13:47:19Z -- Bump version to 1.11 (`88a201c2`)

**Reason:** Immediately followed by fix commit 72a5676e ("Fix typo in setup.py.tmpl") touching overlapping files (sonnet.ts)
**Fixed by:** `72a5676e` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Bump version to 1.11
PiperOrigin-RevId: 165922088
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Bump version to 1.11]


```

---



## 2017-08-09T17:42:05Z -- Remove config dict/Mapping checks to allow custom dict types. (`e945ecbf`)

**Reason:** Immediately followed by fix commit b7ded90b ("Fixes bias compatibility between NHWC and NCHW data formats in Conv2D.") touching overlapping files (sonnet.ts)
**Fixed by:** `b7ded90b` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Remove config dict/Mapping checks to allow custom dict types.
PiperOrigin-RevId: 164743160
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Remove config dict/Mapping checks to allow custom dict types.]


```

---



## 2017-07-21T16:13:14Z -- Fixes style of tensor shapes in docstrings. (`1c96671a`)

**Reason:** Immediately followed by fix commit 75b4969f ("Fix python3 installation - merge of GH PR #54") touching overlapping files (sonnet.ts)
**Fixed by:** `75b4969f` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fixes style of tensor shapes in docstrings.
PiperOrigin-RevId: 162747641
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fixes style of tensor shapes in docstrings.]


```

---



## 2017-07-21T16:02:57Z -- Update discussion around tf.nn.rnn_cell.RNNCell in Sonnet docs. (`e20385a5`)

**Reason:** Immediately followed by fix commit 1c96671a ("Fixes style of tensor shapes in docstrings.") touching overlapping files (sonnet.ts)
**Fixed by:** `1c96671a` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update discussion around tf.nn.rnn_cell.RNNCell in Sonnet docs.
PiperOrigin-RevId: 162746641
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update discussion around tf.nn.rnn_cell.RNNCell in Sonnet docs.]


```

---



## 2017-07-04T20:56:57Z -- Mac-compatible fix for install.sh (`34bb8891`)

**Reason:** Immediately followed by fix commit 5e582253 ("fix package name in docstrings.") touching overlapping files (sonnet.ts)
**Fixed by:** `5e582253` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Mac-compatible fix for install.sh
PiperOrigin-RevId: 160909229
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Mac-compatible fix for install.sh]


```

---



## 2017-07-04T16:25:50Z -- install.sh now supports relative paths as well as absolute. (`043774c6`)

**Reason:** Immediately followed by fix commit 34bb8891 ("Mac-compatible fix for install.sh") touching overlapping files (sonnet.ts)
**Fixed by:** `34bb8891` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
install.sh now supports relative paths as well as absolute.
Fixes github issue 48. Thanks @githuboml for the suggestion.

PiperOrigin-RevId: 160899365
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[install.sh now supports relative paths as well as absolute.]


```

---



## 2017-07-03T17:10:21Z -- Update changelog for 1.4 (`6a8525b0`)

**Reason:** Immediately followed by fix commit 2bd0193f ("Fixes default name of CausalConv1D.") touching overlapping files (sonnet.ts)
**Fixed by:** `2bd0193f` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update changelog for 1.4
PiperOrigin-RevId: 160837270
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update changelog for 1.4]


```

---



## 2017-06-19T17:13:23Z -- Update changelog, bump to version 1.2 (`d61b885f`)

**Reason:** Immediately followed by fix commit ffa0e421 ("Fix CUDA dependencies of bazel GPU build.") touching overlapping files (sonnet.ts)
**Fixed by:** `ffa0e421` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update changelog, bump to version 1.2
PiperOrigin-RevId: 159441579
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update changelog bump to version 1.2]


```

---



## 2017-06-19T15:36:20Z -- Fix CUDA dependencies of bazel GPU build. (`5cd82433`)

**Reason:** Immediately followed by fix commit 058eb113 ("Fix bad docstring in BatchFlatten. Add test to assert behavior is as described.") touching overlapping files (sonnet.ts)
**Fixed by:** `058eb113` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix CUDA dependencies of bazel GPU build.
PiperOrigin-RevId: 159430170
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix CUDA dependencies of bazel GPU build.]


```

---



## 2017-06-19T13:36:15Z -- Deprecates the use_batch_norm_* options to LSTM. Please switch to BatchNormLSTM if you require these options, in a future version they will no longer be supported by LSTM. (`93d54718`)

**Reason:** Immediately followed by fix commit 5cd82433 ("Fix CUDA dependencies of bazel GPU build.") touching overlapping files (sonnet.ts)
**Fixed by:** `5cd82433` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Deprecates the use_batch_norm_* options to LSTM. Please switch to BatchNormLSTM if you require these options, in a future version they will no longer be supported by LSTM.
PiperOrigin-RevId: 159420083
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Deprecates the use_batch_norm_ options to LSTM. Please switch to BatchNormLSTM if you require these options in a future version they will no longer be supported by LSTM.]


```

---



## 2017-06-12T18:16:23Z -- reinforce example (`a4a2fd60`)

**Reason:** Immediately followed by fix commit d886aaf1 ("Fix typos and formatting in changelog") touching overlapping files (sonnet.ts)
**Fixed by:** `d886aaf1` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
reinforce example
GitOrigin-RevId=41b62948a57b6c67f499f7743d7cef367fb53013
PiperOrigin-RevId: 158737717
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[reinforce example]


```

---



## 2017-06-05T16:01:50Z -- Raise an error if __call__ is called before __init__. (`b5a480c8`)

**Reason:** Immediately followed by fix commit 4c6bdcfc ("Use tolerance to fix initializers_test.") touching overlapping files (sonnet.ts)
**Fixed by:** `4c6bdcfc` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Raise an error if __call__ is called before __init__.
Currently if this is attempted, an AttributeError is raised. This change provides a more explicit (and more helpful) error message.

PiperOrigin-RevId: 158019730
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Raise an error if __call__ is called before __init__.]


```

---



## 2017-06-01T11:10:51Z -- Fix GPU build failure. (`85a58de0`)

**Reason:** Self-identified failure / WIP in commit subject ("Fix GPU build failure.")

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fix GPU build failure.
PiperOrigin-RevId: 157696653
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fix GPU build failure.]


```

---



## 2017-06-01T10:26:35Z -- Make calling AbstractModule.__init__ without named arguments throw an exception. (`601c4f39`)

**Reason:** Immediately followed by fix commit 85a58de0 ("Fix GPU build failure.") touching overlapping files (sonnet.ts)
**Fixed by:** `85a58de0` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Make calling AbstractModule.__init__ without named arguments throw an exception.
PiperOrigin-RevId: 157694146
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Make calling AbstractModule.__init__ without named arguments throw an exception.]


```

---



## 2017-05-31T11:50:53Z -- Update io_bazel_rules_closure dependency. (`1f1fc35c`)

**Reason:** Immediately followed by fix commit 03ada1f8 ("Fix typos in mlp.py and convnet.py.") touching overlapping files (sonnet.ts)
**Fixed by:** `03ada1f8` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Update io_bazel_rules_closure dependency.
PiperOrigin-RevId: 157573864
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Update io_bazel_rules_closure dependency.]


```

---



## 2017-04-28T19:12:55Z -- Implementation of Layer Normalization (https://arxiv.org/abs/1607.06450) for LSTM module. (`e3d1fe7a`)

**Reason:** Immediately followed by fix commit 2c70faf3 ("Add link to TensorFlow docs for VALID/SAME and fix existing links.") touching overlapping files (sonnet.ts)
**Fixed by:** `2c70faf3` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Implementation of Layer Normalization (https://arxiv.org/abs/1607.06450) for LSTM module.
PiperOrigin-RevId: 154567537
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Implementation of Layer Normalization https://arxiv.org/abs/1607.06450 for LSTM module.]


```

---



## 2017-04-28T14:10:57Z -- Setup Sonnet Jenkins tests for py2. (`697c20f7`)

**Reason:** Immediately followed by fix commit 578e3360 ("fixes for python3 compatibility") touching overlapping files (sonnet.ts)
**Fixed by:** `578e3360` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Setup Sonnet Jenkins tests for py2.
PiperOrigin-RevId: 154536824
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Setup Sonnet Jenkins tests for py2.]


```

---



## 2017-04-27T16:59:21Z -- 1. Add a clone method to snt.Linear (`a4c239c8`)

**Reason:** Immediately followed by fix commit 23972bbf ("Fix Clone tests to be compatible with TF 1.0.1.") touching overlapping files (sonnet.ts)
**Fixed by:** `23972bbf` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
1. Add a clone method to snt.Linear
2. Add property getters for initializers, partitioners and regularizers

PiperOrigin-RevId: 154435159
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[1. Add a clone method to snt.Linear]


```

---



## 2017-04-19T16:23:41Z -- Workaround for crash in some broken GCC compiler versions. (`9d209830`)

**Reason:** Self-identified failure / WIP in commit subject ("Workaround for crash in some broken GCC compiler versions.")

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Workaround for crash in some broken GCC compiler versions.
PiperOrigin-RevId: 153595288
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Workaround for crash in some broken GCC compiler versions.]


```

---



## 2017-04-13T16:58:57Z -- Fixed reuse_vars documentation. (`a33ec9a2`)

**Reason:** Immediately followed by fix commit 5ab8510d ("Fix doc-level comment for batch_norm_test.") touching overlapping files (sonnet.ts)
**Fixed by:** `5ab8510d` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Fixed reuse_vars documentation.
PiperOrigin-RevId: 153071731
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Fixed reuse_vars documentation.]


```

---



## 2017-04-13T16:30:32Z -- Sonnet: fix typo in readme. (`5049ec24`)

**Reason:** Immediately followed by fix commit a33ec9a2 ("Fixed reuse_vars documentation.") touching overlapping files (sonnet.ts)
**Fixed by:** `a33ec9a2` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Sonnet: fix typo in readme.
PiperOrigin-RevId: 153068865
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Sonnet: fix typo in readme.]


```

---



## 2017-04-13T15:22:41Z -- fix TextModel use_skip_connections usage (`bf96c21c`)

**Reason:** Immediately followed by fix commit 5049ec24 ("Sonnet: fix typo in readme.") touching overlapping files (sonnet.ts)
**Fixed by:** `5049ec24` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
fix TextModel use_skip_connections usage
GitOrigin-RevId=363600f378d742ec5622098218c706badfc573bc
PiperOrigin-RevId: 153062776
```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[fix TextModel use_skip_connections usage]


```

---



## 2017-04-06T16:20:35Z -- Initial commit. (`3a305e16`)

**Reason:** Immediately followed by fix commit bf96c21c ("fix TextModel use_skip_connections usage") touching overlapping files (sonnet.ts)
**Fixed by:** `bf96c21c` (see CORRECT.md for recovery commit)

**Files touched:**
- `sonnet.ts`

**Commit message:**
```
Initial commit.

```

**Diff:**
```diff
diff --git a/sonnet.ts b/sonnet.ts
--- a/sonnet.ts
+++ b/sonnet.ts
@@ -1,1 +1,3 @@
+[Initial commit.]


```

---
