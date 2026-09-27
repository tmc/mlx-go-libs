# mlx-go-libs

This repository publishes the native MLX libraries that
[mlx-go](https://github.com/tmc/mlx-go) loads, as GitHub release assets. It
holds no code; each release is built from a tagged mlx-go revision.

## Releases

A release is tagged `libs-v<core>-rev<N>`, where `<core>` is the MLX release
the libraries are built from and `N` counts rebuilds of that core. Each
release has one archive per platform:

	mlx-go-libs-<core>-rev<N>-<platform>.tar.gz

| Platform | Contents |
|---|---|
| `darwin-arm64` | macOS on Apple silicon, Metal. Developer ID signed and notarized. |
| `linux-arm64` | Linux arm64, CPU. |
| `linux-arm64-cuda13` | Linux arm64, CUDA 13 (including DGX Spark / GB10). |
| `linux-amd64` | Linux amd64, CPU. |
| `linux-amd64-cuda13` | Linux amd64, CUDA 13. |

Each archive contains a `SHA256SUMS` file covering every other file in it,
and has a SLSA provenance statement beside it
(`mlx-go-libs-<version>-<platform>.intoto.jsonl`). The release notes list
each archive's sha256, the sources it was built from, and the host
libraries it needs. The CUDA archives do not include NVIDIA's shared
libraries; the host provides them.

## Use with mlx-go

mlx-go downloads the archive it needs on first use and caches it. Each
tagged mlx-go release names one library release and records the sha256 of
its archives, so there is nothing to configure:

	go get github.com/tmc/mlx-go

On a Linux CUDA host, install the CUDA archive first; without it, mlx-go
downloads the CPU archive and runs on the CPU:

	go run github.com/tmc/mlx-go/cmd/mlx-lib-setup -platform linux-arm64-cuda13

To use libraries from an archive you downloaded yourself, extract it and set
`MLX_LIB_PATH` to the directory holding the libraries (`lib/` in the CUDA
archives).

## License

The libraries are built from MLX and mlx-c, which are MIT licensed, and
from the third-party code listed in [NOTICE](NOTICE). The license texts
are in [THIRD_PARTY_LICENSES](THIRD_PARTY_LICENSES). Each archive carries
the same files. The mlx-go parts are Apache 2.0; see [LICENSE](LICENSE).
