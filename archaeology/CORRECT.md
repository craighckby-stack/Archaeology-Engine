# Correct Commits Ledger (CORRECT.md)

> Full content and diffs of every commit that succeeded without failure or immediate reversion (newest commits first).

## Mon, 31 Aug 2026 10:19:19 -0700 -- `\b` also matches next to ".", "$", "%" and "-", so the readability rule (`86098128`)

**Author:** Unknown

**Files touched:**
- `tests/test_normalizer.py`
- `whisper/normalizers/english.py`

**Commit message:**
```
`\b` also matches next to ".", "$", "%" and "-", so the readability rule

```

**Diff:**
```diff
that rewrites the digit 1 as "one" fired inside numbers as well:
"three point one" gave "3 one", "one point five" gave "one.5",
"one dollar" gave " one" and "one percent" gave "one ".

Require whitespace on both sides so only a lone 1 is rewritten. The
existing tests already pin the shape for the neighboring values
("3 point 2" -> "3.2", "3 cents" -> "¢3", "ninety percent" -> "90%",
"minus 500" -> "-500"); the value one now matches them.


Claude-Session: https://claude.ai/code/session_01TLWefdPAbf8QT4fS8Pkk68

Co-authored-by: Claude Opus 5 (1M context) <noreply@anthropic.com>
---
 tests/test_normalizer.py       | 8 ++++++++
 whisper/normalizers/english.py | 6 ++++--
 2 files changed, 12 insertions(+), 2 deletions(-)

diff --git a/tests/test_normalizer.py b/tests/test_normalizer.py
index 3bc65744c..7f89da0ec 100644
--- a/tests/test_normalizer.py
+++ b/tests/test_normalizer.py
@@ -42,13 +42,16 @@ def test_number_normalizer(std):
     assert std("3.14") == "3.14"
     assert std("3 point 2") == "3.2"
     assert std("3 point 14") == "3.14"
+    assert std("3 point 1") == "3.1"
     assert std("fourteen point 4") == "14.4"
+    assert std("one point five") == "1.5"
     assert std("two point two five dollars") == "$2.25"
     assert std("two hundred million dollars") == "$200000000"
     assert std("$20.1 million") == "$20100000"
 
     assert std("ninety percent") == "90%"
     assert std("seventy six per cent") == "76%"
+    assert std("one percent") == "1%"
 
     assert std("double oh seven") == "007"
     assert std("double zero seven") == "007"
@@ -61,11 +64,13 @@ def test_number_normalizer(std):
 
     assert std("minus 500") == "-500"
     assert std("positive twenty thousand") == "+20000"
+    assert std("minus one") == "-1"
 
     assert std("two dollars and seventy cents") == "$2.70"
     assert std("3 cents") == "¢3"
     assert std("$0.36") == "¢36"
     assert std("three euros and sixty five cents") == "€3.65"
+    assert std("one dollar and one cent") == "$1.01"
 
     assert std("three and a half million") == "3500000"
     assert std("forty eight and a half dollars") == "$48.5"
@@ -73,6 +78,9 @@ def test_number_normalizer(std):
     assert std("10 th") == "10th"
     assert std("10th") == "10th"
 
+    assert std("one") == "one"
+    assert std("ones") == "ones"
+
 
 def test_spelling_normalizer():
     std = EnglishSpellingNormalizer()
diff --git a/whisper/normalizers/english.py b/whisper/normalizers/english.py
index 4932042bc..21c3a54e5 100644
--- a/whisper/normalizers/english.py
+++ b/whisper/normalizers/english.py
@@ -434,8 +434,10 @@ def extract_cents(m: Match):
         s = re.sub(r"([€£$])([0-9]+) (?:and )?¢([0-9]{1,2})\b", combine_cents, s)
         s = re.sub(r"[€£$]0.([0-9]{1,2})\b", extract_cents, s)
 
-        # write "one(s)" instead of "1(s)", just for the readability
-        s = re.sub(r"\b1(s?)\b", r"one\1", s)
+        # write "one(s)" instead of "1(s)", just for the readability;
+        # only when it stands alone, since `\b` would also match inside
+        # numbers like "$1", "1%", "1.5" and "3.1"
+        s = re.sub(r"(?<!\S)1(s?)(?!\S)", r"one\1", s)
 
         return s
 



```

---

## Tue, 28 Jul 2026 21:18:29 +0100 -- Co-authored-by: Dorijan Magasic <dorijan.magasic@example.com> (`5f86d1d8`)

**Author:** Unknown

**Files touched:**
- `whisper/model.py`

**Commit message:**
```
Co-authored-by: Dorijan Magasic <dorijan.magasic@example.com>

```

**Diff:**
```diff
---
 whisper/model.py | 7 +++++++
 1 file changed, 7 insertions(+)

diff --git a/whisper/model.py b/whisper/model.py
index e53744738..1dec35cff 100644
--- a/whisper/model.py
+++ b/whisper/model.py
@@ -121,6 +121,13 @@ def qkv_attention(
         v = v.view(*v.shape[:2], self.n_head, -1).permute(0, 2, 1, 3)
 
         if SDPA_AVAILABLE and MultiHeadAttention.use_sdpa:
+            if k.shape[0] == 1 and q.shape[0] != 1:
+                # Cross-attention K/V have batch 1 and broadcast against the
+                # beam-expanded query; the fused SDPA kernels reject the batch
+                # mismatch and fall back to math, so expand K/V to a stride-0
+                # view (no copy) to keep them on the fast path.
+                k = k.expand(q.shape[0], *k.shape[1:])
+                v = v.expand(q.shape[0], *v.shape[1:])
             a = scaled_dot_product_attention(
                 q, k, v, is_causal=mask is not None and n_ctx > 1
             )



```

---

## Wed, 29 Jul 2026 00:35:58 +0530 -- This adds --output_format jsonl to the CLI, producing a .jsonl file (`90fdc511`)

**Author:** Unknown

**Files touched:**
- `whisper/transcribe.py`
- `whisper/utils.py`

**Commit message:**
```
This adds --output_format jsonl to the CLI, producing a .jsonl file

```

**Diff:**
```diff
where each line is a JSON object representing one segment, making it
easy to pipe into jq or other line-oriented tools.
---
 whisper/transcribe.py |  2 +-
 whisper/utils.py      | 11 +++++++++++
 2 files changed, 12 insertions(+), 1 deletion(-)

diff --git a/whisper/transcribe.py b/whisper/transcribe.py
index 0a4cc3623..56b29fbc8 100644
--- a/whisper/transcribe.py
+++ b/whisper/transcribe.py
@@ -531,7 +531,7 @@ def valid_model_name(name):
     parser.add_argument("--model_dir", type=str, default=None, help="the path to save model files; uses ~/.cache/whisper by default")
     parser.add_argument("--device", default="cuda" if torch.cuda.is_available() else "cpu", help="device to use for PyTorch inference")
     parser.add_argument("--output_dir", "-o", type=str, default=".", help="directory to save the outputs")
-    parser.add_argument("--output_format", "-f", type=str, default="all", choices=["txt", "vtt", "srt", "tsv", "json", "all"], help="format of the output file; if not specified, all available formats will be produced")
+    parser.add_argument("--output_format", "-f", type=str, default="all", choices=["txt", "vtt", "srt", "tsv", "json", "jsonl", "all"], help="format of the output file; if not specified, all available formats will be produced")
     parser.add_argument("--verbose", type=str2bool, default=True, help="whether to print out the progress and debug messages")
 
     parser.add_argument("--task", type=str, default="transcribe", choices=["transcribe", "translate"], help="whether to perform X->X speech recognition ('transcribe') or X->English translation ('translate')")
diff --git a/whisper/utils.py b/whisper/utils.py
index 13792f764..79658fc75 100644
--- a/whisper/utils.py
+++ b/whisper/utils.py
@@ -293,6 +293,16 @@ def write_result(
         json.dump(result, file)
 
 
+class WriteJSONL(ResultWriter):
+    extension: str = "jsonl"
+
+    def write_result(
+        self, result: dict, file: TextIO, options: Optional[dict] = None, **kwargs
+    ):
+        for segment in result["segments"]:
+            print(json.dumps(segment), file=file, flush=True)
+
+
 def get_writer(
     output_format: str, output_dir: str
 ) -> Callable[[dict, TextIO, dict], None]:
@@ -302,6 +312,7 @@ def get_writer(
         "srt": WriteSRT,
         "tsv": WriteTSV,
         "json": WriteJSON,
+        "jsonl": WriteJSONL,
     }
 
     if output_format == "all":



```

---

## Wed, 15 Apr 2026 12:32:15 -0400 -- * Pin pre-commit hook revisions to immutable commits (`04f449b8`)

**Author:** Unknown

**Files touched:**
- `.pre-commit-config.yaml`

**Commit message:**
```
* Pin pre-commit hook revisions to immutable commits

```

**Diff:**
```diff
Co-authored-by: Codex <noreply@openai.com>

* Add version comments for pinned pre-commit hook revisions

---------

Co-authored-by: Codex <noreply@openai.com>
Co-authored-by: Codex <codex@openai.com>
---
 .pre-commit-config.yaml | 8 ++++----
 1 file changed, 4 insertions(+), 4 deletions(-)

diff --git a/.pre-commit-config.yaml b/.pre-commit-config.yaml
index 514f94091..471da0573 100644
--- a/.pre-commit-config.yaml
+++ b/.pre-commit-config.yaml
@@ -1,6 +1,6 @@
 repos:
   - repo: https://github.com/pre-commit/pre-commit-hooks
-    rev: v5.0.0
+    rev: cef0300fd0fc4d2a87a85fa2093c6b283ea36f4b  # v5.0.0
     hooks:
       - id: check-json
       - id: end-of-file-fixer
@@ -11,17 +11,17 @@ repos:
       - id: check-added-large-files
         args: [--maxkb=4096]
   - repo: https://github.com/psf/black
-    rev: 25.1.0
+    rev: 8a737e727ac5ab2f1d4cf5876720ed276dc8dc4b  # 25.1.0
     hooks:
       - id: black
   - repo: https://github.com/pycqa/isort
-    rev: 6.0.0
+    rev: 0a0b7a830386ba6a31c2ec8316849ae4d1b8240d  # 6.0.0
     hooks:
       - id: isort
         name: isort (python)
         args: ["--profile", "black", "-l", "88", "--trailing-comma", "--multi-line", "3"]
   - repo: https://github.com/pycqa/flake8.git
-    rev: 7.1.1
+    rev: cf1542cefa3e766670b2066dd75c4571d682a649  # 7.1.1
     hooks:
       - id: flake8
         types: [python]



```

---

## Fri, 27 Mar 2026 16:08:20 -0500 -- --- (`cba37681`)

**Author:** Unknown

**Files touched:**
- `.github/workflows/python-publish.yml`
- `.github/workflows/test.yml`

**Commit message:**
```
---

```

