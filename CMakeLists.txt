cmake_minimum_required(VERSION 2.8.3)

project(qore-easyexif-module)

set (VERSION_MAJOR 1)
set (VERSION_MINOR 0)
set (VERSION_PATCH 0)

# where to look first for cmake modules, before ${CMAKE_ROOT}/Modules/ is checked
set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake )


FIND_PACKAGE (Qore REQUIRED)


include_directories( ${CMAKE_SOURCE_DIR}/src/easyexif )


set(CPP_SRC
    src/easyexif/exif.cpp
)
set(QPP_SRC
    src/easyexif.qpp
)


#set(EFSW_QMOD qlib/FilePoller.qm)


#add_definitions(-DUSE_UTF8)

qore_wrap_qpp_value(QPP_SOURCES METALIST QPP_META_SRC ${QPP_SRC})

SET (module_name "easyexif")
add_library(${module_name} SHARED ${CPP_SRC} ${QPP_SOURCES})
# easyexif 3rd party lib requires pretty new c++ standard
set_property(TARGET ${module_name} PROPERTY CXX_STANDARD 11)

qore_binary_module(${module_name} "${VERSION_MAJOR}.${VERSION_MINOR}.${VERSION_PATCH}")
#qore_user_modules(${EFSW_QMOD})

qore_dist("${VERSION_MAJOR}.${VERSION_MINOR}.${VERSION_PATCH}")

# install qpp metadata files
if (COMMAND qore_install_qpp_metadata)
    qore_install_qpp_metadata(${module_name} ${QPP_META_SRC})
else()
    install(FILES ${QPP_META_SRC}
            DESTINATION ${CMAKE_INSTALL_DATADIR}/qore/metadata)
endif()

qore_config_info()
