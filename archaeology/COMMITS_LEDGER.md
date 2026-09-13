# Complete Commit Ledger

Total Commits Analyzed: 171

### [OK] 86098128 - `\b` also matches next to ".", "$", "%" and "-", so the readability rule
**Author:** Unknown | **Date:** Mon, 31 Aug 2026 10:19:19 -0700

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

### [OK] 5f86d1d8 - Co-authored-by: Dorijan Magasic <dorijan.magasic@example.com>
**Author:** Unknown | **Date:** Tue, 28 Jul 2026 21:18:29 +0100

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

### [OK] 90fdc511 - This adds --output_format jsonl to the CLI, producing a .jsonl file
**Author:** Unknown | **Date:** Wed, 29 Jul 2026 00:35:58 +0530

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

### [OK] 04f449b8 - * Pin pre-commit hook revisions to immutable commits
**Author:** Unknown | **Date:** Wed, 15 Apr 2026 12:32:15 -0400

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

### [OK] cba37681 - ---
**Author:** Unknown | **Date:** Fri, 27 Mar 2026 16:08:20 -0500

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

### [OK] c0d2f624 - ---
**Author:** Unknown | **Date:** Wed, 25 Jun 2025 18:05:47 -0700

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

### [OK] db7fbc75 - ---
**Author:** Unknown | **Date:** Wed, 25 Jun 2025 18:02:39 -0700

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

### [OK] 31243bad - ---
**Author:** Unknown | **Date:** Wed, 25 Jun 2025 18:00:48 -0700

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

### [OK] 1f8fc975 - * Fix: Update torch.load to use weights_only=True to prevent security warning
**Author:** Unknown | **Date:** Thu, 26 Jun 2025 02:54:30 +0200

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

### [OK] 679ae1d1 - Co-authored-by: Jong Wook Kim <jongwook@openai.com>
**Author:** Unknown | **Date:** Wed, 25 Jun 2025 18:42:09 -0600

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

### [OK] f50c4f26 - Updated README given info from https://github.com/openai/whisper/discussions/2483
**Author:** Unknown | **Date:** Wed, 25 Jun 2025 20:03:47 -0400

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

### [OK] 86899243 - * Update triton kernel using _unsafe_update_src
**Author:** Unknown | **Date:** Thu, 26 Jun 2025 02:02:54 +0200

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

### [OK] 5dff4db8 - * Update LibriSpeech.ipynb
**Author:** Unknown | **Date:** Thu, 26 Jun 2025 03:55:15 +0400

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

### [OK] dd985ac4 - Bumps the github-actions group with 3 updates: [actions/checkout](https://github.com/actions/checkout), [actions/setup-python](https://github.com/actions/setup-python) and [softprops/action-gh-release](https://github.com/softprops/action-gh-release).
**Author:** Unknown | **Date:** Tue, 13 May 2025 11:22:31 -0700

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

### [OK] e1e6aa60 - Automates the creation of pull requests like
**Author:** Unknown | **Date:** Tue, 13 May 2025 20:10:43 +0200

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

### [OK] e6a5fc0f - ---
**Author:** Unknown | **Date:** Tue, 13 May 2025 18:43:34 +0200

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

### [OK] 13907bed - * GitHub Actions: Add Python 3.13 to the testing
**Author:** Unknown | **Date:** Tue, 13 May 2025 06:10:40 +0200

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

### [OK] 517a43ec - using `-m build --sdist` instead of `setup.py sdist`
**Author:** Unknown | **Date:** Sat, 4 Jan 2025 12:56:16 -0800

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

### [OK] dd4d010d - ---
**Author:** Unknown | **Date:** Sat, 4 Jan 2025 10:38:35 +0100

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

### [OK] 26a7cacc - * pre-commit autoupdate && pre-commit run --all-files
**Author:** Unknown | **Date:** Sat, 4 Jan 2025 10:02:18 +0100

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

### [OK] 6c1d8f1e - ---
**Author:** Unknown | **Date:** Sat, 4 Jan 2025 09:47:12 +0100

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

### [OK] 90db0de1 - * Bugfix: Illogical "Avoid computing higher temperatures on no_speech"
**Author:** Unknown | **Date:** Sun, 1 Dec 2024 05:47:01 +0000

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

### [OK] fc5ded7d - ---
**Author:** Unknown | **Date:** Tue, 26 Nov 2024 09:37:01 -0800

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

### [OK] 173ff7dd - ---
**Author:** Unknown | **Date:** Wed, 13 Nov 2024 08:35:54 +0800

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

### [OK] 271445b2 - Default now uses Turbo instead of Small
**Author:** Unknown | **Date:** Mon, 4 Nov 2024 08:00:30 +0100

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

### [OK] 5979f037 - Add option to carry initial_prompt with the sliding window (#2343)
**Author:** kittsil <kittsil@users.noreply.github.com> | **Date:** 2024-10-26T14:17:31Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Add option to carry initial_prompt with the sliding window 2343]