**Diff:**
```diff
 .github/workflows/python-publish.yml |  8 ++++----
 .github/workflows/test.yml           | 10 +++++-----
 2 files changed, 9 insertions(+), 9 deletions(-)

diff --git a/.github/workflows/python-publish.yml b/.github/workflows/python-publish.yml
index ff8f12257..f70e0a37b 100644
--- a/.github/workflows/python-publish.yml
+++ b/.github/workflows/python-publish.yml
@@ -8,14 +8,14 @@ jobs:
   deploy:
     runs-on: ubuntu-latest
     steps:
-    - uses: actions/checkout@v4
-    - uses: actions-ecosystem/action-regex-match@v2
+    - uses: actions/checkout@34e114876b0b11c390a56381ad16ebd13914f8d5 # v4
+    - uses: actions-ecosystem/action-regex-match@9e6c4fb3d5e898f505be7a1fb6e7b0a278f6665b # v2
       id: regex-match
       with:
         text: ${{ github.event.head_commit.message }}
         regex: '^Release ([^ ]+)'
     - name: Set up Python
-      uses: actions/setup-python@v5
+      uses: actions/setup-python@a26af69be951a213d495a4c3e4e4022e16d87065 # v5
       with:
         python-version: '3.12'
     - name: Install dependencies
@@ -24,7 +24,7 @@ jobs:
         pip install setuptools wheel twine build
     - name: Release
       if: ${{ steps.regex-match.outputs.match != '' }}
-      uses: softprops/action-gh-release@v2
+      uses: softprops/action-gh-release@153bb8e04406b158c6c84fc1615b65b24149a1fe # v2
       with:
         tag_name: v${{ steps.regex-match.outputs.group1 }}
     - name: Build and publish
diff --git a/.github/workflows/test.yml b/.github/workflows/test.yml
index 3b53de89f..a37a88f66 100644
--- a/.github/workflows/test.yml
+++ b/.github/workflows/test.yml
@@ -11,10 +11,10 @@ jobs:
   pre-commit:
     runs-on: ubuntu-latest
     steps:
-      - uses: actions/checkout@v4
+      - uses: actions/checkout@34e114876b0b11c390a56381ad16ebd13914f8d5 # v4
       - name: Fetch base branch
         run: git fetch origin ${{ github.base_ref }}
-      - uses: actions/setup-python@v5
+      - uses: actions/setup-python@a26af69be951a213d495a4c3e4e4022e16d87065 # v5
         with:
           python-version: "3.9"
           architecture: x64
@@ -23,7 +23,7 @@ jobs:
         run: |
           echo "dir=$(pip cache dir)" >> $GITHUB_OUTPUT
       - name: pip/pre-commit cache
-        uses: actions/cache@v4
+        uses: actions/cache@0057852bfaa89a56745cba8c7296529d2fc39830 # v4
         with:
           path: |
             ${{ steps.pip-cache.outputs.dir }}
@@ -71,9 +71,9 @@ jobs:
             pytorch-version: 2.5.1
             numpy-requirement: "'numpy'"
     steps:
-      - uses: conda-incubator/setup-miniconda@v3
+      - uses: conda-incubator/setup-miniconda@fc2d68f6413eb2d87b895e92f8584b5b94a10167 # v3
       - run: conda install -n test ffmpeg python=${{ matrix.python-version }}
-      - uses: actions/checkout@v4
+      - uses: actions/checkout@34e114876b0b11c390a56381ad16ebd13914f8d5 # v4
       - run: echo "$CONDA/envs/test/bin" >> $GITHUB_PATH
       - run: pip3 install .["dev"] ${{ matrix.numpy-requirement }} torch==${{ matrix.pytorch-version }}+cpu --index-url https://download.pytorch.org/whl/cpu --extra-index-url https://pypi.org/simple
       - run: pytest --durations=0 -vv -k 'not test_transcribe or test_transcribe[tiny] or test_transcribe[tiny.en]' -m 'not requires_cuda'



```

---

## Wed, 25 Jun 2025 18:05:47 -0700 -- --- (`c0d2f624`)

**Author:** Unknown

**Files touched:**
- `.github/workflows/python-publish.yml`

**Commit message:**
```
---

```

**Diff:**
```diff
 .github/workflows/python-publish.yml | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

diff --git a/.github/workflows/python-publish.yml b/.github/workflows/python-publish.yml
index abc5356cc..ff8f12257 100644
--- a/.github/workflows/python-publish.yml
+++ b/.github/workflows/python-publish.yml
@@ -21,7 +21,7 @@ jobs:
     - name: Install dependencies
       run: |
         python -m pip install --upgrade pip
-        pip install setuptools wheel twine
+        pip install setuptools wheel twine build
     - name: Release
       if: ${{ steps.regex-match.outputs.match != '' }}
       uses: softprops/action-gh-release@v2



```

---

## Wed, 25 Jun 2025 18:02:39 -0700 -- --- (`db7fbc75`)

**Author:** Unknown

**Files touched:**
- `.github/workflows/python-publish.yml`

**Commit message:**
```
---

```

**Diff:**
```diff
 .github/workflows/python-publish.yml | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

diff --git a/.github/workflows/python-publish.yml b/.github/workflows/python-publish.yml
index 715c8ebaf..abc5356cc 100644
--- a/.github/workflows/python-publish.yml
+++ b/.github/workflows/python-publish.yml
@@ -17,7 +17,7 @@ jobs:
     - name: Set up Python
       uses: actions/setup-python@v5
       with:
-        python-version: '3.8'
+        python-version: '3.12'
     - name: Install dependencies
       run: |
         python -m pip install --upgrade pip



```

---

## Wed, 25 Jun 2025 18:00:48 -0700 -- --- (`31243bad`)

**Author:** Unknown

**Files touched:**
- `CHANGELOG.md`
- `whisper/version.py`

**Commit message:**
```
---

```

**Diff:**
```diff
 CHANGELOG.md       | 21 +++++++++++++++++++++
 whisper/version.py |  2 +-
 2 files changed, 22 insertions(+), 1 deletion(-)

diff --git a/CHANGELOG.md b/CHANGELOG.md
index 715289925..0876010ee 100644
--- a/CHANGELOG.md
+++ b/CHANGELOG.md
@@ -1,5 +1,26 @@
 # CHANGELOG
 
+## [v20250625](https://github.com/openai/whisper/releases/tag/v20250625)
+
+* Fix: Update torch.load to use weights_only=True to prevent security w… ([#2451](https://github.com/openai/whisper/pull/2451))
+* Fix: Ensure DTW cost tensor is on the same device as input tensor ([#2561](https://github.com/openai/whisper/pull/2561))
+* docs: updated README to specify translation model limitation ([#2547](https://github.com/openai/whisper/pull/2547))
+* Fixed triton kernel update to support latest triton versions ([#2588](https://github.com/openai/whisper/pull/2588))
+* Fix: GitHub display errors for Jupyter notebooks ([#2589](https://github.com/openai/whisper/pull/2589))
+* Bump the github-actions group with 3 updates ([#2592](https://github.com/openai/whisper/pull/2592))
+* Keep GitHub Actions up to date with GitHub's Dependabot ([#2486](https://github.com/openai/whisper/pull/2486))
+* pre-commit: Upgrade black v25.1.0 and isort v6.0.0 ([#2514](https://github.com/openai/whisper/pull/2514))
+* GitHub Actions: Add Python 3.13 to the testing ([#2487](https://github.com/openai/whisper/pull/2487))
+* PEP 621: Migrate from setup.py to pyproject.toml ([#2435](https://github.com/openai/whisper/pull/2435))
+* pre-commit autoupdate && pre-commit run --all-files ([#2484](https://github.com/openai/whisper/pull/2484))
+* Upgrade GitHub Actions ([#2430](https://github.com/openai/whisper/pull/2430))
+* Bugfix: Illogical "Avoid computing higher temperatures on no_speech" ([#1903](https://github.com/openai/whisper/pull/1903))
+* Updating README and doc strings to reflect that n_mels can now be 128 ([#2049](https://github.com/openai/whisper/pull/2049))
+* fix typo data/README.md ([#2433](https://github.com/openai/whisper/pull/2433))
+* Update README.md ([#2379](https://github.com/openai/whisper/pull/2379))
+* Add option to carry initial_prompt with the sliding window ([#2343](https://github.com/openai/whisper/pull/2343))
+* more pytorch versions in tests ([#2408](https://github.com/openai/whisper/pull/2408))
+
 ## [v20240930](https://github.com/openai/whisper/releases/tag/v20240930)
 
 * allowing numpy 2 in tests ([#2362](https://github.com/openai/whisper/pull/2362))
diff --git a/whisper/version.py b/whisper/version.py
index b4b3350a4..67426aa1c 100644
--- a/whisper/version.py
+++ b/whisper/version.py
@@ -1 +1 @@
-__version__ = "20240930"
+__version__ = "20250625"



```

---

## Thu, 26 Jun 2025 02:54:30 +0200 -- * Fix: Update torch.load to use weights_only=True to prevent security warning (`1f8fc975`)

**Author:** Unknown

**Files touched:**
- `whisper/__init__.py`

**Commit message:**
```
* Fix: Update torch.load to use weights_only=True to prevent security warning

```

**Diff:**
```diff
* Update __init__.py

* Update __init__.py

---------

Co-authored-by: Jong Wook Kim <jongwook@openai.com>
---
 whisper/__init__.py | 3 ++-
 1 file changed, 2 insertions(+), 1 deletion(-)

diff --git a/whisper/__init__.py b/whisper/__init__.py
index e210718f3..f284ec045 100644
--- a/whisper/__init__.py
+++ b/whisper/__init__.py
@@ -147,7 +147,8 @@ def load_model(
     with (
         io.BytesIO(checkpoint_file) if in_memory else open(checkpoint_file, "rb")
     ) as fp:
-        checkpoint = torch.load(fp, map_location=device)
+        kwargs = {"weights_only": True} if torch.__version__ >= "1.13" else {}
+        checkpoint = torch.load(fp, map_location=device, **kwargs)
     del checkpoint_file
 
     dims = ModelDimensions(**checkpoint["dims"])



```

---

## Wed, 25 Jun 2025 18:42:09 -0600 -- Co-authored-by: Jong Wook Kim <jongwook@openai.com> (`679ae1d1`)

**Author:** Unknown

**Files touched:**
- `whisper/timing.py`

**Commit message:**
```
Co-authored-by: Jong Wook Kim <jongwook@openai.com>

```

**Diff:**
```diff
---
 whisper/timing.py | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

diff --git a/whisper/timing.py b/whisper/timing.py
index e5634142b..2340000bc 100644
--- a/whisper/timing.py
+++ b/whisper/timing.py
@@ -117,7 +117,7 @@ def dtw_cuda(x, BLOCK_SIZE=1024):
     x_skew = x_skew.T.contiguous()
     cost = torch.ones(N + M + 2, M + 2) * np.inf
     cost[0, 0] = 0
-    cost = cost.cuda()
+    cost = cost.to(x.device)
     trace = torch.zeros_like(cost, dtype=torch.int32)
 
     dtw_kernel[(1,)](



```

---

## Wed, 25 Jun 2025 20:03:47 -0400 -- Updated README given info from https://github.com/openai/whisper/discussions/2483 (`f50c4f26`)

**Author:** Unknown

**Files touched:**
- `README.md`

**Commit message:**
```
Updated README given info from https://github.com/openai/whisper/discussions/2483

```

