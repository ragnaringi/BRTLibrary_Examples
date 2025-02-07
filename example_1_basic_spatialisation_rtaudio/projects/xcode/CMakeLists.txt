cmake_minimum_required(VERSION 3.16.0 FATAL_ERROR)

set(CMAKE_SYSTEM_VERSION 10.0 CACHE STRING "" FORCE)

project(BasicSpatialisationRTAudio CXX)

# Set the minimum C++ standard to C++17
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Find zlib
find_package(ZLIB REQUIRED)


################################################################################
# Global configuration types
################################################################################
set(CMAKE_CONFIGURATION_TYPES
    "Debug"
    "Release"
    CACHE STRING "" FORCE
)

################################################################################
# Global compiler options
################################################################################
if(MSVC)
    # remove default flags provided with CMake for MSVC
    set(CMAKE_CXX_FLAGS "")
    set(CMAKE_CXX_FLAGS_DEBUG "")
    set(CMAKE_CXX_FLAGS_RELEASE "")
endif()

################################################################################
# Global linker options
################################################################################
if(MSVC)
    # remove default flags provided with CMake for MSVC
    set(CMAKE_EXE_LINKER_FLAGS "")
    set(CMAKE_MODULE_LINKER_FLAGS "")
    set(CMAKE_SHARED_LINKER_FLAGS "")
    set(CMAKE_STATIC_LINKER_FLAGS "")
    set(CMAKE_EXE_LINKER_FLAGS_DEBUG "${CMAKE_EXE_LINKER_FLAGS}")
    set(CMAKE_MODULE_LINKER_FLAGS_DEBUG "${CMAKE_MODULE_LINKER_FLAGS}")
    set(CMAKE_SHARED_LINKER_FLAGS_DEBUG "${CMAKE_SHARED_LINKER_FLAGS}")
    set(CMAKE_STATIC_LINKER_FLAGS_DEBUG "${CMAKE_STATIC_LINKER_FLAGS}")
    set(CMAKE_EXE_LINKER_FLAGS_RELEASE "${CMAKE_EXE_LINKER_FLAGS}")
    set(CMAKE_MODULE_LINKER_FLAGS_RELEASE "${CMAKE_MODULE_LINKER_FLAGS}")
    set(CMAKE_SHARED_LINKER_FLAGS_RELEASE "${CMAKE_SHARED_LINKER_FLAGS}")
    set(CMAKE_STATIC_LINKER_FLAGS_RELEASE "${CMAKE_STATIC_LINKER_FLAGS}")
endif()

################################################################################
# Common utils
################################################################################
include(CMake/Utils.cmake)

################################################################################
# Additional Global Settings(add specific info there)
################################################################################
include(CMake/GlobalSettingsInclude.cmake OPTIONAL)

################################################################################
# Use solution folders feature
################################################################################
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

################################################################################
# Sub-projects
################################################################################


set(PROJECT_NAME BasicSpatialisationRTAudio)

################################################################################
# Source groups
################################################################################
set(include
    "../../../BRTLibrary/include/BRTLibrary.h"
)
source_group("include" FILES ${include})

set(Header_Files
    "../../src/BasicSpatialisationRTAudio.h"
)
source_group("Header Files" FILES ${Header_Files})

set(Source_Files
    "../../src/BasicSpatialisationRTAudio.cpp"
)
source_group("Source Files" FILES ${Source_Files})

set(ALL_FILES
    ${Header_Files}
    ${Source_Files}
)

################################################################################
# Target
################################################################################
add_executable(${PROJECT_NAME} ${ALL_FILES})

use_props(${PROJECT_NAME} "${CMAKE_CONFIGURATION_TYPES}" "${DEFAULT_CXX_PROPS}")
set(ROOT_NAMESPACE BasicSpatialisationRTAudio)

################################################################################
# Output directory
################################################################################
set_target_properties(${PROJECT_NAME} PROPERTIES
    OUTPUT_DIRECTORY_DEBUG   "${CMAKE_SOURCE_DIR}//build/${CMAKE_VS_PLATFORM_NAME}/$<CONFIG>/"
    OUTPUT_DIRECTORY_RELEASE "${CMAKE_SOURCE_DIR}//build/${CMAKE_VS_PLATFORM_NAME}/$<CONFIG>/"
)

################################################################################
# Include directories
################################################################################
target_include_directories(${PROJECT_NAME} PUBLIC
    "${CMAKE_CURRENT_SOURCE_DIR}/../../../third_party_libraries/rtaudio;"
    "${CMAKE_CURRENT_SOURCE_DIR}/../../../third_party_libraries/rtaudio/include;"
    "${CMAKE_CURRENT_SOURCE_DIR}/../../../BRTLibrary/include"
)

