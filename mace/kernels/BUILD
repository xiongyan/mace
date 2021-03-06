# Description:
# Mace neon kernels.
#
package(
    default_visibility = ["//visibility:public"],
)

licenses(["notice"])  # Apache 2.0

load(
    "//mace:mace.bzl",
    "if_android",
    "if_neon_enabled",
    "if_openmp_enabled",
    "if_android_armv7",
    "if_hexagon_enabled",
    "if_opencl_enabled",
)

cc_library(
    name = "kernels",
    srcs = glob(
        [
            "*.cc",
            "arm/*.cc",
        ],
        exclude = [
            "*_test.cc",
            "*_benchmark.cc",
            "arm/*_test.cc",
            "buffer_inverse_transform.cc",
            "buffer_transform.cc",
            "lstm_cell.cc",
            "winograd_transform.cc",
        ],
    ) + if_opencl_enabled(glob(
        [
            "opencl/*.cc",
            "opencl/image/*.cc",
            "opencl/buffer/*.cc",
            "buffer_inverse_transform.cc",
            "buffer_transform.cc",
            "lstm_cell.cc",
            "winograd_transform.cc",
        ],
        exclude = [
            "opencl/*_test.cc",
        ],
    )),
    hdrs = glob(
        [
            "*.h",
            "arm/*.h",
        ],
    ) + if_opencl_enabled(glob([
        "opencl/*.h",
        "opencl/image/*.h",
        "opencl/buffer/*.h",
    ])),
    copts = [
        "-Werror",
        "-Wextra",
        "-Wno-missing-field-initializers",
    ] + if_openmp_enabled([
        "-fopenmp",
    ]) + if_neon_enabled([
        "-DMACE_ENABLE_NEON",
    ]) + if_android_armv7([
        "-mfpu=neon",
    ]) + if_android_armv7([
        "-mfloat-abi=softfp",
    ]) + if_opencl_enabled([
        "-DMACE_ENABLE_OPENCL",
    ]) + if_hexagon_enabled([
        "-DMACE_ENABLE_HEXAGON",
    ]),
    linkopts = if_android(["-lm"]),
    deps = [
        "//mace/core",
        "@gemmlowp",
        "@tflite",
    ],
)

cc_test(
    name = "kernels_test",
    testonly = 1,
    srcs = glob(
        [
            "*_test.cc",
            "arm/*_test.cc",
            "opencl/*_test.cc",
        ],
    ),
    copts = [
        "-Werror",
        "-Wextra",
        "-Wno-missing-field-initializers",
    ] + if_openmp_enabled([
        "-fopenmp",
    ]) + if_neon_enabled([
        "-DMACE_ENABLE_NEON",
    ]) + if_android_armv7([
        "-mfpu=neon",
        "-mfloat-abi=softfp",
    ]) + if_opencl_enabled([
        "-DMACE_ENABLE_OPENCL",
    ]) + if_hexagon_enabled([
        "-DMACE_ENABLE_HEXAGON",
    ]),
    linkopts = ["-fopenmp"],
    linkstatic = 1,
    deps = [
        ":kernels",
        "//mace/ops",
        "@gtest",
        "@gtest//:gtest_main",
    ],
)

cc_test(
    name = "kernels_benchmark",
    testonly = 1,
    srcs = glob(["*_benchmark.cc"]),
    copts = [
        "-Werror",
        "-Wextra",
        "-Wno-missing-field-initializers",
    ] + if_openmp_enabled([
        "-fopenmp",
    ]) + if_neon_enabled([
        "-DMACE_ENABLE_NEON",
    ]) + if_android_armv7([
        "-mfpu=neon",
        "-mfloat-abi=softfp",
    ]) + if_opencl_enabled([
        "-DMACE_ENABLE_OPENCL",
    ]) + if_hexagon_enabled([
        "-DMACE_ENABLE_HEXAGON",
    ]),
    linkopts = ["-fopenmp"],
    linkstatic = 1,
    deps = [
        ":kernels",
        "//mace/core:test_benchmark_main",
        "//mace/ops",
        "//third_party/eigen3",
        "@gemmlowp",
    ],
)
