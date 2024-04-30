# **Extended Terrain Types**
> A glossary detailing the new Terrain Types added by the "Extended Terrain Types" patch
## About

MKDD Extender now has a patch that adds a mechanic featured in many later Mario Kart titles; __Bounce Pads__. This site goes over the mechanics added,
and how to implement them into your own Custom Tracks.

[Bouncy Terrain](#/bouncy_terrain_type/BOUNCY_TERRAIN_TYPE.md)

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

  * Either a Mini Turbo or Boost will set forwards momentum up to a minimum if a Bounce Pad does have a lot of forwards momentum.
 
    * You probably shouldn't be setting the forwards momentum to be so low that this breaks your track.  

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


https://github.com/lance-o/bouncy_material/assets/61329703/d479ea74-63ad-4cb7-9b22-704134b7dbc0



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

![blender_fjZrPXNCgt](https://github.com/lance-o/bouncy_material/assets/61329703/81d953a7-81b1-426f-9b6c-f6487f292da3)


You'll probably not see anything. This is because MKDD tracks are actually ridiculously large. 

To see it, first press __N__. This will bring up a bar on the side. There should be a tab called __"View"__.

Select the paramater __"End"__ and type in some huge number. I just type __99999999__ every time.


![blender_iUGIBZpenh](https://github.com/lance-o/bouncy_material/assets/61329703/2f7f0757-2467-4799-afe8-82ce0aa8afd6)



Zoom out by scrolling your mouse wheel down, and hopefully your collision model should come into view. It might take a lot of scrolling,
because it really is huge.


![blender_OmAAqMgvUy](https://github.com/lance-o/bouncy_material/assets/61329703/18a11824-47d1-4e56-b867-a850f960fdde)


If your model has an orange outline, then it's __selected__, which is good. If it doesn't, click on the model itself or in the inspector
on the right. Then, go to __Edit Mode__.

![blender_Jwvqr6gohD](https://github.com/lance-o/bouncy_material/assets/61329703/c1ef9729-4fb7-43ae-8416-389928e9e210)


After switching to edit mode, you should be able to see vertices and edges all over the model. If you can't, try pressing and holding __Z__.
This will allow you to change how Blender renders the mode. I like to keep mine in __Solid__, and __Wireframe__ is useful as well.

Near where you changed the mode to __Edit Mode__, there should be these things:

![blender_2Y52WDut7w](https://github.com/lance-o/bouncy_material/assets/61329703/b91fbb65-f38a-497a-a41e-c0020c0e771d)

From left to right, these correspont to __Vertices__, __Edges__ and __Faces__. I'm going to select the third one, so that
Blender is now in __Face Select Mode__. That means that when I click on my model, it will select an entire face of
the model.

Go ahead and select a face on your track.

Now, let's take a look at the __Material__ of the track. Near the bottom-right, click this thing.

![blender_prbLTnfj3z](https://github.com/lance-o/bouncy_material/assets/61329703/8fca615b-8c47-4f06-9596-2634133502f0)


This will open up the __Material Properties__ tab. This is where we actually set a material to use the bounce material.

Over here is our ground materials. 

![blender_faMWzfg56i](https://github.com/lance-o/bouncy_material/assets/61329703/312dd296-b85a-43bf-a827-865228a56c4d)

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

![blender_jBKwERhL2v](https://github.com/lance-o/bouncy_material/assets/61329703/4d11d907-91d4-491b-bd01-c975f870b85d)
![blender_4bWtoHuVxO](https://github.com/lance-o/bouncy_material/assets/61329703/4cf767f4-e0dc-48a5-ad5f-db139fd0f5c3)


Type in the box what you need your material name to be called. In my case, it's this:

![blender_lP9GoVmDfO](https://github.com/lance-o/bouncy_material/assets/61329703/eaf801f6-5574-4338-b545-c5d5bbf8fecf)


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


![blender_kp3g70eoGm](https://github.com/lance-o/bouncy_material/assets/61329703/f399233a-61b0-45e3-83e7-9a07cdf91475)

Then, in the __Material Properties Tab__, scroll down until you find the material you just made.
Click on the material.

With the faces you want to apply it to still selected, click __"Assign"__.

![blender_FMkEAudmzq](https://github.com/lance-o/bouncy_material/assets/61329703/73a4d83d-1907-4030-8f84-6949fc1243d3)

Those faces will now be using your new material, instead of whatever they used before. To test this,
click on some other faces that don't use your new material, and scroll back to the new material and click on it.

Press __"Select"__, and the faces you set to your new material should now also be selected.

Now we know it's set, export the scene as __.obj__.

![blender_0llxSWuQci](https://github.com/lance-o/bouncy_material/assets/61329703/65d14c08-6d79-43a3-83ed-faedf3938d02)


You don't need to mess with any settings while saving, the defaults should be fine.

Since you already had a __.obj__ of your course, be careful to rename this one so you know it's been changed.

Go through the entire process of putting it back into its original .arc file. That means putting it back through __MKDD-Collision__,
and into its original .arc extracted folder, renamed and then packed back into a .arc.

Now, it needs to be put through MKDD-Extender. If you don't know how to set up a mod for use with MKDD-Extender, 
you may find [MKDD-Patcher](https://github.com/RenolY2/mkdd-track-patcher)'s instructions on how to format a mod folder particularly useful.

With your mod folder set up correctly, open __MKDD-Extender__.

![python3 10_PhXEVxh0Pv](https://github.com/lance-o/bouncy_material/assets/61329703/23891722-900c-4023-a8e5-00dc7e1523a1)

At the top, click "__Browse__" next the to "__Input ISO File__" bar and locate your __.iso__ of Mario Kart: Double Dash!!. 

If "__Output ISO File__" does not automatically fill, click "__Browse__" next to that as well, and enter the name you want the patched iso to have.

"__Custom Courses Directory__" should be set to the location of the mod folder you set up.

If everything is entered, it should look like this:

![python3 10_HAIiksjc5Z](https://github.com/lance-o/bouncy_material/assets/61329703/8dc3e04f-1c26-48c0-9fb8-8a3719049873)

Now, on the left are all the mods that MKDD-Extender has detected. Assuming the one you made has appeared in there, drag it onto one of the 
slots in the middle. This will add your modded track to the corresponding track slot in-game.

To enable the Bouncy materials patch, click on "__Options__".

![python3 10_v6IVYr44sM](https://github.com/lance-o/bouncy_material/assets/61329703/806555b7-4149-4176-bf29-c5305f18216e)


Under "__Code Patches__", you should find "__Bouncy Material__". Tick the box next to it, and now when you press "__Build__"
on the right of the main window of MKDD-Extender, it will apply that patch to your new, patched Double Dash __.iso__.

Open up your new __.iso__ in __Dolphin__, and also, open up __Dolphin Memory Engine__. Load up your modded track in Time Trials,
and if all has gone correctly, it should be your modded track.

Now, turn your attention to __Dolphin Memory Engine__. 

![DolphinMemoryEngine_b17Gd1SZoZ](https://github.com/lance-o/bouncy_material/assets/61329703/ff17f5a7-2115-44c9-8217-85fb938b5f06)


Launch the Memory Viewer.

![DolphinMemoryEngine_GiEZDQ3UEZ](https://github.com/lance-o/bouncy_material/assets/61329703/4bb72940-0cdf-4a27-95dc-0e020db66f1b)

You're now looking at the real-time memory of Mario Kart: Double Dash!!. 

In the box named "Jump to an address", type the following:

> 8000523c

![DolphinMemoryEngine_QAfMJPrPIU](https://github.com/lance-o/bouncy_material/assets/61329703/4c939ab8-de5d-4cc8-aa32-05b066b58f73)


It should look like that. 

Remember how we left the 8-character hash at the end of your material name as "00000000", as it would instead
read from "somewhere in memory"?

The first 8 characters __Dolphin Memory Engine__ is that place in memory. You can now type in whatever you'd like into
those 8 characters, and then drive onto the track you assigned the Bounce Pad material to. It should bounce you in accordance to 
what you just put in _Dolphin Memory Engine__.

Now, you can change the direction it bounces you on the fly. When you find a setting you like, you may write it down 
(or, if Blender is still open, go and change the material immediately). 


https://github.com/lance-o/bouncy_material/assets/61329703/d18488a2-8889-447d-9de9-372b4b09f326


> NOTE: This example is from when the address used was different. Please only use the address "0x8000523C".

Now, for every new Bounce Pad material you add to the track, you may use the workflow of setting the ending 8 characters as 
"00000000" in __Blender__ to do this in __Dolphin Memory Engine__, and then enter the final setting into __Blender__ when you
have found one you like.

If your Custom Track is ready to be uploaded, please remember to add a "code_patches" line to your trackinfo.
It should look something like the following:

![notepad++_KSRMk6xYrF](https://github.com/lance-o/bouncy_material/assets/61329703/6a906b8d-3cb9-4be0-a027-8c5f83d331f5)




### tldr

For the game to use Bounce Pad, set up your material according to the following structure:

> Roadtype_0xB0XX_0x01_0xYYYYFFFF

with B0 being the Bounce Pad collision flag, XX being the sound flag you want your material to have, YYYY being the amount
of force to add upwards x100 in hex and FFFF being the amount of force to add forwards x100 in hex.

Since it's hard to gauge exactly how far it'll push you based on that information, leave YYYYFFFF as 00000000.
This will cause the patch to read from a place in memory instead.

Use __Dolphin Memory Engine's Memory Viewer__ and go to 0x8000523C. Treat the first 8 values the same way you
would the material name; you may experiment with different bounce settings in this way without having to recompile the game.


https://github.com/lance-o/bouncy_material/assets/61329703/43582a35-8024-4010-b193-ad1cc1b07d4b


If your Custom Track is ready to be uploaded, please remember to add a "code_patches" line to your trackinfo.
It should look something like the following:

![notepad++_KSRMk6xYrF](https://github.com/lance-o/bouncy_material/assets/61329703/72a512df-6baf-418a-a216-c26e4b849712)


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
