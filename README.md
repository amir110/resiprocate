# Overview

The reSIProcate repository contains the following C++ libraries and applications:
* resip library: comprehensive (RFC3261) SIP stack
* dum (Dialog Usage Manager) library: high level SIP library for creating SIP user agents (no media stack)
* recon library: high level SIP UA library with media stack integration
* rePro application: SIP Proxy server
* reTurn application: STUN/TURN server
* MOHParkServer appplication: a SIP based Music-on-hold server using recon
* reConServer application: a SIP based B2BUA server with media using recon
* reTurn library: STUN/TURN client library
* tfm library: a SIP based SIP framework

Please see the following wiki for more information: www.resiprocate.org


# CMake Instructions

## Building on *nix Systems

To create an in-tree build:
Navigate to git root folder where top level CMakeLists.txt file is
```
$ cmake .
$ make
```

To create an _out of tree_ build:
```
$ mkdir cmake_build # Or any other name
$ cd cmake_build
$ cmake ..
$ make
```

Once this is built, you can run the unit tests with:
```
$ ctest
```
or, for verbose output:
```
$ ctest -V
```

If you want to start fresh either delete the out of tree build directory or
delete the CMakeCache.txt file.

### Tips
* Some linux packages can be hard to track down.  If you don't need the features that enabling these packages provides you can disable them in the build.  One such package is QPID Proton.
```
$ cmake -DBUILD_QPID_PROTON=OFF .
```

### Required Packages For Default Enabled CMake Settings
* git
* make
* cmake
* clang or g++ (resip CI uses clang-13, lld-13, g++-10)
* gperf
* libssl-dev (WITH_SSL)
* libc-ares-dev (WITH_C_ARES)
* libasio-dev
* libboost-all-dev
* libdb++-dev
* libsrtp2-dev
* libgeoip-dev (USE_MAXMIND_GEOIP)
* libpopt-dev (USE_POPT)
* libcppunit-dev (BUILD_TFM)
* libnetxx-dev (BUILD_TFM)
* libqpid-proton-cpp12-dev (BUILD_QPID_PROTON)
* sox (REGENERATE_MEDIA_SAMPLES)
* xxd (REGENERATE_MEDIA_SAMPLES)

### Optional Packages
* default-libmysqlclient-dev (USE_MYSQL)
* libfmt-dev (USE_FMT)
* libgloox-dev (BUILD_ICHAT_GW)
* libgstreamermm-1.0-dev (USE_GSTREAMER)
* postgresql-server-dev-all (USE_POSTGRESQL)
* libradcli-dev (RESIP_HAVE_RADCLI)
* libsipxtapi-dev (USE_SIPXTAPI)
* libsnmp-dev (USE_NETSNMP)
* libpq-dev (USE_SOCI_POSTGRESQL)
* libsoci-dev (USE_SOCI_POSTGRESQL)
* libtelepathy-qt5-dev (BUILD_TELEPATHY_CM)
* libwebsocketpp-dev (USE_KURENTO)
* libxerces-c-dev
* perl
* python3-cxx-dev (BUILD_PYTHON)
* python3-dev (BUILD_PYTHON)


## Building on Windows Systems

### Using Visual Studio GUI

Navigate to git root folder where top level CMakeLists.txt file is.
```
> cmake .
```
Or to build recon with sipXtapi media support you need to do the following:
```
> cmake . -DUSE_SIPXTAPI=ON
```
Or if you plan on using VS GUI you might want to enable the following to get the full sipXtapi projects into the resip solution file:
```
> cmake . -DUSE_SIPXTAPI=ON -DSIPXTAPI_PROJS_IN_VS_GUI=ON
```

* Open resiprocate.sln (from _build folder) in visual studio:  Build -> Build Solution
* Note: If you used -DSIPXTAPI_PROJS_IN_VS_GUI=ON then you must build once before the sipXtapi projects are fetched from GitHub.  After this reload the resiprocate.sln and build again.  This is only needed after initial setting up the build.
* If you want to run the unit tests, then right click on the RUN_TESTS project and Build it.


### Using CMake from command line

Navigate to git root folder where top level CMakeLists.txt file is.
```
> cmake . -B _build
```
OR to build recon with sipXtapi media support you need to do the following:
```
> cmake . -B _build -DUSE_SIPXTAPI=ON
```

To build navigate to git root folder where top level CMakeLists.txt file is.
```
> cmake --build _build --config Debug --parallel
```
For unit tests:
```
> cd _build
> ctest --build-config Debug --output-on-failure
```

### Using Ninja via Visual Studio GUI
* Open the resiprocate folder by using Visual Studio Open Folder feature
* Use CMake GUI for changing CMake build settings and options: Project->CMake Settings for resiprocate
* Build -> Build All


## Configuration Flags

CMake "cached" variables are used to specify options such as whether c-ares
should be used or not. You can get the list with:

```
$ cmake -LH
```

You can set them on the command line like:

```
$ cmake -DWITH_C_ARES=true .
```

# Future Considerations

## Installation Packages

CMake supports rpm/deb?/NSIS to build installable packages using CPack. If this
built-in support can not be used we could always specify internal build targets
that will run external commands to package the appropriate bundle depending on
the platform.
