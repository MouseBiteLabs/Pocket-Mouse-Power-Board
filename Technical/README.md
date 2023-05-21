# Technical Design Document

This write-up serves as a technical explainer as to how the Pocket Mouse Power (PMP) Board operates. The board takes the shape of the original Game Boy Color (CGB) power board, and should also serve as a Game Boy Pocket (MGB) replacement. As mentioned in the main file, the board only supplies 5 V output - therefore, original LCD screens are unsupported. You must have a modded Game Boy with an IPS kit in order to use this board properly, as many of those do not require the use of the LCD supplies. (Some MGB screen kits utilize the -18 V supply, though, so your mileage may vary)

## Schematic and Board Interfacing

The Pocket Mouse has seven pins, similar to the Game Boy Color power board. In the below schematic, the pins are laid out similar to how they appear on the board - three on the left, four on the right. Some of these pins serve slightly different functions, so I will compare them here:

- Pin 1 is the input voltage (from the batteries or DC jack, labelled "SW" in the schematic) *after* the power switch - when the switch is off, pin 1 sees no voltage. This retains the same function as the original power board for the CGB and MGB.
- Pin 2 is permanently connected to VCC, which is the input voltage *before* the power switch. This differs from the CGB and MGB, where this pin was not connected to anything (or completely missing).
- Pin 3 is GND, same as CGB and MGB.
- Pin 4 is also GND, same as the CGB. This pin did not appear on the MGB power board.
- Pin 5 is not used. This was the 13.6 V rail for the positive supply on the LCD screen of the CGB. This was GND on the MGB (and was called pin 4).
- Pin 6 is also not used. On a CGB, this pin is supposed to be the -15 V rail for the negative LCD supply. This was -18 V (VEE) for the MGB's LCD screen (and called pin 5).
- Pin 7 is the 5 V output, same as CGB and MGB.

Here's the schematic in full:

![image](https://github.com/MouseBiteLabs/Pocket-Mouse-Power-Board/assets/97127539/80f18c17-f6ff-4180-b920-6cb04e39f9a0)

## Boost Converter (U1)

Like many other aftermarket reuglator boards for Game Boys, the PMP uses the TPS61202 boost converter. <a href="https://www.ti.com/lit/ds/symlink/tps61202.pdf?ts=1680457966449&ref_url=https%253A%252F%252Fwww.ti.com%252Fproduct%252FTPS61202">The datasheet for this chip</a> provides many sample circuit layouts, all of which are considerably simple to follow as the chip takes care of most of the complicated aspects, leaving the user to only need to pick a handful of discrete parts. In my case, this pretty much means just the input and output capacitors (C3 and C5, respectively) and the boost inductor L1; the aux capacitor, C4, is simply assigned at 0.1 uF. 

The boost converter is enabled when the voltage on the EN pin is above 400 mV. The voltage divider made by R5 and R8 (supplied by the output of the power switch) means the voltage is ~91% of the input voltage to the system if the switch is on, or zero if the switch is off or the undervoltage lockout is latched by U2. The undervoltage lockout is explained in the next subheadings.

The boost inductor L1 is recommended to be at minimum Vin * 0.5 (us/A), but more generally just 2.2 uH as a "best performance" value. Inductance loss due to saturation should be taken into account - the maximum current the power board should output is less than 250 mA, but the inductor I chose saturates at ~1.2 A, which should be plenty of headroom.

The output capacitor is recommended to be at least five times the value of L1 (in uH), so in this case, greater than ~11 uF. There is no upper limit to the output capacitance. 22 uF seems like a good fit. The input capacitor should be large enough to supply the boost converter with enough power during transients. Any value above 4.7 uF is recommended. Since I'm already using a 22 uF for the output, might as well use that for the input as well.

Ceramic capacitors experience a drop in capacitance as the DC voltage bias across them increases, so it's always good to have headroom. X5R or X7R capacitors are recommended due to their temperature stability. The graph below is an example of the drop in capacitance ceramic capacitors can experience with respect to the applied voltage across them. Therefore, the input and output capacitors I have chosen are at least 5x their rated value to keep capacitance losses below 20%.

<p align="center"><img width="668" height="356" src="https://user-images.githubusercontent.com/97127539/229528007-aba53dc8-b9ca-4f0b-8d48-cba03f65231a.png"></p>

<i>Source: <a href="https://www.omicron-lab.com/fileadmin/assets/Bode_100/ApplicationNotes/DC_Biased/App_Note_DC_Bias_Impedance_Caps_V2_0.pdf">DC Biased Impedance Measurements Capacitors</a></i>

## Undervoltage Detection (U2)

U2 is a TPS3840 chip which monitors the voltage on the batteries via the VDD pin. When it detects the voltage has gone below a specific threshold, the /RESET pin is pulled low. This output is open-drain, so it does not affect the PSU_EN net while not in an error state. The ultimate effect of this chip is it will disable the boost converter if the battery voltage drops too low; this is to 1) protect the batteries from overdischarging and 2) to prevent extremely annoying bootloops when the batteries die.

