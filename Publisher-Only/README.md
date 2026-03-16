# Publisher Only

The Publisher Only sample application is a very simple application meant to get
a new developer started using the Vonage Linux SDK.

Upon building and running this sample application, you should be able to have
one-way audio and video communication using Vonage. This sample application is
intended to be used in a tiny single-board computer, such as the Raspberry Pi.

You will need a valid [Vonage Video API](https://developer.vonage.com/)
account to build this app.

## Building and running the sample app

Let's build this sample for the Raspberry Pi device. For now the SDK build for
this device is distributed via a direct download in the Vonage developer center
as a `tgz` package.

Before building the sample application you have to download the Vonage Linux SDK
package and uncompress it to a local folder you create (such as /assets):

```bash
$ cd assets; wget https://tokbox.com/downloads/libopentok_linux_llvm_armv7-2.32.1
```

Uncompress the Vonage Linux SDK package in the [assets](../assets) folder.
Uncompress the version of the SDK for the target host of your sample application.
For example, for ARMv7 uncompress the libopentok_linux_llvm_armv7 tgz file:

```bash
$ cd assets; unp libopentok_linux_llvm_armv7-2.32.1.tgz
```

This will create a `libopentok_linux_llvm_armv7` folder at
[assets/libopentok_linux_llvm_armv7](../assets/libopentok_linux_llvm_armv7). The
build system will try find the Vonage Linux SDK header files and library under
the `assets` folder.

The folder above contains a handy `CMakeListz.txt` file the developer can use if
`CMake` is used in the application. The developer would need to add it via the
`ADD_SUBDIRECTORY()` command, add `$<TARGET_PROPERTY:libopentok,INTERFACE_INCLUDE_DIRECTORIES>`
to the list of include folders with the `INCLUDE_DIRECTORIES()` command and
finally add `libopentok` to the list of target link libraries.

```
  ADD_SUBDIRECTORY(${CMAKE_CURRENT_SOURCE_DIR}/../assets/libopentok_linux_llvm_armv7 ${CMAKE_CURRENT_BINARY_DIR}/libopentok)

  INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR}/../common/src ${LIBSDL2_INCLUDE_DIRS} $<TARGET_PROPERTY:libopentok,INTERFACE_INCLUDE_DIRECTORIES>)
  LINK_DIRECTORIES(${LIBSDL2_LIBRARY_DIRS})

  ADD_EXECUTABLE(${PROJECT_NAME} main.cpp ${CMAKE_CURRENT_SOURCE_DIR}/../common/src/renderer.cpp)
  TARGET_LINK_LIBRARIES(${PROJECT_NAME} ${LIBSDL2_LIBRARIES} libopentok)
```

This application uses [CMake](https://cmake.org). Before building
the sample application, install it and these other dependencies:

  - build-essential
  - cmake
  - clang
  - libc++-dev
  - libc++abi-dev
  - pkg-config
  - libasound2
  - libpulse-dev
  - libsdl2-dev

Next, we will build the sample application. Since it's good practice to create a build
folder, let's go ahead and create it in the project directory:

```bash
$ mkdir Publisher-Only/build
```

Copy the [config-sample.h](../common/src/config-sample.h) file as `config.h` at
`Publisher-Only/`:

```bash
$ cp common/src/config-sample.h  Publisher-Only/config.h
```

Edit the `config.h` file and add your Vonage Application Id, Vonage session ID, and token for that session. For test purposes,
you can obtain a session ID and token from the project page in your
[Vonage Dashboard](https://dashboard.vonage.com/). However,
in a production application, you will need to dynamically obtain the session
ID and token from a web service that uses one of
the [Vonage Video API server SDKs](https://developer.vonage.com/en/video/server-sdks/overview).

Next, create the building bits using `cmake`:

```
$ cd Publisher-Only/build
$ CC=clang CXX=clang++ cmake ..
```

Note we are using `clang/clang++` compilers.

Use `make` to build the code:

```bash
$ make
```

When the `publisher_only` binary is built, run it:

```bash
$ ./publisher_only
```

You can use the [Vonage Playground](https://tools.vonage.com/video/playground)
to connect to the Vonage session in a web browser and view the stream published
by the application.

You can end the sample application by typing Control + C in the console.

## Understanding the code

This application uses the same concepts that
the [Basic Video Chat](../Basic-Video-Chat) application uses. See the
[Understanding the code section](../Basic-Video-Chat/README.md#understanding-the-code)
of that application's README file (and disregard information about subscribing
to streams) to see how the code uses the Vonage Linux SDK.

## Next steps

The [Basic Video Chat application](../Basic-Video_Chat) sample builds upon the
Publisher Only sample, adding an Vonage stream subscriber in addition to a publisher. 

See the [Vonage Video API developer center](https://developer.vonage.com/)
for more information on the Vonage Linux SDK.
