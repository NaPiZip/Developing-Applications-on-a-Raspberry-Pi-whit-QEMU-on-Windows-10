somehow<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/0b/Qt_logo_2016.svg/1200px-Qt_logo_2016.svg.png" alt="Qt_Logo" height="42px" width="42px" align="left">
<img src="https://upload.wikimedia.org/wikipedia/de/thumb/c/cb/Raspberry_Pi_Logo.svg/1200px-Raspberry_Pi_Logo.svg.png" alt="Raspberry_Pi_Logo" height="42px" width="42px" align="left">
<img src="https://proxy.duckduckgo.com/iu/?u=http%3A%2F%2Fcdn.raspberry.tips%2F2015%2F04%2FQemu-logo21.png&f=1" alt="QEMU_Logo" height="42px" width="42px" align="left">
<img src="https://proxy.duckduckgo.com/iu/?u=https%3A%2F%2Fseeklogo.com%2Fimages%2FW%2Fwindows-10-icon-logo-5BC5C69712-seeklogo.com.png&f=1" alt="Windows_10_Logo" height="42px" width="42px" align="left"><br>

# Developing Applications on a Raspberry Pi with QEMU on Windows 10
<div>
    <a href="https://github.com/NaPiZip/Docker_GUI_Apps_on_Windows">
        <img src="https://img.shields.io/badge/Document%20Version-0.0.1-green.svg"/>
    </a>
    <a href="https://www.qemu.org/">
        <img src="https://img.shields.io/badge/QEMU%20x64-3.1.0--rc0-blue.svg"/>
    </a>
    <a href="https://www.microsoft.com">
        <img src="https://img.shields.io/badge/Windows%2010%20x64-10.0.17134%20Build%2017134-blue.svg"/>
    </a>
    <a href="https://downloads.raspberrypi.org/raspbian/images/raspbian-2017-12-01/">
        <img src="https://img.shields.io/badge/Raspbian%20stretch-2018--04--18-blue.svg"/>
    </a>
    <a href="http://gnutoolchains.com/raspberry/">
        <img src="https://img.shields.io/badge/Raspberry--gcc-6.3.0--r3-blue.svg"/>
    </a>
    <a href="http://download.qt.io/archive/qt/5.11/5.11.2/submodules/">
        <img src="https://img.shields.io/badge/qt%20everywhere%20src-5.11.2-blue.svg"/>
    </a>
    <a href="https://www.msys2.org/">
        <img src="https://img.shields.io/badge/MSYS2-x86__64--20180531-blue.svg"/>
    </a>
</div><br>

