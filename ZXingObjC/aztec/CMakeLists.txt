cmake_minimum_required( VERSION 3.5 )

project( ZXingObjC-aztec )

set( HEADER_FILES
    ZXAztecWriter.h
    ZXingObjCAztec.h
    ZXAztecDetectorResult.h
    ZXAztecReader.h )

set( SOURCE_FILES 
    ZXAztecDetectorResult.m
    ZXAztecReader.m
    ZXAztecWriter.m )

add_library( aztec OBJECT ${HEADER_FILES} ${SOURCE_FILES} )
set_property( TARGET aztec APPEND_STRING PROPERTY COMPILE_FLAGS -fobjc-arc )
target_link_libraries( aztec
    PRIVATE
        common
        common-detector
        common-reedsolomon
        core
        aztec-decoder
        aztec-detector
        aztec-encoder )
target_include_directories( aztec
    PUBLIC
        ${CMAKE_CURRENT_LIST_DIR} )

add_library( aztec-interface INTERFACE )
target_link_libraries( aztec-interface INTERFACE aztec )
target_sources( aztec-interface
    INTERFACE
        $<TARGET_OBJECTS:aztec>
        $<TARGET_OBJECTS:aztec-decoder>
        $<TARGET_OBJECTS:aztec-detector>
        $<TARGET_OBJECTS:aztec-encoder> )

add_subdirectory( encoder )
add_subdirectory( decoder )
add_subdirectory( detector )
