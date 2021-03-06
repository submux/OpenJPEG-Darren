IF(BUILD_CODEC OR BUILD_MJ2)
#
#
IF(UNIX OR CYGWIN)
 SET(CMAKE_INCLUDE_PATH /usr/include /usr/local/include /opt/include 
	/opt/local/include /usr/include/libpng /usr/include/libpng14
	/usr/include/libpng12 /usr/local/include/libpng 
	/usr/local/include/libpng14 /usr/local/include/libpng12
	/opt/include/libpng /opt/include/libpng14 /opt/include/libpng12
	/opt/local/include/libpng /opt/local/include/libpng14)
 SET(CMAKE_LIBRARY_PATH /usr/lib /usr/local/lib /opt/lib /opt/local/lib)
ENDIF()
#
 FIND_PACKAGE(ZLIB)
#
 IF(ZLIB_LIBRARY STREQUAL "ZLIB_LIBRARY-NOTFOUND")
   SET(ZLIB_FOUND 0)
 ENDIF(ZLIB_LIBRARY STREQUAL "ZLIB_LIBRARY-NOTFOUND")
#
 IF(ZLIB_FOUND)
   SET(HAVE_ZLIB_H 1)
   SET(HABE_LIBZ 1)
   SET(Z_LIBNAME ${ZLIB_LIBRARIES})
   SET(Z_INCLUDE_DIRNAME ${ZLIB_INCLUDE_DIR}) 
 ENDIF(ZLIB_FOUND)
#
 FIND_PACKAGE(PNG)
#
 IF(PNG_LIBRARY STREQUAL "PNG_LIBRARY-NOTFOUND")
   SET(PNG_FOUND 0)
 ENDIF(PNG_LIBRARY STREQUAL "PNG_LIBRARY-NOTFOUND")
#
 IF(PNG_FOUND)
   SET(HAVE_PNG_H 1)
   SET(HAVE_LIBPNG 1)
   SET(PNG_LIBNAME ${PNG_LIBRARIES})
   SET(PNG_INCLUDE_DIRNAME ${PNG_INCLUDE_DIR})
 ENDIF(PNG_FOUND)
#
 FIND_PACKAGE(TIFF)
#
 IF(TIFF_LIBRARY STREQUAL "TIFF_LIBRARY-NOTFOUND")
   SET(TIFF_FOUND 0)
 ENDIF(TIFF_LIBRARY STREQUAL "TIFF_LIBRARY-NOTFOUND")
#
 IF(TIFF_FOUND)
   SET(HAVE_TIFF_H 1)
   SET(HAVE_LIBTIFF 1)
   SET(TIFF_LIBNAME ${TIFF_LIBRARIES})
   SET(TIFF_INCLUDE_DIRNAME ${TIFF_INCLUDE_DIR})
 ENDIF(TIFF_FOUND)
#
 SET(LCMS12_MISSING 1)
 INCLUDE(${OPENJPEG_SOURCE_DIR}/thirdparty/FindLCMS2.cmake OPTIONAL)
#
 IF(LCMS2_FOUND)
  SET(LCMS12_MISSING 0)
  SET(HAVE_LCMS2_H 1)
  SET(HAVE_LIBLCMS2 1)
  SET(LCMS_LIBNAME ${LCMS2_LIBRARY})
  SET(LCMS_INCLUDE_DIRNAME ${LCMS2_INCLUDE_DIR})
 ENDIF(LCMS2_FOUND)
#
 IF(NOT LCMS2_FOUND)
  INCLUDE(${OPENJPEG_SOURCE_DIR}/thirdparty/FindLCMS.cmake OPTIONAL)
#
  IF(LCMS_FOUND)
   SET(LCMS12_MISSING 0)
   SET(HAVE_LCMS1_H 1)
    SET(HAVE_LIBLCMS1 1)
    SET(LCMS_LIBNAME ${LCMS_LIBRARY} )
    SET(LCMS_INCLUDE_DIRNAME ${LCMS_INCLUDE_DIR})
  ENDIF(LCMS_FOUND)
 ENDIF(NOT LCMS2_FOUND)
#-------------------------------------------------------------
 OPTION(BUILD_THIRDPARTY "Build the thirdparty executables" ON)
#
 IF(NOT ZLIB_FOUND OR NOT PNG_FOUND OR NOT TIFF_FOUND OR LCMS12_MISSING) 
  IF(BUILD_THIRDPARTY)
   SET(HAVE_ZLIB_H 1)
   SET(HAVE_LIBZ 1)
   SET(HAVE_PNG_H 1)
   SET(HAVE_LIBPNG 1)
   SET(HAVE_TIFF_H 1)
   SET(HAVE_LIBTIFF 1)
   SET(HAVE_LCMS2_H 1)
   SET(HAVE_LIBLCMS2 1)
#
#ADD_SUBDIRECTORY(${OPENJPEG_SOURCE_DIR}/thirdparty)
#
   LINK_DIRECTORIES(${CMAKE_BINARY_DIR}/thirdparty/lib)
#
   IF(NOT ZLIB_FOUND)
    INCLUDE_DIRECTORIES(${OPENJPEG_SOURCE_DIR}/thirdparty/include)
    SET(ZLIB_INCLUDE_DIRNAME ${OPENJPEG_SOURCE_DIR}/thirdparty/include)
    SET(Z_LIBNAME z)
    SET(ZLIB_FOUND 1)
   ENDIF(NOT ZLIB_FOUND)
#
   IF(NOT PNG_FOUND)
    SET(PNG_INCLUDE_DIRNAME ${OPENJPEG_SOURCE_DIR}/thirdparty/libpng)
    SET(PNG_LIBNAME png)
    SET(PNG_FOUND 1)
   ENDIF(NOT PNG_FOUND)
#
   IF(NOT LCMS_FOUND AND NOT LCMS2_FOUND)
    SET(LCMS_INCLUDE_DIRNAME ${OPENJPEG_SOURCE_DIR}/thirdparty/liblcms2/include)
    SET(LCMS_LIBNAME lcms2)
    SET(LCMS2_FOUND 1)
   ENDIF(NOT LCMS_FOUND AND NOT LCMS2_FOUND)
#
   IF(NOT TIFF_FOUND)
    SET(TIFF_INCLUDE_DIRNAME ${OPENJPEG_SOURCE_DIR}/thirdparty/libtiff)
    SET(TIFF_LIBNAME tiff)
    SET(TIFF_FOUND 1)
   ENDIF(NOT TIFF_FOUND)
#
  ENDIF(BUILD_THIRDPARTY)
 ENDIF(NOT ZLIB_FOUND OR NOT PNG_FOUND OR NOT TIFF_FOUND OR LCMS12_MISSING)
#
ENDIF(BUILD_CODEC OR BUILD_MJ2)
#
IF(BUILD_CODEC)
 ADD_SUBDIRECTORY(codec)
ENDIF(BUILD_CODEC)
#
IF(BUILD_MJ2)
 ADD_SUBDIRECTORY(mj2)
ENDIF(BUILD_MJ2)
#
