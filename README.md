# water-detection-pcb
This is a PCB that detects whether or not water is touching the connector (J2). If water connects to the connector then the NMOS gate will have a voltage, and that voltage will turn on both a buzzer and a red LED. If the water isn't detected (i.e., there's no conductivity such that the gate has a voltage from the VCC) then the green LED turns on. This project is in progress and the ultimate goal is to connect an ESP32 to the PCB to send a notification to your phone when water is detected. 12V was used for this.

## Schematic
<img width="785" height="653" alt="Screenshot 2026-08-14 at 5 56 16 PM" src="https://github.com/user-attachments/assets/285f1dbb-93fa-4552-b975-fd1b4e47988d" />

## PCB
<img width="927" height="593" alt="Screenshot 2026-08-14 at 5 56 06 PM" src="https://github.com/user-attachments/assets/3f21aee4-b944-4eae-bff4-50eebaf3f8fe" />

## Explaining Everything
### Big Idea
Water touches J2, i.e., the connector, and this causes conductivity between pin 1 and pin 2 of J2. This conductivity allows the VCC to be drawn into the gates of both MOSFETs (P and N channel). The 10M ohm resistor exists for two reasons. Reason one:  Without a resistance the gate would "float", which is essentially whenever a MOSFET gate doesn't have a reference to GND. Because gates often act as capacitors insofar as the gate can store energy, this can cause the gate to act unpredictably (which would cause the buzzer to sometimes buzz and the red led to sometimes be on). Reason two: water has its own resistance of roughly 500k-1M ohms. So the 10M ohm resistor is put there to act as a voltage divider (which is when two resistors are in series). If the 10M resistor were much less, say 1k ohms, then this could make it so that the PCB no longer accurately detects whether or not water is present at the connector. This is because the voltage drop across a resistor of much less ohms would be far less, and if the voltage drop across the resistor were much less than the gate would have much less voltage supplied to it (in turn this could cause the gate to not turn on as MOSFETs have to have a certain threshold voltage met at the gate). 


### MOSFET
MOSFETs act as switches. The control signal (which means what is used to turn on a transistor) for a MOSFET is voltage. When voltage is supplied to the gate the NMOS will turn on; the opposite is true of a PMOS (I will explain why below). A MOSFET is needed because water has a significant amount of resistance, so the red LED and active buzzer cannot be powered with the very small amount of current that you'd get from water. So the MOSFET is there to act as a switch that allows the 12V to provide substantially more current. 

### P Channel MOSFET
A P Channel MOSFET is essentially the exact opposite of an N Channel MOSFET. Rather than source going to GND, like in an N Channel MOSFET, the source goes to VCC. Any MOSFET must have a certain threshold met with the voltage in the gate relative to the voltage in the source. Since the source is to VCC, the source is always 12V. The source's voltage must be greater than the gate's voltage, as the Vgs (Voltage at the gain relative to voltage at the source, given by Vgs = Vg-Vs) cannot equal 0 for the MOSFET to be turned on. 

### N Channel MOSFET
The source goes to GND. For the gate to turn the NMOS on the voltage at the source cannot equal the voltage at the gate (again, Vgs must be non-zero and must also meet a threshold value). 

### Jack DC
The power is being supplied in the Jack DC by the house. It is sent through a USB. 

### Capacitor
A capacitor stores energy in an electric field. It is here so that the buzzer and LED has enough current to function (because water may not be touching the connector for that long).


