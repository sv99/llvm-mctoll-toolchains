llvm-mctoll test toolchains
===========================

Toolchains for cross compilation tests for llvm-mctoll projects on the OSX.

Toolchains grabbed from docker images.

Preparing docker for running arm images. Based on [multiarch/qemu-user-static](https://github.com/multiarch/qemu-user-static)

```bash
# prepare docker
docker run --rm --privileged multiarch/qemu-user-static --reset -p yes
```

Toolchains
----------

Minimal toolchains

```bash
# x86_64-linux-gnu toolchain (for OSX)
docker run --rm -it -v $(pwd):/work -w /work amd64/ubuntu:20.04 bash
# in the docker
apt update
apt install libc6-dev libgcc-9-dev
tar czvf x86_64-linux-gnu.tgz /lib /usr/include /usr/lib/gcc /usr/lib/*linux* /usr/lib/os-release 
```

```bash
# arm-linux-gnueabihf toolchain
docker run --rm -it -v $(pwd):/work -w /work arm32v7/ubuntu:20.04 bash
# in the docker
apt update
apt install libc6-dev libgcc-9-dev
tar czvf arm-linux-gnueabihf.tgz /lib /usr/include /usr/lib/gcc /usr/lib/*linux* /usr/lib/ld-* /usr/lib/os-release --hard-dereference
```

Test build example
------------------

```bash
# ELF 32-bit ARM Linux
clang --sysroot /path_to/toolchain/arm-linux-gnueabihf -target arm-linux-gnueabihf -fuse-ld=lld -o hello-arm -v hello.c
file helllo-arm

# ELF 64-bit x86_64 Linux
clang --sysroot /path_to/toolchain/x86_64-linux-gnu -target x86_64-linux-gnu -fuse-ld=lld -o hello-lin -v hello.c
file helllo-lin
```