```

---

### [OK] cdb81479 - more pytorch versions in tests (#2408)
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2024-10-26T00:30:02Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[more pytorch versions in tests 2408]


```

---

### [OK] 25639fc1 - Release 20240930
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2024-09-30T18:20:53Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Release 20240930]


```

---

### [OK] 260bbcfc - allowing numpy 2 in tests (#2362)
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2024-09-30T18:18:17Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[allowing numpy 2 in tests 2362]


```

---

### [OK] 25e5c364 - large-v3-turbo model (#2361)
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2024-09-30T17:59:51Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[large-v3-turbo model 2361]


```

---

### [OK] b66b46f3 - test on python/pytorch versions up to 3.12 and 2.4.1 (#2360)
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2024-09-30T17:33:56Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[test on python/pytorch versions up to 3.12 and 2.4.1 2360]


```

---

### [OK] 27f97132 - using sdpa if available (#2359)
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2024-09-30T17:27:14Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[using sdpa if available 2359]


```

---

### [OK] 423492dd - Release 20240927
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2024-09-27T23:43:58Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Release 20240927]


```

---

### [OK] 279133e3 - pinning numpy<2 in tests (#2332)
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2024-09-10T17:43:21Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[pinning numpy2 in tests 2332]


```

---

### [OK] 32d55d5d - Relax triton requirements for compatibility with pytorch 2.4 and newer (#2307)
**Author:** Jianan Xing <jiananxing@users.noreply.github.com> | **Date:** 2024-09-10T16:53:08Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Relax triton requirements for compatibility with pytorch 2.4 and newer 2307]


```

---

### [OK] ba3f3cd5 - Skip silence around hallucinations (#1838)
**Author:** ryanheise <ryanheise@users.noreply.github.com> | **Date:** 2023-12-18T20:11:16Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Skip silence around hallucinations 1838]


```

---

### [OK] 8bc88606 - Fix triton env marker (#1887)
**Author:** Bob Lin <boblin@users.noreply.github.com> | **Date:** 2023-12-11T15:39:08Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix triton env marker 1887]


```

---

### [WRONG] e58f2880 - Release 20231117
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-11-17T19:59:28Z
**Note:** Immediately followed by fix commit 8bc88606 ("Fix triton env marker (#1887)") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Release 20231117]


```

---

### [OK] 1cea4357 - Relax triton requirements for compatibility with pytorch 2.1 and newer (#1802)
**Author:** Eugene Indenbom <eugeneindenbom@users.noreply.github.com> | **Date:** 2023-11-13T17:43:42Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Relax triton requirements for compatibility with pytorch 2.1 and newer 1802]


```

---

### [OK] fcfeaf1b - Release 20231106
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-11-06T18:14:04Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Release 20231106]


```

---

### [OK] c5d42560 - large-v3 (#1761)
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-11-06T18:10:30Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[large-v3 1761]


```

---

### [OK] f6f01c56 - Release 20231105
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-11-06T11:08:56Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Release 20231105]


```

---

### [OK] 746aaaea - remove tiktoken pin (#1759)
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-11-06T11:05:21Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[remove tiktoken pin 1759]


```

---

### [OK] b9f17e1f - docs: Disambiguation of the term "relative speed" in the README (#1751)
**Author:** Philippe Hebert <philippehebert@users.noreply.github.com> | **Date:** 2023-11-06T10:43:07Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[docs: Disambiguation of the term relative speed in the README 1751]


```

---

### [OK] 7dfcd563 - allow_pickle=False while loading of mel matrix IN audio.py (#1511)
**Author:** Mohamad Zamini <mohamadzamini@users.noreply.github.com> | **Date:** 2023-11-06T10:28:51Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[allow_pickleFalse while loading of mel matrix IN audio.py 1511]


```

---

### [OK] b7d277ac - handling transcribe exceptions. (#1682)
**Author:** Marco Zucconelli <marcozucconelli@users.noreply.github.com> | **Date:** 2023-11-06T10:06:19Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[handling transcribe exceptions. 1682]


```

---

### [OK] 6ed314fe - Add new option to generate subtitles by a specific number of words (#1729)
**Author:** amosal <amosal@users.noreply.github.com> | **Date:** 2023-11-06T09:49:33Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Add new option to generate subtitles by a specific number of words 1729]


```

---

### [OK] b38a1f20 - Fix exception when an audio file with no speech is provided (#1396)
**Author:** Jordi Mas <jordimas@users.noreply.github.com> | **Date:** 2023-10-10T17:01:01Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix exception when an audio file with no speech is provided 1396]


```

---

### [WRONG] 0a60fcaa - Release 20230918
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-09-19T00:13:19Z
**Note:** Immediately followed by fix commit b38a1f20 ("Fix exception when an audio file with no speech is provided (#1396)") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Release 20230918]


```

---

### [OK] 5f957da5 - Update test.yml
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-09-18T23:38:17Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update test.yml]


```

---

### [OK] 8b330df0 - Add .pre-commit-config.yaml (#1528)
**Author:** Arthur Kim <arthurkim@users.noreply.github.com> | **Date:** 2023-09-18T23:15:33Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Add .pre-commit-config.yaml 1528]


```

