# gst-rtsp-client
gst-launch enhanced to allow tweaking buried properties in elements under rtspsrc

gstreamer's rtspsrc element lacks properties to completely configure the rtpbin and rtpjitterbuffer contained within.
This is merely a version of gst-launch-1.0 enhanced to:
  1)  Find the rtspsrc element named rtspsrc
  2)  Modify the max-disorder-time of its rtpbin element
  3)  Modify the following properties of its rtpjitterbuffer element:
  4)     rtx-min-delay, rtx-delay-reorder, rtxMinRetryTimeout

These are all set to hardcode values for now.
Future plans include parsing property key/value pairs on the command line.


## Building

You need to have [CMake](https://cmake.org/) built and installed. If you are running Debian or Ubuntu, install the following packages (provided as a bash command for copy-paste convenience):

```bash
sudo apt install -y cmake libglib2.0-dev libgstreamer1.0-dev
```

Go to the source directory and run:

```bash
mkdir build
cd build
cmake ..
make
```

You may run the server from the build directory:

```bash
cd src
./gst-rtsp-client --help
```

...or install the binary in your system:

```bash
sudo make install
gst-rtsp-client --help
```
