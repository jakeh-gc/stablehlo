load("@llvm-project//mlir:tblgen.bzl", "gentbl_cc_library", "gentbl_filegroup", "td_library")

package(
    default_visibility = ["//visibility:public"],
    licenses = ["notice"],
)

exports_files([
    "LICENSE",
    "stablehlo/integrations/python/ChloModule.cpp",
    "stablehlo/integrations/python/StablehloModule.cpp",
])

filegroup(
    name = "stablehlo_ops_td_filegroup",
    srcs = glob(["stablehlo/dialect/*.td"]),
)

cc_library(
    name = "base",
    srcs = [
        "stablehlo/dialect/Base.cpp",
    ],
    hdrs = [
        "stablehlo/dialect/Base.h",
    ],
    deps = [
        ":base_attr_interfaces_inc_gen",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:BytecodeReader",
        "@llvm-project//mlir:BytecodeWriter",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:InferTypeOpInterface",
        "@llvm-project//mlir:QuantOps",
        "@llvm-project//mlir:ShapeDialect",
        "@llvm-project//mlir:Support",
    ],
)

gentbl_cc_library(
    name = "base_attr_interfaces_inc_gen",
    tbl_outs = [
        (
            ["-gen-attr-interface-decls"],
            "stablehlo/dialect/BaseAttrInterfaces.h.inc",
        ),
        (
            ["-gen-attr-interface-defs"],
            "stablehlo/dialect/BaseAttrInterfaces.cpp.inc",
        ),
    ],
    tblgen = "@llvm-project//mlir:mlir-tblgen",
    td_file = "stablehlo/dialect/Base.td",
    deps = [":stablehlo_ops_td_files"],
)

td_library(
    name = "base_td_files",
    srcs = [
        "stablehlo/dialect/Base.td",
    ],
    deps = [
        "@llvm-project//mlir:InferTypeOpInterfaceTdFiles",
        "@llvm-project//mlir:OpBaseTdFiles",
        "@llvm-project//mlir:QuantizationOpsTdFiles",
    ],
)

cc_library(
    name = "broadcast_utils",
    srcs = [
        "stablehlo/dialect/BroadcastUtils.cpp",
    ],
    hdrs = [
        "stablehlo/dialect/BroadcastUtils.h",
    ],
    deps = [
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:ShapeDialect",
    ],
)

gentbl_cc_library(
    name = "chlo_attrs_inc_gen",
    tbl_outs = [
        (
            ["-gen-attrdef-decls"],
            "stablehlo/dialect/ChloAttrs.h.inc",
        ),
        (
            ["-gen-attrdef-defs"],
            "stablehlo/dialect/ChloAttrs.cpp.inc",
        ),
    ],
    tblgen = "@llvm-project//mlir:mlir-tblgen",
    td_file = "stablehlo/dialect/ChloOps.td",
    deps = [
        ":chlo_ops_td_files",
    ],
)

CHLO_CAPI_SOURCES = [
    "stablehlo/integrations/c/ChloAttributes.cpp",
    "stablehlo/integrations/c/ChloDialect.cpp",
]

CHLO_CAPI_HEADERS = [
    "stablehlo/integrations/c/ChloAttributes.h",
    "stablehlo/integrations/c/ChloDialect.h",
]

cc_library(
    name = "chlo_capi",
    srcs = CHLO_CAPI_SOURCES,
    hdrs = CHLO_CAPI_HEADERS,
    deps = [
        ":chlo_ops",
        "@llvm-project//mlir:CAPIIR",
    ],
)

# Header-only target, used when using the C API from a separate shared library.
cc_library(
    name = "chlo_capi_headers",
    hdrs = CHLO_CAPI_HEADERS,
    deps = [
        "@llvm-project//mlir:CAPIIRHeaders",
    ],
)

# Alwayslink target, used when exporting the C API from a shared library.
cc_library(
    name = "chlo_capi_objects",
    srcs = CHLO_CAPI_SOURCES,
    hdrs = CHLO_CAPI_HEADERS,
    deps = [
        ":chlo_ops",
        "@llvm-project//mlir:CAPIIRObjects",
    ],
    alwayslink = True,
)

