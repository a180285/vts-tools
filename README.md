# VTS-Tools

[VTS-Tools](https://github.com/melown/vts-tools) is set of command line
programms used in Melown 3D rendering stack (server-side) for data preparation
and manipulation.

## User documentation

VTS-Tools user documentation is being created in [separate
project](https://github.com/melown/workshop) and can be seen
on https://vts-geospatial.org

## Download, build and install

You basically need just 2 steps to get VTS-Tools installed: `git clone` the
source code from the repository and `make` it. But there are some tricky parts
of the process, so read carefully following compilation howto.

### Dependencies

#### Basic deps

Make sure, you have `cmake` and `g++` installed, before you try to compile
anything.

```
sudo apt-get update
sudo apt-get install cmake g++
```

#### VTS dependencies

**Note**: ubuntu 20.04 use following command to add source
```
# You can Install Registry from Melown repository
wget -O - http://cdn.melown.com/packages/keys/oss.packages%40melown.com.key | sudo apt-key add
cd /etc/apt/sources.list.d/ && sudo wget http://cdn.melown.com/packages/conf/melown-focal.list
```

```bash
# add gpg key.
echo '
-----BEGIN PGP PUBLIC KEY BLOCK-----

mQENBFlnN2MBCADCWSlApTuqIMzx3LRRrP/isby7cCuKXbNDeNzmn3jLA3LH+OYD
v4jP/BLxFVYuobmgvaue3TgPkpRxtbaFSRcp+vBI204+UEepyBi7CQQ1HrzIufab
OVkprkisvFxkLsvzbgkp/4347U61zxdzlj7MDtlqCHJHX3daznUk7IjPrBsQxFbr
R+GubwkC1tRNFSwiZpA/8pQpq4ZZVuXObusbobH1CfHz6WxB4KRB3F+C/nszzEYn
hNwh30G9gb02EfGucj703K4HR/mB6OZDU+hjHa+PhJtWsTZQtjN71PGWdzTQTZz3
gKsEmxminttEWaqUeCWw5dKUVFg5zo6UDY4fABEBAAG0LU1lbG93biBPU1MgUGFj
a2FnZXMgPG9zcy5wYWNrYWdlc0BtZWxvd24uY29tPokBOAQTAQIAIgUCWWc3YwIb
AwYLCQgHAwIGFQgCCQoLBBYCAwECHgECF4AACgkQ+y0WIAMBJqQ9ywf9EjEH/rmf
eBjvysjYCc9RXr5WazOtkeB9z2DwIVFJE2zvfTh6fUWM+AobO9gzbq//R5tL44eM
bkU9GEOHovybE848bZUbw+TU1HVsj99mjzNPqw9Vmz7SgegIUAOd0/628Vhx2Qd8
VJzteGfpL/2zPXX8hIEhfm9InlPypFQgnj99Zdq+wopRF2ZbxHTb00v/cUOqioyB
lYtnS32NML2nY+714zrrn5tdpjnPtQmXOks4EubPvDp7iR2H7SVfgio2owuEpEfQ
TsL4liVMPNl/TMEya1ymSkcNVysXS05dooS/cqorYOFH6L3p9k0KFwXwiunRV+oH
QQ701odVh1okrbkBDQRZZzdjAQgAsy0T4xfZXcTpNMbM0VBiso4QSPaJeoNptlXX
harsxXxHk9SpgiwpOj8wjc8Rls+rNnUufBIeA4GTpiqXW8BXEawGoA8H6F/rGYat
OR+jogx4IcZg0+v33FAH/vB7mQBXJVbtVcUYeHufYRo+pO1UPDDEhJ3PUPeiYSVV
FxN3Rq+HqTX6kHupTRLicSzlLJqehb/KZCVJ/rZnadMC1t1tYpvx6U3/FPmg7VNb
Higcy3PpQBOwFfo/0nt1Oka3HzmZnU9HT8oDd5WiZhLLU+e6mLIkIudhswFjq+jj
sD+NakfyHoTMmRVejpIQw1I1wrm58FWP1p2jREB9GKkIeVGN4wARAQABiQEfBBgB
AgAJBQJZZzdjAhsMAAoJEPstFiADASakeOIH/Rrk3CpajqsnlGSq33snhAG4j4Z8
BagedUUD00SOlELlNqeqQQJFA5nxyD2q5TLdogVPbzprb5L5+7Nzx6hBvi9bBnPR
Gc05moy++ivhGRY1VqZEN3s3Ll7OaxFNxJTC7vunhRkw/XEHxzOo199vCPRtXWY5
Wd37rDs1KL16hpqubYbKeuqj+attC03/KkwxpIVzUe5JI6NUDZUxUfa5riYSxBXU
YKSxv/zsEunQeftPm6AQ0E2hNyfA/qQtOiXQvE+QTnHMSruq+D7bg9sR96AAJwCG
X6+oVkdu9e3yyvrqBQvEafqAGWJnHhTKSvaDvDR11arQUCUBcVbuIkDu2t4=
=UcdF
-----END PGP PUBLIC KEY BLOCK-----
' | sudo apt-key add
```

Before you can run [VTS-Tools](https://github.com/melown/vts-tools), you
need at least [VTS-Registry](https://github.com/melown/vts-registry) downloaded
and installed in your system. Please refer to related
[README.md](https://github.com/Melown/vts-registry/blob/master/README.md) file,
about how to install and compile VTS-Registry.

#### Unpackaged deps

[VTS-Tools](https://github.com/melown/vts-tools) is using (among other
libraries) [OpenMesh](https://www.openmesh.org/). You have to download and
install OpenMesh library and this is, how you do it

```
git clone --recursive https://www.graphics.rwth-aachen.de:9000/OpenMesh/OpenMesh.git
cd OpenMesh
mkdir build
cd build
cmake ..
make -j4
sudo make install
```

#### Installing packaged dependencies

Now we can download and install rest of the dependencies, which are needed to
get VTS-Tools compiled:

```
sudo apt-get install \
    libboost-dev \
    libboost-thread-dev \
    libboost-program-options-dev \
    libboost-filesystem-dev \
    libboost-regex-dev \
    libboost-iostreams-dev\
    libboost-python-dev \
    libopencv-dev libopencv-core-dev libopencv-highgui-dev \
    libopencv-photo-dev libopencv-imgproc-dev libeigen3-dev libgdal-dev \
    libproj-dev libgeographic-dev libjsoncpp-dev \
    libprotobuf-dev protobuf-compiler libprocps-dev libmagic-dev gawk sqlite3 \
    libassimp-dev \
    libtinyxml2-dev
```

### Clone and Download

The source code can be downloaded from
[GitHub repository](https://github.com/a180285/vts-tools), but since there are
external dependences, you have to use `--recursive` switch while cloning the
repo.


```
git clone --recursive https://github.com/a180285/vts-tools.git
```

**NOTE:** Otherwise you should clone submodule by yourself, like this:
```bash
git submodule update --init --recursive
```

**NOTE:** If you did clone from GitHub previously without the `--recursive`
parameter, you should probably delete the `vts-tools` directory and clone
again. The build will not work otherwise.


### Configure and build

For building VTS-Tools, you just have to use ``make``

```
cd tools
mkdir build
cd build
cmake ..
make -j4 # to compile in 4 threads
```

### Installing

Default target location (for later `make install`) is `/usr/local/` directory.
You can set the `CMAKE_INSTALL_PREFIX` variable, to change it:

```
make set-variable VARIABLE=CMAKE_INSTALL_PREFIX=/install/prefix
```

You should see compilation progress. Depends, how many threads you allowed for
the compilation (the `-jNUMBER` parameter) it might take couple of minutes to an
hour of compilation time.

The binaries are then stored in `bin` directory. Development libraries are
stored in `lib` directory.

You should be able to call `make install`, which will install to either defaul
location `/usr/local/` or to directory defined previously by the
`CMAKE_INSTALL_PREFIX` variable (see previous part).

When you specify the `DESTDIR` variable, resulting files will be saved in
`$DESTDIR/$CMAKE_INSTALL_PREFIX` directory (this is useful for packaging), e.g.

```
make install DESTDIR=/home/user/tmp/
```

## Install from Melown repository

We provide precompiled packages for some popular linux distributions. See [Melown OSS package repository
](http://cdn.melown.com/packages/) for more information. This repository contains all needed packages to run
VTS OSS software.

## How to contribute

Check the [CONTRIBUTING.md](CONTRIBUTING.md) file.
