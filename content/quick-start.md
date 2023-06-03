---
title: "Quick Start Guide"
date: 2023-01-26T01:04:19-08:00
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
<img src="..\images\quick-start\animbp-override.png" alt="Override AnimBP Initialize" style="width:800px;"/>
{{< /rawhtml >}}

You may now assign the Anim Blueprint to the mesh in your pawn.

# EVERYTHING BELOW HAS NOT BEEN REWRITTEN YET. PLEASE BE PATIENT.

### 2D characters

For a basic rundown on Paper2D, [read the Unreal Engine Paper2D documentation.](https://docs.unrealengine.com/4.27/en-US/AnimatingObjects/Paper2D/)

When creating flipbooks for the Night Sky Engine, there are a few extra things that must be done. Set Frames Per Second to 60, and set Frame Run to 5 for each sprite.

{{< rawhtml >}}
<img src="..\images\quick-start\paper2d.png" alt="Paper2D Settings" style="width:400px;"/>
{{< /rawhtml >}}

Now, navigate to your blueprint, and under Class Defaults, add an element to Flipbook Data for each of your character's flipbooks.

{{< rawhtml >}}
<img src="..\images\quick-start\flipbook-data.png" alt="Set Flipbook Data" style="width:400px;"/>
{{< /rawhtml >}}

## Coding the state

Now that animations have been taken care of, let's get back to working on the Stand state.

From Event On Enter, create a new node to Get the variable named Parent. This is a reference to the character blueprint. Using this reference, you may use any blueprint-exposed variable or function.

For now, drag off the Parent node, and make a node for the function "Set Cel Name". This sets the internal cel name, which is used to grab the correct frame for collision and animations. The notation for this is (animation name)_XX, where XX is the key frame number. For sprite and 3D ArcSystemWorks characters, this will also seek to the correct frame of the animation. For standard 3D characters, this will only set the correct animation. Standard 3D characters will increment their animation by one frame every update.

{{< rawhtml >}}
<img src="..\images\quick-start\cel-name.png" alt="Set Cel Name" style="width:500px;"/>
{{< /rawhtml >}}

Now, from Event On Update, create a node sequence like this: 

{{< rawhtml >}}
<img src="..\images\quick-start\update.png" alt="Update Function" style="width:900px;"/>
{{< /rawhtml >}}

Let me break down what's going on here:

This is used to get the current frame of the state. When plugged into a Branch node, you may use it to execute code only on a specific frame.

{{< rawhtml >}}
<img src="..\images\quick-start\on-frame.png" alt="Is on Frame" style="width:400px;"/>
{{< /rawhtml >}}

Is on Frame is a shortcut to compare the value of "Anim Time" against the provided frame number. Anim Time is incremented every frame by the engine, and resets when changing states. It may also be manually changed within the state in order to jump to different parts of the state.

Finally, we set the Cel Name if on the correct frame. This lets us update the animation frame and collision frame at the right time.

This process should repeated for each frame of the animation for sprite and 3D ArcSystemWorks characters, or for every hitbox update for standard 3D characters. 

Once the last frame of the animation is reached, there are two options:

A. Jump to a different state. From the Parent node, create a Jump to State node. New Name should be set to the name of the new state.

{{< rawhtml >}}
<img src="..\images\quick-start\jump-to-state.png" alt="Jump to State" style="width:600px;"/>
{{< /rawhtml >}}

B. Set the Anim Time to jump backwards in the state. From the Parent node, create a Set Anim Time node. Set this to one frame _before_ the frame you wish to jump to. This is useful for looping animations, such as standing, crouching, or walking. For 3D characters, you will also want to set the Anim BP Time to the proper frame of the animation; otherwise, the animation will pause at the last frame.


## Create State Data Asset

Now that the Stand state has been completed, you need to assign the State to the pawn. To do this, you must create a State Data Asset.

Create a Data Asset like with the animations... but this time, use a base class of State Data Asset. Add an element to the State Array, and assign your Stand state to here. Make sure to not accidentally assign the base Stand state, or another character's Stand state!

Now, open your character blueprint. Under Class Defaults, assign your character's State Data Asset to the value named "State Data Asset".

{{< rawhtml >}}
<img src="..\images\quick-start\state-data.png" alt="State Data Asset" style="width:1280px;"/>
{{< /rawhtml >}}

# Add to character select

Now you have a character that can animate! They're not very complete, but this is just a quick start guide.

...However, you won't be able to use them unless you add them to the character select screen! To do so, navigate to Content/UI/CharaSelect.

Open CharaSelectData, and add a new entry to Chara Datas. Put the character name here, and assign the correct blueprint to Player Class. The Chara Texture will be used to give an icon to the character select button.

{{< rawhtml >}}
<img src="..\images\quick-start\cselect-data.png" alt="CharaSelectData" style="width:1280px;"/>
{{< /rawhtml >}}

Now, open W_CSelectLocal, and copy-paste one of the character select buttons and put it somewhere. No need to modify it in any way, it will automatically be assigned the correct data.

{{< rawhtml >}}
<img src="..\images\quick-start\cselect.png" alt="Character Select" style="width:1280px;"/>
{{< /rawhtml >}}

## Test it out

Now that everything is set up, you may run the game in-editor. Select your characters (one per side), select a stage, and then select LocalPlay. If everything went right, you should see the characters standing on each side in their standing idle!

## Explore the engine

That's it for the quick start guide. However, there is much more the engine can offer than what I've described here! In the future, there will be a more complete character creation guide. For now, please experiment with what you find in my provided characters, and try to make something new!