project(NeoMathEngineAvx)

add_library(${PROJECT_NAME} SHARED common.cpp)

target_compile_features(${PROJECT_NAME} PUBLIC cxx_std_11)

target_sources(${PROJECT_NAME}
    PRIVATE

    # Sources
    ./src/AvxMathEngine.cpp
    # Headers
    ./common.h
    ./src/BlobConvolution.h
    ./src/BlobConvolutionImpl.h
)

string(TOUPPER ${CMAKE_SYSTEM_NAME} UPPERCASE_CMAKE_SYSTEM_NAME)
target_compile_definitions(${PROJECT_NAME} PUBLIC _${UPPERCASE_CMAKE_SYSTEM_NAME})

target_include_directories(${PROJECT_NAME}
    PRIVATE
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/../../../../include>
)

# Add some definitions
target_compile_definitions(${PROJECT_NAME}
    PUBLIC
        "$<$<OR:$<CONFIG:RelWithDebInfo>,$<CONFIG:MinSizeRel>>:_RELEASE>"
        "$<$<CONFIG:Debug>:_DEBUG>"
        "$<$<CONFIG:Release>:_FINAL>"
)

target_link_libraries(${PROJECT_NAME} PRIVATE NeoMathEngine)

# OpenMP
link_openmp(${PROJECT_NAME})

# AVX
if(WIN32)
    target_compile_options(${PROJECT_NAME} PRIVATE /arch:AVX)
elseif(LINUX OR DARWIN)
    target_compile_options(${PROJECT_NAME} PRIVATE $<$<COMPILE_LANGUAGE:CXX>:-mavx -mfma>)
endif()

# Win resources
if(WIN32)
        if(USE_FINE_OBJECTS)
        target_include_directories(${PROJECT_NAME} PRIVATE 
            $<BUILD_INTERFACE:$<$<COMPILE_LANGUAGE:RC>:${FINE_ROOT}/Build/Inc ${FINE_ROOT}/FineObjects>>
        )
    else()
        target_include_directories(${PROJECT_NAME} PRIVATE 
            $<BUILD_INTERFACE:$<$<COMPILE_LANGUAGE:RC>:${CMAKE_CURRENT_SOURCE_DIR}/../../../../../Build/Inc>>
        )
    endif()
    enable_language(RC)
    target_sources(${PROJECT_NAME} PRIVATE ./NeoMathEngineAvx.rc)
endif()

# Install
if(NeoMathEngine_INSTALL)
    if(USE_FINE_OBJECTS)
        fine_install(TARGETS ${PROJECT_NAME})
    else()
        install(
            TARGETS ${PROJECT_NAME}
            LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
            )
    endif()
endif()
