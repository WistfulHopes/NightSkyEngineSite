+++
title = "About"
description = "What is Night Sky Engine?"
aliases = ["about-us", "about-night-sky", "contact"]
author = "WistfulHopes"
+++

## What is Night Sky Engine?

Night Sky Engine is a free and open source framework for 2D fighting games made in Unreal Engine 5. It is designed to be powerful yet easy to learn, with several key features powering it.

## What is Night Sky Engine written in?

Night Sky Engine uses high-level scripting provided by Unreal's Blueprint system, giving those with less programming experience the ability to create their own characters. If this scripting isn't enough, the native C++ backend can be edited freely to extend functionality.

## What are some key features of Night Sky Engine?

Using the Blueprint system and exposed native functions, Night Sky Engine allows for very in-depth control of character states. States are event-driven with many callback functions, and those functions can be extended to add movement, control frame data, create timers, and more.

Night Sky Engine also allows for both 2D sprite-based characters and 3D characters. As an added bonus, there's support for both traditionally animated 3D characters, and Arc System Works-like animated 3D characters.

One of the more important features included is rollback netcode using GGPO. Stable netplay across the world is possible thanks to this.

## Should I download now, or wait for later?

Night Sky Engine is in early development. If you are looking to make a "serious" project with it, I could not recommend using Night Sky Engine at this time. Despite the gameplay code being quite usable, everything else, such as menus and options, are woefully lacking. You would have to implement lots on your own, and some of these changes would be incompatible with future engine builds.

However, if you're just interested in messing around, want to get a head start, or wish to contribute to the engine, then you can get started today! Head to [Download]({{< ref "/download" >}} ) to get started.