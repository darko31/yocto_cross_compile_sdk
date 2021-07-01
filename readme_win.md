Toolchain for Windows - note this is tested on Poky Thud 2.6.1 version

1. Setup and build the image for your target device
2. Download meta-mingw and add it to your bblayers, available at 

    >git://git.yoctoproject.org/meta-mingw

4. Modify settings in your local.conf file, add line 

    >SDKMACHINE ?= "x86_64-mingw32"

4. Build the toolchain with 

    >bitbake meta-toolchain

5. Transfer generated SDK located at build/tmp/deploy/sdk/<sdk>.xz to your Windows machine.
6. I've had some issues with the environment setup file, when using CMake, so I've replaced lines at the top of the file.
  Replaced this
      
    >set SDKROOT=%~sdp0%
  
    >set SDKTARGETSYSROOT=%SDKROOT%\sysroots\aarch64-poky-linux
  
    with this

    >set SDKROOT=%~dp0%
    
    >set SDKROOT=%SDKROOT:\=/%

    >set SDKTARGETSYSROOT=%SDKROOT%/sysroots/aarch64-poky-linux
  
    It may be a good idea to repack the SDK with these changes for later distribution.
    Okay, now SDK is ready, it can be used directly or you can call environment.bat file to setup all the necessary env variables.

7. If you are using CMake, make sure that Win machine has installed CMake and make.
   Call environment.bat to setup env vars and call cmake with following arguments in order to use Unix makefiles
    
    > cmake .. -DCMAKE_TOOLCHAIN_FILE=../toolchain.txt -G "Unix Makefiles"
    
    Example toolchain.txt is available here in the repo.
  
8. After that, call make to cross compile your project and transfer the generated binary to your target board.
   
