# CMakeList.txt: файл проекта CMake верхнего уровня; проведите глобальную конфигурацию
# и включите сюда подпроекты.
#
cmake_minimum_required (VERSION 3.2)

project (helloworld VERSION 0.0.$ENV {TRAVIS_BUILD_NUMBER})

find_package (Boost COMPONENTS unit_test_framework REQUIRED)

configure_file (version.h.in version.h)

add_executable (helloworld_cli main.cpp)

add_library (helloworld lib.cpp)

add_executable (test_main_cli tets_main.cpp)

set_target_properties (tets_main_cli PROPERTIES
	COMPILE_DEFINATION BOOST_TEST_DYN_LINK
	INCLUDE_DIRECTORIES ${Boost_INCLUDE_DIR}
)

target_link_libraries (helloworld_cli helloworld)

target_link_libraries (test_main_cli
	${Boost_LIBRARIES}
	helloworld
)

install (TARGETS helloworld_cli RUNTIME DESTINATION bin)

set (CPACK_GENERATOR_DEB)

set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")

set (CPACK_PACKAGE_CONTACT elektronchik439991@gmail.com)

include (CPack)

enable_testing()

add_test (test_version_valid tets_main_cli)
