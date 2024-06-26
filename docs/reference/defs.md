# `//ll:defs.bzl`

Import these rules in your `BUILD.bazel` files.

To load for example the `ll_binary` rule:

```python
load("@rules_ll//ll:defs.bzl", "ll_binary")
```

<a id="ll_binary"></a>

## `ll_binary`

<pre><code>ll_binary(<a href="#ll_binary-name">name</a>, <a href="#ll_binary-deps">deps</a>, <a href="#ll_binary-srcs">srcs</a>, <a href="#ll_binary-data">data</a>, <a href="#ll_binary-hdrs">hdrs</a>, <a href="#ll_binary-angled_includes">angled_includes</a>, <a href="#ll_binary-compilation_mode">compilation_mode</a>, <a href="#ll_binary-compile_flags">compile_flags</a>,
          <a href="#ll_binary-compile_string_flags">compile_string_flags</a>, <a href="#ll_binary-defines">defines</a>, <a href="#ll_binary-depends_on_llvm">depends_on_llvm</a>, <a href="#ll_binary-experimental_device_intrinsics">experimental_device_intrinsics</a>,
          <a href="#ll_binary-exposed_angled_includes">exposed_angled_includes</a>, <a href="#ll_binary-exposed_defines">exposed_defines</a>, <a href="#ll_binary-exposed_hdrs">exposed_hdrs</a>, <a href="#ll_binary-exposed_includes">exposed_includes</a>,
          <a href="#ll_binary-exposed_interfaces">exposed_interfaces</a>, <a href="#ll_binary-includes">includes</a>, <a href="#ll_binary-interfaces">interfaces</a>, <a href="#ll_binary-libraries">libraries</a>, <a href="#ll_binary-link_flags">link_flags</a>, <a href="#ll_binary-link_string_flags">link_string_flags</a>,
          <a href="#ll_binary-sanitize">sanitize</a>, <a href="#ll_binary-toolchain_configuration">toolchain_configuration</a>)</code></pre>
Creates an executable.

Example:

  ```python
  ll_binary(
      srcs = ["my_executable.cpp"],
  )
  ```
`attributes`

