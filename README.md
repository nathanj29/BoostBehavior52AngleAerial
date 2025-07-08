# Journey left stick input mapping and boost energy behavior for a 52° angle aerial dive
Tool made to help understand **how Journey computes raw stick input**, and visualize how it translates in-game.

Its second main goal is helping **getting a better understanding of left stick position in regards to boost energy behavior and charging time required** upon Fancy Flying. Maybe it can even help training **Flap Boost** !

Tracks the left stick movements (both raw and Journey-processed), toggles between Slow and Flap pressure range, shows the maximum boost energy corresponding to each pressure points.

Visit [this link](https://nathanj29.github.io/BoostBehavior52AngleAerial/) to use the tool.



## Features of the tool
* Displays 2 graphics: raw left stick input vs _Journey_ processed input.
* Shows the 3 left stick **deadzone types**, upon loading the tool.
* Tracks the left stick inputs by using the browser API.
* **Shows the left stick coordinates** in the bottom right corner of each graphic.
* Displays the maximum boost energy you can charge for every stick positions.
* The legend compares maximum boost energy and charging time required to reach it.
* Toggles between Flap and Slow pressure range (Also includes the "full" stick pressure).
* Shows the "twirl" vs "flap" 50% pressure circle that dictates the jumping animation the wayfarer has in the air : Avoid twirling for Flap boosting as you won't jump as high.
* Lets you select between 2 input modes: "Game-accurate" or "Smooth"
* Measures the framerate the tool is running at to better adjust the left stick trail length.
* You can **zoom in and out** by resizing the browser window, doing ctrl + mouse wheel, or doing ctrl and the + or - key.



## How to use the Fancy Flicker?
Just open the link in any browser and move the left stick.

Alternatively, add the tool to OBS:



## Adding the tool to OBS
It can be added as a browser source to stream/record your practice session, or just play from the preview window with it.

Here is the link to add https://nathanj29.github.io/FancyFlicker/

Dimensions of the source: 2455 (width) x 1130 (height). Use ctrl+d or ctrl+f to center and fit to your preview window, you can of course also crop out the left graphic fully if you only want the right one.



## Why so specific? (52° angle, only in the air)

    image right side slow à coté d'une 2eme flap pressure
![useless image description](https://i.imgur.com/hF563yj.png)

The max boost energy graphics above are designed for when the **camera aims fully down (52° angle)**, and for **aerial dives** only:

**Flatter camera angles** don't allow diving with such strong sideways left stick pressure, they permit weaker max boost values, and have different slow or flap pressure range thresholds (white curves).

If diving on the ground instead (**Dropshoot**):
* Max boost energy colors would look very similar, if not the same. 
* Slow and flap pressure range thresholds would be very slightly different in their shape.
* Lighter flap pressure becomes impossible to apply without lifting off the ground, especially on sand as opposed to solid ground.
* The jump animation transition (blue curve) would move up significantly and instead of showing a "twirl vs flap" transition, "twirl" would be replaced by either twirling or running normally, and "flap" would be replaced by a [variety of unique jumping animations](https://youtu.be/-50GmclgT5c) which change based on how the wayfarer was moving prior to the initial dive (standing still, walking, or sliding)... Being under this blue curve is only possible on solid ground which offers a more stable contact.

Note **holding the camera stick fully sideways** also greatly impacts the way these graphics look. Being **frozen** does too... Or being in **Paradise**...



## fgjlkdjgkd
![chart pressure comparison](https://i.imgur.com/OyyFeJm.png)

_Journey_ recognizes 90 possible stick pressure points (outside of 0% in-game pressure, measured whenever the stick is in the deadzone). These pressure points are scaled from the raw stick input used for this tool: each are spaced by 0.781% with one another, but they don't recessarily return in-game pressure spaced equally ! The gold lines correspond to **the minimal vertical pressure range the game acknowledges**, which return an in-game camera pressure of 0,00000186%.
The true stick pressure to apply for this ranges from between 29.690% to 30.470%.



Smooth vs game accurate
smooth = linear interpolation of stick pressure from the raw stick input after ignoring the vertical or horizontal axis deadzones. This mode may be visually more pleasing but has less precision.



## Speed limitations
### Diagonal deviation

Upon using full stick pressure they indicate **boost energy and speed meter indicator** (for aerial dives and dropshoots). sideways ds for speedruns...
upon applying a **full stick pressure**, max boost energy colors also perfectly correlate to in-game **horizontal speed** ! (lower boost energy = more speed)

Square mode (or standard mode) is the mode based on which most controllers rely on for sticks inputs: the sticks move in the shape of a circle physically, but the signal they transmit is in the shape of a square, with pressure ranging from between -1 to 1 for the vertical (Y) and horizontal (X) axis (each measured independently).

The resulting signal received has a maximal magnitude of 1, if purely on the horizontal or vertical axis. For diagonals however the combined pressure of both axis physically contained inside of a circle often translates into a maximal magnitude reaching beyond 1 ! (More or less, depending on the controller itself, the controller brand, and how worn out the sticks you are using are.)
The combined maximal magnitudes for every directions therefore take the shape of a square with rounded corners ; which you can test for your controller on [this website](https://hardwaretester.com/gamepad) by clicking "Test circularity".

![circularity test on my controller](https://i.imgur.com/C4HxBP7.png)

This image above shows the circularity of my left and right sticks (xbox series controller): Notice how the top left and top right parts of my left stick have a stronger deviation due to the plastic of the stick being worn out from overuse...

This mechanic of having stronger diagonal input possibilities is called the « Average error » (or circularity error/radial deviation): it is completely normal to have it to some degree (8 to 14% deviation is typical for most controller brands).

### Journey's cross-shaped deadzone consequences
When a game processes raw stick inputs, it often ensures inputs with a magnitude bigger than 1 are corrected by clamping their pressure value back over the circumference of a circle with a magnitude of 1. This helps preventing too strong inputs that would otherwise lead to faster diagonal movements or unusual camera speed. Note having some degree of radial deviation is what ensures you can always reach this magnitude of 1 for every input directions !

On PC, _Journey_ with its stick input processing logic however completely bypasses the ridiculously huge horizontal and vertical axis deadzones ; acting as if a game-processed pressure of 0% started at 29.689% raw stick input, for either axis.
This leads to the "magnitude = 1" representation no longer lying on the circumference of a circle, but instead on **an extended square with rounded corners**, where the arching starts at the edges of the horizontal and vertical axis deadzones: This shape corresponds to the **gold dashed line** you can view on the following image, and **it corresponds to a magnitude of 1 for every input directions based on _Journey_'s mapping** (or to put it simply: the expected range of motion to reach for _Journey_) !

![clamping explained](https://i.imgur.com/Dzn3XNZ.png)

This gold dashed line shape is a problem for diagonal inputs because even with some stick radial deviation, most controller brands won't be able to reach it fully in every directions ! (At least not without a custom re-calibration of pressure sensitivity, or worn out sticks.) For the camera this could lead to a speed issue, however for some reason on PC **_Journey_ doesn't even apply clamping at all to the right stick's pressure positions with a magnitude exceeding 1 !** (but the left stick does apply it correctly.) This means playing with a right stick that has a really strong radial deviation involves it being possible to move the camera abnormally fast (yet nowhere near as fast as with a mouse of course).

For most controllers it should remain possible to maintain 100% sideways pressure on the camera while also moving it a little up or down in-game (just peaking out of the vertical axis deadzone) ! If you were to succeed in giving a raw input of (X=1, Y=1), the right stick would give a camera speed corresponding to the maximal magnitude of 1.41 ! The camera would then be moving at 1.41 times its theoretical max speed. This can result in some controllers being more advantageous than others for _Journey_.

I have a theory clamping was removed by the PC porting team solely for the right stick, to allow for quicker mouse movements...

### PlayStation right stick clamping?
It is highly probable the reason why PlayStation versions of _Journey_ have slow camera speed in diagonals is due to clamping working (almost) as intended. If you were to move the camera fully sideways and then making it move slightly up or down in-game you would instantly notice it slowing down in speed significantly however ! The reason I believe to be behind this is that _Journey_ doesn't apply clamping to the right stick the way it's supposed to: instead of clamping pressure values over the gold dotted line from the image above, it instead clamps these right stick values over the shape of a perfect circle (acting as if the right stick horizontal and vertical axis deadzones didn't exist, even though they do)! A circular clamping pattern can't account for the deadzones of _Journey_, which means it also clamps down pressure values with a magnitude inferior to 1! Hence why diagonal right stick inputs aren't convenient to use at all on PlayStation... Right stick camera control was added late during development of the game (it used to be purely Sixaxis at first), so it is hard to know whether this slower diagonal camera speed could be a mistake or intended.

Keep in mind that for PlayStation, instead of using diagonal right stick inputs you can combine the use of the right stick (purely on the X or Y axis), with Sixaxis on the opposite axis at the same time (by moving the controller) ! This allows to reach speeds than can even exceed PC controller camera speed.



## Known limitations
* The right stick position updates more frequently when the tool runs at a higher framerate. **Expect it to sometimes fail showing you successfully flicked when using it at 30 FPS**: it is possible the right stick moves out of the deadzone only in-between 2 frames and thus doesn't reset the 3 seconds countdown, even though it technically did in the game itself. This issue happens only for extremely quick flicks that barely move out of the deadzone, and they are more rare at 60 fps.
* Colored flash timing and trail length were adjusted based on 30, 60 and 120 FPS values. If your monitor has a super high refresh rate, expect a trail longer than usual or flashes occuring a little earlier than designed for.
* If you open the tool on a browser that runs it at an inconsistent framerate, it is possible the tool will fail to reset the 3 seconds timer from time to time: e.g. if the framerate suddenly drops below 61 FPS, or below 31 FPS.



## Credits
ChatGPT and ClaudeAI for helping to code everything.

This [french article](https://www.cowcotland.com/articles/4639/marre-des-manettes-pourries-comment-bien-choisir-sa-manette-partie-1-les-joysticks.html) for explaining a lot about how stick input works, and how games handle them. It also helped making the clamping mechanic of the tool work perfectly as intended.
