# Copyright 2021 gRPC authors.
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

load("//bazel:grpc_build_system.bzl", "grpc_cc_library", "grpc_cc_proto_library", "grpc_cc_test", "grpc_internal_proto_library", "grpc_package")
load("//test/core/test_util:grpc_fuzzer.bzl", "grpc_fuzz_test")
load("//test/cpp/microbenchmarks:grpc_benchmark_config.bzl", "HISTORY", "grpc_cc_benchmark")

licenses(["notice"])

grpc_package(name = "test/core/promise")

grpc_cc_library(
    name = "poll_matcher",
    testonly = True,
    hdrs = ["poll_matcher.h"],
    external_deps = ["gtest"],
    visibility = [
        "//test/core:__subpackages__",
    ],
    deps = ["//src/core:poll"],
)

grpc_cc_library(
    name = "test_wakeup_schedulers",
    testonly = True,
    hdrs = ["test_wakeup_schedulers.h"],
    external_deps = ["gtest"],
    visibility = ["//test/core:__subpackages__"],
)

grpc_cc_library(
    name = "test_context",
    testonly = True,
    hdrs = ["test_context.h"],
    visibility = [
        "//test/core:__subpackages__",
        "//test/cpp:__subpackages__",
    ],
    deps = ["//src/core:context"],
)

grpc_cc_test(
    name = "poll_test",
    srcs = ["poll_test.cc"],
    external_deps = ["gtest"],
    uses_event_engine = False,
    uses_polling = False,
    deps = ["//src/core:poll"],
)

grpc_cc_test(
    name = "interceptor_list_test",
    srcs = ["interceptor_list_test.cc"],
    external_deps = ["gtest"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "test_context",
        "//:ref_counted_ptr",
        "//src/core:arena",
        "//src/core:event_engine_memory_allocator",
        "//src/core:interceptor_list",
        "//src/core:memory_quota",
        "//src/core:resource_quota",
    ],
)

grpc_cc_test(
    name = "context_test",
    srcs = ["context_test.cc"],
    external_deps = ["gtest"],
    uses_event_engine = False,
    uses_polling = False,
    deps = ["//src/core:context"],
)

grpc_cc_test(
    name = "cancel_callback_test",
    srcs = ["cancel_callback_test.cc"],
    external_deps = ["gtest"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "//src/core:cancel_callback",
        "//src/core:poll",
    ],
)

grpc_cc_test(
    name = "promise_test",
    srcs = ["promise_test.cc"],
    external_deps = ["gtest"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "//:promise",
        "//src/core:json_writer",
    ],
)

grpc_cc_test(
    name = "arena_promise_test",
    srcs = ["arena_promise_test.cc"],
    external_deps = ["gtest"],
    tags = [
        "promise_test",
        "resource_quota_test",
    ],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "test_context",
        "//:exec_ctx",
        "//:ref_counted_ptr",
        "//src/core:arena_promise",
        "//src/core:event_engine_memory_allocator",
        "//src/core:memory_quota",
        "//src/core:resource_quota",
        "//test/core/test_util:grpc_test_util_unsecure",
    ],
)

grpc_cc_test(
    # Why promise_map_test and not map_test?
    # third_party/benchmark defines a map_test in its cmakefile, and we depend
    # on that from our cmakefile, and cmake wants to have a flat namespace to
    # deal with xcode/visual studio limitations... sooo... promise_map_test it
    # is.
    name = "promise_map_test",
    srcs = ["map_test.cc"],
    external_deps = [
        "absl/functional:any_invocable",
        "absl/log",
        "gtest",
    ],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "poll_matcher",
        "//:promise",
        "//src/core:map",
    ],
)

grpc_cc_test(
    name = "match_promise_test",
    srcs = ["match_promise_test.cc"],
    external_deps = [
        "absl/functional:any_invocable",
        "gtest",
    ],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "poll_matcher",
        "//:promise",
        "//src/core:match_promise",
    ],
)

grpc_cc_test(
    name = "race_test",
    srcs = ["race_test.cc"],
    external_deps = ["gtest"],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "//src/core:poll",
        "//src/core:race",
    ],
)

grpc_cc_test(
    name = "prioritized_race_test",
    srcs = ["prioritized_race_test.cc"],
    external_deps = ["gtest"],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "//src/core:poll",
        "//src/core:prioritized_race",
    ],
)

