# Copyright 2016 Google Inc. All Rights Reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
################################################################################
#

proxy_use_local=True

load(
    "//src/envoy/mixer:repositories.bzl",
    "mixer_client_repositories",
)

mixer_client_repositories(use_local=proxy_use_local)

load(
    "@mixerclient_git//:repositories.bzl",
    "mixerapi_repositories",
)

mixerapi_repositories(use_local=proxy_use_local)

bind(
    name = "boringssl_crypto",
    actual = "//external:ssl",
)

ENVOY_SHA = "e593fedc3232fbb694f3ec985567a2c7dff05212"  # Oct 31, 2017

http_archive(
    name = "envoy",
    strip_prefix = "envoy-" + ENVOY_SHA,
    url = "https://github.com/envoyproxy/envoy/archive/" + ENVOY_SHA + ".zip",
)

load("@envoy//bazel:repositories.bzl", "envoy_dependencies")

envoy_dependencies(repository="@envoy", skip_targets=["io_bazel_rules_go"])

load("@envoy//bazel:cc_configure.bzl", "cc_configure")

cc_configure()

load("@envoy_api//bazel:repositories.bzl", "api_dependencies")

api_dependencies()

git_repository(
    name = "io_bazel_rules_go",
    commit = "9cf23e2aab101f86e4f51d8c5e0f14c012c2161c",  # Oct 12, 2017 (Add `build_external` option to `go_repository`)
    remote = "https://github.com/bazelbuild/rules_go.git",
)

load("@mixerapi_git//:api_dependencies.bzl", "mixer_api_for_proxy_dependencies")
mixer_api_for_proxy_dependencies()

ISTIO_SHA = "d0142e1afe41c18917018e2fa85ab37254f7e0ca"

if proxy_use_local:
    local_repository(
	name = "io_istio_istio",
	path = "../../go/src/istio.io/istio"
    )
else:
    git_repository(
        name = "io_istio_istio",
        commit = ISTIO_SHA,
        remote = "https://github.com/istio/istio",
    )

load("@io_bazel_rules_go//go:def.bzl", "go_repository")

go_repository(
    name = "org_uber_go_zap",
    commit = "9cabc84638b70e564c3dab2766efcb1ded2aac9f",  # Jun 8, 2017 (v1.4.1)
    importpath = "go.uber.org/zap",
)

go_repository(
    name = "org_uber_go_atomic",
    commit = "4e336646b2ef9fc6e47be8e21594178f98e5ebcf",  # Apr 12, 2017 (v1.2.0)
    importpath = "go.uber.org/atomic",
)

go_repository(
    name = "com_github_spf13_pflag",
    commit = "e57e3eeb33f795204c1ca35f56c44f83227c6e66",
    importpath = "github.com/spf13/pflag",
)

go_repository(
    name = "com_github_spf13_cobra",
    commit = "2df9a531813370438a4d79bfc33e21f58063ed87",
    importpath = "github.com/spf13/cobra",
)

go_repository(
    name = "com_github_inconshreveable_mousetrap",
    commit = "76626ae9c91c4f2a10f34cad8ce83ea42c93bb75",
    importpath = "github.com/inconshreveable/mousetrap",
)

load("//src/envoy/mixer/integration_test:repositories.bzl", "mixer_test_repositories")
mixer_test_repositories()
