# Build instructions (Linux)

## Prerequisites

* Python 2.7.x. Some distributions like CentOS still use Python 2.6.x
so you may need to check your Python version with `python -V`.
* Node.js v0.12.x. There are various ways to install Node. One can download
source code from [Node.js] (http://nodejs.org) and compile from source.
Doing so permits installing Node to your own home directory as a standard user.
Or try repositories such as [NodeSource] (https://nodesource.com/blog/nodejs-v012-iojs-and-the-nodesource-linux-repositories)
* Clang 3.4 or later
* Development headers of GTK+ and libnotify

On Ubuntu, install the following libraries:

```bash
$ sudo apt-get install build-essential clang libdbus-1-dev libgtk2.0-dev \
                       libnotify-dev libgnome-keyring-dev libgconf2-dev \
                       libasound2-dev libcap-dev libcups2-dev libxtst-dev \
                       libxss1 gcc-multilib g++-multilib
```

Other distributions may offer similar packages for installation via package
managers such as yum. Or one can compile from source code.

## If You Use Virtual Machines For Building

If you plan to build electron on a virtual machine, you will need a fixed-size
device container of at least 25 gigabytes in size.

## Getting the code

```bash
$ git clone https://github.com/atom/electron.git
```

## Bootstrapping

The bootstrap script will download all necessary build dependencies and create
build project files. You must have Python 2.7.x for the script to succeed.
Downloading certain files could take a long time. Notice that we are using
`ninja` to build Electron so there is no `Makefile` generated.

```bash
$ cd electron
$ ./script/bootstrap.py -v
```

### Cross compilation

If you want to cross compile for `arm` or `ia32` targets, you can pass the
`--target_arch` parameter to the `bootstrap.py` script:

```bash
$ ./script/bootstrap.py -v --target_arch=arm
```

## Building

If you would like to build both `Release` and `Debug` targets:

```bash
$ ./script/build.py
```

This script will cause a very large Electron executable to be placed in
the directory `out/R`. The file size is in excess of 1.3 gigabytes. This
happens because the Release target binary contains debugging symbols.
To reduce the file size, run the `create-dist.py` script:

```bash
$ ./script/create-dist.py
```

This will put a working distribution with much smaller file sizes in
the `dist` directory. After running the create-dist.py script, you
may want to remove the 1.3+ gigabyte binary which is still in out/R.

You can also build the `Debug` target only:

```bash
$ ./script/build.py -c D
```

After building is done, you can find the `electron` debug binary under `out/D`.

## Cleaning

To clean the build files:

```bash
$ ./script/clean.py
```

## Troubleshooting

Make sure you have installed all the build dependencies.

## Tests

```bash
$ ./script/test.py
```
