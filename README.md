This repo contains a prebuilt [Skia](https://skia.org/) m76 library for Ubuntu 16.04.6 x86_64
used by Travis CI.

## Dependencies

```sh
sudo apt-get install git libfontconfig1-dev

# Instal clang 7, because Ubuntu 16.04 uses 3.7, which is too old.
echo 'deb http://apt.llvm.org/xenial/ llvm-toolchain-xenial-7 main' | sudo tee -a /etc/apt/sources.list
echo 'deb-src http://apt.llvm.org/xenial/ llvm-toolchain-xenial-7 main' | sudo tee -a /etc/apt/sources.list
wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key|sudo apt-key add -
sudo apt-get update
sudo apt-get install clang-7 lldb-7 lld-7

# Download ninja, because Ubuntu 16.04 uses 1.5, which is too old.
wget https://github.com/ninja-build/ninja/releases/download/v1.8.2/ninja-linux.zip
unzip ninja-linux.zip -d .
```

## Build

```sh
git clone https://skia.googlesource.com/skia.git
cd skia
git fetch --all
git checkout -b m76 origin/chrome/m76
python tools/git-sync-deps
bin/gn gen out/Shared --args='
    is_official_build=false
    is_component_build=true
    skia_enable_discrete_gpu=false
    skia_enable_gpu=false
    skia_enable_particles=false
    skia_enable_pdf=false
    skia_enable_skottie=false
    skia_enable_skshaper=false
    skia_enable_tools=false
    skia_use_libheif=false
    skia_use_libwebp=false
    skia_use_lua=false
    skia_use_piex=false
    skia_use_system_libjpeg_turbo=false
    is_debug=false
    cc="clang-7"
    cxx="clang++-7"
    werror=false'
../ninja -C out/Shared
```