| Name  | Description |
| :---- | :---------- |
| <a id="ll_binary-name"></a>`name` | <code><a href="https://bazel.build/concepts/labels#target-names">Name</a></code>, required.<br><br> A unique name for this target.   |
| <a id="ll_binary-deps"></a>`deps` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> The dependencies for this target.<br><br>Use `ll_library` targets here. Other targets won't work.   |
| <a id="ll_binary-srcs"></a>`srcs` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> Compilable source files for this target.<br><br>Allowed file extensions: `[".ll", ".o", ".S", ".c", ".cl", ".cpp"]`.<br><br>Place headers in the `hdrs` attribute.   |
| <a id="ll_binary-data"></a>`data` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> Extra files made available to compilation and linking steps.<br><br>Not appended to the default command line arguments, but available to actions. Reference these files manually for instance in the `includes`, and `compile_flags` attributes.<br><br>Use this attribute to make intermediary outputs from non-ll targets, for example from `rules_cc` or `filegroup`, available to the rule.   |
| <a id="ll_binary-hdrs"></a>`hdrs` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> Header files for this target.<br><br>When including a header file with a nested path, for instance `#include "some/path/myheader.h"`, add `"some/path"` to `includes` to make it visible to the rule.<br><br>Unavailable to downstream targets.   |
| <a id="ll_binary-angled_includes"></a>`angled_includes` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Angled include paths, relative to the target workspace.<br><br>Useful if you require include prefix stripping for dynamic paths, for instance the ones generated by `bzlmod`. Instead of `compile_flags = ["-Iexternal/mydep.someversion/include"]`, use `angled_includes = ["include"]` to add the path to the workspace automatically.<br><br>Expands paths starting with `$(GENERATED)` to the workspace location in the `GENDIR` path.<br><br>Unavailable to downstream targets.   |
| <a id="ll_binary-compilation_mode"></a>`compilation_mode` | <code>String</code>, optional, defaults to <code>"cpp"</code>.<br><br> Enables compilation of heterogeneous single source files.<br><br>Prefer this attribute over adding SYCL/HIP/CUDA flags manually in the `compile_flags` and `link_flags`.<br><br>See [CUDA and HIP](../guides/cuda_and_hip.md).<br><br>`"cpp"` The default C++ toolchain.<br><br>`"cuda_nvptx"` The CUDA toolchain.<br><br>`"cuda_nvptx_nvcc"` The CUDA toolchain with `nvcc` as device compiler.<br><br>`"hip_nvptx"` The HIP toolchain.<br><br>`"bootstrap"` The bootstrap toolchain used by internal dependencies of the `ll_toolchain`.   |
| <a id="ll_binary-compile_flags"></a>`compile_flags` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Flags for the compiler.<br><br>Pass a list of strings here. For instance `["-O3", "-std=c++20"]`.<br><br>Split flag pairs `-Xclang -somearg` into separate flags `["-Xclang", "-somearg"]`.<br><br>Unavailable to downstream targets.   |
| <a id="ll_binary-compile_string_flags"></a>`compile_string_flags` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> Flags for the compiler in the form of `string_flag`s.<br><br>Splits the values of each `string_flag` along colons like so:<br><br><pre><code class="language-python">load("@bazel_skylib//rules:common_settings.bzl", "string_flag")&#10;&#10;string_flag(&#10;    name = "myflags",&#10;    build_setting_default = "a:b:c",&#10;)&#10;&#10;ll_library(&#10;    # ...&#10;    # Equivalent to `compile_flags = ["a", "b", "c"]`&#10;    compile_string_flags = [":myflags"],&#10;)</code></pre><br><br>Useful for externally configurable build attributes, such as generated flags from Nix environments.   |
| <a id="ll_binary-defines"></a>`defines` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Defines for this target.<br><br>Pass a list of strings here. For instance `["MYDEFINE_1", "MYDEFINE_2"]`.<br><br>Unavailable to downstream targets.   |
| <a id="ll_binary-depends_on_llvm"></a>`depends_on_llvm` | <code>Boolean</code>, optional, defaults to <code>False</code>.<br><br> Whether this target directly depends on targets from the `llvm-project-overlay`.<br><br>Setting this to `True` makes the `cc_library` targets from the LLVM project overlay available to this target.   |
| <a id="ll_binary-experimental_device_intrinsics"></a>`experimental_device_intrinsics` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> Custom intrinsics for device compilation.<br><br>Adds `-Xarch_device -include<thefile>` to the compile commands for this target.   |
| <a id="ll_binary-exposed_angled_includes"></a>`exposed_angled_includes` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Exposed angled include paths, relative to the original target workspace.<br><br>Expands paths starting with `$(GENERATED)` to the workspace location in the `GENDIR` path.<br><br>Added to the compile command line arguments of direct dependents.   |
| <a id="ll_binary-exposed_defines"></a>`exposed_defines` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Exposed defines for this target.<br><br>Added to the compile command line arguments of direct dependents.   |
| <a id="ll_binary-exposed_hdrs"></a>`exposed_hdrs` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> Exposed headers for this target.<br><br>Direct dependents can see exposed headers. Put the public API headers for libraries here.   |
| <a id="ll_binary-exposed_includes"></a>`exposed_includes` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Exposed include paths, relative to the original target workspace.<br><br>Expands paths starting with `$(GENERATED)` to the workspace location in the `GENDIR` path.<br><br>Added to the compile command line arguments of direct dependents.   |
| <a id="ll_binary-exposed_interfaces"></a>`exposed_interfaces` | <code><a href="https://bazel.build/rules/lib/dict">Dictionary: Label -> String</a></code>, optional, defaults to <code>{}</code>.<br><br> Transitive interfaces for this target.<br><br>See [C++ modules](../guides/modules.md) for a guide.<br><br>Makes precompiled modules and compiled objects visible to direct dependents. Files in this attribute can see BMIs from modules in `interfaces`.<br><br>Primary module interfaces go here.   |
| <a id="ll_binary-includes"></a>`includes` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Include paths, relative to the target workspace.<br><br>Uses `-iquote`.<br><br>Useful if you need custom include prefix stripping for dynamic paths, for instance the ones generated by `bzlmod`. Instead of `compile_flags = ["-iquoteexternal/mydep.someversion/include"]`, use `includes = ["include"]` to add the path to the workspace automatically.<br><br>Expands paths starting with `$(GENERATED)` to the workspace location in the `GENDIR` path.<br><br>Unavailable to downstream targets.   |
| <a id="ll_binary-interfaces"></a>`interfaces` | <code><a href="https://bazel.build/rules/lib/dict">Dictionary: Label -> String</a></code>, optional, defaults to <code>{}</code>.<br><br> Module interfaces for this target.<br><br>See [C++ modules](../guides/modules.md) for a guide.<br><br>Makes precompiled modules and compiled objects visible to direct dependents and to `exposed_interfaces`.<br><br>For instance, you can put module partitions in `interfaces` and the primary module interface in `exposed_interfaces`.<br><br>Files in the same `interfaces` attribute can't see each other's BMIs.   |
| <a id="ll_binary-libraries"></a>`libraries` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> Libraries linked to the final executable.<br><br>Adds these libraries to the command line arguments for the linker.   |
| <a id="ll_binary-link_flags"></a>`link_flags` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Flags for the linker.<br><br>Place library search paths and external link targets here.<br><br>Assuming you have a library `/some/path/libmylib.a` on your host system, you can make `mylib.a` available to the linker by passing `["-L/some/path", "-lmylib"]` to this attribute.<br><br>Prefer the `libraries` attribute for library files already present within the Bazel build graph.   |
| <a id="ll_binary-link_string_flags"></a>`link_string_flags` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> Flags for the linker in the form of `string_flag`s.<br><br>See `compile_string_flags` for semantics.<br><br>Prefer the `libraries` attribute for library files already present within the Bazel build graph.   |
| <a id="ll_binary-sanitize"></a>`sanitize` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Enable sanitizers for this target.<br><br>See the [Sanitizers](../guides/sanitizers.md) guide.<br><br>Some sanitizers come with heavy performance penalties. Enabling several sanitizers at the same time often breaks builds. If possible, use just one at a time.<br><br>Sanitizers produce nondeterministic error reports. Run sanitized executables several times and build them with different optimization levels to maximize coverage.<br><br>`"address"`: Enable AddressSanitizer to detect memory errors.<br><br>`"leak"`: Enable LeakSanitizer to detect memory leaks.<br><br>`"memory"`: Enable MemorySanitizer to detect uninitialized reads.<br><br>`"undefined_behavior"`: Enable UndefinedBehaviorSanitizer to detect     undefined behavior.<br><br>`"thread"`: Enable ThreadSanitizer to detect data races.   |
| <a id="ll_binary-toolchain_configuration"></a>`toolchain_configuration` | <code><a href="https://bazel.build/concepts/labels">Label</a></code>, optional, defaults to <code>"@rules_ll//ll:current_ll_toolchain_configuration"</code>.<br><br> TODO   |


