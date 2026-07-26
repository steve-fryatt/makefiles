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

The `VERSION` variable can be set to a `n.nn` value (eg. `VERSION=1.23`) when calling Make to define a build version number; if unset, the Git commit hash for `HEAD` is used.

Some scripts allow the `TARGET` variable to be set to `linux` or `riscos` to determine the type of binary to be generated.

### Menu generation

Within `Basic` and `CApp`, it is possible to pass a set of Wimp menu definitions to [MenuGen](https://github.com/steve-fryatt/menugen) and have them turned into a binary menu data file. The binary file will be written to `OUTPUT/APP/UKRES/MENUS` - that is, within the target application's UK Resources folder. For this to work, `APP` should be set to the name of the application folder; `OUTPUT` defaults to `build`, and `UKRES` defaults to `Resources/UK`.

The menu source file should be stored within the `MENUDIR` folder, which by default is `menus/` in the root of the project. The name of the file should be placed in `MENUSRC`, which if not set explicitly defaults to `menudef`.

[MenuGen](https://github.com/steve-fryatt/menugen) is called via the `MENUGEN` variable, and any flags which need to be passed to it can be set in the `MENUGENFLAGS` variable.

### Documentation

The documentation will be built using either [Mantools](https://github.com/steve-fryatt/mantools) or [XMLMan](https://github.com/steve-fryatt/xmlman). The source files should be pointed to by the `MANSRC` variable, which is either the name of a single file, or a space-separated list if the manual source comprises more than one file. In either case, the first name in the list is passed to the manual build tool, whilst the rest are additional source files used by Make to trigger a re-build of the outputs. If the first filename ends with `.xml` then XMLMan will be used for the build; otherwise, Mantools will be used. The files are all assumed to be within the manual source folder.

If a Mantools manual uses a sprite, then this can be pointed to by the `MANSPR` variable.

The documentation can be written out in Text, HTML and StrongHelp formats to files within the application's UK Resources folder with names given by the `TEXTHELP`, `SHHELP` and `HTHELP` variables respectively. `TEXTHELP` defaults to `HelpText,fff`, whilst the others are unset by default.

To make the documentation obvious to an end user, it can be copied into the root of the distribution archive. This is done by setting the `README` and `HTMLHELP` variables to the names of the required top-level files (there is no provision to place a StrongHelp file in the archive root); `README` defaults to `ReadMe,fff` whilst `HTMLHELP` is unset by default. The `READMEHDR` variable supplies the name of a header file within the manual source folder (which defaults to `Header`), that is copied into the top of the ReadMe file. This is configured to be placed correctly for the default output from Mantools or XMLMan, but may not work if the output format varies.

For internationalisation and the ability to fall back to plain text when a more structure manual format isn't available, it is expected that the application's !Help file will be a small BASIC program which loads the required file. The source of the program should be in plain text form within the manual source folder, and its name is given by the `FINDHELPSRC` variable - this defaults to `Help.bbt`. The tokenised BASIC will be written to a file in the application folder root; its name is stored in the `FINDHELP` variable, and defaults to `!Help,ffb`.

The manual source folder is pointed to by the `MANSRC` variable. It defaults to `manual` at the root of the project.

A licence can be copied from the root of the project folder in to the root of the distribution archive. The original filename should be given in the `LICSRC` variable, and the target name in the `LICENCE` variable; these default to `Licence` and `Licence,fff` respectively.

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

### Obey file substitutions

Within `Basic`, `CApp` and `Module` it is possible to insert the build version into Obey files within the `OUTPUT` folder. If `APP` is set to the name of the application folder, then the resulting files will be placed within `OUTPUT/APP/` - that is, within the root of the application folder.

The files to be substituted should be stored within the `SUBDIR` folder, which by default is `subs/` in the root of the project, and listed in `SUBS`. To process an application's standard !Boot and !Run files, the source files would usually be saved as `subs/!Boot,feb` and `subs/!Run,feb`, and `SUBS` set to

    SUBS := !Boot,feb !Run.feb

If `APP` is also set to

    APP := !App

then the resulting files will be written to `build/!App/!Boot,feb` and `build/!App/!Run,feb` respectively. If `APP` is not set, then the files will be written to `build/!Boot,feb` and `build/!Run,feb`.

Within each of the source files, any occurrences of the text `{{VERSION}}` will be replaced with the version number associated with the build: either the value passed in with the `VERSION` variable, or the Git commit hash. This will most likely be used for setting the `App$Version` system variable, for example:

    Set MyApp$Version "{{VERSION}}"

### C Object folders

When working with `Capp`, `CLib` and `Cross`, object and dependency files will be stored in sub-folders within `OBJDIR` - specifically `APP_DEPDIR`, `APP_OBJDIR`, `TEST_DEPDIR` and `TEST_OBJDIR`. These will all default to sensible values, and do not need to be set by the calling Makefile.

If there are sib-folders within the source `SRCDIR` folder, then `OBJS_SUBDIRS` should be set to a list of these folders. For example

    OBJS_SUBDIRS = core extra

### C unit tests

When working with `Capp`, `CLib` and `Cross`, [Unity unit tests](https://www.throwtheswitch.org/unity) can optionally be built using the `test` target.

If present, test infrastructure should be located within the `TESTDIR` folder, which defaults to `tests/`. Within this, the test sources should be placed inside `TEST_SRCDIR` and have filenames starting with the prefix `TEST_PREFIX`. These latter variables default to `tests/` and `test_` respectively, so an example test source file might be `test/tests/test_example.c`.

The Unity source files `unity.c`, `unity.h` and `unity_internals.h` should be placed within the `TEST_UNITYDIR` folder, which defaults to `test/unity/`. Executable files, which are built for the current target platform, are saved within `TEST_ABSDIR` - giving a default location of `test/absolute/`. These will be cleaned, but other files within the folder will be preserved if present.

The presence of unit tests is detected by the presence of `test/tests/test_*.c` files (dependant on the variables detailed above).

When building tests, the `UNIT_TESTING` macro is defined to enable the C preprocessor to include or exclude code as required. It will probably be necessary to prevent an application's `main()` function from being built in a unit text context, for example. The intermediate object files are stored separately, so that variations in the code are preserved between builds. Additional flags are passed to `CC` when building the application sources for unit testing, using `TEST_CCFLAGS`: by default, this is used to remove the `Wunused-function` warning as this can be spurious if parts of the source are being omitted.

### Backups

It is possible to `make backup` to dump all of the files from within the project into a dated zip file whose name is based on `ARCHIVE` with the current date in `YYYYMMDD` format appended (eg. `project20260124.zip`). This provides a means to snapshot the state of the project at a given moment in time.

The backup archive will be created in the _parent_ folder which contains the project. If the file already exists, _it will be deleted_ before a new archive is created.

Zip will be called via the `ZIP` variable, and uses the GCCSDK binary which can convert `,xxx` filename suffixes into RISC OS filetypes. It is possible to specify the flags passed to Zip by setting the `BUZIPFLAGS` in the parent makefile. If this is not done, then suitable defaults are used.

Licence
-------

Shared Makefiles are licensed under the EUPL, Version 1.2 only (the "Licence"); you may not use this work except in compliance with the Licence.

You may obtain a copy of the Licence at <http://joinup.ec.europa.eu/software/page/eupl>.

Unless required by applicable law or agreed to in writing, software distributed under the Licence is distributed on an "**as is**"; basis, **without warranties or conditions of any kind**, either express or implied.

See the Licence for the specific language governing permissions and limitations under the Licence.