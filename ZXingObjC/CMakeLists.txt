cmake_minimum_required( VERSION 3.5 )

project( ZXingObjC VERSION 3.4.5 )

option( ZXINGOBJC_AZTEC "Aztec barcode support" TRUE )
option( ZXINGOBJC_DATAMATRIX "Datamatrix support" TRUE )
option( ZXINGOBJC_MAXICODE "Maxicode support" TRUE )
option( ZXINGOBJC_ONED "1D barcode support" TRUE )
option( ZXINGOBJC_PDF417 "PDF417 support" TRUE )
option( ZXINGOBJC_QRCODE "QR code support" TRUE )

add_subdirectory( client )
add_subdirectory( common )
add_subdirectory( core )

add_subdirectory( aztec )
add_subdirectory( datamatrix )
add_subdirectory( maxicode )
add_subdirectory( multi )
add_subdirectory( oned )
add_subdirectory( pdf417 )
add_subdirectory( qrcode )

set( HEADER_FILES
        ZXMultiFormatReader.h
        ZXMultiFormatWriter.h )

set( SOURCE_FILES
        ZXMultiFormatReader.m
        ZXMultiFormatWriter.m )

add_library( zxing SHARED ${HEADER_FILES} ${SOURCE_FILES} )

target_link_libraries( zxing PRIVATE client common core multi )
find_library( FOUNDATION_FRAMEWORK Foundation )
find_library( AVFOUNDATION_FRAMEWORK AVFoundation )
find_library( QUARTZCORE_FRAMEWORK QuartzCore )
target_link_libraries( zxing PRIVATE ${FOUNDATION_FRAMEWORK} ${AVFOUNDATION_FRAMEWORK} ${QUARTZCORE_FRAMEWORK} )

if( CMAKE_SYSTEM_NAME STREQUAL Darwin )
    find_library( COCOA_FRAMEWORK Cocoa )
    find_library( COREMEDIA_FRAMEWORK CoreMedia )
    target_link_libraries( zxing PRIVATE ${COCOA_FRAMEWORK} ${COREMEDIA_FRAMEWORK} )
endif()

if( CMAKE_SYSTEM_NAME STREQUAL iOS )
    find_library( UIKIT_FRAMEWORK UIKit )
    find_library( COREGRAPHICS_FRAMEWORK CoreGraphics )
    find_library( COREVIDEO_FRAMEWORK CoreVideo )
    find_library( IMAGEIO_FRAMEWORK ImageIO )
    target_link_libraries( zxing
        PRIVATE
            ${COREGRAPHICS_FRAMEWORK}
            ${COREVIDEO_FRAMEWORK}
            ${IMAGEIO_FRAMEWORK}
            ${UIKIT_FRAMEWORK} )
endif()

set( ZXINGOBJC_PUBLIC_HEADERS )
list( APPEND ZXINGOBJC_PUBLIC_HEADERS ${CMAKE_CURRENT_LIST_DIR}/core/ZXingObjCCore.h )
list( APPEND ZXINGOBJC_PUBLIC_HEADERS ${CMAKE_CURRENT_BINARY_DIR}/ZXingObjC.h )

target_compile_definitions( zxing PUBLIC ZXINGOBJC_USE_SUBSPECS )
if( ZXINGOBJC_AZTEC )
    target_compile_definitions( zxing PUBLIC ZXINGOBJC_AZTEC )
    target_link_libraries( zxing PRIVATE aztec )
    list( APPEND ZXINGOBJC_PUBLIC_HEADERS ${CMAKE_CURRENT_LIST_DIR}/aztec/ZXingObjCAztec.h )
endif( ZXINGOBJC_AZTEC )

if( ZXINGOBJC_DATAMATRIX )
    target_compile_definitions( zxing PUBLIC ZXINGOBJC_DATAMATRIX )
    target_link_libraries( zxing PRIVATE datamatrix )
    list( APPEND ZXINGOBJC_PUBLIC_HEADERS ${CMAKE_CURRENT_LIST_DIR}/datamatrix/ZXingObjCDataMatrix.h )
endif( ZXINGOBJC_DATAMATRIX )

if( ZXINGOBJC_MAXICODE )
    target_compile_definitions( zxing PUBLIC ZXINGOBJC_MAXICODE )
    target_link_libraries( zxing PRIVATE maxicode )
    list( APPEND ZXINGOBJC_PUBLIC_HEADERS ${CMAKE_CURRENT_LIST_DIR}/maxicode/ZXingObjCMaxiCode.h )
endif( ZXINGOBJC_MAXICODE )

if( ZXINGOBJC_ONED )
    target_compile_definitions( zxing PUBLIC ZXINGOBJC_ONED )
    target_link_libraries( zxing PRIVATE oned )
    list( APPEND ZXINGOBJC_PUBLIC_HEADERS ${CMAKE_CURRENT_LIST_DIR}/oned/ZXingObjCOneD.h )
endif( ZXINGOBJC_ONED )

if( ZXINGOBJC_PDF417 )
    target_compile_definitions( zxing PUBLIC ZXINGOBJC_PDF417 )
    target_link_libraries( zxing PRIVATE pdf417 )
    list( APPEND ZXINGOBJC_PUBLIC_HEADERS ${CMAKE_CURRENT_LIST_DIR}/pdf417/ZXingObjCPDF417.h )
endif( ZXINGOBJC_PDF417 )

if( ZXINGOBJC_QRCODE )
    target_compile_definitions( zxing PUBLIC ZXINGOBJC_QRCODE )
    target_link_libraries( zxing PRIVATE qrcode )
    list( APPEND ZXINGOBJC_PUBLIC_HEADERS ${CMAKE_CURRENT_LIST_DIR}/qrcode/ZXingObjCQRCode.h )
endif( ZXINGOBJC_QRCODE )

list( APPEND ZXINGOBJC_PUBLIC_HEADERS ${CMAKE_CURRENT_LIST_DIR}/ZXMultiFormatReader.h )
list( APPEND ZXINGOBJC_PUBLIC_HEADERS ${CMAKE_CURRENT_LIST_DIR}/ZXMultiFormatWriter.h )
set_target_properties( zxing PROPERTIES
        FRAMEWORK TRUE
        LIBRARY_OUTPUT_NAME ZXingObjC
        MACOSX_FRAMEWORK_IDENTIFIER com.zxing.ZXingObjC
        PUBLIC_HEADER "${ZXINGOBJC_PUBLIC_HEADERS}" )

target_sources( zxing PUBLIC ${ZXINGOBJC_PUBLIC_HEADERS} )

foreach( ZXINGOBJC_PUBLIC_HEADER ${ZXINGOBJC_PUBLIC_HEADERS} )
    get_filename_component( ZXINGOBJC_SUBSPEC ${ZXINGOBJC_PUBLIC_HEADER} NAME )
    string( APPEND ZXINGOBJC_UMBRELLA_IMPORTS "#import <ZXingObjC/${ZXINGOBJC_SUBSPEC}>\n" )
endforeach()
configure_file( ZXingObjC.h.in ${CMAKE_CURRENT_BINARY_DIR}/ZXingObjC.h )
