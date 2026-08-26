# water-detection-pcb
Whenever water touches a wire attached to a connector attached to this PCB it will sound a buzzer, turn on a red LED, and send a notification to the user's phone that water touched the connector. Whenever the connector is being left alone a green LED will be turned on using backwards logic to how the red LED turns on (the red LED uses a N-channel MOSFET, but the green LED uses a P-Channel MOSFET). When water touches the wire a signal will be sent to an ESP32 which will send a signal to the user's phone via wifi to alert the user that water touched the wire.

## Schematic
<img width="841" height="538" alt="Screenshot 2026-08-25 at 11 09 10 PM" src="https://github.com/user-attachments/assets/39eed9fd-9edc-41f7-a116-85c7cb70cffb" />

### Color Coding
Red: 12V
Black: GND
Green: Analog Inputs
Yellow: Digital I/O
Blue: PWM Outputs
Orange: Gate


## PCB
<img width="617" height="581" alt="Screenshot 2026-08-16 at 11 27 35 AM" src="https://github.com/user-attachments/assets/ed96e20d-a835-4025-a75d-05c88209d51f" />

## 3D
<img width="766" height="579" alt="Screenshot 2026-08-16 at 11 24 25 AM" src="https://github.com/user-attachments/assets/1703e7a1-fab8-4133-9a33-59a67fe91678" />

## PCB Prior to Soldering
<img width="3024" height="4032" alt="IMG_3382" src="https://github.com/user-attachments/assets/cae13b4c-7999-42fb-ad23-13419467006c" />

<img width="3024" height="4032" alt="IMG_3381" src="https://github.com/user-attachments/assets/9f3b13c0-b3ee-4493-954b-f058e6324096" />

## Me Soldering the PCB

<img width="4284" height="5712" alt="IMG_3445" src="https://github.com/user-attachments/assets/89bdc650-5830-4bdc-8e3b-25e61b731ceb" />

<img width="3024" height="4032" alt="IMG_3444" src="https://github.com/user-attachments/assets/55e4f32a-6b7c-4988-b023-b82879d361c3" />

## What the PCB looks like now (this will be updated)

<img width="3024" height="4032" alt="IMG_3389" src="https://github.com/user-attachments/assets/ff44e495-de32-45cf-a11a-2ee2bca2910b" />

<img width="3024" height="4032" alt="IMG_3385" src="https://github.com/user-attachments/assets/5d2c3a85-eaad-4913-8f9b-99000de5d7dd" />



## Explaining Everything

### Big Idea
Whenever water touches a wire attached to a connector on this PCB, it will sound a buzzer, turn on a red LED, and send a notification to the user's phone that water touched the connector. Whenever the wire isn't being touched by water a green LED will be turned on using backwards logic to how the red LED turns on (the red LED uses a N-channel MOSFET, but the green LED uses a P-Channel MOSFET). But how? Water touches wires that are connected to pins 1 and 2 of a connector (labeled J2 in the schematic). This causes conductivity between pin 1 and pin 2 of this connector. This conductivity allows the VCC to be drawn into the gates of three MOSFETs (P and N-channel). The reason is simple: VCC is constantly being sent to pin 1 of the water-probe connector, and pin 2 is connected to a gate that turns on 2 MOSFETs and turns off one MOSFET (the PMOS). The water acts as a wire to allow both pins to connect, allowing conduction between pins 1 and 2. The text below explains how MOSFETs work and how the N-Channel and P-Channel MOSFETs work in this project.

### MOSFET
MOSFETs act as switches. The control signal (which means what is used to turn on a transistor) for a MOSFET is voltage. When voltage is supplied to the gate the NMOS will turn on; the opposite is true of a PMOS (I will explain why below). A MOSFET is needed because water has a significant amount of resistance, so the red LED and active buzzer cannot be powered with the very small amount of current that you'd get from water. So the MOSFET is there to act as a switch that allows the 12V to provide substantially more current. 

### P Channel MOSFET
A P Channel MOSFET is essentially the exact opposite of an N Channel MOSFET. Rather than source going to GND, like in an N Channel MOSFET, the source goes to VCC. Any MOSFET must have a certain threshold met with the voltage in the gate relative to the voltage in the source. Since the source is to VCC, the source is always 12V. The source's voltage must be greater than the gate's voltage, as the Vgs (Voltage at the gain relative to voltage at the source, given by Vgs = Vg-Vs) cannot equal 0 for the MOSFET to be turned on.

### N Channel MOSFET (connected to buzzer and red LED)
The source goes to GND. For the gate to turn the NMOS on the voltage at the source cannot equal the voltage at the gate (again, Vgs must be non-zero and must also meet a threshold value). When the gate has a voltage being supplied from the water and VCC the drain and source are now conductive, and the buzzer and red LED are now powered as there's VCC going through the anode of the LED and buzzer and GND going to the cathode of the buzzer and LED. 