**Diff:**
```diff
---
 README.md | 26 ++++++++++++++++++--------
 1 file changed, 18 insertions(+), 8 deletions(-)

diff --git a/README.md b/README.md
index 696869c1e..196b48f62 100644
--- a/README.md
+++ b/README.md
@@ -77,25 +77,35 @@ Whisper's performance varies widely depending on the language. The figure below
 
 ![WER breakdown by language](https://github.com/openai/whisper/assets/266841/f4619d66-1058-4005-8f67-a9d811b77c62)
 
-
-
 ## Command-line usage
 
 The following command will transcribe speech in audio files, using the `turbo` model:
 
-    whisper audio.flac audio.mp3 audio.wav --model turbo
+```bash
+whisper audio.flac audio.mp3 audio.wav --model turbo
+```
+
+The default setting (which selects the `turbo` model) works well for transcribing English. However, **the `turbo` model is not trained for translation tasks**. If you need to **translate non-English speech into English**, use one of the **multilingual models** (`tiny`, `base`, `small`, `medium`, `large`) instead of `turbo`. 
+
+For example, to transcribe an audio file containing non-English speech, you can specify the language:
 
-The default setting (which selects the `turbo` model) works well for transcribing English. To transcribe an audio file containing non-English speech, you can specify the language using the `--language` option:
+```bash
+whisper japanese.wav --language Japanese
+```
 
-    whisper japanese.wav --language Japanese
+To **translate** speech into English, use:
 
-Adding `--task translate` will translate the speech into English:
+```bash
+whisper japanese.wav --model medium --language Japanese --task translate
+```
 
-    whisper japanese.wav --language Japanese --task translate
+> **Note:** The `turbo` model will return the original language even if `--task translate` is specified. Use `medium` or `large` for the best translation results.
 
 Run the following to view all available options:
 
-    whisper --help
+```bash
+whisper --help
+```
 
 See [tokenizer.py](https://github.com/openai/whisper/blob/main/whisper/tokenizer.py) for the list of all available languages.
 



```

---

## Thu, 26 Jun 2025 02:02:54 +0200 -- * Update triton kernel using _unsafe_update_src (`86899243`)

**Author:** Unknown

**Files touched:**
- `whisper/triton_ops.py`

**Commit message:**
```
* Update triton kernel using _unsafe_update_src

```

**Diff:**
```diff
* support old triton versions

* refactored changes to update triton kernel only once

* Update triton_ops.py

---------

Co-authored-by: Jong Wook Kim <jongwook@openai.com>
Co-authored-by: Jong Wook Kim <ilikekjw@gmail.com>
---
 whisper/triton_ops.py | 14 +++++++++++---
 1 file changed, 11 insertions(+), 3 deletions(-)

diff --git a/whisper/triton_ops.py b/whisper/triton_ops.py
index edd456414..13d417bb2 100644
--- a/whisper/triton_ops.py
+++ b/whisper/triton_ops.py
@@ -60,7 +60,7 @@ def kernel(
         tl.store(y_ptr + offsets, MIDDLE_ROW_HERE, mask=mask)  # noqa: F821
 
     kernel = triton.JITFunction(kernel.fn)
-    kernel.src = kernel.src.replace(
+    new_kernel = kernel.src.replace(
         "    LOAD_ALL_ROWS_HERE",
         "\n".join(
             [
@@ -69,7 +69,8 @@ def kernel(
             ]
         ),
     )
-    kernel.src = kernel.src.replace(
+
+    new_kernel = new_kernel.replace(
         "    BUBBLESORT_HERE",
         "\n\n".join(
             [
@@ -90,7 +91,14 @@ def kernel(
             ]
         ),
     )
-    kernel.src = kernel.src.replace("MIDDLE_ROW_HERE", f"row{filter_width // 2}")
+
+    new_kernel = new_kernel.replace("MIDDLE_ROW_HERE", f"row{filter_width // 2}")
+
+    if hasattr(kernel, "_unsafe_update_src") is True:
+        kernel._unsafe_update_src(new_kernel)
+        kernel.hash = None
+    else:
+        kernel.src = new_kernel
 
     return kernel
 



```

---

## Thu, 26 Jun 2025 03:55:15 +0400 -- * Update LibriSpeech.ipynb (`5dff4db8`)

**Author:** Unknown

**Files touched:**
- `notebooks/LibriSpeech.ipynb`
- `notebooks/Multilingual_ASR.ipynb`

**Commit message:**
```
* Update LibriSpeech.ipynb

```

**Diff:**
```diff
Update LibriSpeech.ipynb

* Update Multilingual_ASR.ipynb
---
 notebooks/LibriSpeech.ipynb      | 3 ++-
 notebooks/Multilingual_ASR.ipynb | 3 ++-
 2 files changed, 4 insertions(+), 2 deletions(-)

diff --git a/notebooks/LibriSpeech.ipynb b/notebooks/LibriSpeech.ipynb
index 3d90e652c..602bbe452 100644
--- a/notebooks/LibriSpeech.ipynb
+++ b/notebooks/LibriSpeech.ipynb
@@ -949,7 +949,8 @@
       "style": "IPY_MODEL_039b53f2702c4179af7e0548018d0588",
       "value": " 164/164 [05:08&lt;00:00,  1.86s/it]"
      }
-    }
+    },
+    "state": {}
    }
   }
  },
diff --git a/notebooks/Multilingual_ASR.ipynb b/notebooks/Multilingual_ASR.ipynb
index 2d32e0e02..f19e3e009 100644
--- a/notebooks/Multilingual_ASR.ipynb
+++ b/notebooks/Multilingual_ASR.ipynb
@@ -4219,7 +4219,8 @@
             "_view_name": "StyleView",
             "description_width": ""
           }
-        }
+        },
+        "state": {}
       }
     }
   },



```

---

## Tue, 13 May 2025 11:22:31 -0700 -- Bumps the github-actions group with 3 updates: [actions/checkout](https://github.com/actions/checkout), [actions/setup-python](https://github.com/actions/setup-python) and [softprops/action-gh-release](https://github.com/softprops/action-gh-release). (`dd985ac4`)

**Author:** Unknown

**Files touched:**
- `.github/workflows/python-publish.yml`

**Commit message:**
```
Bumps the github-actions group with 3 updates: [actions/checkout](https://github.com/actions/checkout), [actions/setup-python](https://github.com/actions/setup-python) and [softprops/action-gh-release](https://github.com/softprops/action-gh-release).

```

**Diff:**
```diff
Updates `actions/checkout` from 3 to 4
- [Release notes](https://github.com/actions/checkout/releases)
- [Changelog](https://github.com/actions/checkout/blob/main/CHANGELOG.md)
- [Commits](https://github.com/actions/checkout/compare/v3...v4)

Updates `actions/setup-python` from 4 to 5
- [Release notes](https://github.com/actions/setup-python/releases)
- [Commits](https://github.com/actions/setup-python/compare/v4...v5)

Updates `softprops/action-gh-release` from 1 to 2
- [Release notes](https://github.com/softprops/action-gh-release/releases)
- [Changelog](https://github.com/softprops/action-gh-release/blob/master/CHANGELOG.md)
- [Commits](https://github.com/softprops/action-gh-release/compare/v1...v2)

---
updated-dependencies:
- dependency-name: actions/checkout
  dependency-version: '4'
  dependency-type: direct:production
  update-type: version-update:semver-major
  dependency-group: github-actions
- dependency-name: actions/setup-python
  dependency-version: '5'
  dependency-type: direct:production
  update-type: version-update:semver-major
  dependency-group: github-actions
- dependency-name: softprops/action-gh-release
  dependency-version: '2'
  dependency-type: direct:production
  update-type: version-update:semver-major
  dependency-group: github-actions
...

Signed-off-by: dependabot[bot] <support@github.com>
Co-authored-by: dependabot[bot] <49699333+dependabot[bot]@users.noreply.github.com>
---
 .github/workflows/python-publish.yml | 6 +++---
 1 file changed, 3 insertions(+), 3 deletions(-)

diff --git a/.github/workflows/python-publish.yml b/.github/workflows/python-publish.yml
index c8680685c..715c8ebaf 100644
--- a/.github/workflows/python-publish.yml
+++ b/.github/workflows/python-publish.yml
@@ -8,14 +8,14 @@ jobs:
   deploy:
     runs-on: ubuntu-latest
     steps:
-    - uses: actions/checkout@v3
+    - uses: actions/checkout@v4
     - uses: actions-ecosystem/action-regex-match@v2
       id: regex-match
       with:
         text: ${{ github.event.head_commit.message }}
         regex: '^Release ([^ ]+)'
     - name: Set up Python
-      uses: actions/setup-python@v4
+      uses: actions/setup-python@v5
       with:
         python-version: '3.8'
     - name: Install dependencies
@@ -24,7 +24,7 @@ jobs:
         pip install setuptools wheel twine
     - name: Release
       if: ${{ steps.regex-match.outputs.match != '' }}
-      uses: softprops/action-gh-release@v1
+      uses: softprops/action-gh-release@v2
       with:
         tag_name: v${{ steps.regex-match.outputs.group1 }}
     - name: Build and publish



```

---

## Tue, 13 May 2025 20:10:43 +0200 -- Automates the creation of pull requests like (`e1e6aa60`)

**Author:** Unknown

**Files touched:**
- `.github/dependabot.yml`

**Commit message:**
```
Automates the creation of pull requests like

```

**Diff:**
```diff
* #2430

* [Keeping your actions up to date with Dependabot](https://docs.github.com/en/code-security/dependabot/working-with-dependabot/keeping-your-actions-up-to-date-with-dependabot)
* [Configuration options for the dependabot.yml file - package-ecosystem](https://docs.github.com/en/code-security/dependabot/dependabot-version-updates/configuration-options-for-the-dependabot.yml-file#package-ecosystem)
---
 .github/dependabot.yml | 13 +++++++++++++
 1 file changed, 13 insertions(+)
 create mode 100644 .github/dependabot.yml

diff --git a/.github/dependabot.yml b/.github/dependabot.yml
new file mode 100644
index 000000000..be006de9a
--- /dev/null
+++ b/.github/dependabot.yml
@@ -0,0 +1,13 @@
+# Keep GitHub Actions up to date with GitHub's Dependabot...
+# https://docs.github.com/en/code-security/dependabot/working-with-dependabot/keeping-your-actions-up-to-date-with-dependabot
+# https://docs.github.com/en/code-security/dependabot/dependabot-version-updates/configuration-options-for-the-dependabot.yml-file#package-ecosystem
+version: 2
+updates:
+  - package-ecosystem: github-actions
+    directory: /
+    groups:
+      github-actions:
+        patterns:
+          - "*"  # Group all Actions updates into a single larger pull request
+    schedule:
+      interval: weekly



```

---

## Tue, 13 May 2025 18:43:34 +0200 -- --- (`e6a5fc0f`)

**Author:** Unknown

**Files touched:**
- `.pre-commit-config.yaml`

**Commit message:**
```
---

```

**Diff:**
```diff
 .pre-commit-config.yaml | 4 ++--
 1 file changed, 2 insertions(+), 2 deletions(-)

diff --git a/.pre-commit-config.yaml b/.pre-commit-config.yaml
index 48df249ca..514f94091 100644
--- a/.pre-commit-config.yaml
+++ b/.pre-commit-config.yaml
@@ -11,11 +11,11 @@ repos:
       - id: check-added-large-files
         args: [--maxkb=4096]
   - repo: https://github.com/psf/black
-    rev: 24.10.0
+    rev: 25.1.0
     hooks:
       - id: black
   - repo: https://github.com/pycqa/isort
-    rev: 5.13.2
+    rev: 6.0.0
     hooks:
       - id: isort
         name: isort (python)



```

