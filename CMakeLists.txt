cmake_minimum_required(VERSION 3.10)
project(gst-rtsp-client)

find_library(_LIB_GLIB
  NAMES
  glib-2.0
  libglib-2.0
  libglib-2.0.so
)
find_library(_LIB_GSTREAMER
  NAMES
  gstreamer-1.0
  libgstreamer-1.0
  libgstreamer-1.0.so
)
find_library(_LIB_GOBJECT
  NAMES
  gobject-2.0
  libgobject-2.0
  libgobject-2.0.so
)
find_path(_INCLUDE_GLIB glib.h
  HINTS
  /usr/include
  /usr/include/glib-2.0
)
find_path(_INCLUDE_GST gst/gst.h
  HINTS
  /usr/include
  /usr/include/gstreamer-1.0
)

# Terrible, terrible hack to find glibconfig (which is in /usr/lib/platform-triplet/glib-2.0/include, of all places!)
if (_LIB_GLIB)
  get_filename_component(_LIBDIR_GLIB ${_LIB_GLIB} DIRECTORY)
else()
  message(FATAL_ERROR "Could not find glib")
endif()

find_path(_INCLUDE_GLIBCONFIG glibconfig.h
  HINTS
  /usr/include
  /usr/include/glib-2.0
  ${_LIBDIR_GLIB}/glib-2.0/include
)

message(STATUS "Using libglib-2.0 from ${_LIB_GLIB}")
message(STATUS "Using header files from ${_INCLUDE_GST}, ${_INCLUDE_GLIB}")

if (NOT _INCLUDE_GLIB OR NOT _INCLUDE_GLIBCONFIG)
  message(FATAL_ERROR "Could not find glib headers")
endif()

if (NOT _LIB_GSTREAMER)
  message(FATAL_ERROR "Could not find GStreamer")
endif()

if (NOT _LIB_GOBJECT)
  message(FATAL_ERROR "Could not find gobject")
endif()

add_library(GST_RTSP_SERVER INTERFACE)
target_link_libraries(GST_RTSP_SERVER INTERFACE 
  ${_LIB_GLIB}
  ${_LIB_GSTREAMER}
  ${_LIB_GOBJECT}
)
target_include_directories(GST_RTSP_SERVER INTERFACE
  ${_INCLUDE_GLIB}
  ${_INCLUDE_GLIBCONFIG}
  ${_INCLUDE_GST}
)

add_subdirectory(src)
