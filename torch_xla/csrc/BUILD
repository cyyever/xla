load(
    "//bazel:rules_def.bzl",
    "ptxla_cc_library",
)

genrule(
    name = "gen_lazy_tensor",
    srcs = ["//codegen:xla_native_functions.yaml"],
    outs = [
        "LazyIr.h",
        "LazyNonNativeIr.h",
        "RegisterAutogradXLA.cpp",
        "RegisterXLA.cpp",
        "XLANativeFunctions.cpp",
        "XLANativeFunctions.h",
    ],
    cmd = ";".join([
        "$(location //codegen:lazy_tensor_generator) $(location //codegen:lazy_tensor_generator).runfiles $< $(RULEDIR)",
        "$(location //codegen:fix_includes) $(OUTS)",
    ]),
    tags = [
        "local",
        "no-remote-exec",
    ],
    tools = [
        "//codegen:fix_includes",
        "//codegen:lazy_tensor_generator",
    ],
)

ptxla_cc_library(
    name = "tensor",
    srcs = [
        "aten_xla_type.cpp",
        ":RegisterAutogradXLA.cpp",
        "nms_op.cpp",
        "xla_op_builder.cpp",
        ":RegisterXLA.cpp",
        ":XLANativeFunctions.cpp",
        "aten_xla_bridge.cpp",
        "convert_ops.cpp",
        "convolution.cpp",
        "cross_replica_reduces.cpp",
        "data_ops.cpp",
        "debug_util.cpp",
        "elementwise.cpp",
        "helpers.cpp",
        "aten_autograd_ops.cpp",
        "ir.cpp",
        "ir_dump_util.cpp",
        "ir_util.cpp",
        "lowering_context.cpp",
        "matrix.cpp",
        "nll_loss.cpp",
        "op_by_op_executor.cpp",
        "pooling.cpp",
        "random.cpp",
        "reduction.cpp",
        "tensor.cpp",
        "tensor_methods.cpp",
        "tensor_ops.cpp",
        "xla_graph_executor.cpp",
        "tensor_impl.cpp",
        "tensor_util.cpp",
        "token_handler.cpp",
        "torch_util.cpp",
        "view.cpp",
        "xla_backend_impl.cpp",
        "xla_lower_util.cpp",
        "xla_sharding_util.cpp",
        "softmax_builder.cpp",
        "batch_norm.cpp",
        "resize_ops.cpp",
    ] + glob(["ops/*.cpp"]),
    hdrs = [
        ":LazyIr.h",
        "tensor_methods.h",
        "tensor_ops.h",
        ":LazyNonNativeIr.h",
        ":XLANativeFunctions.h",
        "xla_op_builder.h",
        "resize_ops.h",
        "nms_op.h",
        "aten_xla_bridge.h",
        "convert_ops.h",
        "convolution.h",
        "data_ops.h",
        "cross_replica_reduces.h",
        "softmax_builder.h",
        "debug_util.h",
        "elementwise.h",
        "generated_file_include.h",
        "helpers.h",
        "aten_autograd_ops.h",
        "xla_graph_executor.h",
        "ir.h",
        "ir_dump_util.h",
        "ir_util.h",
        "lowering_context.h",
        "matrix.h",
        "nll_loss.h",
        "op_by_op_executor.h",
        "batch_norm.h",
        "pooling.h",
        "random.h",
        "reduction.h",
        "tensor.h",
        "tensor_impl.h",
        "tensor_util.h",
        "token_handler.h",
        "torch_util.h",
        "view.h",
        "xla_backend_impl.h",
        "xla_lower_util.h",
        "xla_sharding_util.h",
    ] + glob(["ops/*.h"]),
    deps = [
        ":aten_cpu_fallback",
        ":computation",
        ":device",
        ":einsum_utilities",
        ":ir_builder",
        ":layout_manager",
        ":shape_builder",
        "//third_party/xla_client:async_task",
        "//third_party/xla_client:computation_client",
        "@com_google_absl//absl/hash",
        "@com_google_absl//absl/memory",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:optional",
        "@com_google_absl//absl/types:span",
        "@org_tensorflow//tensorflow/compiler/tf2xla/kernels:conv_op_helpers",
        "@org_tensorflow//tensorflow/compiler/xla:comparison_util",
        "@org_tensorflow//tensorflow/compiler/xla:literal_util",
        "@org_tensorflow//tensorflow/compiler/xla:permutation_util",
        "@org_tensorflow//tensorflow/compiler/xla:shape_util",
        "@org_tensorflow//tensorflow/compiler/xla:types",
        "@org_tensorflow//tensorflow/compiler/xla/client:xla_builder",
        "@org_tensorflow//tensorflow/compiler/xla/client/lib:arithmetic",
        "@org_tensorflow//tensorflow/compiler/xla/client/lib:comparators",
        "@org_tensorflow//tensorflow/compiler/xla/client/lib:constants",
        "@org_tensorflow//tensorflow/compiler/xla/client/lib:logdet",
        "@org_tensorflow//tensorflow/compiler/xla/client/lib:math",
        "@org_tensorflow//tensorflow/compiler/xla/client/lib:slicing",
        "@org_tensorflow//tensorflow/compiler/xla/stream_executor:dnn",
        "@org_tensorflow//tensorflow/core:lib",
        "@org_tensorflow//tensorflow/core/framework:tensor_shape",
        "@org_tensorflow//tensorflow/core/kernels:conv_ops",
        "@org_tensorflow//tensorflow/core/lib/core:errors",
        "@org_tensorflow//tensorflow/core/lib/gtl:inlined_vector",
        "@org_tensorflow//tensorflow/core/profiler/lib:traceme",
        "@org_tensorflow//third_party/python_runtime:headers",
        "@pybind11//:pybind11_embed",  # libpyton
    ],
)

