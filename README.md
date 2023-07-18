# Pocket Mouse Power Board

This is a power regulator board for my <a href="https://github.com/MouseBiteLabs/Game-Boy-Pocket-Color">Game Boy Pocket Color (MGBC)</a> project. This *should* work perfectly fine on a regular Game Boy Color or Pocket, but I haven't exhaustively tested it yet. I have, however, tested it in my MGBC project extensively.

![image](https://user-images.githubusercontent.com/97127539/235564188-b096e7be-f18f-4230-86a4-a3ee6293c007.png)
![image](https://user-images.githubusercontent.com/97127539/235564273-9be6551b-4502-4b76-bf6e-bf85e09e2950.png)

*Note: Images show v1.2, rather than the current v1.3. The differences are minimal - see details below.*

## Disclaimer

I highly suggest testing this board before installation in a Game Boy. Failure to do so may result in an irreversibly damaged Game Boy. See the testing sequence below.

**This is a complicated build that requires high level soldering skills. Please do not attempt if you do not have advanced soldering experience. Do not attempt to hand-solder the QFN component on this board, you must use either hot air or a hot plate. I am not responsible for any damage done to you or your Game Boy. You accept any and all risks associated with attempting this project.**

## Board Characteristics and Order Information

The zipped folder contains all the gerber files for this board.
-	Layers: 2
-	Thickness: 1.6mm or thinner
-	Surface Finish: ENIG or HASL (ENIG may be easier to solder)


**I sell this board on Etsy, so you don't have to buy multiples from board fabricators:**

<a href="https://www.etsy.com/listing/1473483915/pocket-mouse-power-board"><img src="https://github-production-user-asset-6210df.s3.amazonaws.com/97127539/239718536-5c9aefe3-0628-4434-b8d8-55ff80ac3bbc.png" alt="PCB from Etsy" /></a> 

You can alternatively use the zipped folder at any board fabricator you like. You may also buy the board from PCBWay using this link (disclosure: I receive 10% of the sale value to go towards future PCB orders):

<a href="https://www.pcbway.com/project/shareproject/Pocket_Mouse_Power_Board_3c1c3bc6.html"><img src="https://www.pcbway.com/project/img/images/frompcbway-1220.png" alt="PCB from PCBWay" /></a> 

## Features

Some features of this power board include: 

* Changing the power switch to a "soft power switch," meaning the main power of the Game Boy does not flow through the switch. A dirty power switch can severly impact performance, so using this scheme can remedy that issue.
* Generally improved efficiency over the OEM regulator, and increased switching frequency.
* Bootloop protection circuitry to prevent overdischarge of AAA batteries, and to allow the Game Boy to die gracefully instead of violently stuttering until the regulator gives up.

## Testing

I suggest testing the assembled board externally before mounting it to your Game Boy. If you have misassembled it, a part is incorrect, if there is a hidden solder bridge, or one of many other issues - your 5 V supply might be too high when you turn it on! This is not ideal for your Game Boy! So, please, test the board before you fully connect it to the system.

![image](https://github.com/MouseBiteLabs/Pocket-Mouse-Power-Board/assets/97127539/4b9cfd69-4874-4d1a-8229-f0895ddd776d)

### Version 1.3 Testing
- Connect pin 3 (white box) to negative side of the input voltage source - such as a battery holder with alligator clips or a benchtop power supply.
- Connect pins 1 and 2 (red and yellow boxes) to positive side of the input voltage source. The voltage from the power source must be at least 2.2 V (do not exceed ~4.5 V).
- Turn power on.
- Measure voltage using a multimeter in DC voltage mode - positive probe on pin 7 (blue box) and negative probe on pin 3 (white box). The voltage should read between 4.95 V and 5.05 V.

### Version 1.2 Testing
- Connect pin 3 (white box) to negative side of the input voltage source - such as a battery holder with alligator clips or a benchtop power supply.
- Connect pin 2 (yellow box) to positive side of the input voltage source. The voltage from the power source must be at least 2.2 V (do not exceed ~4.5 V).
- Turn power on. Wait 10 seconds for capacitors to fully charge.
- Connect pin 1 (red box) to pin 2 (yellow box) using a switch (preferred) or some other means, such as a wire.
- Measure voltage using a multimeter in DC voltage mode - positive probe on pin 7 (blue box) and negative probe on pin 3 (white box). The voltage should read between 4.95 V and 5.05 V.

### Safely Testing Any Version Using a Game Boy
- Connect pins 1 (red box), 2 (yellow box), and 3 (white box) to the proper locations on the Game Boy circuit board using wire. DO NOT CONNECT PIN 7 (blue box)!!
- Connect input voltage source to the battery tabs (battery holder with alligator clips, power supply, or simply putting the board in the back half of a Game Boy shell with batteries). Voltage source must be at least 2.2 V (do not exceed ~4.5 V).
- Turn the power switch on.
- Measure voltage using a multimeter in DC voltage mode - positive probe on pin 7 (blue box) and negative probe on pin 3 (white box). The voltage should read between 4.95 V and 5.05 V.

## Using with a Game Boy Color or Game Boy Pocket

*Note: This board only supplies the 5V power rail - there are no LCD supplies. This board is only a proper replacement if you have modded your system with a modern IPS kit.*

If you wish to use this on an original Game Boy system, you must:

* Remove U5 from the Game Boy circuit board (the DC/DC converter board) and replace it with this one. Make sure you have a header pin populated for pin 2 on the socket!
* On the main Game Boy PCB, add a wire from pin 2 of the U5 socket to the node labelled SW1-VCC. This node connects to the power switch and F1, so you can choose either points. Note that Game Boy Pockets do not have pin 2 available, so you must run a wire directly from the Pocket Mouse Power Board's pin 2 instead.
* If you are soldering a wire to the fuse directly, **MAKE SURE YOU CHOOSE THE CORRECT SIDE**. It must be the side that's directly connected to the power switch. A multimeter will not tell you which side of the fuse is which if you have the fuse on the board.

![image](https://user-images.githubusercontent.com/97127539/229369773-2288882a-4835-4864-8fc3-efc7d6048814.png)

## Bill of Materials

A prepopulated cart from Mouser can be found here: https://www.mouser.com/ProjectManager/ProjectDetail.aspx?AccessID=8cd8ac3cba. The quantities in the cart are enough such that there is at least one extra component in case you drop or lose one, except for U1 and U2 as they are more expensive. You may want to consider ordering multiple quantities of those parts just in case.

If parts are out of stock or backordered, you can search for the parts on Digikey. The only parts that have no replacements are U1 and U2, every other component has suitable alternate parts you can search for.

C2 is removed on v1.3, and D1 is only used on v1.3.

| Ref Des        | QTY | Value       | Footprint | Type             | Notes                                        | Link                                                                                                                                                                                                                                                   |
| -------------- | --- | ----------- | --------- | ---------------- | -------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| C1, C3, C5     | 3   | 22uF        | 0805      | Capacitor (MLCC) | X5R or better, 16V or higher                 | https://www.mouser.com/ProductDetail/Murata-Electronics/GRM21BR61E226ME44K?qs=hNud%2FORuBR25jDsUehWlrQ%3D%3D                                                                                                                                           |
| C2, C4         | 2   | 0.1uF       | 0603      | Capacitor (MLCC) | C2 is removed on v1.3                        | [https://www.mouser.com/ProductDetail/KEMET/C0603C104M3RACTU?qs=7q2aiX3Gdlh4qBRaMcnohQ%3D%3D](https://www.mouser.com/ProductDetail/KEMET/C0603C104M3RACTU?qs=7q2aiX3Gdlh4qBRaMcnohQ%3D%3D)                                                             |
| D1             | 1   | BAT42       | SOD323    | Schottky Diode   | Only used on v1.3                            | https://www.mouser.com/ProductDetail/78-BAT42WS-E3-18
| L1             | 1   | 2.2uH       | 1212      | Inductor         | Saturation current higher than 1 A preferred | [https://www.mouser.com/ProductDetail/Murata-Electronics/LQH3NPN2R2MMEL?qs=sJ5gq5MLFXqbWZz8NzZoog%3D%3D](https://www.mouser.com/ProductDetail/Murata-Electronics/LQH3NPN2R2MMEL?qs=sJ5gq5MLFXqbWZz8NzZoog%3D%3D)                                       |
| Q1             | 1   | MMBT3906    | SOT23     | PNP BJT          |                                              | [https://www.mouser.com/ProductDetail/Micro-Commercial-Components-MCC/MMBT3906HE3-TP?qs=HBWAp0VN4Rh%2Ft2ZPx%252BV99A%3D%3D](https://www.mouser.com/ProductDetail/Micro-Commercial-Components-MCC/MMBT3906HE3-TP?qs=HBWAp0VN4Rh%2Ft2ZPx%252BV99A%3D%3D) |
| Q2, Q4         | 2   | 2N7002      | SOT23     | N-channel MOSFET |                                              | [https://www.mouser.com/ProductDetail/Nexperia/2N7002NXBKR?qs=%252B6g0mu59x7J2ddJstTJGkQ%3D%3D](https://www.mouser.com/ProductDetail/Nexperia/2N7002NXBKR?qs=%252B6g0mu59x7J2ddJstTJGkQ%3D%3D)                                                         |
| Q3             | 1   | MMBT3904    | SOT23     | NPN BJT          |                                              | [https://www.mouser.com/ProductDetail/Nexperia/MMBT3904VL?qs=cnAQGvEIVkKbCwIpHJoHxQ%3D%3D](https://www.mouser.com/ProductDetail/Nexperia/MMBT3904VL?qs=cnAQGvEIVkKbCwIpHJoHxQ%3D%3D)                                                                   |
| R1, R4, R6, R8 | 4   | 100k        | 0603      | Resistor         |                                              | [https://www.mouser.com/ProductDetail/YAGEO/RC0603FR-07100KL?qs=e1ok2LiJcmaihem8Va5%2Fsw%3D%3D](https://www.mouser.com/ProductDetail/YAGEO/RC0603FR-07100KL?qs=e1ok2LiJcmaihem8Va5%2Fsw%3D%3D)                                                         |
| R2, R3, R5     | 3   | 10k         | 0603      | Resistor         |                                              | [https://www.mouser.com/ProductDetail/YAGEO/RC0603FR-0710KL?qs=grNVn54RoB%252B3GtjbJj3wJQ%3D%3D](https://www.mouser.com/ProductDetail/YAGEO/RC0603FR-0710KL?qs=grNVn54RoB%252B3GtjbJj3wJQ%3D%3D)                                                       |
| U1             | 1   | TPS61202    | WSON-10   | Boost Converter  |                                              | [https://www.mouser.com/ProductDetail/Texas-Instruments/TPS61202DSCR?qs=WxL8HmPi5r6YtrNaHRAS2Q%3D%3D](https://www.mouser.com/ProductDetail/Texas-Instruments/TPS61202DSCR?qs=WxL8HmPi5r6YtrNaHRAS2Q%3D%3D)                                             |
| U2             | 1   | TPS3840DL20 | SOT23-5   | Supervisory IC   |                                              | [https://www.mouser.com/ProductDetail/Texas-Instruments/TPS3840DL20DBVR?qs=T3oQrply3y%2FZsfSrLIG7Ww%3D%3D](https://www.mouser.com/ProductDetail/Texas-Instruments/TPS3840DL20DBVR?qs=T3oQrply3y%2FZsfSrLIG7Ww%3D%3D)                                   |

## Potential Issues

- Using a flash cart, such as the Everdrive or EZ-Flash, the power draw during start-up may be too high and cause the battery voltage to drop below the 2V threshold. You may be able to get it to boot by turning the power switch off and on again. Regardless, this should only occur when you have very low battery life. The components I have chosen should effectively prevent this error from occurring, but I have not tested every system configuration possible.
- **Version 1.2 only:** When first placing batteries in, you must wait a few seconds for the capacitors to charge up, before it will operate correctly. This is only upon first insertion - once the batteries have been in the system for longer than a few seconds, it will operate completely normally.

## Revision History

### v1.3
- Added diode to fast charge C1 (removes need to wait a few seconds before use after placing batteries in the first time)
- Added test points for various nets
- Removed C2

### v1.2
- Changed C1 to 0805 size, changed value to 22uF
- R6 changed to 100k
- Added test points for U2 supply voltage and PSU_EN net

### v1.1
- Removed some unnecessary components
- Changed the voltage cut-off to 2V instead of 1.8V to enable reliable bootloop prevention

### v1.0
- Release version

## License
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/80x15.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>. You are able to copy and redistribute the material in any medium or format, as well as remix, transform, or build upon the material for any purpose (even commercial) - but you **must** give appropriate credit, provide a link to the license, and indicate if any changes were made.

©MouseBiteLabs 2023
