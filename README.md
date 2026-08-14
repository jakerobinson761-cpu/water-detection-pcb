# water-detection-pcb
This is a PCB that detects whether or not water is touching the connector (J3). If water connects to the connector then the NMOS gate will have a voltage, and that voltage will turn on both a buzzer and a red LED. If the water isn't detected (i.e., there's no conductivity such that the gate has a voltage from the VCC) then the green LED turns on. This project is in progress and the ultimate goal is to connect an ESP32 to the PCB to send a notification to your phone when water is detected.

## Schematic Image

