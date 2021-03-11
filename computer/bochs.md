[TOC]
download bochs source code.
brew install sdl
tar bochs source code
cd bochs
```
./configure --enable-ne2000 \
            --enable-all-optimizations \
            --enable-cpu-level=6 \
            --enable-x86-64 \
            --enable-vmx=2 \
            --enable-pci \
            --enable-usb \
            --enable-usb-ohci \
            --enable-e1000 \
            --enable-debugger \
            --enable-disasm \
            --disable-debugger-gui \
            --with-sdl \
            --prefix=$HOME/opt/bochs
```
```
make
make install
```
vim ~/.zprofile
```
export BXSHARE="$HOME/opt/bochs/share/bochs"
export PATH="$PATH:$HOME/opt/bochs/bin"
```