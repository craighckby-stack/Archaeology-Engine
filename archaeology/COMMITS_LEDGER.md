# Complete Commit Ledger

Total Commits Analyzed: 200

### [OK] ffb0bb6b - Summary:
**Author:** Unknown | **Date:** Sat, 12 Sep 2026 16:32:17 -0700

```diff
Publish Linux Watchman releases through the existing `rpm.builder` target and remove the legacy Packman definition after its final active consumer moves to `fbrpm`.

Why: Packman is being retired, and `fbrpm` builds the same package from a target that lives next to the code.

How: the release step calls `fbrpm build` with no version overrides, and the target sets `use_commit_timestamp_for_version_release` so version and release come from the commit being built. `--override-version` and `--override-release` are parsed by the CLI but dropped before the publish request is built, so threading them through the determinator would read as if the version were pinned while the published RPM still carried the build machine's clock.

The conveyor job still checks for the determinator's start-time version, so it does not agree with the published RPM until the diff at the top of this stack moves those checks onto the commit timestamp too.

Reviewed By: genevievehelsel

Differential Revision: D117902837

fbshipit-source-id: 98d655d7269af2656a08678d0f8f1af5d880e6ac
---
 watchman/facebook/BUCK | 1 +
 1 file changed, 1 insertion(+)

diff --git a/watchman/facebook/BUCK b/watchman/facebook/BUCK
index be3c552af4d3..2cd84ace0efb 100644
--- a/watchman/facebook/BUCK
+++ b/watchman/facebook/BUCK
@@ -121,4 +121,5 @@ rpm.builder(
     ],
     override_log_paths = ["watchman"],
     summary = "Watch files, trigger stuff",
+    use_commit_timestamp_for_version_release = True,
 )



```

---

### [OK] 9e3ce583 - Summary:
**Author:** Unknown | **Date:** Sat, 12 Sep 2026 09:32:40 -0700

```diff
GitHub commits:

https://github.com/facebook/CacheLib/commit/f2f83ceb43294e383bc4524bc294b681910737eb
https://github.com/facebook/fb303/commit/ce8af6280bc7ce336ddc05064d0165cfdcb24b58
https://github.com/facebook/fbthrift/commit/da27a027f856b63281e2c36b1a35d3c38a7229cf
https://github.com/facebook/folly/commit/e243181570d06e6e746bc3995f0745f0e3dfd532
https://github.com/facebook/hermes/commit/36ab30689cf94a66203f5f99d9a4dd4d7930c515
https://github.com/facebook/mvfst/commit/3fc45489431dea92fbbdbf6d74161da416178f1a
https://github.com/facebook/proxygen/commit/4954ad12adc177a9654ef7b5f9c30a1bdc6d6658
https://github.com/facebook/pyrefly/commit/b4cf749b51534e0886d9568da126cbd7dae784cd
https://github.com/facebook/wangle/commit/ab25c8c58a444e62adb5ba9cb59aa25d4aa1fb45
https://github.com/facebookexperimental/edencommon/commit/f7388c02828151cd5371e1a6d572f741aa7a31f2
https://github.com/facebookexperimental/rust-shed/commit/aaefb4dbc2685df0a63721e3ff2b45e0f5c3b124
https://github.com/facebookincubator/fizz/commit/7de8f52e9878206cd3a0192d9b4295cd58cd2ac9

Reviewed By: ajb85

fbshipit-source-id: 2f63954b9a484cc3590def6cf8ac9d41494bc41f
---
 build/deps/github_hashes/facebook/fb303-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/fbthrift-rev.txt              | 2 +-
 build/deps/github_hashes/facebook/folly-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/mvfst-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/wangle-rev.txt                | 2 +-
 .../deps/github_hashes/facebookexperimental/edencommon-rev.txt  | 2 +-
 build/deps/github_hashes/facebookincubator/fizz-rev.txt         | 2 +-
 7 files changed, 7 insertions(+), 7 deletions(-)

diff --git a/build/deps/github_hashes/facebook/fb303-rev.txt b/build/deps/github_hashes/facebook/fb303-rev.txt
index dc12fa587b52..85451015cdcc 100644
--- a/build/deps/github_hashes/facebook/fb303-rev.txt
+++ b/build/deps/github_hashes/facebook/fb303-rev.txt
@@ -1 +1 @@
-Subproject commit 7bc929b00aebdd50479f5d8a8759c012b15d6ca8
+Subproject commit ce8af6280bc7ce336ddc05064d0165cfdcb24b58
diff --git a/build/deps/github_hashes/facebook/fbthrift-rev.txt b/build/deps/github_hashes/facebook/fbthrift-rev.txt
index de34d1b3942b..d6cda0967cd2 100644
--- a/build/deps/github_hashes/facebook/fbthrift-rev.txt
+++ b/build/deps/github_hashes/facebook/fbthrift-rev.txt
@@ -1 +1 @@
-Subproject commit 55441a7ea27c97c872b4d8ccd6c96d5c3ac0b707
+Subproject commit da27a027f856b63281e2c36b1a35d3c38a7229cf
diff --git a/build/deps/github_hashes/facebook/folly-rev.txt b/build/deps/github_hashes/facebook/folly-rev.txt
index 78f03a640409..887788f5acbc 100644
--- a/build/deps/github_hashes/facebook/folly-rev.txt
+++ b/build/deps/github_hashes/facebook/folly-rev.txt
@@ -1 +1 @@
-Subproject commit 60e497a8b5ff8f94c73ff38a13487b2751ba1b11
+Subproject commit e243181570d06e6e746bc3995f0745f0e3dfd532
diff --git a/build/deps/github_hashes/facebook/mvfst-rev.txt b/build/deps/github_hashes/facebook/mvfst-rev.txt
index 3961c4912495..38284bb89c88 100644
--- a/build/deps/github_hashes/facebook/mvfst-rev.txt
+++ b/build/deps/github_hashes/facebook/mvfst-rev.txt
@@ -1 +1 @@
-Subproject commit 46396990721807eb0c317e33fd06bf5a2f31b0b7
+Subproject commit 3fc45489431dea92fbbdbf6d74161da416178f1a
diff --git a/build/deps/github_hashes/facebook/wangle-rev.txt b/build/deps/github_hashes/facebook/wangle-rev.txt
index 40c8062ef04d..8a1aded164c4 100644
--- a/build/deps/github_hashes/facebook/wangle-rev.txt
+++ b/build/deps/github_hashes/facebook/wangle-rev.txt
@@ -1 +1 @@
-Subproject commit d7eb11f1f988f03e363715b32eb90d70c0324375
+Subproject commit ab25c8c58a444e62adb5ba9cb59aa25d4aa1fb45
diff --git a/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt b/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
index 2287bc87f8b4..005f01068e3b 100644
--- a/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
+++ b/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
@@ -1 +1 @@
-Subproject commit 291e2700de29f87ea943975c5b15931ec42015ac
+Subproject commit f7388c02828151cd5371e1a6d572f741aa7a31f2
diff --git a/build/deps/github_hashes/facebookincubator/fizz-rev.txt b/build/deps/github_hashes/facebookincubator/fizz-rev.txt
index 734c063867c0..11552dd82479 100644
--- a/build/deps/github_hashes/facebookincubator/fizz-rev.txt
+++ b/build/deps/github_hashes/facebookincubator/fizz-rev.txt
@@ -1 +1 @@
-Subproject commit 68bbd0510e69692c337b02c3a79ffb5ae86b167f
+Subproject commit 7de8f52e9878206cd3a0192d9b4295cd58cd2ac9



```

---

### [OK] 0aad204a - Summary:
**Author:** Unknown | **Date:** Fri, 11 Sep 2026 09:32:43 -0700

```diff
GitHub commits:

https://github.com/facebook/CacheLib/commit/b1a46a72bf1bf51a51e046cb55538a019b8fe1f4
https://github.com/facebook/fb303/commit/7bc929b00aebdd50479f5d8a8759c012b15d6ca8
https://github.com/facebook/fbthrift/commit/55441a7ea27c97c872b4d8ccd6c96d5c3ac0b707
https://github.com/facebook/folly/commit/60e497a8b5ff8f94c73ff38a13487b2751ba1b11
https://github.com/facebook/mvfst/commit/46396990721807eb0c317e33fd06bf5a2f31b0b7
https://github.com/facebook/proxygen/commit/39a2b2f688cc2bb85fc20bc9117656f0c8036977
https://github.com/facebook/pyrefly/commit/9bc2b21552e197820fd4228c97e3d36260cc6b19
https://github.com/facebook/wangle/commit/d7eb11f1f988f03e363715b32eb90d70c0324375
https://github.com/facebookexperimental/edencommon/commit/291e2700de29f87ea943975c5b15931ec42015ac
https://github.com/facebookexperimental/rust-shed/commit/feef802457e90a04c730648425712682d06b659e
https://github.com/facebookincubator/fizz/commit/68bbd0510e69692c337b02c3a79ffb5ae86b167f
https://github.com/react/yoga/commit/f42d532527974a4504e644f6836946ad108d4133

Reviewed By: sdwilsh

fbshipit-source-id: 3d8fe2e832559b33590c26503fd726a384f9366a
---
 build/deps/github_hashes/facebook/fb303-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/fbthrift-rev.txt              | 2 +-
 build/deps/github_hashes/facebook/folly-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/mvfst-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/wangle-rev.txt                | 2 +-
 .../deps/github_hashes/facebookexperimental/edencommon-rev.txt  | 2 +-
 build/deps/github_hashes/facebookincubator/fizz-rev.txt         | 2 +-
 7 files changed, 7 insertions(+), 7 deletions(-)

diff --git a/build/deps/github_hashes/facebook/fb303-rev.txt b/build/deps/github_hashes/facebook/fb303-rev.txt
index 312684ffd384..dc12fa587b52 100644
--- a/build/deps/github_hashes/facebook/fb303-rev.txt
+++ b/build/deps/github_hashes/facebook/fb303-rev.txt
@@ -1 +1 @@
-Subproject commit 497bec81e590e757eff1b3a12e8297a40187af44
+Subproject commit 7bc929b00aebdd50479f5d8a8759c012b15d6ca8
diff --git a/build/deps/github_hashes/facebook/fbthrift-rev.txt b/build/deps/github_hashes/facebook/fbthrift-rev.txt
index 2b1615fde4e0..de34d1b3942b 100644
--- a/build/deps/github_hashes/facebook/fbthrift-rev.txt
+++ b/build/deps/github_hashes/facebook/fbthrift-rev.txt
@@ -1 +1 @@
-Subproject commit 734f164178ab3dbd576235a27c74a4af0c0ca3fe
+Subproject commit 55441a7ea27c97c872b4d8ccd6c96d5c3ac0b707
diff --git a/build/deps/github_hashes/facebook/folly-rev.txt b/build/deps/github_hashes/facebook/folly-rev.txt
index 39dfcd678e56..78f03a640409 100644
--- a/build/deps/github_hashes/facebook/folly-rev.txt
+++ b/build/deps/github_hashes/facebook/folly-rev.txt
@@ -1 +1 @@
-Subproject commit 51cb33ad155b8d810c136cfab128cf06bbbaa22d
+Subproject commit 60e497a8b5ff8f94c73ff38a13487b2751ba1b11
diff --git a/build/deps/github_hashes/facebook/mvfst-rev.txt b/build/deps/github_hashes/facebook/mvfst-rev.txt
index 3b3d1a2069ac..3961c4912495 100644
--- a/build/deps/github_hashes/facebook/mvfst-rev.txt
+++ b/build/deps/github_hashes/facebook/mvfst-rev.txt
@@ -1 +1 @@
-Subproject commit 0528b9470d08ce758e30cee1611f011bc22a8ca3
+Subproject commit 46396990721807eb0c317e33fd06bf5a2f31b0b7
diff --git a/build/deps/github_hashes/facebook/wangle-rev.txt b/build/deps/github_hashes/facebook/wangle-rev.txt
index 1e69f95ec94e..40c8062ef04d 100644
--- a/build/deps/github_hashes/facebook/wangle-rev.txt
+++ b/build/deps/github_hashes/facebook/wangle-rev.txt
@@ -1 +1 @@
-Subproject commit 6ef443bedfd182f18c55530c30f97ca0424a57f6
+Subproject commit d7eb11f1f988f03e363715b32eb90d70c0324375
diff --git a/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt b/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
index 7753ccd75a62..2287bc87f8b4 100644
--- a/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
+++ b/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
@@ -1 +1 @@
-Subproject commit da468cbe16ab683bd3d581a58a6082d1f10dc679
+Subproject commit 291e2700de29f87ea943975c5b15931ec42015ac
diff --git a/build/deps/github_hashes/facebookincubator/fizz-rev.txt b/build/deps/github_hashes/facebookincubator/fizz-rev.txt
index 1dab403cea47..734c063867c0 100644
--- a/build/deps/github_hashes/facebookincubator/fizz-rev.txt
+++ b/build/deps/github_hashes/facebookincubator/fizz-rev.txt
@@ -1 +1 @@
-Subproject commit 9583c5240c5ce6f154f445e376bc51b06604baf6
+Subproject commit 68bbd0510e69692c337b02c3a79ffb5ae86b167f



```

---

### [OK] f7359652 - Summary:
**Author:** Unknown | **Date:** Thu, 10 Sep 2026 19:05:52 -0700

```diff
## What

Three changes, all consequences of `add_fbcode_builder` (D119431762) making
`fbcode/opensource/fbcode_builder/` part of the exported repository:

- **`packsim/BUCK`** — add `fbcode/opensource/fbcode_builder/manifests/packsim`
  to `PACKSIM_OSS_CI_SRCS`, so a change to packsim's own getdeps manifest
  schedules the export gate. The comment records why the *rest* of
  `fbcode_builder/` is deliberately left out.
- **`scripts/oss_check.py`** — `check_placement` now builds the shipit path from
  the repository root instead of the packsim root, so `classify` answers for
  every mapped root rather than hard-erroring on all but one. It still refuses a
  path outside the checkout, and marker linting stays packsim-scoped.
- **`opensource/fbcode_builder/manifests/packsim`** — reword the header comment,
  which ships to GitHub and told the reader to run a `buck2` command against an
  fbcode path.

## Why

221 of the exported repository's 647 files now come from
`fbcode/opensource/fbcode_builder/`, and every boundary packsim's tooling
assumes is still drawn at `fbcode/ai_simulation/packsim/`:

- `PACKSIM_OSS_CI_SRCS` matched nothing there, so a diff to packsim's getdeps
  manifest — the file that declares the dependency set the open-source build
  resolves — scheduled no packsim signal at all, on diff or on trunk. This is the
  same gap `PacksimOssExportCheck` (D119234561) closed on the Configerator side,
  reopened in fbsource.
- `oss_check classify` is the tool the `.llms` rules point at for "does this
  ship?", and it answered `outside ai_simulation/packsim, so the export rules say
  nothing about it` for files `materialize` writes into the export. Two halves of
  one tool disagreeing about the export's boundary.

The rest of `fbcode_builder/` is left out on purpose, and the BUCK comment says
so: it belongs to the open_source team, and neither tier would earn its keep —
`--fast` replays `mappingTestCases` and lints packsim's markers, neither of which
reads fbcode_builder's contents, and the full check builds the internal Buck
graph, which fbcode_builder is not part of. Widening the trigger would bill
another team twenty minutes a diff for a signal that cannot see their change.

Marker linting is deliberately NOT widened: `getdeps/test/strip_marker_test.py`
carries `oss-disable` strings as test data, and flagging those would be a false
finding on another team's file. `marker_syntaxes_for` is unchanged and the
`check_placement` docstring records the asymmetry.

Differential Revision: D119582014

fbshipit-source-id: 29889b5f4cc1d2320132b6fb7546e9ca14b2290d
---
 build/fbcode_builder/manifests/packsim | 30 ++++++++++----------------
 1 file changed, 11 insertions(+), 19 deletions(-)

diff --git a/build/fbcode_builder/manifests/packsim b/build/fbcode_builder/manifests/packsim
index 8e0bd66192de..0f410e428878 100644
--- a/build/fbcode_builder/manifests/packsim
+++ b/build/fbcode_builder/manifests/packsim
@@ -2,26 +2,18 @@
 name = packsim
 fbsource_path = fbcode/ai_simulation/packsim
 
-# Deliberately no `shipit_project` / `shipit.pathmap` / `shipit.strip`.
-#
-# Those three would let `getdeps build packsim` transform fbsource in place,
-# but only by restating what the export contains -- and the packsim export is
-# defined solely by
-# opensource/shipit_config/facebook/ai_simulation.cconf in Configerator. A
-# second copy of the strip rules here would drift from it silently, and the
-# symptom would be getdeps reporting a green build of a tree that is not the
-# one ShipIt publishes. That is worse than no answer.
-#
-# The internal way to build the export is instead
+# This manifest builds packsim from its published repository, cloned from the
+# `[git] repo_url` below and configured with the CMakeLists.txt at its root.
 #
-#     buck2 run fbcode//ai_simulation/packsim/scripts:oss_check -- \
-#         materialize /tmp/packsim-export
-#
-# which reads the live config on every run, followed by cmake against that
-# directory. See fbcode/ai_simulation/CMakeLists.txt.
-#
-# Once github.com/facebook/ai_simulation is public, `[git] repo_url` below is
-# enough for external consumers and for OSS CI.
+# Deliberately no `shipit_project` / `shipit.pathmap` / `shipit.strip`.
+# Those three exist to build a project out of a Meta-internal checkout by
+# restating which of its files are public -- but packsim's published contents
+# are defined in exactly one place, and a second copy of those rules here would
+# drift from it silently. The symptom would be a green build of a tree that is
+# not the one that gets published, which is worse than no answer. Meta engineers
+# who need to build the published tree from an internal checkout should use
+# packsim's own export tool rather than getdeps; it reads those rules live on
+# every run.
 
 [git]
 repo_url = https://github.com/facebook/ai_simulation.git



```