gentbl_cc_library(
    name = "chlo_enums_inc_gen",
    tbl_outs = [
        (
            ["-gen-enum-decls"],
            "stablehlo/dialect/ChloEnums.h.inc",
        ),
        (
            ["-gen-enum-defs"],
            "stablehlo/dialect/ChloEnums.cpp.inc",
        ),
    ],
    tblgen = "@llvm-project//mlir:mlir-tblgen",
    td_file = "stablehlo/dialect/ChloOps.td",
    deps = [
        ":chlo_ops_td_files",
    ],
)

gentbl_cc_library(
    name = "chlo_ops_inc_gen",
    tbl_outs = [
        (
            ["-gen-op-decls"],
            "stablehlo/dialect/ChloOps.h.inc",
        ),
        (
            ["-gen-op-defs"],
            "stablehlo/dialect/ChloOps.cpp.inc",
        ),
    ],
    tblgen = "@llvm-project//mlir:mlir-tblgen",
    td_file = "stablehlo/dialect/ChloOps.td",
    deps = [
        ":chlo_ops_td_files",
    ],
)

filegroup(
    name = "chlo_ops_py_files",
    srcs = [
        "stablehlo/integrations/python/mlir/dialects/chlo.py",
        ":chlo_ops_py_gen",
    ],
)

gentbl_filegroup(
    name = "chlo_ops_py_gen",
    tbl_outs = [
        (
            [
                "-gen-python-op-bindings",
                "-bind-dialect=chlo",
            ],
            "stablehlo/integrations/python/mlir/dialects/_chlo_ops_gen.py",
        ),
    ],
    tblgen = "@llvm-project//mlir:mlir-tblgen",
    td_file = "stablehlo/integrations/python/mlir/dialects/ChloOps.td",
    deps = [
        ":chlo_ops_py_td_files",
    ],
)

td_library(
    name = "chlo_ops_py_td_files",
    srcs = [
        "@llvm-project//mlir:include/mlir/Bindings/Python/Attributes.td",
    ],
    includes = ["include"],
    deps = [
        ":chlo_ops_td_files",
        "@llvm-project//mlir:OpBaseTdFiles",
    ],
)

td_library(
    name = "chlo_ops_td_files",
    srcs = [
        "stablehlo/dialect/ChloEnums.td",
        "stablehlo/dialect/ChloOps.td",
    ],
    deps = [
        ":base_td_files",
        "@llvm-project//mlir:BuiltinDialectTdFiles",
        "@llvm-project//mlir:ControlFlowInterfacesTdFiles",
        "@llvm-project//mlir:OpBaseTdFiles",
    ],
)

cc_library(
    name = "chlo_ops",
    srcs = [
        "stablehlo/dialect/ChloBytecode.cpp",
        "stablehlo/dialect/ChloOps.cpp",
    ],
    hdrs = [
        "stablehlo/dialect/ChloBytecode.h",
        "stablehlo/dialect/ChloOps.h",
    ],
    deps = [
        ":base",
        ":broadcast_utils",
        ":chlo_attrs_inc_gen",
        ":chlo_enums_inc_gen",
        ":chlo_ops_inc_gen",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:BytecodeReader",
        "@llvm-project//mlir:BytecodeWriter",
        "@llvm-project//mlir:ComplexDialect",
        "@llvm-project//mlir:ControlFlowInterfaces",
        "@llvm-project//mlir:Dialect",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:InferTypeOpInterface",
        "@llvm-project//mlir:QuantOps",
        "@llvm-project//mlir:TransformUtils",
    ],
)

cc_library(
    name = "version",
    srcs = [
        "stablehlo/dialect/Version.cpp",
    ],
    hdrs = [
        "stablehlo/dialect/Version.h",
    ],
    deps = [
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:Support",
    ],
)

gentbl_cc_library(
    name = "vhlo_attr_interfaces_inc_gen",
    tbl_outs = [
        (
            ["-gen-attr-interface-decls"],
            "stablehlo/dialect/VhloAttrInterfaces.h.inc",
        ),
        (
            ["-gen-attr-interface-defs"],
            "stablehlo/dialect/VhloAttrInterfaces.cpp.inc",
        ),
    ],
    tblgen = "@llvm-project//mlir:mlir-tblgen",
    td_file = "stablehlo/dialect/VhloAttrs.td",
    deps = [
        ":vhlo_ops_td_files",
    ],
)

