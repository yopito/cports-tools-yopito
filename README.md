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

* store output into timestamped logfile `log/<date>.<category>.<pkgname>.<arch>/<pkgname>.build.log`  
  Helps comparing with previous build.

* dump package information into logfile `log/<date>.<category>.<pkgname>.<arch>/<gen-package>.apk.info`  
  include all apk metadata and file list content.  
  There is a file for each built or generated package.

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
Use `build.scratch` for each build and display a summary of builds.

