# Generated from orogen/lib/orogen/templates/tasks/CMakeLists.txt

# personal hard coded paths for first working iteration
##include_directories(/home/con/rock/install/include)
##include_directories(/home/con/rock/install/include/apriltag)

include_directories($ENV{AUTOPROJ_CURRENT_ROOT}/install/include)
include_directories($ENV{AUTOPROJ_CURRENT_ROOT}/install/include/apriltag)

include(apriltagsTaskLib)
ADD_LIBRARY(${APRILTAGS_TASKLIB_NAME} SHARED 
    ${APRILTAGS_TASKLIB_SOURCES})
add_dependencies(${APRILTAGS_TASKLIB_NAME}
    regen-typekit)

TARGET_LINK_LIBRARIES(${APRILTAGS_TASKLIB_NAME}
    ${OrocosRTT_LIBRARIES}
    ${APRILTAGS_TASKLIB_DEPENDENT_LIBRARIES})
SET_TARGET_PROPERTIES(${APRILTAGS_TASKLIB_NAME}
    PROPERTIES LINK_INTERFACE_LIBRARIES "${APRILTAGS_TASKLIB_INTERFACE_LIBRARIES}")
SET_TARGET_PROPERTIES(${APRILTAGS_TASKLIB_NAME}
    PROPERTIES INTERFACE_LINK_LIBRARIES "${APRILTAGS_TASKLIB_INTERFACE_LIBRARIES}")

INSTALL(TARGETS ${APRILTAGS_TASKLIB_NAME}
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib/orocos)

INSTALL(FILES ${APRILTAGS_TASKLIB_HEADERS}
    DESTINATION include/orocos/apriltags)

