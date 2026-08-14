# water-detection-pcb
This is a PCB that detects whether or not water is touching the connector (J2). If water connects to the connector then the NMOS gate will have a voltage, and that voltage will turn on both a buzzer and a red LED. If the water isn't detected (i.e., there's no conductivity such that the gate has a voltage from the VCC) then the green LED turns on. This project is in progress and the ultimate goal is to connect an ESP32 to the PCB to send a notification to your phone when water is detected.

## Schematic
<img width="785" height="653" alt="Screenshot 2026-08-14 at 5 56 16 PM" src="https://github.com/user-attachments/assets/285f1dbb-93fa-4552-b975-fd1b4e47988d" />

## PCB
<img width="927" height="593" alt="Screenshot 2026-08-14 at 5 56 06 PM" src="https://github.com/user-attachments/assets/3f21aee4-b944-4eae-bff4-50eebaf3f8fe" />

