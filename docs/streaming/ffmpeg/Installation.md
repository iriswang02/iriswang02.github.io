# MinGW/FFmpeg installation guide



### Installing Requirements

##### MinGW "mainline"

Go to [http://www.mingw.org](http://www.mingw.org/) and look for the "Download" page.

The recommended way to install MinGW/MSys is through the automated installer, `mingw-get-setup.exe`.

When basic core packages were downloaded, please add 4 more packages:

- mingw-developer-toolkit
- mingw32-base
- mingw32-gcc-g++
- msys-base

Then click `apply all changes`, and wait for installation.

##### YASM

Go to http://yasm.tortall.net/ and look for the "Download" page.

1. Download `Win64 VS2010.zip`,  create a copy of the .exe file in `./MinGW/msys/1.0/bin` and rename it to `yasm.exe`, so it can be invoked by using simply `yasm`.
2. **OR** you may directly install `YASM` in `msys`. The method is the same as installation in Linux system.

##### NASM

Go to https://www.nasm.us/ and download the latest stable release. Copy `nasm.exe` to `./MinGW/msys/1.0/bin`.

##### SDL

SDL is required for ffplay and the SDL output device. 

Go to https://www.libsdl.org/index.php and look for the latest version.

1. Download and copy all files in `/lib`, `/bin`, `/include`, `/share` to the corresponding directory under `./MinGW/msys/1.0`.
2. **OR** you may directly install `SDL` in `msys`. The method is the same as installation in Linux system.

##### x264

Go to http://www.videolan.org/developers/x264.html and download source code. Unzip all files in `./MinGW/msys/1.0/home/user/` and install it. The method is the same as installation in Linux system.

If error `make: *** [libx264.a] Error 5` occurs:

Edit the following lines in `configure`

```shell
if ${cross_prefix}gcc-ar --version >/dev/null 2>&1; then
    #AR="${AR-${cross_prefix}gcc-ar}" - Original
    AR="${AR-${cross_prefix}ar}"  # - Edited
else
    AR="${AR-${cross_prefix}ar}"
fi
if ${cross_prefix}gcc-ranlib --version >/dev/null 2>&1; then
    #RANLIB="${RANLIB-${cross_prefix}gcc-ranlib}" - Original
    RANLIB="${RANLIB-${cross_prefix}ranlib}" # - Edited
else
    RANLIB="${RANLIB-${cross_prefix}ranlib}"
fi
```

##### msys.bat

Add the following line after `@echo off`:

`Call "path/to/vcvars32.bat"`

##### link.exe

Find `link.exe` in `./MinGW/msys/1.0/bin/` and rename it to avoid clash with 

VC's `link.exe`. You may revert it after compilation.

##### pkg-config/glib

pkg-config: http://ftp.gnome.org/pub/gnome/binaries/win32/dependencies/pkg-config_0.23-3_win32.zip

glib: http://ftp.gnome.org/pub/gnome/binaries/win32/glib/2.18/glib_2.18.4-1_win32.zip

Unzip and copy `pkg-config.exe` and `libglib-2.0-0.dll` to `./MinGW/bin/`

##### Config pkg-config

Add the following lines after `if [$MSYSTEM == MINGW32]; then ... fi` in `./MinGW/msys/1.0/etc/profile`:

```shell
if [ -z “$PKG_CONFIG” ]; then
export PKG_CONFIG=path/to/pkg-config.exe 
fi
if [ -z “$PKG_CONFIG_PATH” ]; then
export PKG_CONFIG_PATH=MinGW/lib/pkgconfig:/usr/local/lib/pkgconfig
fi
```

##### Compile

Open `msys.bat` under `./MinGW/msys/1.0`. 

Navigate to the directory of ffmpeg, and run

```shell
#1
./configure --enable-shared --enable-yasm --enable-libx264 --enable-gpl --enable-sdl --prefix=./vs2017_build
#2
make all
#3
make install
```



*Don't forget to copy all `.dll` files (including `libx264-161.dll` and `SDL2.dll`) to the folder where you want to run .exe file.*