# **Bouncy Material**
> How this material works and how to add it to your MKDD custom tracks

## About

MKDD Extender now has a patch that adds a mechanic featured in many later Mario Kart titles; __Bounce Pads__. This site goes over the mechanics added,
and how to implement them into your own Custom Tracks.

[Mechanics](#mechanics)

[How to use](#how-to-use)

[Known issues](#known-issues)

[Bugs](#bugs)



## Mechanics


### Basics
When driving over ground marked as a bounce pad, Karts will be propelled forwards and upwards by an amount of force specified by a track creator.

* A track can have as many bounce pads as it has materials; which is to say, __there is not really a hard limit on how many different Bounce Pads
can exist in one track__.


### Launch / Liftoff
Before you bounce off of a bounce pad, your speed may be influenced by a Mushroom, or a Mini Turbo:

* A Mushroom, or similar effects such as dash pads will __increase your forwards and upwards momentum during the bounce__.

* A Mini Turbo will similarly increase your forwards momentum, but it will also __lower the upwards momentum during the bounce__.

  * This means you may not want to Mini Turbo before bouncing if you're trying to go up.

  * Either a Mini Turbo or Boost will set forwards momentum up to a minimum if a Bounce Pad does not add a lot it.

  * The effects of a __Mushroom and a Mini Turbo can stack__ additively.

* _Stars_ and _Lightning_ do __not__ affect your momentum after being bounced.

Holding Drift while touching a Bounce Pad will increase your upwards momentum.


### Airborne State
The time spent airborne after being launced this way is different than usual ramps and cannons.
While airborne after touching a Bounce Pad:

* Your forwards velocity will never change.

* You can slightly move your Kart left or right.

* Holding Drift while airborne after a bounce will decrease the speed of your ascent, but not descent.

* You can hold up or down to increase or decrease the speed of your descent, respectively.

 * Example of the effect of holding a direction below.


https://github.com/lance-o/bouncy-material/assets/61329703/4c827cb3-199a-4d9e-aff6-792af76c517e


## How to Use

### If you already know how to make Custom Tracks, then you may read the [tldr version](#tldr).

To add this to your Custom Track, you'll need some things.

* [Blender](https://www.blender.org/download/) for adding the correct materials (and honestly the entire rest of a Custom Track).
* [MKDD-Collision](https://mkdd.org/wiki/Mkdd-collision) to convert the collision data to something Blender can use and back.
* [GCFT](https://mkdd.org/wiki/GCFT) to extract and repack .arc files from MKDD's main.dol.
* [MKDD-Extender](https://github.com/cristian64/mkdd-extender/releases/tag/v1.8.0) to patch your MKDD.iso with the Bouncy materials patch.
* [Dolphin](https://dolphin-emu.org/download) to play Mario Kart Double Dash!! and test your settings for your bounce.
* [Dolphin Memory Engine](https://github.com/aldelaro5/dolphin-memory-engine/releases/tag/v1.0.0) for help with setting how far to bounce you.

Tutorials on how to use these programs can be found [here](https://mkdd.org/wiki/Category:Tutorials).
The most important thing is extracting the __.bol__ (collision) file from MKDD and converting it into a __.obj__ file.

Import your __.obj__ into Blender.

![blender_fjZrPXNCgt](https://github.com/lance-o/bouncy-material/assets/61329703/bac8225c-21b0-46c1-8ecb-fe0dfaf89765)





You'll probably not see anything. This is because MKDD tracks are actually ridiculously large. 

To see it, first press __N__. This will bring up a bar on the side. There should be a tab called __"View"__.

Select the paramater __"End"__ and type in some huge number. I just type __99999999__ every time.

![blender_iUGIBZpenh](https://github.com/lance-o/bouncy-material/assets/61329703/5148d5b2-5d5d-4925-98a6-05543ebb6169)



Zoom out by scrolling your mouse wheel down, and hopefully your collision model should come into view. It might take a lot of scrolling,
because it really is huge.

![blender_OmAAqMgvUy](https://github.com/lance-o/bouncy-material/assets/61329703/28d56439-95e8-439c-9d4d-423a7d9696a1)



If your model has an orange outline, then it's __selected__, which is good. If it doesn't, click on the model itself or in the inspector
on the right. Then, go to __Edit Mode__.

![blender_Jwvqr6gohD](https://github.com/lance-o/bouncy-material/assets/61329703/39135978-ccbe-4066-bec5-f606bb3d3192)


After switching to edit mode, you should be able to see vertices and edges all over the model. If you can't, try pressing and holding __Z__.
This will allow you to change how Blender renders the mode. I like to keep mine in __Solid__, and __Wireframe__ is useful as well.

Near where you changed the mode to __Edit Mode__, there should be these things:

![blender_2Y52WDut7w](https://github.com/lance-o/bouncy-material/assets/61329703/d8480337-76a1-454f-a7fe-e29677e51616)

From left to right, these correspont to __Vertices__, __Edges__ and __Faces__. I'm going to select the third one, so that
Blender is now in __Face Select Mode__. That means that when I click on my model, it will select an entire face of
the model.

Go ahead and select a face on your track.

Now, let's take a look at the __Material__ of the track. Near the bottom-right, click this thing.

![blender_prbLTnfj3z](https://github.com/lance-o/bouncy-material/assets/61329703/c5c9fab3-a257-4649-8a90-05aa96d0f78e)

This will open up the __Material Properties__ tab. This is where we actually set a material to use the bounce material.

Over here is our ground materials. 

![blender_faMWzfg56i](https://github.com/lance-o/bouncy-material/assets/61329703/b5827486-ffab-474f-969e-24740d42f399)

Click anywhere on the model (with __Face Select Mode__ enabled), and the entry over on the right will change to whatever 
that face's material is set to. If you're creating a new .bco from scratch, I'd recommend looking at a vanilla one to
see what it should look like.

Now, the name of the material is important. As an example, let's say a material name is this:

> Roadtype_0x0101_0x01_0x00000000.001

The part that dictates what kind of road it is, is the first 0x0101 area. This can be split up into

> 0xAABB

where AA is the collision flag, and BB is the sound flag. In the case of my example material, that means the collision
flag is 01 and the sound flag is 01 as well.

The first part, the collision flag, is what you'll need to change for the patch to recognize that this part of the track should bounce you.
To make it bounce, it needs to be set to __0xB0__. If we renamed the material to make it bounce, it would now be 

> Roadtype_0xB001_0x01_0x00000000.001

Instead of renaming this material, we'll make a new one; otherwise, every piece of ground that originally used this material
will bounce you. 

To do that, press the __"+"__ button on the right of the Materials list, and then press __"New"__ underneath the Materials list.

![blender_jBKwERhL2v](https://github.com/lance-o/bouncy-material/assets/61329703/6fb1a003-9c96-4dc5-a2a6-85e628838bf9)
![blender_4bWtoHuVxO](https://github.com/lance-o/bouncy-material/assets/61329703/43984818-d6d2-4a3e-b3cb-c8e256812be8)

Type in the box what you need your material name to be called. In my case, it's this:

![image](https://github.com/lance-o/bouncy-material/assets/61329703/6e769841-e8a9-4b20-ac95-4a22b1c87456)

Press Enter, and your new material's name will be updated to reflect what you typed in.

As a note, the __".001"__ on the end of the Material name is irrelevant. You may remove it if it annoys you.

The material is set up so that it's recognized as a Bounce Pad, but it still needs to be set so that it knows how strong
a bounce it should apply. That part is defined in the 8-character hash at the end of the material name.

Currently, it's set as

> 0x00000000.001

or, without the irrelevant ending

> 0x00000000

This string will be read as a hexadecimal number. Actually, it'll be read as __two__.
It can be split up into the following:

> 0xYYYYFFFF

where YYYY is the amount of upwards momentum, and FFFF is the amount of forwards momentum.

So, if I put in 50005000, it would launch you by 5000 upwards and 5000 forwards, right?

Not exactly, because

* It's hexadecimal, so it would actually be 0x5000, or 20480 in decimal.

* The number entered is actually divided by 100 (in decimal, not hexadecimal).

So then, 50005000 would actually launch you by 204.8 both upwards and forwards.

So, how far is that? It's hard to gauge on your own. There is another feature of the Bounce Pad 
to help with this:

> If the 8 character hash is set to exactly 00000000, then it instead reads from somewhere in memory.

For this, we'll need to apply the material to the ground and compile everything we have so far into a playable .iso.

Firstly, in Blender, select whatever face(s) you want to set to your new bounce material.

![blender_kp3g70eoGm](https://github.com/lance-o/bouncy-material/assets/61329703/2d997762-c977-47e7-9c75-b3e6a1c940a1)

Then, in the __Material Properties Tab__, scroll down until you find the material you just made.
Click on the material.

With the faces you want to apply it to still selected, click __"Assign"__.

![blender_FMkEAudmzq](https://github.com/lance-o/bouncy-material/assets/61329703/fa8bdb23-dce2-43bc-898f-d1ed5be8e4e9)

Those faces will now be using your new material, instead of whatever they used before. To test this,
click on some other faces that don't use your new material, and scroll back to the new material and click on it.

Press __"Select"__, and the faces you set to your new material should now also be selected.

Now we know it's set, export the scene as __.obj__.

![blender_0llxSWuQci](https://github.com/lance-o/bouncy-material/assets/61329703/97f01d52-2aa2-4f47-8eec-ef17ad83860a)

You don't need to mess with any settings while saving, the defaults should be fine.

Since you already had a __.obj__ of your course, be careful to rename this one so you know it's been changed.

Go through the entire process of putting it back into its original .arc file. That means putting it back through __MKDD-Collision__,
and into its original .arc extracted folder, renamed and then packed back into a .arc.

Now, it needs to be put through MKDD-Extender. If you don't know how to set up a mod for use with MKDD-Extender, 
you may find [MKDD-Patcher](https://github.com/RenolY2/mkdd-track-patcher)'s instructions on how to format a mod folder particularly useful.

With your mod folder set up correctly, open __MKDD-Extender__.

![image](https://github.com/lance-o/bouncy-material/assets/61329703/1a4f7d63-7ccc-41fa-b006-b21e72d6faf9)

At the top, click "__Browse__" next the to "__Input ISO File__" bar and locate your __.iso__ of Mario Kart: Double Dash!!. 

If "__Output ISO File__" does not automatically fill, click "__Browse__" next to that as well, and enter the name you want the patched iso to have.

"__Custom Courses Directory__" should be set to the location of the mod folder you set up.

If everything is entered, it should look like this:

![image](https://github.com/lance-o/bouncy-material/assets/61329703/98da72e6-c25a-4fc2-b7ab-63f6a52220ab)

Now, on the left are all the mods that MKDD-Extender has detected. Assuming the one you made has appeared in there, drag it onto one of the 
slots in the middle. This will add your modded track to the corresponding track slot in-game.

To enable the Bouncy materials patch, click on "__Options__".

![image](https://github.com/lance-o/bouncy-material/assets/61329703/d548f8ee-370a-4163-b3bb-796eb9d22024)

Under "__Code Patches__", you should find "__Bouncy Material__". Tick the box next to it, and now when you press "__Build__"
on the right of the main window of MKDD-Extender, it will apply that patch to your new, patched Double Dash __.iso__.

Open up your new __.iso__ in __Dolphin__, and also, open up __Dolphin Memory Engine__. Load up your modded track in Time Trials,
and if all has gone correctly, it should be your modded track.

Now, turn your attention to __Dolphin Memory Engine__. 

![image](https://github.com/lance-o/bouncy-material/assets/61329703/3bdbf4d7-b502-4c18-928d-5e8a539557f0)

Launch the Memory Viewer.

![image](https://github.com/lance-o/bouncy-material/assets/61329703/3ef1d3eb-00f0-48ba-9489-1daa744e7c04)

You're now looking at the real-time memory of Mario Kart: Double Dash!!. 

In the box named "Jump to an address", type the following:

> 8000523c

![image](https://github.com/lance-o/bouncy-material/assets/61329703/fbfce198-d9e1-40f2-b4f7-62fa0fcbc5e8)

It should look like that. 

Remember how we left the 8-character hash at the end of your material name as "00000000", as it would instead
read from "somewhere in memory"?

The first 8 characters __Dolphin Memory Engine__ is that place in memory. You can now type in whatever you'd like into
those 8 characters, and then drive onto the track you assigned the Bounce Pad material to. It should bounce you in accordance to 
what you just put in _Dolphin Memory Engine__.

Now, you can change the direction it bounces you on the fly. When you find a setting you like, you may write it down 
(or, if Blender is still open, go and change the material immediately). 

https://github.com/lance-o/bouncy-material/assets/61329703/36d60976-8698-4404-891b-bf182b9b9208

> NOTE: This example is from when the address used was different. Please only use the address "0x8000523C".

Now, for every new Bounce Pad material you add to the track, you may use the workflow of setting the ending 8 characters as 
"00000000" in __Blender__ to do this in __Dolphin Memory Engine__, and then enter the final setting into __Blender__ when you
have found one you like.

If your Custom Track is ready to be uploaded, please remember to add a "code_patches" line to your trackinfo.
It should look something like the following:

![image](https://github.com/lance-o/bouncy-material/assets/61329703/6a139cf4-5728-4928-bec7-ceaa6201f2de)


### tldr

For the game to use Bounce Pad, set up your material according to the following structure:

> Roadtype_0xB0XX_0x01_0xYYYYFFFF

with B0 being the Bounce Pad collision flag, XX being the sound flag you want your material to have, YYYY being the amount
of force to add upwards x100 in hex and FFFF being the amount of force to add forwards x100 in hex.

Since it's hard to gauge exactly how far it'll push you based on that information, leave YYYYFFFF as 00000000.
This will cause the patch to read from a place in memory instead.

Use __Dolphin Memory Engine's Memory Viewer__ and go to 0x8000523C. Treat the first 8 values the same way you
would the material name; you may experiment with different bounce settings in this way without having to recompile the game.

https://github.com/lance-o/bouncy-material/assets/61329703/36d60976-8698-4404-891b-bf182b9b9208

If your Custom Track is ready to be uploaded, please remember to add a "code_patches" line to your trackinfo.
It should look something like the following:

![image](https://github.com/lance-o/bouncy-material/assets/61329703/6a139cf4-5728-4928-bec7-ceaa6201f2de)

## Known Issues

There are imperfections in the current implementation.

Firstly, it cannot work on the Demo/Debug version of the game. This isn't a huge loss, but for completion's sake it
may be important that this is eventually possible.

Secondly, the Kart's statistics are currently not taken into account when starting a bounce. This means, all karts 
will have the exact same speeds when bouncing. This includes external modifiers to Kart speed, such as the current CC.

Thirdly, there's...

# Bugs

### __Clipping__

* Gold Kart will regularly clip into the ground when landing. This appears to be a quirk of its low suspension.

### __Unintended Side-Effects__

* Getting hit on top of a bounce pad will often cause Karts to rotate and bounce in a different direction to the one they were initially
trying to go.

* Bonking into a wall will not remove any momentum from the Kart. This is ordinarily fine, but in the case where a thin pillar is 
collided with, the Kart will appear to stop, fall, then fly forwards in the direction it was going.

### __High Speeds__

* The highest speeds possible from bouncing *should* not cause problems, but it is known that travelling *very fast* will cause
crashes. The theory for why is to do with the speedometer attempting to display numbers that it was never meant to handle,
but as it persists, _it is advisable to not use the bounce material in situations where a Cannon object would do the job_.

### __Bug Reports__

If you'd like to report a bug that isn't listed, or have had a bug reported to you by someone playing your track, then the person
to message is __@alllarrynolurr__ via __Discord__.
