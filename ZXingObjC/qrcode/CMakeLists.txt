cmake_minimum_required( VERSION 3.5 )

project( ZXingObjC-qrcode )

set( HEADER_FILES
    ZXingObjCQRCode.h
    ZXQRCodeReader.h
    ZXQRCodeWriter.h )

set( SOURCE_FILES 
    ZXQRCodeWriter.m
    ZXQRCodeReader.m )

add_library( qrcode OBJECT ${HEADER_FILES} ${SOURCE_FILES} )
target_link_libraries( qrcode
    PRIVATE
        common
        core
        qrcode-decoder
        qrcode-detector
        qrcode-encoder
        qrcode-multi
        qrcode-multi-detector )
target_include_directories( qrcode
    PUBLIC
        ${CMAKE_CURRENT_LIST_DIR} )
set_property( TARGET qrcode APPEND_STRING PROPERTY COMPILE_FLAGS -fobjc-arc )

add_library( qrcode-interface INTERFACE )
target_link_libraries( qrcode-interface INTERFACE qrcode )
target_sources( qrcode-interface
        INTERFACE
        $<TARGET_OBJECTS:qrcode>
        $<TARGET_OBJECTS:qrcode-decoder>
        $<TARGET_OBJECTS:qrcode-detector>
        $<TARGET_OBJECTS:qrcode-encoder>
        $<TARGET_OBJECTS:qrcode-multi>
        $<TARGET_OBJECTS:qrcode-multi-detector> )
    
add_subdirectory( encoder )
add_subdirectory( multi )
add_subdirectory( decoder )
add_subdirectory( detector )