<a id="ll_compilation_database"></a>

## `ll_compilation_database`

<pre><code>ll_compilation_database(<a href="#ll_compilation_database-name">name</a>, <a href="#ll_compilation_database-config">config</a>, <a href="#ll_compilation_database-exclude">exclude</a>, <a href="#ll_compilation_database-targets">targets</a>)</code></pre>
Executable target for building a
[compilation database](https://clang.llvm.org/docs/JSONCompilationDatabase.html)
and running [clang-tidy](https://clang.llvm.org/extra/clang-tidy/) on it.

For a full guide see [Clang-Tidy](../guides/clang_tidy.md).

See [`rules_ll/examples`](https://github.com/eomii/rules_ll/tree/main/examples) for examples.
`attributes`

| Name  | Description |
| :---- | :---------- |
| <a id="ll_compilation_database-name"></a>`name` | <code><a href="https://bazel.build/concepts/labels#target-names">Name</a></code>, required.<br><br> A unique name for this target.   |
| <a id="ll_compilation_database-config"></a>`config` | <code><a href="https://bazel.build/concepts/labels">Label</a></code>, required.<br><br> The label of a `.clang-tidy` configuration file.   |
| <a id="ll_compilation_database-exclude"></a>`exclude` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Exclude all targets whose path includes one at least one of the provided strings.   |
| <a id="ll_compilation_database-targets"></a>`targets` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, required.<br><br> The labels added to the compilation database.   |


<a id="ll_coverage_test"></a>

## `ll_coverage_test`

<pre><code>ll_coverage_test(<a href="#ll_coverage_test-name">name</a>, <a href="#ll_coverage_test-target">target</a>)</code></pre>
TODO
`attributes`

| Name  | Description |
| :---- | :---------- |
| <a id="ll_coverage_test-name"></a>`name` | <code><a href="https://bazel.build/concepts/labels#target-names">Name</a></code>, required.<br><br> A unique name for this target.   |
| <a id="ll_coverage_test-target"></a>`target` | <code><a href="https://bazel.build/concepts/labels">Label</a></code>, required.<br><br> The executable to run and collect coverage data from.   |


<a id="ll_library"></a>

## `ll_library`

<pre><code>ll_library(<a href="#ll_library-name">name</a>, <a href="#ll_library-deps">deps</a>, <a href="#ll_library-srcs">srcs</a>, <a href="#ll_library-data">data</a>, <a href="#ll_library-hdrs">hdrs</a>, <a href="#ll_library-angled_includes">angled_includes</a>, <a href="#ll_library-compilation_mode">compilation_mode</a>, <a href="#ll_library-compile_flags">compile_flags</a>,
           <a href="#ll_library-compile_string_flags">compile_string_flags</a>, <a href="#ll_library-defines">defines</a>, <a href="#ll_library-depends_on_llvm">depends_on_llvm</a>, <a href="#ll_library-emit">emit</a>, <a href="#ll_library-experimental_device_intrinsics">experimental_device_intrinsics</a>,
           <a href="#ll_library-exposed_angled_includes">exposed_angled_includes</a>, <a href="#ll_library-exposed_defines">exposed_defines</a>, <a href="#ll_library-exposed_hdrs">exposed_hdrs</a>, <a href="#ll_library-exposed_includes">exposed_includes</a>,
           <a href="#ll_library-exposed_interfaces">exposed_interfaces</a>, <a href="#ll_library-includes">includes</a>, <a href="#ll_library-interfaces">interfaces</a>, <a href="#ll_library-sanitize">sanitize</a>, <a href="#ll_library-shared_object_link_flags">shared_object_link_flags</a>,
           <a href="#ll_library-shared_object_link_string_flags">shared_object_link_string_flags</a>, <a href="#ll_library-toolchain_configuration">toolchain_configuration</a>, <a href="#ll_library-version_script">version_script</a>)</code></pre>
Creates a static archive.

Example:

  ```python
  ll_library(
      srcs = ["my_library.cpp"],
  )
  ```
`attributes`

| Name  | Description |
| :---- | :---------- |
| <a id="ll_library-name"></a>`name` | <code><a href="https://bazel.build/concepts/labels#target-names">Name</a></code>, required.<br><br> A unique name for this target.   |
| <a id="ll_library-deps"></a>`deps` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> The dependencies for this target.<br><br>Use `ll_library` targets here. Other targets won't work.   |
| <a id="ll_library-srcs"></a>`srcs` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> Compilable source files for this target.<br><br>Allowed file extensions: `[".ll", ".o", ".S", ".c", ".cl", ".cpp"]`.<br><br>Place headers in the `hdrs` attribute.   |
| <a id="ll_library-data"></a>`data` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> Extra files made available to compilation and linking steps.<br><br>Not appended to the default command line arguments, but available to actions. Reference these files manually for instance in the `includes`, and `compile_flags` attributes.<br><br>Use this attribute to make intermediary outputs from non-ll targets, for example from `rules_cc` or `filegroup`, available to the rule.   |
| <a id="ll_library-hdrs"></a>`hdrs` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> Header files for this target.<br><br>When including a header file with a nested path, for instance `#include "some/path/myheader.h"`, add `"some/path"` to `includes` to make it visible to the rule.<br><br>Unavailable to downstream targets.   |
| <a id="ll_library-angled_includes"></a>`angled_includes` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Angled include paths, relative to the target workspace.<br><br>Useful if you require include prefix stripping for dynamic paths, for instance the ones generated by `bzlmod`. Instead of `compile_flags = ["-Iexternal/mydep.someversion/include"]`, use `angled_includes = ["include"]` to add the path to the workspace automatically.<br><br>Expands paths starting with `$(GENERATED)` to the workspace location in the `GENDIR` path.<br><br>Unavailable to downstream targets.   |
| <a id="ll_library-compilation_mode"></a>`compilation_mode` | <code>String</code>, optional, defaults to <code>"cpp"</code>.<br><br> Enables compilation of heterogeneous single source files.<br><br>Prefer this attribute over adding SYCL/HIP/CUDA flags manually in the `compile_flags` and `link_flags`.<br><br>See [CUDA and HIP](../guides/cuda_and_hip.md).<br><br>`"cpp"` The default C++ toolchain.<br><br>`"cuda_nvptx"` The CUDA toolchain.<br><br>`"cuda_nvptx_nvcc"` The CUDA toolchain with `nvcc` as device compiler.<br><br>`"hip_nvptx"` The HIP toolchain.<br><br>`"bootstrap"` The bootstrap toolchain used by internal dependencies of the `ll_toolchain`.   |
| <a id="ll_library-compile_flags"></a>`compile_flags` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Flags for the compiler.<br><br>Pass a list of strings here. For instance `["-O3", "-std=c++20"]`.<br><br>Split flag pairs `-Xclang -somearg` into separate flags `["-Xclang", "-somearg"]`.<br><br>Unavailable to downstream targets.   |
| <a id="ll_library-compile_string_flags"></a>`compile_string_flags` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> Flags for the compiler in the form of `string_flag`s.<br><br>Splits the values of each `string_flag` along colons like so:<br><br><pre><code class="language-python">load("@bazel_skylib//rules:common_settings.bzl", "string_flag")&#10;&#10;string_flag(&#10;    name = "myflags",&#10;    build_setting_default = "a:b:c",&#10;)&#10;&#10;ll_library(&#10;    # ...&#10;    # Equivalent to `compile_flags = ["a", "b", "c"]`&#10;    compile_string_flags = [":myflags"],&#10;)</code></pre><br><br>Useful for externally configurable build attributes, such as generated flags from Nix environments.   |
| <a id="ll_library-defines"></a>`defines` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Defines for this target.<br><br>Pass a list of strings here. For instance `["MYDEFINE_1", "MYDEFINE_2"]`.<br><br>Unavailable to downstream targets.   |
| <a id="ll_library-depends_on_llvm"></a>`depends_on_llvm` | <code>Boolean</code>, optional, defaults to <code>False</code>.<br><br> Whether this target directly depends on targets from the `llvm-project-overlay`.<br><br>Setting this to `True` makes the `cc_library` targets from the LLVM project overlay available to this target.   |
| <a id="ll_library-emit"></a>`emit` | <code>List of strings</code>, optional, defaults to <code>["archive"]</code>.<br><br> Sets the output mode.<br><br>You can enable several output types at the same time.<br><br>`"archive"` invokes the archiver and adds an archive with a `.a` extension to the outputs.<br><br>`"shared_object"` invokes the linker and adds a shared object with a `.so` extension to the outputs.<br><br>`"objects"` adds loose object files to the outputs.   |
| <a id="ll_library-experimental_device_intrinsics"></a>`experimental_device_intrinsics` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> Custom intrinsics for device compilation.<br><br>Adds `-Xarch_device -include<thefile>` to the compile commands for this target.   |
| <a id="ll_library-exposed_angled_includes"></a>`exposed_angled_includes` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Exposed angled include paths, relative to the original target workspace.<br><br>Expands paths starting with `$(GENERATED)` to the workspace location in the `GENDIR` path.<br><br>Added to the compile command line arguments of direct dependents.   |
| <a id="ll_library-exposed_defines"></a>`exposed_defines` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Exposed defines for this target.<br><br>Added to the compile command line arguments of direct dependents.   |
| <a id="ll_library-exposed_hdrs"></a>`exposed_hdrs` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> Exposed headers for this target.<br><br>Direct dependents can see exposed headers. Put the public API headers for libraries here.   |
| <a id="ll_library-exposed_includes"></a>`exposed_includes` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Exposed include paths, relative to the original target workspace.<br><br>Expands paths starting with `$(GENERATED)` to the workspace location in the `GENDIR` path.<br><br>Added to the compile command line arguments of direct dependents.   |
| <a id="ll_library-exposed_interfaces"></a>`exposed_interfaces` | <code><a href="https://bazel.build/rules/lib/dict">Dictionary: Label -> String</a></code>, optional, defaults to <code>{}</code>.<br><br> Transitive interfaces for this target.<br><br>See [C++ modules](../guides/modules.md) for a guide.<br><br>Makes precompiled modules and compiled objects visible to direct dependents. Files in this attribute can see BMIs from modules in `interfaces`.<br><br>Primary module interfaces go here.   |
| <a id="ll_library-includes"></a>`includes` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Include paths, relative to the target workspace.<br><br>Uses `-iquote`.<br><br>Useful if you need custom include prefix stripping for dynamic paths, for instance the ones generated by `bzlmod`. Instead of `compile_flags = ["-iquoteexternal/mydep.someversion/include"]`, use `includes = ["include"]` to add the path to the workspace automatically.<br><br>Expands paths starting with `$(GENERATED)` to the workspace location in the `GENDIR` path.<br><br>Unavailable to downstream targets.   |
| <a id="ll_library-interfaces"></a>`interfaces` | <code><a href="https://bazel.build/rules/lib/dict">Dictionary: Label -> String</a></code>, optional, defaults to <code>{}</code>.<br><br> Module interfaces for this target.<br><br>See [C++ modules](../guides/modules.md) for a guide.<br><br>Makes precompiled modules and compiled objects visible to direct dependents and to `exposed_interfaces`.<br><br>For instance, you can put module partitions in `interfaces` and the primary module interface in `exposed_interfaces`.<br><br>Files in the same `interfaces` attribute can't see each other's BMIs.   |
| <a id="ll_library-sanitize"></a>`sanitize` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Enable sanitizers for this target.<br><br>See the [Sanitizers](../guides/sanitizers.md) guide.<br><br>Some sanitizers come with heavy performance penalties. Enabling several sanitizers at the same time often breaks builds. If possible, use just one at a time.<br><br>Sanitizers produce nondeterministic error reports. Run sanitized executables several times and build them with different optimization levels to maximize coverage.<br><br>`"address"`: Enable AddressSanitizer to detect memory errors.<br><br>`"leak"`: Enable LeakSanitizer to detect memory leaks.<br><br>`"memory"`: Enable MemorySanitizer to detect uninitialized reads.<br><br>`"undefined_behavior"`: Enable UndefinedBehaviorSanitizer to detect     undefined behavior.<br><br>`"thread"`: Enable ThreadSanitizer to detect data races.   |
| <a id="ll_library-shared_object_link_flags"></a>`shared_object_link_flags` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Flags for the linker when emitting shared objects.<br><br>Used if `emit` includes `"shared_object"`.   |
| <a id="ll_library-shared_object_link_string_flags"></a>`shared_object_link_string_flags` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> Flags for the linker when emitting shared objects in the form of `string_flag`s.<br><br>See `compile_string_flags` for semantics.<br><br>Used if `emit` includes `"shared_object"`.   |
| <a id="ll_library-toolchain_configuration"></a>`toolchain_configuration` | <code><a href="https://bazel.build/concepts/labels">Label</a></code>, optional, defaults to <code>"@rules_ll//ll:current_ll_toolchain_configuration"</code>.<br><br> TODO   |
| <a id="ll_library-version_script"></a>`version_script` | <code><a href="https://bazel.build/concepts/labels">Label</a></code>, optional, defaults to <code>None</code>.<br><br> Optional version script used during shared object linking.<br><br>Used if `emit` includes `"shared_object"`.   |


<a id="ll_test"></a>

## `ll_test`

<pre><code>ll_test(<a href="#ll_test-name">name</a>, <a href="#ll_test-deps">deps</a>, <a href="#ll_test-srcs">srcs</a>, <a href="#ll_test-data">data</a>, <a href="#ll_test-hdrs">hdrs</a>, <a href="#ll_test-angled_includes">angled_includes</a>, <a href="#ll_test-compilation_mode">compilation_mode</a>, <a href="#ll_test-compile_flags">compile_flags</a>,
        <a href="#ll_test-compile_string_flags">compile_string_flags</a>, <a href="#ll_test-defines">defines</a>, <a href="#ll_test-depends_on_llvm">depends_on_llvm</a>, <a href="#ll_test-experimental_device_intrinsics">experimental_device_intrinsics</a>,
        <a href="#ll_test-exposed_angled_includes">exposed_angled_includes</a>, <a href="#ll_test-exposed_defines">exposed_defines</a>, <a href="#ll_test-exposed_hdrs">exposed_hdrs</a>, <a href="#ll_test-exposed_includes">exposed_includes</a>, <a href="#ll_test-exposed_interfaces">exposed_interfaces</a>,
        <a href="#ll_test-includes">includes</a>, <a href="#ll_test-interfaces">interfaces</a>, <a href="#ll_test-libraries">libraries</a>, <a href="#ll_test-link_flags">link_flags</a>, <a href="#ll_test-link_string_flags">link_string_flags</a>, <a href="#ll_test-sanitize">sanitize</a>,
        <a href="#ll_test-toolchain_configuration">toolchain_configuration</a>)</code></pre>
Testable wrapper around `ll_binary`.

Consider using this rule over skylib's `native_test` targets to propagate shared
libraries to the test invocations.

Example:

  ```python
  ll_test(
      name = "amdgpu_test",
      srcs = ["my_executable.cpp"],
      compilation_mode = "hip_amdgpu",
      compile_flags = OFFLOAD_ALL_AMDGPU + [
          "-std=c++20",
      ],
      tags = ["amdgpu"],  # Not required, but makes grouping tests easier.
  )
  ```
`attributes`

| Name  | Description |
| :---- | :---------- |
| <a id="ll_test-name"></a>`name` | <code><a href="https://bazel.build/concepts/labels#target-names">Name</a></code>, required.<br><br> A unique name for this target.   |
| <a id="ll_test-deps"></a>`deps` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> The dependencies for this target.<br><br>Use `ll_library` targets here. Other targets won't work.   |
| <a id="ll_test-srcs"></a>`srcs` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> Compilable source files for this target.<br><br>Allowed file extensions: `[".ll", ".o", ".S", ".c", ".cl", ".cpp"]`.<br><br>Place headers in the `hdrs` attribute.   |
| <a id="ll_test-data"></a>`data` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> Extra files made available to compilation and linking steps.<br><br>Not appended to the default command line arguments, but available to actions. Reference these files manually for instance in the `includes`, and `compile_flags` attributes.<br><br>Use this attribute to make intermediary outputs from non-ll targets, for example from `rules_cc` or `filegroup`, available to the rule.   |
| <a id="ll_test-hdrs"></a>`hdrs` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> Header files for this target.<br><br>When including a header file with a nested path, for instance `#include "some/path/myheader.h"`, add `"some/path"` to `includes` to make it visible to the rule.<br><br>Unavailable to downstream targets.   |
| <a id="ll_test-angled_includes"></a>`angled_includes` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Angled include paths, relative to the target workspace.<br><br>Useful if you require include prefix stripping for dynamic paths, for instance the ones generated by `bzlmod`. Instead of `compile_flags = ["-Iexternal/mydep.someversion/include"]`, use `angled_includes = ["include"]` to add the path to the workspace automatically.<br><br>Expands paths starting with `$(GENERATED)` to the workspace location in the `GENDIR` path.<br><br>Unavailable to downstream targets.   |
| <a id="ll_test-compilation_mode"></a>`compilation_mode` | <code>String</code>, optional, defaults to <code>"cpp"</code>.<br><br> Enables compilation of heterogeneous single source files.<br><br>Prefer this attribute over adding SYCL/HIP/CUDA flags manually in the `compile_flags` and `link_flags`.<br><br>See [CUDA and HIP](../guides/cuda_and_hip.md).<br><br>`"cpp"` The default C++ toolchain.<br><br>`"cuda_nvptx"` The CUDA toolchain.<br><br>`"cuda_nvptx_nvcc"` The CUDA toolchain with `nvcc` as device compiler.<br><br>`"hip_nvptx"` The HIP toolchain.<br><br>`"bootstrap"` The bootstrap toolchain used by internal dependencies of the `ll_toolchain`.   |
| <a id="ll_test-compile_flags"></a>`compile_flags` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Flags for the compiler.<br><br>Pass a list of strings here. For instance `["-O3", "-std=c++20"]`.<br><br>Split flag pairs `-Xclang -somearg` into separate flags `["-Xclang", "-somearg"]`.<br><br>Unavailable to downstream targets.   |
| <a id="ll_test-compile_string_flags"></a>`compile_string_flags` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> Flags for the compiler in the form of `string_flag`s.<br><br>Splits the values of each `string_flag` along colons like so:<br><br><pre><code class="language-python">load("@bazel_skylib//rules:common_settings.bzl", "string_flag")&#10;&#10;string_flag(&#10;    name = "myflags",&#10;    build_setting_default = "a:b:c",&#10;)&#10;&#10;ll_library(&#10;    # ...&#10;    # Equivalent to `compile_flags = ["a", "b", "c"]`&#10;    compile_string_flags = [":myflags"],&#10;)</code></pre><br><br>Useful for externally configurable build attributes, such as generated flags from Nix environments.   |
| <a id="ll_test-defines"></a>`defines` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Defines for this target.<br><br>Pass a list of strings here. For instance `["MYDEFINE_1", "MYDEFINE_2"]`.<br><br>Unavailable to downstream targets.   |
| <a id="ll_test-depends_on_llvm"></a>`depends_on_llvm` | <code>Boolean</code>, optional, defaults to <code>False</code>.<br><br> Whether this target directly depends on targets from the `llvm-project-overlay`.<br><br>Setting this to `True` makes the `cc_library` targets from the LLVM project overlay available to this target.   |
| <a id="ll_test-experimental_device_intrinsics"></a>`experimental_device_intrinsics` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> Custom intrinsics for device compilation.<br><br>Adds `-Xarch_device -include<thefile>` to the compile commands for this target.   |
| <a id="ll_test-exposed_angled_includes"></a>`exposed_angled_includes` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Exposed angled include paths, relative to the original target workspace.<br><br>Expands paths starting with `$(GENERATED)` to the workspace location in the `GENDIR` path.<br><br>Added to the compile command line arguments of direct dependents.   |
| <a id="ll_test-exposed_defines"></a>`exposed_defines` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Exposed defines for this target.<br><br>Added to the compile command line arguments of direct dependents.   |
| <a id="ll_test-exposed_hdrs"></a>`exposed_hdrs` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> Exposed headers for this target.<br><br>Direct dependents can see exposed headers. Put the public API headers for libraries here.   |
| <a id="ll_test-exposed_includes"></a>`exposed_includes` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Exposed include paths, relative to the original target workspace.<br><br>Expands paths starting with `$(GENERATED)` to the workspace location in the `GENDIR` path.<br><br>Added to the compile command line arguments of direct dependents.   |
| <a id="ll_test-exposed_interfaces"></a>`exposed_interfaces` | <code><a href="https://bazel.build/rules/lib/dict">Dictionary: Label -> String</a></code>, optional, defaults to <code>{}</code>.<br><br> Transitive interfaces for this target.<br><br>See [C++ modules](../guides/modules.md) for a guide.<br><br>Makes precompiled modules and compiled objects visible to direct dependents. Files in this attribute can see BMIs from modules in `interfaces`.<br><br>Primary module interfaces go here.   |
| <a id="ll_test-includes"></a>`includes` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Include paths, relative to the target workspace.<br><br>Uses `-iquote`.<br><br>Useful if you need custom include prefix stripping for dynamic paths, for instance the ones generated by `bzlmod`. Instead of `compile_flags = ["-iquoteexternal/mydep.someversion/include"]`, use `includes = ["include"]` to add the path to the workspace automatically.<br><br>Expands paths starting with `$(GENERATED)` to the workspace location in the `GENDIR` path.<br><br>Unavailable to downstream targets.   |
| <a id="ll_test-interfaces"></a>`interfaces` | <code><a href="https://bazel.build/rules/lib/dict">Dictionary: Label -> String</a></code>, optional, defaults to <code>{}</code>.<br><br> Module interfaces for this target.<br><br>See [C++ modules](../guides/modules.md) for a guide.<br><br>Makes precompiled modules and compiled objects visible to direct dependents and to `exposed_interfaces`.<br><br>For instance, you can put module partitions in `interfaces` and the primary module interface in `exposed_interfaces`.<br><br>Files in the same `interfaces` attribute can't see each other's BMIs.   |
| <a id="ll_test-libraries"></a>`libraries` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> Libraries linked to the final executable.<br><br>Adds these libraries to the command line arguments for the linker.   |
| <a id="ll_test-link_flags"></a>`link_flags` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Flags for the linker.<br><br>Place library search paths and external link targets here.<br><br>Assuming you have a library `/some/path/libmylib.a` on your host system, you can make `mylib.a` available to the linker by passing `["-L/some/path", "-lmylib"]` to this attribute.<br><br>Prefer the `libraries` attribute for library files already present within the Bazel build graph.   |
| <a id="ll_test-link_string_flags"></a>`link_string_flags` | <code><a href="https://bazel.build/concepts/labels">List of labels</a></code>, optional, defaults to <code>[]</code>.<br><br> Flags for the linker in the form of `string_flag`s.<br><br>See `compile_string_flags` for semantics.<br><br>Prefer the `libraries` attribute for library files already present within the Bazel build graph.   |
| <a id="ll_test-sanitize"></a>`sanitize` | <code>List of strings</code>, optional, defaults to <code>[]</code>.<br><br> Enable sanitizers for this target.<br><br>See the [Sanitizers](../guides/sanitizers.md) guide.<br><br>Some sanitizers come with heavy performance penalties. Enabling several sanitizers at the same time often breaks builds. If possible, use just one at a time.<br><br>Sanitizers produce nondeterministic error reports. Run sanitized executables several times and build them with different optimization levels to maximize coverage.<br><br>`"address"`: Enable AddressSanitizer to detect memory errors.<br><br>`"leak"`: Enable LeakSanitizer to detect memory leaks.<br><br>`"memory"`: Enable MemorySanitizer to detect uninitialized reads.<br><br>`"undefined_behavior"`: Enable UndefinedBehaviorSanitizer to detect     undefined behavior.<br><br>`"thread"`: Enable ThreadSanitizer to detect data races.   |
| <a id="ll_test-toolchain_configuration"></a>`toolchain_configuration` | <code><a href="https://bazel.build/concepts/labels">Label</a></code>, optional, defaults to <code>"@rules_ll//ll:current_ll_toolchain_configuration"</code>.<br><br> TODO   |
