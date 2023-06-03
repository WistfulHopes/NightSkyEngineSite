---
title: "Quick Start Guide"
---

## Creating a character

To create a character, navigate to Content/Blueprints/Characters, and create a new folder for your character. Inside that folder, create a blueprint that inherits either from BP_PlayerObject (for 3D characters) or BP_SpritePlayerObject (for 2D characters).

{{< rawhtml >}}
<img src="..\images\quick-start\create-character.png" alt="Create character blueprint" style="width:720px;"/>
{{< /rawhtml >}}

After your character is created, you may assign the mesh (or sprite flipbook) the character uses the same way as any Unreal Engine Actor. For more information, [read the Unreal Engine Actor documentation.](https://docs.unrealengine.com/5.2/en-US/actors-in-unreal-engine/)

I recommend placing the character assets inside Content/CharacterAssets/(character name). You may look at the included characters for an idea of how to organize the assets.

## Creating a state

However, this by itself isn't enough to have a functioning character! You must create a State blueprint, which contains several important parts of your character's code. Within State blueprints, the character's functionality and animations may be implemented.

First, create a States folder within your character's folder. Inside that folder, create a blueprint child of the ST_CommonStand state.

{{< rawhtml >}}
<img src="..\images\quick-start\create-state.png" alt="Create state blueprint" style="width:720px;"/>
{{< /rawhtml >}}

Name it ST_(character name)Stand. When you open the file, there won't be any code inside the blueprint. This is where you override functions from the base blueprint. These functions are automatically executed by the engine code under specific conditions.

{{< rawhtml >}}
<img src="..\images\quick-start\override-function.png" alt="Create state blueprint" style="width:500px;"/>
{{< /rawhtml >}}

For now, override the Exec function. Right click on the Event nodes and click "Add call to parent function" button. Then, plug the Event function into the corresponding Parent function.

{{< rawhtml >}}
<img src="..\images\quick-start\parent-function.png" alt="Create state blueprint" style="width:300px;"/>
{{< /rawhtml >}}

If you don't do this, the base code will not execute, and many issues may occur.

## Assigning animations

This is all well and good, but without animations being set up, how will you know what your character is doing? So now, it's time to create an Anim Data asset for 3D characters, or add to the Flipbook Data asset for 2D characters.

### 3D Characters

Within Content/CharacterAssets/Animations/, create a Data Asset by right-clicking in the Content Browser, navigating to Miscellaneous, and clicking Data Asset.

{{< rawhtml >}}
<img src="..\images\quick-start\anim-data.png" alt="Create Anim Data" style="width:1280px;"/>
{{< /rawhtml >}}

A pop-up will appear asking which class to use for the asset. Select AnimData.

{{< rawhtml >}}
<img src="..\images\quick-start\data-asset.png" alt="Select Data Asset class" style="width:500px;"/>
{{< /rawhtml >}}

For each animation you wish to use, add an element to the Anim Datas array. Give it a name, and assign the correct animation. The name will be used to call the correct animation from the State blueprints.

Now, create an Anim Blueprint. Select the mesh skeleton, and ABP_CommonArcSys as the parent class for ArcSystemWorks characters. A traditional 3D animation template is under construction.

{{< rawhtml >}}
<img src="..\images\quick-start\anim-blueprint.png" alt="Create Anim Blueprint" style="width:400px;"/>
{{< /rawhtml >}}

{{< rawhtml >}}
<img src="..\images\quick-start\animbp-class.png" alt="Select AnimBP Class" style="width:300px;"/>
{{< /rawhtml >}}

Under Class Defaults, set Anim Data to your character's Anim Data asset.

{{< rawhtml >}}
<img src="..\images\quick-start\animbp-defaults.png" alt="Set AnimBP Defaults" style="width:800px;"/>
{{< /rawhtml >}}

You may now assign the Anim Blueprint to the mesh in your pawn.

### 2D characters

For a basic rundown on Paper2D, [read the Unreal Engine Paper2D documentation.](https://docs.unrealengine.com/5.2/en-US/paper-2d-in-unreal-engine/)

After creating a flipbook, make a Flipbook Data asset.

{{< rawhtml >}}
<img src="..\images\quick-start\flipbook-data.png" alt="Create Flipbook Data" style="width:600px;"/>
{{< /rawhtml >}}

Assign flipbooks to it the same way you would 3D animations.

Now, navigate to your blueprint, and under Class Defaults, set the Flipbook Data variable.

{{< rawhtml >}}
<img src="..\images\quick-start\set-flipbook-data.png" alt="Set Flipbook Data" style="width:720px;"/>
{{< /rawhtml >}}

## Coding the state

Now that animations have been taken care of, let's get back to working on the Stand state.

From Parent: Exec, drag off the execution pin and add a new macro node called "Label Gate". This node lets you set "labels" in your state that you can return to later in the state. This is useful for looping animations, for example.

{{< rawhtml >}}
<img src="..\images\quick-start\label-gate.png" alt="Create Label Gate" style="width:800px;"/>
{{< /rawhtml >}}

Under Label Gate, you'll see a property named "Name". Set this to whatever you want, but I prefer "loop" for looping animations.

Then, drag off the Label Gate node, but this time, create a macro node named "Cel Gate".

{{< rawhtml >}}
<img src="..\images\quick-start\cel-gate.png" alt="Create Cel Gate" style="width:600px;"/>
{{< /rawhtml >}}

This node creates what I call a "cel", named after animation cels. The first cel in a state activates immediately, running the code from Exec. After that, no cels in the state will execute until the duration of the current cel elapses. When this occurs, the next cel connected to Skip will activate, and so on.

This is all well and good, but we haven't added anything to actually affect the state yet! So now, right click in the Event Graph and Get the Parent variable. Make sure this is the one under the category "Variables->States", and not a function!

{{< rawhtml >}}
<img src="..\images\quick-start\get-parent.png" alt="Get Parent" style="width:400px;"/>
{{< /rawhtml >}}

The Parent variable is a link to the Battle Object that currently owns the state. For states like Stand, this links to the player.

Drag off from the Parent variable and create a function node called "Set Cel Name".

{{< rawhtml >}}
<img src="..\images\quick-start\cel-name.png" alt="Set Cel Name" style="width:500px;"/>
{{< /rawhtml >}}

The Cel Name links directly to a Collision Data asset, grabbing the correct set of hurt/hitboxes, the animation to use, and the frame of the animation to use. More on that later.

Repeat creating Cel Gates and Cel Names like this until you are done with your state. 

Once the last frame of the state is reached, there are three mains options:

A. Exit the state. From the Skip execution pin on the last cel gate, drag off and create an "Exit State" macro. Depending on the current character stance (standing, crouching, jumping), this will jump to the corresponding state.

{{< rawhtml >}}
<img src="..\images\quick-start\exit-state.png" alt="Exit State" style="width:500px;"/>
{{< /rawhtml >}}

B. Jump to another state. Create a new Cel Gate with a duration of 1. From the Parent node, drag off and get the "Player" variable. This allows you to access any player-specific functions. Then, drag off from the Player node and create the function node "Jump to State". This node leaves the current state and goes to the specified state.

{{< rawhtml >}}
<img src="..\images\quick-start\jump-to-state.png" alt="Jump to State" style="width:720px;"/>
{{< /rawhtml >}}

C. Go to a label. Create a new Cel Gate with a duration of 1. From the Parent node, drag off and create the function node "Goto Label". If a label with the specified name was placed somewhere else in the state, this will goto that label, automatically activating the next cel gate. This is important for looping animations. 

{{< rawhtml >}}
<img src="..\images\quick-start\jump-to-state.png" alt="Goto Label" style="width:500px;"/>
{{< /rawhtml >}}

For the Stand state, you'll want to go to the label "loop" (or whatever you called it). 

## Create State Data Asset

Now that the Stand state has been completed, you need to assign the State to the pawn. To do this, you must create a State Data asset.

Create a Data Asset like with the animations, but this time, use a base class of State Data asset. Add an element to the State Array, and assign your Stand state to here. Make sure to not accidentally assign the base Stand state, or another character's Stand state!

Now, open your character blueprint. Under Class Defaults, assign your character's State Data asset to the value named "Chara State Data".

# Add to character select

Now you have a character that can animate! They're not very complete, but this is just a quick start guide.

However, you won't be able to use them unless you add them to the character select screen. To do so, navigate to Content/Blueprints/CharaSelect.

Open DA_CharaSelect, and add a new entry to Chara Select Structs. Put the character name here, and assign the correct blueprint to Player Class. The Chara Texture will be used to give an icon to the character select button.

{{< rawhtml >}}
<img src="..\images\quick-start\cselect-data.png" alt="Chara Select Data" style="width:1280px;"/>
{{< /rawhtml >}}

## Test it out

Now that everything is set up, you may run the game in-editor. Under Content/Maps, open the "MainMenu_PL" map. Press play, and try either Versus or Training. Select your characters, and have at it!

## Explore the engine

That's it for the quick start guide. However, there is much more the engine can offer than what I've described here! In the future, there will be a more complete character creation guide. For now, please experiment with what you find in my provided characters, and try to make something new!