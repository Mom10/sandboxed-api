# Copyright 2019 Google LLC. All Rights Reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

licenses(["notice"])  # Apache 2.0

load("//sandboxed_api/bazel:sapi.bzl", "sapi_library")

py_library(
    name = "code",
    srcs = ["code.py"],
)

py_test(
    name = "code_test",
    size = "small",
    srcs = [
        "code_test.py",
        "code_test_util.py",
    ],
    python_version = "PY2",
    deps = [
        ":code",
        "@com_google_absl_py//absl/testing:absltest",
        "@com_google_absl_py//absl/testing:parameterized",
    ],
)

py_binary(
    name = "sapi_generator",
    srcs = ["sapi_generator.py"],
    python_version = "PY2",
    visibility = ["//visibility:public"],
    deps = [
        ":code",
        "@com_google_absl_py//absl:app",
    ],
)

cc_library(
    name = "tests",
    srcs = [
        "testdata/tests.cc",
        "testdata/tests.h",
        "testdata/tests2.cc",
    ],
    alwayslink = 1,
)

# Targets for testing if generated code compiles
sapi_library(
    name = "tests_sapi_generator",
    embed = True,
    input_files = [
        "testdata/tests.cc",
        "testdata/tests2.cc",
    ],
    lib = ":tests",
    lib_name = "Tests",
    namespace = "sapi::tests",
)

cc_binary(
    name = "build_test_bin",
    srcs = [
        "testdata/main.cc",
        ":tests_sapi_generator.sapi.h",
        ":tests_sapi_generator_embed.h",
    ],
    deps = [
        "//sandboxed_api:sapi",
        "//sandboxed_api:vars",
        "//sandboxed_api/sandbox2",
    ],
)

# Compilation test - always passes if dependency builds properly
sh_test(
    name = "build_test",
    srcs = ["build_test.sh"],
    data = [":build_test_bin"],
    tags = ["nobuilder"],
)