gentbl_cc_library(
    name = "vhlo_attrs_inc_gen",
    tbl_outs = [
        (
            ["-gen-attrdef-decls"],
            "stablehlo/dialect/VhloAttrs.h.inc",
        ),
        (
            ["-gen-attrdef-defs"],
            "stablehlo/dialect/VhloAttrs.cpp.inc",
        ),
    ],
    tblgen = "@llvm-project//mlir:mlir-tblgen",
    td_file = "stablehlo/dialect/VhloOps.td",
    deps = [
        ":vhlo_ops_td_files",
    ],
)

gentbl_cc_library(
    name = "vhlo_enums_inc_gen",
    tbl_outs = [
        (
            ["-gen-enum-decls"],
            "stablehlo/dialect/VhloEnums.h.inc",
        ),
        (
            ["-gen-enum-defs"],
            "stablehlo/dialect/VhloEnums.cpp.inc",
        ),
    ],
    tblgen = "@llvm-project//mlir:mlir-tblgen",
    td_file = "stablehlo/dialect/VhloEnums.td",
    deps = [
        ":vhlo_ops_td_files",
    ],
)

gentbl_cc_library(
    name = "vhlo_op_interfaces_inc_gen",
    tbl_outs = [
        (
            ["-gen-op-interface-decls"],
            "stablehlo/dialect/VhloOpInterfaces.h.inc",
        ),
        (
            ["-gen-op-interface-defs"],
            "stablehlo/dialect/VhloOpInterfaces.cpp.inc",
        ),
    ],
    tblgen = "@llvm-project//mlir:mlir-tblgen",
    td_file = "stablehlo/dialect/VhloOps.td",
    deps = [
        ":vhlo_ops_td_files",
    ],
)

cc_library(
    name = "vhlo_ops",
    srcs = [
        "stablehlo/dialect/VhloBytecode.cpp",
        "stablehlo/dialect/VhloOps.cpp",
    ],
    hdrs = [
        "stablehlo/dialect/VhloBytecode.h",
        "stablehlo/dialect/VhloOps.h",
    ],
    deps = [
        ":base",
        ":stablehlo_assembly_format",
        ":version",
        ":vhlo_attr_interfaces_inc_gen",
        ":vhlo_attrs_inc_gen",
        ":vhlo_enums_inc_gen",
        ":vhlo_op_interfaces_inc_gen",
        ":vhlo_ops_inc_gen",
        ":vhlo_type_interfaces_inc_gen",
        ":vhlo_types_inc_gen",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:QuantOps",
        "@llvm-project//mlir:ShapeDialect",
        "@llvm-project//mlir:Support",
    ],
)

gentbl_cc_library(
    name = "vhlo_ops_inc_gen",
    tbl_outs = [
        (
            ["-gen-op-decls"],
            "stablehlo/dialect/VhloOps.h.inc",
        ),
        (
            ["-gen-op-defs"],
            "stablehlo/dialect/VhloOps.cpp.inc",
        ),
    ],
    tblgen = "@llvm-project//mlir:mlir-tblgen",
    td_file = "stablehlo/dialect/VhloOps.td",
    deps = [
        ":vhlo_ops_td_files",
    ],
)

td_library(
    name = "vhlo_ops_td_files",
    srcs = [
        "stablehlo/dialect/VhloAttrs.td",
        "stablehlo/dialect/VhloBase.td",
        "stablehlo/dialect/VhloDialect.td",
        "stablehlo/dialect/VhloEnums.td",
        "stablehlo/dialect/VhloOps.td",
        "stablehlo/dialect/VhloTypes.td",
    ],
    deps = [
        "@llvm-project//mlir:BuiltinDialectTdFiles",
        "@llvm-project//mlir:OpBaseTdFiles",
        "@llvm-project//mlir:ShapeOpsTdFiles",
    ],
)

gentbl_cc_library(
    name = "vhlo_type_interfaces_inc_gen",
    tbl_outs = [
        (
            ["-gen-type-interface-decls"],
            "stablehlo/dialect/VhloTypeInterfaces.h.inc",
        ),
        (
            ["-gen-type-interface-defs"],
            "stablehlo/dialect/VhloTypeInterfaces.cpp.inc",
        ),
    ],
    tblgen = "@llvm-project//mlir:mlir-tblgen",
    td_file = "stablehlo/dialect/VhloTypes.td",
    deps = [
        ":vhlo_ops_td_files",
    ],
)

