if(USE_CPP_PACKAGE)

  set(CPP_PACKAGE_OP_H_HEADER ${CMAKE_CURRENT_LIST_DIR}/include/mxnet-cpp/op.h)

  if(MSVC)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /utf-8")
  endif(MSVC)

  if(UNIX)
    # HACK
    set(env_cmd LD_LIBRARY_PATH=$<TARGET_FILE_DIR:dmlc::dmlc>)
  else()
    set(env_cmd "")
  endif()

  add_custom_target(
    cpp_package_op_h ALL
    BYPRODUCTS ${CPP_PACKAGE_OP_H_HEADER}
    MAIN_DEPENDENCY mxnet
    DEPENDS mxnet ${CMAKE_CURRENT_SOURCE_DIR}/scripts/OpWrapperGenerator.py
    COMMAND echo "Running: OpWrapperGenerator.py"
    COMMAND ${env_cmd} python OpWrapperGenerator.py $<TARGET_FILE:mxnet>
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/scripts
  )

  if(BUILD_CPP_EXAMPLES)
    add_subdirectory(example)
  endif()

  install(DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

endif()
