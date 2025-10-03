Shared Makefiles
================

Common tools for cross-compiling RISC OS software.


Introduction
------------

The Shared Makefiles are a collection of Makefiles containing useful rules for building RISC OS software under the [GCCSDK](http://www.riscos.info/index.php/GCCSDK). They are required by RISC OS source code found amongst the adjacent repositories.


Installation
------------

To install and use the Shared Makefiles, it will be necessary to have suitable Linux system with a working installation of the [GCCSDK](http://www.riscos.info/index.php/GCCSDK).

It will also be necessary to ensure that the `SFTOOLS_MAKE` variable is set to a suitable location within the current environment. For example

	export SFTOOLS_MAKE=/home/steve/sftools/make

where the path is changed to suit your local settings and installation requirements. To install the Makefiles, use

	make install

from the root folder of the project, which will copy the necessary files in to the location indicated by `$SFTOOLS_MAKE`.

To build the manual, Mantools must also have been installed. This must be done *after* installing the Shared Makefiles; returning to the Shared Makefiles project and running

	make install

again will cause the manual to be built if Mantools have been installed correctly.


Usage Notes
-----------

Makefiles should be used with GNUMake.

The `VERSION` variable can be set to a `n.nn` value (eg. `VERSION=1.23`) to define a build version number; if unset, the Git commit hash for `HEAD` is used.

Some scripts allow `TARGET` to be set to `linux` or `riscos` to determine the type of binary to be generated.

### Build information

When the toolchain is called to build the main executable, a build version, build date and additional build information will be passed in as defines:

* For GCC, the constants `BUILD_VERSION`, `BUILD_DATE` and `BUILD_INFO` will be set.
* For Tokenize, the variables `build_version$`, `build_date$` and `build_info$` will be set.
* For AsAsm, the constants `BuildVersion`, `BuildDate` and `BuildInfo` will be set.

The build version will be the version in the `VERSION` variable when the Makefile was called. If this variable was unset or empty, the first seven digits of the Git commit hash of the project folder will be used.

The build date will be the date that the Makefile was called, in the form `DD Mmm YYYY`.

The build info will be the contents of the `BUILDINFO` variable, which is unset by default. This can be set by the calling Makefile, to provide some project-specific information for the build process. An example being Puzzles, which passes the Git commit hash for the upstream code submodule by including

    BUILDINFO := $(shell cd src/core; git rev-parse --short=7 HEAD)

in its Makefile before including `CApp`.


Licence
-------

Shared Makefiles are licensed under the EUPL, Version 1.2 only (the "Licence"); you may not use this work except in compliance with the Licence.

You may obtain a copy of the Licence at <http://joinup.ec.europa.eu/software/page/eupl>.

Unless required by applicable law or agreed to in writing, software distributed under the Licence is distributed on an "**as is**"; basis, **without warranties or conditions of any kind**, either express or implied.

See the Licence for the specific language governing permissions and limitations under the Licence.