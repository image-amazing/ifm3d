find_library(LIB_glog NAMES glog)

file(GLOB IFM3D_TOOLS_SOURCES *.cpp)

add_library(ifm3d_tools OBJECT ${IFM3D_TOOLS_SOURCES})
set_property(TARGET ifm3d_tools PROPERTY POSITION_INDEPENDENT_CODE 1)

add_library(ifm3d_tools_shared SHARED $<TARGET_OBJECTS:ifm3d_tools>)
target_link_libraries(
  ifm3d_tools_shared
  ifm3d_camera_shared
  ${LIB_glog}
  ${Boost_LIBRARIES}
  )

set_target_properties(ifm3d_tools_shared PROPERTIES
  VERSION ${IFM3D_VERSION_STRING}
  OUTPUT_NAME ifm3d_tools
  LINKER_LANGUAGE CXX
  )

install(TARGETS ifm3d_tools_shared DESTINATION lib)