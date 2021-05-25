package(default_visibility = ["//visibility:public"])

load(
    "@envoy//bazel:envoy_build_system.bzl",
    "envoy_cc_binary",
)

cc_library(
    name = "libwallarm",
    linkopts = ["-L/source -Wl,--whole-archive -lwallarm -Wl,--no-whole-archive -lwallarmmisc"],
)

envoy_cc_binary(
    name = "envoy_wallarm",
    repository = "@envoy",
    deps = [
        ":libwallarm",
        "@envoy//source/exe:envoy_main_entry_lib",
    ],
)
