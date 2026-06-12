
# Raspberry Pi Pico Standalone Button Box

A plug-and-play USB HID button box built using a Raspberry Pi Pico (RP2040) running Arduino-based firmware. Designed primarily for sim racing (Assetto Corsa, iRacing, ETS2) and flight simulation, this controller features direct-wired inputs utilizing "Pulse-on-Change" logic to prevent stuck inputs from latching toggle and rotary switches.

<img width="1081" height="693" alt="Capture2" src="https://github.com/user-attachments/assets/164dd837-06b6-4c0d-8c8f-128a42a96b66" />

---

## Features

* **14 Fully Mapped Inputs:**
  * 4 momentary pushbuttons.
  * 1 two-way toggle/latching switch (mapped as 2 independent inputs).
  * M5 Bottom mounting holes for mounting to 4080 aluminium extrusion.
  * 1 eight-way rotary switch (mapped as 8 independent inputs).
* **Direct-Input Wiring:** Eliminates the need for a matrix grid, reducing complex wiring layouts.
* **Zero Diodes Required:** Because the build layout guarantees individual or sequential input handling, no anti-ghosting diodes are needed.
* **Pulse-on-Change Logic:** Prevents multi-position rotary dials and toggle switches from holding a Windows button down permanently. Turning to a position triggers a clean, 80ms momentary pulse before releasing, ensuring perfect game compatibility.

---

<img width="1044" height="503" alt="sdadasda" src="https://github.com/user-attachments/assets/1c976bed-1a57-4ebf-bc12-135c1cb3f3d5" />

## Pinout Configuration

The controller uses 14 sequential GPIO pins on the Raspberry Pi Pico. All components share a common ground line connected to any available `GND` pin on the board.

| Component | Physical Unit | Pico GPIO Pin | Windows Button Index |
| :--- | :--- | :--- | :--- |
| **Pushbutton 1** | Button 1 | **GPIO 0** | Button 1 |
| **Pushbutton 2** | Button 2 | **GPIO 1** | Button 2 |
| **Pushbutton 3** | Button 3 | **GPIO 2** | Button 3 |
| **Pushbutton 4** | Button 4 | **GPIO 3** | Button 4 |
| **2-Way Switch** | Position 1 | **GPIO 4** | Button 5 (80ms Pulse) |
| **2-Way Switch** | Position 2 | **GPIO 5** | Button 6 (80ms Pulse) |
| **8-Way Rotary** | Position 1 | **GPIO 6** | Button 7 (80ms Pulse) |
| **8-Way Rotary** | Position 2 | **GPIO 7** | Button 8 (80ms Pulse) |
| **8-Way Rotary** | Position 3 | **GPIO 8** | Button 9 (80ms Pulse) |
| **8-Way Rotary** | Position 4 | **GPIO 9** | Button 10 (80ms Pulse) |
| **8-Way Rotary** | Position 5 | **GPIO 10** | Button 11 (80ms Pulse) |
| **8-Way Rotary** | Position 6 | **GPIO 11** | Button 12 (80ms Pulse) |
| **8-Way Rotary** | Position 7 | **GPIO 12** | Button 13 (80ms Pulse) |
| **8-Way Rotary** | Position 8 | **GPIO 13** | Button 14 (80ms Pulse) |

---

## Schematic & Wiring Logic

```text
[Button 1] -------------------> GPIO 0
[Button 2] -------------------> GPIO 1
[Button 3] -------------------> GPIO 2
[Button 4] -------------------> GPIO 3

[2-Way Toggle Pos 1] ---------> GPIO 4
[2-Way Toggle Pos 2] ---------> GPIO 5

[8-Way Rotary Pos 1] ---------> GPIO 6
[8-Way Rotary Pos 2] ---------> GPIO 7
...
[8-Way Rotary Pos 8] ---------> GPIO 13

ALL COMMON / GND PINS -------> [Linked Together] -------> Pico GND (e.g., Pin 3)
