cmake_minimum_required( VERSION 3.5 )

project( ZXingObjC-datamatrix )

set( HEADER_FILES
    ZXDataMatrixWriter.h
    ZXDataMatrixReader.h
    ZXingObjCDataMatrix.h )

set( SOURCE_FILES 
    ZXDataMatrixReader.m
    ZXDataMatrixWriter.m )

add_library( datamatrix OBJECT ${HEADER_FILES} ${SOURCE_FILES} )
target_link_libraries( datamatrix
    PRIVATE
        common
        core
        datamatrix-decoder
        datamatrix-detector
        datamatrix-encoder )
target_include_directories( datamatrix
    PUBLIC
        ${CMAKE_CURRENT_LIST_DIR} )
set_property( TARGET datamatrix APPEND_STRING PROPERTY COMPILE_FLAGS -fobjc-arc )

add_library( datamatrix-interface INTERFACE )
target_link_libraries( datamatrix-interface INTERFACE datamatrix )
target_sources( datamatrix-interface
    INTERFACE
        $<TARGET_OBJECTS:datamatrix>
        $<TARGET_OBJECTS:datamatrix-decoder>
        $<TARGET_OBJECTS:datamatrix-detector>
        $<TARGET_OBJECTS:datamatrix-encoder> )
    
add_subdirectory( encoder )
add_subdirectory( decoder )
add_subdirectory( detector )
