# YUV420P_Player
YUV420P_Player using ffmpeg

## Checkout

Checkout source code from git repository
```
git clone https://github.com/melchi45/YUV420P_Player.git
cd YUV420P_Player
```

## Dependency for build for windows build

```
git submodule init
```

## vcpkg

about vcpkg from this url.
https://blogs.msdn.microsoft.com/vcblog/2016/09/19/vcpkg-a-tool-to-acquire-and-build-c-open-source-libraries-on-windows/
https://docs.microsoft.com/en-us/cpp/vcpkg

build vcpkg to submodule folder
```
cd vcpkg
.\bootstrap-vcpkg.bat
```

Then, to hook up user-wide integration, run (note: requires admin on first use)
```
.\vcpkg integrate install
PS D:\workspace\YUV420P_Player\vcpkg> .\vcpkg integrate install
Applied user-wide integration for this vcpkg root.

All MSBuild C++ projects can now #include any installed libraries.
Linking will be handled automatically.
Installing new libraries will make them instantly available.

CMake projects should use: "-DCMAKE_TOOLCHAIN_FILE=D:/workspace/YUV420P_Player/vcpkg/scripts/buildsystems/vcpkg.cmake"
```

Install any packages for x86(x86-windows), x64(x64-windows), arm(arm-windows), arm64(arm64-winodws) windows with
```
.\vcpkg install ffmpeg:x64-windows ffmpeg:x86-windows
.\vcpkg install glew:x86-windows glew:x64-windows
.\vcpkg install glfw3:x86-windows glfw3:x64-windows
.\vcpkg install zlib:x86-windows zlib:x64-windows
.\vcpkg install libpng:x86-windows libpng:x64-windows
```

Finally, create a New Project (or open an existing one) in Visual Studio 2017 or 2015. All installed libraries are immediately ready to be `#include`'d and used in your project.
For CMake projects, simply include our toolchain file. See our [using a package](docs/examples/using-sqlite.md) example for the specifics.
## Tab-Completion / Auto-Completion
`Vcpkg` supports auto-completion of commands, package names, options etc. To enable tab-completion in Powershell, use
```
.\vcpkg integrate powershell
```
and restart Powershell.

Check packages list with
```
.\vcpkg list
```


mkdir build
cd build

cmake .. -G "Visual Studio 15 2017" -DVCPKG_TARGET_TRIPLET=x86-windows -DCMAKE_TOOLCHAIN_FILE="D:/workspace/YUV420P_Player/vcpkg/scripts/buildsystems/vcpkg.cmake"

cmake .. -Bstatic -G "Visual Studio 15 2017" -DVCPKG_TARGET_TRIPLET=x86-windows -DCMAKE_TOOLCHAIN_FILE="D:/workspace/YUV420P_Player/vcpkg/scripts/buildsystems/vcpkg.cmake"
cmake --build static --config Release