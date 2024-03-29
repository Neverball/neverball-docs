<!--

NOTE. This documentation is an extended version of entities.ent in the
NeverballPack. Please keep them both synchronized where relevant.

-->
# Entities

While some of Neverball entities use entity names derived from Quake, this is only of historic relevance. All of these entities behave differently in some way. All unsupported entities and unsupported attributes of supported entities are ignored. The Neverball entities follow. All applicable entity attributes are documented here.

## info_player_start

The *info_player_start* entity defines a ball. While multiple balls may be defined, Neverball uses only the first of them. Neverputt requires an *info_player_start* entity for each player slot, plus one.

Key       | Description
----------|------------
radius    | The ball radius in meters. The default radius is "0.25". In Neverputt, this should be set to "0.0625".

Note that the "angle" attribute does NOT determine the initial facing direction. The player begins each level looking down the Y axis. If your level begins wrong, you must rotate your map.

## info_player_deathmatch

The *info_player_deathmatch* entity defines a goal.

Key    | Description
-------|------------
radius | Goal radius in meters. The default radius is "0.75". In Neverputt, this is usually set to "0.1375".

## light

The *light* entity defines a coin.

Key    | Description
-------|------------
light  | Value of the coin. Neverball draws coins in denominations of 1, 5, and 10.

Consider the radius of your ball and place coins within reach from the floor.

## item_clock

The *item_clock* entity defines a "clock" item.

Key    | Description
-------|------------
light  | Value of the clock. Neverball draws clocks in denominations of 5, 15, and 30. All units are in seconds.

## item_health_small

The *item_health_small* entity defines a "shrink" item.

## item_health_large

The *item_health_large* entity defines a "grow" item.

## path_corner

The *path_corner* entity defines a path segment of a moving object.

Key        | Description
-----------|------------
targetname | Name by which other entities refer to this entity. The default name generated by Radiant is usually fine, but you will want to set it yourself as your maps become more complex.
target     | Refers to another *path_corner* defining the destination of the path. This attribute may be automatically specified using Radiant's "Connect Entities" feature. To make a path from point A to point B, first select *path_corner* A, then select *path_corner* B, then "Connect Entities" (Control-K).
speed      | Duration of the trip along this path segment, in seconds. The default is "1.0". The precision of this value is limited to milliseconds (three digits after the decimal point).
state      | Determines whether the path is enabled. An object will only move along an enabled path. An object may be stopped and started by toggling the state of the path to which it is attached. "0" means no, "1" means yes. The default is "1".
smooth     | Determines whether an object moves smoothly along this path segment or at a constant speed. "0" means no, "1" means yes. The default is "1".
angles     | Defines the final orientation of the object at this *path_corner*. The values are angles giving pitch (up/down), yaw (left/right), and roll. Use Radiant's rotation tool to set the general orientation, and then adjust the values manually.  As an object travels between *path_corners*, it rotates through the orientations of those *path_corners*.
angle      | Same as setting "angles" to "0 &lt;angle&gt; 0". Radiant uses and recognizes both.

To create a pause in a path, simply position two connected *path_corner* entities at the same point. The trip will have zero length but the object will still spend "speed" seconds making the journey.

Note that the "speed" attribute has a different meaning to Neverball than it does to Quake. In Quake, it gives the speed in units per second. In Neverball it gives the travel time in seconds, regardless of the distance between one point and the next.

## func_train

The *func_train* entity defines the geometry of a moving object. Think of it as a container or a grouping mechanism for lumps, rather than an object in-and-of itself.

Key     | Description
--------|------------
target  | Refers to the first *path_corner* of the path along which the object will travel.
target2 | Refers to a second path that controls the orientation of the object. This is useful when you want to control the position and orientation independently of each other.

To create a *func_train*, first select all of the brushes that form the moving object, then select *func_train* from the entities pop-up (right click) menu.

To assign a *func_train* to a path, first select the *func_train*, then select the first *path_corner* entity of the path, then "Connect Entities". Multiple *func_trains* may be assigned to one path.

To destroy a *func_train* leaving only the original brushes, select the *func_train*, and choose "Ungroup Entities" from the "Selection" menu.

Note that *func_trains* are positioned differently in Neverball than in Quake. Quake ignores the placement of the *func_train* in space and requires an origin specification. Neverball simply uses the location of the first *path_corner* to define the origin. When creating a moving object, just place it at the beginning of its path and everything should work out.

