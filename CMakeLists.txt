cmake_minimum_required(VERSION 3.13.0)
project(frequalizer VERSION 2.0.0)

add_subdirectory(lib/JUCE)

option(INSTALL_AFTER_BUILD "Automatically copy built plugins in system directories after build" OFF)

# useful for linting (i.e. `clangd`)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# check which formats we want to build
set(FORMATS "VST3")

if (APPLE)
    list(APPEND FORMATS "AU")
endif()

# add the plugin targets
juce_add_plugin(frequalizer
	COMPANY_NAME "teobe"
	PLUGIN_MANUFACTURER_CODE "TEOB"
	PLUGIN_CODE "FqOS"
	FORMATS ${FORMATS}
	VST3_CATEGORIES "Fx" "Analyzer" "EQ"
	AU_MAIN_TYPE "kAudioUnitType_Effect"
	COMPANY_WEBSITE "https://teobe.net"
	COMPANY_EMAIL "info@teobe.net"
	BUNDLE_ID "net.teobe.frequalizer"
	PLUGIN_NAME "Frequalizer"
	PRODUCT_NAME "Frequalizer"
)

# add the sources
target_sources(frequalizer
	PRIVATE
		src/FrequalizerEditor.cpp
		src/FrequalizerProcessor.cpp
)

# add required flags
target_link_libraries(frequalizer
	PRIVATE
		# compiler flags
		juce::juce_recommended_warning_flags
		juce::juce_recommended_config_flags
		juce::juce_recommended_lto_flags
		# juce modules
		juce::juce_opengl
		juce::juce_dsp
		juce::juce_audio_utils
)
target_compile_definitions(frequalizer
	PUBLIC
		JUCE_VST3_CAN_REPLACE_VST2=OFF
		JUCE_WEB_BROWSER=OFF
)
set_target_properties(frequalizer
	PROPERTIES
		CXX_STANDARD 17
)

# setup the copying to the output folder
if(NOT INSTALL_AFTER_BUILD)
	message("Installing to dist/")
	foreach(FORMAT ${FORMATS})
		set_target_properties(frequalizer_${FORMAT} PROPERTIES JUCE_PLUGIN_COPY_DIR ${CMAKE_SOURCE_DIR}/dist)
	endforeach()
endif()

juce_enable_copy_plugin_step(frequalizer)