grpc_cc_test(
    name = "promise_factory_test",
    srcs = ["promise_factory_test.cc"],
    external_deps = [
        "absl/functional:bind_front",
        "gtest",
    ],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "//src/core:poll",
        "//src/core:promise_factory",
    ],
)

grpc_cc_test(
    name = "if_test",
    srcs = ["if_test.cc"],
    external_deps = [
        "gtest",
        "absl/strings",
    ],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = ["//src/core:if"],
)

grpc_cc_test(
    name = "switch_test",
    srcs = ["switch_test.cc"],
    external_deps = ["gtest"],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "//src/core:poll",
        "//src/core:switch",
    ],
)

grpc_cc_test(
    name = "loop_test",
    srcs = ["loop_test.cc"],
    external_deps = [
        "gtest",
        "absl/status",
        "absl/strings",
    ],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "//src/core:loop",
        "//src/core:seq",
    ],
)

grpc_cc_test(
    name = "join_test",
    srcs = ["join_test.cc"],
    external_deps = ["gtest"],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "poll_matcher",
        "//src/core:join",
        "//src/core:poll",
        "//src/core:status_flag",
    ],
)

grpc_cc_test(
    name = "try_join_test",
    srcs = ["try_join_test.cc"],
    external_deps = [
        "absl/utility",
        "gtest",
    ],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = ["//src/core:try_join"],
)

grpc_cc_test(
    name = "all_ok_test",
    srcs = ["all_ok_test.cc"],
    external_deps = [
        "absl/utility",
        "gtest",
    ],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "//:promise",
        "//src/core:all_ok",
    ],
)

grpc_cc_test(
    name = "seq_test",
    srcs = ["seq_test.cc"],
    external_deps = ["gtest"],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "//src/core:json_writer",
        "//src/core:seq",
    ],
)

grpc_cc_test(
    name = "try_seq_test",
    srcs = ["try_seq_test.cc"],
    external_deps = ["gtest"],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "poll_matcher",
        "//src/core:json_writer",
        "//src/core:try_seq",
    ],
)

grpc_cc_test(
    name = "try_seq_metadata_test",
    srcs = ["try_seq_metadata_test.cc"],
    external_deps = ["gtest"],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "//:grpc",
        "//:grpc_public_hdrs",
        "//:ref_counted_ptr",
        "//src/core:arena",
        "//src/core:memory_quota",
        "//src/core:poll",
        "//src/core:resource_quota",
        "//src/core:try_seq",
    ],
)

grpc_cc_test(
    name = "activity_test",
    srcs = ["activity_test.cc"],
    external_deps = ["gtest"],
    flaky = True,
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "test_wakeup_schedulers",
        "//:promise",
        "//src/core:activity",
        "//src/core:join",
        "//src/core:poll",
        "//src/core:seq",
        "//src/core:wait_set",
    ],
)

grpc_cc_test(
    name = "latch_test",
    srcs = ["latch_test.cc"],
    external_deps = [
        "absl/status",
        "gtest",
    ],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "test_wakeup_schedulers",
        "//src/core:activity",
        "//src/core:join",
        "//src/core:latch",
        "//src/core:seq",
    ],
)

grpc_cc_test(
    name = "for_each_test",
    srcs = ["for_each_test.cc"],
    external_deps = ["gtest"],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "test_wakeup_schedulers",
        "//:ref_counted_ptr",
        "//src/core:activity",
        "//src/core:arena",
        "//src/core:event_engine_memory_allocator",
        "//src/core:for_each",
        "//src/core:inter_activity_pipe",
        "//src/core:join",
        "//src/core:map",
        "//src/core:memory_quota",
        "//src/core:pipe",
        "//src/core:resource_quota",
        "//src/core:seq",
        "//src/core:try_seq",
    ],
)

grpc_cc_test(
    name = "map_pipe_test",
    srcs = ["map_pipe_test.cc"],
    external_deps = ["gtest"],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "test_wakeup_schedulers",
        "//:ref_counted_ptr",
        "//src/core:activity",
        "//src/core:arena",
        "//src/core:event_engine_memory_allocator",
        "//src/core:for_each",
        "//src/core:join",
        "//src/core:map",
        "//src/core:map_pipe",
        "//src/core:memory_quota",
        "//src/core:pipe",
        "//src/core:poll",
        "//src/core:resource_quota",
        "//src/core:seq",
    ],
)