---

### [OK] 21010ef4 - fix doc of TextDecoder (#1526)
**Author:** sqhao <sqhao@users.noreply.github.com> | **Date:** 2023-09-18T23:09:59Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[fix doc of TextDecoder 1526]


```

---

### [WRONG] 29b7df62 - Update model-card.md (#1643)
**Author:** Nino Risteski <ninoristeski@users.noreply.github.com> | **Date:** 2023-09-18T22:59:49Z
**Note:** Immediately followed by fix commit 21010ef4 ("fix doc of TextDecoder (#1526)") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update model-card.md 1643]


```

---

### [OK] e8622f9a - word timing tweaks (#1559)
**Author:** taylorchu <taylorchu@users.noreply.github.com> | **Date:** 2023-08-07T21:48:56Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[word timing tweaks 1559]


```

---

### [OK] b91c9076 - Avoid rearranging all caches (#1483)
**Author:** WangChou Lu <wangchoulu@users.noreply.github.com> | **Date:** 2023-07-06T19:48:08Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Avoid rearranging all caches 1483]


```

---

### [OK] f572f216 - Improve timestamp heuristics. (#1461)
**Author:** ryanheise <ryanheise@users.noreply.github.com> | **Date:** 2023-06-29T23:51:24Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Improve timestamp heuristics. 1461]


```

---

### [OK] 248b6cb1 - fix condition_on_previous_text (#1224)
**Author:** Valentin Berkes <valentinberkes@users.noreply.github.com> | **Date:** 2023-05-05T07:31:35Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[fix condition_on_previous_text 1224]


```

---

### [WRONG] 7ca9fbea - Fix numba depreceation notice (#1233)
**Author:** Paul Willot <paulwillot@users.noreply.github.com> | **Date:** 2023-05-05T06:48:06Z
**Note:** Immediately followed by fix commit 248b6cb1 ("fix condition_on_previous_text (#1224)") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix numba depreceation notice 1233]


```

---

### [WRONG] b1c0815c - Updated README.md to provide more insight on BLEU and specific appendices (#1236)
**Author:** Brett Balquist <brettbalquist@users.noreply.github.com> | **Date:** 2023-05-05T06:47:45Z
**Note:** Immediately followed by fix commit 7ca9fbea ("Fix numba depreceation notice (#1233)") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Updated README.md to provide more insight on BLEU and specific appendices 1236]


```

---

### [OK] e334ff14 - Avoid computing higher temperatures on no_speech segments (#1279)
**Author:** Théo BOYER <thoboyer@users.noreply.github.com> | **Date:** 2023-05-05T00:02:36Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Avoid computing higher temperatures on no_speech segments 1279]


```

---

### [OK] 55237228 - Dropped unused execute bit from mel_filters.npz. (#1254)
**Author:** petterreinholdtsen <petterreinholdtsen@users.noreply.github.com> | **Date:** 2023-05-04T17:58:56Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Dropped unused execute bit from mel_filters.npz. 1254]


```

---

### [OK] 8035e9ef - Drop ffmpeg-python dependency and call ffmpeg directly. (#1242)
**Author:** petterreinholdtsen <petterreinholdtsen@users.noreply.github.com> | **Date:** 2023-05-04T17:53:59Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Drop ffmpeg-python dependency and call ffmpeg directly. 1242]


```

---

### [OK] e69930cb - Python 3.11 (#1171)
**Author:** Johnny <johnny@users.noreply.github.com> | **Date:** 2023-05-04T17:42:09Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Python 3.11 1171]


```

---

### [OK] c09a7ae2 - Update decoding.py (#1219)
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-04-11T22:13:13Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update decoding.py 1219]


```

---

### [OK] b0022b32 - Update decoding.py (#1155)
**Author:** Fernando O. Gallego <fernandoogallego@users.noreply.github.com> | **Date:** 2023-04-11T22:06:03Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update decoding.py 1155]


```

---

### [OK] 76c901ab - Update README.md to reference tiktoken (#1105)
**Author:** Arseniy Bushyn <arseniybushyn@users.noreply.github.com> | **Date:** 2023-04-11T00:39:17Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update README.md to reference tiktoken 1105]


```

---

### [OK] 43940fc9 - Implement max line width and max line count, and make word highlighting optional (#1184)
**Author:** ryanheise <ryanheise@users.noreply.github.com> | **Date:** 2023-04-11T00:28:35Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Implement max line width and max line count and make word highlighting optional 1184]


```

---

### [WRONG] 255887f2 - Squash long words at window and sentence boundaries. (#1114)
**Author:** ryanheise <ryanheise@users.noreply.github.com> | **Date:** 2023-04-11T00:23:53Z
**Note:** Self-identified failure / WIP in commit subject ("Squash long words at window and sentence boundaries. (#1114)")

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Squash long words at window and sentence boundaries. 1114]