gentbl_cc_library(
    name = "vhlo_types_inc_gen",
    tbl_outs = [
        (
            ["-gen-typedef-decls"],
            "stablehlo/dialect/VhloTypeDefs.h.inc",
        ),
        (
            ["-gen-typedef-defs"],
            "stablehlo/dialect/VhloTypeDefs.cpp.inc",
        ),
    ],
    tblgen = "@llvm-project//mlir:mlir-tblgen",
    td_file = "stablehlo/dialect/VhloOps.td",
    deps = [
        ":vhlo_ops_td_files",
    ],
)

gentbl_cc_library(
    name = "stablehlo_pass_inc_gen",
    strip_include_prefix = ".",
    tbl_outs = [
        (
            [
                "-gen-pass-decls",
            ],
            "stablehlo/transforms/Passes.h.inc",
        ),
    ],
    tblgen = "@llvm-project//mlir:mlir-tblgen",
    td_file = "stablehlo/transforms/Passes.td",
    deps = ["@llvm-project//mlir:PassBaseTdFiles"],
)

cc_library(
    name = "stablehlo_passes",
    srcs = [
        "stablehlo/transforms/StablehloLegalizeToVhlo.cpp",
        "stablehlo/transforms/VhloLegalizeToStablehlo.cpp",
        "stablehlo/transforms/VhloToVersion.cpp",
    ],
    hdrs = [
        "stablehlo/transforms/MapStablehloToVhlo.h",
        "stablehlo/transforms/Passes.h",
    ],
    deps = [
        ":stablehlo_ops",
        ":stablehlo_ops_inc_gen",
        ":stablehlo_pass_inc_gen",
        ":stablehlo_type_conversion",
        ":version",
        ":vhlo_ops",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:Pass",
        "@llvm-project//mlir:Support",
        "@llvm-project//mlir:Transforms",
    ],
)

cc_library(
    name = "stablehlo_type_conversion",
    srcs = ["stablehlo/transforms/TypeConversion.cpp"],
    hdrs = ["stablehlo/transforms/TypeConversion.h"],
    deps = [
        ":stablehlo_ops",
        ":vhlo_ops",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:FuncDialect",
        "@llvm-project//mlir:FuncTransforms",
        "@llvm-project//mlir:Transforms",
    ],
)

cc_library(
    name = "reference_element",
    srcs = [
        "stablehlo/reference/Element.cpp",
    ],
    hdrs = [
        "stablehlo/reference/Element.h",
    ],
    deps = [
        ":reference_errors",
        ":reference_types",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:ComplexDialect",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:Support",
    ],
)

cc_library(
    name = "reference_errors",
    hdrs = [
        "stablehlo/reference/Errors.h",
    ],
    deps = [
        "@llvm-project//llvm:Support",
    ],
)

cc_library(
    name = "reference_interpreter",
    srcs = [
        "stablehlo/reference/Interpreter.cpp",
    ],
    hdrs = [
        "stablehlo/reference/Interpreter.h",
    ],
    deps = [
        ":reference_errors",
        ":reference_ops",
        ":reference_tensor",
        ":stablehlo_ops",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:FuncDialect",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:Support",
    ],
)

cc_library(
    name = "reference_ops",
    srcs = [
        "stablehlo/reference/Ops.cpp",
    ],
    hdrs = [
        "stablehlo/reference/Ops.h",
    ],
    deps = [
        ":reference_element",
        ":reference_errors",
        ":reference_tensor",
        ":reference_types",
        ":stablehlo_ops",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:Support",
    ],
)

cc_library(
    name = "reference_tensor",
    srcs = [
        "stablehlo/reference/Index.cpp",
        "stablehlo/reference/Tensor.cpp",
    ],
    hdrs = [
        "stablehlo/reference/Index.h",
        "stablehlo/reference/Tensor.h",
    ],
    deps = [
        ":reference_element",
        ":reference_errors",
        ":reference_types",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:Support",
    ],
)

cc_library(
    name = "reference_types",
    srcs = [
        "stablehlo/reference/Types.cpp",
    ],
    hdrs = [
        "stablehlo/reference/Types.h",
    ],
    deps = [
        "@llvm-project//mlir:IR",
    ],
)

cc_library(
    name = "register",
    srcs = [
        "stablehlo/dialect/Register.cpp",
    ],
    hdrs = [
        "stablehlo/dialect/Register.h",
    ],
    deps = [
        ":chlo_ops",
        ":stablehlo_ops",
        ":vhlo_ops",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:SparseTensorDialect",
    ],
)

