load("//tools:clang.bzl", "clang_tool")  # todo: this should be decoupled and use the library toolchain, not the compiler one

clang_tool("llvm-diff")  # depended on by compiler_tests

clang_tool("llvm-symbolizer")

cc_binary(
    name = "pipeline_test_runner",
    testonly = 1,
    srcs = [
        "pipeline_test_runner.cc",
    ],
    linkstatic = select({
        "//tools/config:linkshared": 0,
        "//conditions:default": 1,
    }),
    visibility = ["//tools:__pkg__"],
    deps = [
        "//ast/desugar",
        "//ast/treemap",
        "//cfg",
        "//cfg/builder",
        "//class_flatten",
        "//core",
        "//definition_validator",
        "//infer",
        "//local_vars",
        "//main/autogen",
        "//main/lsp",
        "//main/minimize",
        "//main/pipeline/semantic_extension:none",
        "//namer",
        "//packager",
        "//payload",
        "//resolver",
        "//rewriter",
        "//test/helpers",
        "@cxxopts",
        "@doctest",
        "@doctest//:doctest_custom_main",
    ],
)

cc_binary(
    name = "lsp_test_runner",
    testonly = 1,
    srcs = [
        "lsp_test_runner.cc",
    ],
    linkstatic = select({
        "//tools/config:linkshared": 0,
        "//conditions:default": 1,
    }),
    visibility = ["//tools:__pkg__"],
    deps = [
        "//common",
        "//main/lsp",
        "//payload",
        "//test/helpers",
        "@cxxopts",
        "@doctest",
        "@doctest//:doctest_custom_main",
    ],
)

cc_binary(
    name = "parser_test_runner",
    testonly = 1,
    srcs = [
        "parser_test_runner.cc",
    ],
    linkstatic = select({
        "//tools/config:linkshared": 0,
        "//conditions:default": 1,
    }),
    visibility = ["//tools:__pkg__"],
    deps = [
        "//ast/desugar",
        "//ast/treemap",
        "//cfg",
        "//cfg/builder",
        "//class_flatten",
        "//core",
        "//definition_validator",
        "//infer",
        "//local_vars",
        "//main/autogen",
        "//main/pipeline/semantic_extension:none",
        "//namer",
        "//payload",
        "//resolver",
        "//rewriter",
        "//test/helpers",
        "@cxxopts",
        "@doctest",
        "@doctest//:doctest_custom_main",
    ],
)

cc_binary(
    name = "print_document_symbols",
    testonly = 1,
    srcs = [
        "print_document_symbols.cc",
    ],
    linkstatic = select({
        "//tools/config:linkshared": 0,
        "//conditions:default": 1,
    }),
    visibility = ["//tools:__pkg__"],
    deps = [
        "//main/lsp",
        "//payload",
    ],
)

cc_test(
    name = "hello-test",
    size = "small",
    srcs = ["hello-test.cc"],
    linkstatic = select({
        "//tools/config:linkshared": 0,
        "//conditions:default": 1,
    }),
    visibility = ["//tools:__pkg__"],
    deps = [
        "//ast",
        "//ast/treemap",
        "//common",
        "//core",
        "//core/serialize",
        "//parser",
        "//payload",
        "@cxxopts",
        "@doctest",
        "@doctest//:doctest_main",
    ],
)

cc_test(
    name = "pkg-autocorrects-test",
    size = "small",
    srcs = ["pkg_autocorrects_test.cc"],
    linkstatic = select({
        "//tools/config:linkshared": 0,
        "//conditions:default": 1,
    }),
    visibility = ["//tools:__pkg__"],
    deps = [
        "//ast",
        "//ast/desugar",
        "//common",
        "//common/concurrency",
        "//core",
        "//local_vars",
        "//packager",
        "//parser",
        "@doctest",
        "@doctest//:doctest_main",
    ],
)

cc_test(
    name = "error-check-test",
    size = "small",
    srcs = ["error-check-test.cc"],
    linkstatic = select({
        "//tools/config:linkshared": 0,
        "//conditions:default": 1,
    }),
    visibility = ["//tools:__pkg__"],
    deps = [
        "//ast",
        "//ast/desugar",
        "//common",
        "//core",
        "//parser",
        "@doctest",
        "@doctest//:doctest_main",
    ],
)

# Passes with --config=dbg but I'm not smart enough to figure out how to make it
# only run when in that config
# sh_test(
#     name = "backtrace-test",
#     size = "small",
#     srcs = ["backtrace-test.sh"],
#     data = ["backtrace-test-raise"],
# )

