#
#  Copyright (c) 2013, PAL Robotics, S.L. Author: Gennaro Raiola, Karsten Knese
#  Copyright 2010 CNRS
#

CMAKE_MINIMUM_REQUIRED(VERSION 2.6)

project(dyn_oppoint_modifier)

find_package(catkin REQUIRED COMPONENTS)
find_package(PkgConfig REQUIRED)

catkin_package(
	INCLUDE_DIRS include
	LIBRARIES dyn_oppoint_modifier
	DEPENDS jrl-mathtools jrl-mal dynamic-graph dynamic-graph-python sot-core
)

include_directories(include ${catkin_INCLUDE_DIRS})

add_library(dyn_oppoint_modifier
	src/DynOppointModifier.cpp
	include/sot-core/DynOppointModifier.hh
)
target_link_libraries(dyn_oppoint_modifier ${catkin_LIBRARIES})

set(PKG_DEP dynamic-graph dynamic-graph-python jrl-mathtools jrl-mal sot-core)
foreach(DEP ${PKG_DEP})
	message("invoking dependency check for: ${DEP}")
	pkg_check_modules(${DEP} REQUIRED ${DEP})
	message("use dependency for:  ${${DEP}_LDFLAGS}")
	target_link_libraries(dyn_oppoint_modifier ${${DEP}_LDFLAGS})
endforeach(DEP)

# ADD SUBDIRECTORY FOR PYTHON WRAPPER EXPORT
add_subdirectory(src)


install(TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)