```

---

### [OK] a151816b - python-publish.yml: bump actions version to fix node warning (#1211)
**Author:** K.B.Dharun Krishna <kbdharunkrishna@users.noreply.github.com> | **Date:** 2023-04-10T20:54:09Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[python-publish.yml: bump actions version to fix node warning 1211]


```

---

### [WRONG] b5851c6c - Update tokenizer.py (#1163)
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-03-29T20:12:36Z
**Note:** Immediately followed by fix commit a151816b ("python-publish.yml: bump actions version to fix node warning (#1211)") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update tokenizer.py 1163]


```

---

### [OK] 6dea21fd - Release 20230314
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-03-15T07:39:05Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Release 20230314]


```

---

### [OK] 79c43e48 - abort find_alignment on empty input (#1090)
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-03-14T19:47:58Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[abort find_alignment on empty input 1090]


```

---

### [OK] 5f9ac653 - Fix truncated words list when the replacement character is decoded (#1089)
**Author:** Guillaume Klein <guillaumeklein@users.noreply.github.com> | **Date:** 2023-03-14T16:32:41Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix truncated words list when the replacement character is decoded 1089]


```

---

### [WRONG] ba88b8e1 - fix github language stats getting dominated by jupyter notebook (#1076)
**Author:** Akash Mahajan <akashmahajan@users.noreply.github.com> | **Date:** 2023-03-14T07:07:09Z
**Note:** Immediately followed by fix commit 5f9ac653 ("Fix truncated words list when the replacement character is decoded (#1089)") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[fix github language stats getting dominated by jupyter notebook 1076]


```

---

### [WRONG] 671ac5a4 - Fix alignment between the segments and the list of words (#1087)
**Author:** Guillaume Klein <guillaumeklein@users.noreply.github.com> | **Date:** 2023-03-13T23:34:09Z
**Note:** Immediately followed by fix commit ba88b8e1 ("fix github language stats getting dominated by jupyter notebook (#1076)") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix alignment between the segments and the list of words 1087]


```

---

### [WRONG] 839639a2 - Use tiktoken (#1044)
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-03-13T09:34:16Z
**Note:** Immediately followed by fix commit 671ac5a4 ("Fix alignment between the segments and the list of words (#1087)") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Use tiktoken 1044]


```

---

### [OK] ad3250a8 - Release 20230308
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-03-08T23:48:57Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Release 20230308]


```

---

### [OK] c4b50c08 - kwargs in decode() for convenience (#1061)
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-03-08T23:46:38Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[kwargs in decode for convenience 1061]


```

---

### [OK] 38f2f4d9 - fix all_tokens handling that caused more repetitions and discrepancy in JSON (#1060)
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-03-08T23:34:07Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[fix all_tokens handling that caused more repetitions and discrepancy in JSON 1060]


```

---

### [WRONG] aac47c98 - fix typo
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-03-08T04:43:49Z
**Note:** Immediately followed by fix commit 38f2f4d9 ("fix all_tokens handling that caused more repetitions and discrepancy in JSON (#1060)") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[fix typo]


```

---

### [WRONG] 26807ec6 - Release 20230307
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-03-08T04:36:29Z
**Note:** Immediately followed by fix commit aac47c98 ("fix typo") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Release 20230307]


```

---

### [WRONG] 919a7134 - attempt to fix the repetition/hallucination issue identified in #1046 (#1052)
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-03-08T04:08:45Z
**Note:** Self-identified failure / WIP in commit subject ("attempt to fix the repetition/hallucination issue identified in #1046 (#1052)")

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[attempt to fix the repetition/hallucination issue identified in 1046 1052]


```

---

### [WRONG] 38e990d8 - Use triton==2.0.0 (#1053)
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-03-08T00:56:31Z
**Note:** Immediately followed by fix commit 919a7134 ("attempt to fix the repetition/hallucination issue identified in #1046 (#1052)") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Use triton2.0.0 1053]


```

---

### [OK] 924e1f8e - Try installing triton only if linux & x86_64 (#1051)
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-03-07T19:31:40Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Try installing triton only if linux  x86_64 1051]


```

---

### [OK] 4b0d5e58 - Update setup.py
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-03-07T12:47:46Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update setup.py]


```

---

### [OK] 8180fde9 - Release 20230306
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-03-07T02:50:41Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Release 20230306]


```

---

### [OK] c6e4e5ef - remove auxiliary audio extension (#1021)
**Author:** Local State <localstate@users.noreply.github.com> | **Date:** 2023-03-07T01:48:14Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[remove auxiliary audio extension 1021]


```

---

### [OK] b80bcf61 - apply formatting with `black` (#1038)
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-03-06T23:50:37Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[apply formatting with black 1038]


```

---

### [OK] 500d0fe9 - word-level timestamps in `transcribe()` (#869)
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-03-06T22:00:49Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[word-level timestamps in transcribe 869]


```

---

### [OK] eab8d920 - Decoding improvements (#1033)
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-03-06T19:32:32Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Decoding improvements 1033]


