load(
  "//bazel:grpc_build_system.bzl",
  "grpc_cc_library",
  "grpc_generate_one_off_targets",
  "grpc_upb_proto_library",
  "grpc_upg_proto_reflection_library",
  "python_config_settings",
)
load("@bazel_skylib//lib:selects.bzl", "selects")

licenses(["reciprocal"])

package(
  default_visibility = ["//visibility:public"],
  features = [
    "layering_check",
    "-parse_headers",
  ],
)

exports_files([
  "LICENSE",
  "etc/roots.pem"
])

config_setting(
  name = "grpc_no_ares",
  values = {"define": "grpc_no_ares=true"}
)

config_setting(
  name = "grpc_no_xds_define",
  values = {"define": "grpc_no_xds=true"},
)

# in the case that gRPC is built as a shared library, even when the user does not depend on it, the distribution might still include binder transport code 
# you can keep the file size down by specifying (omit the binder transport):
# --define=grpc_no_binder=true 
# If you are not using bazel, you can also set GRPC_NO_BINDER for the same results
config_setting(
  name = "grpc_no_binder_define",
  values = {"define": "grpc_no_binder=true"},
)

config_setting(
  name = "android",
  values = {"crosstool_top": "//external:android/crosstool"},
)

config_setting(
  name = "ios",
  values = {"apple_platform_type": "ios"},
)

selects.config_setting_group(
  name = "grpc_no_xds",
  match_any = [
    ":grpc_no_xds_define",
    # when we turn off XDS support via --define=grpc_no_xds=true, we also turn it off on mobile OSes where the filesize is important and where it probably won't be used
    ":android", ":ios",
  ],
)

selects.config_setting_group(
  name = "grpc_no_binder",
  match_any = [
    ":grpc_no_binder_define",
    # iOS doesn't need binder
    ":ios",
  ],
)

selects.config_setting_group(
  name = "grpc_no_rls",
  match_any = [
    # we don't need this on mobile OSes and we want to keep filesize down
    ":android", ":ios",
  ],
)

# Fuzzers can be built as fuzzers or tests
config_setting(
  name = "grpc_build_fuzzers",
  values = {"define": "grpc_build_fuzzers=true"},
)

config_setting(
  name = "grpc_allow_exceptions",
  values = {"define": "GRPC_ALLOW_EXCEPTIONS=1"}
)

config_setting(
  name = "grpc_disallow_exceptions",
  values = {"define": "GRPC_ALLOW_EXCEPTIONS=0"},
)

config_setting(
  name = "remote_execution",
  values = {"define": "GRPC_PORT_ISOLATED_RUNTIME=1"},
)

config_setting(
  name = "windows",
  values = {"cpu": "x64_windows"}
)

config_setting(
  name = "windows_msvc",
  values = {"cpu": "x64_windows_msvc"},
)

config_setting(
  name = "mac_x86_64",
  values = {"cpu": "darwin"},
)

config_setting(
  name = "use_stric_warning",
  values = {"define": "use_strict_warning=true"},
)

config_setting(
  name = "disable_use_abseil_status",
  values = {"define": "use_abseil_status=false"},
)

python_config_settings()

# please update this alongside build_handwritten.yaml
g_stands_for = "gamma"      # @unused
core_version = "26.0.0"     # @unused
version      = "1.49.0-dev" # @unused

GPR_PUBLIC_HDRS = [
  "include/grpc/support/alloc.h",
  "include/grpc/support/atm.h",
  "include/grpc/support/atm_gcc_atomic.h",
  "include/grpc/support/atm_gcc_sync.h",
  "include/grpc/support/atm_windows.h",
  "include/grpc/support/cpu.h",
  "include/grpc/support/log.h",
  "include/grpc/support/log_windows.h",
  "include/grpc/support/port_platform.h",
  "include/grpc/support/string_util.h",
  "include/grpc/support/sync.h",
  "include/grpc/support/sync_abseil.h",
  "include/grpc/support/sync_custom.h",
  "include/grpc/support/sync_generic.h",
  "include/grpc/support/sync_posix.h",
  "include/grpc/support/sync_windows.h",
  "include/grpc/support/thd_id.h",
  "include/grpc/support/time.h",
]

