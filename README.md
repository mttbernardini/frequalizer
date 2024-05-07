# Frequalizer

[![Build Status](https://github.com/mttbernardini/frequalizer/actions/workflows/build.yml/badge.svg)](https://github.com/mttbernardini/frequalizer/actions/workflows/build.yml/badge.svg)

This project provides a 6-band Parametric Equalizer built using [JUCE](https://github.com/juce-framework/JUCE) and the `juce::dps` module.

The code is based on the work of [Foley Finest](https://github.com/ffAudio/Frequalizer) and has been forked in order to continue development for my personal needs, since, at the time of writing, the original project is unmaintaned since 2022.

**⚠️ Compatibiliy Note:** branding and IDs are replaced with my own! This plugin is NOT compatible with the original Foley Finest's Frequalizer and is treated as a different plug-in by Audio Hosts and DAWs.


## Download

Pre-built VST3 or Audio Unit are available over the [Releases](https://github.com/mttbernardini/frequalizer/releases) page.

After download, move the VST/AU file to the expected location of your OS.

Note for macOS users: the Audio Unit is self-signed. To use it you will need to remove the quarantine flag using `xattr -rd com.apple.quarantine path/to/frequalizer.component` after downloading.

## Build

Clone this repo and its submodules:

    git clone --recurse-submodules https://github.com/mttbernardini/frequalizer
    cd frequalizer

Use CMake to build:

    cmake -B build
    cmake --build build

You'll find the built plugins under `dist/`.
