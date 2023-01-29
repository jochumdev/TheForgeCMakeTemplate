project(Game)

set(_FILES
    main.cpp 
    AsteroidSim.cpp 
    simplexnoise1234.cpp 
    TextureGen.cpp 
    ${FORGE_SOURCE_DIR}/Middleware_3/PaniniProjection/Panini.cpp
)

add_executable(${PROJECT_NAME} ${_FILES})
target_link_libraries(${PROJECT_NAME} PRIVATE TheForgeOS TheForgeRenderer)

# GPUCfg
add_custom_command(
        TARGET ${PROJECT_NAME} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_directory ${CMAKE_CURRENT_SOURCE_DIR}/../resources/GPUCfg ${CMAKE_CURRENT_BINARY_DIR}/GPUCfg
)

# Fonts
add_custom_command(
        TARGET ${PROJECT_NAME} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_directory ${CMAKE_CURRENT_SOURCE_DIR}/../resources/Fonts ${CMAKE_CURRENT_BINARY_DIR}/Fonts
)

# Textures
add_custom_command(
        TARGET ${PROJECT_NAME} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_directory ${CMAKE_CURRENT_SOURCE_DIR}/../resources/Textures ${CMAKE_CURRENT_BINARY_DIR}/Textures
)

# Scripts
file(GLOB _SCRIPTS ${CMAKE_CURRENT_SOURCE_DIR}/Scripts/*.lua)
if (_SCRIPTS)
    add_custom_command(
        TARGET ${PROJECT_NAME} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E make_directory ${CMAKE_CURRENT_BINARY_DIR}/Scripts/
    )
    add_custom_command(
        TARGET ${PROJECT_NAME} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
        ${_SCRIPTS}
        ${CMAKE_CURRENT_BINARY_DIR}/Scripts/)
endif ()

# Shaders
file(GLOB SHADERS ${CMAKE_CURRENT_SOURCE_DIR}/Shaders/FSL/*.fsl)
string(JOIN "%" _SHADERS ${FORGE_UI_SHADERS} ${FORGE_TEXT_SHADERS} ${FORGE_PANINI_SHADERS} ${SHADERS})

add_custom_command(
    TARGET ${PROJECT_NAME} POST_BUILD
    COMMAND ${CMAKE_COMMAND} 
        -DFORGE_SOURCE_DIR="${FORGE_SOURCE_DIR}"
        -DFORGE_RENDERERS="${FORGE_RENDERERS}"
        -DFCS_VERBOSE=ON
        -DFCS_INCREMENTAL=ON
        -DFCS_DESTINATION="${CMAKE_CURRENT_BINARY_DIR}/Shaders"
        -DFCS_BINARYDESTINATION="${CMAKE_CURRENT_BINARY_DIR}/CompiledShaders"
        -DFCS_SHADERS="${_SHADERS}"
        -P ${FORGE_SHADERCOMPILER_SCRIPT}
)