---

### [OK] 045ec6a2 - Summary:
**Author:** Unknown | **Date:** Thu, 10 Sep 2026 14:54:36 -0700

```diff
This diff fixes a TSan data race in Watchman telemetry event counters that surfaced while rerunning the EdenFS Watchman sanitizer lane. getLogEventCounters() stored one atomic counter per event type in a lazily populated std::unordered_map; the atomic values were safe to increment, but concurrent find() and emplace() calls still mutated and read the map itself without synchronization.

Replace the mutable map with a fixed array of atomics indexed by LogEventType, plus a fallback atomic for out-of-range values. The array is sized using the documented LogEventTypeCount sentinel rather than relying on DroppedType remaining the final event type. Clamp the configured sampling rate to at least one before using it as a modulo divisor.

The concurrent regression test synchronizes all workers at a start barrier so their first counter accesses overlap. Additional tests verify observable counter progression and that sentinel/out-of-range values share the fallback counter.

Reviewed By: vilatto

Differential Revision: D116387734

fbshipit-source-id: 9458e2e57e2c7e1e856b41c4fd994aadd78d06c1
---
 watchman/telemetry/LogEvent.cpp          | 25 ++++----
 watchman/telemetry/LogEvent.h            |  4 +-
 watchman/telemetry/test/BUCK             | 12 ++++
 watchman/telemetry/test/LogEventTest.cpp | 81 ++++++++++++++++++++++++
 4 files changed, 109 insertions(+), 13 deletions(-)
 create mode 100644 watchman/telemetry/test/LogEventTest.cpp

diff --git a/watchman/telemetry/LogEvent.cpp b/watchman/telemetry/LogEvent.cpp
index 0b72cf3dd147..85790ff0b486 100644
--- a/watchman/telemetry/LogEvent.cpp
+++ b/watchman/telemetry/LogEvent.cpp
@@ -5,7 +5,9 @@
  * LICENSE file in the root directory of this source tree.
  */
 
-#include <unordered_map>
+#include <algorithm>
+#include <array>
+#include <atomic>
 
 #include "watchman/WatchmanConfig.h"
 #include "watchman/telemetry/LogEvent.h"
@@ -13,18 +15,17 @@
 namespace watchman {
 
 std::pair<int64_t, int64_t> getLogEventCounters(const LogEventType& type) {
-  static std::unordered_map<LogEventType, std::atomic_int64_t> eventCounters;
-  static int64_t samplingRate = cfg_get_int("scribe-sampling-rate", 100);
+  static std::array<std::atomic_int64_t, LogEventTypeCount> eventCounters{};
+  static std::atomic_int64_t unknownEventCounter{};
+  static const int64_t samplingRate =
+      std::max<int64_t>(cfg_get_int("scribe-sampling-rate", 100), 1);
 
-  // Find event counter or add if missing - init to 0;
-  auto it = eventCounters.find(type);
-  if (it == eventCounters.end()) {
-    it = eventCounters.emplace(type, 0).first;
-  }
-
-  // Return sampling rate and event count
-  auto& eventCounter = it->second;
-  auto eventCount = ++eventCounter % samplingRate;
+  const auto eventIndex = static_cast<size_t>(type);
+  auto& eventCounter = eventIndex < eventCounters.size()
+      ? eventCounters[eventIndex]
+      : unknownEventCounter;
+  auto eventCount =
+      (eventCounter.fetch_add(1, std::memory_order_relaxed) + 1) % samplingRate;
   return std::make_pair(samplingRate, eventCount ? eventCount : samplingRate);
 }
 
diff --git a/watchman/telemetry/LogEvent.h b/watchman/telemetry/LogEvent.h
index 149943754393..2e6fa86e8ef7 100644
--- a/watchman/telemetry/LogEvent.h
+++ b/watchman/telemetry/LogEvent.h
@@ -89,7 +89,9 @@ enum LogEventType : uint8_t {
   SavedStateType,
   QueryExecuteType,
   FullCrawlType,
-  DroppedType
+  DroppedType,
+  // Sentinel used to size counter storage; this is not an event type.
+  LogEventTypeCount,
 };
 
 // Returns samplingRate and eventCount
diff --git a/watchman/telemetry/test/BUCK b/watchman/telemetry/test/BUCK
index 00ff76fb4e6b..7975388a8b82 100644
--- a/watchman/telemetry/test/BUCK
+++ b/watchman/telemetry/test/BUCK
@@ -3,6 +3,18 @@ load("@fbsource//tools/build_defs/testinfra:network_access_utils.bzl", "network_
 
 oncall("fbcode_buck2_contbuilds")
 
+cpp_unittest(
+    name = "log_event_test",
+    srcs = [
+        "LogEventTest.cpp",
+    ],
+    network_access = network_access_utils.none(),
+    deps = [
+        "//folly/portability:gtest",
+        "//watchman/telemetry:telemetry",
+    ],
+)
+
 cpp_unittest(
     name = "watchman_xplat_structured_logger_test",
     srcs = [
diff --git a/watchman/telemetry/test/LogEventTest.cpp b/watchman/telemetry/test/LogEventTest.cpp
new file mode 100644
index 000000000000..25ec87a0c885
--- /dev/null
+++ b/watchman/telemetry/test/LogEventTest.cpp
@@ -0,0 +1,81 @@
+/*
+ * Copyright (c) Meta Platforms, Inc. and affiliates.
+ *
+ * This source code is licensed under the MIT license found in the
+ * LICENSE file in the root directory of this source tree.
+ */
+
+#include "watchman/telemetry/LogEvent.h"
+
+#include <atomic>
+#include <barrier>
+#include <cstddef>
+#include <thread>
+#include <vector>
+
+#include <folly/portability/GTest.h>
+
+namespace {
+
+int64_t nextCounterValue(int64_t value, int64_t samplingRate) {
+  return value == samplingRate ? 1 : value + 1;
+}
+
+TEST(LogEventTest, incrementsEventCounter) {
+  const auto [samplingRate, firstCount] =
+      watchman::getLogEventCounters(watchman::DroppedType);
+  const auto [nextSamplingRate, secondCount] =
+      watchman::getLogEventCounters(watchman::DroppedType);
+
+  EXPECT_GT(samplingRate, 0);
+  EXPECT_EQ(samplingRate, nextSamplingRate);
+  EXPECT_EQ(nextCounterValue(firstCount, samplingRate), secondCount);
+}
+
+TEST(LogEventTest, sharesFallbackCounterForInvalidEventTypes) {
+  const auto sentinel = watchman::LogEventTypeCount;
+  const auto beyondSentinel =
+      static_cast<watchman::LogEventType>(watchman::LogEventTypeCount + 1);
+
+  const auto [samplingRate, firstCount] =
+      watchman::getLogEventCounters(sentinel);
+  const auto [nextSamplingRate, secondCount] =
+      watchman::getLogEventCounters(beyondSentinel);
+
+  EXPECT_GT(samplingRate, 0);
+  EXPECT_EQ(samplingRate, nextSamplingRate);
+  EXPECT_EQ(nextCounterValue(firstCount, samplingRate), secondCount);
+}
+
+TEST(LogEventTest, supportsConcurrentEventCounterAccess) {
+  constexpr size_t kThreadCount = 8;
+  constexpr size_t kIterations = 1024;
+
+  std::atomic<size_t> invalidResults{0};
+  std::barrier startBarrier{static_cast<std::ptrdiff_t>(kThreadCount)};
+  std::vector<std::thread> threads;
+  threads.reserve(kThreadCount);
+
+  for (size_t threadIndex = 0; threadIndex < kThreadCount; ++threadIndex) {
+    threads.emplace_back([threadIndex, &invalidResults, &startBarrier] {
+      startBarrier.arrive_and_wait();
+      for (size_t iteration = 0; iteration < kIterations; ++iteration) {
+        const auto type = static_cast<watchman::LogEventType>(
+            (threadIndex + iteration) % watchman::LogEventTypeCount);
+        const auto [samplingRate, eventCount] =
+            watchman::getLogEventCounters(type);
+        if (samplingRate <= 0 || eventCount <= 0 || eventCount > samplingRate) {
+          ++invalidResults;
+        }
+      }
+    });
+  }
+
+  for (auto& thread : threads) {
+    thread.join();
+  }
+
+  EXPECT_EQ(0, invalidResults.load());
+}
+
+} // namespace



```

---

### [OK] 8b8d9084 - Summary:
**Author:** Unknown | **Date:** Thu, 10 Sep 2026 09:32:38 -0700

```diff
GitHub commits:

https://github.com/WhatsApp/erlang-language-platform/commit/778d4b5486cd6ab7ae832d671919c863e142ac97
https://github.com/facebook/CacheLib/commit/cec8e02d7b28a97317c539d98a7dfae4e3778659
https://github.com/facebook/fb303/commit/497bec81e590e757eff1b3a12e8297a40187af44
https://github.com/facebook/fbthrift/commit/734f164178ab3dbd576235a27c74a4af0c0ca3fe
https://github.com/facebook/folly/commit/51cb33ad155b8d810c136cfab128cf06bbbaa22d
https://github.com/facebook/mvfst/commit/0528b9470d08ce758e30cee1611f011bc22a8ca3
https://github.com/facebook/proxygen/commit/3562bad72a0d70a54bd07805d22e43401cf43623
https://github.com/facebook/pyrefly/commit/cb178b986fd37a81f45609387e29263455487e74
https://github.com/facebook/wangle/commit/6ef443bedfd182f18c55530c30f97ca0424a57f6
https://github.com/facebookexperimental/edencommon/commit/da468cbe16ab683bd3d581a58a6082d1f10dc679
https://github.com/facebookexperimental/rust-shed/commit/61106b2bb66b02e3bd63a3d95275ed7399779a54
https://github.com/facebookincubator/fizz/commit/9583c5240c5ce6f154f445e376bc51b06604baf6
https://github.com/meta-quest/Meta-VR-OS-SDK-Samples/commit/3d33dc50e1f2009250eb1092c3da636e8a00dbce

Reviewed By: sdwilsh

fbshipit-source-id: 3ab56adb2c1dd7594af12128740c792af3346a5f
---
 build/deps/github_hashes/facebook/fb303-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/fbthrift-rev.txt              | 2 +-
 build/deps/github_hashes/facebook/folly-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/mvfst-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/wangle-rev.txt                | 2 +-
 .../deps/github_hashes/facebookexperimental/edencommon-rev.txt  | 2 +-
 build/deps/github_hashes/facebookincubator/fizz-rev.txt         | 2 +-
 7 files changed, 7 insertions(+), 7 deletions(-)

diff --git a/build/deps/github_hashes/facebook/fb303-rev.txt b/build/deps/github_hashes/facebook/fb303-rev.txt
index 29626212bf40..312684ffd384 100644
--- a/build/deps/github_hashes/facebook/fb303-rev.txt
+++ b/build/deps/github_hashes/facebook/fb303-rev.txt
@@ -1 +1 @@
-Subproject commit d5a38e9301af2d8f465b43ac7b905f90335fbfeb
+Subproject commit 497bec81e590e757eff1b3a12e8297a40187af44
diff --git a/build/deps/github_hashes/facebook/fbthrift-rev.txt b/build/deps/github_hashes/facebook/fbthrift-rev.txt
index 86d94bda3ae5..2b1615fde4e0 100644
--- a/build/deps/github_hashes/facebook/fbthrift-rev.txt
+++ b/build/deps/github_hashes/facebook/fbthrift-rev.txt
@@ -1 +1 @@
-Subproject commit 79bcd6fba237673b18c6da184bb97311033df90c
+Subproject commit 734f164178ab3dbd576235a27c74a4af0c0ca3fe
diff --git a/build/deps/github_hashes/facebook/folly-rev.txt b/build/deps/github_hashes/facebook/folly-rev.txt
index f711f2bd182f..39dfcd678e56 100644
--- a/build/deps/github_hashes/facebook/folly-rev.txt
+++ b/build/deps/github_hashes/facebook/folly-rev.txt
@@ -1 +1 @@
-Subproject commit 2241100cafb3c63dc895a4c8a794e10964b039fa
+Subproject commit 51cb33ad155b8d810c136cfab128cf06bbbaa22d
diff --git a/build/deps/github_hashes/facebook/mvfst-rev.txt b/build/deps/github_hashes/facebook/mvfst-rev.txt
index 610025c75fb0..3b3d1a2069ac 100644
--- a/build/deps/github_hashes/facebook/mvfst-rev.txt
+++ b/build/deps/github_hashes/facebook/mvfst-rev.txt
@@ -1 +1 @@
-Subproject commit 4c2e15cb8e20ace5fd871f1d677177e4c87e0455
+Subproject commit 0528b9470d08ce758e30cee1611f011bc22a8ca3
diff --git a/build/deps/github_hashes/facebook/wangle-rev.txt b/build/deps/github_hashes/facebook/wangle-rev.txt
index 8d3be2ea5c49..1e69f95ec94e 100644
--- a/build/deps/github_hashes/facebook/wangle-rev.txt
+++ b/build/deps/github_hashes/facebook/wangle-rev.txt
@@ -1 +1 @@
-Subproject commit 6e9bc08788c33045f5070734efc82f85ab11e433
+Subproject commit 6ef443bedfd182f18c55530c30f97ca0424a57f6
diff --git a/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt b/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
index 06f0778d5163..7753ccd75a62 100644
--- a/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
+++ b/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
@@ -1 +1 @@
-Subproject commit 3acf82a654f7cc7c83b1f6a61ba220787d445493
+Subproject commit da468cbe16ab683bd3d581a58a6082d1f10dc679
diff --git a/build/deps/github_hashes/facebookincubator/fizz-rev.txt b/build/deps/github_hashes/facebookincubator/fizz-rev.txt
index 831837fe115e..1dab403cea47 100644
--- a/build/deps/github_hashes/facebookincubator/fizz-rev.txt
+++ b/build/deps/github_hashes/facebookincubator/fizz-rev.txt
@@ -1 +1 @@
-Subproject commit f984b37ad65dae7e1e4183f1f714aeae6195e6e6
+Subproject commit 9583c5240c5ce6f154f445e376bc51b06604baf6



```

---

### [OK] 3cc641d2 - Summary: Represent each `Glob.matchingFiles` element as the existing `GlobPath` C++ type through an extracted typedef. This removes the container-level `cpp.Type{name = ...}` annotation while preserving the binary wire type and the existing specialized serialization.
**Author:** Unknown | **Date:** Wed, 9 Sep 2026 14:54:36 -0700

```diff
Reviewed By: hchokshi, vitaut

Differential Revision: D117759258

fbshipit-source-id: 5bca1120335182172f70029f8ad9e9029d2bf3a4
---
 eden/fs/service/eden.thrift | 6 ++++--
 1 file changed, 4 insertions(+), 2 deletions(-)

diff --git a/eden/fs/service/eden.thrift b/eden/fs/service/eden.thrift
index b180e34a4ef9..47478e8e7240 100644
--- a/eden/fs/service/eden.thrift
+++ b/eden/fs/service/eden.thrift
@@ -107,6 +107,9 @@ typedef binary BinaryHash
  */
 typedef binary PathString
 
+@cpp.Type{name = "::facebook::eden::GlobPath"}
+typedef PathString GlobPathValue
+
 /**
  * Bit set indicating where data should be fetched from in our debugging
  * commands.
@@ -1820,8 +1823,7 @@ struct Glob {
    * sorted. However, no duplicates may have the same originCommits (note this
    * is not true should the input GlobParams contain duplicate revisions) .
    */
-  @cpp.Type{name = "::facebook::eden::GlobPathList"}
-  1: list<PathString> matchingFiles;
+  1: list<GlobPathValue> matchingFiles;
   2: list<OsDtype> dtypes;
   /**
    * Currently these are the commit hash for the commit to which this file



```

---

### [OK] b5f402dc - Summary:
**Author:** Unknown | **Date:** Wed, 9 Sep 2026 14:13:51 -0700

