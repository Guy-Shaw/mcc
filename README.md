# mcc -- Run multiple C / C++ compilers

### Description

`mcc` runs multiple C / C++ compilers on a given set of C source file(s).

For each source file, some combination of four C / C++ are run,
all with the same options.

The compilers `mcc` knows about are:

  1. gcc
  2. clang
  3. g++
  4. clang++


Sometimes the goal is to just make sure C code builds clean
using both gcc and clang.  Or C++ code builds with g++ and clang++.

But there are times when the goal is to write code in a subset C
that is also a subset of legal C++.  In that case,
you want to run all 4 compilers.

### Typical use

If you have a Makefile with a target something like:

```
SRC_C := file1.c file2.c

%.o: %.c
        $(CC) $(CFLAGS) -c $<
```


Then you can add a target, 'mcc', that would be something like:

```
mcc:
        mcc --mcc:header $(CFLAGS) -c -- $(SRC_C)

```

`mcc` will use the same CFLAGS.

Options beginning with `--mcc:` are reserved for specifying `mcc`
behavior, and they are processed first.  The underlying compilers
do not those options.

### mcc options

####
--debug

####
--verbose

####
--simulate

Do not actually run the compilers.
Show the underlying command lines that would be run.
Then exit.

####
--cols=<colums>

Set number of columns for purposes of determining how wide
the report should be.  In particular, the headers, if any,
are displayed this wide.  The default is the environment
variable, 'COLUMNS', if it is set; but if that is not
specified, the it is $(tput cols) if stdout is to a terminal.
Otherwise, it is 80 columns.

####
--headers

Just before running each compiler,
print a header showing which compiler is being invoked.

####
--cpp-flags=s

Append C preprocessor flags to the ones that are already specified
on the command line.

####
--exclude=<pattern>
Exclude certain source files.
This is convenient, for example, when there are certain files
that you know are troublesome, but it is awkward to play with
Makefile lists of C file or with shell filename generation, just
to exclude some files.

####
--compilers=<compiler>

Where <compiler> is a comma-separated list of compiler names.
This option can be given more than once on the command line.

The list of valid compiler names is:

  1. gcc
  2. clang
  3. g++
  4. clang++


### Placeholders

`mcc` will make the following substitutions on the command line:

  1. `__OBJECT__`  with the source file name with the extension changed to '.o'.

  2. `__C__` with the source file name.

The symbol `__C__` will be substituted with the source filename,
without regard to its extension.  It is just assumed to be a C or C++
source file.  It should have an extension of '.c', '.cc'.

Clang++ is more fussy about the .c vs .cc extensions.
`mcc` converts '.c' to '.cc' when it runs clang++.

## License

See the license at the top of the Perl source code.


-- Guy Shaw

   gshaw@acm.org

