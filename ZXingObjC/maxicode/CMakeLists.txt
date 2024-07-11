cmake_minimum_required( VERSION 3.5 )

project( ZXingObjC-maxicode )

set( HEADER_FILES
    ZXMaxiCodeReader.h
    ZXingObjCMaxiCode.h )

set( SOURCE_FILES 
    ZXMaxiCodeReader.m )

add_library( maxicode STATIC ${HEADER_FILES} ${SOURCE_FILES} )
target_link_libraries( maxicode PRIVATE common core maxicode-decoder )
target_include_directories( maxicode
    PUBLIC
        ${CMAKE_CURRENT_LIST_DIR} )
set_property( TARGET maxicode APPEND_STRING PROPERTY COMPILE_FLAGS -fobjc-arc )
    
add_subdirectory( decoder )