cc_binary(
    name = "backtrace-test-raise",
    srcs = ["backtrace-test-raise.cc"],
    linkstatic = select({
        "//tools/config:linkshared": 0,
        "//conditions:default": 1,
    }),
    visibility = ["//tools:__pkg__"],
)

cc_binary(
    name = "backtrace-test-error",
    srcs = ["backtrace-test-error.cc"],
    linkstatic = select({
        "//tools/config:linkshared": 0,
        "//conditions:default": 1,
    }),
    visibility = ["//tools:__pkg__"],
    deps = ["//common"],
)

load(":pipeline_test.bzl", "pipeline_tests", "single_package_rbi_test")

filegroup(
    name = "rbi_gen_files",
    srcs = glob(["testdata/packager/rbi_gen_files/**/*.rb"]),
)

single_package_rbi_test(
    name = "end_to_end_rbi_test",
    rb_files = ["//test:rbi_gen_files"],
)

pipeline_tests(
    "test_corpus",
    glob(
        [
            "testdata/**/*.rb",
            "testdata/**/*.exp",
        ],
        exclude = [
            "testdata/compiler/**/*",
            "testdata/ruby_benchmark/**/*",
        ],
    ),
    "PosTests",
)

pipeline_tests(
    "test_packager",
    glob(
        [
            "testdata/packager/**/*.rb",
            "testdata/packager/**/*.exp",
        ],
    ),
    "PackagerTests",
)

pipeline_tests(
    "whitequark_parser_corpus",
    glob([
        "whitequark/**/*.rb",
        "whitequark/**/*.exp",
    ]),
    "WhitequarkParserTests",
)

pipeline_tests(
    "test_corpus_lsp",
    glob(
        [
            "testdata/**/*.rb",
            "testdata/**/*.exp",
        ],
        exclude = [
            "testdata/compiler/**/*",
            "testdata/ruby_benchmark/**/*",
        ],
    ),
    "LSPTests",
    extra_files = ["testdata/lsp/rubyfmt-stub/rubyfmt"],
)

test_suite(
    name = "test",
    tests = ["test_corpus"],
)

test_suite(
    name = "whitequark_parser_tests",
    tests = ["whitequark_parser_corpus"],
)

load(":compiler_tests.bzl", "compiler_tests")

compiler_tests(
    "compiler",
    glob([
        "testdata/compiler/**/*.rb",
        "testdata/ruby_benchmark/**/*.rb",
    ]),
    tags = ["compiler"],
)

sh_library(
    name = "logging",
    srcs = ["logging.sh"],
)

sh_library(
    name = "hermetic_tar",
    srcs = ["hermetic_tar.sh"],
)

sh_binary(
    name = "generate_out_file",
    srcs = ["generate_out_file.sh"],
    data = [
        "patch_require.rb",
        "//gems/sorbet-runtime",
        "@sorbet_ruby_2_7_for_compiler//:ruby",
    ],
    deps = [
        ":logging",
        "@bazel_tools//tools/bash/runfiles",
    ],
)

sh_binary(
    name = "build_extension",
    srcs = ["build_extension.sh"],
    data = ["//compiler:sorbet"],
    tags = ["compiler"],
    deps = [
        ":hermetic_tar",
        ":logging",
        "@bazel_tools//tools/bash/runfiles",
    ],
)

sh_binary(
    name = "test_corpus_runner",
    srcs = ["test_corpus_runner.sh"],
    tags = ["compiler"],
    deps = [
        ":logging",
    ],
)

sh_binary(
    name = "validate_exp",
    srcs = ["validate_exp.sh"],
    data = [
        "diff-diff.rb",
        "@sorbet_ruby_2_7_for_compiler//:ruby",
    ],
    tags = ["compiler"],
    deps = [
        ":logging",
        "@bazel_tools//tools/bash/runfiles",
    ],
)

sh_binary(
    name = "single_package_runner",
    srcs = ["test_single_package_runner.sh"],
    data = [
        "rbi_gen_package_runner.rb",
        "//gems/sorbet-runtime",
        "//main:sorbet",
        "@sorbet_ruby_2_7//:ruby",
    ],
    deps = [
        ":logging",
        "@bazel_tools//tools/bash/runfiles",
    ],
)

sh_binary(
    name = "filecheck",
    testonly = 1,
    srcs = ["filecheck.sh"],
    data = ["@llvm//:FileCheck"],
    tags = ["compiler"],
    deps = [
        ":logging",
    ],
)
