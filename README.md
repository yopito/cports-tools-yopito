# cports-tools-yopito

various tools for chimera-linux cports

*Contents*

* [build.scratch](#build.scratch)
* [rebuild.my.pkg](#rebuild.my.pkg)

<a id="build.scratch"></a>
## build.scratch

`build.scratch` is a wrapper on top on `cbuild.py` to ease to-day-to packaging: 

* build each package from a *fresh* `masterdir`.  
  This helps on tracking dependencies, since build environment is not polluted
  from a previous build.

* store output into timestamped logfile `log/<date>.<category>.<pkgname>.<arch>/build.log`  
  Helps to compare builds attempts.

* dump package information into file `log/<date>.<category>.<pkgname>.<arch>/<gen-package>.apk.info`  
  Include all apk metadata and file list content.  
  There is a file for each built package (requested or needed).

* (todo) order packaging build according to their required dependency

* (todo) option to build for every crossbuild targets


### Usage

Copy (or link) `build.scratch` in the same dir than `cbuild.py`.

Then:
```
[cports.git] $ ./build.scratch [-n] ["cbuild.py options"] main/pkg1 contrib/pkg2 ...
```
Detailed usage : `$ ./build.scratch -h`

### Example

Let's build package `contrib/dialog` for (cross) arch `riscv64`.  
Will generate the following files:
* cbuild.py build log: `log/2021-09-15_011326.contrib.dialog.riscv64/build.log`
* info about (the only) generated package : `log/2021-09-15_011326.contrib.dialog.riscv64/dialog.riscv64.apk.info` 

```
[cports.git] $ ./build.scratch '-a riscv64' contrib/dialog
```
```
# input cbuild args: -a riscv64
# package(s) requested to build:  contrib/dialog
# package(s) to build:  contrib/dialog
# XXX packages are not ordered by their build depedency yet

# cmd: test -f contrib/dialog/template.py
# [contrib/dialog] create a fresh masterdir ...
# cmd: ./cbuild.py zap
# cmd: ./cbuild.py binary-bootstrap > /dev/null
ERROR: 194 errors updating directory permissions
# [contrib/dialog] build logfile: log/2021-09-15_011326.contrib.dialog.riscv64/build.log
# cmd: mkdir -p /home/yopito/chimera/cports.git/log/2021-09-15_011326.contrib.dialog.riscv64
# cmd: (2>&1 ./cbuild.py --force --no-color -a riscv64 pkg contrib/dialog) > log/2021-09-15_011326.contrib.dialog.riscv64/build.log
# [contrib/dialog] dump subpkg dialog info into log/2021-09-15_011326.contrib.dialog.riscv64/dialog.riscv64.apk.info
# cmd: apk --root /tmp/apkroot.BL8ARcTs --arch riscv64 --allow-untrusted --repository /build/chimera/packages/contrib info -a dialog >> log/2021-09-15_011326.contrib.dialog.riscv64/dialog.riscv64.apk.info
# cmd: 2> /dev/null tar tzvf /build/chimera/packages/contrib/riscv64/dialog-1.3.20210621-r0.apk >> log/2021-09-15_011326.contrib.dialog.riscv64/dialog.riscv64.apk.info
# That's all folks !
```

<a id="rebuild.my.pkg"></a>
## rebuild.my.pkg

A loop to build a (hardcoded) list of packages for all target architecures.  
Uses `build.scratch` to perform each build (generating log and info files) and display a summary of builds.

### Example: build all packages of current git branch

Build all packages added or modified since master branch, for every available architectures:
```
$ git log --oneline --name-status master.. \
      | grep -F /template.py | grep "^[AM]" | awk '{print $2}' \
      | tac | xargs -n 1 dirname | xargs ../tools-yopito.git/rebuild.my.pkg
...
(calls to build.scratch for each packages cross each target arch)
...
# Summary:
FAIL  arch=x86_64   pkg  contrib/neovim
FAIL  arch=aarch64  pkg  contrib/neovim
FAIL  arch=ppc64    pkg  contrib/neovim
FAIL  arch=ppc64le  pkg  contrib/neovim
FAIL  arch=riscv64  pkg  contrib/neovim
OK    arch=x86_64   pkg  contrib/gtest
OK    arch=aarch64  pkg  contrib/gtest
OK    arch=ppc64    pkg  contrib/gtest
OK    arch=ppc64le  pkg  contrib/gtest
OK    arch=riscv64  pkg  contrib/gtest
OK    arch=x86_64   pkg  contrib/dinit
FAIL  arch=aarch64  pkg  contrib/dinit
FAIL  arch=ppc64    pkg  contrib/dinit
FAIL  arch=ppc64le  pkg  contrib/dinit
FAIL  arch=riscv64  pkg  contrib/dinit
...
```