---

## Tue, 13 May 2025 06:10:40 +0200 -- * GitHub Actions: Add Python 3.13 to the testing (`13907bed`)

**Author:** Unknown

**Files touched:**
- `.github/workflows/test.yml`

**Commit message:**
```
* GitHub Actions: Add Python 3.13 to the testing

```

**Diff:**
```diff
* GitHub Actions: Add Python 3.13 to the testing

* numba==0.61.0rc2; python_version=='3.13'

* triton>=2; python_version<'3.13'

* fail-fast: false

* Numba v0.61.0 is released

https://github.com/numba/numba/releases

* Update pyproject.toml
---
 .github/workflows/test.yml | 6 +++++-
 1 file changed, 5 insertions(+), 1 deletion(-)

diff --git a/.github/workflows/test.yml b/.github/workflows/test.yml
index 16c7ff726..3b53de89f 100644
--- a/.github/workflows/test.yml
+++ b/.github/workflows/test.yml
@@ -40,6 +40,7 @@ jobs:
     needs: pre-commit
     runs-on: ubuntu-latest
     strategy:
+      fail-fast: false
       matrix:
         include:
           - python-version: '3.8'
@@ -64,7 +65,10 @@ jobs:
             pytorch-version: 2.4.1
             numpy-requirement: "'numpy'"
           - python-version: '3.12'
-            pytorch-version: 2.5.0
+            pytorch-version: 2.5.1
+            numpy-requirement: "'numpy'"
+          - python-version: '3.13'
+            pytorch-version: 2.5.1
             numpy-requirement: "'numpy'"
     steps:
       - uses: conda-incubator/setup-miniconda@v3



```

---

## Sat, 4 Jan 2025 12:56:16 -0800 -- using `-m build --sdist` instead of `setup.py sdist` (`517a43ec`)

**Author:** Unknown

**Files touched:**
- `.github/workflows/python-publish.yml`

**Commit message:**
```
using `-m build --sdist` instead of `setup.py sdist`

```

**Diff:**
```diff
---
 .github/workflows/python-publish.yml | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

diff --git a/.github/workflows/python-publish.yml b/.github/workflows/python-publish.yml
index 4b91a2ac0..c8680685c 100644
--- a/.github/workflows/python-publish.yml
+++ b/.github/workflows/python-publish.yml
@@ -33,5 +33,5 @@ jobs:
         TWINE_USERNAME: __token__
         TWINE_PASSWORD: ${{ secrets.PYPI_API_TOKEN }}
       run: |
-        python setup.py sdist
+        python -m build --sdist
         twine upload dist/*



```

---

## Sat, 4 Jan 2025 10:38:35 +0100 -- --- (`dd4d010d`)

**Author:** Unknown

**Files touched:**
- `pyproject.toml`
- `setup.py`

**Commit message:**
```
---

```

**Diff:**
```diff
 pyproject.toml | 48 +++++++++++++++++++++++++++++++++++++++++++++++-
 setup.py       | 42 ------------------------------------------
 2 files changed, 47 insertions(+), 43 deletions(-)
 delete mode 100644 setup.py

diff --git a/pyproject.toml b/pyproject.toml
index 84637eb2e..21b90e737 100644
--- a/pyproject.toml
+++ b/pyproject.toml
@@ -1,3 +1,50 @@
+[build-system]
+build-backend = "setuptools.build_meta"
+
+requires = [ "setuptools>=61.2" ]
+
+[project]
+name = "openai-whisper"
+description = "Robust Speech Recognition via Large-Scale Weak Supervision"
+readme.content-type = "text/markdown"
+readme.file = "README.md"
+license = { text = "MIT" }
+authors = [ { name = "OpenAI" } ]
+requires-python = ">=3.8"
+classifiers = [
+  "Programming Language :: Python :: 3 :: Only",
+  "Programming Language :: Python :: 3.8",
+  "Programming Language :: Python :: 3.9",
+  "Programming Language :: Python :: 3.10",
+  "Programming Language :: Python :: 3.11",
+  "Programming Language :: Python :: 3.12",
+  "Programming Language :: Python :: 3.13",
+]
+dynamic = [ "version" ]
+dependencies = [
+  "more-itertools",
+  "numba",
+  "numpy",
+  "tiktoken",
+  "torch",
+  "tqdm",
+  "triton>=2; (platform_machine=='x86_64' and sys_platform=='linux') or sys_platform=='linux2'",
+]
+optional-dependencies.dev = [ "black", "flake8", "isort", "pytest", "scipy" ]
+urls = { Homepage = "https://github.com/openai/whisper" }
+scripts.whisper = "whisper.transcribe:cli"
+
+[tool.setuptools]
+py-modules = [ "whisper" ]
+include-package-data = true
+
+[tool.setuptools.dynamic]
+version = { attr = "whisper.version.__version__" }
+
+[tool.setuptools.packages.find]
+exclude = [ "tests*" ]
+namespaces = false
+
 [tool.black]
 
 [tool.isort]
@@ -5,4 +52,3 @@ profile = "black"
 include_trailing_comma = true
 line_length = 88
 multi_line_output = 3
-
diff --git a/setup.py b/setup.py
deleted file mode 100644
index 73c4eb831..000000000
--- a/setup.py
+++ /dev/null
@@ -1,42 +0,0 @@
-import platform
-import sys
-from pathlib import Path
-
-import pkg_resources
-from setuptools import find_packages, setup
-
-
-def read_version(fname="whisper/version.py"):
-    exec(compile(open(fname, encoding="utf-8").read(), fname, "exec"))
-    return locals()["__version__"]
-
-
-requirements = []
-if sys.platform.startswith("linux") and platform.machine() == "x86_64":
-    requirements.append("triton>=2.0.0")
-
-setup(
-    name="openai-whisper",
-    py_modules=["whisper"],
-    version=read_version(),
-    description="Robust Speech Recognition via Large-Scale Weak Supervision",
-    long_description=open("README.md", encoding="utf-8").read(),
-    long_description_content_type="text/markdown",
-    readme="README.md",
-    python_requires=">=3.8",
-    author="OpenAI",
-    url="https://github.com/openai/whisper",
-    license="MIT",
-    packages=find_packages(exclude=["tests*"]),
-    install_requires=[
-        str(r)
-        for r in pkg_resources.parse_requirements(
-            Path(__file__).with_name("requirements.txt").open()
-        )
-    ],
-    entry_points={
-        "console_scripts": ["whisper=whisper.transcribe:cli"],
-    },
-    include_package_data=True,
-    extras_require={"dev": ["pytest", "scipy", "black", "flake8", "isort"]},
-)



```

---

## Sat, 4 Jan 2025 10:02:18 +0100 -- * pre-commit autoupdate && pre-commit run --all-files (`26a7cacc`)

**Author:** Unknown

**Files touched:**
- `.github/workflows/test.yml`
- `.pre-commit-config.yaml`
- `whisper/normalizers/basic.py`
- `whisper/utils.py`

**Commit message:**
```
* pre-commit autoupdate && pre-commit run --all-files

```

**Diff:**
```diff
* Black formatter needs a current version of Python
---
 .github/workflows/test.yml   |  4 ++--
 .pre-commit-config.yaml      |  8 ++++----
 whisper/normalizers/basic.py | 22 +++++++++++++---------
 whisper/utils.py             |  8 +++++---
 4 files changed, 24 insertions(+), 18 deletions(-)

diff --git a/.github/workflows/test.yml b/.github/workflows/test.yml
index 106c66b96..16c7ff726 100644
--- a/.github/workflows/test.yml
+++ b/.github/workflows/test.yml
@@ -16,7 +16,7 @@ jobs:
         run: git fetch origin ${{ github.base_ref }}
       - uses: actions/setup-python@v5
         with:
-          python-version: "3.8"
+          python-version: "3.9"
           architecture: x64
       - name: Get pip cache dir
         id: pip-cache
@@ -33,7 +33,7 @@ jobs:
             ${{ runner.os }}-pip-pre-commit
       - name: pre-commit
         run: |
-          pip install -U pre-commit
+          pip install --upgrade pre-commit
           pre-commit install --install-hooks
           pre-commit run --all-files
   whisper-test:
diff --git a/.pre-commit-config.yaml b/.pre-commit-config.yaml
index 3f5a74b6d..48df249ca 100644
--- a/.pre-commit-config.yaml
+++ b/.pre-commit-config.yaml
@@ -1,6 +1,6 @@
 repos:
   - repo: https://github.com/pre-commit/pre-commit-hooks
-    rev: v4.0.1
+    rev: v5.0.0
     hooks:
       - id: check-json
       - id: end-of-file-fixer
@@ -11,17 +11,17 @@ repos:
       - id: check-added-large-files
         args: [--maxkb=4096]
   - repo: https://github.com/psf/black
-    rev: 23.7.0
+    rev: 24.10.0
     hooks:
       - id: black
   - repo: https://github.com/pycqa/isort
-    rev: 5.12.0
+    rev: 5.13.2
     hooks:
       - id: isort
         name: isort (python)
         args: ["--profile", "black", "-l", "88", "--trailing-comma", "--multi-line", "3"]
   - repo: https://github.com/pycqa/flake8.git
-    rev: 6.0.0
+    rev: 7.1.1
     hooks:
       - id: flake8
         types: [python]
diff --git a/whisper/normalizers/basic.py b/whisper/normalizers/basic.py
index a82403203..8690ae71c 100644
--- a/whisper/normalizers/basic.py
+++ b/whisper/normalizers/basic.py
@@ -30,15 +30,19 @@ def remove_symbols_and_diacritics(s: str, keep=""):
     and drop any diacritics (category 'Mn' and some manual mappings)
     """
     return "".join(
-        c
-        if c in keep
-        else ADDITIONAL_DIACRITICS[c]
-        if c in ADDITIONAL_DIACRITICS
-        else ""
-        if unicodedata.category(c) == "Mn"
-        else " "
-        if unicodedata.category(c)[0] in "MSP"
-        else c
+        (
+            c
+            if c in keep
+            else (
+                ADDITIONAL_DIACRITICS[c]
+                if c in ADDITIONAL_DIACRITICS
+                else (
+                    ""
+                    if unicodedata.category(c) == "Mn"
+                    else " " if unicodedata.category(c)[0] in "MSP" else c
+                )
+            )
+        )
         for c in unicodedata.normalize("NFKD", s)
     )
 
diff --git a/whisper/utils.py b/whisper/utils.py
index 9b9b13862..13792f764 100644
--- a/whisper/utils.py
+++ b/whisper/utils.py
@@ -209,9 +209,11 @@ def iterate_subtitles():
 
                         yield start, end, "".join(
                             [
-                                re.sub(r"^(\s*)(.*)$", r"\1<u>\2</u>", word)
-                                if j == i
-                                else word
+                                (
+                                    re.sub(r"^(\s*)(.*)$", r"\1<u>\2</u>", word)
+                                    if j == i
+                                    else word
+                                )
                                 for j, word in enumerate(all_words)
                             ]
                         )



```

---

## Sat, 4 Jan 2025 09:47:12 +0100 -- --- (`6c1d8f1e`)

**Author:** Unknown

**Files touched:**
- `.github/workflows/test.yml`

**Commit message:**
```
---

```

