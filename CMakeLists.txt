PROJECT (uac)
CMAKE_MINIMUM_REQUIRED (VERSION 2.6)
INCLUDE (CMakeMacro.cmake)

USE_PACKAGE (expat expat.h)
USE_PACKAGE (MAFSA MAFSA/automaton.h)
USE_PACKAGE (coda coda/coda.h)

ADD_LIBRARY (uac STATIC uac/sla.c uac/uac.cpp uac/uac-base.c)
ADD_LIBRARY (uacbase STATIC uac/sla.c uac/uac-base.c)

ADD_EXECUTABLE (uac_compiler uac/uac_compiler.cpp)
TARGET_LINK_LIBRARIES (uac_compiler uac ${LIB_coda} ${LIB_expat})
ADD_CUSTOM_COMMAND (TARGET uac_compiler POST_BUILD COMMAND ./uac_compiler ${CMAKE_CURRENT_SOURCE_DIR}/uac/uac.xml uac.automaton)

ADD_EXECUTABLE (uac_detect uac/uac_detect.cpp)
TARGET_LINK_LIBRARIES (uac_detect uac ${LIB_coda} ${LIB_MAFSA} ${LIB_expat})

INSTALL (FILES uac/uac.h DESTINATION include/uac)
INSTALL (TARGETS uac DESTINATION lib)
INSTALL (TARGETS uacbase DESTINATION lib)
INSTALL (FILES uac/uac.xml ${CMAKE_CURRENT_BINARY_DIR}/uac.automaton DESTINATION share/uac)
INSTALL (TARGETS uac_detect uac_compiler DESTINATION bin)

