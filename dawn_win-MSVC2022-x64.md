Based on https://dawn.googlesource.com/dawn/+/HEAD/docs/quickstart-cmake.md

```console
C:\DEV\WebGPU_dawn>git clone https://dawn.googlesource.com/dawn
```

I think the --depth=1 should work, even if they do a "full" clone in the guide...
```console
git clone --depth=1 git@github.com:google/dawn.git
```

ATTENTION:
The option ```--config Release``` is missing in the guide, which causes 3rd-party libs to be built in ```out\Release\<...>\Debug```, 
which subsequently leads to the install script failing as it can't find the files that it expects in ```out\Release\<...>\Release```!
I added this switch to both the build and the install command to be on the safe side, but it's probalby only needed for the ```cmake --build ...``` command.

Run the build commands from a VS2022 x64 dev batch command shell ('x64 Native Tools Command Prompt for VS2022' from Start Menu).
<b>NOTE</b>: do NOT build into the directory ```build``` as you would usually do for CMake projects! 
There is already a ```build``` directory in the checked out source code of the <b>dawn</b> project. 
From my experience, the project will not build successfully in the ```build``` directory - just use the ```out\Release``` folder as is used in the guide.

```console
C:\DEV\WebGPU_dawn\dawn>cmake -S . -B .\out\Release -DDAWN_FETCH_DEPENDENCIES=ON -DDAWN_ENABLE_INSTALL=ON -DCMAKE_BUILD_TYPE=Release
C:\DEV\WebGPU_dawn\dawn>cmake --build .\out\Release --config Release
C:\DEV\WebGPU_dawn\dawn>cmake --install .\out\Release --config Release --prefix install/Release
```
