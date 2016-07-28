JS-HOM
======

Something approximating Vanilla Doom's Hall-of-Mirrors shimmer in JavaScript

by [Scott Smitelli](mailto:scott@smitelli.com)

What?
-----

This is a thing I threw together to try to understand exactly how the "Hall of
Mirrors" shimmering effect arises in the original DOS version of Doom.

Check it out at [https://rawgit.com/smitelli/js-hom/master/js-hom.html](https://rawgit.com/smitelli/js-hom/master/js-hom.html).

Things Learned
--------------

Doom caps its frame rate at 35 FPS. There are three screen buffers.

If the chainsaw is in view, the swing no longer runs at 35 FPS and instead drops
to the "idle" sprite animation speed. Possibly an implementation oversight.

The weapon swing roughly draws the lower arc of a circle, 32 pixels in diameter.
The left-to-right sweep places the sprite in the exact same spots as the
right-to-left sweep, except for the very bottom point of the circle (the X
coordinate for this point varies by 1 depending on the direction; seems to be
round-off error.) One complete swing cycle is 64 frames long, which does not
divide evenly into the three screen buffers.

The "angle" in the cycle is dictated by the game clock. Beginning to move the
player at different times will jump into the animation cycle at different
angles. The magnitude of movement is controlled by the player's bob value, which
is based on player velocity and affects the view's height from the floor as
well.

The original swing code appears to be in
[p_pspr.c, in A_WeaponReady()](https://github.com/id-Software/DOOM/blob/77735c3ff0772609e9c8d29e3ce2ab42ff54d20b/linuxdoom-1.10/p_pspr.c#L275).
There is also a function P_CalcSwing() in that file that's similar, but looks to
be unused.

The DOM was a poor choice.
