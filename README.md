# SpriteAnimator

[SpriteAnimator](https://github.com/fatmanspanda/SpriteAnimator/wiki/Sprite-Animator) is a program designed to help debug the appearance of a sprite as it's being created. It uses information based off of Link's animations in game to recreate every animation with an imported sprite. It is downloadable as a runnable `JAR` file from [the super project's release page](https://github.com/fatmanspanda/ALttPNG/releases).

## Features

Along with animating sprites, SpriteAnimator also includes various features to control the display:
* Backgrounds to test how a sprite looks in various areas
  * Character sprites can be moved against the background by clicking or dragging your mouse pointer. (v1.5+)
* The ability to view animations with a sword or shield of any level
* The ability to view character sprites in any of the 4 mail palettes
* The ability to toggle shadows
* The ability to toggle sprites that don't fall under the above categories (NOTE: Swag duck cannot be turned off)
* The ability to toggle neutral poses (default standing position) to assess how smoothly a sprite moves into an animation
* The ability to zoom in on sprites, up to 500%
* The ability to speed up or slow down animations
* The ability to view animations step-by-step, which also includes a table of what sprite sheet cells are used and how they are transformed.

## Terminology used in this project
Some terms we need to use are overloaded (they can mean multiple things). To avoid confusion, here's a brief glossary of what means what when we say it. Not everything is perfectly written, so feel free to ask questions if something doesn't seem to mean what you think.
* **Frame** - A repaint cycle of the SNES.
  * *The SNES runs at 60 **frames** per second.*
* **Step** - A distinct section of a full animation. Steps last for a variable number of frames. *Note: The same animation can have a variable number of steps, as identical steps will be merged when possible; see notes below.*
  * ***Step 1** of the hammer animation shows Link holding the hammer in the air for 3 frames.*
* **Sprite** - A specific entity or subentity's graphic.
  * *Link's attack animation includes a **sprite** for the sword.*
  * *Link himself is normally composed of 2 **sprites**: a head and a body.*
* **Cell** - A specific subentity of a full character sprite, defined by strict bounds.
  * *Link's right-facing head is the first **cell** of his sprite sheet.*
* **Index** (*pl. indices*) - An alphanumerical name for a location on the character sprite sheet. *see: [Mike's chart](http://alttp.mymm1.com/sprites/sheets/?sprite=link&skin=green)*
  * *Link's dungeon map icon is **index** K7 on his sprite sheet.*

## Notes about animation decisions
* SpriteAnimator tries its best to emulate the timing of the SNES (60 FPS); however, it is slightly slower with its repaint cycles. 1/60 of a second is approximately 16.66ms. The closest this program can achieve without massive overhead is 17ms.
* Most left-facing animations are simply a mirror of the right-facing animation, and as such have been omitted.
* Every bug net animation uses the same poses, but in a different order. As such, only the right-facing swing was included.
* Miscellaneous sprites were included to make sure held items could be held as best as possible; however, other effects (such as dust clouds and sparkles) were omitted to decrease the workload in creating a finished product.
* For end-user control, consecutive identical animation steps are merged. For example: when neutral poses are off, attack right has 9 steps with a sword and 7 steps without. This is because in step pairs [1,2] and [8,9], Link does not change his pose, but the sword still moves.
* The character sprite in the zap animation disappears every other frame. This is to emulate Link's invincibility flicker, which the animation is never seen without.
* Ether is a very long animation because there are palette swaps every 4 frames in Link's final pose. It might even need to be longer to be truly accurate, as spanda got bored and frustrated with counting.

## Animation oddities that are not errors
The following apparent errors are problems directly with the game itself, and not this program. They are best seen with the vanilla Link sprite. Unless otherwise stated, animation steps referenced have no equipment displayed and no neutral poses (Sword level: Off; Shield level: Off; Misc. sprites: Off; Neutral: Off).
* Stair-climbing can vary by location. The stair-climbing animations in this program were researched from Blind's hut in Kakariko Village. (Oddity discovered by Osaka)
* The bunny walk animations seem fast. Movement is apparently faster when bunny form is activated by a spinner. (Oddity discovered by Glan)
* Hookshot right has no shield, even though hookshot up and hookshot down do.
* In attack right, consecutive steps 5, 6, 7 have alternating overlap between Link's arm and Link's head.
* In attack up, Link's head goes particularly high above his body on step 4.
* Quake does not have the smoothest movement in the jump and fall.
* The sword on the final step (43) of quake is cut off. This was probably done in vanilla to make it appear to be in the ground.
* The sprites for the shovel are very small.
* Index `AB7`, a normally blank cell, appears on steps 5 and 10 of tree pull. 
* Link's head becomes detached from his body (empty space visible) in the following animations:
  * Attack (step 3)
  * Swim (step 4)
  * Grab down (steps 2, 3, 4)
  * Push down (all steps)
  * Ether (steps 10–51)

## Known bugs/issues
* Very rarely, the animation process will enter a hyperspeed mode. The problem appears to be a thread conflict that comes down to frame-perfect timing, making it difficult to reproduce. This error is fixed by pressing the reset button.
* Tooltips have a lifetime of only 596 hours, after which you must move your mouse out-then-in to refresh them.

## Animation data
The bulk of the groundwork for this program was researched by Mike Trethewey and TWRoxas, available [here](http://alttp.mymm1.com/sprites/includes/animations.txt). From there, individual animations were adjusted and researched frame-by-frame, with some help from RyuTech.

As of v1.5, animation data is stored in a `json` format, free for anyone to use. It is only asked that you credit the research time devoted here for that data.

SpriteAnimator imports the [`org.json` library](https://github.com/stleary/JSON-java) created by user stleary (Sean Leary).

JSON file:
* [/Database/AnimationData.json](https://github.com/fatmanspanda/SpriteAnimator/blob/master/SpriteAnimator/Database/AnimationData.json)

Definitions:
* key `row` in `sprite` array objects: [/Database/SheetRow.java](https://github.com/fatmanspanda/SpriteAnimator/blob/master/SpriteAnimator/Database/SheetRow.java)
* key `size` in `sprite` array objects: [/Database/DrawSize.java](https://github.com/fatmanspanda/SpriteAnimator/blob/master/SpriteAnimator/Database/DrawSize.java)
* key `trans` in `sprite` array objects: [/Database/Transformation.java](https://github.com/fatmanspanda/SpriteAnimator/blob/master/SpriteAnimator/Database/Transformation.java)

Resources:
* Item sprites: [/images/equipment.png](https://github.com/fatmanspanda/SpriteAnimator/blob/master/images/equipment.png)
* Zap palette colors: `static final byte[][] ZAP_PALETTE` (signed bytes) in [SpriteManipulator/SpriteManipulator.java](https://github.com/fatmanspanda/SpriteManipulator/blob/master/SpriteManipulator.java)
