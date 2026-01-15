# imgui-impl-ctr

ImGui for Nintendo 3ds...

## Building

For building the example do

```bash
# Linux / Mac
cmake -B build -DIMGUI_CTR_BUILD_EXAMPLE=ON --toolchain $DEVKITPRO/cmake/3DS.cmake
# Windows (PowerShell)
cmake -B build -DIMGUI_CTR_BUILD_EXAMPLE=ON --toolchain $ENV:DEVKITPRO/cmake/3DS.cmake
# Building
cmake --build build
```

## Usage (in your Project)

### Submodule and CMake

```bash
git submodule add https://github.com/npid7/imgui-impl-ctr
```

> [!CAUTION]
> Not every ImGui Version is supported yet. `v1.90.9` and `v1.90.9-docking` are the last
> versions i confirmed as working. As this Project started at `v1.87` i cannot confirm any version
> outside of `v1.87` to `v1.90.9` as working.

First but this into your `CMakeLists.txt`

```cmake
# Set the version BEFORE the add_subdirectory (if you need to change it)
set(IMGUICTR_IMGUI_VERSION "v1.90.9" CACHE STRING "" FORCE)
# Path to the submodule
add_subdirectory(imgui-impl-ctr)

# Linking
target_link_libraries(${PROJECT_NAME} PRIVATE imgui-ctr)
```

### Makefile

First you need to build and install the library [look here](#building)

#### The default way
if you execute the following, the library will be installed into portlibs

```bash
cmake --install build 
```

#### The alternative way

```bash
# Configure                                 This part depends on your os           Installation path
cmake -B build -DIMGUI_CTR_BUILD_EXAMPLE=ON --toolchain $DEVKITPRO/cmake/3DS.cmake -DCMAKE_INSTALL_PREFIX=inst
cmake --build build
cmake --install build
```

With this you can grap the `include` and `lib` folder out of inst and put it into yout project into a directory called libs.

#### Editing your makefile

With this you can simply add `imgui` and `imgui-ctr` to the libs in the folowing order

```makefile
LIBS := imgui-ctr imgui citro3d ctru m

# Alternitive way needs to add your libs dir into LIBDIRS
LIBDIRS := $(CURDIR)/libs $(PORTLIBS) $(CTRULIB)
```

## Credits
- [tobid7](https://github.com/tobid7) : Lead developer, Adding TTF support
- [mtheall](https://github.com/mtheall) : ftpd's [imgui code](https://github.com/mtheall/ftpd/tree/master/source/3ds) (base of this project)
- [devkitpro](https://github.com/devkitPro) : [libctru](https://github.com/devkitPro/libctru), [citro3d](https://github.com/devkitPro/citro3d)