```

---

### [OK] 3e1780fd - Update README.md (#894)
**Author:** Roman Vasilenko <romanvasilenko@users.noreply.github.com> | **Date:** 2023-03-04T00:41:59Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update README.md 894]


```

---

### [OK] 7858aa9c - Fix infinite loop caused by incorrect timestamp tokens prediction (#914)
**Author:** Andrey Chernykh <andreychernykh@users.noreply.github.com> | **Date:** 2023-02-01T23:46:51Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix infinite loop caused by incorrect timestamp tokens prediction 914]


```

---

### [WRONG] 5c1a8c10 - clarify that 3.11 is not supported
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-01-27T08:01:49Z
**Note:** Immediately followed by fix commit 7858aa9c ("Fix infinite loop caused by incorrect timestamp tokens prediction (#914)") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[clarify that 3.11 is not supported]


```

---

### [OK] 4e635c66 - Update README.md about Python 3.8+ requirement
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-01-24T22:45:56Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update README.md about Python 3.8 requirement]


```

---

### [OK] a6b36ede - drop python 3.7 support (#889)
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-01-24T22:05:57Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[drop python 3.7 support 889]


```

---

### [OK] 55f690af - Release 20230124
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-01-24T19:11:08Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Release 20230124]


```

---

### [OK] 7f1ef223 - handle printing even if sys.stdout.buffer is not available (#887)
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-01-24T18:12:04Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[handle printing even if sys.stdout.buffer is not available 887]


```

---

### [OK] f5bfe004 - Add TSV formatted output in transcript, using integer start/end times in milliseconds. (#228)
**Author:** Niels Mayer <nielsmayer@users.noreply.github.com> | **Date:** 2023-01-22T08:27:17Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Add TSV formatted output in transcript using integer start/end times in milliseconds. 228]


```

---

### [OK] da600abd - Added --output_format option (#333)
**Author:** Aaryan YVS <aaryanyvs@users.noreply.github.com> | **Date:** 2023-01-22T07:58:38Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Added --output_format option 333]


```

---

### [OK] 9f7aba60 - Handle XDG_CACHE_HOME properly for download_root (#864)
**Author:** zer0-x <zer0x@users.noreply.github.com> | **Date:** 2023-01-21T09:09:39Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Handle XDG_CACHE_HOME properly for download_root 864]


```

---

### [OK] 12e10894 - use stdout for printing transcription progress (#867)
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-01-20T08:54:05Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[use stdout for printing transcription progress 867]


```

---

### [OK] ea1c2667 - Fix bug where mm is mistakenly replaced with hmm in e.g. 20mm (#659)
**Author:** Markus Hennerbichler <markushennerbichler@users.noreply.github.com> | **Date:** 2023-01-18T18:41:11Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix bug where mm is mistakenly replaced with hmm in e.g. 20mm 659]


```

---

### [WRONG] 8135a7c3 - verbose outputs from pytest
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-01-18T18:30:18Z
**Note:** Immediately followed by fix commit ea1c2667 ("Fix bug where mm is mistakenly replaced with hmm in e.g. 20mm (#659)") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[verbose outputs from pytest]


```

---

### [OK] 9d646db9 - print '?' if a letter can't be encoded using the system default encoding (#859)
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-01-18T07:28:36Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[print  if a letter cant be encoded using the system default encoding 859]


```

---

### [OK] 37a4f1be - Release 20230117
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-01-18T00:08:28Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Release 20230117]


```

---

### [OK] b9f9b433 - Add github action to automatically push to pypi on Release x.y.z commit (#681)
**Author:** Romain Beaumont <romainbeaumont@users.noreply.github.com> | **Date:** 2023-01-17T23:50:26Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Add github action to automatically push to pypi on Release x.y.z commit 681]


```

---

### [OK] f0083e7e - Use ndimage.median_filter instead of signal.medfilter (#812)
**Author:** Umar Farooqi <umarfarooqi@users.noreply.github.com> | **Date:** 2023-01-17T22:43:05Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Use ndimage.median_filter instead of signal.medfilter 812]


```

---

### [OK] a84191fa - rename GitHub workflow
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-01-17T21:54:40Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[rename GitHub workflow]


```

---

### [OK] b1d213c0 - allow test_transcribe to run on CPU when CUDA is not available
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-01-17T21:43:36Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[allow test_transcribe to run on CPU when CUDA is not available]


```

---

### [OK] 493dfffa - add github action to run pytest
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-01-17T21:35:48Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[add github action to run pytest]


```

---

### [OK] 0f39c89d - Update README.md (#804)
**Author:** Mikko Vedru <mikkovedru@users.noreply.github.com> | **Date:** 2023-01-17T07:46:42Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update README.md 804]


```

---

### [OK] 6df3ea1f - Support batch-dimension in log_mel_spectogram (#839)
**Author:** Markus Hennerbichler <markushennerbichler@users.noreply.github.com> | **Date:** 2023-01-17T07:46:15Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Support batch-dimension in log_mel_spectogram 839]


