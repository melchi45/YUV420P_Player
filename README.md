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

## Additional OpenGL library
### Windows
On Windows you need to include the gl.h header for OpenGL 1.1 support and link against OpenGL32.lib. Both are a part of the [Windows SDK](http://msdn.microsoft.com/en-us/windows/bb980924.aspx). In addition, you might want the following headers which you can get from [http://www.opengl.org/registry](http://www.opengl.org/registry) .

[<GL/glext.h>](https://www.opengl.org/registry/api/GL/glext.h) - OpenGL 1.2 and above compatibility profile and extension interfaces..

[<GL/glcorearb.h>](https://www.opengl.org/registry/api/GL/glcorearb.h) - OpenGL core profile and ARB extension interfaces, as described in appendix G.2 of the OpenGL 4.3 Specification. Does not include interfaces found only in the compatibility profile.

[<GL/glxext.h>](https://www.opengl.org/registry/api/GL/glxext.h) - GLX 1.3 and above API and GLX extension interfaces.

[<GL/wglext.h>](https://www.opengl.org/registry/api/GL/wglext.h) - WGL extension interfaces.
### Linux
On Linux you need to link against libGL.so, which is usually a symlink to libGL.so.1, which is yet a symlink to the actual library/driver which is a part of your graphics driver. For example, on my system the actual driver library is named libGL.so.256.53, which is the version number of the nvidia driver I use. You also need to include the gl.h header, which is usually a part of a Mesa or Xorg package. Again, you might need glext.h and glxext.h from http://www.opengl.org/registry . glxext.h holds GLX extensions, the equivalent to wglext.h on Windows.

If you want to use OpenGL 3.x or OpenGL 4.x functionality without the functionality which were moved into the GL_ARB_compatibility extension, use the new gl3.h header from the registry webpage. It replaces gl.h and also glext.h (as long as you only need core functionality).

Last but not the least, glaux.h is not a header associated with OpenGL. I assume you've read the awful NEHE tutorials and just went along with it. Glaux is a horribly outdated Win32 library (1996) for loading uncompressed bitmaps. Use something better, like libPNG, which also supports alpha channels.

## Build

```
mkdir build
cd build

cmake .. -G "Visual Studio 15 2017" -DVCPKG_TARGET_TRIPLET=x86-windows -DCMAKE_TOOLCHAIN_FILE="D:/workspace/YUV420P_Player/vcpkg/scripts/buildsystems/vcpkg.cmake"
```
### Build without IDE Tools
```
cmake .. -Bstatic -G "Visual Studio 15 2017" -DVCPKG_TARGET_TRIPLET=x86-windows -DCMAKE_TOOLCHAIN_FILE="D:/workspace/YUV420P_Player/vcpkg/scripts/buildsystems/vcpkg.cmake"
cmake --build static --config Release
```