cc_library(
    name = "stablehlo_assembly_format",
    srcs = [
        "stablehlo/dialect/AssemblyFormat.cpp",
    ],
    hdrs = [
        "stablehlo/dialect/AssemblyFormat.h",
    ],
    deps = [
        ":base",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:Support",
    ],
)

gentbl_cc_library(
    name = "stablehlo_attrs_inc_gen",
    tbl_outs = [
        (
            ["-gen-attrdef-decls"],
            "stablehlo/dialect/StablehloAttrs.h.inc",
        ),
        (
            ["-gen-attrdef-defs"],
            "stablehlo/dialect/StablehloAttrs.cpp.inc",
        ),
    ],
    tblgen = "@llvm-project//mlir:mlir-tblgen",
    td_file = "stablehlo/dialect/StablehloOps.td",
    deps = [
        ":stablehlo_ops_td_files",
    ],
)

STABLEHLO_CAPI_SOURCES = [
    "stablehlo/integrations/c/StablehloAttributes.cpp",
    "stablehlo/integrations/c/StablehloDialect.cpp",
    "stablehlo/integrations/c/StablehloTypes.cpp",
]

STABLEHLO_CAPI_HEADERS = [
    "stablehlo/integrations/c/StablehloAttributes.h",
    "stablehlo/integrations/c/StablehloDialect.h",
    "stablehlo/integrations/c/StablehloTypes.h",
]

cc_library(
    name = "stablehlo_capi",
    srcs = STABLEHLO_CAPI_SOURCES,
    hdrs = STABLEHLO_CAPI_HEADERS,
    deps = [
        ":stablehlo_ops",
        "@llvm-project//mlir:CAPIIR",
    ],
)

# Header-only target, used when using the C API from a separate shared library.
cc_library(
    name = "stablehlo_capi_headers",
    hdrs = STABLEHLO_CAPI_HEADERS,
    deps = [
        "@llvm-project//mlir:CAPIIRHeaders",
    ],
)

# Alwayslink target, used when exporting the C API from a shared library.
cc_library(
    name = "stablehlo_capi_objects",
    srcs = STABLEHLO_CAPI_SOURCES,
    hdrs = STABLEHLO_CAPI_HEADERS,
    deps = [
        ":stablehlo_ops",
        "@llvm-project//mlir:CAPIIRObjects",
    ],
    alwayslink = True,
)

gentbl_cc_library(
    name = "stablehlo_enums_inc_gen",
    tbl_outs = [
        (
            ["-gen-enum-decls"],
            "stablehlo/dialect/StablehloEnums.h.inc",
        ),
        (
            ["-gen-enum-defs"],
            "stablehlo/dialect/StablehloEnums.cpp.inc",
        ),
    ],
    tblgen = "@llvm-project//mlir:mlir-tblgen",
    td_file = "stablehlo/dialect/StablehloOps.td",
    deps = [
        ":stablehlo_ops_td_files",
    ],
)

gentbl_cc_library(
    name = "stablehlo_ops_inc_gen",
    tbl_outs = [
        (
            ["-gen-op-decls"],
            "stablehlo/dialect/StablehloOps.h.inc",
        ),
        (
            ["-gen-op-defs"],
            "stablehlo/dialect/StablehloOps.cpp.inc",
        ),
    ],
    tblgen = "@llvm-project//mlir:mlir-tblgen",
    td_file = "stablehlo/dialect/StablehloOps.td",
    deps = [
        ":stablehlo_ops_td_files",
    ],
)

filegroup(
    name = "stablehlo_ops_py_files",
    srcs = [
        "stablehlo/integrations/python/mlir/dialects/stablehlo.py",
        ":stablehlo_ops_py_gen",
    ],
)

gentbl_filegroup(
    name = "stablehlo_ops_py_gen",
    tbl_outs = [
        (
            [
                "-gen-python-op-bindings",
                "-bind-dialect=stablehlo",
            ],
            "stablehlo/integrations/python/mlir/dialects/_stablehlo_ops_gen.py",
        ),
    ],
    tblgen = "@llvm-project//mlir:mlir-tblgen",
    td_file = "stablehlo/integrations/python/mlir/dialects/StablehloOps.td",
    deps = [
        ":stablehlo_ops_py_td_files",
    ],
)

