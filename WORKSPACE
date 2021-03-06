workspace(name = "com_google_googleapis")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

##############################################################################
# Common
##############################################################################

load("//:repository_rules.bzl", "switched_rules_by_language")

switched_rules_by_language(
    name = "com_google_googleapis_imports",
    cc = True,
    csharp = True,
    gapic = True,
    go = True,
    grpc = True,
    java = True,
    nodejs = True,
    php = True,
    python = True,
    ruby = True,
)

# Protobuf depends on very old version of bazel_skylib (forward compatible with the new one).
# Importing older version of bazel_skylib first (this is one of the things that protobuf_deps() call
# below will do) breaks the grpc repositories, so importing the proper version explicitly before
# everything else.
http_archive(
    name = "bazel_skylib",
    urls = ["https://github.com/bazelbuild/bazel-skylib/releases/download/0.9.0/bazel_skylib-0.9.0.tar.gz"],
)

http_archive(
    name = "com_google_protobuf",
    strip_prefix = "protobuf-c60aaf79e63b911b2c04c04e1eacb4f3c36ef790",  # this is 3.9.1 with fixes
    urls = ["https://github.com/protocolbuffers/protobuf/archive/c60aaf79e63b911b2c04c04e1eacb4f3c36ef790.zip"],
)

load("@com_google_protobuf//:protobuf_deps.bzl", "protobuf_deps")

protobuf_deps()

http_archive(
    name = "rules_proto",
    sha256 = "602e7161d9195e50246177e7c55b2f39950a9cf7366f74ed5f22fd45750cd208",
    strip_prefix = "rules_proto-97d8af4dc474595af3900dd85cb3a29ad28cc313",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/rules_proto/archive/97d8af4dc474595af3900dd85cb3a29ad28cc313.tar.gz",
        "https://github.com/bazelbuild/rules_proto/archive/97d8af4dc474595af3900dd85cb3a29ad28cc313.tar.gz",
    ],
)

load("@rules_proto//proto:repositories.bzl", "rules_proto_dependencies", "rules_proto_toolchains")

rules_proto_dependencies()

rules_proto_toolchains()

# Note gapic-generator contains java-specific and common code, that is why it is imported in common
# section
http_archive(
    name = "com_google_api_codegen",
    strip_prefix = "gapic-generator-b32c73219d617f90de70bfa6ff0ea0b0dd638dfe",
    urls = ["https://github.com/googleapis/gapic-generator/archive/b32c73219d617f90de70bfa6ff0ea0b0dd638dfe.zip"],
)

##############################################################################
# C++
##############################################################################
# C++ must go before everything else, since the key dependencies (protobuf and gRPC)
# are C++ repositories and they have the highest chance to have the correct versions of the
# transitive dependencies (for those dependencies which are shared by many other repositories
# imported in this workspace).
#
# Note, even though protobuf and gRPC are mostly written in C++, they are used to generate stuff
# for most of the other languages as well, so they can be considered as the core cross-language
# dependencies.

http_archive(
    name = "com_github_grpc_grpc",
    strip_prefix = "grpc-8347f4753568b5b66e49111c60ae2841278d3f33",  # this is 1.25.0 with fixes
    urls = ["https://github.com/grpc/grpc/archive/8347f4753568b5b66e49111c60ae2841278d3f33.zip"],
)

load("@com_github_grpc_grpc//bazel:grpc_deps.bzl", "grpc_deps")

grpc_deps()

load("@upb//bazel:repository_defs.bzl", "bazel_version_repository")

bazel_version_repository(
    name = "bazel_version",
)

load("@build_bazel_rules_apple//apple:repositories.bzl", "apple_rules_dependencies")

apple_rules_dependencies()

load("@build_bazel_apple_support//lib:repositories.bzl", "apple_support_dependencies")

apple_support_dependencies()

##############################################################################
# Java
##############################################################################

http_archive(
    name = "com_google_api_gax_java",
    strip_prefix = "gax-java-9f83322606b982bebc4ebb2cd1d33485a1776396", # v1.52.0 plus bazel-2.0.0 compatibility fix
    urls = ["https://github.com/googleapis/gax-java/archive/9f83322606b982bebc4ebb2cd1d33485a1776396.zip"],
)

