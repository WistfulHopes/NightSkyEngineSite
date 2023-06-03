+++ 
date = 2023-01-26T00:02:59-08:00
title = "Download"
description = "Download and compile Night Sky Engine"
slug = ""
author = "WistfulHopes"
tags = []
categories = []
externalLink = ""
series = []
+++

# Download

To get Night Sky Engine, use a Git client, such as [the official client](https://git-scm.com/), or [Github Desktop](https://desktop.github.com/), and clone the Night Sky Engine repository at https://github.com/WistfulHopes/NightSkyEngineRewrite.git. If using the command line, type in the following:

    git clone https://github.com/WistfulHopes/NightSkyEngineRewrite.git

# Compile the engine

To compile the Night Sky Engine, you will need Unreal Engine 5.2 and Visual Studio. The version of Visual Studio must be at least 2019. For detailed instructions on installing Visual Studio for use with Unreal Engine, [click here](https://docs.unrealengine.com/5.3/en-US/setting-up-visual-studio-development-environment-for-cplusplus-projects-in-unreal-engine/).

After Visual Studio is set up, you will need to generate Visual Studio project files for Night Sky Engine. To do so, right-click on NightSkyEngine.uproject, and click on "Generate Visual Studio project files".

{{< rawhtml >}}
<img src="..\images\download\generate-project-files.png" alt="Generate project files" style="width:300px;"/>
{{< /rawhtml >}}

After doing so, there will be a new file in the folder, "NightSkyEngine.sln". Open it, and once Visual Studio has loaded, press Ctrl+B to compile the engine.

From here, read the [Quick Start Guide]({{< ref "/quick-start" >}} ).