# **LibreELEC USB/SD Creator**

This is a lightweight image writing app with a simple four-step GUI for creating LibreELEC USB/SD Card installation media on Linux, macOS and Windows. It automatically displays the currently available downloads (defaulting to the latest release) and detects OS locale to switch the GUI to a matching local language.

## Translation Changes

Changes to master language (en_GB) strings can be submitted via pull request to this GitHub repo. English strings are periodically synchronised to a Transifex project: https://www.transifex.com/libreelec/libreelec-usb-sd-creator allowing contributors to translate them into other languages. Translated strings are periodically synchronised from Transifex back to this repo for inclusion in the next release.

Pull requests for all other languages will be rejected. You will be asked to submit changes via Transifex.

## Translation Languages

Requests for new languages should be made on Transifex. Once a new language has been approved it will be added to the translation project where you can conribute translated strings.

Please note: languages are translated by people (you) not magic!

## Issues and Support

Issues should be reported via the forum here: https://forum.libreelec.tv/board/41-usb-sd-creator-support

# **How to compile the USB/SD Creator**

Build instructions are supplied for Windows x64 (Installer and Portable) and MacOS (Intel and Apple Silicon). Instructions tested locally on Mac Sonoma and Windows 11.

It is possible to build for Linux, but for now instructions are TBD.

# Windows

## Install pre-requisites

### 1. Install 7zip
https://www.7-zip.org/download.html

### 2. Install Git
https://git-scm.com/download/win

### 3. Install CMake
https://cmake.org/download, select windows x64 installer and "Add cmake to PATH".

### 4. Install Python
https://www.python.org/downloads/windows/ select Windows installer (64-bit) for download
at install select "Add Python to PATH"

### 5. Install Inno Setup (Compiler for Installer)

https://jrsoftware.org/isdl.php, download and install the latest stable version.

#### 5. Add 7-Zip to PATH variable

`C:\Program Files\7-Zip`

## Building for Windows using MSys2 and MinGW

### 1. Install msys2 and MinGW

Install the latest msys2 via the installer (not the base installer): https://repo.msys2.org/distrib/x86_64/

After the install, from the resulting msys2 console install mingw:

```
pacman -S mingw-w64-x86_64-cmake mingw-w64-x86_64-gcc mingw-w64-x86_64-ninja mingw-w64-x86_64-zlib mingw-w64-x86_64-qt6-base mingw-w64-x86_64-qt6-tools
```

Add msys2 to PATH: `C:\msys64\mingw64\bin`. Then do a restart of windows.

Note: you must use a standard commnd prompt when using mingw for the build.

### 2. Clone Git Repo
Clone the repository to `%UserProfile%/usb-sd-creator`
`git clone https://github.com/LibreELEC/usb-sd-creator.git`

### 3. Build USB-SD-Creator

Assuming the repo is in your home directory

```
cd %UserProfile%/usb-sd-creator
```

#### Debug build

```
cmake -S . -B build -G Ninja && cmake --build build
```

#### Release build
```
cmake --preset release-ninja && cmake --build --preset release
```

### 4. Build Installer

#### Debug build

```
cd build
cpack -C Debug
```

#### Release build

##### Create installer

```
cpack --preset release
```

##### Create Zip for portable installs (UWP)

```
cpack --preset release -G ZIP
```

### 5. Run USB-SD-Creator

Run the installer in `build/cpack`. Then run the app from Start Menu: `LibreELEC USB-SD Creator x64`.

## Building for Windows using Visual Studio (MSVC)

### 1. Install Qt 6.7.2 and Visual Studio Community 2022

#### Install required Qt packages

To see the available Qt versions run:

```
aqt list-qt windows desktop
```

Install the required packages:

```
aqt install-qt --outputdir ~/Qt windows desktop 6.7.2 win64_msvc2019_64 --archives qtbase qttools opengl32sw d3dcompiler_47 --external 7z.exe
aqt install-qt --outputdir ~/Qt windows desktop 6.7.2 win64_msvc2019_64 --modules debug_info --external 7z.exe
```