```

---

### [OK] 70861c7c - Fix tiny transcribe() docstring typo (#857)
**Author:** adamreis <adamreis@users.noreply.github.com> | **Date:** 2023-01-17T06:42:01Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix tiny transcribe docstring typo 857]


```

---

### [WRONG] f82bc59f - torch.concatenate -> torch.cat for compatibility
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2023-01-10T18:53:18Z
**Note:** Immediately followed by fix commit 70861c7c ("Fix tiny transcribe() docstring typo (#857)") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[torch.concatenate - torch.cat for compatibility]


```

---

### [OK] 28769fcf - word-level timestamps in Multilingual_ASR notebook
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2022-12-31T17:03:42Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[word-level timestamps in Multilingual_ASR notebook]


```

---

### [OK] 53807677 - MultiHeadAttention to return qk as well
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2022-12-30T08:53:06Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[MultiHeadAttention to return qk as well]


```

---

### [OK] 9323b252 - Revert "saving the qk matrix in the attention module for convenience"
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2022-12-30T06:53:31Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Revert saving the qk matrix in the attention module for convenience]


```

---

### [WRONG] 68e44bd8 - saving the qk matrix in the attention module for convenience
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2022-12-30T06:02:52Z
**Note:** Reverted by commit 9323b252 ("Revert "saving the qk matrix in the attention module for convenience"")

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[saving the qk matrix in the attention module for convenience]


```

---

### [OK] 0b5dcfde - large-v2 figure and arxiv url update
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2022-12-09T05:12:39Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[large-v2 figure and arxiv url update]


```

---

### [OK] b9265e57 - Update Hebrew language code to he per IANA registry (#401)
**Author:** altryne <altryne@users.noreply.github.com> | **Date:** 2022-12-07T18:45:31Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update Hebrew language code to he per IANA registry 401]


```

---

### [OK] fd8f80c8 - Explicitly closing model file after reading it (#630)
**Author:** Paul Harter <paulharter@users.noreply.github.com> | **Date:** 2022-12-06T17:07:19Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Explicitly closing model file after reading it 630]


```

---

### [OK] 4179ed24 - add large-v2 model
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2022-12-05T16:07:14Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[add large-v2 model]


```

---

### [OK] ec1b34bb - fix compression ratio function (#561)
**Author:** jumon <jumon@users.noreply.github.com> | **Date:** 2022-12-04T23:27:42Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[fix compression ratio function 561]


```

---

### [WRONG] eff383b2 - invoking __call__ instead of forward()
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2022-11-16T12:18:50Z
**Note:** Immediately followed by fix commit ec1b34bb ("fix compression ratio function (#561)") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[invoking __call__ instead of forward]


```

---

### [OK] 02aa851a - fix to return only the text token ids
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2022-11-16T00:25:11Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[fix to return only the text token ids]


```

---

### [WRONG] 76148a56 - suppress generating non-timestamp tokens at the beginning (#532)
**Author:** jumon <jumon@users.noreply.github.com> | **Date:** 2022-11-15T19:44:36Z
**Note:** Immediately followed by fix commit 02aa851a ("fix to return only the text token ids") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[suppress generating non-timestamp tokens at the beginning 532]


```

---

### [OK] 9f70a352 - Fix attention caching to make it actually work (#370)
**Author:** Vicki Anand <vickianand@users.noreply.github.com> | **Date:** 2022-10-19T23:44:03Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix attention caching to make it actually work 370]


```

---

### [WRONG] 7f3e408e - Add package metadata to setup.py (#315)
**Author:** Sumana Harihareswara <sumanaharihareswara@users.noreply.github.com> | **Date:** 2022-10-17T20:51:16Z
**Note:** Immediately followed by fix commit 9f70a352 ("Fix attention caching to make it actually work (#370)") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Add package metadata to setup.py 315]


```

---

### [OK] f6805700 - Fix bug (#305)
**Author:** Michael Monashev <michaelmonashev@users.noreply.github.com> | **Date:** 2022-10-17T18:38:20Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix bug 305]


```

---

### [WRONG] d18e9ea5 - transcribe() on English-only model won't complain when language="en" is not given
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2022-10-09T09:40:12Z
**Note:** Immediately followed by fix commit f6805700 ("Fix bug (#305)") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[transcribe on English-only model wont complain when languageen is not given]


```

---

### [OK] 82725cea - infer download_root from XDG_CACHE_HOME if avail (#257)
**Author:** David Marx <davidmarx@users.noreply.github.com> | **Date:** 2022-10-09T09:14:03Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[infer download_root from XDG_CACHE_HOME if avail 257]


```

---

### [OK] 35713c66 - Add --threads option to transcribe (#278)
**Author:** eudoxos <eudoxos@users.noreply.github.com> | **Date:** 2022-10-09T09:11:15Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Add --threads option to transcribe 278]


```

---

### [OK] 9e653bd0 - Fixed CoW RuntimeError in DecodingTask.run() (#240)
**Author:** Corentin Jemine <corentinjemine@users.noreply.github.com> | **Date:** 2022-10-04T15:49:31Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fixed CoW RuntimeError in DecodingTask.run 240]


```