**Diff:**
```diff
 .github/workflows/test.yml | 10 +++++-----
 1 file changed, 5 insertions(+), 5 deletions(-)

diff --git a/.github/workflows/test.yml b/.github/workflows/test.yml
index 84b81cc00..106c66b96 100644
--- a/.github/workflows/test.yml
+++ b/.github/workflows/test.yml
@@ -11,10 +11,10 @@ jobs:
   pre-commit:
     runs-on: ubuntu-latest
     steps:
-      - uses: actions/checkout@v3
+      - uses: actions/checkout@v4
       - name: Fetch base branch
         run: git fetch origin ${{ github.base_ref }}
-      - uses: actions/setup-python@v4
+      - uses: actions/setup-python@v5
         with:
           python-version: "3.8"
           architecture: x64
@@ -23,7 +23,7 @@ jobs:
         run: |
           echo "dir=$(pip cache dir)" >> $GITHUB_OUTPUT
       - name: pip/pre-commit cache
-        uses: actions/cache@v3
+        uses: actions/cache@v4
         with:
           path: |
             ${{ steps.pip-cache.outputs.dir }}
@@ -67,9 +67,9 @@ jobs:
             pytorch-version: 2.5.0
             numpy-requirement: "'numpy'"
     steps:
-      - uses: conda-incubator/setup-miniconda@v2
+      - uses: conda-incubator/setup-miniconda@v3
       - run: conda install -n test ffmpeg python=${{ matrix.python-version }}
-      - uses: actions/checkout@v3
+      - uses: actions/checkout@v4
       - run: echo "$CONDA/envs/test/bin" >> $GITHUB_PATH
       - run: pip3 install .["dev"] ${{ matrix.numpy-requirement }} torch==${{ matrix.pytorch-version }}+cpu --index-url https://download.pytorch.org/whl/cpu --extra-index-url https://pypi.org/simple
       - run: pytest --durations=0 -vv -k 'not test_transcribe or test_transcribe[tiny] or test_transcribe[tiny.en]' -m 'not requires_cuda'



```

---

## Sun, 1 Dec 2024 05:47:01 +0000 -- * Bugfix: Illogical "Avoid computing higher temperatures on no_speech" (`90db0de1`)

**Author:** Unknown

**Files touched:**
- `whisper/transcribe.py`

**Commit message:**
```
* Bugfix: Illogical "Avoid computing higher temperatures on no_speech"

```

**Diff:**
```diff
Bugfix for https://github.com/openai/whisper/pull/1279

It's "silence" when decoding has failed due to `compression_ratio_threshold` too, when further down the code it's not "silence" anymore.

"Silence" should be only when decoding has failed due to `logprob_threshold`.

Like described there:
https://github.com/openai/whisper/blob/8bc8860694949db53c42ba47ddc23786c2e02a8b/whisper/transcribe.py#L421

And in code there:
https://github.com/openai/whisper/blob/8bc8860694949db53c42ba47ddc23786c2e02a8b/whisper/transcribe.py#L243-L251

* Fix if "logprob_threshold=None"

---------

Co-authored-by: Jong Wook Kim <jongwook@openai.com>
---
 whisper/transcribe.py | 2 ++
 1 file changed, 2 insertions(+)

diff --git a/whisper/transcribe.py b/whisper/transcribe.py
index 8eb6a7185..0a4cc3623 100644
--- a/whisper/transcribe.py
+++ b/whisper/transcribe.py
@@ -214,6 +214,8 @@ def decode_with_fallback(segment: torch.Tensor) -> DecodingResult:
             if (
                 no_speech_threshold is not None
                 and decode_result.no_speech_prob > no_speech_threshold
+                and logprob_threshold is not None
+                and decode_result.avg_logprob < logprob_threshold
             ):
                 needs_fallback = False  # silence
             if not needs_fallback:



```

---

## Tue, 26 Nov 2024 09:37:01 -0800 -- --- (`fc5ded7d`)

**Author:** Unknown

**Files touched:**
- `README.md`
- `whisper/audio.py`

**Commit message:**
```
---

```

**Diff:**
```diff
 README.md        | 2 +-
 whisper/audio.py | 4 ++--
 2 files changed, 3 insertions(+), 3 deletions(-)

diff --git a/README.md b/README.md
index 1a661d781..696869c1e 100644
--- a/README.md
+++ b/README.md
@@ -126,7 +126,7 @@ audio = whisper.load_audio("audio.mp3")
 audio = whisper.pad_or_trim(audio)
 
 # make log-Mel spectrogram and move to the same device as the model
-mel = whisper.log_mel_spectrogram(audio).to(model.device)
+mel = whisper.log_mel_spectrogram(audio, n_mels=model.dims.n_mels).to(model.device)
 
 # detect the spoken language
 _, probs = model.detect_language(mel)
diff --git a/whisper/audio.py b/whisper/audio.py
index cf6c66ad9..826250f37 100644
--- a/whisper/audio.py
+++ b/whisper/audio.py
@@ -122,7 +122,7 @@ def log_mel_spectrogram(
         The path to audio or either a NumPy array or Tensor containing the audio waveform in 16 kHz
 
     n_mels: int
-        The number of Mel-frequency filters, only 80 is supported
+        The number of Mel-frequency filters, only 80 and 128 are supported
 
     padding: int
         Number of zero samples to pad to the right
@@ -132,7 +132,7 @@ def log_mel_spectrogram(
 
     Returns
     -------
-    torch.Tensor, shape = (80, n_frames)
+    torch.Tensor, shape = (n_mels, n_frames)
         A Tensor that contains the Mel spectrogram
     """
     if not torch.is_tensor(audio):



```

---

## Wed, 13 Nov 2024 08:35:54 +0800 -- --- (`173ff7dd`)

**Author:** Unknown

**Files touched:**
- `data/README.md`

**Commit message:**
```
---

```

**Diff:**
```diff
 data/README.md | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

diff --git a/data/README.md b/data/README.md
index 3b4aea12d..fcb320001 100644
--- a/data/README.md
+++ b/data/README.md
@@ -45,7 +45,7 @@ We downloaded the [CHiME-5 dataset](https://spandh.dcs.shef.ac.uk//chime_challen
 
 ### AMI-IHM, AMI-SDM1
 
-We preprocessed the [AMI Corpus](https://groups.inf.ed.ac.uk/ami/corpus/overview.shtml) by following the stage 0 ad 2 of the [s5b recipe](https://github.com/kaldi-asr/kaldi/tree/master/egs/ami/s5b).
+We preprocessed the [AMI Corpus](https://groups.inf.ed.ac.uk/ami/corpus/overview.shtml) by following the stage 0 and 2 of the [s5b recipe](https://github.com/kaldi-asr/kaldi/tree/master/egs/ami/s5b).
 
 
 ## Long-form English-only datasets



```

---

## Mon, 4 Nov 2024 08:00:30 +0100 -- Default now uses Turbo instead of Small (`271445b2`)

**Author:** Unknown

**Files touched:**
- `README.md`

**Commit message:**
```
Default now uses Turbo instead of Small

```

**Diff:**
```diff
---
 README.md | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

diff --git a/README.md b/README.md
index 910b7dbae..1a661d781 100644
--- a/README.md
+++ b/README.md
@@ -85,7 +85,7 @@ The following command will transcribe speech in audio files, using the `turbo` m
 
     whisper audio.flac audio.mp3 audio.wav --model turbo
 
-The default setting (which selects the `small` model) works well for transcribing English. To transcribe an audio file containing non-English speech, you can specify the language using the `--language` option:
+The default setting (which selects the `turbo` model) works well for transcribing English. To transcribe an audio file containing non-English speech, you can specify the language using the `--language` option:
 
     whisper japanese.wav --language Japanese
 



```

---

## 2024-10-26T14:17:31Z -- Add option to carry initial_prompt with the sliding window (#2343) (`5979f037`)

**Author:** kittsil <kittsil@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Add option to carry initial_prompt with the sliding window (#2343)
* Add option to carry initial_prompt with the sliding window

Add an option `carry_initial_prompt = False` to `whisper.transcribe()`.
When set to `True`, `initial_prompt` is prepended to each internal `decode()` call's `prompt`.
If there is not enough context space at the start of the prompt, the prompt is left-sliced to make space.

* Prevent redundant initial_prompt_tokens

* Revert unnecessary .gitignore change

---------

Co-authored-by: Kittsil <kittsil@gmail.com>
Co-authored-by: Jong Wook Kim <jongwook@openai.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Add option to carry initial_prompt with the sliding window 2343]


```

---

## 2024-10-26T00:30:02Z -- more pytorch versions in tests (#2408) (`cdb81479`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
more pytorch versions in tests (#2408)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[more pytorch versions in tests 2408]


```

---

## 2024-09-30T18:20:53Z -- Release 20240930 (`25639fc1`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Release 20240930

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Release 20240930]


```

---

## 2024-09-30T18:18:17Z -- allowing numpy 2 in tests (#2362) (`260bbcfc`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
allowing numpy 2 in tests (#2362)
* allowing numpy 2 in tests

* allowing numpy 2 in tests
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[allowing numpy 2 in tests 2362]


```

---

