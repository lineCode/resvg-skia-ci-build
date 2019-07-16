This repo contains a prebuilt [Skia](https://skia.org/) m76 library for Ubuntu 16.04.6 x86_64
used by Travis CI.

## Build

Was built on Gentoo Stable.

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
    skia_use_harfbuzz=false
    skia_use_icu=false
    skia_use_libheif=false
    skia_use_libwebp=false
    skia_use_lua=false
    skia_use_piex=false
    is_debug=false
    cc="clang"
    cxx="clang++"
    werror=false'
ninja -C out/Shared
```
