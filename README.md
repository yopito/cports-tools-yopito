# cports-tools-yopito
various tools around chimera-linux cports

*Contents*

* [build.scratch](#build.scratch)

<a id="build.scratch"></a>
## build.scratch

`build.scratch` is a wrapper on top on `cbuild.py` to ease to-day-to packaging: 

* build each package from a *fresh* `masterdir`.  
  This helps on tracking dependencies, since build environment is not polluted
  from a previous build.

* store output into timestamped logfile: `log/<date>.<pkgname>/<pkgname>.build.log`  
  Helps comparing with previous build.

* (todo) keep information of package content within a a file: `log/<date>.<pkgname>/<pkgname>.apk.info`  
  includes package metadata, (sorted) depedencies, (sorted) file list content.  
  If a package contains subpackages, each subpackage is also caracterized : file `log/<date>.<pkgname>/<subpkg>.apk.info`

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


