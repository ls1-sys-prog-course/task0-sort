# Implement sort(1)

The goal of this assigment is to get familiar with the task format and
your chosen system programming language (C, C++ or Rust). 
It will be not part of your final grade.
Once your repository is created from the general template,
the continous integration services will build your project and will run
the tests on your programs (see the Actions tab on github).

## The build system

Each assignment comes with a template [Makefile](Makefile) as the make build system that
needs to be adapted depending on the programming language.
All assignments will try to build the `all` target within the Makefile like this:

```console
$ make all
```

So make sure your that the `all` target will produce all executables required for the
tests. 

The github build environment comes with all tools for building C, C++ and Rust pre-installed.
At the time of writing the following set up is installed:

- C/C++ compilers: gcc 10.3.0
- C/C++ build systems: cmake: 3.16.3 autoconf: 2.69, automake: 1.16.1
- Rust compiler/build system: rustc / cargo: 1.55.0

## Tests

Our tests will lookup exectuables in one of the following directories (assuming `./` is the project root):

- `./`
- `./target/release`
- `./target/debug`

where the latter two directories are usually used by Rust's build system
[cargo](https://doc.rust-lang.org/cargo/index.html).

After that it runs individual tests coming from the `tests/` folder (test
scripts are prefixed with `test_`).
Each test program is a python3 script and can be run individually, i.e.:

```console
python3 ./tests/test_sort.py
```

For convenience our Makefile also comes with `check` target which will run all tests in serial:

```console
$ make check
```

For the rare occassion that bugs are experienced in the CI but not
locally, it is also possible to run the github action environment locally
with [docker](https://www.docker.com/) using this [container
image](https://github.com/orgs/ls1-courses/packages/container/package/ls1-runner):

``` console
# This will mount your current directory as /code into the container
 docker run -ti --entrypoint=/bin/bash -v $(pwd):/code --rm ghcr.io/ls1-sys-prog-course-internal/ls1-runner:latest
```

## The assignment for this week

1. Your task is it to write a program that reads lines from standard input (also known as stdin)
and prints all lines sorted to standard output (also known as stdout) in ascending order.
For simplicty all test inputs can be assumed
[ASCII](https://en.wikipedia.org/wiki/ASCII) encoded and the comparison is done
byte-wise. The program will be called this:

``` console
./sort < input-file
```

2. Furthermore, your program should accept a flag as the first argument on command line `-r` which
will reverse the output (sorted in descending order):

``` console
./sort -r < input-file
```

3. Make sure your program can also sort its input using a fixed amount of memory.
We will test your program by applying `ulimit -v 131072` in its parent shell,
which will limit the program to 128MiB:

``` console
bash -c 'ulimit -v 131072; ./sort < input-file'
```

*Hint:* This is commonly known as external sort.

### 1. Test: tests/test_sort.py

Your program output is compared against `sort` from coreutils using Scottish wikipedia article names dataset as an input.

### 2. Test: tests/test_sort_reverse.py

Your program output is compared against `sort -r` from coreutils using Scottish wikipedia article names dataset as an input.

### 3. Test: tests/test_sort_limited_memory.py

Your program output is compared against `sort` from coreutils using English wikipedia article names dataset,
while the memory is limited to 128MiB.
