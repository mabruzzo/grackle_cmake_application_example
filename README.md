This repository illustrates an example of how one might directly embed the build of Grackle into a downstream CMake project.
This example requires version 3.16 of CMake or newer.

**NOTE:** this is mostly a proof of concept. Keep in mind this is just a proof of concept (only tested on version 20.04 of ubuntu). If your system has hdf5 or your desired compilers installed in an atypical location, there are all sorts of standard hints you could give to cmake to help prod things along. In practice, these sorts of hints are usually automatically handled by the downstream application (e.g. see Enzo-E) or are well documented by the downstream application


```sh
# clone the repository and recursively initialize all of its git submodules
git clone --recursive https://github.com/mabruzzo/grackle_cmake_application_example.git
cd grackle_cmake_application_example

# configure the build-system to build the application (and grackle) inside a
# directory called ./build (this exact name is arbitrary)
cmake -B build
# instruct cmake to actually build the example application
cmake --build build

# here, we actually execute the example program.
# - This program expects a single argument, the path to a datafile.
# - since we recursively initialized all of the submodules, we can find these
#   datafiles inside the current directory hierarchy
./build/example_app ./external/grackle/grackle_data_files/input/CloudyData_UVB=HM2012.h5
```

If you want to cleanup, you just need to recursively delete the build directory
```sh
rm -r ./build
```

By default, the build-system is configured to use a static library build of grackle. If you instead want to build grackle as a shared library, you can use the alternative commands provided below.

```sh
# in this example, we are building the application in a directory called
# ./build-alt (you are free to build it in a directory called ./build, but if
# you followed the above instructions, you should be sure to delete the ./build
# directory first)

# the key difference here: we're configuring the BUILD_SHARED_LIBS option
cmake -B build-alt -D BUILD_SHARED_LIBS=ON
cmake --build build-alt

# execute the example
./build-alt/example_app ./external/grackle/grackle_data_files/input/CloudyData_UVB=HM2012.h5
```