*func_train* is a [body](#bodies).

## target_teleporter

The *target_teleporter* entity defines a teleporter.

Key    | Description
-------|------------
radius | Teleporter radius. The default is "0.5".
target | Refers to a *target_position* entity defining the destination of the teleporter.

Unlike the goal entity, the center of the editor's entity box defines the origin. So to define a teleporter flush with the floor, embed the entity box halfway in the floor.

## info_camp

The *info_camp* entity defines a switch. A switch's behavior is similar to a teleporter.

Key       | Description
----------|------------
radius    | Switch radius. The default is "0.5".
target    | Refers to the *path_corner* that the switch controls.
state     | Intial state of the switch. "0" is off (default), "1" is on. This parallels the "state" attribute of the *path_corner*. An *info_camp* entity should always have the same initial "state" value as the *path_corner* it targets.
timer     | A delay time. The time begins when the switch is toggled to its non-initial state. The switch toggles back to its initial state when the timer expires. A timer value of zero (default) indicates an untimed switch. This may be used to define a door that opens only for a moment before closing, or a *func_train* that moves along its path in discrete activated steps. The precision of this value is limited to milliseconds (three digits after the decimal point).
invisible | Defines an invisible switch. "0" is off (default), "1" is on.

## info_player_intermission

The *info_player_intermission* entity defines the camera position at the beginning of a level fly-in.

Key    | Description
-------|------------
target | Refers to the *target_position* defining the view direction.

NOTE: A Neverputt map should contain two *info_player_intermission* entities. The first one defines the fly-in when a level begins. The second is only used for the fly-by on the main menu. The same *target_position* may be used for both, but it's better if each has its own. In general, the first *info_player_intermission* should show a broad overview of the entire map. The second one should show some interesting aspect of the map in detail. The order in which these two *info_player_intermission* entities appear in the entities list (and consequently in the .map file) determines which is used for the fly-in.

## target_position

The *target_position* entity defines the initial view direction of a level fly-in, as well as the destination of a teleporter.

Key        | Description
-----------|------------
targetname | Name by which other entities refer to this entity.

To define a two-way pair of teleporters, place the *target_position* of each coincident with the *target_teleporter* of the other.

## misc_model

The *misc_model* entity imports an arbitrary polygonal model into a level. It may be used to increase the visual detail of a level without increasing the physical detail. Arbitrary normal vectors are allowed, so the *misc_model* entity can be used to produce smoothly shaded curves.

*misc_model* entities define visible geometry, but not physical geometry. So, if the ball is to bounce off of a *misc_model* entity, the entity should be placed within one or more invisible structural lumps.

Key   | Description
------|------------
model | Filename of the model relative to the data directory.

The model must be in OBJ format. It must have triangular tesselation. All vertices must have normals and texture coordinates. A 3D modeller such as Blender or Wings3D may be used to create and export OBJ models.

See the note on [scale and coordinate systems][scale].

*misc_model* is a [body](#bodies).

## worldspawn

The *worldspawn* entity defines most static level geometry and fully defines a level through its attributes.

Key     | Description
--------|------------
message | Intro text that appears as a level begins. A "\" (backslash) character starts a new line. Limited space is available. Wrapping text within the intro text box is often a process of trial and error.
back    | Path to the background file.
grad    | Path to the background gradient image. Also valid in Neverputt.
song    | Path to the background music file. Also valid in Neverputt.
shot    | Path to the level shot file.
goal    | Number of coins required to unlock the goal.
time    | Level time limit in hundredths of a second.
time_hs | Default values for Best Times highscore, in order: Hard, Medium, and Easy (optional, defaults to the time limit).
goal_hs | Default values for Fast Unlock highscore, in order: Hard, Medium, and Easy (optional, defaults to the time limit).
coin_hs | Default values for Most Coins highscore, in order: Hard, Medium, and Easy (optional, defaults to required coins).
version | Level version. It is specified as "X.Y", where X is incremented every time the level is changed in a way that breaks existing replays, and Y is incremented for all other changes.
author  | Author's name
bonus   | Marks the level as a bonus level.
idle    | Neverputt: Time to wait after the ball has stopped before starting the next shot. This is useful if you have moving objects that may hit the ball while the player is making the shot.
par     | Neverputt: The number of strokes required to complete the hole.

For portability, all filenames should use the "/" (forward slash) character as the directory separator.

*worldspawn* is a [body](#bodies).

## info_null

The *info_null* entity defines an animated billboard. Billboards are fundamental in the creation of backgrounds and ball skins, and are used to add visual details to a level. A description of *info_null* is available in the [background documentation](Backgrounds.md).

## Bodies

Internally the *misc_model*, *func_train* and *worldspawn* entities are represented in the same way and are called "bodies".

A body can follow a path like a *func_train* and can have a detail model attached to it like a *misc_model*.

A body that imports a detail model can have a "material" attribute set on it. A detail model that does not define its own materials will use the material given in this attribute. For example, a number of .map files that define ball geometry import the generic basic-ball.obj model and each sets a different material to import it with.

## Scale and coordinate systems
[scale]: #scale-and-coordinate-systems

Neverball uses the standard OpenGL coordinate system: X points left, Y points up, and Z points backwards / towards the viewer. Radiant uses a different coordinate system: X points left, Y points forward and Z points up.

Additionally, 64 Radiant units correspond to 1 Neverball unit (conventionally called a meter).

*mapc* converts between these coordinate systems and units, so normally you don't have to think about this, with one exception: OBJ models use Neverball units and coordinate system. For example, *zrail2.obj* is a 2 units long railing along the Z axis in Neverball. However, when you import the model as a *misc_model* in Radiant, it will be displayed with the wrong scale and orientation. This makes it hard to work with, so here's a workaround: in Radiant, set "modelscale" to "64" and "angles" to "0 0 90" on the *misc_model* entity. *mapc* will ignore these, while Radiant can use them to display the model properly.