---

### [WRONG] 02b74308 - Fix timestamps and strip extraneous whitespace in WebVTT output (#219)
**Author:** Tom Stuart <tomstuart@users.noreply.github.com> | **Date:** 2022-10-03T21:51:07Z
**Note:** Immediately followed by fix commit 9e653bd0 ("Fixed CoW RuntimeError in DecodingTask.run() (#240)") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix timestamps and strip extraneous whitespace in WebVTT output 219]


```

---

### [WRONG] 0b1ba3d4 - Add model_dir to arguments (#202)
**Author:** Jibin Mathew <jibinmathew@users.noreply.github.com> | **Date:** 2022-09-30T21:45:51Z
**Note:** Immediately followed by fix commit 02b74308 ("Fix timestamps and strip extraneous whitespace in WebVTT output (#219)") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Add model_dir to arguments 202]


```

---

### [OK] 60132ade - Use , character instead of . for SRT output. (#197)
**Author:** Caleb McQuillin <calebmcquillin@users.noreply.github.com> | **Date:** 2022-09-30T03:44:12Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Use  character instead of . for SRT output. 197]


```

---

### [OK] 7cb4cc21 - allowing nonzero initial temperature
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2022-09-30T01:05:12Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[allowing nonzero initial temperature]


```

---

### [OK] 30dc5c58 - pointer to the show and tell section
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2022-09-29T21:57:12Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[pointer to the show and tell section]


```

---

### [OK] 5905e503 - Update README.md (#161)
**Author:** Szabolcs Pasztor <szabolcspasztor@users.noreply.github.com> | **Date:** 2022-09-29T21:18:54Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update README.md 161]


```

---

### [OK] 0457aac3 - Adds missing command for install (mac) (#90)
**Author:** Fabiano <fabiano@users.noreply.github.com> | **Date:** 2022-09-29T21:08:58Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Adds missing command for install mac 90]


```

---

### [OK] deafef05 - Update audio.py (#178)
**Author:** sawadata <sawadata@users.noreply.github.com> | **Date:** 2022-09-29T19:34:04Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Update audio.py 178]


```

---

### [OK] 2b0c2971 - Don't update duration if last timestamp is same as begin (#191)
**Author:** Vicki Anand <vickianand@users.noreply.github.com> | **Date:** 2022-09-29T19:27:48Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Dont update duration if last timestamp is same as begin 191]


```

---

### [OK] 62fe7f10 - patience definition to match the paper
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2022-09-28T02:00:41Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[patience definition to match the paper]


```

---

### [OK] b4308c47 - fix: transcribe verbosity (#140)
**Author:** Nick Konovalchuk <nickkonovalchuk@users.noreply.github.com> | **Date:** 2022-09-26T18:46:21Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[fix: transcribe verbosity 140]


```

---

### [WRONG] 9c8183a1 - Use PyTorch as logits transpose for ONNX support (#141)
**Author:** Michael Goin <michaelgoin@users.noreply.github.com> | **Date:** 2022-09-26T17:54:26Z
**Note:** Immediately followed by fix commit b4308c47 ("fix: transcribe verbosity (#140)") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Use PyTorch as logits transpose for ONNX support 141]


```

---

### [OK] 2037b65f - Context prompt (#128)
**Author:** VulumeCode <vulumecode@users.noreply.github.com> | **Date:** 2022-09-26T12:22:33Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Context prompt 128]


```

---

### [OK] fc0f4098 - Write each sentence as a separate line for the txt output (#101)
**Author:** EliEron <elieron@users.noreply.github.com> | **Date:** 2022-09-26T11:52:28Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Write each sentence as a separate line for the txt output 101]


```

---

### [OK] 520796a3 - fix token suppression (#123)
**Author:** VulumeCode <vulumecode@users.noreply.github.com> | **Date:** 2022-09-26T11:35:21Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[fix token suppression 123]


```

---

### [WRONG] ead77fab - add srt subtitle export utility (#102)
**Author:** fatih <fatih@users.noreply.github.com> | **Date:** 2022-09-26T10:50:26Z
**Note:** Immediately followed by fix commit 520796a3 ("fix token suppression (#123)") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[add srt subtitle export utility 102]


```

---

### [OK] 5485428c - arch linux ffmpeg install (#93)
**Author:** Ashutosh Tripathi <ashutoshtripathi@users.noreply.github.com> | **Date:** 2022-09-26T10:24:47Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[arch linux ffmpeg install 93]


```

---

### [OK] 9e7e418f - add progress bar for transcribe loop (#100)
**Author:** fatih <fatih@users.noreply.github.com> | **Date:** 2022-09-26T10:24:13Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[add progress bar for transcribe loop 100]


```

---

### [OK] 5d8d3e75 - add --condition_on_previous_text
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2022-09-25T12:16:08Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[add --condition_on_previous_text]


```

---

### [OK] 2d3032de - improved warning message for English-only models
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2022-09-25T09:10:36Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[improved warning message for English-only models]


```

