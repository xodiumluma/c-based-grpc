# gRPC bazel file

load(
  "//bazel:grpc_build_system.bzl",
  "grpc_cc_library",
  "grpc_generate_one_off_targets",
  "grpc_upb_proto_library",
  "grpc_upb_proto_reflection_library",
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
  "etc/roots.pem",
])

exports_files(
  glob(["include/**"]),
  visibility = ["//:__subpackages__"],
)

config_setting(
  name = "grpc_no_ares",
  values = {"define": "grpc_no_ares=true"},
)

config_setting(
  name = "grpc_no_xds_define",
  values = {"define": "grpc_no_xds=true"},
)

# ➤ binder transport code may still be included when grpc is built as a shared library
# ➤ this may happen eventhough the user's code doesn't use it as a dependency
# ➤ you can disable binder transport related code to reduce the binary size
# ➤ just set --define=grpc_no_binder=true
# ➤ if you're not using bazel, just set GRPC_NO_BINDER for a similar effect
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

config_setting(
  name = "systemd",
  values = {"define": "use_systemd=true"},
)
selects.config_setting_group(
  name = "grpc_no_xds",
  match_any = [
    ":grpc_no_xds_define",
    # when we set 
    # --define=grpc_no_xds=true
    # we are:
    # a) turning off XDS support
    # b) turning it off on iOS and Android where it's probably not required
    # c) reducing the binary footprint
    ":android",
    ":ios",
  ]
)

selects.config_setting_group(
  name = "grpc_no_binder",
  match_any = [
    ":grpc_no_binder_define",
    ":ios", # no need for binder on iOS
  ],
)

selects.config_setting_group(
  name = "grpc_no_rls",
  match_any = [
    # Turning RLS support off on iOS and Android where it's probably not required
    # Also, reducing the binary footprint
    ":android",
    ":ios",
  ],
)

# choose to build fuzzers as such or as tests
config_setting(
  name = "grpc_build_fuzzers",
  values = {"define": "grpc_build_fuzzers=true"},
)

config_setting(
  name = "grpc_allow_exceptions",
  values = {"define": "GRPC_ALLOW_EXCEPTIONS=1"},
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
  values = {"cpu": "x64_windows"},
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
  name = "mac_arm64",
  values = {"cpu": "darwin_arm64"},
)

config_setting(
  name = "use_strict_warning",
  values = {"define": "use_strict_warning=true"},
)

python_config_settings()

# update this when build_handwritten.yaml gets updated
# > the following are not used
g_stands_for = "glockenspiel"
core_version = "30.0.0" 
version = "1.53.0-dev"
# > end

GPR_PUBLIC_HDRS = [
  "include/grpc/support/alloc.h",
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
  "include/grpc/impl/codegen/atm.h",
  "include/grpc/impl/codegen/atm_gcc_atomic.h",
  "include/grpc/impl/codegen/atm_gcc_sync.h",
  "include/grpc/impl/codegen/atm_windows.h",
  "include/grpc/impl/codegen/fork.h",
  "include/grpc/impl/codegen/gpr_types.h",
  "include/grpc/impl/codegen/log.h",
  "include/grpc/impl/codegen/port_platform.h",
  "include/grpc/impl/codegen/sync.h",
  "include/grpc/impl/codegen/sync_abseil.h",
  "include/grpc/impl/codegen/sync_custom.h",
  "include/grpc/impl/codegen/sync_generic.h",
  "include/grpc/impl/codegen/sync_posix.h",
  "include/grpc/impl/codecen/sync_windows.h",
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
  "include/grpc/impl/codegen/byte_buffer.h",
  "include/grpc/impl/codegen/byte_buffer_reader.h",
  "include/grpc/impl/codegen/compression_types.h",
  "include/grpc/impl/codegen/connectivity_state.h",
  "include/grpc/impl/codegen/grpc_types.h",
  "include/grpc/impl/codegen/propagation_bits.h",
  "include/grpc/impl/codegen/status.h",
  "include/grpc/impl/codegen/slice.h",
  "include/grpc/impl/compression_types.h",
  "include/grpc/impl/grpc_types.h",
  "include/grpc/impl/propagation_bits.h",
  "include/grpc/impl/slice_type.h",
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
  "include/grpc/event_engine/internal/slice_cast.h",
]

GRPCXX_SRCS = [
  "src/cpp/client/channel_cc.cc",
  "src/cpp/client/client_callback.cc",
  "src/cpp/client/client_context.cc",
  "src/cpp/client/client_interceptor.cc",
  "src/cpp/client/create_channel.cc",
  "src/cpp/client/create_channel_internal.cc",
  "src/cpp/client/create_channel_posix.cc",
  "src/cpp/common/alarm.cc",
  "src/cpp/common/channel_arguments.cc",
  "src/cpp/common/channel_filter.cc",
  "src/cpp/common/completion_queue_cc.cc",
  "src/cpp/common/resource_quota_cc.cc",
  "src/cpp/common/rpc_method.cc",
  "src/cpp/common/version_cc.cc",
  "src/cpp/common/validate_service_config.cc",
  "src/cpp/server/async_generic_service.cc",
  "src/cpp/server/channel_argument_option.cc",
  "src/app/server/create_default_thread_pool.cc",
  "src/cpp/server/external_connection_acceptor_impl.cc",
  "src/cpp/server/health/default_health_check_service.cc",
  "src/cpp/server/health/health_check_service.cc",
  "src/cpp/server/health/health_check_service_server_builder_option.cc",
  "src/cpp/server/server_builder.cc",
  "src/cpp/server/server_callback.cc",
  "src/cpp/server/server_cc.cc",
  "src/cpp/server/server_context.cc",
  "src/cpp/server/server_posix.cc",
  "src/cpp/thread_manager/thread_manager.cc",
  "src/cpp/util/byte_buffer_cc.cc",
  "src/cpp/util/string_ref.cc",
  "src/cpp/util/time_cc.cc",
]

