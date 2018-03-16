<div align="center">
  <img src="https://www.tensorflow.org/images/tf_logo_transp.png"><br><br>
</div>

-----------------

If you compile for host linux, make sure you have cmake (version >= 3.10.2), gcc (version >= 6.3). If you also compile for arm, make sure you toolchain's attribute CMAKE_SYSTEM_PROCESSOR contain "arm", or "Arm", or "ARM", as I use this attribute to detect cross-compiling. It's been tested for linux host, armv8-a. 


## Compile for host Linux

```
cd tensorflow/contrib/cmake
mkdir x86    # if you also want to cross-compile later, do NOT change the directory name.
cd x86
cmake ..  -DCMAKE_BUILD_TYPE=Release -Dtensorflow_BUILD_PYTHON_BINDINGS=OFF -Dtensorflow_BUILD_SHARED_LIB=ON
make -j$(nproc)
```

The output is in the directory "x86". The shared library is libtensorflow.so. The protoc tools sits in the sub-folder "protobuf/src/protobuf". If you want to use the c plus plus api, which needs to you run several protoc commands to generate source files. The following cross-compiling process will also invoke this command to generate source files.


## Cross-Compile

Make sure you have finished the last step and compiled a version for the host.

```
cd tensorflow/contrib/cmake
mkdir arm
cd arm
cmake ..  -DCMAKE_BUILD_TYPE=Release -Dtensorflow_BUILD_PYTHON_BINDINGS=OFF -Dtensorflow_BUILD_SHARED_LIB=ON -DCMAKE_TOOLCHAIN_FILE=<YOU TOOLCHAIN FILE>
make -j$(nproc)
```

## Others

For both of the above cmake commands, you can also pass in "-Dtensorflow_BUILD_CONTRIB_KERNELS=OFF", if you don't need it in your library.

## Details

1. changed the executable programs for cross-compling.
2. added "-DCMAKE_TOOLCHAIN_FILE=${CMAKE_TOOLCHAIN_FILE}" for third-party dependences that use cmake to build.
3. added "--host=aarch64-linux-gnu" for third-party dependences that use autoconfig/make to build.
4. changed the urls of the some third-party dependences to the ones bazel uses for building tensorflow.

## TODO

1. make a minimum runtime library for inference.
