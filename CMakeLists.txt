cmake_minimum_required(VERSION 3.13.0)
project(frequalizer VERSION 1.1.0)
add_subdirectory(External/JUCE)

# check which formats we want to build
set(FORMATS "VST3")

if (APPLE)
    list(APPEND FORMATS "AU")
endif()

# add the plugin targets
juce_add_plugin(frequalizer
	COMPANY_NAME "foleysfinest"
	PLUGIN_MANUFACTURER_CODE "TEOB"
	PLUGIN_CODE "FqOS"
	FORMATS ${FORMATS}
	VST3_CATEGORIES "Fx" "Analyzer" "EQ"
	AAX_CATEGORY "AAX_ePlugInCategory_EQ"
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
		src/Analyser.h
		src/FrequalizerEditor.cpp
		src/FrequalizerEditor.h
		src/FrequalizerProcessor.cpp
		src/FrequalizerProcessor.h
		src/SocialButtons.h
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

# setup the copying to the output folder
set(COPY_FOLDER ${CMAKE_SOURCE_DIR}/dist)

foreach(FORMAT ${FORMATS})
    get_target_property(ARTEFACTS_DIR frequalizer_${FORMAT} LIBRARY_OUTPUT_DIRECTORY)
    add_custom_command(TARGET frequalizer_${FORMAT} POST_BUILD COMMAND ${CMAKE_COMMAND} -E copy_directory ${ARTEFACTS_DIR} ${COPY_FOLDER})
endforeach()
