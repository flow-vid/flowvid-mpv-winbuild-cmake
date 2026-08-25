# FlowVid Windows libmpv recipe

Pinned CMake/MinGW cross-build recipe used by
[`flowvid-libmpv-desktop`](https://github.com/flow-vid/flowvid-libmpv-desktop) to produce the LGPL
`libmpv-2.dll` bundled with FlowVid Desktop.

This repository does not publish binaries. Its inherited publication workflows were removed because
artifact ownership belongs to `flowvid-libmpv-desktop`. The consuming workflow checks out an exact
recipe commit, builds the native archive and records the resulting digest.

## FlowVid configuration

- The behavior-defining mpv and FFmpeg revisions are recorded in [`VERSIONS.md`](VERSIONS.md).
- [`compile-lgpl-libmpv.patch`](compile-lgpl-libmpv.patch) removes GPL-incompatible components from
  the FlowVid build.
- The supported release path is 64-bit Windows with Clang through the consuming repository's manual
  workflows.
- This repository remains a build recipe. Downloads belong on the consuming repository's Releases
  page.

## Local build outline

A modern Linux environment with CMake, Ninja, Clang and the package prerequisites is required. The
same entry points used by CI are:

```sh
cmake -DTARGET_ARCH=x86_64-w64-mingw32 \
  -DCOMPILER_TOOLCHAIN=clang \
  -G Ninja -B build64 -S .
cd build64
ninja llvm
ninja rustup
ninja llvm-clang
ninja mpv
```

The complete dependency graph is defined under `packages/`; the consuming workflow is the
authoritative executable build specification.

## Upstream and license

This repository is derived from
[`shinchiro/mpv-winbuild-cmake`](https://github.com/shinchiro/mpv-winbuild-cmake) and retains its
history and component licenses. The FlowVid build's licensing rationale and exclusions are
documented in `flowvid-libmpv-desktop/LICENSE-NOTES.md`.
