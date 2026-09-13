# Failed Commits Ledger (WRONG.md)

> Record of every commit that failed, was reverted, or required immediate patching, paired with its recovery link (newest commits first).

## 2023-11-17T19:59:28Z -- Release 20231117 (`e58f2880`)

**Reason:** Immediately followed by fix commit 8bc88606 ("Fix triton env marker (#1887)") touching overlapping files (whisper.ts)
**Fixed by:** `8bc88606` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Release 20231117

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Release 20231117]


```

---

## 2023-09-19T00:13:19Z -- Release 20230918 (`0a60fcaa`)

**Reason:** Immediately followed by fix commit b38a1f20 ("Fix exception when an audio file with no speech is provided (#1396)") touching overlapping files (whisper.ts)
**Fixed by:** `b38a1f20` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Release 20230918

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Release 20230918]


```

---

## 2023-09-18T22:59:49Z -- Update model-card.md (#1643) (`29b7df62`)

**Reason:** Immediately followed by fix commit 21010ef4 ("fix doc of TextDecoder (#1526)") touching overlapping files (whisper.ts)
**Fixed by:** `21010ef4` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Update model-card.md (#1643)
fixed a few typos
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update model-card.md 1643]


```

---

## 2023-05-05T06:48:06Z -- Fix numba depreceation notice (#1233) (`7ca9fbea`)

**Reason:** Immediately followed by fix commit 248b6cb1 ("fix condition_on_previous_text (#1224)") touching overlapping files (whisper.ts)
**Fixed by:** `248b6cb1` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Fix numba depreceation notice (#1233)
From numba 0.57 raise a warning if `nopython` is not supplied:
https://numba.readthedocs.io/en/stable/reference/deprecation.html#deprecation-of-object-mode-fall-back-behaviour-when-using-jit
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix numba depreceation notice 1233]


```

---

## 2023-05-05T06:47:45Z -- Updated README.md to provide more insight on BLEU and specific appendices (#1236) (`b1c0815c`)

**Reason:** Immediately followed by fix commit 7ca9fbea ("Fix numba depreceation notice (#1233)") touching overlapping files (whisper.ts)
**Fixed by:** `7ca9fbea` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Updated README.md to provide more insight on BLEU and specific appendices (#1236)
* Updated README.md to provide more insight on BLEU and specific appendices in the research paper

* Update README.md

---------

Co-authored-by: Jong Wook Kim <jongwook@openai.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Updated README.md to provide more insight on BLEU and specific appendices 1236]


```

---

## 2023-04-11T00:23:53Z -- Squash long words at window and sentence boundaries. (#1114) (`255887f2`)

**Reason:** Self-identified failure / WIP in commit subject ("Squash long words at window and sentence boundaries. (#1114)")

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Squash long words at window and sentence boundaries. (#1114)
* Squash long words at window and sentence boundaries.

* Formatting requirements.

* Fix squashing logic to point to correct words.

---------

Co-authored-by: Jong Wook Kim <jongwook@openai.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Squash long words at window and sentence boundaries. 1114]


```

---

## 2023-03-29T20:12:36Z -- Update tokenizer.py (#1163) (`b5851c6c`)

