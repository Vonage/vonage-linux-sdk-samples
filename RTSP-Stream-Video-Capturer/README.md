# RTSP stream Video Capturer

This sample application shows how to use a video capturer being used by a
publisher in your application. It is important to note that a custom video
capturer always belongs to a given publisher. The video capturer captures
video frames from a RTSP stream. A publisher using this video capturer pipes the
RTSP stream into a Vonage session.

This sample builds upon the [Publisher Only](../Publisher-Only) sample, but with
a publisher that uses a RTSP stream video capturer.

You will need a valid [Vonage Video API](https://developer.vonage.com/)
account to build this app.

## Setting up your environment

### Vonage SDK

Building this sample application requires having a local installation of the
Vonage Linux SDK.

#### On Debian-based Linuxes

The Vonage Linux SDK for x86_64 is available as a Debian
package. We maintain our own Debian repository on packagecloud. 
For Debian 13, follow these steps to install the packages from our repository.

* Add packagecloud repository:

```bash
curl -s https://packagecloud.io/install/repositories/tokbox/debian/script.deb.sh | sudo bash
```

* Install the Vonage Linux SDK packages.

```bash
sudo apt install libopentok-dev
```

#### On non-Debian-based Linuxes

Download the Vonage SDK from [Vonage developer portal](https://developer.vonage.com/en/video/client-sdks/linux/overview#tgz-packages)
and extract it and set the `LIBOPENTOK_PATH` environment variable to point to the path where you extracted the SDK.
For example:

```bash
wget https://tokbox.com/downloads/libopentok_linux_llvm_x86_64-2.32.1
tar xvf libopentok_linux_llvm_x86_64-2.32.1
export LIBOPENTOK_PATH=<path_to_SDK>
```

## Other dependencies

Before building the sample application you will need to install the following dependencies

### On Debian-based Linuxes

```bash
sudo apt install build-essential cmake clang libc++-dev libc++abi-dev \
    pkg-config libasound2 libpulse-dev libsdl2-dev libopencv-dev
```

### On Fedora

```bash
sudo dnf groupinstall "Development Tools" "Development Libraries"
sudo dnf install SDL2-devel clang pkg-config libcxx-devel libcxxabi-devel cmake opencv-devel
```

Note that this sample app has been tested to work with OpenCV 4.5.

## Building and running the sample app


Once you have installed the dependencies, you can build the sample application.
Since it's good practice to create a build folder, let's go ahead and create it
in the project directory:

```bash
$ mkdir RTSP-Stream-Video-Capturer/build
```

Copy the [config-sample.h](../common/src/config-sample.h) file as `config.h` at
`RTSP-Stream-Video-Capturer/`:

```bash
$ cp common/src/config-sample.h  RTSP-Stream-Video-Capturer/config.h
```

Edit the `config.h` file and add your Vonage Application Id, Vonage session ID, and token for that session. For test purposes,
you can obtain a session ID and token from the project page in your
[Vonage Dashboard](https://dashboard.vonage.com/). However,
in a production application, you will need to dynamically obtain the session
ID and token from a web service that uses one of
the [Vonage Video API server SDKs](https://developer.vonage.com/en/video/server-sdks/overview).

Edit the `main.cpp` file and the RTSP URL you want to capture video frames from.
```
#define DEFAULT_RTSP_SERVER_URL "<My RTSP URL>"
```

If you do not have a RTSP camera avaible you can install VLC to run a RTSP server. For example,

```
vlc -vvv SampleVideo_720x480_20mb.mp4 --sout '#rtp{dst=127.0.0.1,port=1234,sdp=rtsp://127.0.0.1:8080/test.sdp}'
```
and then the RTSP URL would be
```
#define DEFAULT_RTSP_SERVER_URL "rtsp://127.0.0.1:8080/test.sdp"
```

Next, create the building bits using `cmake`:

```bash
$ cd RTSP-Stream-Video-Capturer/build
$ CC=clang CXX=clang++ cmake ..
```

Note we are using `clang/clang++` compilers.

Use `make` to build the code:

```bash
$ make
```

When the `rtsp_stream_video_capturer` binary is built, run it:

```bash
$ ./rtsp_stream_video_capturer
```

You can use the [Vonage Playground](https://tools.vonage.com/video/playground)
to connect to the Vonage session in a web browser, view the stream published
by the RTSP Stream Video Capturer app, and publish a stream that the app can subscribe to.

You can end the sample application by typing Control + C in the console.

## Next steps

See the [Vonage Video API developer center](https://developer.vonage.com/)
for more information on the Vonage Linux SDK.
