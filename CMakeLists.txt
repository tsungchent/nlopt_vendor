cmake_minimum_required(VERSION 3.5)
project(nlopt_vendor)

find_package(ament_cmake REQUIRED)

include(ExternalProject)

# We can't check version numbers easily as the nlopt script doesn't export them.
# file(MAKE_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
set(VERSION 2.7.1)

ExternalProject_Add(nlopt_src
  GIT_REPOSITORY https://github.91chi.fun//https://github.com/stevengj/nlopt.git
  GIT_TAG v${VERSION}
  UPDATE_COMMAND ""
  CONFIGURE_COMMAND cd ../nlopt_src && 
    cmake .
      -DCMAKE_INSTALL_PREFIX:PATH=${CMAKE_CURRENT_BINARY_DIR}/nlopt_install
      -DCMAKE_BUILD_TYPE:STRING=Release
  BUILD_COMMAND cd ../nlopt_src && cmake --build . --config Release
  INSTALL_COMMAND cd ../nlopt_src && cmake --install .
)
install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/nlopt_install/
        DESTINATION ${CMAKE_INSTALL_PREFIX}/)
        
ament_package(CONFIG_EXTRAS "cmake/nlopt-vendor-extras.cmake.in")