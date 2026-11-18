
include(${CMAKE_CURRENT_SOURCE_DIR}/../../max-sdk-base/script/max-pretarget.cmake)

set(LIBS_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../../libs")


include_directories( 
	"${C74_INCLUDES}"
	"${LIBS_PATH}/fftw/include"
)


add_library( 
	${PROJECT_NAME} 
	MODULE
	${PROJECT_NAME}.c
)

if(APPLE)
	target_link_libraries(${PROJECT_NAME} PUBLIC "-framework Accelerate")
	target_link_libraries(${PROJECT_NAME} PUBLIC ${LIBS_PATH}/fftw/libfftw3f.a)
elseif(WIN32)
	# FFTW (single precision), static. Provided by vcpkg (triplet x64-windows-static), see .github/workflows/windows-freezer.yml
	find_package(FFTW3f CONFIG REQUIRED)
	target_link_libraries(${PROJECT_NAME} PUBLIC FFTW3::fftw3f)
endif()

include(${CMAKE_CURRENT_SOURCE_DIR}/../../max-sdk-base/script/max-posttarget.cmake)
