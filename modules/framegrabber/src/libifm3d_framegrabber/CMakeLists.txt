find_package(Boost REQUIRED COMPONENTS system date_time regex)
find_package(glog QUIET CONFIG NAMES google-glog glog)
if (NOT TARGET glog::glog)
	find_library(LIB_glog NAMES glog)
else()
	set(LIB_glog glog::glog)
endif (NOT TARGET glog::glog)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

file(GLOB IFM3D_FG_SOURCES *.cpp)

add_library(
  ifm3d_framegrabber_shared SHARED ${IFM3D_FG_SOURCES}
  )
set_property(TARGET ifm3d_framegrabber_shared PROPERTY POSITION_INDEPENDENT_CODE 1)

target_link_libraries(
  ifm3d_framegrabber_shared
  ifm3d_camera_shared
  ${LIB_glog}
  ${Boost_LIBRARIES}
  ${CMAKE_THREAD_LIBS_INIT}
  )

set_target_properties(ifm3d_framegrabber_shared PROPERTIES
  VERSION ${IFM3D_VERSION_STRING}
  OUTPUT_NAME ifm3d_framegrabber
  LINKER_LANGUAGE CXX
  )

install(TARGETS ifm3d_framegrabber_shared
  DESTINATION lib
  COMPONENT framegrabber
  )
