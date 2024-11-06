Based on 
https://github.com/gfx-rs/wgpu-native/wiki/Getting-Started

Rust must be installed, then run the following commands to update the toolchain (rust & cargo):
```console
rustup update
cargo update
```

Another requirement is LLVM, and there are links to follow:
https://rust-lang.github.io/rust-bindgen/requirements.html
which again links
https://clang.llvm.org/get_started.html
BUT i have already prepared a condensed readme on installing a correctly configured LLVM toolchain for wgpu-native in this repo [LLVM-clang-lld_win-MSVC2022-x64.md](LLVM-clang-lld_win-MSVC2022-x64.md).

Cloning - again make sure NOT to copy the HTTPS link, but the SSH link instead (when cloning the HTTPS link, the connection kept interrupting, thus the cloning op kept failing):
```console
git clone git@github.com:gfx-rs/wgpu-native.git
```

installed meson-1.6.0-64.msi

Release build, and install (NOTE: in the official instructions, the don't even mention meson, they use make/Makefiles ...):

```console
C:\DEV\WebGPU_wgpu-native-C-from-rust\wgpu-native> meson setup --buildtype=release --prefix=%CD%\meson-install-release meson-build-release

C:\DEV\WebGPU_wgpu-native-C-from-rust\wgpu-native>cd meson-build-release
C:\DEV\WebGPU_wgpu-native-C-from-rust\wgpu-native\meson-build-release>meson compile

C:\DEV\WebGPU_wgpu-native-C-from-rust\wgpu-native\meson-build-release>meson install
```

NOTE: also have a look in 
```console
C:\DEV\WebGPU_wgpu-native-C-from-rust\wgpu-native\target\release
```
as there may be some important files there, which the install script just didn't pick up... (?)
