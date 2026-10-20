# palmvein-ir

An all-in-one IR illumination board for palm vein imaging, built using 16 high-power 860nm LEDs in two independently controlled channels.

<p align="center">
  <img src="https://github.com/user-attachments/assets/1e36b437-3c27-4209-960c-ca9d269097ec" alt="Front of the board" width="49%"/>
  <img src="https://github.com/user-attachments/assets/494c16f6-3b47-4f0b-a189-7367112cee78" alt="Back of the board" width="49%"/>
</p>

## Why?

I built this board to serve as a second, more complete iteration of my original ir-led-board design [here](https://github.com/brianwalczak/ir-led-board). The main limitation of the first iteration was that it used 16 low-current IR LEDs, which made images less detailed because the infrared light didn't penetrate the palm effectively. The images were good for a proof-of-concept neural network but lacked the detail needed for a high detection accuracy. The square design also didn't accurately reflect the shape of an actual palm.

Another issue was the camera cutout. I decided to switch to the OV9281 for its global shutter and lack of a Bayer filter (which makes it the better camera for IR imaging). However, it uses a longer barrel lens, so the board needed a larger, circular camera cutout.

The board also lacked a position sensor, which made it difficult to calculate the palm's distance from the camera. For this first prototype, I tried to estimate palm distance using the average dimensions of a human palm, but I found out it was very unreliable, especially among varying age groups. I found that a distance sensor is the most reliable solution because adding an algorithm that estimates age range would honestly be way overengineered (and still unreliable among some people).

Finally, I added a dedicated microcontroller to handle the LED processing separately from the Raspberry Pi, which allows for precise LED flickering and switching without being affected by Linux delays!

## Features

I designed this board as an all-in-one solution to power and control the infrared LEDs, handle distance calculations, and to power the Raspberry Pi:

- 16x 860nm IR LEDs (OSRAM SFH 4718A) in two independently controlled channels
- Constant-current drivers (AL8861Y-13) with PWM brightness control, both running at about 909 mA
- Microcontroller (CH32V003) for LED control and channel switching
- Time-of-flight distance sensor (VL53L4CDV0DH/1) for measuring the palm's distance from the camera
- USB-C PD power at 20V, which also powers the Raspberry Pi 5 through a 5V buck converter
- Surge protector and eFuse for overvoltage and overcurrent protection

I chose USB-C over a fixed DC power supply because it's easier to use and more compact. The Raspberry Pi is powered through a custom JST cable connected to its 5V pins.

## Bill of Materials (BOM)

The key components for this project are listed below, but you can find the full BOM (including capacitors, resistors, etc.) [here](bom.csv).

| Qty | Component | Part |
|----:|-----------|------|
| 16 | 860nm IR LED | OSRAM SFH 4718A |
| 1 | Microcontroller | CH32V003A4M6 |
| 1 | Distance sensor | VL53L4CDV0DH/1 |
| 1 | USB-C PD trigger | CH224A |
| 1 | Pi buck converter (5V, 5A) | SY8366HQQC |
| 1 | 3.3V regulator | AMS1117-3.3 |
| 2 | LED driver | AL8861Y-13 |
| 1 | eFuse | MX25947ES |
| 1 | Surge protector (TDS) | TDS2211P.C(ES) |
| 1 | USB-C port | TYPE-C-31-M-12 |
| 1 | JST VH 6-pin connector | S6P-VH(LF)(SN)(P) |
| 1 | 4-pin programming header | PZ2.54-1x4PWZ-L13.8-A |

**Not included:** Raspberry Pi 5, OV9281-110 Camera, and a 65W+ USB-C PD charger.

## License

All hardware design files are released under the [CERN-OHL-P-2.0](LICENSE) license! Symbols, footprints, and 3D models were imported from EasyEDA using JLCImport and are not covered by this license.