ptxla_cc_library(
    name = "device",
    srcs = ["device.cpp"],
    hdrs = ["device.h"],
    deps = [
        "//third_party/xla_client:computation_client",
        "//third_party/xla_client:util",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:optional",
        "@org_tensorflow//tensorflow/compiler/xla/client:xla_builder",
    ],
)

ptxla_cc_library(
    name = "computation",
    srcs = ["computation.cpp"],
    hdrs = ["computation.h"],
    deps = [
        "//third_party/xla_client:computation_client",
        "@org_tensorflow//tensorflow/compiler/xla/client:xla_builder",
        "@org_tensorflow//tensorflow/compiler/xla/client:xla_computation",
    ],
)

ptxla_cc_library(
    name = "layout_manager",
    srcs = ["layout_manager.cpp"],
    hdrs = ["layout_manager.h"],
    deps = [
        ":device",
        "//third_party/xla_client:debug_macros",
        "//third_party/xla_client:sys_util",
        "//third_party/xla_client:tf_logging",
        "//third_party/xla_client:util",
        "@com_google_absl//absl/strings",
        "@org_tensorflow//tensorflow/compiler/xla:shape_util",
        "@org_tensorflow//tensorflow/core/framework:tensor_shape",
    ],
)

cc_library(
    name = "einsum_utilities",
    hdrs = ["ops/einsum_utilities.h"],
    deps = [
        "//third_party/xla_client:debug_macros",
    ],
)

ptxla_cc_library(
    name = "aten_cpu_fallback",
    srcs = ["aten_cpu_fallback.cpp"],
    hdrs = ["aten_cpu_fallback.h"],
    deps = [
        ":function_call_tracker",
        "//third_party/xla_client:debug_macros",
        "//third_party/xla_client:metrics",
        "//third_party/xla_client:tf_logging",
    ],
)

ptxla_cc_library(
    name = "function_call_tracker",
    srcs = ["function_call_tracker.cpp"],
    hdrs = ["function_call_tracker.h"],
    deps = [
        "//third_party/xla_client:sys_util",
        "@com_google_absl//absl/strings",
        "@org_tensorflow//tensorflow/core/platform:stacktrace",
    ],
)

ptxla_cc_library(
    name = "ir_builder",
    hdrs = ["ir_builder.h"],
    deps = [
        "@com_google_absl//absl/types:span",
        "@org_tensorflow//tensorflow/compiler/xla/client:xla_builder",
    ],
)

ptxla_cc_library(
    name = "shape_builder",
    srcs = ["shape_builder.cpp"],
    hdrs = ["shape_builder.h"],
    deps = [
        "@com_google_absl//absl/types:span",
        "@org_tensorflow//tensorflow/compiler/xla:shape_util",
        "@org_tensorflow//tensorflow/compiler/xla:types",
    ],
)

cc_library(
    name = "version",
    srcs = ["version.cpp"],
    hdrs = ["version.h"],
)

ptxla_cc_library(
    name = "init_python_bindings",
    srcs = ["init_python_bindings.cpp"],
    deps = [
        ":computation",
        ":device",
        ":tensor",
        ":version",
        "//third_party/xla_client:computation_client",
        "//third_party/xla_client:mesh_service",
        "//third_party/xla_client:metrics",
        "//third_party/xla_client:metrics_analysis",
        "//third_party/xla_client:metrics_reader",
        "//third_party/xla_client:multi_wait",
        "//third_party/xla_client:profiler",
        "//third_party/xla_client:record_reader",
        "//third_party/xla_client:sys_util",
        "//third_party/xla_client:thread_pool",
        "//third_party/xla_client:util",
        "//third_party/xla_client:xla_util",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:variant",
        "@org_tensorflow//tensorflow/compiler/xla/python/profiler/internal:traceme_wrapper",
        "@org_tensorflow//tensorflow/compiler/xla/service:hlo_parser",
        "@org_tensorflow//tensorflow/compiler/xla/service:hlo_pass_pipeline",
        "@org_tensorflow//tensorflow/compiler/xla/service:hlo_verifier",
        "@org_tensorflow//tensorflow/compiler/xla/service:sharding_propagation",
        "@org_tensorflow//tensorflow/compiler/xla/service/spmd:spmd_partitioner",
        "@org_tensorflow//tensorflow/core",
        "@org_tensorflow//tensorflow/core:protos_all_cc",
        "@org_tensorflow//tensorflow/core/platform:env",
        "@org_tensorflow//tensorflow/core/profiler/lib:traceme",
        "@org_tensorflow//tensorflow/python/profiler/internal:profiler_pywrap_impl",
    ],
)