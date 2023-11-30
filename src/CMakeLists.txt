add_executable(gst-rtsp-client gst-rtsp-client.c)
target_link_libraries(gst-rtsp-client PUBLIC GST_RTSP_SERVER)

install(TARGETS gst-rtsp-client
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib/static
)
