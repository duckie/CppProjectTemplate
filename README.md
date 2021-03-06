C++ project template
====================

[![Travis CI Build Status](https://api.travis-ci.org/duckie/cpp_project_template.svg?branch=master)](https://travis-ci.org/duckie/cpp_project_template)
[![Appveyor Build Status](https://ci.appveyor.com/api/projects/status/ik18h87j8fg6s968?svg=true)](https://ci.appveyor.com/project/duckie/cpp-project-template)
[![codecov.io](http://codecov.io/github/duckie/cpp_project_template/coverage.svg?branch=master)](http://codecov.io/github/duckie/cpp_project_template?branch=master)
[![Documentation Status](https://readthedocs.org/projects/cpp-project-template/badge/?version=latest)](http://cpp-project-template.readthedocs.io/en/latest/?badge=latest)



This is a layout to bootstrap a complete C++ project with several modules, test code and various tooling to get on a sane development workflow. The directory layout looks over engineered for so small a source code, but it aims at being able to scale with a lot of files, modules and 3rdparties while avoiding clutter and name conflicts.

The actual source has three modules:
* *projectlib* is the business code of a library, with public headers and compilation units. The `projectlib` directory name is replicated for public headers to keep consistency with system wide deployment through a package.
* *projectrun* contains only compilation units and is a business executable.
* *testlib* contains only compilation units (yet) and is supposed to hold test code, objects and tools shared by the other modules.

Non system third parties are supposed to be stored or referenced (either via copy, submodule or _CMake_ external project) in the `3rdparty` directory. System third parties may be referenced to directly with _CMake_ modules.

Tooling includes:
* Automatic configuration for [YouCompleteMe](https://github.com/Valloric/YouCompleteMe)
* Code coverage with `gcov` and `lcov`
* Code formatting with `clang-format`
* Code checking with `clang-tidy`
* Cloud continuous integration
* Sanitizers builds
* A trick to prefix every test executable with your own command, with arguments if you want (ex: `valgrind --leak-check=full`) without using the strange CMake Dart thing.
* Documentation generation with [Doxygen](http://doxygen.org) and [Sphinx](http://www.sphinx-doc.org)+[Breathe](http://breathe.readthedocs.io).

This project is distributed under the [MIT license](https://opensource.org/licenses/MIT).

### Simple compilation

```bash
mkdir build
cd build
cmake ../
make
ctest
./bin/runcppproject
```

The cmake output will prompt about detected tools and availabe targets to use them.


### clang-tidy

To execute `clang-tidy` on every file compiled by the project, from the build dir :

```bash
./run-clang-tidy.py
```

It will execute `clang-tidy` with the same `-I` and `-D` options from your CMake files. Any additional argument passed to the script is forwarded. Example:

```bash
./run-clang-tidy.py -checks=*
```

### Sanitizer builds

To make a sanitizer build, customize `cmake/SanitizerBlacklist.txt.in` at your will then

```bash
cmake ../ -DPROJECT_BUILD_SANITIZER_TYPE=${sanitizer_type}
```

Where `sanitizer_type` is either `address`, `memory`, `thread` or `undefined`.

### Prefixing tests

A function called `project_add_test` has the exact same signature than the built-in `add_test`. Tests added with it are prefixed before launch with the content of the `PROJECT_TEST_COMMANDS_PREFIX` variable. Example:


```bash
cmake ../ -DPROJECT_TEST_COMMANDS_PREFIX="valgrind --leak-check=full"
```
