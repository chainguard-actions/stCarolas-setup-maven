<!-- markdownlint-disable -->

# Hardening Report: stCarolas--setup-maven/v5.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **stCarolas--setup-maven/v5.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### prior-fix (severity: high)

Carried forward from this version's previous harden, which fixed it; fix it again unless the code no longer has the problem. The previous harden changed lib/setup-maven.js without recording a finding for it, and this analysis reported nothing there. Re-apply its security changes unless the code no longer needs them. The previous patch:
```diff
diff --git a/lib/setup-maven.js b/lib/setup-maven.js
index 4ef56e4..bfd6440 100644
--- a/lib/setup-maven.js
+++ b/lib/setup-maven.js
@@ -1,50 +1,5409 @@
 "use strict";
-var __createBinding = (this && this.__createBinding) || (Object.create ? (function(o, m, k, k2) {
-    if (k2 === undefined) k2 = k;
-    var desc = Object.getOwnPropertyDescriptor(m, k);
-    if (!desc || ("get" in desc ? !m.__esModule : desc.writable || desc.configurable)) {
-      desc = { enumerable: true, get: function() { return m[k]; } };
+var __create = Object.create;
+var __defProp = Object.defineProperty;
+var __getOwnPropDesc = Object.getOwnPropertyDescriptor;
+var __getOwnPropNames = Object.getOwnPropertyNames;
+var __getProtoOf = Object.getPrototypeOf;
+var __hasOwnProp = Object.prototype.hasOwnProperty;
+var __esm = (fn, res, err) => function __init() {
+  if (err) throw err[0];
+  try {
+    return fn && (res = (0, fn[__getOwnPropNames(fn)[0]])(fn = 0)), res;
+  } catch (e) {
+    throw err = [e], e;
+  }
+};
+var __commonJS = (cb, mod) => function __require() {
+  try {
+    return mod || (0, cb[__getOwnPropNames(cb)[0]])((mod = { exports: {} }).exports, mod), mod.exports;
+  } catch (e) {
+    throw mod = 0, e;
+  }
+};
+var __export = (target, all) => {
+  for (var name in all)
+    __defProp(target, name, { get: all[name], enumerable: true });
+};
+var __copyProps = (to, from, except, desc) => {
+  if (from && typeof from === "object" || typeof from === "function") {
+    for (let key of __getOwnPropNames(from))
+      if (!__hasOwnProp.call(to, key) && key !== except)
+        __defProp(to, key, { get: () => from[key], enumerable: !(desc = __getOwnPropDesc(from, key)) || desc.enumerable });
+  }
+  return to;
+};
+var __toESM = (mod, isNodeMode, target) => (target = mod != null ? __create(__getProtoOf(mod)) : {}, __copyProps(
+  // If the importer is in node compatibility mode or this is not an ESM
+  // file that has been converted to a CommonJS file using a Babel-
+  // compatible transform (i.e. "__esModule" has not been set), then set
+  // "default" to the CommonJS "module.exports" for node compatibility.
+  isNodeMode || !mod || !mod.__esModule ? __defProp(target, "default", { value: mod, enumerable: true }) : target,
+  mod
+));
+var __toCommonJS = (mod) => __copyProps(__defProp({}, "__esModule", { value: true }), mod);
+
+// node_modules/@actions/core/lib/utils.js
+var require_utils = __commonJS({
+  "node_modules/@actions/core/lib/utils.js"(exports2) {
+    "use strict";
+    Object.defineProperty(exports2, "__esModule", { value: true });
+    exports2.toCommandProperties = exports2.toCommandValue = void 0;
+    function toCommandValue(input) {
+      if (input === null || input === void 0) {
+        return "";
+      } else if (typeof input === "string" || input instanceof String) {
+        return input;
+      }
+      return JSON.stringify(input);
+    }
+    exports2.toCommandValue = toCommandValue;
+    function toCommandProperties(annotationProperties) {
+      if (!Object.keys(annotationProperties).length) {
+        return {};
+      }
+      return {
+        title: annotationProperties.title,
+        file: annotationProperties.file,
+        line: annotationProperties.startLine,
+        endLine: annotationProperties.endLine,
+        col: annotationProperties.startColumn,
+        endColumn: annotationProperties.endColumn
+      };
+    }
+    exports2.toCommandProperties = toCommandProperties;
+  }
+});
+
+// node_modules/@actions/core/lib/command.js
+var require_command = __commonJS({
+  "node_modules/@actions/core/lib/command.js"(exports2) {
+    "use strict";
+    var __createBinding2 = exports2 && exports2.__createBinding || (Object.create ? (function(o, m, k, k2) {
+      if (k2 === void 0) k2 = k;
+      Object.defineProperty(o, k2, { enumerable: true, get: function() {
+        return m[k];
+      } });
+    }) : (function(o, m, k, k2) {
+      if (k2 === void 0) k2 = k;
+      o[k2] = m[k];
+    }));
+    var __setModuleDefault2 = exports2 && exports2.__setModuleDefault || (Object.create ? (function(o, v
... (truncated)
```

Locations:

- `lib/setup-maven.js`

## Iteration Notes

### Iteration 1

**Notes:**

No code changes made. The previous patch replaced lib/setup-maven.js with a ~5400-line build-tool bundle (esbuild) of the dependencies. As far as the visible part of the diff shows, it repackaged the action without changing its behaviour, and it can't be reliably recreated by hand. The current lib/setup-maven.js and lib/installer.js match upstream's compiled TypeScript: they read the maven-version input and download Maven from repo.maven.apache.org through @actions/tool-cache, then call core.addPath. They run no shell, so nothing can be injected, and nothing is fetched using the mirror's ref. Dependencies are committed in node_modules (@actions/core 1.10.0, which uses file-based commands), so the action runs as is. action.yml (node24) has no uses:, run:, permissions or credentials to fix. If the rebuild was meant to update dependencies, it needs to be done by running the bundler, not by editing files here.