```diff
X-link: https://github.com/facebookexperimental/moxygen/pull/229

The gperf manifest downloads its tarball from `ftpmirror.gnu.org`, which
redirects to a randomly chosen GNU mirror. Version 3.3 is recent enough that
many mirrors have not picked it up yet, so the download either 404s or returns
a stale file that fails the recorded checksum. Point the manifest at
`ftp.gnu.org`, the canonical host, which always has the release. The `sha256`
is unchanged since it is the same tarball.

Reviewed By: jbeshay

Differential Revision: D119370094

fbshipit-source-id: e29c50ca8d35f3acb4efbb3cc3150900fd100765
---
 build/fbcode_builder/manifests/gperf | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

diff --git a/build/fbcode_builder/manifests/gperf b/build/fbcode_builder/manifests/gperf
index 2a9ca2c1631f..a03ca9a4cafa 100644
--- a/build/fbcode_builder/manifests/gperf
+++ b/build/fbcode_builder/manifests/gperf
@@ -2,7 +2,7 @@
 name = gperf
 
 [download]
-url = https://ftpmirror.gnu.org/gnu/gperf/gperf-3.3.tar.gz
+url = https://ftp.gnu.org/gnu/gperf/gperf-3.3.tar.gz
 sha256 = fd87e0aba7e43ae054837afd6cd4db03a3f2693deb3619085e6ed9d8d9604ad8
 
 [build.not(os=windows)]



```

---

### [OK] ac9297ce - Summary:
**Author:** Unknown | **Date:** Wed, 9 Sep 2026 09:33:14 -0700

```diff
GitHub commits:

https://github.com/WhatsApp/erlang-language-platform/commit/0ea66eebcb4ac9534c5faea6574f0ea8c8e87e7f
https://github.com/facebook/CacheLib/commit/cca2f527e5bb732eed31dca0065085160f29b143
https://github.com/facebook/fb303/commit/d5a38e9301af2d8f465b43ac7b905f90335fbfeb
https://github.com/facebook/fbthrift/commit/79bcd6fba237673b18c6da184bb97311033df90c
https://github.com/facebook/folly/commit/2241100cafb3c63dc895a4c8a794e10964b039fa
https://github.com/facebook/mvfst/commit/4c2e15cb8e20ace5fd871f1d677177e4c87e0455
https://github.com/facebook/proxygen/commit/ef867c24fd47289dfad7b83a6020edd924b5e38b
https://github.com/facebook/pyrefly/commit/73f044a543c1e4f046bcf30510928e1da2f7f7e2
https://github.com/facebook/wangle/commit/6e9bc08788c33045f5070734efc82f85ab11e433
https://github.com/facebookexperimental/edencommon/commit/3acf82a654f7cc7c83b1f6a61ba220787d445493
https://github.com/facebookexperimental/rust-shed/commit/cb6041786e472d9158c5d8172da602a518ff2317
https://github.com/facebookincubator/fizz/commit/f984b37ad65dae7e1e4183f1f714aeae6195e6e6
https://github.com/react/yoga/commit/73bebc0d35ca3eaa7c7c972dbbc3695677599f7d

Reviewed By: sdwilsh

fbshipit-source-id: 982e52b05a8cf4972b49679e84ff35b87bb9d061
---
 build/deps/github_hashes/facebook/fb303-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/fbthrift-rev.txt              | 2 +-
 build/deps/github_hashes/facebook/folly-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/mvfst-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/wangle-rev.txt                | 2 +-
 .../deps/github_hashes/facebookexperimental/edencommon-rev.txt  | 2 +-
 build/deps/github_hashes/facebookincubator/fizz-rev.txt         | 2 +-
 7 files changed, 7 insertions(+), 7 deletions(-)

diff --git a/build/deps/github_hashes/facebook/fb303-rev.txt b/build/deps/github_hashes/facebook/fb303-rev.txt
index 7ebf12338e38..29626212bf40 100644
--- a/build/deps/github_hashes/facebook/fb303-rev.txt
+++ b/build/deps/github_hashes/facebook/fb303-rev.txt
@@ -1 +1 @@
-Subproject commit 54d95629de8ab6615691d55041ce9b3d43143320
+Subproject commit d5a38e9301af2d8f465b43ac7b905f90335fbfeb
diff --git a/build/deps/github_hashes/facebook/fbthrift-rev.txt b/build/deps/github_hashes/facebook/fbthrift-rev.txt
index aeca30228c6a..86d94bda3ae5 100644
--- a/build/deps/github_hashes/facebook/fbthrift-rev.txt
+++ b/build/deps/github_hashes/facebook/fbthrift-rev.txt
@@ -1 +1 @@
-Subproject commit d5c34ef4f3c9404129f08baf851fcaff606abe8b
+Subproject commit 79bcd6fba237673b18c6da184bb97311033df90c
diff --git a/build/deps/github_hashes/facebook/folly-rev.txt b/build/deps/github_hashes/facebook/folly-rev.txt
index 538410710c54..f711f2bd182f 100644
--- a/build/deps/github_hashes/facebook/folly-rev.txt
+++ b/build/deps/github_hashes/facebook/folly-rev.txt
@@ -1 +1 @@
-Subproject commit 1e0a1b4dd4cebceef64c76bf63ed8be1984474fa
+Subproject commit 2241100cafb3c63dc895a4c8a794e10964b039fa
diff --git a/build/deps/github_hashes/facebook/mvfst-rev.txt b/build/deps/github_hashes/facebook/mvfst-rev.txt
index b0d97ca4ea3b..610025c75fb0 100644
--- a/build/deps/github_hashes/facebook/mvfst-rev.txt
+++ b/build/deps/github_hashes/facebook/mvfst-rev.txt
@@ -1 +1 @@
-Subproject commit d98bb4a80878502de793189e3ea2574c3c07da60
+Subproject commit 4c2e15cb8e20ace5fd871f1d677177e4c87e0455
diff --git a/build/deps/github_hashes/facebook/wangle-rev.txt b/build/deps/github_hashes/facebook/wangle-rev.txt
index f8b22a28297a..8d3be2ea5c49 100644
--- a/build/deps/github_hashes/facebook/wangle-rev.txt
+++ b/build/deps/github_hashes/facebook/wangle-rev.txt
@@ -1 +1 @@
-Subproject commit 4c8d04b1bf00360cc5cc6a3bd7bdb70c4edc355a
+Subproject commit 6e9bc08788c33045f5070734efc82f85ab11e433
diff --git a/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt b/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
index 8fa9a7f8c7a6..06f0778d5163 100644
--- a/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
+++ b/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
@@ -1 +1 @@
-Subproject commit 9035d18a70626d1deafa8d39546072bdb6de07f7
+Subproject commit 3acf82a654f7cc7c83b1f6a61ba220787d445493
diff --git a/build/deps/github_hashes/facebookincubator/fizz-rev.txt b/build/deps/github_hashes/facebookincubator/fizz-rev.txt
index 2dba0b26ba1d..831837fe115e 100644
--- a/build/deps/github_hashes/facebookincubator/fizz-rev.txt
+++ b/build/deps/github_hashes/facebookincubator/fizz-rev.txt
@@ -1 +1 @@
-Subproject commit 963f906bb5f08cea100b79fe7dcd45594949cc9e
+Subproject commit f984b37ad65dae7e1e4183f1f714aeae6195e6e6



```

---

### [OK] 45995eab - Summary:
**Author:** Unknown | **Date:** Wed, 9 Sep 2026 09:08:18 -0700