The specific part I chose was the TPS3840DL20. The DL indicates the /RESET is an active-low open-drain output. (PL would indicate active-low push-pull output, PH would indicate active-high push-pull, both of which aren’t suitable for my specific application). The threshold voltage is determined by the last two numbers, which for my selection is 20 for 2.0 V. I chose this cutoff point of 2 V because the voltage level is still high enough to reliably activate the transistor circuit for the latching circuit (explained in the next subheading). I found that using a threshold at 1.8 V instead caused unreliable operation, because the batteries are too drained when they reach that level. The added time for dropping the cutoff down 200 mV was only about 5 minutes of gameplay, so it's a worthy sacrifice to bump it up to 2 V instead.

You may notice the VDD pin of U2 isn’t directly powered by the battery voltage (VCC). C1 and R6 on the battery voltage filter out any power supply transients, which can be seen during normal gameplay, but also during initial turn-on. During the boost converter start-up, the loads on the 5 V supply will draw a considerable amount of current from the batteries for nearly a full second, causing the battery voltage to dip quite low - low enough that U2 would detect an undervoltage and shut off the converter before it could finish starting up! Furthermore, loud audio can introduce kHz range ripple on the battery voltage, so the low pass filter will smooth that out to prevent nuisance faults. The current draw of the TPS3840 is less than a microamp, so the voltage drop across R6 is negligible for detecting the *actual* dead battery case. The low consumption also means U2 won't have any perceptible impact to battery life despite always being connected to the batteries.

The CT pin on this chip introduces a delayed start function. Adding a 0.1 uF capacitor to this pin will cause the /RESET output to be held low for approximately 62 milliseconds no matter what the supply voltage is. This is really only useful in the event you put the batteries in while the power switch is already on, because the delay only applies the first time power is applied to the chip. But because the time constant from C1/R6 is so large, this pin doesn't functionally do anything. You can safely omit C2 without impacting operation.

## Latching Circuit (Q1 - Q4)

Without a latching circuit, when U1's enable pin is pulled low by U2 to disable the output once the batteries are discharged enough, all loads will turn off. This has the effect of removing the load of the Game Boy from the batteries. When a load is disconnected from a battery, the voltage rises to a higher value (no load voltage, or open circuit voltage). Because the battery voltage is now once again higher than the undervoltage threshold of U2, the EN pin on U1 is released, and the boost converter restarts. But... that then increases the load on the batteries, the battery voltage drops below U2's detection threshold, and the system shuts off again. And that cycle would repeat. A bootloop! The end effect is a stuttering system with flashing power LED and screen, and a whine or tapping sound out of the speaker. Not the graceful death the original Game Boy experienced when the batteries ran out of juice. 

In order to fix this, a latching circuit is implemented alongisde U2, so that as soon as the batteries die the first time, the system keeps itself shut off until the power switch was cycled off and on again to reset the latch. The latching circuit consists of Q1 through Q4 and R1 through R4. <a href="https://www.ti.com/lit/an/snva836a/snva836a.pdf">This appnote from TI explains the process.</a>

A short explanation: turning on Q2 with a sufficient gate-source voltage sets up the latch. The next time the gate of Q2 (the PSU_EN net) is pulled to GND by the /RESET pin, Q4 is turned on to conduct and the /MR pin (master reset) is also pulled low. On the TPS3840, whenever /MR is low, /RESET will also automatically be pulled low no matter if VCC is above or below the undervoltage trip point. So even if the voltage rises back up above the trip limit, /RESET will keep Q4 conducting, which keeps /MR low as well. So the only way to reset the whole thing is to turn the power switch off and reset the latch.

An added bonus is that there's an internal 100k pull-up resistor on /MR to the VDD pin. When /MR is latched to GND, this causes a voltage divider to be set up with this pull-up resistor and R6, which is also 100k. This cuts the battery voltage that U2 sees in half, and ensures the voltage on the VDD pin is below the "VDD(min)" limit for operation (1.5V), further keeping the enable line from powering on the boost converter.

## Resources
-	<a href="https://www.ti.com/lit/ds/symlink/tps61202.pdf?ts=1680457966449&ref_url=https%253A%252F%252Fwww.ti.com%252Fproduct%252FTPS61202">TPS61202 datasheet</a>
-	<a href="https://www.ti.com/lit/ds/symlink/tps3840.pdf?ts=1656386735143">TPS3840 datasheet</a>
-	<a href="https://www.ti.com/lit/an/snva836a/snva836a.pdf">Latching a Voltage Supervisor Application Note</a>
- <a href="https://www.omicron-lab.com/fileadmin/assets/Bode_100/ApplicationNotes/DC_Biased/App_Note_DC_Bias_Impedance_Caps_V2_0.pdf">DC Biased Impedance Measurements Capacitors</a></i>

## License
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/80x15.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>. You are able to copy and redistribute the material in any medium or format, as well as remix, transform, or build upon the material for any purpose (even commercial) - but you **must** give appropriate credit, provide a link to the license, and indicate if any changes were made.

©MouseBiteLabs 2023
