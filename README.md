# Additional processes definition in CepGen
This repository provides a simple skeletton of an implementation of a C++ process example in CepGen.

As for other CepGen-derived projects, the build system is [CMake](https://cmake.org/), and its "standard recipe".

If you are not using a precompiled version of CepGen (i.e. your version of CepGen is [built from sources](https://cepgen.hepforge.org/install)), the `CEPGEN_PATH` environment variable should be set to the base source directory:

```bash
export CEPGEN_PATH=/path/to/cepgen/install
```

> [!NOTE]
> On LXPLUS, CepGen is part of the LCG standard stack since 2023. The setting up of your own instance is therefore not necessary, as it can be retrieved automatically using e.g. one of the nightly builds for cc9/gcc11:
> ```bash
> export CEPGEN_PATH=/cvmfs/sft.cern.ch/lcg/nightlies/dev4/Sat/MCGenerators/cepgen/1.2.5patch2/x86_64-el9-gcc11-opt/
> ```

Then, the usual CMake build recipe can be used:

```bash
mkdir build
cd build
cmake ..
make [-j]
```

The resulting object is a shared library containing all requirements to define a process in CepGen's runtime environment.
By default, the library name is set to `CepGenProcessExample`, overridable through the `-DPROCESS_NAME=MyProcessName` addition to the `cmake` command above, or directly in the `CMakeLists.txt` file.

Once compiled and linked into a `.so`, or `.dll` shared library, it can be loaded into the CepGen runtime environment and added into the processes collection through the `-a` argument of CepGen's main executable, or using the `cepgen::loadLibrary` statement in custom user-defined executables.

Two examples of steering cards for the factorised process case described [in the CepGen documentation website](https://cepgen.hepforge.org/processes#factorised-processes) are given, both for collinear partons emission, or for $k_{\rm T}$-factorised fluxes.

In the earlier case, one may e.g. use:
```bash
$CEPGEN_PATH/bin/cepgen -a libCepGenFortranProcess.so -i cards/dummy_coll_cfg.py
```

> [!NOTE]
> In case you have trouble running the `cepgen` executable (e.g. `$CEPGEN_PATH/bin/cepgen`) due to a failure to open shared object files, you may enforce the dynamic linking and python search paths to find your `CEPGEN_PATH`, e.g.:
> ```bash
> export LD_LIBRARY_PATH=$CEPGEN_PATH/lib64:$LD_LIBRARY_PATH
> export PYTHONPATH=$CEPGEN_PATH/python:$PYTHONPATH
> ```
