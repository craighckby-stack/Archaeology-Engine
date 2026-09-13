# Failed Commits Ledger (WRONG.md)

> Record of every commit that failed, was reverted, or required immediate patching, paired with its recovery link (newest commits first).

## 2026-08-26T16:33:06Z -- Updating hashes (`c9224fa9`)

**Reason:** Immediately followed by fix commit b498fb68 ("Fix OSS HHVM build x64 and Arm64") touching overlapping files (watchman.ts)
**Fixed by:** `b498fb68` (see CORRECT.md for recovery commit)

**Files touched:**
- `watchman.ts`

**Commit message:**
```
Updating hashes
Summary:
GitHub commits:

https://github.com/facebook/CacheLib/commit/644d6249eb1344acae3a94a49ddda050d36d90ca
https://github.com/facebook/fb303/commit/9f89cfce8f08990df3d28be5f8ec01b493aec7e8
https://github.com/facebook/fbthrift/commit/747fae0fc94346bd26e1017c013c2e44260ffd59
https://github.com/facebook/folly/commit/559798fb5a52d839175c173b7da2c2a2e79d921c
https://github.com/facebook/mvfst/commit/cc0e1cf92bde7f94240780eb7efd51a7509dbf3f
https://github.com/facebook/proxygen/commit/f97f06e02e50b73575ab6f38b9deeabe327fee38
https://github.com/facebook/pyrefly/commit/ee9a4f85f9242e5c12786fee44770f5efae7d9bc
https://github.com/facebook/wangle/commit/f24e0a94d8733b7d56023e1cf8634a2192d010a0
https://github.com/facebookexperimental/edencommon/commit/0c8574bd6938ded9a571faa9661f044fd5dc8f55
https://github.com/facebookexperimental/rust-shed/commit/d4e999e407ca605d10d40568a050a6832726e77f
https://github.com/facebookincubator/fizz/commit/95f9dd8691cc7ca61e1f734ec6794607fd97fe64

Reviewed By: bigfootjon

fbshipit-source-id: 15b3536caa5eac50de89bf44c293e87e7b764185
```

**Diff:**
```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

## 2026-07-14T20:37:20Z -- Fix cmake 3.31.12 macOS universal sha256 in manifest (`7ff5205d`)

**Reason:** Immediately followed by fix commit f5b86739 ("Back out "Fix cmake 3.31.12 macOS universal sha256 in manifest"") touching overlapping files (watchman.ts)
**Fixed by:** `f5b86739` (see CORRECT.md for recovery commit)

**Files touched:**
- `watchman.ts`

**Commit message:**
```
Fix cmake 3.31.12 macOS universal sha256 in manifest
Summary:
Update fbcode/opensource/fbcode_builder/manifests/cmake to use correct sha256 0252a025b40c96363439bd8e14b86f797b7d2363ec6f162eb882d2f99c406fac for cmake-3.31.12-macos-universal.tar.gz, fixing oss-fizz-darwin-getdeps user_error in T276836249.

The Sandcastle workflow was failing hash verification because upstream CMake release artifact changed hash. Expected old 7098ade6... but got 0252a025...

Also update fbcode/tools/lfs/.lfs-pointers to include cmake-3.31.12 and cmake-cmake-3.31.12 variants for macos-universal and linux tar to avoid KeyError fallback.

Reviewed By: vincom2

Differential Revision: D111088825

fbshipit-source-id: 8dcfe9dda36168999fb579e092875134406116a8
```

**Diff:**
```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Fix cmake 3.31.12 macOS universal sha256 in manifest]


```

---

## 2026-07-14T16:32:28Z -- Updating hashes (`df8675f6`)

**Reason:** Immediately followed by fix commit 7ff5205d ("Fix cmake 3.31.12 macOS universal sha256 in manifest") touching overlapping files (watchman.ts)
**Fixed by:** `7ff5205d` (see CORRECT.md for recovery commit)

**Files touched:**
- `watchman.ts`

**Commit message:**
```
Updating hashes
Summary:
GitHub commits:

https://github.com/facebook/CacheLib/commit/0f12f810d94a979890e0a4a7fbe450eab95dcb0e
https://github.com/facebook/fb303/commit/154032db0782257fa25fe5292b5f59ed08ee1489
https://github.com/facebook/fbthrift/commit/436b07ff26e4bc9dd3b12251f5518b842b9e2f61
https://github.com/facebook/folly/commit/61420a5b85bce8b0d7d249834a50fa5ad756e567
https://github.com/facebook/hermes/commit/03930808da08f393c3b2631de1cccb6b0308f9cc
https://github.com/facebook/mvfst/commit/fd4402aed3a51ec1b7122f8da74cb9d73afbb75f
https://github.com/facebook/proxygen/commit/c381748d5fff4b31b113c510e8b90a26d83fdee3
https://github.com/facebook/pyrefly/commit/dfb398d9516895b4bccae20581571c7392a24d1d
https://github.com/facebook/wangle/commit/a37afe841845bf07d9adf143f8a16a7e504af598
https://github.com/facebookexperimental/edencommon/commit/d6ebbb74656196f162678cef41c5d660bd0a167e
https://github.com/facebookexperimental/rust-shed/commit/3d668a330e10501151328865b434d0e07465e761
https://github.com/facebookincubator/fizz/commit/28188bcca9d8315ed90dce6341433937be714cbb
https://github.com/react/yoga/commit/6d3a8e0292d8b3c21fe5a14ec76721940f74f569

Reviewed By: ajb85

fbshipit-source-id: 525eddb3d826942e8c987955abb52460b001e798
```

**Diff:**
```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

## 2026-07-09T16:31:53Z -- Updating hashes (`670a8650`)

**Reason:** Immediately followed by fix commit 99ccc0f0 ("Fix Windows Pyrefly type-check failures for POSIX-only APIs") touching overlapping files (watchman.ts)
**Fixed by:** `99ccc0f0` (see CORRECT.md for recovery commit)

**Files touched:**
- `watchman.ts`

**Commit message:**
```
Updating hashes
Summary:
GitHub commits:

https://github.com/facebook/CacheLib/commit/5c0b4fc1f51a7971175bbac30b656708593e4130
https://github.com/facebook/fb303/commit/e8b0021954f2328b9b954687b2a94dbae100fd17
https://github.com/facebook/fbthrift/commit/f9bd6afbf749d3062c652d313a4568cb114e2e8a
https://github.com/facebook/mvfst/commit/d65d7714df36d420a96e5c3ef15a49e6c012c8ad
https://github.com/facebook/proxygen/commit/3ddb7f89a9bab7d32b1f9c9d9a0e6ad890cc5165
https://github.com/facebook/pyrefly/commit/ec8d60e4c667c962c6b87795bbc048d95a29d8ae
https://github.com/facebook/wangle/commit/f26f18dfa289ed7cd0aadedb503c4a94ae02c1da
https://github.com/facebookexperimental/edencommon/commit/9bbfab31b921d0044ef1eaed36e2b660d75f7bc4
https://github.com/facebookexperimental/rust-shed/commit/f3addac76d1a0dd383c99421808d79ba04267499
https://github.com/facebookincubator/fizz/commit/23a59844d48c1753790e17394bfde40fb5198801
https://github.com/react/yoga/commit/b58c0463281d000725d7fb595210a53762582b2e

Reviewed By: ajb85

fbshipit-source-id: 88855325b7ab30551c1d7978582a4239a195eaa8
```

**Diff:**
```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

## 2026-07-07T21:21:16Z -- ci: fix Mac builds broken by llvm@20 pin in #47 (`3acb7941`)

**Reason:** Self-identified failure / WIP in commit subject ("ci: fix Mac builds broken by llvm@20 pin in #47")

**Files touched:**
- `watchman.ts`

**Commit message:**
```
ci: fix Mac builds broken by llvm@20 pin in #47
Summary:
The `llvm@20` pin that landed in https://github.com/facebook/rebalancer/issues/47 breaks Mac builds. Homebrew's folly bottles are compiled against LLVM 22's libc++; linking against LLVM 20 produces `ld: symbol(s) not found for architecture arm64` on libfolly.