#### Install Visual Studio Community 2022

Install Visual Studio Community 2022 from: https://visualstudio.microsoft.com/vs/community/. Note that you only require the Desktop C++ package which you can select in the online installer.

Note that the command prompt is to be used thoughtout the MSVC build must be started from a standard command prompt for `x64` as follows: `"C:/Program Files/Microsoft Visual Studio/2022/Community/Common7/Tools/VsDevCmd.bat" -arch=x64 -host_arch=x64`.

### 2. Clone Git Repo
Clone the repository to `%UserProfile%/usb-sd-creator`
`git clone https://github.com/LibreELEC/usb-sd-creator.git`

### 3. Build USB-SD-Creator

Assuming the repo is in your home directory

```
cd %UserProfile%/usb-sd-creator
```

#### Debug build

```
cmake -S . -B build -D CMAKE_PREFIX_PATH="%UserProfile%/Qt/6.7.2/msvc2019_64" && cmake --build build
```

#### Release build

```
cmake --preset release-msvc -D CMAKE_PREFIX_PATH="%UserProfile%/Qt/6.7.2/msvc2019_64" && cmake --build --preset release
```

### 4. Build Installer

#### Debug build

```
cd build
cpack -C Debug
```

#### Release build

##### Create installer

```
cpack --preset release
```

##### Create Zip for portable installs (UWP)

```
cpack --preset release -G ZIP
```

### 5. Run USB-SD-Creator

Run the installer in `build/cpack`. Then run the app from Start Menu: `LibreELEC USB-SD Creator x64`.

# MacOS

### Building for MacOS

### 1. Install XCode with Command-line tools

### 2. Setup Qt 6.7.2

#### Install pre-requisites

The build requires both `python3` and `cmake`. If you don't have them installed, run the following commands:

```
brew install python
brew install cmake
```

Now install `aqt`, a command line package manager for `Qt`:

```
pip3 install aqtinstall --break-system-packages
```

#### Install required Qt packages

To see the available Qt versions run:

```
aqt list-qt mac desktop
```

Install the required packages:

```
aqt install-qt --outputdir ~/Qt mac desktop 6.7.2 --archives qtbase qttools
aqt install-qt --outputdir ~/Qt mac desktop 6.7.2 --modules debug_info

```

### 4. Build USB-SD-Creator

Assuming the repo is in your home directory

```
cd ~/usb-sd-creator
```

#### Debug build
```
cmake -S . -B build -D CMAKE_PREFIX_PATH="/Users/$USER/Qt/6.7.2/macos" && cmake --build build
```

#### Release build
```
cmake --preset release -D CMAKE_PREFIX_PATH="/Users/$USER/Qt/6.7.2/macos" && cmake --build --preset release
```

### 5. Run USB-SD-Creator

#### Open the app

Simply double click the app from a finder window in the `build` folder in the repo: `build/LibreELEC USB-SD Creator`

#### Command line

Run the app from the command line, that will prompt for a password:
```
./build/LibreELEC\ USB-SD\ Creator.app/Contents/MacOS/LibreELEC\ USB-SD\ Creator
```

**Or:**

Run the app from the command line using sudo

```
sudo ./build/LibreELEC\ USB-SD\ Creator.app/Contents/MacOS/LibreELEC\ USB-SD\ Creator
```

### 6. Debugging USB-SD-Creator

#### Using Qt Creator

If you need to, install Qt Creator:

```
brew install --cask qt-creator
```

Then simply open CMakeLists.txt in Qt Creator

#### Using XCode

Build the xcode project, and open the project file in Xcode, located in the build folder (note you may need to clear any previous build files before genreating for XCode):

```
cmake -S . -B build -G Xcode -D CMAKE_PREFIX_PATH="/Users/$USER/Qt/6.7.2/macos" && cmake --build build
```