## 2024-09-30T17:59:51Z -- large-v3-turbo model (#2361) (`25e5c364`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
large-v3-turbo model (#2361)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[large-v3-turbo model 2361]


```

---

## 2024-09-30T17:33:56Z -- test on python/pytorch versions up to 3.12 and 2.4.1 (#2360) (`b66b46f3`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
test on python/pytorch versions up to 3.12 and 2.4.1 (#2360)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[test on python/pytorch versions up to 3.12 and 2.4.1 2360]


```

---

## 2024-09-30T17:27:14Z -- using sdpa if available (#2359) (`27f97132`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
using sdpa if available (#2359)
* using sdpa if available

* Update model.py
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[using sdpa if available 2359]


```

---

## 2024-09-27T23:43:58Z -- Release 20240927 (`423492dd`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Release 20240927

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Release 20240927]


```

---

## 2024-09-10T17:43:21Z -- pinning numpy<2 in tests (#2332) (`279133e3`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
pinning numpy<2 in tests (#2332)
* pinning numpy<2 in tests

* pip install together

* pip install together
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[pinning numpy2 in tests 2332]


```

---

## 2024-09-10T16:53:08Z -- Relax triton requirements for compatibility with pytorch 2.4 and newer (#2307) (`32d55d5d`)

**Author:** Jianan Xing <jiananxing@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Relax triton requirements for compatibility with pytorch 2.4 and newer (#2307)
* Relax triton requirements for compatibility with pytorch 2.4 and newer

Similar to https://github.com/openai/whisper/pull/1802, but now when pytorch upgrades to 2.4, it requires triton==3.0.0. I am not sure if it makes sense to remove the upper bound version constraints

* Update requirements.txt
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Relax triton requirements for compatibility with pytorch 2.4 and newer 2307]


```

---

## 2023-12-18T20:11:16Z -- Skip silence around hallucinations (#1838) (`ba3f3cd5`)

**Author:** ryanheise <ryanheise@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Skip silence around hallucinations (#1838)
* Add clip_timestamps option

* Add hallucination_silence_threshold option

* Fix typing for python < 3.9

---------

Co-authored-by: Jong Wook Kim <jongwook@openai.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Skip silence around hallucinations 1838]


```

---

## 2023-12-11T15:39:08Z -- Fix triton env marker (#1887) (`8bc88606`)

**Author:** Bob Lin <boblin@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Fix triton env marker (#1887)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix triton env marker 1887]


```

---

## 2023-11-13T17:43:42Z -- Relax triton requirements for compatibility with pytorch 2.1 and newer (#1802) (`1cea4357`)

**Author:** Eugene Indenbom <eugeneindenbom@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Relax triton requirements for compatibility with pytorch 2.1 and newer (#1802)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Relax triton requirements for compatibility with pytorch 2.1 and newer 1802]


```

---

## 2023-11-06T18:14:04Z -- Release 20231106 (`fcfeaf1b`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Release 20231106

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Release 20231106]


```

---

## 2023-11-06T18:10:30Z -- large-v3 (#1761) (`c5d42560`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
large-v3 (#1761)
* mel_filters() loads 128 mel bins

* can load 100-language models

* large-v3 checkpoint and evals

* add mandarin alias

* remove unused path

* flake8 fix

* formatting fix
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[large-v3 1761]


```

---

## 2023-11-06T11:08:56Z -- Release 20231105 (`f6f01c56`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Release 20231105

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Release 20231105]


```

---

## 2023-11-06T11:05:21Z -- remove tiktoken pin (#1759) (`746aaaea`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
remove tiktoken pin (#1759)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[remove tiktoken pin 1759]


```

---

## 2023-11-06T10:43:07Z -- docs: Disambiguation of the term "relative speed" in the README (#1751) (`b9f17e1f`)

**Author:** Philippe Hebert <philippehebert@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
docs: Disambiguation of the term "relative speed" in the README (#1751)
* docs: defines relative speed in README

* combined paragraphs

---------

Co-authored-by: Jong Wook Kim <jongwook@nyu.edu>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[docs: Disambiguation of the term relative speed in the README 1751]


```

---

## 2023-11-06T10:28:51Z -- allow_pickle=False while loading of mel matrix IN audio.py (#1511) (`7dfcd563`)

**Author:** Mohamad Zamini <mohamadzamini@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
allow_pickle=False while loading of mel matrix IN audio.py (#1511)
* Update audio.py

The `mel_filters` function is using a `np.load` function to load a pre-computed mel filterbank matrix. This function is not thread-safe, which means that if it is called from multiple threads at the same time, it may corrupt the data.

To fix this, you can use the `torch.load` function instead. This function is thread-safe, so it will not corrupt the data if it is called from multiple threads at the same time.

* Update audio.py

updated the docstring

* allow_pickle=False

* newline

---------

Co-authored-by: Jong Wook Kim <jongwook@nyu.edu>
Co-authored-by: Jong Wook Kim <jongwook@openai.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[allow_pickleFalse while loading of mel matrix IN audio.py 1511]


```

---

## 2023-11-06T10:06:19Z -- handling transcribe exceptions. (#1682) (`b7d277ac`)

**Author:** Marco Zucconelli <marcozucconelli@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
handling transcribe exceptions. (#1682)
* handling transcribe() exceptions.

* printing stacktrace

---------

Co-authored-by: invalid <invalid@email.com>
Co-authored-by: Jong Wook Kim <jongwook@nyu.edu>
Co-authored-by: Jong Wook Kim <jongwook@openai.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[handling transcribe exceptions. 1682]


```

---

## 2023-11-06T09:49:33Z -- Add new option to generate subtitles by a specific number of words (#1729) (`6ed314fe`)

**Author:** amosal <amosal@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Add new option to generate subtitles by a specific number of words (#1729)
* ADD parser for new argument --max_words_count

* ADD max_words_count in words_options
ADD warning for max_line_width compatibility

* ADD logic for max_words_count

* rename to max_words_per_line

* make them kwargs

* allow specifying file path by --model

* black formatting

---------

Co-authored-by: Jong Wook Kim <jongwook@nyu.edu>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Add new option to generate subtitles by a specific number of words 1729]


```

---

## 2023-10-10T17:01:01Z -- Fix exception when an audio file with no speech is provided (#1396) (`b38a1f20`)

**Author:** Jordi Mas <jordimas@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Fix exception when an audio file with no speech is provided (#1396)
Co-authored-by: Jong Wook Kim <jongwook@openai.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix exception when an audio file with no speech is provided 1396]


```

---

## 2023-09-18T23:38:17Z -- Update test.yml (`5f957da5`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Update test.yml

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update test.yml]


```

---

## 2023-09-18T23:15:33Z -- Add .pre-commit-config.yaml (#1528) (`8b330df0`)

**Author:** Arthur Kim <arthurkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Add .pre-commit-config.yaml (#1528)
* Add .pre-commit-config.yaml

Co-authored-by: arthur <arthur@rtzr.ai>

* flake8 E741

---------

Co-authored-by: Jong Wook Kim <jongwook@openai.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Add .pre-commit-config.yaml 1528]


```

---

## 2023-09-18T23:09:59Z -- fix doc of TextDecoder (#1526) (`21010ef4`)

**Author:** sqhao <sqhao@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
fix doc of TextDecoder (#1526)
Signed-off-by: haoshengqiang <haoshengqiang@xiaohongshu.com>
Co-authored-by: haoshengqiang <haoshengqiang@xiaohongshu.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[fix doc of TextDecoder 1526]


```

---

## 2023-08-07T21:48:56Z -- word timing tweaks (#1559) (`e8622f9a`)

**Author:** taylorchu <taylorchu@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
word timing tweaks (#1559)
* word timing tweaks

* comment on eot

* clearer comments
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[word timing tweaks 1559]


```

---

## 2023-07-06T19:48:08Z -- Avoid rearranging all caches (#1483) (`b91c9076`)

**Author:** WangChou Lu <wangchoulu@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Avoid rearranging all caches (#1483)
* avoid rearranging all kv_caches

* avoid calculating the same kv_cache from cross attn

* Update decoding.py

* linter fix

---------

Co-authored-by: Jong Wook Kim <jongwook@openai.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Avoid rearranging all caches 1483]


```

---

## 2023-06-29T23:51:24Z -- Improve timestamp heuristics. (#1461) (`f572f216`)

**Author:** ryanheise <ryanheise@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Improve timestamp heuristics. (#1461)
* Improve timestamp heuristics.

* Track pauses with last_speech_timestamp
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Improve timestamp heuristics. 1461]


```

---

## 2023-05-05T07:31:35Z -- fix condition_on_previous_text (#1224) (`248b6cb1`)

**Author:** Valentin Berkes <valentinberkes@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
fix condition_on_previous_text (#1224)
prompt_reset_since is set before all_tokens is extended hence does not have the expected effect.
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[fix condition_on_previous_text 1224]


```

---

## 2023-05-05T00:02:36Z -- Avoid computing higher temperatures on no_speech segments (#1279) (`e334ff14`)

**Author:** Théo BOYER <thoboyer@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Avoid computing higher temperatures on no_speech segments (#1279)
* Avoid computing higher temperatures on no_speech

In decode_with_fallback, we compute higher temperatures in the case where compression_ratio is too high or avg_logprob is too low.
But as the computation of no_speech_prob doens't depend on sampling, we can avoid computing higher temperatures if we detect in the first one that the no_speech condition is fulfilled

* Update transcribe.py

---------

Co-authored-by: Jong Wook Kim <jongwook@openai.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Avoid computing higher temperatures on no_speech segments 1279]


```

---

## 2023-05-04T17:58:56Z -- Dropped unused execute bit from mel_filters.npz. (#1254) (`55237228`)

**Author:** petterreinholdtsen <petterreinholdtsen@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Dropped unused execute bit from mel_filters.npz. (#1254)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Dropped unused execute bit from mel_filters.npz. 1254]


```

---

## 2023-05-04T17:53:59Z -- Drop ffmpeg-python dependency and call ffmpeg directly. (#1242) (`8035e9ef`)

**Author:** petterreinholdtsen <petterreinholdtsen@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Drop ffmpeg-python dependency and call ffmpeg directly. (#1242)
* Drop ffmpeg-python dependency and call ffmpeg directly.

The last ffmpeg-python module release was in 2019[1], upstream seem to be
unavailable[2] and the project development seem to have stagnated[3].  As
the features it provide is trivial to replace using the Python native
subprocess module, drop the dependency.

[1] <URL: https://github.com/kkroening/ffmpeg-python/tags >
[2] <URL: https://github.com/kkroening/ffmpeg-python/issues/760 >
[3] <URL: https://openhub.net/p/ffmpeg-python >

* Rewrote to use subprocess.run() instead of subprocess.Popen().

* formatting changes

* formatting update

* isort fix

* Error checking

* isort 🤦🏻

* flake8 fix

* minor spelling changes

---------

Co-authored-by: Jong Wook Kim <jongwook@openai.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Drop ffmpeg-python dependency and call ffmpeg directly. 1242]


```

---

## 2023-05-04T17:42:09Z -- Python 3.11 (#1171) (`e69930cb`)

**Author:** Johnny <johnny@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Python 3.11 (#1171)
* python 3.11

* python 3.11

* fix

* fix

* fix

* revert changes

* Update requirements.txt

* Trying pip3 install instead

* Excluding cp39 - torch 1.10.2

* Removing 1.10.2 from test

---------

Co-authored-by: Jong Wook Kim <jongwook@openai.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Python 3.11 1171]


```

---

## 2023-04-11T22:13:13Z -- Update decoding.py (#1219) (`c09a7ae2`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Update decoding.py (#1219)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update decoding.py 1219]


```

---

## 2023-04-11T22:06:03Z -- Update decoding.py (#1155) (`b0022b32`)

**Author:** Fernando O. Gallego <fernandoogallego@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Update decoding.py (#1155)
* Update decoding.py

Following the suggestions of @Jeronymous in https://github.com/openai/whisper/pull/914 and https://github.com/openai/whisper/discussions/924, it solves the problem of endless loop.

* Removed blank line and whitespaces in empty lines.

* Suggested changes according to the linter

---------

Co-authored-by: Jong Wook Kim <jongwook@openai.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update decoding.py 1155]


```

---

## 2023-04-11T00:39:17Z -- Update README.md to reference tiktoken (#1105) (`76c901ab`)

**Author:** Arseniy Bushyn <arseniybushyn@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Update README.md to reference tiktoken (#1105)
Co-authored-by: Jong Wook Kim <jongwook@openai.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update README.md to reference tiktoken 1105]


```

---

## 2023-04-11T00:28:35Z -- Implement max line width and max line count, and make word highlighting optional (#1184) (`43940fc9`)

**Author:** ryanheise <ryanheise@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Implement max line width and max line count, and make word highlighting optional (#1184)
* Add highlight_words, max_line_width, max_line_count

* Refactor subtitle generator

---------

Co-authored-by: Jong Wook Kim <jongwook@openai.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Implement max line width and max line count and make word highlighting optional 1184]


```

---

## 2023-04-10T20:54:09Z -- python-publish.yml: bump actions version to fix node warning (#1211) (`a151816b`)

**Author:** K.B.Dharun Krishna <kbdharunkrishna@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
python-publish.yml: bump actions version to fix node warning (#1211)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[python-publish.yml: bump actions version to fix node warning 1211]


```

---

## 2023-03-15T07:39:05Z -- Release 20230314 (`6dea21fd`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Release 20230314

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Release 20230314]


```

---

## 2023-03-14T19:47:58Z -- abort find_alignment on empty input (#1090) (`79c43e48`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
abort find_alignment on empty input (#1090)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[abort find_alignment on empty input 1090]


```

---

## 2023-03-14T16:32:41Z -- Fix truncated words list when the replacement character is decoded (#1089) (`5f9ac653`)

**Author:** Guillaume Klein <guillaumeklein@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Fix truncated words list when the replacement character is decoded (#1089)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix truncated words list when the replacement character is decoded 1089]


```

---

## 2023-03-08T23:48:57Z -- Release 20230308 (`ad3250a8`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Release 20230308

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Release 20230308]


```

---

## 2023-03-08T23:46:38Z -- kwargs in decode() for convenience (#1061) (`c4b50c08`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
kwargs in decode() for convenience (#1061)
* kwargs in decode() for convenience

* formatting fix
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[kwargs in decode for convenience 1061]


```

---

## 2023-03-08T23:34:07Z -- fix all_tokens handling that caused more repetitions and discrepancy in JSON (#1060) (`38f2f4d9`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
fix all_tokens handling that caused more repetitions and discrepancy in JSON (#1060)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[fix all_tokens handling that caused more repetitions and discrepancy in JSON 1060]


```

---

## 2023-03-07T19:31:40Z -- Try installing triton only if linux & x86_64 (#1051) (`924e1f8e`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Try installing triton only if linux & x86_64 (#1051)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Try installing triton only if linux  x86_64 1051]


```

---

## 2023-03-07T12:47:46Z -- Update setup.py (`4b0d5e58`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Update setup.py

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update setup.py]


```

---

## 2023-03-07T02:50:41Z -- Release 20230306 (`8180fde9`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Release 20230306

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Release 20230306]


```

---

## 2023-03-07T01:48:14Z -- remove auxiliary audio extension (#1021) (`c6e4e5ef`)

**Author:** Local State <localstate@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
remove auxiliary audio extension (#1021)
Co-authored-by: Jong Wook Kim <jongwook@openai.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[remove auxiliary audio extension 1021]


```

---

## 2023-03-06T23:50:37Z -- apply formatting with `black` (#1038) (`b80bcf61`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
apply formatting with `black` (#1038)
* applying black (with the default 88-column limit)

* add flake8

* add isort

* fix isort
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[apply formatting with black 1038]


```

---

## 2023-03-06T22:00:49Z -- word-level timestamps in `transcribe()` (#869) (`500d0fe9`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
word-level timestamps in `transcribe()` (#869)
* word-level timestamps in `transcribe()`

* moving to `timing.py`

* numba implementation for dtw, replacing dtw-python

* triton implementation for dtw

* add test for dtw implementations

* triton implementation of median_filter

* a simple word-level timestamps test

* add scipy as dev dependency

* installs an older version of Triton if CUDA < 11.4

* fix broken merge

* loosen nvcc version match regex

* find_alignment() function

* miscellaneous improvements

* skip median filtering when the input is too small

* Expose punctuation options in cli and transcribe() (#973)

* fix merge error

* fix merge error 2

* annotating that word_timestamps is experimental

---------

Co-authored-by: ryanheise <ryan@ryanheise.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[word-level timestamps in transcribe 869]


```

---

## 2023-03-06T19:32:32Z -- Decoding improvements (#1033) (`eab8d920`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Decoding improvements (#1033)
* suppress task tokens (transcribe/translate)

* not ignoring the last segment ending with one timestamp
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Decoding improvements 1033]


```

---

## 2023-03-04T00:41:59Z -- Update README.md (#894) (`3e1780fd`)

**Author:** Roman Vasilenko <romanvasilenko@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Update README.md (#894)
Fixed a few typos and made general improvements for clarity.

Co-authored-by: Jong Wook Kim <jongwook@openai.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update README.md 894]


```

---

## 2023-02-01T23:46:51Z -- Fix infinite loop caused by incorrect timestamp tokens prediction (#914) (`7858aa9c`)

**Author:** Andrey Chernykh <andreychernykh@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Fix infinite loop caused by incorrect timestamp tokens prediction (#914)
* Fix infinite loop caused by incorrect timestamp tokens prediction

https://github.com/openai/whisper/discussions/810

* Update decoding.py

---------

Co-authored-by: Jong Wook Kim <jongwook@openai.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix infinite loop caused by incorrect timestamp tokens prediction 914]


```

---

## 2023-01-24T22:45:56Z -- Update README.md about Python 3.8+ requirement (`4e635c66`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Update README.md about Python 3.8+ requirement

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update README.md about Python 3.8 requirement]


```

---

## 2023-01-24T22:05:57Z -- drop python 3.7 support (#889) (`a6b36ede`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
drop python 3.7 support (#889)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[drop python 3.7 support 889]


```

---

## 2023-01-24T19:11:08Z -- Release 20230124 (`55f690af`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Release 20230124

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Release 20230124]


```

---

## 2023-01-24T18:12:04Z -- handle printing even if sys.stdout.buffer is not available (#887) (`7f1ef223`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
handle printing even if sys.stdout.buffer is not available (#887)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[handle printing even if sys.stdout.buffer is not available 887]


```

---

## 2023-01-22T08:27:17Z -- Add TSV formatted output in transcript, using integer start/end times in milliseconds. (#228) (`f5bfe004`)

**Author:** Niels Mayer <nielsmayer@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Add TSV formatted output in transcript, using integer start/end times in milliseconds. (#228)
* Add CSV format output in transcript, containing lines of characters formatted like: <startTime-in-integer-milliseconds>, <endTime-in-integer-milliseconds>, <transcript-including-commas>

* for easier reading by spreadsheets importing CSV, the third

column of the CSV file is delimited by quotes, and any quote
characters that might be in the transcript (which would interfere with
parsing the third column as a string) are converted to "''".

* fix syntax error

* docstring edit

Co-authored-by: Jong Wook Kim <jongwook@openai.com>
Co-authored-by: Jong Wook Kim <jongwook@nyu.edu>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Add TSV formatted output in transcript using integer start/end times in milliseconds. 228]


```

---

## 2023-01-22T07:58:38Z -- Added --output_format option (#333) (`da600abd`)

**Author:** Aaryan YVS <aaryanyvs@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Added --output_format option (#333)
* Added --output option

--output option will help select the output files that will be generated.

Corrected the logic, which wrongly shows progress bar when verbose is set to False

* Changed output_files variable

* Changed back the tqdm verbose

* refactor output format handling

Co-authored-by: Jong Wook Kim <jongwook@openai.com>
Co-authored-by: Jong Wook Kim <jongwook@nyu.edu>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Added --output_format option 333]


```

---

## 2023-01-21T09:09:39Z -- Handle XDG_CACHE_HOME properly for download_root (#864) (`9f7aba60`)

**Author:** zer0-x <zer0x@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Handle XDG_CACHE_HOME properly for download_root (#864)
Co-authored-by: Jong Wook Kim <jongwook@openai.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Handle XDG_CACHE_HOME properly for download_root 864]


```

---

## 2023-01-20T08:54:05Z -- use stdout for printing transcription progress (#867) (`12e10894`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
use stdout for printing transcription progress (#867)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[use stdout for printing transcription progress 867]


```

---

## 2023-01-18T18:41:11Z -- Fix bug where mm is mistakenly replaced with hmm in e.g. 20mm (#659) (`ea1c2667`)

**Author:** Markus Hennerbichler <markushennerbichler@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Fix bug where mm is mistakenly replaced with hmm in e.g. 20mm (#659)
Co-authored-by: Jong Wook Kim <jongwook@nyu.edu>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix bug where mm is mistakenly replaced with hmm in e.g. 20mm 659]


```

---

## 2023-01-18T07:28:36Z -- print '?' if a letter can't be encoded using the system default encoding (#859) (`9d646db9`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
print '?' if a letter can't be encoded using the system default encoding (#859)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[print  if a letter cant be encoded using the system default encoding 859]


```

---

## 2023-01-18T00:08:28Z -- Release 20230117 (`37a4f1be`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Release 20230117

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Release 20230117]


```

---

## 2023-01-17T23:50:26Z -- Add github action to automatically push to pypi on Release x.y.z commit (#681) (`b9f9b433`)

**Author:** Romain Beaumont <romainbeaumont@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Add github action to automatically push to pypi on Release x.y.z commit (#681)
* Add github action to automatically push to pypi on Release x.y.z commit

* some housekeeping for pypi upload

* add version.py

Co-authored-by: Jong Wook Kim <jongwook@nyu.edu>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Add github action to automatically push to pypi on Release x.y.z commit 681]


```

---

## 2023-01-17T22:43:05Z -- Use ndimage.median_filter instead of signal.medfilter (#812) (`f0083e7e`)

**Author:** Umar Farooqi <umarfarooqi@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Use ndimage.median_filter instead of signal.medfilter (#812)
For a 30s long audio file which didn't have any silence, ndimage.median_filter took 7s where signa.medfilter took 30s.

Co-authored-by: Umar Farooqi <umar@paystash.com>
Co-authored-by: Jong Wook Kim <jongwook@nyu.edu>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Use ndimage.median_filter instead of signal.medfilter 812]


```

---

## 2023-01-17T21:54:40Z -- rename GitHub workflow (`a84191fa`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
rename GitHub workflow

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[rename GitHub workflow]


```

---

## 2023-01-17T21:43:36Z -- allow test_transcribe to run on CPU when CUDA is not available (`b1d213c0`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
allow test_transcribe to run on CPU when CUDA is not available

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[allow test_transcribe to run on CPU when CUDA is not available]


```

---

## 2023-01-17T21:35:48Z -- add github action to run pytest (`493dfffa`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
add github action to run pytest

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[add github action to run pytest]


```

---

## 2023-01-17T07:46:42Z -- Update README.md (#804) (`0f39c89d`)

**Author:** Mikko Vedru <mikkovedru@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Update README.md (#804)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update README.md 804]


```

---

## 2023-01-17T07:46:15Z -- Support batch-dimension in log_mel_spectogram (#839) (`6df3ea1f`)

**Author:** Markus Hennerbichler <markushennerbichler@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Support batch-dimension in log_mel_spectogram (#839)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Support batch-dimension in log_mel_spectogram 839]


```

---

## 2023-01-17T06:42:01Z -- Fix tiny transcribe() docstring typo (#857) (`70861c7c`)

**Author:** adamreis <adamreis@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Fix tiny transcribe() docstring typo (#857)
s/successfully/successively, which I believe was the intent.
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix tiny transcribe docstring typo 857]


```

---

## 2022-12-31T17:03:42Z -- word-level timestamps in Multilingual_ASR notebook (`28769fcf`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
word-level timestamps in Multilingual_ASR notebook

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[word-level timestamps in Multilingual_ASR notebook]


```

---

## 2022-12-30T08:53:06Z -- MultiHeadAttention to return qk as well (`53807677`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
MultiHeadAttention to return qk as well

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[MultiHeadAttention to return qk as well]


```

---

## 2022-12-30T06:53:31Z -- Revert "saving the qk matrix in the attention module for convenience" (`9323b252`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Revert "saving the qk matrix in the attention module for convenience"
This reverts commit 68e44bd83ce6c3e352f74b266aa39d8b649af9e3.
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Revert saving the qk matrix in the attention module for convenience]


```

---

## 2022-12-09T05:12:39Z -- large-v2 figure and arxiv url update (`0b5dcfde`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
large-v2 figure and arxiv url update

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[large-v2 figure and arxiv url update]


```

---

## 2022-12-07T18:45:31Z -- Update Hebrew language code to he per IANA registry (#401) (`b9265e57`)

**Author:** altryne <altryne@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Update Hebrew language code to he per IANA registry (#401)
* Update Hebrew language code to he per IANA registry

Per [IANA registry](https://www.iana.org/assignments/language-subtag-registry/language-subtag-registry), `iw` was deprecated as the code for Hebrew in 1989 and the preferred code is `he`

The correct subtag:
```
%%
Type: language
Subtag: he
Description: Hebrew
Added: 2005-10-16
Suppress-Script: Hebr
%%
```
And the deprecation
```
%%
Type: language
Subtag: iw
Description: Hebrew
Added: 2005-10-16
Deprecated: 1989-01-01
Preferred-Value: he
Suppress-Script: Hebr
%%
```

* Update hebrew ISO code to he

Per discussion, it's ok to make this change without backwards compatibility
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update Hebrew language code to he per IANA registry 401]


```

---

## 2022-12-06T17:07:19Z -- Explicitly closing model file after reading it (#630) (`fd8f80c8`)

**Author:** Paul Harter <paulharter@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Explicitly closing model file after reading it (#630)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Explicitly closing model file after reading it 630]


```

---

## 2022-12-05T16:07:14Z -- add large-v2 model (`4179ed24`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
add large-v2 model
- The "large-v2" model is trained for more epochs with regularization and shows improved performance compared to the previous large.
- It has the same architecture as the original large model.
- When `load_model("large")` is called, the "large-v2" model will be loaded.
- We will soon update the paper regarding this new model.
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[add large-v2 model]


```

---

## 2022-12-04T23:27:42Z -- fix compression ratio function (#561) (`ec1b34bb`)

**Author:** jumon <jumon@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
fix compression ratio function (#561)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[fix compression ratio function 561]


```

---

## 2022-11-16T00:25:11Z -- fix to return only the text token ids (`02aa851a`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
fix to return only the text token ids

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[fix to return only the text token ids]


```

---

## 2022-10-19T23:44:03Z -- Fix attention caching to make it actually work (#370) (`9f70a352`)

**Author:** Vicki Anand <vickianand@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Fix attention caching to make it actually work (#370)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix attention caching to make it actually work 370]


```

---

## 2022-10-17T18:38:20Z -- Fix bug (#305) (`f6805700`)

**Author:** Michael Monashev <michaelmonashev@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Fix bug (#305)
Fix bug: RuntimeError: Expected all tensors to be on the same device, but found at least two devices, cuda:0 and cpu! (when checking argument for argument index in method wrapper__index_select)
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix bug 305]


```

---

## 2022-10-09T09:14:03Z -- infer download_root from XDG_CACHE_HOME if avail (#257) (`82725cea`)

**Author:** David Marx <davidmarx@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
infer download_root from XDG_CACHE_HOME if avail (#257)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[infer download_root from XDG_CACHE_HOME if avail 257]


```

---

## 2022-10-09T09:11:15Z -- Add --threads option to transcribe (#278) (`35713c66`)

**Author:** eudoxos <eudoxos@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Add --threads option to transcribe (#278)
* Add --threads option to transcribe

Torch on CPU uses by default number_of_cores/2. This option allows to
override this default.

* Update transcribe.py

Co-authored-by: Jong Wook Kim <ilikekjw@gmail.com>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Add --threads option to transcribe 278]


```

---

## 2022-10-04T15:49:31Z -- Fixed CoW RuntimeError in DecodingTask.run() (#240) (`9e653bd0`)

**Author:** Corentin Jemine <corentinjemine@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Fixed CoW RuntimeError in DecodingTask.run() (#240)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fixed CoW RuntimeError in DecodingTask.run 240]


```

---

## 2022-09-30T03:44:12Z -- Use , character instead of . for SRT output. (#197) (`60132ade`)

**Author:** Caleb McQuillin <calebmcquillin@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Use , character instead of . for SRT output. (#197)
The SRT format uses the decimal comma character as the fractional separator rather than the decimal point character. Adjust format_timestamp and write_srt to specify the separator character.

See https://en.wikipedia.org/wiki/SubRip#:~:text=the%20fractional%20separator%20used%20is%20the%20comma%2C%20since%20the%20program%20was%20written%20in%20france.
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Use  character instead of . for SRT output. 197]


```

---

## 2022-09-30T01:05:12Z -- allowing nonzero initial temperature (`7cb4cc21`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
allowing nonzero initial temperature

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[allowing nonzero initial temperature]


```

---

## 2022-09-29T21:57:12Z -- pointer to the show and tell section (`30dc5c58`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
pointer to the show and tell section

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[pointer to the show and tell section]


```

---

## 2022-09-29T21:18:54Z -- Update README.md (#161) (`5905e503`)

**Author:** Szabolcs Pasztor <szabolcspasztor@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Update README.md (#161)
* Update README.md

* merging paragraphs

Co-authored-by: Jong Wook Kim <jongwook@nyu.edu>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update README.md 161]


```

---

## 2022-09-29T21:08:58Z -- Adds missing command for install (mac) (#90) (`0457aac3`)

**Author:** Fabiano <fabiano@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Adds missing command for install (mac) (#90)
* Adds missing command for install (mac)

Required for users who didn't previously have Rust installed.

* minor wording change

Co-authored-by: Jong Wook Kim <jongwook@nyu.edu>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Adds missing command for install mac 90]


```

---

## 2022-09-29T19:34:04Z -- Update audio.py (#178) (`deafef05`)

**Author:** sawadata <sawadata@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Update audio.py (#178)
add '-nostdin' argument
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update audio.py 178]


```

---

## 2022-09-29T19:27:48Z -- Don't update duration if last timestamp is same as begin (#191) (`2b0c2971`)

**Author:** Vicki Anand <vickianand@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Don't update duration if last timestamp is same as begin (#191)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Dont update duration if last timestamp is same as begin 191]


```

---

## 2022-09-28T02:00:41Z -- patience definition to match the paper (`62fe7f10`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
patience definition to match the paper

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[patience definition to match the paper]


```

---

## 2022-09-26T18:46:21Z -- fix: transcribe verbosity (#140) (`b4308c47`)

**Author:** Nick Konovalchuk <nickkonovalchuk@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
fix: transcribe verbosity (#140)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[fix: transcribe verbosity 140]


```

---

## 2022-09-26T12:22:33Z -- Context prompt (#128) (`2037b65f`)

**Author:** VulumeCode <vulumecode@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Context prompt (#128)
Co-authored-by: Jong Wook Kim <jongwook@nyu.edu>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Context prompt 128]


```

---

## 2022-09-26T11:52:28Z -- Write each sentence as a separate line for the txt output (#101) (`fc0f4098`)

**Author:** EliEron <elieron@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Write each sentence as a separate line for the txt output (#101)
* Write each sentence as a separate line for the txt output

Write each sentence as a separate line for the txt output

* Update utils.py

Co-authored-by: EliEron <example@example.com>
Co-authored-by: Jong Wook Kim <jongwook@nyu.edu>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Write each sentence as a separate line for the txt output 101]


```

---

## 2022-09-26T11:35:21Z -- fix token suppression (#123) (`520796a3`)

**Author:** VulumeCode <vulumecode@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
fix token suppression (#123)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[fix token suppression 123]


```

---

## 2022-09-26T10:24:47Z -- arch linux ffmpeg install (#93) (`5485428c`)

**Author:** Ashutosh Tripathi <ashutoshtripathi@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
arch linux ffmpeg install (#93)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[arch linux ffmpeg install 93]


```

---

## 2022-09-26T10:24:13Z -- add progress bar for transcribe loop (#100) (`9e7e418f`)

**Author:** fatih <fatih@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
add progress bar for transcribe loop (#100)
* add progress bar to transcribe loop

* improved warning message for English-only models

* add --condition_on_previous_text

* progressbar renames

Co-authored-by: Jong Wook Kim <jongwook@nyu.edu>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[add progress bar for transcribe loop 100]


```

---

## 2022-09-25T12:16:08Z -- add --condition_on_previous_text (`5d8d3e75`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
add --condition_on_previous_text

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[add --condition_on_previous_text]


```

---

## 2022-09-25T09:10:36Z -- improved warning message for English-only models (`2d3032de`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
improved warning message for English-only models

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[improved warning message for English-only models]


```

---

## 2022-09-23T11:11:27Z -- allow hyphens and single quotes between words (`8cf36f35`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
allow hyphens and single quotes between words

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[allow hyphens and single quotes between words]


```

---

## 2022-09-23T06:45:32Z -- nocaptions -> nospeech to match the paper figure (`15ab5482`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
nocaptions -> nospeech to match the paper figure

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[nocaptions - nospeech to match the paper figure]


```

---

## 2022-09-23T06:21:47Z -- Fix possible mistake when loading model to device (#57) (`61989529`)

**Author:** mj-kh <mjkh@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Fix possible mistake when loading model to device (#57)
Before this change, the model is loaded into GPU regardless of the value of "device" argument in CLI.

(e.g. whisper "test.wav" --device cpu loads into GPU anyway)
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix possible mistake when loading model to device 57]


```

---

## 2022-09-23T03:30:45Z -- Add conda environment.yml (and fix requirements.txt) (#8) (`a4fe05aa`)

**Author:** Sidney Radcliffe <sidneyradcliffe@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Add conda environment.yml (and fix requirements.txt) (#8)
* fix: more-itertools name in requirements.txt

* feature: minimal environment.yml for conda

* Revert "feature: minimal environment.yml for conda"

This reverts commit 8fd7438b368b0eb5df85f667fea911f293fa5e6d.

Co-authored-by: Jong Wook Kim <jongwook@nyu.edu>
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Add conda environment.yml and fix requirements.txt 8]


```

---

## 2022-09-23T03:12:37Z -- Fix exception cause in audio.py (#33) (`59f543e2`)

**Author:** Ram Rachum <ramrachum@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Fix exception cause in audio.py (#33)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix exception cause in audio.py 33]


```

---

## 2022-09-23T02:38:37Z -- Fix output_dir argument when audio file is a path (#45) (`759e8d47`)

**Author:** EliEron <elieron@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Fix output_dir argument when audio file is a path (#45)

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix output_dir argument when audio file is a path 45]


```

---

## 2022-09-22T02:51:05Z -- Merge pull request #14 from bquast/patch-1 (`e90b8fa7`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
Merge pull request #14 from bquast/patch-1
make LICENSE a link instead of code-formatted text
```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Merge pull request 14 from bquast/patch-1]


```

---

## 2022-09-21T20:36:25Z -- add section Available models and languages (`49a3ffc9`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
add section Available models and languages

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[add section Available models and languages]


```

---

## 2022-09-21T17:58:56Z -- a note on speed-accuracy tradeoffs (`cfd6bdda`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
a note on speed-accuracy tradeoffs

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[a note on speed-accuracy tradeoffs]


```

---

## 2022-09-21T17:45:03Z -- making small model the default (`834f00a0`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
making small model the default

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[making small model the default]


```

---

## 2022-09-21T15:43:20Z -- initial commit (`6e3be77e`)

**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com>

**Files touched:**
- `whisper.ts`

**Commit message:**
```
initial commit

```

**Diff:**
```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[initial commit]


```

---