grpc_cc_test(
    name = "pipe_test",
    srcs = ["pipe_test.cc"],
    external_deps = [
        "absl/functional:function_ref",
        "absl/status",
        "gtest",
    ],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "test_wakeup_schedulers",
        "//:gpr",
        "//:grpc",
        "//:ref_counted_ptr",
        "//src/core:activity",
        "//src/core:event_engine_memory_allocator",
        "//src/core:join",
        "//src/core:map",
        "//src/core:memory_quota",
        "//src/core:pipe",
        "//src/core:resource_quota",
        "//src/core:seq",
    ],
)

grpc_cc_test(
    name = "inter_activity_pipe_test",
    srcs = ["inter_activity_pipe_test.cc"],
    external_deps = [
        "absl/status",
        "gtest",
    ],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "test_wakeup_schedulers",
        "//src/core:inter_activity_pipe",
        "//src/core:seq",
    ],
)

grpc_cc_test(
    name = "inter_activity_latch_test",
    srcs = ["inter_activity_latch_test.cc"],
    external_deps = [
        "absl/status",
        "gtest",
    ],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "//:grpc",
        "//src/core:default_event_engine",
        "//src/core:event_engine_wakeup_scheduler",
        "//src/core:inter_activity_latch",
        "//src/core:notification",
        "//src/core:seq",
    ],
)

grpc_fuzz_test(
    name = "mpsc_test",
    srcs = ["mpsc_test.cc"],
    external_deps = [
        "gtest",
        "fuzztest",
        "fuzztest_main",
    ],
    tags = ["promise_test"],
    deps = [
        "poll_matcher",
        "//:gpr",
        "//:promise",
        "//src/core:activity",
        "//src/core:mpsc",
    ],
)

grpc_cc_test(
    name = "lock_based_mpsc_test",
    srcs = ["lock_based_mpsc_test.cc"],
    external_deps = [
        "gtest",
    ],
    tags = ["promise_test"],
    deps = [
        "poll_matcher",
        "//:gpr",
        "//:promise",
        "//src/core:activity",
        "//src/core:lock_based_mpsc",
    ],
)

grpc_cc_test(
    name = "observable_test",
    srcs = ["observable_test.cc"],
    external_deps = [
        "absl/strings",
        "gtest",
    ],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "poll_matcher",
        "//src/core:loop",
        "//src/core:map",
        "//src/core:notification",
        "//src/core:observable",
    ],
)

grpc_internal_proto_library(
    name = "promise_fuzzer_proto",
    srcs = ["promise_fuzzer.proto"],
)

grpc_cc_proto_library(
    name = "promise_fuzzer_cc_proto",
    deps = ["promise_fuzzer_proto"],
)

grpc_fuzz_test(
    name = "promise_fuzzer",
    srcs = ["promise_fuzzer.cc"],
    external_deps = [
        "absl/functional:any_invocable",
        "absl/log:check",
        "absl/status",
        "fuzztest",
        "fuzztest_main",
    ],
    tags = ["no_windows"],
    deps = [
        "promise_fuzzer_cc_proto",
        "//:gpr",
        "//:promise",
        "//src/core:activity",
        "//src/core:join",
        "//src/core:map",
        "//src/core:poll",
        "//src/core:race",
        "//src/core:seq",
        "//test/core/test_util:grpc_test_util",
    ],
)

grpc_cc_test(
    name = "exec_ctx_wakeup_scheduler_test",
    srcs = ["exec_ctx_wakeup_scheduler_test.cc"],
    external_deps = [
        "absl/status",
        "gtest",
    ],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "//src/core:activity",
        "//src/core:exec_ctx_wakeup_scheduler",
        "//src/core:poll",
    ],
)

grpc_cc_test(
    name = "wait_for_callback_test",
    srcs = ["wait_for_callback_test.cc"],
    external_deps = [
        "absl/status",
        "gtest",
    ],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "test_wakeup_schedulers",
        "//src/core:map",
        "//src/core:notification",
        "//src/core:wait_for_callback",
    ],
)

