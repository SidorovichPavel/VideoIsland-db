cmake_minimum_required(VERSION 3.5.0)
project(videoisland-db VERSION 0.1.0 LANGUAGES C CXX)
set(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} -std=gnu++23)
# include(CTest)
# enable_testing()

include(FindBoost)
find_package(Boost COMPONENTS system filesystem date_time)

find_package(SOCI)

find_package(PkgConfig REQUIRED)
pkg_check_modules(PC_HTTPSERVER REQUIRED IMPORTED_TARGET
				  libhttpserver>=0.19)
pkg_check_modules(PC_MICROHTTPD REQUIRED IMPORTED_TARGET
				  libmicrohttpd>=0.9.52)

add_executable(videoisland-db main.cpp)

target_link_libraries(videoisland-db ${SOCI_LIBRARY})
target_link_libraries(videoisland-db
	PRIVATE
		${PC_HTTPSERVER}
		PkgConfig::PC_MICROHTTPD
)

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)