### N Channel MOSFET (connected to ESP32) and J3 Explanation
The source goes to GND. When the gate has 0V due to a lack of water touching it the ESP32 is reading 3.3V (which is supplied from the ESP32). This is because there's a pull-up resistor connecting the ESP32 3.3V to a GPIO pin (in J3). Because it's a pull-up resistor, the automatic voltage would be on (as it's being pulled up) rather than 0 (unlike a pull-down resistor, which is what is used for the gate that connects to each MOSFET). The water signal reads 1 (for on) as there's no way to GND (the circuit is incomplete). Once the gate has a voltage the water signal now has a way to GND, so it reads GND or 0 (for off). The ESP32 must have the same GND reference as the GND across the PCB, this is because "ground" is just a reference point of where electrical potential energy begins (sort of like how height=0 in physics 1 isn't an absolute concept but rather just a starting reference point) and not some absolute value. If they aren't connected to the same GND, like how pin 3 of J3 allows, then they would not have the same reference point. 

### Jack DC
The power is being supplied in the Jack DC by the house. It is sent through a USB. This uses 12V DC.

### Capacitor
A capacitor stores energy in an electric field. It is here so that the buzzer and LED has enough current to function (because water may not be touching the connector for that long).

### Calculating the Resistances
#### For the LEDs (1K Ohm Resistors)
The resistance for the LEDs, 1k ohms, was calculated using ohm's law and KVL, which states that the summation of voltages across a closed loop is equal to 0. The summation of voltages here is Voltage Supply (Vs or 12 V) + Voltage Drop across the LED (2.2 V for a SunFounder LED that was used) + Voltage drop across the resistor. This gives the equation 0=Vs-VLED-VResistor or just 0=12V-2.2V-VResistor. Giving VResistor=9.8V (the voltage drop across the resistor is 9.8V). The maximum current that a SunFounder LED can take is 10mA, which is 0.01A. Using Ohm's law (V=IR) we algebraically get R=V/I. Then we get R=9.8V/0.01A or just R=980 Ohms. The closest resistor I had to 980 Ohms was a 1k Ohm resistor, so that resistor was used.
#### For the pull-down resistor (10M Ohm resistor)
The 10M ohm resistor exists for two reasons. Reason one:  Without a resistance the gate would "float", which is essentially whenever a MOSFET gate doesn't have a reference to GND. Because gates often act as capacitors insofar as the gate can store energy, this can cause the gate to act unpredictably (which would cause the buzzer to sometimes buzz and the red led to sometimes be on). Reason two: water has its own resistance of roughly 500k-1M ohms. So the 10M ohm resistor is put there to act as a voltage divider (which is when two resistors are in series). If the 10M resistor were much less, say 1k ohms, then this could make it so that the PCB no longer accurately detects whether or not water is present at the connector. This is because the voltage drop across a resistor of much less ohms would be far less, and if the voltage drop across the resistor were much less than the gate would have much less voltage supplied to it (in turn this could cause the gate to not turn on as MOSFETs have to have a certain threshold voltage met at the gate). 
#### For the pull-up resistor (The 10k Ohm Resistor)
This resistor's value easily could've been a different value (e.g., 4.7k Ohms). However, the resistor's value here should be within a certain range. Let's think about what this circuit is doing. A GPIO pin from the ESP32 is sending a 3.3V to pin 1 of J3. Pin 1 sends that 3.3V to pin 2 of J2, and J2 is also connected to the drain of an NMOS. Why do we even need a resistor? Suppose that the NMOS turns on, as it is of course supposed to, then pin 2 of J3 will go straight to gnd (from drain-->source) when the gate turns on. This will cause a short circuit as the 3.3V will go from basically 0 resistance to gnd, and as the resistance approaches 0 the current increases exponentially (due to I=V/R). But why did I choose 10k? Two reasons: 1. we don't want to waste current from the ESP32 when the NMOS turns on. If the resistance were much smaller then the current supplied from the ESP32 would be much higher (again Ohm's law). We don't want to waste energy, and unnecessarily high currents will waste energy due to energy being P*time=energy and P=VI (so as I increases power increases and as power increases energy increases). For practical purposes we should conserve energy. So if this is the case then why not just have a super high resistor (like why don't not use another 10M Ohm resistor instead of the 10k Ohm resistor?). Reason 2 explains this. 2. if the resistance of the resistor between pin 1 and pin 2 of J3 is too high then the voltage drop across the resistor will be incredibly high (it isn't possible for me to calculate this as I don't have the current for extremely high values. In fact I tried to calculate it and got the voltage drop as 50V when 5microamperes = I, which is standard for how much current ESP32s leak, and R=10M ohms. This voltage drop is clearly impossible, so it seems the manufacturer said that ESP32s generally leak 5 microamps because that's true for a set range of values, which didn't encompass very large resistances like 10M ohms). If you use the 10K ohm resistor then you will have a voltage drop (Vd=Vi-Vf) of 0.05. So the voltage being supplied to the water sensor will be 3.25V. This is perfectly sufficient for the ESP32 to use connect to pin 2 (the water sensor) and use the pin as a digital input. If the digital input reads 1 then the water is connecting a wire connected to J2. If the digital input reads 0 then the water isn't connecting a wire connected to J2.

