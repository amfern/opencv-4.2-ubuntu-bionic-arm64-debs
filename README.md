# Precompiled opencv debian package for ubuntu 18.04 bionic

This project uses buildx to build open cv pacakge for arm64.

On Arch linux install binfmt-qemu-static and qemu-user-static from the AUR on the x86_64 machine/host. binfmt-qemu-static will take care of registering the qemu binaries to binfmt service.
-- or use docker muliarch for this --
docker run --rm --privileged multiarch/qemu-user-static --reset -p yes

the following command should return alot of supported platforms
docker buildx inspect --bootstrap

## compile:
```bash
make release-arm64-tar # raspberrypi 4
make release-amd64-cuda-tar # any x86 with nvidia card
make release-arm64-cuda-l4t-tar # nvidia jetson tx2(can be built only on nvidia jetson)
```
l4t jetson image must be build on jetson, with nvidia runtime enabled as default, as nvidia doesn't provide `nvcr.io/nvidia/cuda-arm64` container with 10.2 cuda.
so libcuda.so and other libcublas.so must be mounted by the runtime
https://forums.developer.nvidia.com/t/libcublas-file-size-is-0-in-jetson-docker-image/180676

## installation
Download the release tar from releases, untar and install with apt
```bash
apt install -f ./*.deb
```

If you encounter conflicts, either remove the current installation of opencv or overwrite the files with this command, although it is strongly 
discouraged to overwriting files
```
sudo apt -o Dpkg::Options::="--force-overwrite" install -f ./*.deb
```
