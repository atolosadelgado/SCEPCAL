cmake_minimum_required(VERSION 3.12 FATAL_ERROR)
cmake_policy(SET CMP0074 NEW)

project(SCEPCAL)

IF(NOT TARGET DD4hep::DDCore)
  find_package ( DD4hep REQUIRED )
  include ( ${DD4hep_DIR}/cmake/DD4hep.cmake )
  include ( ${DD4hep_DIR}/cmake/DD4hepBuild.cmake )
ENDIF()

dd4hep_configure_output()

add_subdirectory(SCEPCALsegmentation)

dd4hep_add_plugin(SCEPCALConstructor SOURCES src/SCEPCALConstructor.cpp
  USES DD4hep::DDCore DD4hep::DDRec DD4hep::DDCond ROOT::Core ROOT::Geom ROOT::GenVector ROOT::MathCore SCEPCALsegmentation)
target_include_directories(SCEPCALConstructor PUBLIC SCEPCALsegmentation/include)
install(TARGETS SCEPCALConstructor LIBRARY DESTINATION lib)
