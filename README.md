
# Raspberry Pi Pico Standalone Button Box

A plug-and-play USB HID button box built using a Raspberry Pi Pico (RP2040) running Arduino-based firmware. Designed primarily for sim racing (Assetto Corsa, iRacing, ETS2) and flight simulation, this controller features direct-wired inputs utilizing "Pulse-on-Change" logic to prevent stuck inputs from latching toggle and rotary switches.

<img width="4032" height="3024" alt="image" src="https://github.com/user-attachments/assets/1cd00f6a-bb46-49db-bf7c-d8902de78f42" />


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


## Code

Copy the code and paste it into arduino ide and upload to the raspberry pi pico.

```text

#include <Joystick.h>

const int NUM_BUTTONS = 14;
const int BUTTON_PINS[NUM_BUTTONS] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13};
int lastButtonState[NUM_BUTTONS] = {0};


void setup() {
  for (int i = 0; i < NUM_BUTTONS; i++) {
    pinMode(BUTTON_PINS[i], INPUT_PULLUP); 
    lastButtonState[i] = !digitalRead(BUTTON_PINS[i]); 
  }
  
  Joystick.begin(); 
}

void loop() {
  // 1. STANDARD PUSHBUTTONS (Pins 0 to 3)
  for (int i = 0; i <= 3; i++) {
    int currentButtonState = !digitalRead(BUTTON_PINS[i]);
    if (currentButtonState != lastButtonState[i]) {
      Joystick.setButton(i, currentButtonState);
      lastButtonState[i] = currentButtonState;
    }
  }

  // 2. 2-WAY SWITCH & 8-WAY ROTARY (Pins 4 to 13)
  for (int i = 4; i < NUM_BUTTONS; i++) {
    int currentSwitchState = !digitalRead(BUTTON_PINS[i]);
    if (currentSwitchState != lastButtonState[i]) {
      if (currentSwitchState == 1) {
        Joystick.setButton(i, 1); 
        delay(80);                
        Joystick.setButton(i, 0); 
      }
      lastButtonState[i] = currentSwitchState; 
    }
  }
  
  delay(10); 
}


```
## Schematic & Wiring Logic

IMPORTANT : Keep wires relatively long to allow opening of the front plate for the mounting screws.

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

```

Also on my Printables : (https://www.printables.com/model/1750208-sim-racing-button-box)