The correct fix (which was developed but didn't make it into the https://github.com/facebook/rebalancer/issues/47 squash merge) is to:
1. Revert the `llvm@20` pin back to `brew install llvm` (LLVM 22) in `getdeps_mac.yml`, `before_all_macos.sh`, and `wheels.yml`
2. Remove `fmt` from `[homebrew]` in `manifests/fmt` so getdeps builds fmt 12.1.0 from source as a static library — no `-DFMT_SHARED`, so `fmt::format` stays header-inline and visible under LLVM 22's libc++

This matches Linux behavior (getdeps already builds fmt from source there).

X-link: https://github.com/facebook/rebalancer/pull/48

Reviewed By: yangneu2015

Differential Revision: D110918926

Pulled By: r-barnes

fbshipit-source-id: fd15b2f83f14bd8c6077a6ecf210eeebf106e459
```

**Diff:**
```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[ci: fix Mac builds broken by llvm20 pin in 47]


```

---

## 2026-07-07T16:31:32Z -- Updating hashes (`a819bf99`)

**Reason:** Immediately followed by fix commit 3acb7941 ("ci: fix Mac builds broken by llvm@20 pin in #47") touching overlapping files (watchman.ts)
**Fixed by:** `3acb7941` (see CORRECT.md for recovery commit)

**Files touched:**
- `watchman.ts`

**Commit message:**
```
Updating hashes
Summary:
GitHub commits:

https://github.com/facebook/CacheLib/commit/fe0e84291b173373d3cc1c6fe05c67a5599499ea
https://github.com/facebook/fb303/commit/3f33394cf0c8c397cc0b0ec047209d5bf2723a5e
https://github.com/facebook/fbthrift/commit/b8e35263fd6b60c0efd6b3cfe2a186f3ffe159b1
https://github.com/facebook/folly/commit/9874b4aa1f971459be6c0476c88f02c7ac8485b8
https://github.com/facebook/hermes/commit/896d643e7453f507b062140f849f89ecf5448a88
https://github.com/facebook/mvfst/commit/c5470eadcde91306e89001599bef59557f92352b
https://github.com/facebook/proxygen/commit/0254d345dce96297c3b9e98e5daf98bb828b81d9
https://github.com/facebook/pyrefly/commit/e5120f79bde50eda2e2fb855763b59d987494f96
https://github.com/facebook/wangle/commit/c270ff0421f8237237bf4b2a0f53083fdb08ae67
https://github.com/facebookexperimental/edencommon/commit/1e2155ca13d9b639c23a8017d6afd589e2ed1f60
https://github.com/facebookexperimental/rust-shed/commit/3ccba4373cef41d13b57a393ae24d6f165bd1a0e
https://github.com/facebookincubator/fizz/commit/93cf3f61ffd7220a80002f00b5385c27956f4ffb

Reviewed By: ajb85

fbshipit-source-id: bcdcecf3d539f5e482c2dbc96b2bf63b95175c9a
```

**Diff:**
```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

## 2026-06-30T19:26:39Z -- ci: fix macOS wheel delocate failure and linux-sdk test_solve absence (`2e862a90`)

**Reason:** Self-identified failure / WIP in commit subject ("ci: fix macOS wheel delocate failure and linux-sdk test_solve absence")

**Files touched:**
- `watchman.ts`

**Commit message:**
```
ci: fix macOS wheel delocate failure and linux-sdk test_solve absence
Summary:
Two independent CI failures on `main`, both fixed in this PR.

### 1. macOS wheels: `delocate DelocationError`

**Root cause:** cmake 3.31.x / scikit-build-core 0.12.x silently drops `LC_RPATH` entries from MODULE and SHARED targets on macOS arm64. The built `_rebalancer.cpython-*.so` and `_lib/librebalancer.dylib` have empty `LC_RPATH`, so delocate-wheel 0.13.0 raises `DelocationError` when it can't resolve `rpath/libfolly.*.dylib` and `rpath/librebalancer.dylib`.

**Fix:** New `tools/wheels/repair_macos.sh` (`CIBW_REPAIR_WHEEL_COMMAND_MACOS`) that patches the missing rpaths before handing off to `delocate-wheel`:
1. Adds `loader_path/_lib` to `_rebalancer.cpython-*.so` so delocate can walk the dep chain to `librebalancer.dylib`
2. Adds each getdeps/brew prefix `lib/` dir (from `.cmake_prefix_path`) to `_lib/librebalancer.dylib` so delocate can find and bundle transitive deps (`libfolly`, `libglog`, `libfmt`, …) into `.dylibs/`

### 2. linux-sdk: `test_solve: No such file or directory`

**Root cause:** `build_linux_sdk.sh` passed `PACKAGING_TEST=ON` via `--extra-cmake-defines`, but getdeps silently excludes `--extra-cmake-defines` from its build-cache key. When a cached build exists (from a prior CI run), getdeps reuses the cached ninja graph which predates `PACKAGING_TEST` and omits `test_solve`. The SDK artifact is uploaded without the binary; the deb smoke test then fails with exit 127.

**Fix 1 (primary):** Move `PACKAGING_TEST=ON` into the manifest `[cmake.defines]` block — manifest defines ARE part of the cache key, so getdeps always builds `test_solve`.

**Fix 2 (belt-and-suspenders):** `build_linux_sdk.sh` now detects absence of `test_solve` after the getdeps build and compiles it directly from the installed headers/library using clang, so the artifact is always complete even against a stale cache primed before this manifest change.

X-link: https://github.com/facebook/rebalancer/pull/39

Reviewed By: kvelakur

Differential Revision: D110218346

Pulled By: r-barnes

fbshipit-source-id: fcb57f9ef51870a1c9ce38d3457ff8ef8e8d0b02
```

**Diff:**
```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[ci: fix macOS wheel delocate failure and linux-sdk test_solve absence]


```

---

## 2026-06-30T19:03:09Z -- ci: use default Xcode for Mac getdeps instead of pinned Xcode 16.2 (`2cd72ade`)

**Reason:** Immediately followed by fix commit 2e862a90 ("ci: fix macOS wheel delocate failure and linux-sdk test_solve absence") touching overlapping files (watchman.ts)
**Fixed by:** `2e862a90` (see CORRECT.md for recovery commit)

**Files touched:**
- `watchman.ts`

**Commit message:**
```
ci: use default Xcode for Mac getdeps instead of pinned Xcode 16.2
Summary:
The Mac workflow pinned DEVELOPER_DIR to /Applications/Xcode_16.2.app, which GitHub has since rotated off the macOS runner image, so xcrun fails ("missing DEVELOPER_DIR path") before any build starts. The job also uses Homebrew LLVM, which always pulls the newest clang, so pinning an old SDK against an ever-newer compiler is the fragile combination that caused the earlier libc++ header mismatches; a newer default SDK pairs better.

Point DEVELOPER_DIR at /Applications/Xcode.app/Contents/Developer (the runner's default-selected Xcode symlink), which survives image rotations. Updated in the generated workflow, the getdeps workflow_generator that emits it, and the golden-file test fixture so they stay consistent.

X-link: https://github.com/facebook/folly/pull/2666

Reviewed By: sandarsh

Differential Revision: D110202021

Pulled By: afrind

fbshipit-source-id: c121cfaa33c5a7d791b0405aaf5b62c7c3a4dc11
```

**Diff:**
```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[ci: use default Xcode for Mac getdeps instead of pinned Xcode 16.2]


```

---

## 2026-06-16T16:49:09Z -- Upgrade getdeps template's mozilla-actions/sccache-action from v0.0.9 -> v0.0.10 (`ba7bd548`)

**Reason:** Immediately followed by fix commit 9fe66d53 ("Fix cmake 3.31.12 macOS universal sha256 in manifest") touching overlapping files (watchman.ts)
**Fixed by:** `9fe66d53` (see CORRECT.md for recovery commit)

**Files touched:**
- `watchman.ts`

**Commit message:**
```
Upgrade getdeps template's mozilla-actions/sccache-action from v0.0.9 -> v0.0.10
Summary: Someone flagged this on the Cachelib repo in [this PR](https://github.com/facebook/CacheLib/pull/461).  This apparently fixes a deprecation warning for node20.js by bumping to node24 ([relevant PR](https://github.com/Mozilla-Actions/sccache-action/pull/245) for v0.0.10).

Reviewed By: xavierd

Differential Revision: D108750058

fbshipit-source-id: b17971b6bd5c17d56eddfa56a9d03d295aa2e882
```

**Diff:**
```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Upgrade getdeps templates mozilla-actions/sccache-action from v0.0.9 - v0.0.10]


```

---

## 2026-06-03T16:32:37Z -- Updating hashes (`59916a20`)

**Reason:** Immediately followed by fix commit b1ad139d ("Upgrade CMake to 3.31.12 and fix Windows sccache via /Z7") touching overlapping files (watchman.ts)
**Fixed by:** `b1ad139d` (see CORRECT.md for recovery commit)

**Files touched:**
- `watchman.ts`

**Commit message:**
```
Updating hashes
Summary:
GitHub commits:

https://github.com/facebook/CacheLib/commit/6d121f225d5849ab00f4315ce6340b85525b09ed
https://github.com/facebook/fb303/commit/6514f283676b6d2445e286de9328ee142fb8734b
https://github.com/facebook/fbthrift/commit/8871d437c31e68c8a605420d9a7ff12f57173f84
https://github.com/facebook/folly/commit/c3c20935112ba581a73a1e8549152d434986ca72
https://github.com/facebook/hermes/commit/81c2c9e7c3ad5b8ffc8decf5e5e450a216e2817f
https://github.com/facebook/mvfst/commit/371ad18be00edb497d34f6e4523662c7fe0fd203
https://github.com/facebook/proxygen/commit/530fef841924a3a951e32262a40d1c6c60f7095c
https://github.com/facebook/wangle/commit/56e09aff8a97a98c1de995b58fff7b8c9fe2737e
https://github.com/facebook/yoga/commit/4a59d0940146a7d4f0b9f73cf7ba08cf8d2b818b
https://github.com/facebookexperimental/edencommon/commit/044edca05bbe383647f9d09d898d0bd4473d1b66
https://github.com/facebookexperimental/rust-shed/commit/4d34d77b666d2b6f4acf7885f4020cb094ebd38b
https://github.com/facebookincubator/fizz/commit/75e3c542b52c2e405889f4afd40906df95efb115

Reviewed By: bigfootjon

fbshipit-source-id: 8154a0369f3cf8980add31fcad705e1bec52453b
```

**Diff:**
```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

## 2026-05-27T16:33:02Z -- Updating hashes (`46a4c248`)

**Reason:** Immediately followed by fix commit ca920c5b ("Vendor openr IDL + NetworkUtil, OSS build fixes") touching overlapping files (watchman.ts)
**Fixed by:** `ca920c5b` (see CORRECT.md for recovery commit)

**Files touched:**
- `watchman.ts`

**Commit message:**
```
Updating hashes
Summary:
GitHub commits:

https://github.com/WhatsApp/eqwalizer/commit/854896722776ed3b6c3bad5f7fff0b1ee028d97e
https://github.com/facebook/CacheLib/commit/7aa1b965446e0f7afaaa9e77112224c2d24a2c82
https://github.com/facebook/fb303/commit/6fe7994ba74b5ee227b3e8e183ed9a3937551ab7
https://github.com/facebook/fbthrift/commit/68a730b5e38bb507c2e0528107a285d5ab08c0e1
https://github.com/facebook/folly/commit/2bdc8ce1acf66a2d046809c6d5e8914bd529b972
https://github.com/facebook/mvfst/commit/c2a799537e8317b0423f92c9fb663999c4070c69
https://github.com/facebook/proxygen/commit/c4ea06455b836076b91df0b93b3d06d8555e8ce0
https://github.com/facebook/pyrefly/commit/495b1f9997a846c536dd2863883077a39916fcdb
https://github.com/facebook/wangle/commit/50ea6b3869401c7f3d8d10374ee521a9e187a5af
https://github.com/facebookexperimental/edencommon/commit/5b902c86c62d810873d81d61f3bcce8286a13f77
https://github.com/facebookexperimental/rust-shed/commit/7c480a112fa29a673283da454de2a214a83e81f8
https://github.com/facebookincubator/fizz/commit/8fe04a934316a9db6853b1cacd169ca0f8db528f

Reviewed By: pranavcivi

fbshipit-source-id: 6e099a746d568fe78cc78abd9cb9016243e515fc
```

**Diff:**
```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

## 2026-05-26T18:50:21Z -- Upgrade gperf to 3.3 in fbcode_builder manifests (`c46ae6c6`)

**Reason:** Immediately followed by fix commit 5e6ec176 ("Fix missing Meta+license text in new file") touching overlapping files (watchman.ts)
**Fixed by:** `5e6ec176` (see CORRECT.md for recovery commit)

**Files touched:**
- `watchman.ts`

**Commit message:**
```
Upgrade gperf to 3.3 in fbcode_builder manifests
Summary:
X-link: https://github.com/facebook/proxygen/pull/619

Bump gperf from 3.1 to 3.3 in the fbcode_builder manifest and add the corresponding LFS pointer for the new tarball.

Reviewed By: jbeshay, kvtsoy

Differential Revision: D106373850

fbshipit-source-id: e57201a74ea06c12852a50efb5b5b858d712777c
```

**Diff:**
```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Upgrade gperf to 3.3 in fbcode_builder manifests]


```

---