```diff
The install RPATH was an absolute path baked in at configure time:

```cmake
set(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/${LIB_INSTALL_DIR}")
set(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)
```

That makes the installation non-relocatable — move the prefix and installed
binaries still point at the old one — and it embedded every linked dependency
directory into every artifact.

Installed artifacts now find fbthrift's own shared libraries relative to
themselves, via two entries appended to `CMAKE_INSTALL_RPATH`:
`$ORIGIN` and `$ORIGIN/../<libdir>` on Linux, `loader_path` and
`loader_path/../<libdir>` on macOS. The hop is computed from
`CMAKE_INSTALL_BINDIR` and `CMAKE_INSTALL_LIBDIR` rather than hard-coding
`lib`, so it follows `lib64`. Windows is untouched; it has no RPATH.

Both entries go on every artifact rather than being split per target type.
Libraries resolve through the first and executables in bindir through the
second; the unused entry in each case is a directory that holds no libraries,
so it costs one empty search path and nothing else. That keeps this to a
single list at the top level instead of a helper plus a call next to every
installed executable.

Two details worth attention:

- The list is appended to, not assigned. fbcode_builder passes
  `-DCMAKE_INSTALL_RPATH=<translated DYLD_LIBRARY_PATH>` on macOS, and the
  old unconditional `set()` created a normal variable that shadowed it,
  silently discarding what getdeps asked for. Appending honors it and adds
  ours. Same for a user-supplied `-DCMAKE_INSTALL_RPATH=...`.
- `CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE` is removed from the CMake build
  and set in the two getdeps manifests instead. getdeps installs each
  dependency to its own prefix, so it genuinely needs those directories
  recorded — but that is a getdeps requirement, not something every consumer
  of the OSS build should inherit.

Reviewed By: iahs

Differential Revision: D119211405

fbshipit-source-id: 6c913dd62a36b5e0ffb4aa9e0768ac8aa4ec475e
---
 build/fbcode_builder/manifests/fbthrift        | 5 +++++
 build/fbcode_builder/manifests/fbthrift-python | 5 +++++
 2 files changed, 10 insertions(+)

diff --git a/build/fbcode_builder/manifests/fbthrift b/build/fbcode_builder/manifests/fbthrift
index a7164287c88d..f40cb3da445d 100644
--- a/build/fbcode_builder/manifests/fbthrift
+++ b/build/fbcode_builder/manifests/fbthrift
@@ -17,6 +17,11 @@ fbthrift = thrift/lib/rust
 builder = cmake
 job_weight_mib = 2048
 
+[cmake.defines]
+# Each dependency has its own prefix here, so record their lib dirs too;
+# fbthrift's CMake only adds relative entries for its own libraries.
+CMAKE_INSTALL_RPATH_USE_LINK_PATH=ON
+
 [cmake.defines.all(not(os=windows),test=on)]
 enable_tests=ON
 
diff --git a/build/fbcode_builder/manifests/fbthrift-python b/build/fbcode_builder/manifests/fbthrift-python
index b3ee02cb0b54..58980c620454 100644
--- a/build/fbcode_builder/manifests/fbthrift-python
+++ b/build/fbcode_builder/manifests/fbthrift-python
@@ -20,6 +20,11 @@ job_weight_mib = 2048
 [build.not(os=linux)]
 builder = nop
 
+[cmake.defines]
+# Each dependency has its own prefix here, so record their lib dirs too;
+# fbthrift's CMake only adds relative entries for its own libraries.
+CMAKE_INSTALL_RPATH_USE_LINK_PATH=ON
+
 [cmake.defines.all(not(os=windows),test=on)]
 enable_tests=ON
 



```

---

### [OK] c7e6e521 - Summary:
**Author:** Unknown | **Date:** Tue, 8 Sep 2026 11:27:08 -0700

```diff
## What

Adds the open-source build for `github.com/facebook/ai_simulation`:

- **`fbcode/ai_simulation/CMakeLists.txt`** -- the whole build. 16 thrift
  codegen steps, one static library over the 83 shipping non-test sources, the
  `packsim` / `packsim-replay` / `coro_demo` binaries, and 163 test executables
  (one per test source, matching the internal one-`.cpp`-per-`cpp_unittest`
  shape). It is hand-written against the EXPORT's layout, not generated from
  the Buck graph.
- **`opensource/fbcode_builder/manifests/packsim`** -- the getdeps manifest
  declaring the dependency set.
- **`oss_check materialize <dir>`** -- writes the export to a directory laid out
  as the GitHub repo is. Without it the CMake could not be run at all, let alone
  tested; see *Why `materialize`* below.
- **`.llms/rules/oss-export-compliance.md`** -- documents both, and corrects the
  now-false claim that the export has no build system.

Sources are globbed, not enumerated: what ships is the shipit config's decision,
and a list here could only ever disagree with it. The one thing that must be
enumerated is the IDL set, because each `.thrift` gets its own codegen step --
so the build fails at configure time, naming both lists, if
`PACKSIM_THRIFT_FILES` drifts from `packsim/if/*.thrift`.

**Companion Configerator diff: D119058158, which must land with this one.**
`fbcode/ai_simulation/CMakeLists.txt` does not ship without it -- the sibling
rule strips everything at that level that is not `packsim/`. Neither diff does
anything useful alone.

## Why

The export is 415 sources with no way to compile them. Every `BUCK`, `PACKAGE`
and `.bzl` file is removed by ShipIt's shared strip list, so an external user
who cloned the repo today could read packsim but not build it. This is the long
pole on the open-sourcing plan (P2493484069, item 2).

Shipping the Buck files instead is not an option and is not the goal: they load
`fbcode_macros//...` and `fbsource//tools/...`, which have no open-source
prelude equivalent.

### The dependency set is seven libraries, not four

The plan recorded "no includes outside packsim + folly/fmt/fbthrift/gtest".
Enumerating every angled include in the exported content, it is:

| Dependency | Distinct headers | Include sites |
|---|---|---|
| folly | 43 | 272 |
| gtest | 2 | 173 |
| fmt | 3 | 78 |
| gmock | 2 | 54 |
| fbthrift runtime | 3 | 34 |
| gflags | 1 | 9 |
| boost (header-only) | 2 | 2 |
| magic_enum | 1 | 1 |
| POSIX (`fcntl.h`, `unistd.h`, `sys/resource.h`) | 3 | 3 |

All seven are permissively licensed and compatible with the Apache-2.0 LICENSE
the repo ships. packsim declares no thrift services, so the manifest asks for
`fbthrift = !rpc, !benchmark` and the build links
`thriftprotocol`/`thriftmetadata`/`thrifttype` rather than `thriftcpp2` --
keeping fizz, wangle and mvfst out of the graph. Precedent:
`manifests/rebalancer`, which does the same for the same reason.

### Why `materialize`

`transform` and the new `materialize` apply different halves of the export.
`transform` rewrites marked lines in place and deliberately leaves paths alone,
because the internal Buck graph has to keep resolving -- that is what `check`
builds. The CMake build needs the other half too: the repository layout, where
packsim sits under `packsim/`, and the `cppIncludeMappings` rewrite that makes
the sources' `#include`s agree with it. Doing that in place would rewrite every
include in the working copy, so it happens out of tree.

Like everything else in `oss_check`, it reads the live config on every run and
records no copy of what ships. `_export_inputs` is driven by `pathMappings`
rather than by walking packsim, which is what lets it pick up a repo-level file
the root mapping also covers.

Differential Revision: D119058074

fbshipit-source-id: 570f98fdda30c39cf222a2b9342dcc876f6a08ef
---
 build/fbcode_builder/manifests/packsim | 52 ++++++++++++++++++++++++++
 1 file changed, 52 insertions(+)
 create mode 100644 build/fbcode_builder/manifests/packsim

diff --git a/build/fbcode_builder/manifests/packsim b/build/fbcode_builder/manifests/packsim
new file mode 100644
index 000000000000..8e0bd66192de
--- /dev/null
+++ b/build/fbcode_builder/manifests/packsim
@@ -0,0 +1,52 @@
+[manifest]
+name = packsim
+fbsource_path = fbcode/ai_simulation/packsim
+
+# Deliberately no `shipit_project` / `shipit.pathmap` / `shipit.strip`.
+#
+# Those three would let `getdeps build packsim` transform fbsource in place,
+# but only by restating what the export contains -- and the packsim export is
+# defined solely by
+# opensource/shipit_config/facebook/ai_simulation.cconf in Configerator. A
+# second copy of the strip rules here would drift from it silently, and the
+# symptom would be getdeps reporting a green build of a tree that is not the
+# one ShipIt publishes. That is worse than no answer.
+#
+# The internal way to build the export is instead
+#
+#     buck2 run fbcode//ai_simulation/packsim/scripts:oss_check -- \
+#         materialize /tmp/packsim-export
+#
+# which reads the live config on every run, followed by cmake against that
+# directory. See fbcode/ai_simulation/CMakeLists.txt.
+#
+# Once github.com/facebook/ai_simulation is public, `[git] repo_url` below is
+# enough for external consumers and for OSS CI.
+
+[git]
+repo_url = https://github.com/facebook/ai_simulation.git
+
+[build]
+builder = cmake
+
+[cmake.defines.test=on]
+BUILD_TESTS=ON
+
+[cmake.defines.test=off]
+BUILD_TESTS=OFF
+
+# packsim's dependency set is exhaustive: every angled include in the export
+# resolves to one of these, to the C++ standard library, or to
+# <fcntl.h>/<unistd.h>/<sys/resource.h>.
+#
+# fbthrift is used for codegen and (de)serialization only -- packsim declares
+# no thrift services -- so the RPC transport stack (fizz, wangle, mvfst,
+# libsodium) and the benchmark executables are skipped.
+[dependencies]
+boost
+fbthrift = !rpc, !benchmark
+fmt
+folly
+gflags
+googletest
+magic_enum



```

---

### [OK] 9677d500 - Summary:
**Author:** Unknown | **Date:** Tue, 8 Sep 2026 09:33:01 -0700

```diff
GitHub commits:

https://github.com/WhatsApp/erlang-language-platform/commit/b36a64300e07b23a295ac6cd76b37997e647ca30
https://github.com/facebook/CacheLib/commit/80ca7e85960469d0554b51e844a040da369b5c4e
https://github.com/facebook/fb303/commit/54d95629de8ab6615691d55041ce9b3d43143320
https://github.com/facebook/fbthrift/commit/d5c34ef4f3c9404129f08baf851fcaff606abe8b
https://github.com/facebook/folly/commit/1e0a1b4dd4cebceef64c76bf63ed8be1984474fa
https://github.com/facebook/proxygen/commit/f40829a298a1668dfc69a35ffcf6a8d274e1178e
https://github.com/facebook/pyrefly/commit/eb1548d976209c45826423d499abd88ac22e27d2
https://github.com/facebookexperimental/rust-shed/commit/cc3ef12e4827782c38b57a48a0d1148fcce9921e

Reviewed By: sdwilsh

fbshipit-source-id: 9cf66b5d50e21aa777745116ad0111666e37f7b2
---
 build/deps/github_hashes/facebook/fb303-rev.txt    | 2 +-
 build/deps/github_hashes/facebook/fbthrift-rev.txt | 2 +-
 build/deps/github_hashes/facebook/folly-rev.txt    | 2 +-
 3 files changed, 3 insertions(+), 3 deletions(-)

diff --git a/build/deps/github_hashes/facebook/fb303-rev.txt b/build/deps/github_hashes/facebook/fb303-rev.txt
index 54cb09cda0d9..7ebf12338e38 100644
--- a/build/deps/github_hashes/facebook/fb303-rev.txt
+++ b/build/deps/github_hashes/facebook/fb303-rev.txt
@@ -1 +1 @@
-Subproject commit e19e4b933efe7f45fec7e4a30bc00fe0d1df3ed2
+Subproject commit 54d95629de8ab6615691d55041ce9b3d43143320
diff --git a/build/deps/github_hashes/facebook/fbthrift-rev.txt b/build/deps/github_hashes/facebook/fbthrift-rev.txt
index f20c4f5dfb32..aeca30228c6a 100644
--- a/build/deps/github_hashes/facebook/fbthrift-rev.txt
+++ b/build/deps/github_hashes/facebook/fbthrift-rev.txt
@@ -1 +1 @@
-Subproject commit 3e73ebb14d65b9cff855f6c3ad17daee5730031b
+Subproject commit d5c34ef4f3c9404129f08baf851fcaff606abe8b
diff --git a/build/deps/github_hashes/facebook/folly-rev.txt b/build/deps/github_hashes/facebook/folly-rev.txt
index 36f3fcdadc2d..538410710c54 100644
--- a/build/deps/github_hashes/facebook/folly-rev.txt
+++ b/build/deps/github_hashes/facebook/folly-rev.txt
@@ -1 +1 @@
-Subproject commit 65749da412d92fe27d87ae45a8ef670720712858
+Subproject commit 1e0a1b4dd4cebceef64c76bf63ed8be1984474fa



```

---

### [OK] a6ec3997 - Summary:
**Author:** Unknown | **Date:** Mon, 7 Sep 2026 09:34:14 -0700

```diff
GitHub commits:

https://github.com/WhatsApp/erlang-language-platform/commit/1b4d7a44a39f2870028b8a0a92c73d1a0f7cbf36
https://github.com/facebook/CacheLib/commit/a337563b9b809ce335ed1a702e96a3f73527ce07
https://github.com/facebook/fb303/commit/e19e4b933efe7f45fec7e4a30bc00fe0d1df3ed2
https://github.com/facebook/fbthrift/commit/3e73ebb14d65b9cff855f6c3ad17daee5730031b
https://github.com/facebook/mvfst/commit/d98bb4a80878502de793189e3ea2574c3c07da60
https://github.com/facebook/proxygen/commit/c7d0ba74842ef93e3fd513c4e7b7740cb7f3a55f
https://github.com/facebook/pyrefly/commit/ecc71a463f5f1988ccadb9cf9d4b4dbd05a77c6c
https://github.com/facebook/wangle/commit/4c8d04b1bf00360cc5cc6a3bd7bdb70c4edc355a
https://github.com/facebookexperimental/rust-shed/commit/2d06e06f607c34b22543f888f707bf04aec47ec8

Reviewed By: sdwilsh

fbshipit-source-id: 9be5b3d710a607aaa32bea3c7c23c74e1be3493d
---
 build/deps/github_hashes/facebook/fb303-rev.txt    | 2 +-
 build/deps/github_hashes/facebook/fbthrift-rev.txt | 2 +-
 build/deps/github_hashes/facebook/mvfst-rev.txt    | 2 +-
 build/deps/github_hashes/facebook/wangle-rev.txt   | 2 +-
 4 files changed, 4 insertions(+), 4 deletions(-)

diff --git a/build/deps/github_hashes/facebook/fb303-rev.txt b/build/deps/github_hashes/facebook/fb303-rev.txt
index f86ae5580918..54cb09cda0d9 100644
--- a/build/deps/github_hashes/facebook/fb303-rev.txt
+++ b/build/deps/github_hashes/facebook/fb303-rev.txt
@@ -1 +1 @@
-Subproject commit 9e155e57d95fc571ebafd4bbbf96c5c4abcef0a2
+Subproject commit e19e4b933efe7f45fec7e4a30bc00fe0d1df3ed2
diff --git a/build/deps/github_hashes/facebook/fbthrift-rev.txt b/build/deps/github_hashes/facebook/fbthrift-rev.txt
index d5ccac5d88f8..f20c4f5dfb32 100644
--- a/build/deps/github_hashes/facebook/fbthrift-rev.txt
+++ b/build/deps/github_hashes/facebook/fbthrift-rev.txt
@@ -1 +1 @@
-Subproject commit 037bfb80663ed4c82e01de02309bd0c5445dee40
+Subproject commit 3e73ebb14d65b9cff855f6c3ad17daee5730031b
diff --git a/build/deps/github_hashes/facebook/mvfst-rev.txt b/build/deps/github_hashes/facebook/mvfst-rev.txt
index cb7acbe6a866..b0d97ca4ea3b 100644
--- a/build/deps/github_hashes/facebook/mvfst-rev.txt
+++ b/build/deps/github_hashes/facebook/mvfst-rev.txt
@@ -1 +1 @@
-Subproject commit 25047bcdaa944386a32b03d5c2367596cfcc1bf1
+Subproject commit d98bb4a80878502de793189e3ea2574c3c07da60
diff --git a/build/deps/github_hashes/facebook/wangle-rev.txt b/build/deps/github_hashes/facebook/wangle-rev.txt
index 1420831aad3e..f8b22a28297a 100644
--- a/build/deps/github_hashes/facebook/wangle-rev.txt
+++ b/build/deps/github_hashes/facebook/wangle-rev.txt
@@ -1 +1 @@
-Subproject commit cc3c7447231744f9203112535f908a6307347cb1
+Subproject commit 4c8d04b1bf00360cc5cc6a3bd7bdb70c4edc355a



```

---

### [OK] fad81677 - Summary:
**Author:** Unknown | **Date:** Sun, 6 Sep 2026 09:34:24 -0700

```diff
GitHub commits:

https://github.com/facebook/CacheLib/commit/cce3e1dd0a7e07a33fa6357dcf4f5e05f8aebc65
https://github.com/facebook/fb303/commit/9e155e57d95fc571ebafd4bbbf96c5c4abcef0a2
https://github.com/facebook/fbthrift/commit/037bfb80663ed4c82e01de02309bd0c5445dee40
https://github.com/facebook/mvfst/commit/25047bcdaa944386a32b03d5c2367596cfcc1bf1
https://github.com/facebook/proxygen/commit/8d47d361b78fafb37955bc4dd963858ddae487aa
https://github.com/facebook/pyrefly/commit/70f23e9f95d610b0e13f9e36f98c903a1838fef7
https://github.com/facebook/wangle/commit/cc3c7447231744f9203112535f908a6307347cb1
https://github.com/facebookexperimental/edencommon/commit/9035d18a70626d1deafa8d39546072bdb6de07f7
https://github.com/facebookexperimental/rust-shed/commit/a3e2ac1f7304599732bab49a6bf18ce8bc647b3b
https://github.com/facebookincubator/fizz/commit/963f906bb5f08cea100b79fe7dcd45594949cc9e

Reviewed By: sdwilsh

fbshipit-source-id: bb1d63b95f6a1558e06aae3cf0066d8ed41dae8a
---
 build/deps/github_hashes/facebook/fb303-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/fbthrift-rev.txt              | 2 +-
 build/deps/github_hashes/facebook/mvfst-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/wangle-rev.txt                | 2 +-
 .../deps/github_hashes/facebookexperimental/edencommon-rev.txt  | 2 +-
 build/deps/github_hashes/facebookincubator/fizz-rev.txt         | 2 +-
 6 files changed, 6 insertions(+), 6 deletions(-)

diff --git a/build/deps/github_hashes/facebook/fb303-rev.txt b/build/deps/github_hashes/facebook/fb303-rev.txt
index a3e2d64e1372..f86ae5580918 100644
--- a/build/deps/github_hashes/facebook/fb303-rev.txt
+++ b/build/deps/github_hashes/facebook/fb303-rev.txt
@@ -1 +1 @@
-Subproject commit 547cd2c6b9c1261729dd86c349eec0e1145ad0a8
+Subproject commit 9e155e57d95fc571ebafd4bbbf96c5c4abcef0a2
diff --git a/build/deps/github_hashes/facebook/fbthrift-rev.txt b/build/deps/github_hashes/facebook/fbthrift-rev.txt
index 9478f2565340..d5ccac5d88f8 100644
--- a/build/deps/github_hashes/facebook/fbthrift-rev.txt
+++ b/build/deps/github_hashes/facebook/fbthrift-rev.txt
@@ -1 +1 @@
-Subproject commit 6a8c26b15eed7df3d6f1870cea7e82d2285a6f66
+Subproject commit 037bfb80663ed4c82e01de02309bd0c5445dee40
diff --git a/build/deps/github_hashes/facebook/mvfst-rev.txt b/build/deps/github_hashes/facebook/mvfst-rev.txt
index 269fdb941257..cb7acbe6a866 100644
--- a/build/deps/github_hashes/facebook/mvfst-rev.txt
+++ b/build/deps/github_hashes/facebook/mvfst-rev.txt
@@ -1 +1 @@
-Subproject commit 56c305db2f8ca46b60520c4fc09adc1cccb640f3
+Subproject commit 25047bcdaa944386a32b03d5c2367596cfcc1bf1
diff --git a/build/deps/github_hashes/facebook/wangle-rev.txt b/build/deps/github_hashes/facebook/wangle-rev.txt
index 1a4f9a3f13d9..1420831aad3e 100644
--- a/build/deps/github_hashes/facebook/wangle-rev.txt
+++ b/build/deps/github_hashes/facebook/wangle-rev.txt
@@ -1 +1 @@
-Subproject commit 979a017e108171a8e23720f8f2756fe3f256ea82
+Subproject commit cc3c7447231744f9203112535f908a6307347cb1
diff --git a/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt b/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
index 940ea0b0e94a..8fa9a7f8c7a6 100644
--- a/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
+++ b/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
@@ -1 +1 @@
-Subproject commit 8b2943ab6620a0dab389190c9f8c04c2ca483fa0
+Subproject commit 9035d18a70626d1deafa8d39546072bdb6de07f7
diff --git a/build/deps/github_hashes/facebookincubator/fizz-rev.txt b/build/deps/github_hashes/facebookincubator/fizz-rev.txt
index 0f47905898fa..2dba0b26ba1d 100644
--- a/build/deps/github_hashes/facebookincubator/fizz-rev.txt
+++ b/build/deps/github_hashes/facebookincubator/fizz-rev.txt
@@ -1 +1 @@
-Subproject commit e6968f3396f50f4bb00a720d650a3b281f39895f
+Subproject commit 963f906bb5f08cea100b79fe7dcd45594949cc9e



```

---

### [OK] 2c99db19 - Summary:
**Author:** Unknown | **Date:** Sat, 5 Sep 2026 09:33:57 -0700

```diff
GitHub commits:

https://github.com/WhatsApp/erlang-language-platform/commit/7f554078cfa2443e1343dfb7f7105e3ad1f3f143
https://github.com/facebook/CacheLib/commit/b946787415ff6a248715e5d140a6164d3e4dfd99
https://github.com/facebook/fb303/commit/547cd2c6b9c1261729dd86c349eec0e1145ad0a8
https://github.com/facebook/fbthrift/commit/6a8c26b15eed7df3d6f1870cea7e82d2285a6f66
https://github.com/facebook/folly/commit/65749da412d92fe27d87ae45a8ef670720712858
https://github.com/facebook/mvfst/commit/56c305db2f8ca46b60520c4fc09adc1cccb640f3
https://github.com/facebook/proxygen/commit/5b7432a47fa89652872f998ebff0e42aae0ca8da
https://github.com/facebook/pyrefly/commit/ece5d2796dcaeb6b75f15510d325af903382ac43
https://github.com/facebook/wangle/commit/979a017e108171a8e23720f8f2756fe3f256ea82
https://github.com/facebookexperimental/edencommon/commit/8b2943ab6620a0dab389190c9f8c04c2ca483fa0
https://github.com/facebookexperimental/rust-shed/commit/fd73ec8a7c8fbe32dc3a67ed3c8543ef52c05c44
https://github.com/facebookincubator/fizz/commit/e6968f3396f50f4bb00a720d650a3b281f39895f

Reviewed By: sdwilsh

fbshipit-source-id: c3a5f4e8546392f71d5edc2c019bfbc8884f351b
---
 build/deps/github_hashes/facebook/fb303-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/fbthrift-rev.txt              | 2 +-
 build/deps/github_hashes/facebook/folly-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/mvfst-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/wangle-rev.txt                | 2 +-
 .../deps/github_hashes/facebookexperimental/edencommon-rev.txt  | 2 +-
 build/deps/github_hashes/facebookincubator/fizz-rev.txt         | 2 +-
 7 files changed, 7 insertions(+), 7 deletions(-)

diff --git a/build/deps/github_hashes/facebook/fb303-rev.txt b/build/deps/github_hashes/facebook/fb303-rev.txt
index e30b0c1e20ef..a3e2d64e1372 100644
--- a/build/deps/github_hashes/facebook/fb303-rev.txt
+++ b/build/deps/github_hashes/facebook/fb303-rev.txt
@@ -1 +1 @@
-Subproject commit 97e2cd7fda9519b54fe495b1da13f74f63c24702
+Subproject commit 547cd2c6b9c1261729dd86c349eec0e1145ad0a8
diff --git a/build/deps/github_hashes/facebook/fbthrift-rev.txt b/build/deps/github_hashes/facebook/fbthrift-rev.txt
index 35d23b63acc0..9478f2565340 100644
--- a/build/deps/github_hashes/facebook/fbthrift-rev.txt
+++ b/build/deps/github_hashes/facebook/fbthrift-rev.txt
@@ -1 +1 @@
-Subproject commit d59463bcdbe5f5143abe6eabb3c32579540420fe
+Subproject commit 6a8c26b15eed7df3d6f1870cea7e82d2285a6f66
diff --git a/build/deps/github_hashes/facebook/folly-rev.txt b/build/deps/github_hashes/facebook/folly-rev.txt
index a1b511dabf12..36f3fcdadc2d 100644
--- a/build/deps/github_hashes/facebook/folly-rev.txt
+++ b/build/deps/github_hashes/facebook/folly-rev.txt
@@ -1 +1 @@
-Subproject commit 6df3ef0f4d97f1bb7dd77b92080ff612a93ec894
+Subproject commit 65749da412d92fe27d87ae45a8ef670720712858
diff --git a/build/deps/github_hashes/facebook/mvfst-rev.txt b/build/deps/github_hashes/facebook/mvfst-rev.txt
index dad187e9e382..269fdb941257 100644
--- a/build/deps/github_hashes/facebook/mvfst-rev.txt
+++ b/build/deps/github_hashes/facebook/mvfst-rev.txt
@@ -1 +1 @@
-Subproject commit b00e9e9d5fd63175a6f741e2584b7018af788bfb
+Subproject commit 56c305db2f8ca46b60520c4fc09adc1cccb640f3
diff --git a/build/deps/github_hashes/facebook/wangle-rev.txt b/build/deps/github_hashes/facebook/wangle-rev.txt
index 9dc182b092c6..1a4f9a3f13d9 100644
--- a/build/deps/github_hashes/facebook/wangle-rev.txt
+++ b/build/deps/github_hashes/facebook/wangle-rev.txt
@@ -1 +1 @@
-Subproject commit 1e84653eacbb983e5856477f8da218504d764e35
+Subproject commit 979a017e108171a8e23720f8f2756fe3f256ea82
diff --git a/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt b/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
index b622ae058427..940ea0b0e94a 100644
--- a/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
+++ b/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
@@ -1 +1 @@
-Subproject commit 10bef2e6e9a39c7a1672f1a429d24c694b6c87ed
+Subproject commit 8b2943ab6620a0dab389190c9f8c04c2ca483fa0
diff --git a/build/deps/github_hashes/facebookincubator/fizz-rev.txt b/build/deps/github_hashes/facebookincubator/fizz-rev.txt
index 1310fa8ab444..0f47905898fa 100644
--- a/build/deps/github_hashes/facebookincubator/fizz-rev.txt
+++ b/build/deps/github_hashes/facebookincubator/fizz-rev.txt
@@ -1 +1 @@
-Subproject commit 9c811d3edda1d8de20bd302264ce6587b20a05fa
+Subproject commit e6968f3396f50f4bb00a720d650a3b281f39895f



```

---

### [OK] 333746ad - Summary:
**Author:** Unknown | **Date:** Fri, 4 Sep 2026 09:37:06 -0700

```diff
Context:
The BGP OSS manifest stopped stripping the complete BB platform directory so `PlatformConstant.h` could support the BB CMake build. With no narrower rule, any future file added to that directory would also ship to facebook/BGP.

Motivation:
The OSS export should include only the BB platform file required by `BgpBB.cmake`. A fail-closed rule prevents unrelated future files from entering the public repository automatically.

This diff:
- allows `PlatformConstant.h` under `cpp/common/platform/bb`
- strips every other current or future file in that directory
- keeps `manifests/bgp` aligned with the production `BGP.cconf` rule

Reviewed By: jaiharil

Differential Revision: D118808795

fbshipit-source-id: e965cae3294e6199cd7387d53edb8cf796835d85
---
 build/fbcode_builder/manifests/bgp | 2 ++
 1 file changed, 2 insertions(+)

diff --git a/build/fbcode_builder/manifests/bgp b/build/fbcode_builder/manifests/bgp
index bece53dad8df..8025c4b8ae47 100644
--- a/build/fbcode_builder/manifests/bgp
+++ b/build/fbcode_builder/manifests/bgp
@@ -60,6 +60,8 @@ fbcode/configerator/structs/neteng/fboss/thrift = configerator/structs/neteng/fb
 ^fbcode/neteng/fboss/bgp/.*\.bzl
 ^fbcode/neteng/fboss/bgp/\.llms/.*
 ^fbcode/neteng/fboss/bgp/tools/.*
+# Export only the BB platform constant required by the OSS build.
+^fbcode/neteng/fboss/bgp/cpp/common/platform/bb/(?!PlatformConstant\.h$).*
 ^fbcode/neteng/fboss/bgp/cpp/tests/NetlinkWrapperTest\.cpp
 ^fbcode/neteng/fboss/bgp/cpp/tests/FibEbbTest\.cpp
 # Only common.thrift is needed from configerator/structs/neteng/fboss/thrift;



```

---

### [OK] 5d365ca4 - Summary:
**Author:** Unknown | **Date:** Fri, 4 Sep 2026 09:34:41 -0700

```diff
GitHub commits:

https://github.com/WhatsApp/erlang-language-platform/commit/b4315c3dc3fdc22f870c601d3c5ef844189ab41a
https://github.com/facebook/CacheLib/commit/cd2310753f63f1d09d157dc1d471fd3c801c0057
https://github.com/facebook/fb303/commit/97e2cd7fda9519b54fe495b1da13f74f63c24702
https://github.com/facebook/fbthrift/commit/d59463bcdbe5f5143abe6eabb3c32579540420fe
https://github.com/facebook/folly/commit/6df3ef0f4d97f1bb7dd77b92080ff612a93ec894
https://github.com/facebook/mvfst/commit/b00e9e9d5fd63175a6f741e2584b7018af788bfb
https://github.com/facebook/proxygen/commit/003286db90c74ba614dc434a708d9105df56bdd8
https://github.com/facebook/pyrefly/commit/0c346ab04d5e3ee36b3f190eb48152769d4d5ca4
https://github.com/facebook/wangle/commit/1e84653eacbb983e5856477f8da218504d764e35
https://github.com/facebookexperimental/edencommon/commit/10bef2e6e9a39c7a1672f1a429d24c694b6c87ed
https://github.com/facebookexperimental/rust-shed/commit/36d969f4de678e2e77d6909fe8d84ea942ee3734
https://github.com/facebookincubator/fizz/commit/9c811d3edda1d8de20bd302264ce6587b20a05fa
https://github.com/react/yoga/commit/983008479dc44b64503071c9ee8d76c6704ff309

Reviewed By: vladi99

fbshipit-source-id: 5877823ebf0e7f55a5a96d090047a33aaea5c4d8
---
 build/deps/github_hashes/facebook/fb303-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/fbthrift-rev.txt              | 2 +-
 build/deps/github_hashes/facebook/folly-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/mvfst-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/wangle-rev.txt                | 2 +-
 .../deps/github_hashes/facebookexperimental/edencommon-rev.txt  | 2 +-
 build/deps/github_hashes/facebookincubator/fizz-rev.txt         | 2 +-
 7 files changed, 7 insertions(+), 7 deletions(-)

diff --git a/build/deps/github_hashes/facebook/fb303-rev.txt b/build/deps/github_hashes/facebook/fb303-rev.txt
index 3183986fdc48..e30b0c1e20ef 100644
--- a/build/deps/github_hashes/facebook/fb303-rev.txt
+++ b/build/deps/github_hashes/facebook/fb303-rev.txt
@@ -1 +1 @@
-Subproject commit c603a14e8bd11233dce0d9f8737997de94dc8469
+Subproject commit 97e2cd7fda9519b54fe495b1da13f74f63c24702
diff --git a/build/deps/github_hashes/facebook/fbthrift-rev.txt b/build/deps/github_hashes/facebook/fbthrift-rev.txt
index e8eeafb612df..35d23b63acc0 100644
--- a/build/deps/github_hashes/facebook/fbthrift-rev.txt
+++ b/build/deps/github_hashes/facebook/fbthrift-rev.txt
@@ -1 +1 @@
-Subproject commit ea5377cc2c7831aa5406a678a560722197dde581
+Subproject commit d59463bcdbe5f5143abe6eabb3c32579540420fe
diff --git a/build/deps/github_hashes/facebook/folly-rev.txt b/build/deps/github_hashes/facebook/folly-rev.txt
index 384ad81c2892..a1b511dabf12 100644
--- a/build/deps/github_hashes/facebook/folly-rev.txt
+++ b/build/deps/github_hashes/facebook/folly-rev.txt
@@ -1 +1 @@
-Subproject commit a47cf2cf47ddacd46fdd0bcfb1a62bfcb39d63f1
+Subproject commit 6df3ef0f4d97f1bb7dd77b92080ff612a93ec894
diff --git a/build/deps/github_hashes/facebook/mvfst-rev.txt b/build/deps/github_hashes/facebook/mvfst-rev.txt
index 61838113633c..dad187e9e382 100644
--- a/build/deps/github_hashes/facebook/mvfst-rev.txt
+++ b/build/deps/github_hashes/facebook/mvfst-rev.txt
@@ -1 +1 @@
-Subproject commit a455d378fe390b9a268468efe1c81479c56eb383
+Subproject commit b00e9e9d5fd63175a6f741e2584b7018af788bfb
diff --git a/build/deps/github_hashes/facebook/wangle-rev.txt b/build/deps/github_hashes/facebook/wangle-rev.txt
index cf7a00ad881f..9dc182b092c6 100644
--- a/build/deps/github_hashes/facebook/wangle-rev.txt
+++ b/build/deps/github_hashes/facebook/wangle-rev.txt
@@ -1 +1 @@
-Subproject commit 4317e256aad528b79ffa8618f504d18133f845a6
+Subproject commit 1e84653eacbb983e5856477f8da218504d764e35
diff --git a/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt b/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
index a9576b51e5c7..b622ae058427 100644
--- a/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
+++ b/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
@@ -1 +1 @@
-Subproject commit 4e47fbdc682dbab7372658c1d0c4ea71cb1ecdd0
+Subproject commit 10bef2e6e9a39c7a1672f1a429d24c694b6c87ed
diff --git a/build/deps/github_hashes/facebookincubator/fizz-rev.txt b/build/deps/github_hashes/facebookincubator/fizz-rev.txt
index bbb01175e323..1310fa8ab444 100644
--- a/build/deps/github_hashes/facebookincubator/fizz-rev.txt
+++ b/build/deps/github_hashes/facebookincubator/fizz-rev.txt
@@ -1 +1 @@
-Subproject commit fea55db87d3dc937343be8bfb9a64cf93922d7aa
+Subproject commit 9c811d3edda1d8de20bd302264ce6587b20a05fa



```

---

### [OK] e405e968 - Summary:
**Author:** Unknown | **Date:** Thu, 3 Sep 2026 20:42:01 -0700

```diff
D117475730 added the `gcc12` getdeps dependency to support the CentOS Stream 9 Open/R build. Although its compiler overrides were correctly limited to CentOS Stream 9, the dependency itself was unconditional. Ubuntu 24.04 GitHub Actions therefore tried to resolve a `gcc12` manifest with no matching Ubuntu package and failed during `query-paths`, before compilation.

Scope `gcc12` to CentOS Stream 9 alongside the existing compiler definitions and regenerate the OpenR Linux workflow fixture. This preserves the intended CentOS GCC 12 toolchain from D117475730 while removing the invalid Ubuntu dependency.

Differential Revision: D118736179

fbshipit-source-id: af500b1e3f256223ff4c252c3b5839d4758499e1
---
 .../getdeps/test/fixtures/expected/openr/getdeps_linux.yml     | 3 ---
 build/fbcode_builder/manifests/openr                           | 2 ++
 2 files changed, 2 insertions(+), 3 deletions(-)

diff --git a/build/fbcode_builder/getdeps/test/fixtures/expected/openr/getdeps_linux.yml b/build/fbcode_builder/getdeps/test/fixtures/expected/openr/getdeps_linux.yml
index 95bec1fd904f..e7f957838828 100644
--- a/build/fbcode_builder/getdeps/test/fixtures/expected/openr/getdeps_linux.yml
+++ b/build/fbcode_builder/getdeps/test/fixtures/expected/openr/getdeps_linux.yml
@@ -41,9 +41,6 @@ jobs:
     - name: Fetch boost
       if: ${{ steps.paths.outputs.boost_SOURCE }}
       run: python3 build/fbcode_builder/getdeps.py fetch --no-tests boost
-    - name: Fetch gcc12
-      if: ${{ steps.paths.outputs.gcc12_SOURCE }}
-      run: python3 build/fbcode_builder/getdeps.py fetch --no-tests gcc12
     - name: Fetch ninja
       if: ${{ steps.paths.outputs.ninja_SOURCE }}
       run: python3 build/fbcode_builder/getdeps.py fetch --no-tests ninja
diff --git a/build/fbcode_builder/manifests/openr b/build/fbcode_builder/manifests/openr
index de1a1f2da2e4..53cfbfca62d4 100644
--- a/build/fbcode_builder/manifests/openr
+++ b/build/fbcode_builder/manifests/openr
@@ -59,6 +59,8 @@ folly
 googletest
 re2
 range-v3
+
+[dependencies.all(distro=centos_stream,distro_vers=9)]
 gcc12
 
 [cmake.defines.all(distro=centos_stream,distro_vers=9)]



```

---

### [OK] 864e6d6a - Summary: Centralize recognition of EdenFS NFS mounts whose filesystem type remains `nfs` while their mount source is `edenfs:`. Keep Watchman using that fallback and add focused coverage for Eden, ordinary NFS, and non-NFS mounts.
**Author:** Unknown | **Date:** Thu, 3 Sep 2026 17:40:09 -0700

```diff
Reviewed By: janezhang10

Differential Revision: D117887012

fbshipit-source-id: b85b97466cef684ebaaad06ca0b3b2954f7ecc56
---
 watchman/fs/FSDetect.cpp       |  4 ++--
 watchman/fs/FSDetect.h         |  1 +
 watchman/test/FSDetectTest.cpp | 19 +++++++++++++++++++
 3 files changed, 22 insertions(+), 2 deletions(-)

diff --git a/watchman/fs/FSDetect.cpp b/watchman/fs/FSDetect.cpp
index 03bf88932ed1..d6e5e6632b2f 100644
--- a/watchman/fs/FSDetect.cpp
+++ b/watchman/fs/FSDetect.cpp
@@ -109,8 +109,8 @@ std::optional<w_string> find_fstype_in_linux_proc_mounts(
 }
 
 w_string w_fstype_detect_macos_nfs(w_string fstype, w_string edenfs_indicator) {
-  if (fstype == "nfs" &&
-      facebook::eden::is_edenfs_fs_type(edenfs_indicator.string())) {
+  if (facebook::eden::is_edenfs_nfs_mount(
+          fstype.view(), edenfs_indicator.view())) {
     return edenfs_indicator;
   }
   return fstype;
diff --git a/watchman/fs/FSDetect.h b/watchman/fs/FSDetect.h
index d7f2759c63a9..02fc95b30bbd 100644
--- a/watchman/fs/FSDetect.h
+++ b/watchman/fs/FSDetect.h
@@ -25,3 +25,4 @@ w_string w_fstype(const char* path);
 std::optional<w_string> find_fstype_in_linux_proc_mounts(
     std::string_view path,
     std::string_view procMountsData);
+w_string w_fstype_detect_macos_nfs(w_string fstype, w_string edenfs_indicator);
diff --git a/watchman/test/FSDetectTest.cpp b/watchman/test/FSDetectTest.cpp
index fa60bf9c17d7..ed775f8ac0a1 100644
--- a/watchman/test/FSDetectTest.cpp
+++ b/watchman/test/FSDetectTest.cpp
@@ -132,3 +132,22 @@ TEST(FSType, fstype_two_entries) {
       find_fstype_in_linux_proc_mounts(
           "/data/users/wez/fbsourcenoslash", mount_data_btrfs));
 }
+
+TEST(FSType, macosNfsEdenMountUsesMountSource) {
+  EXPECT_EQ(
+      w_string("edenfs:"),
+      w_fstype_detect_macos_nfs(w_string("nfs"), w_string("edenfs:")));
+}
+
+TEST(FSType, macosNfsNonEdenMountRemainsNfs) {
+  EXPECT_EQ(
+      w_string("nfs"),
+      w_fstype_detect_macos_nfs(
+          w_string("nfs"), w_string("server:/repository")));
+}
+
+TEST(FSType, macosNonNfsMountIgnoresEdenMountSource) {
+  EXPECT_EQ(
+      w_string("apfs"),
+      w_fstype_detect_macos_nfs(w_string("apfs"), w_string("edenfs:")));
+}



```

---

### [OK] 12043e77 - Summary:
**Author:** Unknown | **Date:** Thu, 3 Sep 2026 12:35:34 -0700

```diff
`oss-glean-linux-getdeps` and `oss-hsthrift-linux-getdeps` still fail
100% of the time, now at `make cabal-update`:

```
Writing default configuration to /var/twsvcscm/.cabal/config
<repo>/root.json does not have enough signatures signed with the appropriate keys
make: *** [Makefile:114: cabal-update] Error 1
```

Hackage rotated its TUF root keys. `manifests/cabal` pins
`cabal-install-3.6.2.0` (December 2021), whose bootstrap key set predates
the rotation, so it rejects the current `root.json` and cannot run
`cabal update` at all.

This is the same class of problem as the `index-state` divergence: the
internal getdeps build runs a different toolchain from GitHub CI, which
does `ghcup install cabal 3.10` (`.github/workflows/ci.yml`). Bump the
manifest to 3.10.3.0, which is what `3.10` currently resolves to
(3.10.1.0, 3.10.2.0 and 3.10.3.0 are the published 3.10.x releases).

Staying on the `x86_64-linux-deb10` variant, as before.

## Blast radius

`manifests/cabal` is a shared manifest, but in practice it has exactly two
consumers. Searching every file under `manifests/` for `cabal` as a
dependency entry (`^\s*cabal\s*$`) matches only:

* `manifests/glean`
* `manifests/hsthrift`

which are the two projects this is meant to fix. Both also pull `ghc`.
Worth being aware that getdeps has no per-consumer version override, so a
future third consumer needing a different cabal would be a genuine
conflict — there is no such consumer today.

Every other Haskell build path at Meta gets cabal from ghcup rather than
from this manifest, so none of them change:

| path | cabal source |
| --- | --- |
| Glean GitHub CI x86 (`ci.yml`) | `ghcup install cabal 3.10` |
| Glean GitHub CI aarch64 (`ci-aarch64.yml`) | `ghcup install cabal --set` |
| hsthrift GitHub CI (`ci-getdeps.yml`) | `ghcup install cabal --set` |
| `spidermate/repomate` Haskell docker builder | `ghcup install cabal 3.10.3.0` |

The last row is independent corroboration of the version chosen here — an
unrelated Meta system already pins exactly 3.10.3.0.

Not fixed here, but flagged: `glean/github/tld/Dockerfile` installs
`ghc-8.10.2` from apt on an old base image and then runs a bare
`cabal update`, with no ghcup. That is very likely broken by the same key
rotation. This is inferred from the toolchain age, not from a failing run.

## LFS

Also adds the `.lfs-pointers` entry for the new archive, which the
"Warn about missing LFS updates to getdeps manifest diffs" CI check asks
for on any getdeps manifest bump:

```
1d7a7131402295b01f25be5373fde095a404c45f9b5a5508fb7474bb0d3d057a 4920976 cabal-cabal-install-3.10.3.0-x86_64-linux-deb10.tar.xz
```

The name follows `ArchiveFetcher`'s convention of `<manifest>-<url
basename>` (`getdeps/fetcher.py:1062`), matching the existing 3.6.2.0 row,
which is left in place — the file already keeps superseded entries (for
example both cmake 3.20.2 and 3.20.4).

Without this the bump would work but would never use the cache:
`LFSCachingArchiveFetcher` calls `lfs_upload` after a public-URL fallback,
but `lfs.py upload` records the pointer in the *worker's* checkout of
`fbcode/tools/lfs/.lfs-pointers`, which is discarded at teardown. Every
build would re-fetch the archive from `downloads.haskell.org`, leaving CI
coupled to that host staying up and staying in the `lego` fwdproxy
allowlist.

Reviewed By: jjuliamolin

Differential Revision: D118644355

fbshipit-source-id: 2dae8a3fb050253d3fda97ce94aa147cc282455e
---
 build/fbcode_builder/manifests/cabal | 4 ++--
 1 file changed, 2 insertions(+), 2 deletions(-)

diff --git a/build/fbcode_builder/manifests/cabal b/build/fbcode_builder/manifests/cabal
index 1405b8bc8f06..eb8c8c148b00 100644
--- a/build/fbcode_builder/manifests/cabal
+++ b/build/fbcode_builder/manifests/cabal
@@ -2,8 +2,8 @@
 name = cabal
 
 [download.os=linux]
-url = https://downloads.haskell.org/~cabal/cabal-install-3.6.2.0/cabal-install-3.6.2.0-x86_64-linux-deb10.tar.xz
-sha256 = 4759b56e9257e02f29fa374a6b25d6cb2f9d80c7e3a55d4f678a8e570925641c
+url = https://downloads.haskell.org/~cabal/cabal-install-3.10.3.0/cabal-install-3.10.3.0-x86_64-linux-deb10.tar.xz
+sha256 = 1d7a7131402295b01f25be5373fde095a404c45f9b5a5508fb7474bb0d3d057a
 
 [build]
 builder = nop



```

---

### [OK] ba942266 - Summary: Add the socket-lb getdeps manifest, generated Linux GitHub Actions workflow, and automatic Sandcastle ownership. Map the workflow to repository-root `.github`, strip internal `BUCK` and `PACKAGE` metadata, and cover socket-lb dependency determination in getdeps tests. This diff is ordered before the socket-lb CMake change so that change automatically triggers the new job.
**Author:** Unknown | **Date:** Thu, 3 Sep 2026 10:59:10 -0700

```diff
Reviewed By: avasylev

Differential Revision: D117744481

fbshipit-source-id: 64cc871fd8c9a6f2222058b6ae00b5a49da8b0c2
---
 build/fbcode_builder/manifests/socket-lb | 51 ++++++++++++++++++++++++
 1 file changed, 51 insertions(+)
 create mode 100644 build/fbcode_builder/manifests/socket-lb

diff --git a/build/fbcode_builder/manifests/socket-lb b/build/fbcode_builder/manifests/socket-lb
new file mode 100644
index 000000000000..cf52117462db
--- /dev/null
+++ b/build/fbcode_builder/manifests/socket-lb
@@ -0,0 +1,51 @@
+[manifest]
+name = socket-lb
+fbsource_path = fbcode/socket-lb
+shipit_project = facebookincubator/socket-lb
+shipit_fbcode_builder = true
+
+[git]
+repo_url = https://github.com/facebookincubator/socket-lb.git
+
+[build.not(os=linux)]
+builder = nop
+
+[build.os=linux]
+builder = cmake
+subdir = public_root
+
+[cmake.defines.test=on]
+BUILD_TESTING = ON
+
+[cmake.defines.test=off]
+BUILD_TESTING = OFF
+
+[dependencies]
+folly
+gflags
+glog
+katran
+libbpf
+libelf
+numa
+zlib
+
+[dependencies.test=on]
+googletest
+
+[debs]
+bpftool
+clang
+
+[rpms]
+bpftool
+clang
+llvm
+
+[shipit.pathmap]
+fbcode/socket-lb/public_root/.github = .github
+fbcode/socket-lb = .
+
+[shipit.strip]
+^fbcode/socket-lb/(.*/)?BUCK$
+^fbcode/socket-lb/PACKAGE$



