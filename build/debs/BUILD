package(default_visibility = ["//visibility:public"])

load("@io_kubernetes_build//defs:deb.bzl", "k8s_deb", "deb_data")
load("@io_kubernetes_build//defs:build.bzl", "release_filegroup")
load("@io_kubernetes_build//defs:pkg.bzl", "pkg_tar")
load("//build:workspace.bzl", "CRI_TOOLS_VERSION")

# We do not include kube-scheduler, kube-controller-manager,
# kube-apiserver, and kube-proxy in this list even though we
# produce debs for them. We recommend that they be run in docker
# images. We use the debs that we produce here to build those
# images.
release_filegroup(
    name = "debs",
    srcs = [
        ":cloud-controller-manager.deb",
        ":cri-tools.deb",
        ":kubeadm.deb",
        ":kubectl.deb",
        ":kubelet.deb",
        ":kubernetes-cni.deb",
    ],
)

[deb_data(
    name = binary,
    data = [
        {
            "files": ["//cmd/" + binary],
            "mode": "0755",
            "dir": "/usr/bin",
        },
    ],
) for binary in [
    "cloud-controller-manager",
    "kubectl",
    "kube-apiserver",
    "kube-controller-manager",
    "kube-proxy",
]]

deb_data(
    name = "kube-scheduler",
    data = [
        {
            "files": ["//cmd/kube-scheduler"],
            "mode": "0755",
            "dir": "/usr/bin",
        },
    ],
)

deb_data(
    name = "kubelet",
    data = [
        {
            "files": ["//cmd/kubelet"],
            "mode": "0755",
            "dir": "/usr/bin",
        },
        {
            "files": ["kubelet.service"],
            "mode": "644",
            "dir": "/lib/systemd/system",
        },
    ],
)

deb_data(
    name = "kubeadm",
    data = [
        {
            "files": ["//cmd/kubeadm"],
            "mode": "0755",
            "dir": "/usr/bin",
        },
        {
            "files": ["10-kubeadm.conf"],
            "mode": "644",
            "dir": "/etc/systemd/system/kubelet.service.d",
        },
    ],
)

pkg_tar(
    name = "kubernetes-cni-data",
    package_dir = "/opt/cni/bin",
    deps = ["@kubernetes_cni//file"],
)

pkg_tar(
    name = "cri-tools-data",
    package_dir = "/usr/bin",
    deps = ["@cri_tools//file"],
)

k8s_deb(
    name = "cloud-controller-manager",
    description = "Kubernetes Cloud Controller Manager",
    version_file = "//build:os_package_version",
)

k8s_deb(
    name = "kubectl",
    description = """Kubernetes Command Line Tool
The Kubernetes command line tool for interacting with the Kubernetes API.
""",
    version_file = "//build:os_package_version",
)

k8s_deb(
    name = "kube-apiserver",
    description = "Kubernetes API Server",
    version_file = "//build:os_package_version",
)

k8s_deb(
    name = "kube-controller-manager",
    description = "Kubernetes Controller Manager",
    version_file = "//build:os_package_version",
)

k8s_deb(
    name = "kube-scheduler",
    description = "Kubernetes Scheduler",
    version_file = "//build:os_package_version",
)

k8s_deb(
    name = "kube-proxy",
    depends = [
        "iptables (>= 1.4.21)",
        "iproute2",
    ],
    description = "Kubernetes Service Proxy",
    version_file = "//build:os_package_version",
)

k8s_deb(
    name = "kubelet",
    depends = [
        "iptables (>= 1.4.21)",
        "kubernetes-cni (>= 0.5.1)",
        "iproute2",
        "socat",
        "util-linux",
        "mount",
        "ebtables",
        "ethtool",
    ],
    description = """Kubernetes Node Agent
The node agent of Kubernetes, the container cluster manager
""",
    version_file = "//build:os_package_version",
)

k8s_deb(
    name = "kubeadm",
    depends = [
        "kubelet (>= 1.8.0)",
        "kubectl (>= 1.8.0)",
        "kubernetes-cni (>= 0.5.1)",
        "cri-tools (>= 1.11.0)",
    ],
    description = """Kubernetes Cluster Bootstrapping Tool
The Kubernetes command line tool for bootstrapping a Kubernetes cluster.
""",
    version_file = "//build:os_package_version",
)

k8s_deb(
    name = "kubernetes-cni",
    description = """Kubernetes Packaging of CNI
The Container Networking Interface tools for provisioning container networks.
""",
    version_file = "//build:cni_package_version",
)

k8s_deb(
    name = "cri-tools",
    description = """Container Runtime Interface tools (crictl)""",
    version = CRI_TOOLS_VERSION,
)

filegroup(
    name = "package-srcs",
    srcs = glob(["**"]),
    tags = ["automanaged"],
    visibility = ["//visibility:private"],
)

filegroup(
    name = "all-srcs",
    srcs = [":package-srcs"],
    tags = ["automanaged"],
)
