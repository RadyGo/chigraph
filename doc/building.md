# Building chigraph from source
Currently, the only way to get chigraph is to build from source. Here we go!

## Installing dependencies
The external dependencies of chigraph are:
- A compiler supporting C++14 (clang 3.7+, gcc 4.8+)
- git (to clone chigraph)
- CMake 3.0+
- LLVM 3.7+
- Qt 5.6+
- KDE Frameworks 5

### Linux

#### Ubuntu/Mint:
```bash
apt-get update
apt-get install git llvm-3.8-dev  qtbase5-dev qtdeclarative5-dev qtscript5-dev libqt5x11extras5-dev libqt5svg5-dev cmake extra-cmake-modules gettext libkf5xmlgui-dev

```
If your version doesn't supply new enough llvm versions, use [LLVM's ppa](https://apt.llvm.org)
#### Arch Linux
I personally use Arch for most of chigraph's development, so this is pretty guaranteed to work:
```bash
sudo pacman -Sy
sudo pacman -S llvm qt5 kf5 cmake git
```
#### Other distributions
If you're not sure how to install qt5 and llvm on your system, then download the prebuilt binaries for each.

You can get qt binaries [here](https://download.qt.io/archive/qt/5.8/5.8.0/) and LLVM binaries [here](http://releases.llvm.org/download.html#3.9.1) (use the "Clang for x86_64 Ubuntu 16.04" download)

If you can't get ahold of a binary package of KDE Frameworks 5, run `./scripts/build_frameworks.sh` to build on in tree (automatically configured to work)


## Compiling Chigraph
In order to compile chigraph, first generate the project files with cmake:
```bash
git clone https://github.com/chigraph/chigraph --recursive 
mkdir chigraph/build
cd chigraph/build
cmake .. -DCMAKE_BUILD_TYPE=<Release|Debug> \
	-G<Your Generator, Use 'Ninja' or 'Unix Makefiles'> \
	-DCMAKE_PREFIX_PATH=<Qt install prefix> \
	-DLLVM_CONFIG=<path to llvm-config of the llvm-version you want to use> \
	-DCLANG_EXE=<path to clang> \
	-DLLI_EXE=<path to lli>
```

You can leave out the last 4 parameters if they are all installed in a standard location.

Some example CMake calls:

Debug with all default locations (works on arch, ubuntu yakkety):
```bash
cmake .. -DCMAKE_BUILD_TYPE=Debug
```
Debug using the LLVM PPAs:
```bash
cmake .. -DCMAKE_BUILD_TYPE=Debug \
	-DLLVM_CONFIG=/usr/lib/llvm-3.9/bin/llvm-config \
	-DCLANG_EXE=/usr/lib/llvm-3.9/bin/clang \
	-DLLI_EXE=/usr/lib/llvm-3.9/bin/lli
```


Then, you can compile it!
```
cmake --build .
```

To test that everything is sane and working, it's a good idea to quickly run the tests (it only takes a few seconds)
```bash
cmake --build . --target test
```

Any errors? [File an issue!](https://github.com/chigraph/chigraph/issues/new)