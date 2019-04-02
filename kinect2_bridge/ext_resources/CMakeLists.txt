cmake_minimum_required(VERSION 2.8.3)

project(external_resources)

if(NOT DEFINED ext_res_GET_CAMERA_CALIBRATIONS)
  set(ext_res_GET_CAMERA_CALIBRATIONS ON CACHE BOOL "Enable to get external resources repositories (i.e. RGBD camera calibrations)")
endif()

if(${ext_res_GET_CAMERA_CALIBRATIONS})

  find_package(Git 1.6.5 REQUIRED)
  include(ExternalProject)

  set(ext_res_CAMERA_CALIBRATION_GIT_REPOSITORY https://bitbucket.org/iaslab-unipd/rgbd_camera_calibrations.git CACHE STRING "Repository containing camera calibrations. Use only public download enabled repositories")

  set(ext_res_COPY_KINECT_V2_CALIBRATIONS ON CACHE BOOL "Copy to ext_res_DESTINATION_DIR the Kinect V2 calibration files")
  set(ext_res_DESTINATION_DIR ${PROJECT_SOURCE_DIR}/../data CACHE PATH "Destination directory for calibration files")
  set(ext_res_SOURCE_DIR ${PROJECT_SOURCE_DIR}/../ext_resources CACHE PATH "Source directory for external resources repositories")

  ExternalProject_Add(rgbd_camera_calibrations
    GIT_REPOSITORY ${ext_res_CAMERA_CALIBRATION_GIT_REPOSITORY}
    GIT_SHALLOW TRUE
    TMP_DIR "${ext_res_SOURCE_DIR}/rgbd_camera_calibrations_etc"
    STAMP_DIR "${ext_res_SOURCE_DIR}/rgbd_camera_calibrations_etc"
    SOURCE_DIR "${ext_res_SOURCE_DIR}/rgbd_camera_calibrations"
    CMAKE_ARGS += "-Dutils_OUTPUT_DIR:=${ext_res_DESTINATION_DIR}"
    CMAKE_ARGS += "-Dutils_COPY_KINECT_V2_CALIBRATIONS:=${ext_res_COPY_KINECT_V2_CALIBRATIONS}"
    BUILD_COMMAND ""
    INSTALL_COMMAND ""
    )

endif()

