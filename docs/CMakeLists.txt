option(BUILD_DOC
        "Create and install the HTML based API documentation (requires Doxygen)" OFF)
IF(BUILD_DOC)
  find_package(Doxygen REQUIRED)

  #This will be the main output of our command
  set(DOXYGEN_INPUT_DIR ${PROJECT_SOURCE_DIR}/src)
  set(DOXYGEN_OUTPUT_DIR ${CMAKE_CURRENT_BINARY_DIR}/output)
  set(DOXYGEN_INDEX_FILE ${DOXYGEN_OUTPUT_DIR}/html/index.html)
  set(DOXYFILE_IN ${CMAKE_CURRENT_SOURCE_DIR}/Doxyfile.in)
  set(DOXYFILE_OUT ${CMAKE_CURRENT_BINARY_DIR}/Doxyfile)


  #Replace variables inside @@ with the current values
  configure_file(${DOXYFILE_IN} ${DOXYFILE_OUT} @ONLY)

  file(MAKE_DIRECTORY ${DOXYGEN_OUTPUT_DIR}) #Doxygen won't create this for us
  add_custom_command(OUTPUT ${DOXYGEN_INDEX_FILE}
          COMMAND ${DOXYGEN_EXECUTABLE} ${DOXYFILE_OUT}
          MAIN_DEPENDENCY ${DOXYFILE_OUT} ${DOXYFILE_IN}
          DEPENDS doxygen_playground_Lib
          COMMENT "Generating docs")

  add_custom_target(Doxygen ALL DEPENDS ${DOXYGEN_INDEX_FILE})
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/html DESTINATION share/doc)
endif()