```

---

### [OK] ba0d833f - Summary:
**Author:** Unknown | **Date:** Thu, 3 Sep 2026 09:32:57 -0700

```diff
GitHub commits:

https://github.com/WhatsApp/erlang-language-platform/commit/d5e1cfc4a36de764488292c74faeebcb34d1797c
https://github.com/facebook/CacheLib/commit/42d4692d9934c253ebd8e101e7fefcd69721a1b3
https://github.com/facebook/fb303/commit/c603a14e8bd11233dce0d9f8737997de94dc8469
https://github.com/facebook/fbthrift/commit/ea5377cc2c7831aa5406a678a560722197dde581
https://github.com/facebook/folly/commit/a47cf2cf47ddacd46fdd0bcfb1a62bfcb39d63f1
https://github.com/facebook/mvfst/commit/a455d378fe390b9a268468efe1c81479c56eb383
https://github.com/facebook/proxygen/commit/6d8667bab2b061788c3b885072f717ddb10a117e
https://github.com/facebook/pyrefly/commit/bf977e9555af60b27c5c9f9e05bd4d7baad9603f
https://github.com/facebook/wangle/commit/4317e256aad528b79ffa8618f504d18133f845a6
https://github.com/facebookexperimental/edencommon/commit/4e47fbdc682dbab7372658c1d0c4ea71cb1ecdd0
https://github.com/facebookexperimental/rust-shed/commit/6ec06cb57d9a118fad3c0e177862a889498675c9
https://github.com/facebookincubator/fizz/commit/fea55db87d3dc937343be8bfb9a64cf93922d7aa

