cmake_minimum_required(VERSION 3.15)

project(HostPluginDemo VERSION 0.1)

set (FETCHCONTENT_QUIET_OFF)

include(FetchContent)

option(JUCE_ENABLE_MODULE_SOURCE_GROUPS "Enable Module Source Groups" ON)
set_property(GLOBAL PROPERTY USE_FOLDERS YES)

FetchContent_Declare (
    JUCE
    GIT_REPOSITORY https://github.com/juce-framework/JUCE.git
    GIT_TAG origin/master 
    GIT_SHALLOW ON
    FIND_PACKAGE_ARGS 7.0.3 GLOBAL)
    

FetchContent_MakeAvailable (JUCE)

juce_add_gui_app(HostPluginDemo
BUNDLE_ID                       com.juce.pluginhost
MICROPHONE_PERMISSION_ENABLED   TRUE
PLUGINHOST_AU                   FALSE

)

# We want all source code in the source directory
file(GLOB_RECURSE SourceFiles CONFIGURE_DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/Source/*.cpp" "${CMAKE_CURRENT_SOURCE_DIR}/Source/*.h")

# No, we don't want our source buried in extra nested folders
set_target_properties("${PROJECT_NAME}" PROPERTIES FOLDER "")

# The Xcode source tree should look like the source tree
source_group(TREE ${CMAKE_CURRENT_SOURCE_DIR}/Source PREFIX "Source" FILES ${SourceFiles})

target_sources("${PROJECT_NAME}" PRIVATE ${SourceFiles})

#add_executable(Main.cpp)


# target_sources adds source files to a target. We pass the target that needs the sources as the
# first argument, then a visibility parameter for the sources which should normally be PRIVATE.
# Finally, we supply a list of source files that will be built into the target. This is a standard
# CMake command.

#juce_generate_juce_header(${PROJECT_NAME})

target_link_libraries(HostPluginDemo PRIVATE
juce::juce_audio_basics
juce::juce_audio_devices
juce::juce_audio_formats
juce::juce_audio_plugin_client
juce::juce_audio_processors
juce::juce_audio_utils
juce::juce_core
juce::juce_data_structures
juce::juce_events
juce::juce_graphics
juce::juce_gui_basics
juce::juce_gui_extra
)

target_compile_definitions(HostPluginDemo PRIVATE
    JUCE_ALSA=1
    JUCE_CONTENT_SHARING=1
    JUCE_DIRECTSOUND=1
    JUCE_DISABLE_CAUTIOUS_PARAMETER_ID_CHECKING=1
    JUCE_PLUGINHOST_LADSPA=1
    JUCE_PLUGINHOST_LV2=1
    JUCE_PLUGINHOST_VST3=1
    JUCE_PLUGINHOST_VST=0
    JUCE_PLUGINHOST_ARA=0
    JUCE_USE_CAMERA=0
    JUCE_USE_CDBURNER=0
    JUCE_USE_CDREADER=0
    JUCE_USE_CURL=0
    JUCE_USE_FLAC=0
    JUCE_USE_OGGVORBIS=1
    JUCE_VST3_HOST_CROSS_PLATFORM_UID=1
    JUCE_WASAPI=1
    JUCE_WEB_BROWSER=0
    JUCE_DISPLAY_SPLASH_SCREEN=0

    PIP_JUCE_EXAMPLES_DIRECTORY_STRING="${JUCE_SOURCE_DIR}/examples"
    )