grpc_cc_test(
    name = "event_engine_wakeup_scheduler_test",
    srcs = ["event_engine_wakeup_scheduler_test.cc"],
    external_deps = [
        "absl/status",
        "gtest",
    ],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "//:grpc",
        "//src/core:activity",
        "//src/core:event_engine_wakeup_scheduler",
        "//src/core:notification",
        "//src/core:poll",
    ],
)

grpc_cc_test(
    name = "sleep_test",
    srcs = ["sleep_test.cc"],
    external_deps = [
        "absl/log",
        "gtest",
    ],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "test_wakeup_schedulers",
        "//:exec_ctx",
        "//:grpc",
        "//:orphanable",
        "//src/core:arena",
        "//src/core:default_event_engine",
        "//src/core:event_engine_context",
        "//src/core:exec_ctx_wakeup_scheduler",
        "//src/core:notification",
        "//src/core:race",
        "//src/core:sleep",
        "//test/core/event_engine:mock_event_engine",
    ],
)

grpc_cc_test(
    name = "status_flag_test",
    srcs = ["status_flag_test.cc"],
    external_deps = ["gtest"],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = ["//src/core:status_flag"],
)

grpc_cc_test(
    name = "promise_mutex_test",
    srcs = ["promise_mutex_test.cc"],
    external_deps = [
        "absl/status",
        "gtest",
    ],
    deps = [
        "test_wakeup_schedulers",
        "//:promise",
        "//src/core:activity",
        "//src/core:join",
        "//src/core:promise_mutex",
        "//src/core:seq",
    ],
)

grpc_cc_test(
    name = "party_test",
    srcs = ["party_test.cc"],
    external_deps = [
        "absl/base:core_headers",
        "absl/log:log",
        "gtest",
    ],
    tags = ["promise_test"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "//:exec_ctx",
        "//:gpr",
        "//:grpc_unsecure",
        "//:ref_counted_ptr",
        "//src/core:1999",
        "//src/core:context",
        "//src/core:default_event_engine",
        "//src/core:event_engine_memory_allocator",
        "//src/core:inter_activity_latch",
        "//src/core:memory_quota",
        "//src/core:notification",
        "//src/core:poll",
        "//src/core:resource_quota",
        "//src/core:seq",
        "//src/core:sleep",
        "//src/core:time",
    ],
)

grpc_cc_test(
    name = "party_mpsc_test",
    srcs = ["party_mpsc_test.cc"],
    external_deps = [
        "absl/base:core_headers",
        "absl/log:log",
        "absl/strings",
        "gtest",
    ],
    tags = [
        "no_windows",
        "party_mpsc_test",
    ],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "poll_matcher",
        "//:exec_ctx",
        "//:gpr",
        "//:grpc_unsecure",
        "//:ref_counted_ptr",
        "//src/core:1999",
        "//src/core:context",
        "//src/core:default_event_engine",
        "//src/core:event_engine_memory_allocator",
        "//src/core:memory_quota",
        "//src/core:mpsc",
        "//src/core:notification",
        "//src/core:poll",
        "//src/core:resource_quota",
        "//src/core:seq",
        "//src/core:sleep",
        "//src/core:time",
    ],
)

grpc_internal_proto_library(
    name = "inter_activity_mutex_test_proto",
    srcs = ["inter_activity_mutex_test.proto"],
)

grpc_cc_proto_library(
    name = "inter_activity_mutex_test_cc_proto",
    deps = ["inter_activity_mutex_test_proto"],
)

grpc_fuzz_test(
    name = "inter_activity_mutex_test",
    srcs = ["inter_activity_mutex_test.cc"],
    external_deps = [
        "gtest",
        "fuzztest",
        "fuzztest_main",
    ],
    tags = ["promise_test"],
    deps = [
        "inter_activity_mutex_test_cc_proto",
        "poll_matcher",
        "//:grpc",
        "//src/core:1999",
        "//src/core:inter_activity_mutex",
        "//src/core:notification",
        "//src/core:resource_quota",
        "//src/core:seq",
        "//src/core:time",
    ],
)

grpc_cc_benchmark(
    name = "bm_party",
    srcs = ["bm_party.cc"],
    monitoring = HISTORY,
    deps = [
        "//:grpc",
        "//src/core:1999",
        "//src/core:default_event_engine",
    ],
)