################################################################################
# Compile definitions
################################################################################

################################################################################
# Dependencies
################################################################################
add_dependencies(${PROJECT_NAME}
    BRTLibrary
    rtaudio
)
set(ADDITIONAL_LIBRARY_DEPENDENCIES
    "$<$<CONFIG:Debug>:"
        "rtaudio_debug;"
        ZLIB::ZLIB
        "-framework CoreAudio"
        "-framework CoreFoundation"
    ">"
    "$<$<CONFIG:Release>:"
        "rtaudio;"
        ZLIB::ZLIB
        "-framework CoreAudio"
        "-framework CoreFoundation"
    ">"
    "mysofa"
)
target_link_libraries(${PROJECT_NAME} PRIVATE "${ADDITIONAL_LIBRARY_DEPENDENCIES}")
# target_link_libraries(MyApp PRIVATE "-framework Cocoa")

target_link_directories(${PROJECT_NAME} PRIVATE
    "$<$<CONFIG:Debug>:"
        "${CMAKE_CURRENT_SOURCE_DIR}/../../../BRTLibrary/include/third_party_libraries/libmysofa/lib/macos"
        "${CMAKE_CURRENT_SOURCE_DIR}//build/rtaudio/${CMAKE_VS_PLATFORM_NAME}/$<CONFIG>"
    ">"
    "$<$<CONFIG:Release>:"
        "${CMAKE_CURRENT_SOURCE_DIR}/../../../BRTLibrary/include/third_party_libraries/libmysofa/lib/macos"
        "${CMAKE_CURRENT_SOURCE_DIR}//build/rtaudio/${CMAKE_VS_PLATFORM_NAME}/$<CONFIG>"
    ">"
)


set(PROJECT_NAME rtaudio)

################################################################################
# Source groups
################################################################################
if(MSVC)
    set(Headers
        "../../../third_party_libraries/rtaudio/include/asio.h"
        "../../../third_party_libraries/rtaudio/include/asiodrivers.h"
        "../../../third_party_libraries/rtaudio/include/asiodrvr.h"
        "../../../third_party_libraries/rtaudio/include/asiolist.h"
        "../../../third_party_libraries/rtaudio/include/asiosys.h"
        "../../../third_party_libraries/rtaudio/include/dsound.h"
        "../../../third_party_libraries/rtaudio/include/functiondiscoverykeys_devpkey.h"
        "../../../third_party_libraries/rtaudio/include/ginclude.h"
        "../../../third_party_libraries/rtaudio/include/iasiodrv.h"
        "../../../third_party_libraries/rtaudio/include/soundcard.h"
        "../../../third_party_libraries/rtaudio/RtAudio.h"
    )
    source_group("Headers" FILES ${Headers})

    set(Sources
        "../../../third_party_libraries/rtaudio/include/asio.cpp"
        "../../../third_party_libraries/rtaudio/include/asiodrivers.cpp"
        "../../../third_party_libraries/rtaudio/include/asiolist.cpp"
        "../../../third_party_libraries/rtaudio/RtAudio.cpp"
    )
    source_group("Sources" FILES ${Sources})

    set(ALL_FILES
        ${Headers}
        ${Sources}
    )
else()
    set(Headers
        "../../../third_party_libraries/rtaudio/RtAudio.h"
    )
    source_group("Headers" FILES ${Headers})

    set(Sources
        "../../../third_party_libraries/rtaudio/RtAudio.cpp"
    )
    source_group("Sources" FILES ${Sources})

    set(ALL_FILES
        ${Headers}
        ${Sources}
    )
endif()

################################################################################
# Target
################################################################################
add_library(${PROJECT_NAME} STATIC ${ALL_FILES})

use_props(${PROJECT_NAME} "${CMAKE_CONFIGURATION_TYPES}" "${DEFAULT_CXX_PROPS}")
################################################################################
# Target name
################################################################################
set_target_properties(${PROJECT_NAME} PROPERTIES
    TARGET_NAME_DEBUG   "${PROJECT_NAME}_debug"
)

################################################################################
# Output directory
################################################################################
set_target_properties(${PROJECT_NAME} PROPERTIES
    OUTPUT_DIRECTORY_DEBUG   "${CMAKE_SOURCE_DIR}//build/rtaudio/${CMAKE_VS_PLATFORM_NAME}/$<CONFIG>/"
    OUTPUT_DIRECTORY_RELEASE "${CMAKE_SOURCE_DIR}//build/rtaudio/${CMAKE_VS_PLATFORM_NAME}/$<CONFIG>/"
)
set_target_properties(${PROJECT_NAME} PROPERTIES
    INTERPROCEDURAL_OPTIMIZATION_RELEASE "TRUE"
)

