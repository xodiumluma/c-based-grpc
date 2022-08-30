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
    "-parse_headers"
  ]
)

exports_files([
  "LICENSE",
  "etc/roots.pem",
])

config_setting(
  name = "grpc_no_ares",
  values = {"define": "grpc_no_ares=true"}
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