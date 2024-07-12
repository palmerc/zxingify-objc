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
    
add_subdirectory( encoder )
add_subdirectory( decoder )
add_subdirectory( detector )
