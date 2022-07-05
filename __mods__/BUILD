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