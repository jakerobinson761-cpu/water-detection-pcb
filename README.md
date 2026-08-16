# water-detection-pcb
This is a PCB that detects whether or not water is touching the connector (J2). If water connects to the connector then the NMOS gate will have a voltage, and that voltage will turn on both a buzzer and a red LED. If the water isn't detected (i.e., there's no conductivity such that the gate has a voltage from the VCC) then the green LED turns on. This project is in progress and the ultimate goal is to connect an ESP32 to the PCB to send a notification to your phone when water is detected. 12V was used for this.

## Schematic
<img width="791" height="553" alt="Screenshot 2026-08-16 at 11 27 20 AM" src="https://github.com/user-attachments/assets/2b10eef1-51b6-4a06-aee2-ef33a4f1d8f4" />

## PCB
<img width="617" height="581" alt="Screenshot 2026-08-16 at 11 27 35 AM" src="https://github.com/user-attachments/assets/ed96e20d-a835-4025-a75d-05c88209d51f" />

## 3D
<img width="766" height="579" alt="Screenshot 2026-08-16 at 11 24 25 AM" src="https://github.com/user-attachments/assets/1703e7a1-fab8-4133-9a33-59a67fe91678" />

## Explaining Everything

### Big Idea
Whenever water touches a connector attached to this PCB it will sound a buzzer, turn on a red LED, and send a notification to the user's phone that water touched the connector. Whenever the connector is being left alone a green LED will be turned on using backwards logic to how the red LED turns on (the red LED uses a N-channel MOSFET, but the green LED uses a P-Channel MOSFET). But how? Water touches J2, i.e., the connector, and this causes conductivity between pin 1 and pin 2 of J2. This conductivity allows the VCC to be drawn into the gates of both MOSFETs (P and N channel). Once 

### MOSFET
MOSFETs act as switches. The control signal (which means what is used to turn on a transistor) for a MOSFET is voltage. When voltage is supplied to the gate the NMOS will turn on; the opposite is true of a PMOS (I will explain why below). A MOSFET is needed because water has a significant amount of resistance, so the red LED and active buzzer cannot be powered with the very small amount of current that you'd get from water. So the MOSFET is there to act as a switch that allows the 12V to provide substantially more current. 

### P Channel MOSFET
A P Channel MOSFET is essentially the exact opposite of an N Channel MOSFET. Rather than source going to GND, like in an N Channel MOSFET, the source goes to VCC. Any MOSFET must have a certain threshold met with the voltage in the gate relative to the voltage in the source. Since the source is to VCC, the source is always 12V. The source's voltage must be greater than the gate's voltage, as the Vgs (Voltage at the gain relative to voltage at the source, given by Vgs = Vg-Vs) cannot equal 0 for the MOSFET to be turned on.

### N Channel MOSFET (connected to buzzer and red LED)
The source goes to GND. For the gate to turn the NMOS on the voltage at the source cannot equal the voltage at the gate (again, Vgs must be non-zero and must also meet a threshold value). When the gate has a voltage being supplied from the water and VCC the drain and source are now conductive, and the buzzer and red LED are now powered as there's VCC going through the anode of the LED and buzzer and GND going to the cathode of the buzzer and LED. 

### N Channel MOSFET (connected to ESP32) and J3 Explanation
The source goes to GND. When the gate has 0V due to a lack of water touching it the ESP32 is reading 3.3V (which is supplied from the ESP32). This is because there's a pull-up resistor connecting the ESP32 3.3V to a GPIO pin (in J3). Because it's a pull-up resistor, the automatic voltage would be on (as it's being pulled up) rather than 0 (unlike a pull-down resistor, which is what is used for the gate that connects to each MOSFET). The water signal reads 1 (for on) as there's no way to GND (the circuit is incomplete). Once the gate has a voltage the water signal now has a way to GND, so it reads GND or 0 (for off). The ESP32 must have the same GND reference as the GND across the PCB, this is because "ground" is just a reference point of where electrical potential energy begins (sort of like how height=0 in physics 1 isn't an absolute concept but rather just a starting reference point) and not some absolute value. If they aren't connected to the same GND, like how pin 3 of J3 allows, then they would not have the same reference point. 

### Jack DC
The power is being supplied in the Jack DC by the house. It is sent through a USB. 

### Capacitor
A capacitor stores energy in an electric field. It is here so that the buzzer and LED has enough current to function (because water may not be touching the connector for that long).

### Calculating the Resistances
#### For the LEDs (1K Ohm Resistors)
The resistance for the LEDs, 1k ohms, was calculated using ohm's law and KVL, which states that the summation of voltages across a closed loop is equal to 0. The summation of voltages here is Voltage Supply (Vs or 12 V) + Voltage Drop across the LED (2.2 V for a SunFounder LED that was used) + Voltage drop across the resistor. This gives the equation 0=Vs-VLED-VResistor or just 0=12V-2.2V-VResistor. Giving VResistor=9.8V (the voltage drop across the resistor is 9.8V). The maximum current that a SunFounder LED can take is 10mA, which is 0.01A. Using Ohm's law (V=IR) we algebraically get R=V/I. Then we get R=9.8V/0.01A or just R=980 Ohms. The closest resistor I had to 980 Ohms was a 1k Ohm resistor, so that resistor was used.
#### For the pull-down resistor (10M Ohm resistor)
The 10M ohm resistor exists for two reasons. Reason one:  Without a resistance the gate would "float", which is essentially whenever a MOSFET gate doesn't have a reference to GND. Because gates often act as capacitors insofar as the gate can store energy, this can cause the gate to act unpredictably (which would cause the buzzer to sometimes buzz and the red led to sometimes be on). Reason two: water has its own resistance of roughly 500k-1M ohms. So the 10M ohm resistor is put there to act as a voltage divider (which is when two resistors are in series). If the 10M resistor were much less, say 1k ohms, then this could make it so that the PCB no longer accurately detects whether or not water is present at the connector. This is because the voltage drop across a resistor of much less ohms would be far less, and if the voltage drop across the resistor were much less than the gate would have much less voltage supplied to it (in turn this could cause the gate to not turn on as MOSFETs have to have a certain threshold voltage met at the gate). 
#### For the pull-up resistor
I will explain this later.

