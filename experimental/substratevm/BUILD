package(default_visibility = ["//visibility:public"])

load("@io_bazel_rules_docker//docker:docker.bzl", "docker_build")
load("@package_bundle//file:packages.bzl", "packages")
load("//cacerts:java.bzl", "cacerts_java")
load("@io_bazel_rules_docker//java:image.bzl", "substratevm_image")

cacerts_java(
    name = "cacerts_java",
)

[docker_build(
    name = "substratevm" if (not mode) else mode[1:],
    base = "//base" + mode,
    debs = [
        packages["zlib1g"],
    ],
    tars = [":cacerts_java"],
) for mode in [
    "",
    ":debug",
]]

load("@io_bazel_rules_docker//contrib:test.bzl", "container_test")

container_test(
    name = "substreatevm_test",
    configs = ["testdata/substratevm.yaml"],
    image = ":latest",
)

substratevm_image(
    name = "check_certs",
    srcs = ["testdata/CheckCerts.java"],
    base = "//substratevm",
    main_class = "testdata.CheckCerts",
)

container_test(
    name = "check_certs_test",
    configs = ["testdata/substratevm.yaml"],
    image = ":substratevm",
)