---

### [OK] 8cf36f35 - allow hyphens and single quotes between words
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2022-09-23T11:11:27Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[allow hyphens and single quotes between words]


```

---

### [OK] 15ab5482 - nocaptions -> nospeech to match the paper figure
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2022-09-23T06:45:32Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[nocaptions - nospeech to match the paper figure]


```

---

### [OK] 61989529 - Fix possible mistake when loading model to device (#57)
**Author:** mj-kh <mjkh@users.noreply.github.com> | **Date:** 2022-09-23T06:21:47Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix possible mistake when loading model to device 57]


```

---

### [WRONG] f296bcd3 - Avoid keeping redundant copies of model weights in memory during load (#42)
**Author:** Niklas K <niklask@users.noreply.github.com> | **Date:** 2022-09-23T03:57:39Z
**Note:** Immediately followed by fix commit 61989529 ("Fix possible mistake when loading model to device (#57)") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Avoid keeping redundant copies of model weights in memory during load 42]


```

---

### [OK] a4fe05aa - Add conda environment.yml (and fix requirements.txt) (#8)
**Author:** Sidney Radcliffe <sidneyradcliffe@users.noreply.github.com> | **Date:** 2022-09-23T03:30:45Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Add conda environment.yml and fix requirements.txt 8]


```

---

### [WRONG] 957ffc77 - Add rust as a dependency (#30)
**Author:** Giovanni Lanzani <giovannilanzani@users.noreply.github.com> | **Date:** 2022-09-23T03:26:38Z
**Note:** Immediately followed by fix commit a4fe05aa ("Add conda environment.yml (and fix requirements.txt) (#8)") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Add rust as a dependency 30]


```

---

### [OK] 59f543e2 - Fix exception cause in audio.py (#33)
**Author:** Ram Rachum <ramrachum@users.noreply.github.com> | **Date:** 2022-09-23T03:12:37Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix exception cause in audio.py 33]


```

---

### [WRONG] c85eaaae - Use UTF-8 encoding to save the txt and vtt files (#37)
**Author:** hanacchi <hanacchi@users.noreply.github.com> | **Date:** 2022-09-23T03:10:55Z
**Note:** Immediately followed by fix commit 59f543e2 ("Fix exception cause in audio.py (#33)") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Use UTF-8 encoding to save the txt and vtt files 37]


```

---

### [OK] 759e8d47 - Fix output_dir argument when audio file is a path (#45)
**Author:** EliEron <elieron@users.noreply.github.com> | **Date:** 2022-09-23T02:38:37Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Fix output_dir argument when audio file is a path 45]


```

---

### [WRONG] c0607e8d - Add scoop install for windows (#48)
**Author:** Micheal Taylor <michealtaylor@users.noreply.github.com> | **Date:** 2022-09-23T02:37:57Z
**Note:** Immediately followed by fix commit 759e8d47 ("Fix output_dir argument when audio file is a path (#45)") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Add scoop install for windows 48]


```

---

### [OK] e90b8fa7 - Merge pull request #14 from bquast/patch-1
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2022-09-22T02:51:05Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Merge pull request 14 from bquast/patch-1]


```

---

### [WRONG] f83cb83a - Merge pull request #24 from ldanilov/patch-1
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2022-09-22T02:48:57Z
**Note:** Immediately followed by fix commit e90b8fa7 ("Merge pull request #14 from bquast/patch-1") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[Merge pull request 24 from ldanilov/patch-1]


```

---

### [WRONG] 45fc3d43 - fixes the link to the model paper
**Author:** Lev Danilov <levdanilov@users.noreply.github.com> | **Date:** 2022-09-22T01:25:17Z
**Note:** Immediately followed by fix commit f83cb83a ("Merge pull request #24 from ldanilov/patch-1") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[fixes the link to the model paper]


```

---

### [WRONG] 08a739ad - make LICENSE a link instead of code-formatted text
**Author:** Bastiaan Quast <bastiaanquast@users.noreply.github.com> | **Date:** 2022-09-21T21:17:02Z
**Note:** Immediately followed by fix commit 45fc3d43 ("fixes the link to the model paper") touching overlapping files (whisper.ts)

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[make LICENSE a link instead of code-formatted text]


```

---

### [OK] 49a3ffc9 - add section Available models and languages
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2022-09-21T20:36:25Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[add section Available models and languages]


```

---

### [OK] cfd6bdda - a note on speed-accuracy tradeoffs
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2022-09-21T17:58:56Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[a note on speed-accuracy tradeoffs]


```

---

### [OK] 834f00a0 - making small model the default
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2022-09-21T17:45:03Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[making small model the default]


```

---

### [OK] 6e3be77e - initial commit
**Author:** Jong Wook Kim <jongwookkim@users.noreply.github.com> | **Date:** 2022-09-21T15:43:20Z

```diff
diff --git a/whisper.ts b/whisper.ts
--- a/whisper.ts
+++ b/whisper.ts
@@ -1,1 +1,3 @@
+[initial commit]


```