td_library(
    name = "stablehlo_ops_py_td_files",
    srcs = [
        "@llvm-project//mlir:include/mlir/Bindings/Python/Attributes.td",
    ],
    deps = [
        ":stablehlo_ops_td_files",
        "@llvm-project//mlir:OpBaseTdFiles",
    ],
)

td_library(
    name = "stablehlo_ops_td_files",
    srcs = [
        "stablehlo/dialect/Base.td",
        "stablehlo/dialect/StablehloAttrs.td",
        "stablehlo/dialect/StablehloEnums.td",
        "stablehlo/dialect/StablehloOps.td",
    ],
    deps = [
        ":base_td_files",
        "@llvm-project//mlir:BuiltinDialectTdFiles",
        "@llvm-project//mlir:OpBaseTdFiles",
        "@llvm-project//mlir:ShapeOpsTdFiles",
    ],
)

cc_library(
    name = "stablehlo_type_inference",
    srcs = [
        "stablehlo/dialect/TypeInference.cpp",
    ],
    hdrs = [
        "stablehlo/dialect/TypeInference.h",
    ],
    deps = [
        ":base",
        ":stablehlo_assembly_format",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:InferTypeOpInterface",
        "@llvm-project//mlir:QuantOps",
        "@llvm-project//mlir:Support",
    ],
)

cc_library(
    name = "stablehlo_ops",
    srcs = [
        "stablehlo/dialect/StablehloBytecode.cpp",
        "stablehlo/dialect/StablehloOps.cpp",
    ],
    hdrs = [
        "stablehlo/dialect/StablehloBytecode.h",
        "stablehlo/dialect/StablehloOps.h",
    ],
    deps = [
        ":base",
        ":stablehlo_assembly_format",
        ":stablehlo_attrs_inc_gen",
        ":stablehlo_enums_inc_gen",
        ":stablehlo_ops_inc_gen",
        ":stablehlo_type_inference",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:ArithDialect",
        "@llvm-project//mlir:ComplexDialect",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:InferTypeOpInterface",
        "@llvm-project//mlir:QuantOps",
        "@llvm-project//mlir:ShapeDialect",
        "@llvm-project//mlir:SparseTensorDialect",
        "@llvm-project//mlir:Support",
        "@llvm-project//mlir:TensorDialect",
    ],
)

cc_binary(
    name = "stablehlo-interpreter",
    srcs = [
        "stablehlo/tools/StablehloInterpreterMain.cpp",
    ],
    deps = [
        ":reference_interpreter",
        ":stablehlo_ops",
        ":test_utils",
        "@llvm-project//mlir:FuncDialect",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:Support",
        "@llvm-project//mlir:TranslateLib",
    ],
)

cc_binary(
    name = "stablehlo-opt",
    srcs = [
        "stablehlo/tools/StablehloOptMain.cpp",
    ],
    deps = [
        ":register",
        ":stablehlo_passes",
        ":test_utils",
        "@llvm-project//mlir:AllPassesAndDialects",
        "@llvm-project//mlir:MlirOptLib",
    ],
)

filegroup(
    name = "test_data",
    testonly = True,
    data = [
        ":stablehlo-interpreter",
        ":stablehlo-opt",
        "@llvm-project//llvm:FileCheck",
    ],
)

gentbl_cc_library(
    name = "test_utils_inc_gen",
    tbl_outs = [
        (
            [
                "-gen-pass-decls",
                "-name=HloTest",
            ],
            "stablehlo/tests/TestUtils.h.inc",
        ),
    ],
    tblgen = "@llvm-project//mlir:mlir-tblgen",
    td_file = "stablehlo/tests/TestUtils.td",
    deps = [
        ":test_utils_td_files",
    ],
)

td_library(
    name = "test_utils_td_files",
    srcs = [
        "stablehlo/tests/TestUtils.td",
    ],
    deps = [
        "@llvm-project//mlir:PassBaseTdFiles",
    ],
)

cc_library(
    name = "test_utils",
    srcs = [
        "stablehlo/tests/TestUtils.cpp",
    ],
    hdrs = [
        "stablehlo/tests/TestUtils.h",
    ],
    deps = [
        ":stablehlo_assembly_format",
        ":test_utils_inc_gen",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:FuncDialect",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:InferTypeOpInterface",
        "@llvm-project//mlir:Pass",
        "@llvm-project//mlir:ShapeDialect",
        "@llvm-project//mlir:Support",
        "@llvm-project//mlir:Transforms",
    ],
)
