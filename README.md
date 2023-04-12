# Pocket Mouse Power Board

This is a power regulator board for my <a href="https://github.com/MouseBiteLabs/Game-Boy-Pocket-Color">Game Boy Pocket Color (MGBC)</a> project. This *should* work perfectly fine on a regular Game Boy Color or Pocket, but I haven't exhaustively tested it yet.

![image](https://user-images.githubusercontent.com/97127539/229369823-6b3c8f6f-b788-4293-b8e0-fbcc4cb27b1e.png)

## Disclaimer

Please keep in mind, I haven't fully tested this power board, so **you accept any and all risks associated with using this board.** That being said, it's similar to a lot of other replacement Game Boy power supplies out there, with a few distinct differences. I'll be completing full testing of this board in the coming weeks.

If you are assembling this yourself, I suggest testing the assembled board externally before mounting it to your Game Boy. This can be done by connecting pins 1 *and* 2 to the positive terminal of an input voltage source with at least 2 V output (using battery clips or an external power supply) and measuring the output voltage on pin 7 (remember to connect pin 3 to the ground/negative end of the power source). Pin 7 should read a consistent ~5 V. At the very least, you can add the assembled board to your Game Boy PCB, but keep pin 7 empty until you confirm with a meter that you are getting a solid 5 V output, after which you can add a wire to connect it to the main board.

## Board Characteristics

The zipped folder contains all the gerber files for this board.
-	Layers: 2
-	Thickness: 1.6mm or thinner
-	Surface Finish: ENIG or HASL (ENIG may be easier to solder)

## Features

Some features of this power board include: 

* Changing the power switch to a "soft power switch," meaning the main power of the Game Boy does not flow through the switch. A dirty power switch can severly impact performance, so using this scheme can remedy that issue.
* Generally improved efficiency over the OEM regulator, and increased switching frequency.
* Improves the audio output of the Game Boy.
* Bootloop protection circuitry to prevent overdischarge of AAA batteries, and to allow the Game Boy to die gracefully instead of violently stuttering until the regulator gives up.

## Using with a Game Boy Color or Game Boy Pocket

*Note: This board only supplies the 5V power rail - there are no LCD supplies. This board is only a proper replacement if you have modded your system with a modern IPS kit.*

If you wish to use this on an original Game Boy system, you must:

* Remove U5 from the Game Boy circuit board (the DC/DC converter board) and replace it with this one. Make sure you have a header pin populated for pin 2 on the socket!
* On the main Game Boy PCB, add a wire from pin 2 of the U5 socket to the node labelled SW1-VCC. This node connects to the power switch and F1, so you can choose either points. 
* If you are soldering a wire to the fuse directly, **MAKE SURE YOU CHOOSE THE CORRECT SIDE**. It must be the side that's directly connected to the power switch. A multimeter will not tell you which side of the fuse is which if you have the fuse on the board.

![image](https://user-images.githubusercontent.com/97127539/229369773-2288882a-4835-4864-8fc3-efc7d6048814.png)

## Bill of Materials

A prepopulated cart from Mouser can be found here: https://www.mouser.com/ProjectManager/ProjectDetail.aspx?AccessID=6cc4472eea. The quantities in the cart are enough such that there is at least one extra component in case you drop or lose one, except for U1 and U2 as they are more expensive. You may want to consider ordering multiple quantities of those parts just in case.

If parts are out of stock or backordered, you can search for the parts on Digikey. The only parts that have no replacements are U1 and U2, every other component has suitable alternate parts you can search for.

| Ref Des        | QTY | Value       | Footprint | Type             | Notes                                        | Link                                                                                                                                                                                                                                                   |
| -------------- | --- | ----------- | --------- | ---------------- | -------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| C1             | 1   | 1uF         | 0603      | Capacitor (MLCC) |                                              | https://www.mouser.com/ProductDetail/KEMET/C0603C105K3PAC?qs=Zh2ruSKDRuDYcd11xelchg%3D%3D                                                                                                                                                              |
| C2, C4         | 2   | 0.1uF       | 0603      | Capacitor (MLCC) |                                              | [https://www.mouser.com/ProductDetail/KEMET/C0603C104M3RACTU?qs=7q2aiX3Gdlh4qBRaMcnohQ%3D%3D](https://www.mouser.com/ProductDetail/KEMET/C0603C104M3RACTU?qs=7q2aiX3Gdlh4qBRaMcnohQ%3D%3D)                                                             |
| C3, C5         | 2   | 22uF        | 0805      | Capacitor (MLCC) | X5R or better, 16V or higher                 | https://www.mouser.com/ProductDetail/Murata-Electronics/GRM21BR61E226ME44K?qs=hNud%2FORuBR25jDsUehWlrQ%3D%3D                                                                                                                                           |
| L1             | 1   | 2.2uH       | 1212      | Inductor         | Saturation current higher than 1 A preferred | [https://www.mouser.com/ProductDetail/Murata-Electronics/LQH3NPN2R2MMEL?qs=sJ5gq5MLFXqbWZz8NzZoog%3D%3D](https://www.mouser.com/ProductDetail/Murata-Electronics/LQH3NPN2R2MMEL?qs=sJ5gq5MLFXqbWZz8NzZoog%3D%3D)                                       |
| Q1             | 1   | MMBT3906    | SOT23     | PNP BJT          |                                              | [https://www.mouser.com/ProductDetail/Micro-Commercial-Components-MCC/MMBT3906HE3-TP?qs=HBWAp0VN4Rh%2Ft2ZPx%252BV99A%3D%3D](https://www.mouser.com/ProductDetail/Micro-Commercial-Components-MCC/MMBT3906HE3-TP?qs=HBWAp0VN4Rh%2Ft2ZPx%252BV99A%3D%3D) |
| Q2, Q4         | 2   | 2N7002      | SOT23     | N-channel MOSFET |                                              | [https://www.mouser.com/ProductDetail/Nexperia/2N7002NXBKR?qs=%252B6g0mu59x7J2ddJstTJGkQ%3D%3D](https://www.mouser.com/ProductDetail/Nexperia/2N7002NXBKR?qs=%252B6g0mu59x7J2ddJstTJGkQ%3D%3D)                                                         |
| Q3             | 1   | MMBT3904    | SOT23     | NPN BJT          |                                              | [https://www.mouser.com/ProductDetail/Nexperia/MMBT3904VL?qs=cnAQGvEIVkKbCwIpHJoHxQ%3D%3D](https://www.mouser.com/ProductDetail/Nexperia/MMBT3904VL?qs=cnAQGvEIVkKbCwIpHJoHxQ%3D%3D)                                                                   |
| R1, R4, R8     | 3   | 100k        | 0603      | Resistor         |                                              | [https://www.mouser.com/ProductDetail/YAGEO/RC0603FR-07100KL?qs=e1ok2LiJcmaihem8Va5%2Fsw%3D%3D](https://www.mouser.com/ProductDetail/YAGEO/RC0603FR-07100KL?qs=e1ok2LiJcmaihem8Va5%2Fsw%3D%3D)                                                         |
| R2, R3, R5, R6 | 4   | 10k         | 0603      | Resistor         |                                              | [https://www.mouser.com/ProductDetail/YAGEO/RC0603FR-0710KL?qs=grNVn54RoB%252B3GtjbJj3wJQ%3D%3D](https://www.mouser.com/ProductDetail/YAGEO/RC0603FR-0710KL?qs=grNVn54RoB%252B3GtjbJj3wJQ%3D%3D)                                                       |
| U1             | 1   | TPS61202    | WSON-10   | Boost Converter  |                                              | [https://www.mouser.com/ProductDetail/Texas-Instruments/TPS61202DSCR?qs=WxL8HmPi5r6YtrNaHRAS2Q%3D%3D](https://www.mouser.com/ProductDetail/Texas-Instruments/TPS61202DSCR?qs=WxL8HmPi5r6YtrNaHRAS2Q%3D%3D)                                             |
| U2             | 1   | TPS3840DL20 | SOT23-5   | Supervisory IC   |                                              | [https://www.mouser.com/ProductDetail/Texas-Instruments/TPS3840DL20DBVR?qs=T3oQrply3y%2FZsfSrLIG7Ww%3D%3D](https://www.mouser.com/ProductDetail/Texas-Instruments/TPS3840DL20DBVR?qs=T3oQrply3y%2FZsfSrLIG7Ww%3D%3D)                                   |

## Potential Issues

- Using a flash cart, such as the Everdrive or EZ-Flash, the power draw during start-up may be too high and cause the battery voltage to drop below the 2V threshold. But if you simply cycle the power switch when this happens, the stored energy in the cart should be enough to get it to start-up normally.

## Revision History

### v1.1
- Removed some unnecessary components
- Changed the voltage cut-off to 2V instead of 1.8V to enable reliable bootloop prevention

### v1.0
- Release version

## License
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/80x15.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>. You are able to copy and redistribute the material in any medium or format, as well as remix, transform, or build upon the material for any purpose (even commercial) - but you **must** give appropriate credit, provide a link to the license, and indicate if any changes were made.

©MouseBiteLabs 2023