**Reason:** Immediately followed by fix commit a151816b ("python-publish.yml: bump actions version to fix node warning (#1211)") touching overlapping files (whisper.ts)
**Fixed by:** `a151816b` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Update tokenizer.py (#1163)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update tokenizer.py 1163]


```

---

## 2023-03-14T07:07:09Z -- fix github language stats getting dominated by jupyter notebook (#1076) (`ba88b8e1`)

**Reason:** Immediately followed by fix commit 5f9ac653 ("Fix truncated words list when the replacement character is decoded (#1089)") touching overlapping files (whisper.ts)
**Fixed by:** `5f9ac653` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
fix github language stats getting dominated by jupyter notebook (#1076)
Co-authored-by: Akash Mahajan <akash.mahajan@microsoft.com>
Co-authored-by: Jong Wook Kim <jongwook@openai.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[fix github language stats getting dominated by jupyter notebook 1076]


```

---

## 2023-03-13T23:34:09Z -- Fix alignment between the segments and the list of words (#1087) (`671ac5a4`)

**Reason:** Immediately followed by fix commit ba88b8e1 ("fix github language stats getting dominated by jupyter notebook (#1076)") touching overlapping files (whisper.ts)
**Fixed by:** `ba88b8e1` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Fix alignment between the segments and the list of words (#1087)
* Fix alignment between the segments and the list of words

* Ensure the word index does not overflow
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix alignment between the segments and the list of words 1087]


```

---

## 2023-03-13T09:34:16Z -- Use tiktoken (#1044) (`839639a2`)

**Reason:** Immediately followed by fix commit 671ac5a4 ("Fix alignment between the segments and the list of words (#1087)") touching overlapping files (whisper.ts)
**Fixed by:** `671ac5a4` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Use tiktoken (#1044)
* use tiktoken==0.3.0

* formatting

* tuple should be safer

* Update whisper/tokenizer.py

Co-authored-by: Ruhollah Majdoddin <r.majdodin@gmail.com>

* use tiktoken 0.3.1

* reflecting suggestions

* cleanup

* bypassing load_tiktoken_bpe to avoid blobfile dep

---------

Co-authored-by: Ruhollah Majdoddin <r.majdodin@gmail.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Use tiktoken 1044]


```

---

## 2023-03-08T04:43:49Z -- fix typo (`aac47c98`)

**Reason:** Immediately followed by fix commit 38f2f4d9 ("fix all_tokens handling that caused more repetitions and discrepancy in JSON (#1060)") touching overlapping files (whisper.ts)
**Fixed by:** `38f2f4d9` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
fix typo

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[fix typo]


```

---

## 2023-03-08T04:36:29Z -- Release 20230307 (`26807ec6`)

**Reason:** Immediately followed by fix commit aac47c98 ("fix typo") touching overlapping files (whisper.ts)
**Fixed by:** `aac47c98` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Release 20230307

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Release 20230307]


```

---

## 2023-03-08T04:08:45Z -- attempt to fix the repetition/hallucination issue identified in #1046 (#1052) (`919a7134`)

**Reason:** Self-identified failure / WIP in commit subject ("attempt to fix the repetition/hallucination issue identified in #1046 (#1052)")

**Files touched:**
- `whisper.ts`

**Commit message:**
```
attempt to fix the repetition/hallucination issue identified in #1046 (#1052)
* attempt to fix the repetition/hallucination issue identified in #1046

* zero-pad the audio instead of spectrogram

* formatting fix

* delete debug print
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[attempt to fix the repetition/hallucination issue identified in 1046 1052]


```

---

## 2023-03-08T00:56:31Z -- Use triton==2.0.0 (#1053) (`38e990d8`)

**Reason:** Immediately followed by fix commit 919a7134 ("attempt to fix the repetition/hallucination issue identified in #1046 (#1052)") touching overlapping files (whisper.ts)
**Fixed by:** `919a7134` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Use triton==2.0.0 (#1053)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Use triton2.0.0 1053]


```

---

## 2023-01-27T08:01:49Z -- clarify that 3.11 is not supported (`5c1a8c10`)

**Reason:** Immediately followed by fix commit 7858aa9c ("Fix infinite loop caused by incorrect timestamp tokens prediction (#914)") touching overlapping files (whisper.ts)
**Fixed by:** `7858aa9c` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
clarify that 3.11 is not supported

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[clarify that 3.11 is not supported]


```

---

## 2023-01-18T18:30:18Z -- verbose outputs from pytest (`8135a7c3`)

**Reason:** Immediately followed by fix commit ea1c2667 ("Fix bug where mm is mistakenly replaced with hmm in e.g. 20mm (#659)") touching overlapping files (whisper.ts)
**Fixed by:** `ea1c2667` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
verbose outputs from pytest

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[verbose outputs from pytest]


```

---

## 2023-01-10T18:53:18Z -- torch.concatenate -> torch.cat for compatibility (`f82bc59f`)

**Reason:** Immediately followed by fix commit 70861c7c ("Fix tiny transcribe() docstring typo (#857)") touching overlapping files (whisper.ts)
**Fixed by:** `70861c7c` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
torch.concatenate -> torch.cat for compatibility

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[torch.concatenate - torch.cat for compatibility]


```

---

## 2022-12-30T06:02:52Z -- saving the qk matrix in the attention module for convenience (`68e44bd8`)

**Reason:** Reverted by commit 9323b252 ("Revert "saving the qk matrix in the attention module for convenience"")
**Fixed by:** `9323b252` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
saving the qk matrix in the attention module for convenience

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[saving the qk matrix in the attention module for convenience]


```

---

## 2022-11-16T12:18:50Z -- invoking __call__ instead of forward() (`eff383b2`)

**Reason:** Immediately followed by fix commit ec1b34bb ("fix compression ratio function (#561)") touching overlapping files (whisper.ts)
**Fixed by:** `ec1b34bb` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
invoking __call__ instead of forward()

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[invoking __call__ instead of forward]


```

---

## 2022-11-15T19:44:36Z -- suppress generating non-timestamp tokens at the beginning (#532) (`76148a56`)

**Reason:** Immediately followed by fix commit 02aa851a ("fix to return only the text token ids") touching overlapping files (whisper.ts)
**Fixed by:** `02aa851a` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
suppress generating non-timestamp tokens at the beginning (#532)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[suppress generating non-timestamp tokens at the beginning 532]


```

---

## 2022-10-17T20:51:16Z -- Add package metadata to setup.py (#315) (`7f3e408e`)

**Reason:** Immediately followed by fix commit 9f70a352 ("Fix attention caching to make it actually work (#370)") touching overlapping files (whisper.ts)
**Fixed by:** `9f70a352` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Add package metadata to setup.py (#315)
Add project summary, license, etc. for display with
"pip show" and similar Python package distribution tools.
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Add package metadata to setup.py 315]


```

---

## 2022-10-09T09:40:12Z -- transcribe() on English-only model won't complain when language="en" is not given (`d18e9ea5`)

**Reason:** Immediately followed by fix commit f6805700 ("Fix bug (#305)") touching overlapping files (whisper.ts)
**Fixed by:** `f6805700` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
transcribe() on English-only model won't complain when language="en" is not given

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[transcribe on English-only model wont complain when languageen is not given]


```

---

## 2022-10-03T21:51:07Z -- Fix timestamps and strip extraneous whitespace in WebVTT output (#219) (`02b74308`)

**Reason:** Immediately followed by fix commit 9e653bd0 ("Fixed CoW RuntimeError in DecodingTask.run() (#240)") touching overlapping files (whisper.ts)
**Fixed by:** `9e653bd0` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Fix timestamps and strip extraneous whitespace in WebVTT output (#219)
* Use two-digit hours in WebVTT timestamps

Per the WebVTT specification [0]:

> A WebVTT timestamp consists of the following components, in the given
> order:
>
> 1. Optionally (required if hours is non-zero):
>   1. Two or more ASCII digits, representing the hours as a base ten
>      integer.
>   2. A U+003A COLON character (:)

YouTube won’t accept timestamps containing single-digit hours.

[0] https://www.w3.org/TR/webvtt1/#webvtt-timestamp

* Strip segment text in WebVTT output

We already do this for plain text and SubRip output, so we should do it
for WebVTT too.
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix timestamps and strip extraneous whitespace in WebVTT output 219]


```

---

## 2022-09-30T21:45:51Z -- Add model_dir to arguments (#202) (`0b1ba3d4`)

**Reason:** Immediately followed by fix commit 02b74308 ("Fix timestamps and strip extraneous whitespace in WebVTT output (#219)") touching overlapping files (whisper.ts)
**Fixed by:** `02b74308` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Add model_dir to arguments (#202)
* Add model_dir to arguments

* minor formatting change

Co-authored-by: Jong Wook Kim <jongwook@openai.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Add model_dir to arguments 202]


```

---

## 2022-09-26T17:54:26Z -- Use PyTorch as logits transpose for ONNX support (#141) (`9c8183a1`)

**Reason:** Immediately followed by fix commit b4308c47 ("fix: transcribe verbosity (#140)") touching overlapping files (whisper.ts)
**Fixed by:** `b4308c47` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Use PyTorch as logits transpose for ONNX support (#141)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Use PyTorch as logits transpose for ONNX support 141]


```

---

## 2022-09-26T10:50:26Z -- add srt subtitle export utility (#102) (`ead77fab`)

**Reason:** Immediately followed by fix commit 520796a3 ("fix token suppression (#123)") touching overlapping files (whisper.ts)
**Fixed by:** `520796a3` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
add srt subtitle export utility (#102)
* add srt subtitle export utility

* simplifying

Co-authored-by: Jong Wook Kim <jongwook@nyu.edu>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[add srt subtitle export utility 102]


```

---

## 2022-09-23T03:57:39Z -- Avoid keeping redundant copies of model weights in memory during load (#42) (`f296bcd3`)

**Reason:** Immediately followed by fix commit 61989529 ("Fix possible mistake when loading model to device (#57)") touching overlapping files (whisper.ts)
**Fixed by:** `61989529` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Avoid keeping redundant copies of model weights in memory during load (#42)
* don't keep copies of model weights in host memory

* adding type annotation

Co-authored-by: Jong Wook Kim <jongwook@nyu.edu>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Avoid keeping redundant copies of model weights in memory during load 42]


```

---

## 2022-09-23T03:26:38Z -- Add rust as a dependency (#30) (`957ffc77`)

**Reason:** Immediately followed by fix commit a4fe05aa ("Add conda environment.yml (and fix requirements.txt) (#8)") touching overlapping files (whisper.ts)
**Fixed by:** `a4fe05aa` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Add rust as a dependency (#30)
* Add rust as a dependency

* Update README.md

Co-authored-by: Jong Wook Kim <ilikekjw@gmail.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Add rust as a dependency 30]


```

---

## 2022-09-23T03:10:55Z -- Use UTF-8 encoding to save the txt and vtt files (#37) (`c85eaaae`)

**Reason:** Immediately followed by fix commit 59f543e2 ("Fix exception cause in audio.py (#33)") touching overlapping files (whisper.ts)
**Fixed by:** `59f543e2` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Use UTF-8 encoding to save the txt and vtt files (#37)
Explicitly set the text encoding to UTF-8 in order to avoid UnicodeEncodeErrors

Co-authored-by: Jong Wook Kim <jongwook@nyu.edu>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Use UTF-8 encoding to save the txt and vtt files 37]


```

---

## 2022-09-23T02:37:57Z -- Add scoop install for windows (#48) (`c0607e8d`)

**Reason:** Immediately followed by fix commit 759e8d47 ("Fix output_dir argument when audio file is a path (#45)") touching overlapping files (whisper.ts)
**Fixed by:** `759e8d47` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Add scoop install for windows (#48)
Adding scoop install to setup for windows for ffmpeg
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Add scoop install for windows 48]


```

---

## 2022-09-22T02:48:57Z -- Merge pull request #24 from ldanilov/patch-1 (`f83cb83a`)

**Reason:** Immediately followed by fix commit e90b8fa7 ("Merge pull request #14 from bquast/patch-1") touching overlapping files (whisper.ts)
**Fixed by:** `e90b8fa7` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Merge pull request #24 from ldanilov/patch-1
fixes the link to the model paper
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Merge pull request 24 from ldanilov/patch-1]


```

---

## 2022-09-22T01:25:17Z -- fixes the link to the model paper (`45fc3d43`)

**Reason:** Immediately followed by fix commit f83cb83a ("Merge pull request #24 from ldanilov/patch-1") touching overlapping files (whisper.ts)
**Fixed by:** `f83cb83a` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
fixes the link to the model paper

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[fixes the link to the model paper]


```

---

## 2022-09-21T21:17:02Z -- make LICENSE a link instead of code-formatted text (`08a739ad`)

**Reason:** Immediately followed by fix commit 45fc3d43 ("fixes the link to the model paper") touching overlapping files (whisper.ts)
**Fixed by:** `45fc3d43` (see CORRECT.md for recovery commit)

**Files touched:**
- `whisper.ts`

**Commit message:**
```
make LICENSE a link instead of code-formatted text

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[make LICENSE a link instead of code-formatted text]


```

---