load("@com_google_api_gax_java//:repository_rules.bzl", "com_google_api_gax_java_properties")

com_google_api_gax_java_properties(
    name = "com_google_api_gax_java_properties",
    file = "@com_google_api_gax_java//:dependencies.properties",
)

load("@com_google_api_gax_java//:repositories.bzl", "com_google_api_gax_java_repositories")

com_google_api_gax_java_repositories()

# gapic-generator transitive
# (goes AFTER java-gax, since both java gax and gapic-generator are written in java and may conflict)
load("@com_google_api_codegen//:repository_rules.bzl", "com_google_api_codegen_properties")

com_google_api_codegen_properties(
    name = "com_google_api_codegen_properties",
    file = "@com_google_api_codegen//:dependencies.properties",
)

load("@com_google_api_codegen//:repositories.bzl", "com_google_api_codegen_repositories")

http_archive(
    name = "com_google_protoc_java_resource_names_plugin",
    strip_prefix = "protoc-java-resource-names-plugin-184e9515f06b1373ae07e6b44062c65b705992ba",
    urls = ["https://github.com/googleapis/protoc-java-resource-names-plugin/archive/184e9515f06b1373ae07e6b44062c65b705992ba.zip"],
)

com_google_api_codegen_repositories()

# protoc-java-resource-names-plugin (loaded in com_google_api_codegen_repositories())
# (required to support resource names feature in gapic generator)
load(
    "@com_google_protoc_java_resource_names_plugin//:repositories.bzl",
    "com_google_protoc_java_resource_names_plugin_repositories",
)

com_google_protoc_java_resource_names_plugin_repositories()

##############################################################################
# Python
##############################################################################
load("@com_google_api_codegen//rules_gapic/python:py_gapic_repositories.bzl", "py_gapic_repositories")

py_gapic_repositories()

http_archive(
    name = "protoc_docs_plugin",
    strip_prefix = "protoc-docs-plugin-b2502d56b5ec2d47e063976da773206af295362d",
    urls = ["https://github.com/googleapis/protoc-docs-plugin/archive/b2502d56b5ec2d47e063976da773206af295362d.zip"],
)

load(
    "@protoc_docs_plugin//:repositories.bzl",
    "protoc_docs_plugin_register_toolchains",
    "protoc_docs_plugin_repositories",
)

protoc_docs_plugin_repositories()

protoc_docs_plugin_register_toolchains()

##############################################################################
# Go
##############################################################################

# rules_go (support Golang under bazel)
http_archive(
    name = "io_bazel_rules_go",
    sha256 = "a82a352bffae6bee4e95f68a8d80a70e87f42c4741e6a448bec11998fcc82329",
    url = "https://github.com/bazelbuild/rules_go/releases/download/0.18.5/rules_go-0.18.5.tar.gz",
)

load("@io_bazel_rules_go//go:deps.bzl", "go_register_toolchains", "go_rules_dependencies")

go_rules_dependencies()

go_register_toolchains()

# bazel-gazelle (support Golang under bazel)
http_archive(
    name = "bazel_gazelle",
    strip_prefix = "bazel-gazelle-0.17.0",
    urls = ["https://github.com/bazelbuild/bazel-gazelle/archive/0.17.0.zip"],
)

load("@bazel_gazelle//:deps.bzl", "gazelle_dependencies")

gazelle_dependencies()

# go_gapic artifacts runtime dependencies (gax-go)
load("@com_google_api_codegen//rules_gapic/go:go_gapic_repositories.bzl", "go_gapic_repositories")

go_gapic_repositories()

##############################################################################
# PHP
##############################################################################

load("@com_google_api_codegen//rules_gapic/php:php_gapic_repositories.bzl", "php", "php_gapic_repositories")

php(
    name = "php",
    urls = ["https://www.php.net/distributions/php-7.1.30.tar.gz"],
    strip_prefix = "php-7.1.30",
    prebuilt_phps = ["@com_google_api_codegen//rules_gapic/php:resources/php-7.1.30_linux_x86_64.tar.gz"],
)

php_gapic_repositories()