################################################################################
# Include directories
################################################################################
target_include_directories(${PROJECT_NAME} PUBLIC
    "${CMAKE_CURRENT_SOURCE_DIR}/../../../third_party_libraries/rtaudio;"
    "${CMAKE_CURRENT_SOURCE_DIR}/../../../third_party_libraries/rtaudio/include;"
)

################################################################################
# Compile definitions
################################################################################
if(MSVC)
    target_compile_definitions(${PROJECT_NAME} PRIVATE
        "NDEBUG;"
        "WIN64;"
        "_CONSOLE;"
        "__WINDOWS_WASAPI__;"
        "_MBCS"
    )
else()
    target_compile_definitions(${PROJECT_NAME} PRIVATE
        "NDEBUG;"
        "__MACOSX_CORE__;"
    )
endif()

################################################################################
# Compile and link options
################################################################################
if(MSVC)
    target_compile_options(${PROJECT_NAME} PRIVATE
        $<$<CONFIG:Debug>:
            /Od
        >
        $<$<CONFIG:Release>:
            /O2
        >
        /GF;
        /Gy;
        /W3;
        /nologo;
        ${DEFAULT_CXX_DEBUG_INFORMATION_FORMAT};
        ${DEFAULT_CXX_EXCEPTION_HANDLING}
    )
    target_link_options(${PROJECT_NAME} PRIVATE
        /OPT:REF;
        /OPT:ICF
    )
endif()

################################################################################
# Dependencies
################################################################################
if(MSVC)
    set(ADDITIONAL_LIBRARY_DEPENDENCIES
        "Dxguid;"
        "dsound;"
        "odbc32;"
        "odbccp32"
    )
    target_link_libraries(${PROJECT_NAME} PUBLIC "${ADDITIONAL_LIBRARY_DEPENDENCIES}")
endif()

set(PROJECT_NAME BRTLibrary)

################################################################################
# Source groups
################################################################################
set(include
    "../../../BRTLibrary/include/BRTLibrary.h"
)
source_group("include" FILES ${include})

set(include__Base
    "../../../BRTLibrary/include/Base/BRTManager.hpp"
    "../../../BRTLibrary/include/Base/Command.hpp"
    "../../../BRTLibrary/include/Base/CommandEntryPointManager.hpp"
    "../../../BRTLibrary/include/Base/EntryPoint.hpp"
    "../../../BRTLibrary/include/Base/EntryPointManager.hpp"
    "../../../BRTLibrary/include/Base/ExitPoint.hpp"
    "../../../BRTLibrary/include/Base/ExitPointManager.hpp"
    "../../../BRTLibrary/include/Base/ListenerModelBase.hpp"
    "../../../BRTLibrary/include/Base/ObserverBase.hpp"
    "../../../BRTLibrary/include/Base/ProcessorBase.hpp"
    "../../../BRTLibrary/include/Base/SourceModelBase.hpp"
)
source_group("include\\Base" FILES ${include__Base})

set(include__Common
    "../../../BRTLibrary/include/Common/AddDelayExpansionMethod.hpp"
    "../../../BRTLibrary/include/Common/AmbisonicEncoder.hpp"
    "../../../BRTLibrary/include/Common/BiquadFilter.hpp"
    "../../../BRTLibrary/include/Common/Buffer.hpp"
    "../../../BRTLibrary/include/Common/CommonDefinitions.hpp"
    "../../../BRTLibrary/include/Common/Conventions.hpp"
    "../../../BRTLibrary/include/Common/EnvelopeDetector.hpp"
    "../../../BRTLibrary/include/Common/ErrorHandler.hpp"
    "../../../BRTLibrary/include/Common/fftsg.hpp"
    "../../../BRTLibrary/include/Common/FiltersChain.hpp"
    "../../../BRTLibrary/include/Common/Fprocessor.hpp"
    "../../../BRTLibrary/include/Common/GlobalParameters.hpp"
    "../../../BRTLibrary/include/Common/Profiler.hpp"
    "../../../BRTLibrary/include/Common/Quaternion.hpp"
    "../../../BRTLibrary/include/Common/SourceListenerRelativePositionCalculation.hpp"
    "../../../BRTLibrary/include/Common/Transform.hpp"
    "../../../BRTLibrary/include/Common/UPCAnechoic.hpp"
    "../../../BRTLibrary/include/Common/Vector3.hpp"
)
source_group("include\\Common" FILES ${include__Common})

set(include__ListenerModels
    "../../../BRTLibrary/include/ListenerModels/ListenerHRTFbasedModel.hpp"
    "../../../BRTLibrary/include/ListenerModels/ListenerVirtualAmbisonicBasedModel.hpp"
)
source_group("include\\ListenerModels" FILES ${include__ListenerModels})