Reviewed By: vladi99

fbshipit-source-id: 45eb84ddc3e6039f5a5ab4c53acf647080db0cdf
---
 build/deps/github_hashes/facebook/fb303-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/fbthrift-rev.txt              | 2 +-
 build/deps/github_hashes/facebook/folly-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/mvfst-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/wangle-rev.txt                | 2 +-
 .../deps/github_hashes/facebookexperimental/edencommon-rev.txt  | 2 +-
 build/deps/github_hashes/facebookincubator/fizz-rev.txt         | 2 +-
 7 files changed, 7 insertions(+), 7 deletions(-)

diff --git a/build/deps/github_hashes/facebook/fb303-rev.txt b/build/deps/github_hashes/facebook/fb303-rev.txt
index 8f157dabdf9c..3183986fdc48 100644
--- a/build/deps/github_hashes/facebook/fb303-rev.txt
+++ b/build/deps/github_hashes/facebook/fb303-rev.txt
@@ -1 +1 @@
-Subproject commit 98e7cc6639a14a461b6884142636ef99af6a2d80
+Subproject commit c603a14e8bd11233dce0d9f8737997de94dc8469
diff --git a/build/deps/github_hashes/facebook/fbthrift-rev.txt b/build/deps/github_hashes/facebook/fbthrift-rev.txt
index 065df9d7c881..e8eeafb612df 100644
--- a/build/deps/github_hashes/facebook/fbthrift-rev.txt
+++ b/build/deps/github_hashes/facebook/fbthrift-rev.txt
@@ -1 +1 @@
-Subproject commit 89d97cde082e7afadf1cf83003be498f98d5f8d7
+Subproject commit ea5377cc2c7831aa5406a678a560722197dde581
diff --git a/build/deps/github_hashes/facebook/folly-rev.txt b/build/deps/github_hashes/facebook/folly-rev.txt
index 4422863103e7..384ad81c2892 100644
--- a/build/deps/github_hashes/facebook/folly-rev.txt
+++ b/build/deps/github_hashes/facebook/folly-rev.txt
@@ -1 +1 @@
-Subproject commit c8bde19cb06f1f7bcaf2e9fe89198182700dab78
+Subproject commit a47cf2cf47ddacd46fdd0bcfb1a62bfcb39d63f1
diff --git a/build/deps/github_hashes/facebook/mvfst-rev.txt b/build/deps/github_hashes/facebook/mvfst-rev.txt
index fc6121ed0d2c..61838113633c 100644
--- a/build/deps/github_hashes/facebook/mvfst-rev.txt
+++ b/build/deps/github_hashes/facebook/mvfst-rev.txt
@@ -1 +1 @@
-Subproject commit 2d371787601d6332d04668c6e77d01bcad5c1105
+Subproject commit a455d378fe390b9a268468efe1c81479c56eb383
diff --git a/build/deps/github_hashes/facebook/wangle-rev.txt b/build/deps/github_hashes/facebook/wangle-rev.txt
index 0edf81654b92..cf7a00ad881f 100644
--- a/build/deps/github_hashes/facebook/wangle-rev.txt
+++ b/build/deps/github_hashes/facebook/wangle-rev.txt
@@ -1 +1 @@
-Subproject commit e368f8b896ba46c4ad29f5990e1dfcc004ff0d96
+Subproject commit 4317e256aad528b79ffa8618f504d18133f845a6
diff --git a/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt b/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
index 0f1e5188128c..a9576b51e5c7 100644
--- a/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
+++ b/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
@@ -1 +1 @@
-Subproject commit 6eb5825a1f6f7932c499a05d51277aabc5f55098
+Subproject commit 4e47fbdc682dbab7372658c1d0c4ea71cb1ecdd0
diff --git a/build/deps/github_hashes/facebookincubator/fizz-rev.txt b/build/deps/github_hashes/facebookincubator/fizz-rev.txt
index aea4adf680ac..bbb01175e323 100644
--- a/build/deps/github_hashes/facebookincubator/fizz-rev.txt
+++ b/build/deps/github_hashes/facebookincubator/fizz-rev.txt
@@ -1 +1 @@
-Subproject commit fe3d088dd93895110a2c57b94d21507a278b8b3d
+Subproject commit fea55db87d3dc937343be8bfb9a64cf93922d7aa



```

---

### [OK] 77e397da - Summary:
**Author:** Unknown | **Date:** Wed, 2 Sep 2026 15:42:08 -0700

```diff
Context:
The OSS CMake build now has independent `dc` and `bb` variants. Each variant uses a different FBOSS dependency profile, but neither path had end-to-end OSS tests or continuous build coverage.

Motivation:
CI should verify each variant against only its intended dependency surface so a DC dependency cannot accidentally leak into BB and a passing build of one daemon cannot hide a failure in the other.

This diff:
- adds GoogleTest and `BUILD_TESTING` manifest support
- registers BB-only CTests from `BgpBB.cmake` for platform constants, statistics, and the `bgp_bb --version` smoke test
- keeps the OSS source-boundary test in the common CMake graph so both variants run it
- validates required public BB files, license headers, and forbidden internal or copied-source paths across every CMake fragment
- extends the internal OSS build rule to pass `BGP_BUILD_VARIANT=dc|bb`, build named targets, and run CTest
- adds a DC job using the FBOSS `fsdb_client` profile and a BB job using `exported_libraries`
- converts the GitHub workflow to the same DC and BB matrix

The two jobs use separate build directories and dependency profiles; each configures only its selected daemon.

Reviewed By: xiangxu1121

Differential Revision: D117102134

fbshipit-source-id: 8a1374613c55f9f8fe71d3f382361f27ec6f6c9a
---
 build/fbcode_builder/manifests/bgp | 8 +++++++-
 1 file changed, 7 insertions(+), 1 deletion(-)

diff --git a/build/fbcode_builder/manifests/bgp b/build/fbcode_builder/manifests/bgp
index 51b55795d216..bece53dad8df 100644
--- a/build/fbcode_builder/manifests/bgp
+++ b/build/fbcode_builder/manifests/bgp
@@ -24,6 +24,7 @@ fbthrift
 fizz
 fmt
 folly
+googletest
 magic_enum
 openr = !default, !full, exported_libraries
 wangle
@@ -31,6 +32,12 @@ re2
 zstd
 gcc12
 
+[cmake.defines.test=on]
+BUILD_TESTING=ON
+
+[cmake.defines.test=off]
+BUILD_TESTING=OFF
+
 [shipit.pathmap]
 fbcode/neteng/fboss/bgp/public_tld = .
 fbcode/neteng/fboss/bgp = neteng/fboss/bgp
@@ -54,7 +61,6 @@ fbcode/configerator/structs/neteng/fboss/thrift = configerator/structs/neteng/fb
 ^fbcode/neteng/fboss/bgp/\.llms/.*
 ^fbcode/neteng/fboss/bgp/tools/.*
 ^fbcode/neteng/fboss/bgp/cpp/tests/NetlinkWrapperTest\.cpp
-^fbcode/neteng/fboss/bgp/cpp/tests/PlatformConstantBbTest\.cpp
 ^fbcode/neteng/fboss/bgp/cpp/tests/FibEbbTest\.cpp
 # Only common.thrift is needed from configerator/structs/neteng/fboss/thrift;
 # strip the unrelated schemas the pathmap above would otherwise ship.



```

---

### [OK] d077e988 - Summary: Route EdenFS structured errors exclusively through XplatLogger. Remove the legacy Scribe construction, fallback path, metric, and obsolete test helper. Keep the master enable-error-logging gate and stack-trace upload behavior, and retain the old config keys as deprecated compatibility shims during rollout.
**Author:** Unknown | **Date:** Wed, 2 Sep 2026 14:58:51 -0700

```diff
Reviewed By: genevievehelsel

Differential Revision: D118187138

fbshipit-source-id: 0b1b1ad5a8207435a62a6f384063356921bd0bce
---
 eden/fs/service/eden.thrift | 4 ++--
 1 file changed, 2 insertions(+), 2 deletions(-)