GRPC_PUBLIC_HDRS = [
  "include/grpc/byte_buffer.h",
  "include/grpc/byte_buffer_reader.h",
  "include/grpc/compression.h",
  "include/grpc/fork.h",
  "include/grpc/grpc.h",
  "include/grpc/grpc_posix.h",
  "include/grpc/grpc_security.h",
  "include/grpc/grpc_security_constants.h",
  "include/grpc/slice.h",
  "include/grpc/slice_buffer.h",
  "include/grpc/status.h",
  "include/grpc/load_reporting.h",
  "include/grpc/support/workaround_list.h",
]

GRPC_PUBLIC_EVENT_ENGINE_HDRS = [
  "include/grpc/byte_buffer.h",
  "include/grpc/byte_buffer_reader.h",
  "include/grpc/compression.h",
  "include/grpc/fork.h",
  "include/grpc/grpc.h",
  "include/grpc/grpc_posix.h",
  "include/grpc/grpc_security.h",
  "include/grpc/grpc_security_constants.h",
  "include/grpc/slice.h",
  "include/grpc/slice_buffer.h",
  "include/grpc/status.h",
  "include/grpc/load_reporting.h",
  "include/grpc/support/workaround_list.h",
]

GRPC_PUBLIC_EVENT_ENGINE_HDRS = [
  "include/grpc/event_engine/endpoint_config.h",
  "include/grpc/event_engine/event_engine.h",
  "include/grpc/event_engine/port.h",
  "include/grpc/event_engine/memory_allocator.h",
  "include/grpc/event_engine/memory_request.h",
  "include/grpc/event_engine/internal/memory_allocator_impl.h",
  "include/grpc/event_engine/slice.h",
  "include/grpc/event_engine/slice_buffer.h",
]

GRPCXX_SRCS = [
  "src/cpp/client/channel_cc.cc",
  "src/cpp/client/client_callback.cc",
  "src/cpp/client/client_context.cc",
  "src/cpp/client/client_interceptor.cc",
  "src/cpp/client/create_channel.cc",
  "src/cpp/client/create_channel_internal.cc",
  "src/cpp/client/create_channel_posix.cc",
  "src/cpp/client/credentials_cc.cc",
  "src/cpp/common/alarm.cc",
  "src/cpp/common/channel_arguments.cc",
  "src/cpp/common/channel_filter.cc",
  "src/cpp/common/completion_queue_cc.cc",
  "src/cpp/common/core_codegen.cc",
  "src/cpp/common/resource_quota_cc.cc",
  "src/cpp/common/rpc_method.cc",
  "src/cpp/common/version_cc.cc",
  "src/cpp/common/validate_service_config.cc",
  "src/cpp/server/async_generic_service.cc",
  "src/cpp/server/channel_argument_option.cc",
  "src/cpp/server/create_default_thread_pool.cc",
  "src/cpp/server/dynamic_thread_pool.cc",
  "src/cpp/server/health/default_health_check_service.cc",
  "src/cpp/server/health/health_check_service.cc",
  "src/cpp/server/health/health_check_service_server_builder_option.cc",
  "src/cpp/server/server_builder.cc",
  "src/cpp/server/server_callback.cc",
  "src/cpp/server/server_cc.cc",
  "src/cpp/server/server_context.cc",
  "src/cpp/server/server_credentials.cc",
  "src/cpp/server/server_posix.cc",
  "src/cpp/thread_manager/thread_manager.cc",
  "src/cpp/util/byte_buffer_cc.cc",
  "src/cpp/util/status.cc",
  "src/cpp/util/string_ref.cc",
  "src/cpp/util/time_cc.cc",
]