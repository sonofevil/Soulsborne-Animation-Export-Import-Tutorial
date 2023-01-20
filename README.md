# Soulsborne animation export & import tutorial

This tutorial is aimed at teaching you the basics of working with animations in Soulsborne games (DS1,2,3, Sekiro, Bloodborne, Elden Ring). It won't teach you how to modify animations, but rather to get to the point where you have something to modify in the first place. As an example I will use the process of porting an animation from the small character model of Sif in the DLC of Dark Souls PTDE to the big character model of Sif from the main game, which is what I will be doing in Part 2 of this tutorial.

I will not explain how to export character models along with animations, because I haven't done that. For modding animations in Soulsborne games, you don't need to mess with the model.

## Prerequisites:
A PC running Windows.
A soulsborne game unpacked for modding. (Use UXM Unpacker: https://www.nexusmods.com/eldenring/mods/1651)

## Tools:
Which tools you need depends on what you are trying to achieve. If you just want to get an animation into a 3D editing program like Blender, you only need 3 tools. If you want you export an animation, possibly modify it, and then import it again, you need much more than that.

But in any case, you'll need [Yabber](https://github.com/JKAnderson/Yabber/releases) to extract .hkx animation files from .anibnd archive files.

## Part 1: I only want to export an animation to use in Blender. I don't care about anything else.

### A: Preparations

In this case, all you need is Yabber, Blender, 3DSMax and the HavokMax plugin.

Download [Yabber](https://github.com/JKAnderson/Yabber/releases) and unzip it anywhere. 
Install [Blender](https://www.blender.org/). 

#### Installing 3DS Max

The 3DS Max version you need depends on the game. If it's Dark Souls Prepare To Die Edition, you'll need 3DS Max 2010 32bit. For later games it's generally safe to assume that FromSoft was using a 3DSMax 64bit version from the year before the game came out, but don't take my word for it. Ask around.

You can get 3DS Max versions starting with 2019 from the Autodesk homepage, but you'll need an account to access the download. If you are a student, you can sign up for a free student license, which is what I did. 2010 still had a 30 day trial without need for signing up, but I'm not sure about current versions. If you can't use an official download, see archive.org link below.

If you want 3DSMax 2018, follow the instructions in this video: https://www.youtube.com/watch?v=B5KjZglDpmQ

For old 3DSMax versions like 2010, look on [archive.org](https://archive.org/search?query=title%3A%283ds+max%29&and%5B%5D=mediatype%3A%22software%22). You don't need an account for this.

Obviously I cannot explain to you how to get unlicensed access to commercial software.

Then install 3DS Max. You can uncheck the other components offered in the installer, you don't need them for this. Fair warning that 3DSMax insists on using several gigabytes of your system partition, regardless of whether you actually install it there.

#### Installing HavokMax

Download a version of [HavokMax](https://github.com/PredatorCZ/HavokMax/releases) *that will work with your game*. If the game is Dark Souls Prepare To Die Edition, you'll need [version 1.10](https://github.com/PredatorCZ/HavokMax/releases/tag/v1.10), because later versions no longer support 32bit, and PTDE is a 32bit game. If it's any newer game, just download the latest version of HavokMax.

From the archive, extract the folder that corresponds to your 3DSMax version. Move the HavokMax.dlu file inside to the "\Plugins" folder inside the 3DS Max installation folder.

### B: From game to Blender

This part involves extracting the animation with Yabber, importing it into 3DS Max, and saving it as something Blender can import.

#### Extracting the animation with Yabber

Yabber doesn't have a GUI, so there are 3 ways to use it: As a command line utility, via drag-and-drop or via the Windows context menu.

Context menu is easily the most convenient, especially if you're a Soulsborne modder and need to use Yabber a lot. To register Yabber in your context menu, simply run Yabber.Context.exe from the Yabber folder you extracted earlier, and follow the instruction. Afterwards you can extract any .[...]bnd file simply by right-clicking it and selecting "Yabber".

Otherwise you can drag and drop the bnd files onto Yabber.exe (or Yabber.DCX.exe if your files end in .dcx) to get the same result. I won't explain how to use the command line.

Next you need to go to your game's install folder (the one where you ran UXM Unpacker). If you actually ran UXM, you'll see a lot of folders with names like "sound" and "script". If not, scroll up and read how to use UXM on their NexusMods page. The only folder that interests us for the purpose of this tutorial is "chr". Here you will find all character models and animations.

You'll need to find out what the ID of the character whose animation you want is. If it is a player model character (yourself and regular human NPCs that are the same size as you), it is most likely c0000. You can find other IDs by referring to one of the pages linked on [this website](http://soulsmodding.wikidot.com/reference:main). Another good way to figure out IDs is to load the map with that character into DSMapStudio and click on the character to see the entity properties.

Once you have the ID, you need to find the .anibnd file that starts with that ID, e.g. c0000.anibnd for the player model animations. Use Yabber to extract it as explained above. Yabber will extract the file to a folder named "[ID]-anibnd" (e.g. c0000-anibnd) inside the "chr" folder. If you enter that folder you will see a "_yabber-bnd3.xml" file and a "Model" folder. The .xml file contains information about which files were extracted, so Yabber knows which files to compress again later, but it doesn't need to concern us here. If you go to "Model\chr\[ID]\\", you'll see a "taeNew" folder and a "hkxwin32" or "hkxwin64" folder. "taeNew" contains animation effects, but we'll ignore those. The hkxwin folder contains a large number of .hkx files with IDs as names, which are the actual animation files. The IDs are what the game uses in its scripts to load animations. There is also a "Skeleton.hkx" files which defines the limbs and joints which will get moved around by the animations.

#### Importing into 3DS Max

First you need to figure out the right animation ID. The best way to do so is to open the .anibnd file in [DSAnimStudio](https://github.com/Meowmaritus/DSAnimStudio/releases/tag/ver4.9.3) and find it in the list of animations in the left panel. Make sure to select "Load UXM unpacked game files" in the project setup dialogue, or it won't let you load the character model. See here for how to use DSAnimStudio: https://github.com/Meowmaritus/DSAnimStudio

To import an animation into 3DS Max, open 3DS Max, click File (or the 3DS Max logo in the upper left corner in early versions) -> Import. Navigate to the "hkxwin32|64" folder and open "Skeleton.hkx". You should see a t-posing stickman version of your character in the viewports. Next, go to Import again and open the .hkx file with the animation ID you want. The stickman should switch to the pose of the first animation frame. In the timeline at the bottom you can see the other frames represented by little rectangles. You can click the "Play animation" arrow in the lower right to watch the animation loop.

#### Exporting for Blender

For this step simply go to File -> Export and export as an .fbx file anywhere you want. You'll see a lot of options that will be important if you want to do stuff like retargeting, but for our purposes, all we need to make sure is that the Include -> Animation checkbox is checked. You'll likely see a popup with warnings. Ignore them.

Next open blender, go to File -> Import -> FBX and select the exported .fbx file to import the animation. Now you can do whatever you were planning to do with it.

## Part 2: 