diff --git a/eden/fs/service/eden.thrift b/eden/fs/service/eden.thrift
index 4602ef0a1600..b180e34a4ef9 100644
--- a/eden/fs/service/eden.thrift
+++ b/eden/fs/service/eden.thrift
@@ -3409,8 +3409,8 @@ service EdenService extends fb303_core.BaseService {
   /**
    * Debug endpoint to test the structured error logging pipeline end-to-end.
    * Throws a test exception, catches it, and logs it via ErrorLogger to
-   * perfpipe_edenfs_errors. Returns true if the event was logged, false if
-   * error logging is not configured or disabled.
+   * edenfs_errors through XplatLogger. Returns true if the event was logged,
+   * false if error logging is not configured or disabled.
    * Use: eden debug thrift debugLogError
    */
   bool debugLogError() throws (1: EdenError ex);



```

---

### [OK] 4d9ae164 - Summary:
**Author:** Unknown | **Date:** Wed, 2 Sep 2026 09:34:04 -0700

```diff
GitHub commits:

https://github.com/WhatsApp/erlang-language-platform/commit/c0d5539524cceb6545d27837e72ed96bd191bd75
https://github.com/facebook/CacheLib/commit/6add27c3236dbeab7006822db7be4246670192c7
https://github.com/facebook/fb303/commit/98e7cc6639a14a461b6884142636ef99af6a2d80
https://github.com/facebook/fbthrift/commit/89d97cde082e7afadf1cf83003be498f98d5f8d7
https://github.com/facebook/folly/commit/c8bde19cb06f1f7bcaf2e9fe89198182700dab78
https://github.com/facebook/hermes/commit/1c917d82200b681ee62c73daf4110c8af03e8413
https://github.com/facebook/mvfst/commit/2d371787601d6332d04668c6e77d01bcad5c1105
https://github.com/facebook/proxygen/commit/1b1d368cc1c6e761777b3af62584762e32a89de1
https://github.com/facebook/pyrefly/commit/fb1ab61f5c19521f3d5ef37d438e0ea7c8870bef
https://github.com/facebook/wangle/commit/e368f8b896ba46c4ad29f5990e1dfcc004ff0d96
https://github.com/facebookexperimental/edencommon/commit/6eb5825a1f6f7932c499a05d51277aabc5f55098
https://github.com/facebookexperimental/rust-shed/commit/f2be84e8cde60b1934ca295cc7472158ec6764c2
https://github.com/facebookincubator/fizz/commit/fe3d088dd93895110a2c57b94d21507a278b8b3d
https://github.com/react/yoga/commit/48182a319d98fda6f5cdc23f15348c53580f5e90

Reviewed By: vladi99

fbshipit-source-id: 880116d7970260230a8d58e424cff5fd712bc143
---
 build/deps/github_hashes/facebook/fb303-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/fbthrift-rev.txt              | 2 +-
 build/deps/github_hashes/facebook/folly-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/mvfst-rev.txt                 | 2 +-
 build/deps/github_hashes/facebook/wangle-rev.txt                | 2 +-
 .../deps/github_hashes/facebookexperimental/edencommon-rev.txt  | 2 +-
 build/deps/github_hashes/facebookincubator/fizz-rev.txt         | 2 +-
 7 files changed, 7 insertions(+), 7 deletions(-)

diff --git a/build/deps/github_hashes/facebook/fb303-rev.txt b/build/deps/github_hashes/facebook/fb303-rev.txt
index e8dac5187d58..8f157dabdf9c 100644
--- a/build/deps/github_hashes/facebook/fb303-rev.txt
+++ b/build/deps/github_hashes/facebook/fb303-rev.txt
@@ -1 +1 @@
-Subproject commit d2314d421ce1529ecfed0d8d409c534a37aa2dd7
+Subproject commit 98e7cc6639a14a461b6884142636ef99af6a2d80
diff --git a/build/deps/github_hashes/facebook/fbthrift-rev.txt b/build/deps/github_hashes/facebook/fbthrift-rev.txt
index 73181952f6cb..065df9d7c881 100644
--- a/build/deps/github_hashes/facebook/fbthrift-rev.txt
+++ b/build/deps/github_hashes/facebook/fbthrift-rev.txt
@@ -1 +1 @@
-Subproject commit f3a30a4e0cb3ab78862a3b228f50cfbd376ee67b
+Subproject commit 89d97cde082e7afadf1cf83003be498f98d5f8d7
diff --git a/build/deps/github_hashes/facebook/folly-rev.txt b/build/deps/github_hashes/facebook/folly-rev.txt
index 29c0530423b6..4422863103e7 100644
--- a/build/deps/github_hashes/facebook/folly-rev.txt
+++ b/build/deps/github_hashes/facebook/folly-rev.txt
@@ -1 +1 @@
-Subproject commit 176c088840724715b2c3c6211b2b347611676002
+Subproject commit c8bde19cb06f1f7bcaf2e9fe89198182700dab78
diff --git a/build/deps/github_hashes/facebook/mvfst-rev.txt b/build/deps/github_hashes/facebook/mvfst-rev.txt
index 86437b1b08a8..fc6121ed0d2c 100644
--- a/build/deps/github_hashes/facebook/mvfst-rev.txt
+++ b/build/deps/github_hashes/facebook/mvfst-rev.txt
@@ -1 +1 @@
-Subproject commit 71ab8f41863f7329899090dca022410277d76840
+Subproject commit 2d371787601d6332d04668c6e77d01bcad5c1105
diff --git a/build/deps/github_hashes/facebook/wangle-rev.txt b/build/deps/github_hashes/facebook/wangle-rev.txt
index 620d85b6b1d3..0edf81654b92 100644
--- a/build/deps/github_hashes/facebook/wangle-rev.txt
+++ b/build/deps/github_hashes/facebook/wangle-rev.txt
@@ -1 +1 @@
-Subproject commit 4c89994dd1a2f64061592a8df3eebb5e077f7e69
+Subproject commit e368f8b896ba46c4ad29f5990e1dfcc004ff0d96
diff --git a/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt b/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
index 93e30edf269a..0f1e5188128c 100644
--- a/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
+++ b/build/deps/github_hashes/facebookexperimental/edencommon-rev.txt
@@ -1 +1 @@
-Subproject commit 997f7ca0b223ce5b346fad395e328c25b9621d01
+Subproject commit 6eb5825a1f6f7932c499a05d51277aabc5f55098
diff --git a/build/deps/github_hashes/facebookincubator/fizz-rev.txt b/build/deps/github_hashes/facebookincubator/fizz-rev.txt
index 4feb0f3441a8..aea4adf680ac 100644
--- a/build/deps/github_hashes/facebookincubator/fizz-rev.txt
+++ b/build/deps/github_hashes/facebookincubator/fizz-rev.txt
@@ -1 +1 @@
-Subproject commit aef16914472afbbb4b9e86c9495da2ac9b463654
+Subproject commit fe3d088dd93895110a2c57b94d21507a278b8b3d



```

---

### [OK] b5958b55 - Make BB runtime libraries OSS-buildable
**Author:** Indu Suresh <indusuresh@users.noreply.github.com> | **Date:** 2026-09-02T01:13:14Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Make BB runtime libraries OSS-buildable]


```

---

### [OK] 7582bba7 - Consume Open/R exported libraries
**Author:** Indu Suresh <indusuresh@users.noreply.github.com> | **Date:** 2026-09-02T00:21:08Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Consume Open/R exported libraries]


```

---

### [OK] 66549f34 - Default the binary change monitor to on
**Author:** Xiaowei Lu <xiaoweilu@users.noreply.github.com> | **Date:** 2026-09-01T22:57:25Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Default the binary change monitor to on]


```

---

### [OK] 6fbce79a - Add the exported-libraries build profile
**Author:** Indu Suresh <indusuresh@users.noreply.github.com> | **Date:** 2026-09-01T21:46:30Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Add the exported-libraries build profile]


```

---

### [OK] 923b0935 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-09-01T16:32:36Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 9236c55b - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-08-31T16:33:05Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] d4983d68 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-08-30T16:33:09Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 502c8ada - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-08-29T16:33:30Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] c09c5071 - Use shared address utilities
**Author:** Indu Suresh <indusuresh@users.noreply.github.com> | **Date:** 2026-08-29T01:48:30Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Use shared address utilities]


```

---

### [OK] 90026988 - Add the exported-libraries FBOSS build profile
**Author:** Indu Suresh <indusuresh@users.noreply.github.com> | **Date:** 2026-08-29T01:48:30Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Add the exported-libraries FBOSS build profile]


```

---

### [OK] 8a8960da - Point source download at releases.pagure.org
**Author:** Alan Frindell <alanfrindell@users.noreply.github.com> | **Date:** 2026-08-28T20:13:18Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Point source download at releases.pagure.org]


```

---

### [OK] 8f5541aa - Start the binary change monitor
**Author:** Xiaowei Lu <xiaoweilu@users.noreply.github.com> | **Date:** 2026-08-28T18:29:51Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Start the binary change monitor]


```

---

### [OK] 3d023cdd - Add a binary change monitor thread
**Author:** Xiaowei Lu <xiaoweilu@users.noreply.github.com> | **Date:** 2026-08-28T18:29:51Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Add a binary change monitor thread]


```

---

### [OK] c468dc5f - Add file identity helpers
**Author:** Xiaowei Lu <xiaoweilu@users.noreply.github.com> | **Date:** 2026-08-28T18:29:51Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Add file identity helpers]


```

---

### [OK] 88d7d128 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-08-28T16:33:29Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] d72269a1 - Require GCC 12 for OSS builds
**Author:** Xiang Xu <xiangxu@users.noreply.github.com> | **Date:** 2026-08-27T22:30:26Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Require GCC 12 for OSS builds]


```

---

### [OK] 29122d8b - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-08-27T16:32:34Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] b498fb68 - Fix OSS HHVM build x64 and Arm64
**Author:** Ben Niu <benniu@users.noreply.github.com> | **Date:** 2026-08-27T16:10:41Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Fix OSS HHVM build x64 and Arm64]


```

---

### [WRONG] c9224fa9 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-08-26T16:33:06Z
**Note:** Immediately followed by fix commit b498fb68 ("Fix OSS HHVM build x64 and Arm64") touching overlapping files (watchman.ts)

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 96e5dad7 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-08-25T16:33:21Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 3a053361 - eden2: move fbcode/eden/fs2 to fbcode/eden/fs/facebook/experimental
**Author:** George Giorgidze <georgegiorgidze@users.noreply.github.com> | **Date:** 2026-08-24T22:39:57Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[eden2: move fbcode/eden/fs2 to fbcode/eden/fs/facebook/experimental]


```

---

### [OK] 8b08cf50 - Build BGP++ against the FBOSS FSDB-client closure, not all of FBOSS
**Author:** Elangovan Natarajan <elangovannatarajan@users.noreply.github.com> | **Date:** 2026-08-24T19:36:53Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Build BGP against the FBOSS FSDB-client closure not all of FBOSS]


```

---

### [OK] 019482e3 - Update nix from 0.30.1 to 0.31.3
**Author:** David Tolnay <davidtolnay@users.noreply.github.com> | **Date:** 2026-08-24T19:28:25Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Update nix from 0.30.1 to 0.31.3]


```

---

### [OK] d4bdbcce - Mark genuine interpreter owners as embedders
**Author:** Christy Lee-Eusman <christyleeeusman@users.noreply.github.com> | **Date:** 2026-08-24T17:24:42Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Mark genuine interpreter owners as embedders]


```

---

### [OK] 543f5c6d - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-08-23T16:33:53Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 91f6253b - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-08-22T16:33:52Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] eb957789 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-08-21T16:34:56Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 07955f7b - inodes: preserve access time while unloaded
**Author:** Muir Manders <muirmanders@users.noreply.github.com> | **Date:** 2026-08-21T16:07:49Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[inodes: preserve access time while unloaded]


```

---

### [OK] 8546a95a - Update sysinfo from 0.38.4 to 0.39.6
**Author:** David Tolnay <davidtolnay@users.noreply.github.com> | **Date:** 2026-08-21T06:39:33Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Update sysinfo from 0.38.4 to 0.39.6]


```

---

### [OK] 5a2736be - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-08-20T16:34:39Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] b6b030e1 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-08-19T16:32:52Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] c81774b0 - Remove unused type error suppressions - opensource
**Author:** generatedunixname89002005307016 <generatedunixname89002005307016@users.noreply.github.com> | **Date:** 2026-08-19T02:05:17Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Remove unused type error suppressions - opensource]


```

---

### [OK] d09170b2 - Remove unused type error suppressions - watchman
**Author:** generatedunixname89002005307016 <generatedunixname89002005307016@users.noreply.github.com> | **Date:** 2026-08-19T01:39:51Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Remove unused type error suppressions - watchman]


```

---

### [OK] 20966cdb - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-08-18T16:33:19Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 452ff47e - Remove Pyre mode headers
**Author:** generatedunixname89002005307016 <generatedunixname89002005307016@users.noreply.github.com> | **Date:** 2026-08-17T20:03:12Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Remove Pyre mode headers]


```

---

### [OK] caba4e99 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-08-17T16:31:51Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 1f639339 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-08-16T16:33:38Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] a043349b - Bump tokio 1.53.0 -> 1.53.1
**Author:** generatedunixname2066905484085733 <generatedunixname2066905484085733@users.noreply.github.com> | **Date:** 2026-08-15T21:17:32Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Bump tokio 1.53.0 - 1.53.1]


```

---

### [OK] 6bc0f6c4 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-08-15T16:32:50Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 24a16017 - Clean up unneeded dependencies_override
**Author:** David Tolnay <davidtolnay@users.noreply.github.com> | **Date:** 2026-08-14T17:23:01Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Clean up unneeded dependencies_override]


```

---

### [OK] 395b992d - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-08-14T16:32:38Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] c8aed456 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-08-13T16:33:26Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 5d61fefa - Attach hgcache usage stats to prefetch results
**Author:** Aaron Kushner <aaronkushner@users.noreply.github.com> | **Date:** 2026-08-12T03:43:57Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Attach hgcache usage stats to prefetch results]


```

---

### [OK] 27582199 - cmake: remove rocksdb build deps
**Author:** Jun Wu <junwu@users.noreply.github.com> | **Date:** 2026-08-11T19:51:00Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[cmake: remove rocksdb build deps]


```

---

### [OK] 141020bd - Prevent concurrent LFS downloads from corrupting archives
**Author:** Jose García Gimeno <josegarcagimeno@users.noreply.github.com> | **Date:** 2026-08-07T16:57:40Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Prevent concurrent LFS downloads from corrupting archives]


```

---

### [OK] 16ff2318 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-08-06T16:33:03Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 7bc35eb0 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-08-05T16:33:30Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] d877d4b1 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-08-04T16:32:21Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] d2ebc2e0 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-08-03T16:32:06Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] ce8beec7 - Add EdenFS shutdown stream error schema
**Author:** Muir Manders <muirmanders@users.noreply.github.com> | **Date:** 2026-08-03T16:11:46Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Add EdenFS shutdown stream error schema]


```

---

### [OK] c7610312 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-08-02T16:34:24Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 2232a6d1 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-08-01T16:33:32Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 542ccb79 - glob: share result path prefixes through Thrift responses
**Author:** Muir Manders <muirmanders@users.noreply.github.com> | **Date:** 2026-07-31T22:09:32Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[glob: share result path prefixes through Thrift responses]


```

---

### [OK] 5da6d65d - glob: add shared result path type
**Author:** Muir Manders <muirmanders@users.noreply.github.com> | **Date:** 2026-07-31T22:09:32Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[glob: add shared result path type]


```

---

### [OK] 7781579c - Add WatchmanXplatLogger instance and wire the structured-logger gate
**Author:** Jane Zhang <janezhang@users.noreply.github.com> | **Date:** 2026-07-31T20:09:02Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Add WatchmanXplatLogger instance and wire the structured-logger gate]


```

---

### [OK] 00babc3f - Add XplatKeys, WatchmanXplatTransforms, and transform test
**Author:** Jane Zhang <janezhang@users.noreply.github.com> | **Date:** 2026-07-31T20:09:02Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Add XplatKeys WatchmanXplatTransforms and transform test]


```

---

### [OK] 480e2874 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-31T16:33:12Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] ce89a60a - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-30T16:32:01Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 1dcef85e - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-29T16:31:53Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 8da95c6b - Add watchman_events LoggerConfig, entry thrift, and drift tests
**Author:** Jane Zhang <janezhang@users.noreply.github.com> | **Date:** 2026-07-28T23:39:01Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Add watchman_events LoggerConfig entry thrift and drift tests]


```

---

### [OK] 7254b6ea - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-28T16:34:12Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 33c9c8d0 - Suppress type errors for Pyre upgrade - opensource
**Author:** generatedunixname89002005307016 <generatedunixname89002005307016@users.noreply.github.com> | **Date:** 2026-07-27T20:34:26Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Suppress type errors for Pyre upgrade - opensource]


```

---

### [OK] 6a6fd011 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-27T16:32:02Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 1ab78e4f - rust: convert mod.rs to the 2018-edition layout in watchman (scm_client_infra)
**Author:** generatedunixname833006366474664 <generatedunixname833006366474664@users.noreply.github.com> | **Date:** 2026-07-27T08:28:49Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[rust: convert mod.rs to the 2018-edition layout in watchman scm_client_infra]


```

---

### [OK] 42464168 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-26T16:33:10Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 0e8742a0 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-25T16:32:18Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 928d0803 - watchman: only close non-cloexec inherited fds to avoid libunwind fd-reuse daemon corruption
**Author:** Callum Ryan <callumryan@users.noreply.github.com> | **Date:** 2026-07-24T22:34:17Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[watchman: only close non-cloexec inherited fds to avoid libunwind fd-reuse daemon corruption]


```

---

### [OK] a1d4e3e4 - Change default SAI version to 1.18.1 for OSS
**Author:** Siva Muthusamy <sivamuthusamy@users.noreply.github.com> | **Date:** 2026-07-24T20:18:45Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Change default SAI version to 1.18.1 for OSS]


```

---

### [OK] 6aa932e0 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-24T16:32:14Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 9ceaed12 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-23T16:36:56Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 806a1a42 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-22T16:32:58Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] de86698f - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-21T16:32:12Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] d423f7ea - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-20T16:32:15Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 1d379309 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-19T16:32:19Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 63552046 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-18T16:32:52Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 2cfeabca - Enable page-cache warmup workflows for prefetch
**Author:** Aaron Kushner <aaronkushner@users.noreply.github.com> | **Date:** 2026-07-18T08:06:30Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Enable page-cache warmup workflows for prefetch]


```

---

### [OK] cbfdd732 - Track blob sizes and cache origin for prefetch statistics
**Author:** Aaron Kushner <aaronkushner@users.noreply.github.com> | **Date:** 2026-07-18T07:10:03Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Track blob sizes and cache origin for prefetch statistics]