set(include__ProcessingModules
    "../../../BRTLibrary/include/ProcessingModules/AmbisonicDomainConvolver.hpp"
    "../../../BRTLibrary/include/ProcessingModules/AmbisonicDomainConvolverProcessor.hpp"
    "../../../BRTLibrary/include/ProcessingModules/BilateralAmbisonicEncoder.hpp"
    "../../../BRTLibrary/include/ProcessingModules/BilateralAmbisonicEncoderProcessor.hpp"
    "../../../BRTLibrary/include/ProcessingModules/DirectivityTFConvolver.hpp"
    "../../../BRTLibrary/include/ProcessingModules/DistanceAttenuation.hpp"
    "../../../BRTLibrary/include/ProcessingModules/DistanceAttenuationProcessor.hpp"
    "../../../BRTLibrary/include/ProcessingModules/HRTFConvolver.hpp"
    "../../../BRTLibrary/include/ProcessingModules/HRTFConvolverProcessor.hpp"
    "../../../BRTLibrary/include/ProcessingModules/MultipleInputProcessor.hpp"
    "../../../BRTLibrary/include/ProcessingModules/NearFieldEffect.hpp"
    "../../../BRTLibrary/include/ProcessingModules/NearFieldEffectProcessor.hpp"
    "../../../BRTLibrary/include/ProcessingModules/SingleBinauralProcessor.hpp"
    "../../../BRTLibrary/include/ProcessingModules/SingleProcessor.hpp"
)
source_group("include\\ProcessingModules" FILES ${include__ProcessingModules})

set(include__Readers
    "../../../BRTLibrary/include/Readers/LibMySofaLoader.hpp"
    "../../../BRTLibrary/include/Readers/SofaReader.hpp"
)
source_group("include\\Readers" FILES ${include__Readers})

set(include__ServiceModules
    "../../../BRTLibrary/include/ServiceModules/AmbisonicBIR.hpp"
    "../../../BRTLibrary/include/ServiceModules/DirectivityTF.hpp"
    "../../../BRTLibrary/include/ServiceModules/DirectivityTFAuxiliarMethods.hpp"
    "../../../BRTLibrary/include/ServiceModules/DirectivityTFDefinitions.hpp"
    "../../../BRTLibrary/include/ServiceModules/Extrapolation.hpp"
    "../../../BRTLibrary/include/ServiceModules/GridsManager.hpp"
    "../../../BRTLibrary/include/ServiceModules/HRTF.hpp"
    "../../../BRTLibrary/include/ServiceModules/HRTFAuxiliarMethods.hpp"
    "../../../BRTLibrary/include/ServiceModules/HRTFDefinitions.hpp"
    "../../../BRTLibrary/include/ServiceModules/HRTFTester.hpp"
    "../../../BRTLibrary/include/ServiceModules/ILD.hpp"
    "../../../BRTLibrary/include/ServiceModules/InterpolationAuxiliarMethods.hpp"
    "../../../BRTLibrary/include/ServiceModules/OfflineInterpolation.hpp"
    "../../../BRTLibrary/include/ServiceModules/OfflineInterpolationAuxiliarMethods.hpp"
    "../../../BRTLibrary/include/ServiceModules/OnlineInterpolation.hpp"
    "../../../BRTLibrary/include/ServiceModules/ServiceModuleInterfaces.hpp"
    "../../../BRTLibrary/include/ServiceModules/VirtualSpeakers.hpp"
)
source_group("include\\ServiceModules" FILES ${include__ServiceModules})

set(include__SourceModels
    "../../../BRTLibrary/include/SourceModels/SourceDirectivityModel.hpp"
    "../../../BRTLibrary/include/SourceModels/SourceSimpleModel.hpp"
)
source_group("include\\SourceModels" FILES ${include__SourceModels})

set(include__third_party_libraries__libmysofa__include
    "../../../BRTLibrary/include/third_party_libraries/libmysofa/include/mysofa.h"
)
source_group("include\\third_party_libraries\\libmysofa\\include" FILES ${include__third_party_libraries__libmysofa__include})

set(include__third_party_libraries__nlohmann
    "../../../BRTLibrary/include/third_party_libraries/nlohmann/json.hpp"
)
source_group("include\\third_party_libraries\\nlohmann" FILES ${include__third_party_libraries__nlohmann})

set(ALL_FILES
    ${include}
    ${include__Base}
    ${include__Common}
    ${include__ListenerModels}
    ${include__ProcessingModules}
    ${include__Readers}
    ${include__ServiceModules}
    ${include__SourceModels}
    ${include__third_party_libraries__libmysofa__include}
    ${include__third_party_libraries__nlohmann}
)

add_custom_target(${PROJECT_NAME} SOURCES ${ALL_FILES})
