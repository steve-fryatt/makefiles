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

To build the manual, Mantools must also have been installed. This msut be done *after* installing the Shared Makefiles; returning to the Shared Makefiles project and running

	make install

again will cause the manual to be built if Mantools have been installed correctly.


Licence
-------

Shared Makefiles are licensed under the EUPL, Version 1.2 only (the "Licence"); you may not use this work except in compliance with the Licence.

You may obtain a copy of the Licence at <http://joinup.ec.europa.eu/software/page/eupl>

Unless required by applicable law or agreed to in writing, software distributed under the Licence is distributed on an "**as is**"; basis, **without warranties or conditions of any kind**, either express or implied.

See the Licence for the specific language governing permissions and limitations under the Licence.