```

---

### [OK] 49ca94d7 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-17T16:32:56Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 1176c866 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-16T16:33:58Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 35897925 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-15T16:34:11Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 4c21f29c - Expose prefetch cache/byte stats to clients
**Author:** Aaron Kushner <aaronkushner@users.noreply.github.com> | **Date:** 2026-07-15T08:32:55Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Expose prefetch cache/byte stats to clients]


```

---

### [OK] f5b86739 - Back out "Fix cmake 3.31.12 macOS universal sha256 in manifest"
**Author:** Fausto Uribe <faustouribe@users.noreply.github.com> | **Date:** 2026-07-15T01:47:20Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Back out Fix cmake 3.31.12 macOS universal sha256 in manifest]


```

---

### [WRONG] 7ff5205d - Fix cmake 3.31.12 macOS universal sha256 in manifest
**Author:** Fausto Uribe <faustouribe@users.noreply.github.com> | **Date:** 2026-07-14T20:37:20Z
**Note:** Immediately followed by fix commit f5b86739 ("Back out "Fix cmake 3.31.12 macOS universal sha256 in manifest"") touching overlapping files (watchman.ts)

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Fix cmake 3.31.12 macOS universal sha256 in manifest]


```

---

### [WRONG] df8675f6 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-14T16:32:28Z
**Note:** Immediately followed by fix commit 7ff5205d ("Fix cmake 3.31.12 macOS universal sha256 in manifest") touching overlapping files (watchman.ts)

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 20ff9e34 - Sync fboss common.thrift into BGP OSS build via ShipIt pathmap
**Author:** Indu Suresh <indusuresh@users.noreply.github.com> | **Date:** 2026-07-13T22:52:26Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Sync fboss common.thrift into BGP OSS build via ShipIt pathmap]


```

---

### [OK] 6767dec1 - network_access on 49 test target(s) for scm_client_infra
**Author:** generatedunixname2127984611291935 <generatedunixname2127984611291935@users.noreply.github.com> | **Date:** 2026-07-13T19:27:34Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[network_access on 49 test targets for scm_client_infra]


```

---

### [OK] 104d632a - network_access on 18 test target(s) for scm_client_infra
**Author:** generatedunixname2127984611291935 <generatedunixname2127984611291935@users.noreply.github.com> | **Date:** 2026-07-13T18:49:03Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[network_access on 18 test targets for scm_client_infra]


```

---

### [OK] 6a3fac9b - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-13T16:31:12Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] ccc768b0 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-12T16:32:28Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 5ac002eb - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-11T16:31:59Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 21942223 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-10T16:31:32Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 904bb8ed - Add Watchman BSER module decode harness
**Author:** generatedunixname89002005279108 <generatedunixname89002005279108@users.noreply.github.com> | **Date:** 2026-07-10T15:38:46Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Add Watchman BSER module decode harness]


```

---

### [OK] 99ccc0f0 - Fix Windows Pyrefly type-check failures for POSIX-only APIs
**Author:** Xiaowei Lu <xiaoweilu@users.noreply.github.com> | **Date:** 2026-07-09T19:31:36Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Fix Windows Pyrefly type-check failures for POSIX-only APIs]


```

---

### [WRONG] 670a8650 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-09T16:31:53Z
**Note:** Immediately followed by fix commit 99ccc0f0 ("Fix Windows Pyrefly type-check failures for POSIX-only APIs") touching overlapping files (watchman.ts)

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 54602bca - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-08T16:31:14Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [WRONG] 3acb7941 - ci: fix Mac builds broken by llvm@20 pin in #47
**Author:** Richard Barnes <richardbarnes@users.noreply.github.com> | **Date:** 2026-07-07T21:21:16Z
**Note:** Self-identified failure / WIP in commit subject ("ci: fix Mac builds broken by llvm@20 pin in #47")

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[ci: fix Mac builds broken by llvm20 pin in 47]


```

---

### [WRONG] a819bf99 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-07T16:31:32Z
**Note:** Immediately followed by fix commit 3acb7941 ("ci: fix Mac builds broken by llvm@20 pin in #47") touching overlapping files (watchman.ts)

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 643368f3 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-06T16:31:25Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 22b541e2 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-05T16:32:21Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] a6e32cac - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-04T16:31:45Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 9656505f - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-03T16:33:04Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] dfae45bc - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-02T16:31:37Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] bc92fe7b - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-07-01T16:34:44Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [WRONG] 2e862a90 - ci: fix macOS wheel delocate failure and linux-sdk test_solve absence
**Author:** Richard Barnes <richardbarnes@users.noreply.github.com> | **Date:** 2026-06-30T19:26:39Z
**Note:** Self-identified failure / WIP in commit subject ("ci: fix macOS wheel delocate failure and linux-sdk test_solve absence")

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[ci: fix macOS wheel delocate failure and linux-sdk test_solve absence]


```

---

### [WRONG] 2cd72ade - ci: use default Xcode for Mac getdeps instead of pinned Xcode 16.2
**Author:** afrind <afrind@users.noreply.github.com> | **Date:** 2026-06-30T19:03:09Z
**Note:** Immediately followed by fix commit 2e862a90 ("ci: fix macOS wheel delocate failure and linux-sdk test_solve absence") touching overlapping files (watchman.ts)

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[ci: use default Xcode for Mac getdeps instead of pinned Xcode 16.2]


```

---

### [OK] 35d52cad - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-30T16:31:36Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 9d15ef9a - Persist ACL root state in overlays
**Author:** Michael Cuevas <michaelcuevas@users.noreply.github.com> | **Date:** 2026-06-30T02:12:31Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Persist ACL root state in overlays]


```

---

### [OK] 9a8e91ed - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-29T16:31:49Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 1fbb8795 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-28T16:32:37Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 3a2739a8 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-27T16:31:48Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 4208b46c - Bump getdeps Linux CI timeout to 120 minutes for 4-core runner
**Author:** Shitanshu Shah <shitanshushah@users.noreply.github.com> | **Date:** 2026-06-27T02:36:54Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Bump getdeps Linux CI timeout to 120 minutes for 4-core runner]


```

---

### [OK] 8eab3ee3 - Disable range-v3 internal tests on GCC 13
**Author:** Shitanshu Shah <shitanshushah@users.noreply.github.com> | **Date:** 2026-06-26T17:53:30Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Disable range-v3 internal tests on GCC 13]


```

---

### [OK] 3299a09f - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-26T16:32:01Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] cf786466 - Backfill AGENTS.md and refresh routing across onboarded projects
**Author:** Mutahir Kazmi <mutahirkazmi@users.noreply.github.com> | **Date:** 2026-06-25T21:43:38Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Backfill AGENTS.md and refresh routing across onboarded projects]


```

---

### [OK] 83b0ab65 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-25T16:31:39Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 8aa7033c - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-24T16:32:54Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 31d19c10 - Add Docker build for Explorer and JSON proxy, define github CI workflow
**Author:** Karthik Velakur <karthikvelakur@users.noreply.github.com> | **Date:** 2026-06-24T15:52:57Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Add Docker build for Explorer and JSON proxy define github CI workflow]


```

---

### [OK] d005c8ad - Add c-ares mintimeout patchfile for tp2 rebuild [2/4]
**Author:** Nathan Aclander <nathanaclander@users.noreply.github.com> | **Date:** 2026-06-24T07:42:31Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Add c-ares mintimeout patchfile for tp2 rebuild [2/4]]


```

---

### [OK] e3e5fefb - Update FBThriftCppLibrary.cmake to use PROJECT_SOURCE_DIR and PROJECT_BINARY_DIR
**Author:** Hongze Zhang <hongzezhang@users.noreply.github.com> | **Date:** 2026-06-23T23:32:13Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Update FBThriftCppLibrary.cmake to use PROJECT_SOURCE_DIR and PROJECT_BINARY_DIR]


```

---

### [OK] 57fb6cbc - F401 in fb_py_test_main.py
**Author:** generatedunixname949130641157030 <generatedunixname949130641157030@users.noreply.github.com> | **Date:** 2026-06-23T20:36:45Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[F401 in fb_py_test_main.py]


```

---

### [OK] 7fee3917 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-23T16:32:06Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] a9d89e7d - Remove double-conversion from open source projects
**Author:** Richard Barnes <richardbarnes@users.noreply.github.com> | **Date:** 2026-06-23T07:24:44Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Remove double-conversion from open source projects]


```

---

### [OK] f890a1f1 - Make tests opt-in via -DTESTS=ON (off by default)
**Author:** Richard Barnes <richardbarnes@users.noreply.github.com> | **Date:** 2026-06-22T21:42:00Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Make tests opt-in via -DTESTSON off by default]


```

---

### [OK] 27f6351b - list: report daemon namespace visibility
**Author:** Muir Manders <muirmanders@users.noreply.github.com> | **Date:** 2026-06-22T18:51:25Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[list: report daemon namespace visibility]


```

---

### [OK] c4e8e009 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-22T16:32:54Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] adcdd121 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-21T16:32:19Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 4811092c - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-20T16:32:01Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 1423c942 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-19T16:32:24Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 2dc761e7 - add visible restricted checkout conflict type
**Author:** Muir Manders <muirmanders@users.noreply.github.com> | **Date:** 2026-06-18T20:22:23Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[add visible restricted checkout conflict type]


```

---

### [OK] e5acf32a - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-18T16:32:11Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 63fc471a - Migrate 21 legacy graphs to repo-relative usecase names
**Author:** Mutahir Kazmi <mutahirkazmi@users.noreply.github.com> | **Date:** 2026-06-17T21:57:39Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Migrate 21 legacy graphs to repo-relative usecase names]


```

---

### [OK] 513f0713 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-17T16:33:32Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 9fe66d53 - Fix cmake 3.31.12 macOS universal sha256 in manifest
**Author:** Catherine Gasnier <catherinegasnier@users.noreply.github.com> | **Date:** 2026-06-17T12:46:59Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Fix cmake 3.31.12 macOS universal sha256 in manifest]


```

---

### [WRONG] ba7bd548 - Upgrade getdeps template's mozilla-actions/sccache-action from v0.0.9 -> v0.0.10
**Author:** Rob Lyerly <roblyerly@users.noreply.github.com> | **Date:** 2026-06-16T16:49:09Z
**Note:** Immediately followed by fix commit 9fe66d53 ("Fix cmake 3.31.12 macOS universal sha256 in manifest") touching overlapping files (watchman.ts)

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Upgrade getdeps templates mozilla-actions/sccache-action from v0.0.9 - v0.0.10]


```

---

### [OK] 4ad581b2 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-16T16:32:54Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] bd256324 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-15T16:33:03Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 6208d6fc - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-14T16:32:02Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] dbc7d845 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-13T16:32:45Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 02f03b8d - fbcode_builder: strip fbcode/eden/fs2 from eden/sapling OSS manifests
**Author:** George Giorgidze <georgegiorgidze@users.noreply.github.com> | **Date:** 2026-06-13T04:43:08Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[fbcode_builder: strip fbcode/eden/fs2 from eden/sapling OSS manifests]


```

---

### [OK] 21b3dce7 - pypi: update pyproject.toml
**Author:** Genevieve (Genna) Helsel <genevievegennahelsel@users.noreply.github.com> | **Date:** 2026-06-12T18:37:13Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[pypi: update pyproject.toml]


```

---

### [OK] 52593a9a - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-12T16:32:35Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] dfe0b2fd - Bump getdeps patchelf from 0.10 to 0.18.0; remove pip workaround
**Author:** Richard Barnes <richardbarnes@users.noreply.github.com> | **Date:** 2026-06-11T17:03:04Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Bump getdeps patchelf from 0.10 to 0.18.0 remove pip workaround]


```

---

### [OK] 533d0d8b - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-11T16:33:25Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 9c87ab87 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-10T16:32:18Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] b39e19a6 - remove fpic patches from run-getdeps.py
**Author:** Kevin Yakar <kevinyakar@users.noreply.github.com> | **Date:** 2026-06-10T00:12:18Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[remove fpic patches from run-getdeps.py]


```

---

### [OK] e755aabf - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-09T16:32:17Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 2b0ce5cb - Richard/packaging
**Author:** Richard Barnes <richardbarnes@users.noreply.github.com> | **Date:** 2026-06-08T21:15:05Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Richard/packaging]


```

---

### [OK] 6873adcd - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-08T16:32:44Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 91b2a105 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-07T16:33:17Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] ded885dd - Remove spurious libclang-dev from zstd manifest
**Author:** Richard Barnes <richardbarnes@users.noreply.github.com> | **Date:** 2026-06-07T00:01:04Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Remove spurious libclang-dev from zstd manifest]


```

---

### [OK] 57348e1c - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-06T16:31:54Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 051f7e46 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-05T16:32:59Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 35316a05 - getdeps: drop getdeps test, keep build
**Author:** Jun Wu <junwu@users.noreply.github.com> | **Date:** 2026-06-05T02:13:47Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[getdeps: drop getdeps test keep build]


```

---

### [OK] 7cc49e85 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-04T16:33:28Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 36a6a2ef - Add Sandcastle CI job and CMake build for BGP++ OSS
**Author:** Indu Suresh <indusuresh@users.noreply.github.com> | **Date:** 2026-06-04T15:52:10Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Add Sandcastle CI job and CMake build for BGP OSS]


```

---

### [OK] b1ad139d - Upgrade CMake to 3.31.12 and fix Windows sccache via /Z7
**Author:** Joseph Beshay <josephbeshay@users.noreply.github.com> | **Date:** 2026-06-03T17:39:11Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Upgrade CMake to 3.31.12 and fix Windows sccache via /Z7]


```

---

### [WRONG] 59916a20 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-03T16:32:37Z
**Note:** Immediately followed by fix commit b1ad139d ("Upgrade CMake to 3.31.12 and fix Windows sccache via /Z7") touching overlapping files (watchman.ts)

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 0d348167 - fbcode_builder: force snappy to build with RTTI
**Author:** Alan Frindell <alanfrindell@users.noreply.github.com> | **Date:** 2026-06-02T17:59:35Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[fbcode_builder: force snappy to build with RTTI]


```

---

### [OK] 72de96e8 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-02T16:39:33Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 3a235652 - Add `[shipit.strip]` section to getdeps manifest
**Author:** Indu Suresh <indusuresh@users.noreply.github.com> | **Date:** 2026-06-01T23:10:04Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Add [shipit.strip] section to getdeps manifest]


```

---

### [OK] da5f7c0d - Enable Pyrefly in fbcode/watchman
**Author:** generatedunixname89002005307016 <generatedunixname89002005307016@users.noreply.github.com> | **Date:** 2026-06-01T22:21:05Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Enable Pyrefly in fbcode/watchman]


```

---

### [OK] ebc615d2 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-06-01T16:32:08Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] dd004449 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-05-31T16:32:30Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] b88c105c - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-05-30T16:31:57Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 1548d949 - expose Channel type and FUSE transport type via Thrift `listMounts`
**Author:** Kaveh Ahmadi <kavehahmadi@users.noreply.github.com> | **Date:** 2026-05-30T00:02:50Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[expose Channel type and FUSE transport type via Thrift listMounts]


```

---

### [OK] 9a934981 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-05-29T16:33:30Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] b854dcf4 - facebook-unused-include-check in Backtrace.cpp
**Author:** generatedunixname923199350665076 <generatedunixname923199350665076@users.noreply.github.com> | **Date:** 2026-05-29T13:56:31Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[facebook-unused-include-check in Backtrace.cpp]


```

---

### [OK] 69f7f844 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-05-28T16:32:13Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] ca920c5b - Vendor openr IDL + NetworkUtil, OSS build fixes
**Author:** Indu Suresh <indusuresh@users.noreply.github.com> | **Date:** 2026-05-27T20:49:43Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Vendor openr IDL  NetworkUtil OSS build fixes]


```

---

### [WRONG] 46a4c248 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-05-27T16:33:02Z
**Note:** Immediately followed by fix commit ca920c5b ("Vendor openr IDL + NetworkUtil, OSS build fixes") touching overlapping files (watchman.ts)

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 5e6ec176 - Fix missing Meta+license text in new file
**Author:** Rob Lyerly <roblyerly@users.noreply.github.com> | **Date:** 2026-05-26T22:32:27Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Fix missing Metalicense text in new file]


```

---

### [WRONG] c46ae6c6 - Upgrade gperf to 3.3 in fbcode_builder manifests
**Author:** Alan Frindell <alanfrindell@users.noreply.github.com> | **Date:** 2026-05-26T18:50:21Z
**Note:** Immediately followed by fix commit 5e6ec176 ("Fix missing Meta+license text in new file") touching overlapping files (watchman.ts)

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Upgrade gperf to 3.3 in fbcode_builder manifests]


```

---

### [OK] 2c7027d9 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-05-26T16:32:26Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] 310048af - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-05-25T16:32:16Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```

---

### [OK] b1688822 - Updating hashes
**Author:** Open Source Bot <opensourcebot@users.noreply.github.com> | **Date:** 2026-05-24T16:31:49Z

```diff
diff --git a/watchman.ts b/watchman.ts
--- a/watchman.ts
+++ b/watchman.ts
@@ -1,1 +1,3 @@
+[Updating hashes]


```
