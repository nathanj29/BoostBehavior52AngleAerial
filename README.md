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
* Shows the "twirl" vs "flap" 50% pressure circle that dictates the jumping animation the wayfarer has in the air upon being in the flap pressure range: Avoid twirling for Flap boosting as you won't jump as high.
* Lets you select between 2 input modes, by clicking the coresponding button: "Game-accurate" or "Smooth".
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
* The jump animation transition (blue curve) would move up significantly and instead of showing a "twirl vs flap" transition, "twirl" would be replaced by either twirling or running normally, and "flap" would be replaced by a [variety of unique jumping animations](https://youtu.be/-50GmclgT5c) which change based on how the wayfarer was moving prior to the initial dive (standing still, walking, or sliding)... Being under this blue curve is only possible on solid ground, which offers a more stable contact.

Other situations for which these grahics would look very different include: **Holding the camera stick fully sideways**, being **frozen**, or being in **Paradise**.



## Stick pressure
### Recognized pressure points
_Journey_ recognizes only 90 fixed stick pressure values when outside the deadzone, for either axis. These pressure points are scaled from the raw stick input every 0.781% (or 0.776% for the last step). The corresponding game-processed pressure values however are not equally spaced with each other: They follow an odd pattern with gaps of either 0.98% or 1.47% between steps (or even 0,00000186% for the first step).

<details>
<summary>Click to view a chart listing every possible pressure values, raw and Journey-processed :</summary>
    
![chart pressure comparison](https://i.imgur.com/OyyFeJm.png) reupload updated version...
</details>

<details>
<summary>Click for details about how this chart was made :</summary>
    
Game-processed pressure points were simply written down one by one from observed values with Cheat Engine. It was confirmed the boost energy you can charge is also based on these 90 increments of possible pressure only.

Here is the formula that was used to find the raw stick input corresponding to each of these 90 steps:
    
![math formula](https://i.imgur.com/8WMHyAW.png)

In this formula, 29.690% corresponds to the first pressure input the game returns as a value different than 0.

(100% - 29.690% + 0.005%) is the full span of the recognized raw input range, with a correction factor for the shorter last step.

The accuracy of raw input pressure range vs game-pressure was confirmed via testing upon using the [HardwareTester website](https://hardwaretester.com/gamepad) on several browsers, and comparing with Cheat Engine pressure values.
</details>



### Clamping / Diagonal deviation

Clamping of 90 values of smooth mode different

    Upon using full stick pressure they indicate **boost energy and speed meter indicator** (for aerial dives and dropshoots). sideways ds for speedruns...
    upon applying a **full stick pressure**, max boost energy colors also perfectly correlate to in-game **horizontal speed** ! (lower boost energy = more speed)



Square mode (or standard mode) is the mode based on which most controllers rely on for stick inputs: the sticks move in the shape of a circle physically (restrained by the top cover of the controller), but the signal they transmit is in the shape of a square. This signal ranges between values of -1 to 1 for the vertical (Y) and horizontal (X) axis ; each measured independently.

When pressing the stick fully up, down, right, or left, the resulting signal has a maximal magnitude of 1. For diagonals however the combined pressure of both axis can often translate into a maximal magnitude exceeding 1 ! (More or less, depending on the controller itself, the controller brand, and how worn out the sticks you are using are.)
If you combine the maximal magnitude reached for every directions into a graphic, it should take the shape of a square with rounded corners ; which you can test for your controller on [this website](https://hardwaretester.com/gamepad) by clicking "Test circularity".

![circularity test on my controller](https://i.imgur.com/C4HxBP7.png)

This image above shows the circularity of my left and right sticks (xbox series controller): Notice how the top left and top right parts of my left stick have a stronger deviation due to the plastic of the stick being worn out from overuse.

This mechanic of having stronger diagonal input possibilities is called the « Average error » (or circularity error/radial deviation): it is completely normal to have it to some degree (8 to 14% deviation is common for most controller brands[^1]). Having some deviation ensures the stick can reach a magnitude of 1 for every directions.

[^1]: [Source](https://www.cowcotland.com/articles/4639-10/marre-des-manettes-pourries-comment-bien-choisir-sa-manette-partie-1-les-joysticks.html).

### Journey's cross-shaped deadzone consequences
When a game processes raw stick inputs, it often ensures inputs with a magnitude bigger than 1 are corrected by clamping their pressure value back over the circumference of a circle with a magnitude of 1. This helps preventing too strong inputs that would otherwise lead to faster diagonal movements or unusual camera speed.

_Journey_ with its stick input processing logic however completely bypasses the ridiculously huge horizontal and vertical axis deadzones (thus starting at 29.690% pressure).
This leads to the "magnitude = 1" representation no longer lying on the circumference of a circle, but instead on **a bigger square with rounded corners**, where the arching starts at the edges of the horizontal and vertical axis deadzones. This shape corresponds to the **gold dashed line** you can view on the following image, and **it represents a magnitude of 1 for every input directions based on _Journey_'s mapping** (or to put it simply: the expected range of motion to reach for _Journey_) ! It's on this line that _Journey_ clamps back too strong left stick inputs.

![clamping explained](https://i.imgur.com/Dzn3XNZ.png)

    left stick only sur graphic

The problem with this bigger "magnitude = 1" shape is that even with some stick radial deviation, **most controller brands won't be able to reach it fully** in every directions ! (At least not without a custom re-calibration of pressure sensitivity, or worn out sticks.) This leads to the wayfarer most likely **not walking as fast if you hold the left stick in diagonals**, which can also translates into lower speed when Fancy Flying not completely on the vertical axis of the left stick...

This can result in some controllers being more advantageous than others for _Journey_, if their stick radial deviation is bigger.

### Clamping explained

magic formula under spoilers



### Input mode
2 input modes are available to display the _Journey_-processed left stick pressure: Game-accurate and Smooth.

The **game-accurate** mode only snaps the right stick over the 90 recognized stick pressure positions. It perfectly translates how _Journey_ interprets the raw stick input.

The **smooth** mode consists in a linear interpolation from the raw stick input after ignoring the vertical or horizontal axis deadzones. This mode may be visually more pleasing upon moving the left stick, but it shows less precision in regards to the true stick coordinates in-game.

clamped values ...







## Known limitations
* The right stick position updates more frequently when the tool runs at a higher framerate. **Expect it to sometimes fail showing you successfully flicked when using it at 30 FPS**: it is possible the right stick moves out of the deadzone only in-between 2 frames and thus doesn't reset the 3 seconds countdown, even though it technically did in the game itself. This issue happens only for extremely quick flicks that barely move out of the deadzone, and they are more rare at 60 fps.
* Colored flash timing and trail length were adjusted based on 30, 60 and 120 FPS values. If your monitor has a super high refresh rate, expect a trail longer than usual or flashes occuring a little earlier than designed for.
* If you open the tool on a browser that runs it at an inconsistent framerate, it is possible the tool will fail to reset the 3 seconds timer from time to time: e.g. if the framerate suddenly drops below 61 FPS, or below 31 FPS.



## Credits
ChatGPT and ClaudeAI for helping to code everything.

This [french article](https://www.cowcotland.com/articles/4639/marre-des-manettes-pourries-comment-bien-choisir-sa-manette-partie-1-les-joysticks.html) for explaining a lot about how stick input works, and how games handle them. It also helped making the clamping mechanism of the tool work perfectly as intended.
