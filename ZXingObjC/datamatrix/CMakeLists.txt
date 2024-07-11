cmake_minimum_required( VERSION 3.5 )

project( ZXingObjC-datamatrix )

set( HEADER_FILES
    ZXDataMatrixWriter.h
    ZXDataMatrixReader.h
    ZXingObjCDataMatrix.h )

set( SOURCE_FILES 
    ZXDataMatrixReader.m
    ZXDataMatrixWriter.m )

add_library( datamatrix STATIC ${HEADER_FILES} ${SOURCE_FILES} )
target_link_libraries( datamatrix
    PRIVATE
        common
        core
        maxicode-decoder
        datamatrix-decoder
        datamatrix-detector
        datamatrix-encoder )
target_include_directories( datamatrix
    PUBLIC
        ${CMAKE_CURRENT_LIST_DIR} )
set_property( TARGET datamatrix APPEND_STRING PROPERTY COMPILE_FLAGS -fobjc-arc )
    
add_subdirectory( encoder )
add_subdirectory( decoder )
add_subdirectory( detector )
