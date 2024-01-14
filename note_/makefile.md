---
layout: splash
author_profile: false
---

<h1> An Introduction to Makefile </h1>

<h2>Content</h2>

- [Preliminaries](#preliminaries)
    - [Working process](#working-process)
    - [Library](#library)
    - [GCC/G++ command](#gccg-command)

- [Introduction of Makefile](#introduction-of-makefile)
    - [Basic](#basic)
    - [Pattern rule](#pattern-rule)
    - [Auto-generate dependency](#auto-generate-dependency)
    - [Variable](#variable)
    - [Grammar](#grammar)
    - [Function](#function)
    - [Include file](#include-file)
    - [Path problem](#path-problem)
    - [Nested execution](#nested-execution)

- [Samples of Makefile](#samples-of-makefile)


-----------------------------------------------------

# Preliminaries

Before the introduction of Makefile, it is better to understand the working flow of executable file building, especially for the multi-files process part. 

In general, the working flow goes like

source code --> compile (Object file) --> link (exe file)

- compile : check grammar

- link : sometimes it need to package some files, in Windows, it's called Library File (.lib); in Unix,  it's called Archive File (.a).


You will find that `make` is a capable tool to automate short interdependent workflows and to build small projects. But for larger projects, you will probably soon run against some of it limitations. Usually, `make` is therefore not used alone but combined with other tools to generate the Makefile completely or in parts.

Website for further introduction and discussion.

- [GNU Make Manual](https://www.gnu.org/software/make/manual/)
- [A Guidance for Make in Chinese](https://seisman.github.io/how-to-write-makefile/overview.html)
- [CMake](https://cmake.org/getting-started/)


--------------------------------------------------------------


## Working process 
- step 1 : Preprocessing
- step 2 : Compilation
- step 3 : Assembly
- step 4 : Linking

The end result, the executable program, contains the compiled source code and various auxiliary routines that make it work. It also contains references to so-called dynamic run-time libraries (in Windows: DLLs, in Linux: shared objects or shared libraries). Without these run-time libraries the program will not start.

Use `ldd` to know what libraries are required.

------------------------------------------------------------

## Library
Using libraries you can build very large programs without having to resort to extremely long command lines.

### Static library
Libraries contain any number of object files in a compact form, so that the command-line becomes far shorter than to include all the object files. For example, `supportLib.a` is a collection of one, two or many object files, all compiled and then put into a library. The extension “.a” is used by Linux and Linux-like platforms. On Windows the extension “.lib” is used. 

Static libraries (or at least parts of their contents) become an integral part of the executable program. The only way to change the routines incorporated in the program is by rebuilding the program with a new version of the library.

``` sh
ar r supportLib.a file1.o file2.o  # build static library
```

The command `ar` with the option r either creates the library (the name appears after the option) or adds new object files to the library (or replaces any existing ones).
Libraries are often sought in directories indicated by an option `-L` or `/LIBPATH`. This saves you from having to specify the exact path for every library.


### Dynamic library
A flexible alternative is to use the so-called dynamic libraries. These libraries remain outside the executable program and as a consequence can be replaced without rebuilding the entire program. Compilers and indeed the operating system itself rely heavily on such dynamic libraries. You could consider dynamic libraries as a sort of executable programs that need a bit of help to be run.

Building dynamic libraries works slightly differently from building static libraries: you use the compiler/linker instead of a tool like `ar` or `lib`.

On Linux: for example
``` makefile
gfortran -fpic -c file1.f90 file2.f90
gfortran -fpic -c file3.f90 ...
gfortran -shared -o supportlib.so file1.o file2.o file3.o ...
```
The differences are that:

- You need to specify a compile option on Linux, for `gfortran` that is `-fpic`, because the object code is slightly different.
- You need to tell in the link step that you want a dynamic library (on Linux: a shared object/library, hence the extension `.so`; on Windows: a dynamic link library, `dll`)

One of the reasons why we use dynamic library (or shared object) is that this file is frequently used in many software and to prepare a copy for each of these softwares will waste some space. 

### Usage of library in makefile
To figure out the position of particular library, you can use full command `path/to/lib/libName`, for example, `${FFTW_DIR}/lib/libfftw3.a`, which is treated as part of the dependent files of the target. 

You can also use short command `-lname`, where `name` in the command should be replaced by a concrete name of the library. When a prerequisite’s name has the form `-lname`, make handles it specially by searching for the file `libname.so`, and, if it is not found, searching for the file `libname.a` in the current directory, in directories specified by matching vpath search paths and the VPATH search path, and then in the directories /lib, /usr/lib, and prefix/lib (normally /usr/local/lib, but MS-DOS/MS-Windows versions of make behave as if prefix is defined to be the root of the DJGPP installation tree). 

For example, `-lmkl_core` is equivalent to `/usr/lib/libmkl_core.a` if there is a library in corresponding directory. 

Finally, to figure out where should we look for libraries to link with, use the option `-L`, for example, 
``` makefile
LDFLAGs = -L. \ # current folder 
          -L$(LIB_DIR)`.
```

In conclusion, you can also figure out many libraries in one command if they are in the same folder, for example,
``` makefile
FFTW_LIB = -L${FFTW_DIR}/lib -lfftw3_mpi -lfftw3 -lm
```

For dynamic library, the case is a little complicated since they are shared object, you have to write the path to dynamic library into the target so that it know where to link those `.so` files. If the path have been part of the environment variables `LD_LIBRARY_PATH`, you can use `-L` and `-l` as above. However, if the path are not in the environment variables `LD_LIBRARY_PATH`, you have to figure out the path for complier, e.g., gcc in the following way
``` makefile
BLASPP_LIB = -lblaspp -L${BLASPP_DIR} -Wl,-rpath=${BLASPP_DIR}
```
where `${BLASPP_DIR}` is the path to position of `libblaspp.so` etc.


Linux command `ldd name_of_exe` is useful to list the dynamic link libraries.

------------------------------------------------------------

## GCC/G++ command
Use `man gcc` to see the help document of gcc

All the `gcc` may need to be replaced by `g++` or `cc`, etc, the command parameters can combine together, multi files are the same 
``` makefile
gcc -c hello.c  # preprocessing, compilation and assembly, generate object file named *.o
gcc -s hello.c  # preprocessing, compilation, generate assembly file named *.s 
gcc -E hello.c > hello.txt # preprocessing only, not generate file, but you can redirect it to a output file
gcc hello.c  # generate executable file named a.out 
gcc -o outputFileName hello.c  # preprocessing, compilation, assembly and link, specify the output file name
gcc -c hello.c -o outputName  # preprocessing, compilation and assembly, generate object file ans specify it name as outputName
gcc -g hello.c  # generate executable file used for debug, which can run in gdb
```

Some other commands
``` makefile
gcc -Wall hello.c  # turn on all warning
gcc -W hello.c  # open some extra warning
gcc hello.c -I /xxx/xxx/file  # equivalent to include<file>
gcc hello.c -L /xxx/xxx/lib  # specify the folder of lib
```
---------------------------------------------------------------


# Introduction of Makefile
"Makefile" means use software "make" to run the text document to make some files. The Makefile gathers some command tell how to make the file you want. 

You can type `make` in command line to make the first target of the Makefile or type `make fileName` to make one of the target files described in the Makefile.  To make multi files, you can add an extra target listing all the target files.

Most commands are Shell-like with a little difference. If there are many Makefile or the Makefile not named Makefile, you can figure out the filename as a parameter for command `make` like `make -f makefile_name` or `make --file=fileName`


## Basic
A template of Makefile always be
``` makefile
target ... : prerequisites ...
    
    command ...
```
`target` relies on files in `prerequisites` whose generated rules defines in `command`, which could be shell command. Command should start with a tab.


**An example**
``` makefile
result.txt: source.txt
    cp source.txt result.txt
source.txt:
    echo "This is the source" > source.txt
```
For multi files, add `all: file1 file2 file3` at the first line and type `make all` or `make`


**Target**

target can be file name or files' name and some operation, which is called "phony target".  
``` makefile
.PHONY: clean  # for distinguishing with file named "clean"
clean:
    rm *.o  # -rm means skip problem when cleaning
```
To clean all the file, just type `make clean`.

Phony target can also be used to print some information
``` makefile
.PHONY: build_msg
build_msg:
    @printf "#\n# Building \n#\n"
```

You can use `&&` to do the two target in a command line, just like `make targetA && make targetB`


**Comment**

Comment line starts with `#`, but it will print the comment line in Makefile, add `@#` to make it not to print   


**Echo**

``` makefile
@echo TEST  # @ is used for not print the echo command, but it will not work in for loop
```

**Line breaker**

Use `\` at the end of the line. For those complex command, like for loop, use `\` to make them work. 

NOTE: extra blank space after `\` will cause error.

----------------------------------------------------------

## Pattern rule
A pattern rule contains the character `%` (exactly one of them) in the target. The target is a pattern for matching file names, the `%` matches any nonempty substring, while other characters match only themselves.

For example, `%.c` as a pattern matches any file name that ends in `.c`, `s.%.c` as a pattern matches any file name that starts with `s.` and ends in `.c` and at least five characters long. (There must be at least one character to match the `%`.)

``` makefile
%.o : %.c
    $(CC) -c $(CFLAGS) $(CPPFLAGS) $< -o $@
```

### Static mode
``` makefile
<targets ...> : <target-pattern> : <prereq-patterns ...>
    <commands>
    ...
```
For example,
``` makefile
objects = foo.o bar.o

all: $(objects)

$(objects): %.o: %.c
    $(CC) -c $(CFLAGS) $< -o $@
```

----------------------------------------------------------


## Auto-generate dependency
There maybe some header files in our prerequisites files, like 
``` makefile
main.o : main.c defs.h
``` 
It's really something if there are a lot of header files and the command line will be long and hard to manage. Since all the include relation have been written in cpp file and we can use the option of compiler `-M` or `-MM` to find the header file listed in source code. However, to connect this option of compiler and Makefile, we need some more effort since `-M` option just prints some texts about the dependency of files and you have to update `.d` file every time they changed.

GNU suggests that to save the dependency generated automatically by compiler into a file, i.e., generate a Makefile `name.d` for every `name.c`, `.d` file saves the dependency of `.c` file. 

This could be done by the following pattern rule code, which generates `.d` files.

``` makefile
%.d: %.c
    @set -e; rm -f $@; \
    $(CC) -M $(CPPFLAGS) $< > $@.$$$$; \
    sed 's,\($*\)\.o[ :]*,\1.o $@ : ,g' < $@.$$$$ > $@; \
    rm -f $@.$$$$
``` 
For the first line, `set` is a linux shell command. The `-e` flag to the shell causes it to exit immediately if the `$(CC)` command (or any other command) fails (exits with a nonzero status). `rm -f $@` means to delete all the target `.d` file. 

The second line means to generate dependency file for every prerequisites file `$<`, i.e., `.c` file using the `-M` option of compiler. `$@` is `%.d` file, for `name.c` file, `%` will be `name` and `$$$$` is a random number. For example, the second line may generate a file `name.d.12345`.  

The third line use `sed` command to substitute from `$@.$$$$` to `$@`, in `sed 's,\($*\)\.o[ :]*,\1.o $@ : ,g'`, `, ` is delimiter (you can also use other type of delimiter). This line adds `.d` file target file, i.e., from
``` makefile
main.o : main.c defs.h
```
to 
``` makefile
main.o main.d : main.c defs.h
```

The final line delete temporary files.

After the command above, `.d` file will be auto-generated and auto-updated. Finally you need to add those rule from `.d` file into our real Makefile, you can use the `include` command just like
``` makefile
sources = foo.c bar.c

include $(source:.c=.d)
```

-------------------------------------------------------

## Variable
``` makefile
VARIABLE = value  # assign when running, dynamic extension
VARIABLE := value  # assign when defining, static extension
VARIABLE ?= value  # assign when it is empty
VARIABLE += value  # append the value to the end of the VARIABLE
```
When calling the variable, put it inside `$()`.

For Shell variables, like `$HOME` or `$variable_name`, use an extra `$` to call them, i.e., `$$HOME`, `$$variable`.

Replace the same part of the variable, like the prefix or postfix, use command `bar:=$(var:a=b)`, for example, `foo:.o=.c` or use command `foo:%.o=%.c`.


### Automatic Variables
``` makefile
$@  # target file
$<  # the first file of the prerequisites list
$?  # all the prerequisites files newer than the target
$^  # all the prerequisites files
$*  # the pattern part described by %
$(@D)  # directory of the target $@
$(@F)  # file name of target $@
$(<D)  # directory of the first prerequisites
$(<F)  # file name of the first prerequisites
```

### Implicit Variables
For example, `$(CC)` refers to the current complier

---------------------------------------------------------


## Grammar
The same as Shell.
``` makefile
ifeq ($(CC), gcc)
    command
else
    command
endif
```

``` makefile
for i in $(LIST); do \
    echo $$i; \
done
```
-------------------------------------------------------


## Function
To call a function, use command `$(function_name argument1,argument2)`. Use blank space to separate function name and argument, use `,` to separate arguments.

### Special functions
`$(subst from,to,text)`: substitute function

`$(patsubst pattern,replacement,text)`

`$(strip string)`

`$(filter pattern1 pattern2,text)`

`$(filter-out pattern,text)`

------------------------------------------------------


## Include file

### include of Makefile
``` makefile
include foo.make
include ../foo.make
```
When make is running and it meets `-I` or `--include-dir`, it will then search under the directory these parameters figure out.

If you want make to simply ignore a makefile which does not exist or cannot be remade, with no error message, use the `-include` directive instead of `include`.

### include of header files
So what if we want to start putting our .h files in an include directory, our source code in a src directory, and some local libraries in a lib directory? Also, can we somehow hide those annoying .o files that hang around all over the place? The answer, of course, is yes. 

``` makefile
gfortran -c tabulate.f90 -I ../sub
```
Just like the example above, we can use `-I directory` to figure out where the files tp be included and the rule has been written in the `.cpp` or `.f90` files.  

------------------------------------------------------


## Path problem

In some large projects, there will be a lot of source files. It is common to sort out these files and put them into different folders. You can use `vpath` to tell Makefile where to find all the source files.
For example, we can use
``` makefile
vpath %.cpp . dir1 dir2
SRC = file1.cpp file2.cpp file3.cpp
```
where `file1.cpp, file2.cpp, file3.cpp` may come from different directories `dir1, dir2`.

Finally, you may use `vpath <pattern>` to clear the rule you have already set up for a pattern file and use `vpath` to clear all the rules.

-----------------------------------------------------


## Nested execution
For module compilation, we may write a Makefile for each of the directories to make them much clearer. For example, we have a sub directory named subdir, then we can write in the main Makefile
``` makefile
subsystem:
    cd subdir && $(MAKE)
```

As for passing variables between different level Makefile, use command
``` makefile
export variable1 = value
```
An single `export` means passing all the variables.

--------------------------------------------------


# Samples of Makefile

## Sample 1

Usually there are several Makefile of a large package and they should be connected by some shell script. 

The sample below gives an illustration for such file structure.

- compile.sh (clean and make file)
``` sh
cd directory  # there will be a Makefile in each directory
make cleanall && make -j # option job means to run jobs simultaneously
```
- make.inc
Put those general parameters and complier rules in this file. For each Makefile in different folders, just include make.inc

- Makefile

``` makefile
# NOTE: This Makefile does NOT support auto-dependency for the .h files.
# If the header files are changed, do "make clean" first.

include ../make.inc

SRCS = pwdft.cpp dgdft.cpp

OBJS = ${SRCS:.cpp=.o}
DEPS = ${SRCS:.cpp=.d}
EXES = ${SRCS:.cpp=}	

pwdft: pwdft.o ${DGDFT_LIB} 
	($(LOADER) -o $@ pwdft.o $(LOADOPTS) )

dgdft: dgdft.o ${DGDFT_LIB} 
	($(LOADER) -o $@ dgdft.o $(LOADOPTS) )

-include ${DEPS}

${DGDFT_LIB}:
	(cd ${DGDFT_DIR}/src; make all)

cleanlib:
	(${RM} -f ${DGDFT_LIB})

cleanall:
	rm -f ${EXES} ${OBJS} ${DEPS} *.d.o
```
----------------------------------------------------


## Sample 2
``` makefile
##############################################################################
#
#  Sample Makefile for C++ applications
#    Works for single and multiple file programs.
#    written by Robert Duvall
#    modified by Owen Astrachan
#    and by Garrett Mitchener
# 
##############################################################################

##############################################################################
# Application-specific variables
# EXEC is the name of the executable file
# SRC_FILES is a list of all source code files that must be linked
#           to create the executable
##############################################################################

EXEC      = usepix
SRC_FILES = application.cc displaycommand.cc filelister.cc menu.cc \
        menuitem.cc pixmap.cc quitcommand.cc readcommand.cc \
        usepix.cc

##############################################################################
# Where to find course related files
# COURSE_DIR is where various header files (.h) and library files (.so and .a)
# are found.  LIB_DIR is where other libraries not specific to the course
# are kept.

# for CS machines
# COURSE_DIR = /usr/project/courses/cps108/lib
# LIB_DIR     = /usr/local/lib

# for acpub machines
COURSE_DIR = /afs/acpub/users/o/l/ola/cps108/lib
LIB_DIR     = /afs/acpub/project/cps/lib

##############################################################################
# Compiler specifications
# These match the variable names given in /usr/share/lib/make/make.rules
# so that make's generic rules work to compile our files.
# gmake prefers CXX and CXXFLAGS for c++ programs
##############################################################################
# Which compiler should be used
CXX = g++
CC = $(CXX)

# What flags should be passed to the compiler

DEBUG_LEVEL     = -g
EXTRA_CCFLAGS   = -Wall
CXXFLAGS        = $(DEBUG_LEVEL) $(EXTRA_CCFLAGS)
CCFLAGS         = $(CXXFLAGS)

# What flags should be passed to the C pre-processor
#   In other words, where should we look for files to include - note,
#   you should never need to include compiler specific directories here
#   because each compiler already knows where to look for its system
#   files (unless you want to override the defaults)

CPPFLAGS        = -I. \
                  -I$(COURSE_DIR)

# What flags should be passed to the linker
#   In other words, where should we look for libraries to link with - note,
#   you should never need to include compiler specific directories here
#   because each compiler already knows where to look for its system files.

LDFLAGS         = -L. \
                  -L$(COURSE_DIR) \
                  -R $(LIB_DIR):$(COURSE_DIR)

# What libraries should be linked with.
# For example, -lm links with libm.so, the math library.
# If you make a library of your own, say, libscandir.a, you have to link it
# in by adding -lscandir here.
LDLIBS          = -lscandir

# All source files have associated object files.
# This line sets `OFILES' to have the same value as `SRC_FILES' but
# with all the .cc's changed into .o's.
O_FILES         = $(SRC_FILES:%.cc=%.o)


###########################################################################
# Additional rules make should know about in order to compile our files
###########################################################################
# all is the default rule
all: $(EXEC)


# exec depends on the object files
# It is made automagically using the LDFLAGS and LOADLIBES variables.
# The .o files are made automagically using the CXXFLAGS variable.
$(EXEC): $(O_FILES)

# to use `makedepend', the target is `depend'
depend:
   ->   makedepend -- $(CXXFLAGS) -- -Y $(SRC_FILES)



# clean up after you're done
clean:
   ->   $(RM) $(O_FILES) core *.rpo

very-clean: clean
   ->   $(RM) $(EXEC)
```

Note that the arrows `->` represent places where you should put tab characters, not eights spaces. It hath been decreed that this shalt be a tab character. Emacs and XEmacs have a makefile mode, which you can get into by typing M-x makefile-mode if it doesn't come up automatically. In makefile mode, pressing the tab key inserts a real tab. Alternatively, the keystrokes C-q C-i or C-q tab will enter a tab character in any mode. (C-q is emacs for quote next key unprocessed.)
