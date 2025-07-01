# Journey left stick input mapping and boost energy behavior for a 52° camera angle aerial dive
Tool made to help understand **how Journey computes raw stick input**, and visualize how it translates in-game.

Its second main goal is helping **getting a better understanding of left stick position in regards to boost energy behavior and charging time** upon Fancy Flying.

Tracks the left stick movements (both raw and Journey-processed), toggles between Slow and Flap pressure range, shows the maximum boost energy corresponding to each pressure points.

Visit [this link](https://nathanj29.github.io/BoostBehavior52AngleAerial/) to use the tool.




Upon applying a **full stick pressure**, the boost energy colors also perfectly translate to in-game **horizontal speed** !





## Features of the Fancy Flicker
* Shows the 3 right stick **deadzone types**, upon loading the tool.
* Tracks the right stick inputs by using the browser API.
* **Shows the right stick coordinates** in the bottom left corner.
* Displays a **3 second countdown** that resets to 3 by exiting the vertical axis deadzone (green color), and starts counting down whenever the right stick goes back in it.
* Shows a **meter bar** on the right side that represents these 3 seconds.
* The outer square edges get **colored flashes** when there is 1.5 and 0.5 seconds left on the countdown. The first one indicates half the countdown is gone, and the second one says that it is time to consider flicking.
* Measures the framerate the tool is running at to better adjust the right stick trail length and give more precision to the colored flashes.
* **Cyan expanding circles** appear at the peak position reached by the right stick upon flicking. Look at them to know how well you perform.
* Possibility of practicing flicking with this tool even without playing the game !
* Indicates when you apply the minimal pressure recognized in-game (see "Golden lines" section)
* You can **zoom in and out** by resizing the browser window, doing ctrl + mouse wheel, or doing ctrl and the + or - key.

## How to use the Fancy Flicker?
Just open the link in any browser (some offer better framerate stability than others).

Move the right stick out of the vertical axis deadzone (green area) to setup the countdown/meter bar and start flicking.

The measured framerate stabilizes after 130 frames are loaded (about 4 seconds at 30 FPS, 2 seconds at 60 FPS)

Alternatively, add the tool to OBS:

## Adding the tool to OBS
It can be added as a browser source to stream/record your practice session, or just play from the preview window with it.

Here is the link to add https://nathanj29.github.io/FancyFlicker/

Dimensions of the source: 2455 (width) x 1130 (height). Use ctrl+d or ctrl+f to center and fit to your preview window, you can of course also crop the left graphic fully if you only want the right one.






## How was this tool made ?


## Why so specific? (52° angle, only in the air)

This boost graphic is designed for when the **camera aims fully down (52° angle)**, and for aerial dives only.

As the camera flattens in general, you won't be able to dive with a strong horizontal pressure, forcing you to stay closer to the vertical axis.
The slow or flap pressure range thresholds will also shift based on the camera angle, thus explaining why for a Slow Level Boost or Infinite Boost we apply a little more left stick pressure as the camera aims more down.
The maximum boost energy you can charge will also be reduced with flatter camera angles.

As a comparison, for Dropshoot-based techniques the overall boost graphic would become more complex:
* Boost energy colors would look very similar overall, if not the same. This means upon using full stick pressure with a 52° angle Dropshoot, this tool would still be very accurate !
* Slow and flap pressure range thresholds (white lines) would be very slightly different in their shape.
* Lighter flap pressure becomes impossible to apply without lifting off the ground, especially on sand as opposed to solid ground.
* The jump animation transition (blue line) moves up significantly and instead of showing a "twirl vs flap" transition, "twirl" is replaced by either twirling or running normally, and "flap" is replaced by a [variety of unique jumping animations](https://youtu.be/-50GmclgT5c) which change based on how the wayfarer was moving prior to the initial dive (standing still, walking, or sliding)... Being under this blue curve is only possible on solid ground which offers a more stable contact.






![useless image description](https://i.imgur.com/hF563yj.png)












## Camera speed limitations
Depending on your controller, or how worn out the sticks are it may be possible that upon using this tool you can move the right stick out of the **dotted line, which represents a mapping of the expected range of motion** _Journey_ considers for the sticks.
This dotted line becomes a circle when not considering the vertical and horizontal axis deadzones.

Usually games will force too strong stick inputs back onto this dotted line, which is adjusted based on the size of the game's horizontal and vertical axis deadzones. Quite often the deadzones aren't nearly as huge as the ones _Journey_ applies, which explains why _Journey_ may be expecting slightly too big stick range of motion for your controller.

_Journey_ for whatever reason only applies clamping to the left stick, for pressure values exceeding this boundary. This means with the right stick it is possible on most controllers to maintain 100% sideways pressure on the camera while also moving it a little up or down in-game ! With a stick worn out enough it also becomes possible to obtain faster than normal camera movements in diagonals ! A brand new stick on the other hand probably won't let you get much close to this dotted line threshold when applying diagonal pressure ; resulting in slightly lower camera speed. Technically, playing without the plastic cover top of your controller should allow for even faster diagonal camera movements in theory (I haven't tried it yet, but maybe you could even expect having 100% vertical and horizontal camera speed at the same time !).



## Golden lines
2 golden lines are displayed on the tool. Entering a golden line or moving past it with the stick and reaching the blue colored background is considered being outside the vertical axis deadzone.

_Journey_ recognizes only 90 possible stick pressure points (outside of 0% pressure, whenever the stick is in the deadzone). These pressure points are scaled from the raw stick input used for this tool. The golden lines correspond to **the minimal vertical pressure range the game acknowledges**, which return an in-game camera pressure of 0,00000186%.
The true stick pressure to apply for this ranges from between 29.690% to 30.470%.

The in-game pressure value returned when the stick is in a golden line is so low that if applied **the camera won't move at all in-game**. Yet _Journey_ still considers it to be a pressure different than 0%. As a result, if you manage to maintain this pressure for prolonged periods of time the camera will never flatten. This can be especially useful for slow circle dropshoot. This pressure range is so precise however that it can't be a reliable option to count on it working every time from just muscle memory!



## Known limitations
* The right stick position updates more frequently when the tool runs at a higher framerate. **Expect it to sometimes fail showing you successfully flicked when using it at 30 FPS**: it is possible the right stick moves out of the deadzone only in-between 2 frames and thus doesn't reset the 3 seconds countdown, even though it technically did in the game itself. This issue happens only for extremely quick flicks that barely move out of the deadzone, and they are more rare at 60 fps.
* Colored flash timing and trail length were adjusted based on 30, 60 and 120 FPS values. If your monitor has a super high refresh rate, expect a trail longer than usual or flashes occuring a little earlier than designed for.
* If you open the tool on a browser that runs it at an inconsistent framerate, it is possible the tool will fail to reset the 3 seconds timer from time to time: e.g. if the framerate suddenly drops below 61 FPS, or below 31 FPS.



## Credits
Loggivan for discovering the 3 seconds countdown existance and that vertical pressure (not horizontal like we thought !) is tied to making flicking work !

ChatGPT and ClaudeAI for helping to code everything.
