Based on https://clang.llvm.org/get_started.html

(as i was building the LLVM toolchain as a requirement for wgpu-native -> 
https://github.com/gfx-rs/wgpu-native/wiki/Getting-Started
i needed to build 'extra-clang-tools', too:
  https://rust-lang.github.io/rust-bindgen/requirements.html
  mentions that 'extra-clang-tools' are needed, too
)


IMPORTANT - for git checkouts don't use the https link, as the download will keep failing otherwise!!!
    git clone --config core.autocrlf=false --depth 1 git://github.com/llvm/llvm-project.git

IMPORTANT - make sure to run the build commands from a VS2022 x64 dev batch command shell ('x64 Native Tools Command Prompt for VS2022' from Start Menu)

Assuming you are cloning to
```console
C:\DEV> git clone --config core.autocrlf=false --depth 1 git://github.com/llvm/llvm-project.git LLVM-Clang
C:\DEV> cd LLVM-Clang   
C:\DEV\LLVM-Clang> git config --local --add remote.origin.fetch '^refs/heads/users/*'
C:\DEV\LLVM-Clang> git config --local --add remote.origin.fetch '^refs/heads/revert-*'
```

In order to build LLVM, clang, clang-tools-extra, and lld (building 'outside' the source directory, otherwise install command later on might not work?) execute the following command. 
Note that i not only build LLVM and clang, but also clang-tools-extra (for wgpu-native, and rust-bindgen, respectively), and just for the sake of a complete toolchain, also lld, the LLVM linker.

UNTESTED, but should work and would be a nice addition - WebAssebmbly support! (got this from a zig LLVM build instruction somewhere)
```console
-DLLVM_TARGETS_TO_BUILD="host;WebAssembly"    
```

```console
C:\DEV\LLVM-Clang> cmake -G "Visual Studio 17 2022" -A x64 -Thost=x64 -S .\llvm-project\llvm -DLLVM_ENABLE_PROJECTS="clang;clang-tools-extra;lld" -B .\build_everything -DLLVM_INSTALL_UTILS=ON -DCMAKE_INSTALL_PREFIX=.\install -DCMAKE_BUILD_TYPE=Release
C:\DEV\LLVM-Clang> cmake --build .\build_everything --config Release
C:\DEV\LLVM-Clang> cmake --install .\build_everything --prefix .\install --config Release
```
NOTE: for install prefix, an absolute path is sometimes required/preferred. In order to still keep the command line 'generic', you can use %CD% to access the current directory:
```console
-DCMAKE_INSTALL_PREFIX=%CD%\install
```

    

Finally, don't forget to set ENV VARS (best to set these system wide in "Edit environment variables for your account" in Windows Start Menu):
```console
set PATH=%PATH%;C:\DEV\LLVM-Clang\install\bin
set LIBCLANG_PATH=C:\DEV\LLVM-Clang\install\lib
```
