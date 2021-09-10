# cports-tools-yopito

various tools around chimera-linux cports

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
  Helps comparing with previous build.

* dump package information into logfile `log/<date>.<category>.<pkgname>.<arch>/<gen-package>.apk.info`  
  include all apk metadata and file list content.  
  There is a file for each built package (requested or needed).

* (todo) order packaging build according to their required dependency

* (todo) also build for every crossbuild targets


### Usage

Copy (or link) `build.scratch` in the same dir than `cbuild.py`.

Then:
```
$ ./build.scratch [-n] [-a target_arch] main/pkg1 contrib/pkg2 ...
```
Detailed usage : `$ ./build.scratch -h`

### (todo) Example


<a id="rebuild.my.pkg"></a>
## rebuild.my.pkg

A loop to build a (hardcoded) list of packages for all target architecures.  
Use `build.scratch` to perform each build and display a summary of builds.

### Example: build all packages of current git branch

Build all packages added or modified since master branch, for every available architectures:
```
$ git log --oneline --name-status master.. \
      | grep -F /template.py | grep "^[AM]" | awk '{print $2}' \
      | tac | xargs -n 1 dirname | xargs ../tools-yopito.git/rebuild.my.pkg
...
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
