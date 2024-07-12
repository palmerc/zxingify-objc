cmake_minimum_required( VERSION 3.5 )

project( ZXingObjC-client )

set( HEADER_FILES
    ZXCaptureDelegate.h
    ZXCGImageLuminanceSourceInfo.h
    ZXImage.h
    ZXCGImageLuminanceSource.h
    ZXCapture.h )

set( SOURCE_FILES 
    ZXImage.m
    ZXCapture.m
    ZXCGImageLuminanceSource.m
    ZXCGImageLuminanceSourceInfo.m )

add_library( client OBJECT ${HEADER_FILES} ${SOURCE_FILES} )
target_link_libraries( client PRIVATE common core )
target_include_directories( client
    PUBLIC
        ${CMAKE_CURRENT_LIST_DIR} )
set_property( TARGET client APPEND_STRING PROPERTY COMPILE_FLAGS -fobjc-arc )

add_library( client-interface INTERFACE )
target_link_libraries( client-interface INTERFACE client )
target_sources( client-interface
    INTERFACE
        $<TARGET_OBJECTS:client>
        $<TARGET_OBJECTS:client-result> )
    
add_subdirectory( result )
