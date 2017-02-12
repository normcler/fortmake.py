# makemake.py
makemake.py is a Python script for generating makefiles that can be used with the GNU make utility for compiling source code. Currently supported programming languages are C and Fortran.
### How it works
The script takes a list of source files, and scans their content to determine how the files depend on each other. It then generates a makefile containing compilation and linking rules that take these dependecies into account. An executable can then be produced simply by calling `make`. The makefile contains additional rules for using predefined groups of compiler flags, e. g. calling `make debug` will compile with flags useful for debugging.

**Important:** With any automatically generated makefile there is always a chance that some dependencies have been handled incorrectly. This can result in sources not getting recompiled when they should, leading to unexpected behaviour when the program is run. It is therefore important that you always verify the list of dependencies printed by makemake.py when it generates a new makefile.
### Installation
1. Download the files in the *src* folder to a destination of your choice.
2. Make sure *makemake.py* is allowed to be executed. To make it executable, use `chmod +x makemake.py`.
3. Make sure the source folder is included in the PATH environment variable. To include the folder in PATH, you can add the line `export PATH=$PATH:<path to source folder>` to your *.bashrc* file (located in the home directory). 
4. You can now run the script from anywhere by typing `makemake.py <arguments>`.

### Usage
The arguments to `makemake.py` are the names of the files you want to create a makefile for. The arguments are separated by spaces, and any arguments containing spaces must be surrounded with double quotes. The files can either be ordinary source code files (e. g. `.c` or `.f90`) or header files (`.h`) that get included in the code.

For files residing in the current working directory, only the name of the file needs to be specified. For files lying in a different folder, the absolute path (starting with `/`) or relative path (starting with `./`) must be added in front of the filename.

An alternative to this is to specify one or more search paths. For source files this is done with the `-S` flag. Just add `-S` somewhere in the argument list, followed by the (absolute or relative) paths that you want to include in the list of search paths. Then, if the script fails to find a source file in the working directory, it automatically searches the paths specified in the list of search paths. This is useful if you have several source files residing in the same directory. There is an equivalent `-H` flag for header file search paths, and an `-L` flag for library search paths. These can all be combined arbitrarily, so e. g. `-SH` would specify paths to search for both source and header files.

The script will automatically recognize the programming language based on the file extension of the source files. You can specify the compiler to use with the `-c` flag. Just write the name of the compiler directly following the flag. The default option will be a compiler from the GNU Compiler Collection, i. e. *gcc* for C and *gfortran* for Fortran.

If compiling the group of source files will result in several executables, one makefile is generated for each executable. Note however that it is not recommended to include multiple executable producing sources that have different dependecies in the same call to makemake.py, as this might cause the script to detect apparent dependencies that you don't want.

By default the script tries to save the newly generated makefile as `makefile`. If a file of that name already exists, you can opt to choose a different name, or, if the existing makefile was generated by makemake.py, the script can rename the two relevant makefiles to `<executable name>.mk` and create a "wrapper" makefile that allows you to choose which executable you want to create every time you call `make`. All files with the *.mk* extension that are present will be included in this wrapper. If such a wrapper already exists, an entry for the newly generated makefile can be added to it. The `-w` flag will tell the script to generate a makefile wrapper even if it doesn't find it necessary. To create an executable `my_prog.x` via the wrapper, simply call `make my_prog`, and the wrapper will run the relevant makefile. Any arguments for that makefile must be specified in the following way: `make my_prog ARGS="<list of arguments>"`.

Here is a list of the available arguments you can add after `make`:
- `debug`:   Compiles with flags useful for debugging.
- `fast`:    Compiles with flags for high performance.
- `profile`: Compiles with flags for profiling.
- `gprof`:   Displays the profiling results with gprof.
- `clean`:   Deletes auxiliary files.
- `help`:    Displays a help text.

You can also specify any additional compilation flags to use with the argument `FLAGS="<additional flags>"`.

Note that you can call `makemake.py` without any arguments to get some more compact usage instructions.