This is a tutorial showing how to develop Qt applications for the Raspberry Pi 3 cross compiling on Windows, using MYSYS2 with GCC and arm tool chain.
- [QEMU](https://www.qemu.org)
- [Qt-everywhere-src](http://download.qt.io/archive/qt/5.11/5.11.2/submodules/)
- [MSYS2](https://www.msys2.org/)
- [Windows toolchain for Raspberry Pi](http://gnutoolchains.com/raspberry/)

## Installation
1. Download the latest version of Windows toolchain for Raspberry/PI e.g [raspberry-gcc6.3.0-r3](http://gnutoolchains.com/raspberry/) and install it.
2. Download and install [MSYS2](https://www.msys2.org/). I used the `msys2-x86_64-20180531.exe` version, after the successful installation, run MSYS2 and execute the following commands:<br>
    ```
    pacman -Syu
    pacman -S base-devel git mercurial cvs wget p7zip
    pacman -S perl ruby python2 mingw-w64-i686-toolchain mingw-w64-x86_64-toolchain
    ```
3. Set the paths for mingw32 and mingw64. I modified `.bash_profile` file located under `C:\msys64\home\napiZip`. I added the following section in order to make the tool chain for the Raspberry available by adding the Windows toolchain for Raspberry/PI folder containing the `arm-linux-gnueabihf-... .exe` files:
```
PATH="${PATH}:/C/SysGCC/raspberry/bin"
```  
  *Note:<br>
   After setting the path you should be able to run the following commands otherwise your path was not set correctly:*<br>
  ```
   $ arm-linux-gnueabihf-gcc -v

  Using built-in specs.
  COLLECT_GCC=C:\SysGCC\raspberry\bin\arm-linux-gnueabihf-gcc.exe
  COLLECT_LTO_WRAPPER=c:/sysgcc/raspberry/bin/../libexec/gcc/arm-linux-gnueabihf/6/lto-wrapper.exe
  Target: arm-linux-gnueabihf
  Configured with: ../../gcc-6-6.3.0/src/configure -v --with-pkgversion='Raspbian 6.3.0-18+rpi1' --with-bugurl=file:///usr/share/doc/gcc-6/README.Bugs --enable-languages=c,c++ --enable-shared --enable-linker-build-id --without-included-gettext --enable-threads=posix --enable-nls --enable-clocale=gnu --enable-libstdcxx-debug --enable-libstdcxx-time=yes --with-default-libstdcxx-abi=new --enable-gnu-unique-object --disable-libitm --disable-libquadmath --with-system-zlib --disable-browser-plugin --enable-gtk-cairo --with-arch-directory=arm --with-target-system-zlib --enable-objc-gc=auto --enable-multiarch --disable-sjlj-exceptions --with-arch=armv6 --with-fpu=vfp --with-float=hard --enable-checking=release --target=arm-linux-gnueabihf --host=i686-w64-mingw32 --prefix /q/gnu/raspberry/out/ --with-sysroot=/q/gnu/raspberry/out/arm-linux-gnueabihf/sysroot
  Thread model: posix
  gcc version 6.3.0 20170516 (Raspbian 6.3.0-18+rpi1)

  $  g++ -v
  Using built-in specs.
  COLLECT_GCC=C:\msys64\mingw32\bin\g++.exe
  COLLECT_LTO_WRAPPER=C:/msys64/mingw32/bin/../lib/gcc/i686-w64-mingw32/7.3.0/lto-wrapper.exe
  Target: i686-w64-mingw32
  Configured with: ../gcc-7.3.0/configure --prefix=/mingw32 --with-local-prefix=/mingw32/local --build=i686-w64-mingw32 --host=i686-w64-mingw32 --target=i686-w64-mingw32 --with-native-system-header-dir=/mingw32/i686-w64-mingw32/include --libexecdir=/mingw32/lib --enable-bootstrap --with-arch=i686 --with-tune=generic --enable-languages=c,lto,c++,objc,obj-c++,fortran,ada --enable-shared --enable-static --enable-libatomic --enable-threads=posix --enable-graphite --enable-fully-dynamic-string --enable-libstdcxx-time=yes --enable-libstdcxx-filesystem-ts=yes --disable-libstdcxx-pch --disable-libstdcxx-debug --disable-isl-version-check --enable-lto --enable-libgomp --disable-multilib --enable-checking=release --disable-rpath --disable-win32-registry --disable-nls --disable-werror --disable-symvers --with-libiconv --with-system-zlib --with-gmp=/mingw32 --with-mpfr=/mingw32 --with-mpc=/mingw32 --with-isl=/mingw32 --with-pkgversion='Rev2, Built by MSYS2 project' --with-bugurl=https://sourceforge.net/projects/msys2 --with-gnu-as --with-gnu-ld --disable-sjlj-exceptions --with-dwarf2
  Thread model: posix
  gcc version 7.3.0 (Rev2, Built by MSYS2 project)
  ```
4. Install and set up QEMU with a Raspberry image distribution, for details see my repository [here](https://github.com/NaPiZip/Emulating-a-Raspberry-Pi-3-with-QEMU).<br>

## Build Tutorial
This tutorial shows the exact steps needed to generate the build artifacts for the Raspberry Pi 3, as well as how to run applications created with Qt within QEMU.

1. Download the latest version of the Qt source code. I used [qt-everywhere-src-5.11.2](http://download.qt.io/archive/qt/5.11/5.11.2/single/).
2. Extract the file `qt-everywhere-src-5.11.2.zip` into any directory of desire.<br>
3. In order to build for the Raspberry Pi we need to make sure that our `sysroot` directory is up to date. We need to ensure that we are linking against the same libraries and we have the same headers as base. The Windows toolchain for Raspberry Pi offers a handy batch script which supports us, the script is called `UpdateSysroot.bat` and is located in `SysGCC\raspberry\TOOLS`. I used the following settings to connect to QEMU:

  ```
  Host name: 127.0.0.1
  User name: pi  
  ```  
  I downloaded the `sysroot` into the following directory `C:\SysGCC\raspberry\arm-linux-gnueabihf\sysroot`.<br>
  *Note:<br>
  SmartTTY is only able to connect to port 22, if you changed the port routing in QEMU, make sure that 22 is available in order to connect, or use `netsh` to create a port proxy, an example can be found [here](https://parsiya.net/blog/2016-06-07-windows-netsh-interface-portproxy/).*
4. The next step after the successful download of the `sysroot` directory is to configure the build. I used `mingw32.exe` , and changed the directory to `/c/SysGCC/raspberry` in my case. The following section shows the creation of a `qt-build` directory, which is then used for the build artifacts, and then the command in order to configure the Qt build.
  ```
  mkdir qt-build
  cd qt-build

  ../qt-everywhere-src-5.11.2/configure -platform win32-g++ -device linux-rasp-pi3-g++ -release -sysroot C:/SysGCC/raspberry/arm-linux-gnueabihf/sysroot -prefix /usr/local/qt5 -device-option "CROSS_COMPILE=arm-linux-gnueabihf-" -nomake examples -opensource -confirm-license
  ```

5. After the configuration is done successfully we need to execute the build, within the `qt-build` directory, with the following command:<br>

  ```
  mingw32-make
  ```
6. Copy the build artefacts by running:<br>
  ```
  mingw32-make install
  ```
  *Note:<br>
  The files are copied into the `sysroot` directory, the exact location depends on how the Qt build was configured, the prefix specifies the location within the `sysroot` directory, in this case `C:\SysGCC\raspberry\arm-linux-gnueabihf\sysroot\usr\local\qt5`. You should see more sub folder within this directory e.g. `bin`, `include` and `lib`.*
7. Open `SmarTTY.exe`, select the target as defined in step tree and connect. Then run the following command:<br>
  ```
  cd /usr/local
  sudo mkdir qt5
  sudo chown pi qt5
  ```
8. Upload the build artefacts with `SmarTTY.exe`. Select  `SCP`-> `Upload`. I used the following path: `C:\SysGCC\raspberry\arm-linux-gnueabihf\sysroot\usr\local\qt5`

9. Run a test application e.g. `qtdiag` which is located in the `bin` directory by calling:<br>
  ```
  ./qtdiag
  ```

## Problems
Here are some issues I ran into. I am hoping by showing some problems and their solutions you guys can adapt the approaches to your problems.

  - Here is an error which occurred during building after I created the configuration.<br>
  ```
  'C:\SysGCC\raspberry\qt-build\qtbase\bin\moc.exe' -DQT_NO_URL_CAST_FROM_STRING -DQT_NO_INTEGER_EVENT_COORDINATES -DQT_NO_FOREACH -DWTF_EXPORT_PRIVATE= -DJS_EXPORT_PRIVATE= -DENABLE_ASSEMBLER_WX_EXCLUSIVE=1 -DWTFReportAssertionFailure=qmlWTFReportAssertionFailure -DWTFReportAssertionFailureWithMessage=qmlWTFReportAssertionFailureWithMessage -DWTFReportBacktrace=qmlWTFReportBacktrace -DWTFInvokeCrashHook=qmlWTFInvokeCrashHook -DENABLE_LLINT=0 -DENABLE_DFG_JIT=0 -DENABLE_DFG_JIT_UTILITY_METHODS=1 -DENABLE_JIT_CONSTANT_BLINDING=0 -DBUILDING_QT__ -DWTF_USE_UDIS86=0 -DNDEBUG -DQT_NO_NARROWING_CONVERSIONS_IN_CONNECT -DQT_BUILD_QML_LIB -DQT_BUILDING_QT -DQT_NO_CAST_TO_ASCII -DQT_ASCII_CAST_WARNINGS -DQT_MOC_COMPAT -DQT_USE_QSTRINGBUILDER -DQT_DEPRECATED_WARNINGS -DQT_DISABLE_DEPRECATED_BEFORE=0x050000 -DQT_NO_EXCEPTIONS -D_LARGEFILE64_SOURCE -D_LARGEFILE_SOURCE -DQT_NO_DEBUG -DQT_NETWORK_LIB -DQT_CORE_LIB -D__ARM_ARCH_6__ --include C:/SysGCC/raspberry/qt-build/qtdeclarative/src/qml/.moc/moc_predefs.h -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtbase/mkspecs/devices/linux-rasp-pi3-g++ -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtdeclarative/src/qml -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtdeclarative/src/qml/memory -IC:/SysGCC/raspberry/qt-build/qtdeclarative/src/qml -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtdeclarative/src/qml/compiler -IC:/SysGCC/raspberry/qt-build/qtdeclarative/src/qml -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtdeclarative/src/qml/jsruntime -IC:/SysGCC/raspberry/qt-build/qtdeclarative/src/qml -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtdeclarative/src/qml/jit -IC:/SysGCC/raspberry/qt-build/qtdeclarative/src/qml -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtdeclarative/src/qml/debugger -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtdeclarative/src/qml/animations -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtdeclarative/src/3rdparty/masm/jit -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtdeclarative/src/3rdparty/masm/assembler -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtdeclarative/src/3rdparty/masm/runtime -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtdeclarative/src/3rdparty/masm/wtf -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtdeclarative/src/3rdparty/masm/stubs -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtdeclarative/src/3rdparty/masm/stubs/wtf -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtdeclarative/src/3rdparty/masm -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtdeclarative/src/3rdparty/masm/disassembler -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtdeclarative/src/3rdparty/masm/disassembler/udis86 -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtdeclarative/src/qml/.generated -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtdeclarative/include -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtdeclarative/include/QtQml -IC:/SysGCC/raspberry/qt-build/qtdeclarative/include -IC:/SysGCC/raspberry/qt-build/qtdeclarative/include/QtQml -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtdeclarative/include/QtQml/5.11.2 -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtdeclarative/include/QtQml/5.11.2/QtQml -IC:/SysGCC/raspberry/qt-build/qtdeclarative/include/QtQml/5.11.2 -IC:/SysGCC/raspberry/qt-build/qtdeclarative/include/QtQml/5.11.2/QtQml -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtbase/include/QtCore/5.11.2 -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtbase/include/QtCore/5.11.2/QtCore -IC:/SysGCC/raspberry/qt-build/qtbase/include/QtCore/5.11.2 -IC:/SysGCC/raspberry/qt-build/qtbase/include/QtCore/5.11.2/QtCore -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtbase/include -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtbase/include/QtNetwork -IC:/SysGCC/raspberry/qt-build/qtbase/include -IC:/SysGCC/raspberry/qt-build/qtbase/include/QtNetwork -IC:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtbase/include/QtCore -IC:/SysGCC/raspberry/qt-build/qtbase/include/QtCore -I. -Ic:/sysgcc/raspberry/arm-linux-gnueabihf/include/c++/6 -Ic:/sysgcc/raspberry/arm-linux-gnueabihf/include/c++/6/backward -Ic:/sysgcc/raspberry/lib/gcc/arm-linux-gnueabihf/6/include -Ic:/sysgcc/raspberry/lib/gcc/arm-linux-gnueabihf/6/include-fixed -Ic:/sysgcc/raspberry/arm-linux-gnueabihf/include -Ic:/sysgcc/raspberry/arm-linux-gnueabihf/include/arm-linux-gnueabihf/c++/6 -Ic:/sysgcc/raspberry/arm-linux-gnueabihf/sysroot/usr/include/arm-linux-gnueabihf -Ic:/sysgcc/raspberry/arm-linux-gnueabihf/sysroot/usr/include C:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtdeclarative/src/qml/util/qqmladaptormodel.cpp -o .moc/qqmladaptormodel.moc
  mingw32-make[2]: *** [Makefile:12064: .moc/qqmladaptormodel.moc] Error -1073741701
  mingw32-make[2]: Leaving directory 'C:/SysGCC/raspberry/qt-build/qtdeclarative/src/qml'
  mingw32-make[1]: *** [Makefile:53: sub-qml-make_first-ordered] Error 2
  mingw32-make[1]: Leaving directory 'C:/SysGCC/raspberry/qt-build/qtdeclarative/src'
  mingw32-make: *** [Makefile:48: sub-src-make_first] Error 2
  ```
  I used `err.exe` to resolve the error code.
  ```
  C:\Program Files\Microsoft SDKs\Windows\v7.1\Bin>err.exe -1073741701
  # for decimal -1073741701 / hex 0xc000007b
    STATUS_INVALID_IMAGE_FORMAT                                    ntstatus.h
  # {Bad Image}
  # %hs is either not designed to run on Windows or it contains
  # an error. Try installing the program again using the
  # original installation media or contact your system
  # administrator or the software vendor for support.
  # as an HRESULT: Severity: FAILURE (1), FACILITY_NULL (0x0), Code 0x7b
  # for decimal 123 / hex 0x7b
    ERROR_INVALID_NAME                                             winerror.h
  # The filename, directory name, or volume label syntax is
  # incorrect.
  # 2 matches found for "-1073741701"
  ```
  Turns out that the error code `-1073741701 = 0x000007b` is translated to `ERROR_INVALID_NAME`. The reason for this particular error was using double forward slashes `//`, notice also the inconsistent use of them for the -prefix argument `-prefix //usr//local/qt5`:<br>
  ```
  ../qt-everywhere-src-5.11.2/configure.bat -platform win32-g++ -device linux-rasp-pi3-g++ -release -sysroot C://SysGCC//raspberry//arm-linux-gnueabihf//sysroot -prefix //usr//local/qt5 -device-option "CROSS_COMPILE=arm-linux-gnueabihf-" -nomake examples -opensource -confirm-license
  ```

- Here is a different error I encountered so far:<br>
  ```
  pi@raspberrypi:/usr/local/qt5/bin $ ./qtdiag
  Illegal instruction
  ```
  This is the output I get when I execute the application in gdb:<br>
  ```
  and "show warranty" for details.
  This GDB was configured as "arm-linux-gnueabihf".
  Type "show configuration" for configuration details.
  For bug reporting instructions, please see:
  <http://www.gnu.org/software/gdb/bugs/>.
  Find the GDB manual and other documentation resources online at:
  <http://www.gnu.org/software/gdb/documentation/>.
  For help, type "help".
  Type "apropos word" to search for commands related to "word"...
  Reading symbols from qtdiag...(no debugging symbols found)...done.
  (gdb) r
  Starting program: /usr/local/qt5/bin/qtdiag
  [Thread debugging using libthread_db enabled]
  Using host libthread_db library "/lib/arm-linux-gnueabihf/libthread_db.so.1".

  Program received signal SIGILL, Illegal instruction.
  0xb610cb30 in qRegisterResourceData(int, unsigned char const*, unsigned char const*, unsign
  ed char const*) () from /usr/local/qt5/lib/libQt5Core.so.5
  ```

- One major problem I encountered is that for some reason the install path is somehow not correct. Take a look at the `qt.conf`, the `DevicePaths`, `Prefix` and `HostPrefix` are not correct, since they are not absolute and the `HostPath` contains the local drive.<br>
  ```
  [EffectivePaths]
  Prefix=..
  [DevicePaths]
  Prefix=C:/usr/local/qt5
  [Paths]
  Prefix=C:/usr/local/qt5
  HostPrefix=C:/SysGCC/raspberry/arm-linux-gnueabihf/sysrootC:/usr/local/qt5
  Sysroot=C:/SysGCC/raspberry/arm-linux-gnueabihf/sysroot
  SysrootifyPrefix=true
  TargetSpec=devices/linux-rasp-pi-g++
  HostSpec=win32-g++
  [EffectiveSourcePaths]
  Prefix=C:/SysGCC/raspberry/qt-everywhere-src-5.11.2/qtbase
  ```
  This leads to the fact, that some environment variables need to be set in order to run Qt applications, I used the folowing commands to resolve that issue:<br>
  ```
  #!/bin/bash

  export LD_LIBRARY_PATH=/usr/local/qt5/lib
  export QT_QPA_PLATFORM_PLUGIN_PATH=/usr/local/qt5/plugins/platforms
  export QT_QPA_FONTDIR=/user/share/fonts
  sudo ldconfig
  ```
## Special thanks to
I thank [etiennedm](https://forum.qt.io/topic/68381/cross-compile-qt-windows-to-raspberry-3), for an awesome article which I used as a starting point. Also thanks to the people from [visualgdb.com](https://visualgdb.com/tutorials/raspberry/qt/embedded/).

## Contributing, Help, Feedback and Questions
To get started with contributing to my GitHub repository, if you need help or if you have any suggestions, feel free to join my [Slack](https://join.slack.com/t/napi-friends/shared_invite/enQtNDg3OTg5NDc1NzUxLWU1MWNhNmY3ZTVmY2FkMDM1ODg1MWNlMDIyYTk1OTg4OThhYzgyNDc3ZmE5NzM1ZTM2ZDQwZGI0ZjU2M2JlNDU) workspace.
