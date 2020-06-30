This repo contains a prebuilt [Skia](https://skia.org/) m85 library for Ubuntu 18.04.3 x86_64
used by Travis CI.

## Dependencies

```sh
sudo apt-get install git ninja-build libfontconfig1-dev

# Install clang 9, because Ubuntu 18.04 uses clang 6, which is too old.
echo 'deb http://apt.llvm.org/bionic/ llvm-toolchain-bionic-9 main' | sudo tee -a /etc/apt/sources.list
echo 'deb-src http://apt.llvm.org/bionic/ llvm-toolchain-bionic-9 main' | sudo tee -a /etc/apt/sources.list
wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key|sudo apt-key add -
sudo apt-get update
sudo apt-get install clang-9 lldb-9 lld-9
```

## Build

```sh
git clone https://skia.googlesource.com/skia.git
cd skia
git fetch --all
git checkout -b m85 origin/chrome/m85
python tools/git-sync-deps
bin/gn gen out/Shared --args='
    is_official_build=false
    is_component_build=true
    paragraph_gms_enabled=false
    paragraph_tests_enabled=false
    skia_enable_android_utils=false
    skia_enable_discrete_gpu=false
    skia_enable_gpu=false
    skia_enable_nvpr=false
    skia_enable_particles=false
    skia_enable_pdf=false
    skia_enable_skottie=false
    skia_enable_skrive=false
    skia_enable_skshaper=false
    skia_enable_tools=false
    skia_use_gl=false
    skia_use_harfbuzz=false
    skia_use_icu=false
    skia_use_libgifcodec=false
    skia_use_libheif=false
    skia_use_libjpeg_turbo_decode=false
    skia_use_libjpeg_turbo_encode=false
    skia_use_libwebp_decode=false
    skia_use_libwebp_encode=false
    skia_use_lua=false
    skia_use_piex=false
    is_debug=false
    cc="clang-9"
    cxx="clang++-9"
    werror=false'
ninja -C out/Shared
```
