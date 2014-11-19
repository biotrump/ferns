cmake_minimum_required(VERSION 2.8  FATAL_ERROR)
PROJECT( FERNS )

#set variable from shell's env var by $ENV{env_var}
#include(cmake/CVEnvImport.cmake)

#opencv should be built first!
set(OpenCV_DIR $ENV{OPENCV_OUT})
#message("++++OpenCV_DIR:""${OpenCV_DIR}")
FIND_PACKAGE( OpenCV REQUIRED)
if(OpenCV_FOUND)
	message("found")
	message( "OpenCV_INCLUDE_DIRS:" "${OpenCV_INCLUDE_DIRS}" )
	message( "OpenCV_INCLUDES:" "${OpenCV_INCLUDES}")
	message( "OpenCV_LIBRARIES:" "${OpenCV_LIBRARIES}")
	message( "OpenCV_LIBS:" "${OpenCV_LIBS}")
	message( "OpenCV_DEFINITIONS:" "${OpenCV_DEFINITIONS}")
else()
	message(FATAL_ERROR "openCV not foud")
endif()

#openMP to avoid link error : omp_xxx error!
# undefined reference to `GOMP_loop_dynamic_start
find_package(OpenMP)
if(OPENMP_FOUND)
	set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
endif()


ADD_EXECUTABLE( ferns_demo
	main.cc
	mcv.cc
	planar_pattern_detector.cc
	planar_pattern_detector_builder.cc
	affine_transformation_range.cc
	buffer_management.cc
	pyr_yape06 homography06.cc
	homography_estimator.cc
	fine_gaussian_pyramid.cc
	mcvGaussianSmoothing.cc
	affine_image_generator06.cc
	fern_based_point_classifier.cc
	ferns.cc
	template_matching_based_tracker.cc
	cmphomo.cc
	)

#message(">>> DSP_ICA_LIBS::${DSP_ICA_LIBS}" )
#dynamic or static link
TARGET_LINK_LIBRARIES( ferns_demo
	${OpenCV_LIBS}
	)
