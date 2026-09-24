# water-detection-pcb
Whenever water touches a wire attached to a connector attached to this PCB it will sound a buzzer, turn on a red LED, and send a notification to the user's phone that water touched the connector. Whenever the connector is being left alone a green LED will be turned on using backwards logic to how the red LED turns on (the red LED uses a N-channel MOSFET, but the green LED uses a P-Channel MOSFET). When water touches the wire a signal will be sent to an ESP32 which will send a signal to the user's phone via wifi to alert the user that water touched the wire (this is yet to be implemented).

## Demonstration Video (Hardware--Prior to ESP32)

[[https://youtube.com/shorts/jh2ppAtmCCc -- this video shows the hardware part of this PCB working, but issues came up later on that had to be resolved.
](https://youtube.com/shorts/bYx4-bdKwYc?feature=share)](https://youtube.com/shorts/bYx4-bdKwYc?feature=share)

## Schematic
<img width="931" height="615" alt="Screenshot 2026-09-23 at 11 18 48 PM" src="https://github.com/user-attachments/assets/64339067-2900-4333-8dd4-24769c0a5a43" />


### Note
Note: the 10M Ohm resistor has since been changed to 10K Ohms, and I plan on changing it to 100K Ohms to solve issues regarding that specific pull-down resistor being too weak.

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

### Week 1 of Building

<img width="4284" height="5712" alt="IMG_3445" src="https://github.com/user-attachments/assets/89bdc650-5830-4bdc-8e3b-25e61b731ceb" />

<img width="3024" height="4032" alt="IMG_3444" src="https://github.com/user-attachments/assets/55e4f32a-6b7c-4988-b023-b82879d361c3" />

### Week 3 of Building

#### (I started wearing safety glasses just in case)

<img width="4284" height="5712" alt="IMG_3545" src="https://github.com/user-attachments/assets/da1da70b-30f5-4bab-9e86-9051a62bd88a" />

<img width="4284" height="5712" alt="IMG_3541" src="https://github.com/user-attachments/assets/4bf4830a-b55a-4eea-94d3-6841d294d92d" />

<img width="3024" height="4032" alt="IMG_3540" src="https://github.com/user-attachments/assets/c26d4249-29bd-469a-b856-f3b57d459bba" />


## What the PCB looks (complete)

<img width="3024" height="4032" alt="IMG_4070" src="https://github.com/user-attachments/assets/90db2736-641d-44e1-a838-1af2a43f9748" />

<img width="4284" height="5712" alt="IMG_4069" src="https://github.com/user-attachments/assets/50ee9ad0-bc89-4b50-af5a-4897cb66a6f6" />

<img width="4032" height="3024" alt="IMG_4071" src="https://github.com/user-attachments/assets/eed638ae-e1c0-4e80-817b-18baa7677760" />



## Parts Destroyed in the Process

<img width="4284" height="5712" alt="IMG_3811" src="https://github.com/user-attachments/assets/2d30353d-613e-410f-a9cd-a8ebbd5d2daf" />

Two PMOSs and One Jack DC were destroyed in the making of this PCB.

Here's another part that was destroyed; it's the 10K ohm resistor in R4 I mistakingly replaced because I didn't check the schematic prior to making hardware changes. 

<img width="3024" height="4032" alt="IMG_3815" src="https://github.com/user-attachments/assets/f7a3ed3a-9db5-4c43-a3c1-e59303e77628" />

The 10M Ohm resistor that was replaced was destroyed, but I forgot to take a picture of it :(

<img width="4284" height="5712" alt="IMG_4013" src="https://github.com/user-attachments/assets/ab68570f-60fe-4e86-9ac9-14973945579d" />

The 10K Ohm resistor that replaced the 10M Resistor was replaced with a 100K Ohm resistor. That destroyed 10K Ohm resistor is shown above.

<img width="3024" height="4032" alt="IMG_4014" src="https://github.com/user-attachments/assets/dfc98024-7a05-44dd-b219-5fe5e3026dc3" />

The red LED randomly stopped working, when a multimeter read the DC voltage from the anode to cathode of said resistor it said 11V. I replaced that LED, and this is the destroyed LED that came out of the PCB.

<img width="4284" height="5712" alt="IMG_4068 (1)" src="https://github.com/user-attachments/assets/1179885e-30f7-45cf-9232-5468c29a249e" />

These probes aren't unusable per se, but they certainly were destroyed. When the green LED remained on even when the water was touching the probes, the "easiest fix" hypothesis was to replace said probes. That is exactly what was done, and the issue (unexpectedly) completely resolved.


## Explaining Everything

### Big Idea
Whenever water touches a wire attached to a connector on this PCB, it will sound a buzzer, turn on a red LED, and send a notification to the user's phone that water touched the connector (the ESP-32 software part is in progress). Whenever the wire isn't being touched by water a green LED will be turned on using backwards logic to how the red LED turns on (the red LED uses a N-channel MOSFET, but the green LED uses a P-Channel MOSFET). But how? Water touches wires that are connected to pins 1 and 2 of a connector (labeled J2 in the schematic). This causes conductivity between pin 1 and pin 2 of this connector. This conductivity allows the VCC to be drawn into the gates of three MOSFETs (P and N-channel). The reason is simple: VCC is constantly being sent to pin 1 of the water-probe connector, and pin 2 is connected to a gate that turns on 2 MOSFETs and turns off one MOSFET (the PMOS). The water acts as a wire to allow both pins to connect, allowing conduction between pins 1 and 2. The text below explains how MOSFETs work and how the N-Channel and P-Channel MOSFETs work in this project.

### MOSFET
MOSFETs act as switches. The control signal (which means what is used to turn on a transistor) for a MOSFET is voltage. When voltage is supplied to the gate the NMOS will turn on; the opposite is true of a PMOS (I will explain why below). A MOSFET is needed because water has a significant amount of relative resistance and the resistance of water can greatly vary (usually, but not always), so the red LED and active buzzer normally cannot be powered with the very small amount of current that you'd (on average) get from water. The resistance of the water, that I got from solving for the resistance of the water using the voltage divider equation (in section "How I finally figured out the resistance of the water"), is 7,120ish Ohms. If the resistance of water is ~7K Ohms then the current the components would get is a mere 0.00171428571A or 1.71mA (this is assuming there was no pull-down resistor on the node that supplies the current--if you add that pull-down resistor to that node then this argument for why we "need" a MOSFET goes out the door). This is by I=V/R where V=12V (voltage source of system is 12V) and R is 7,000 Ohms (resistance of tap water). Of course the resistance of the water isn't constant, because the geometry of the water can change depending on where you put the two probes (a greater length between the probes increases the resistance, and more depth, meaning a greater cross-sectional area between the probes, means the resistance decreases), and the resistivity of water can change depending on the number of ions in the water. More ions means the water is more conductive, which means the resistance is lower. Because the resistance of water can fluctuate so much, I used a voltage-driven transistor (i.e., a MOSFET) instead of a BJT, which would get fluctuations in current in the base of the BJT. A BJT, unlike a MOSFET, would need a continuous base current. A MOSFET, however, only needs a continuous gate voltage, and we can control the voltage at the gate note by putting a resistor (of a similar quantity of Ohms) in series with the water resistor.

### P Channel MOSFET (this connects to the green LED)
A P Channel MOSFET is essentially the exact opposite of an N Channel MOSFET. Rather than source going to GND, like in an N Channel MOSFET, the source goes to VCC. Any MOSFET must have a certain threshold met with the voltage in the gate relative to the voltage in the source. Since the source is to VCC, the source is always 12V. The source's voltage must be greater than the gate's voltage, as the Vgs (Voltage at the gain relative to voltage at the source, given by Vgs = Vg-Vs) cannot equal 0 for the MOSFET to be turned on. 

### N Channel MOSFET (connected to buzzer and red LED)
The source goes to GND. For the gate to turn the NMOS on the voltage at the source cannot equal the voltage at the gate (again, Vgs must be non-zero and must also meet a threshold value). When the gate has a voltage being supplied from the water and VCC the drain and source are now conductive, and the buzzer and red LED are now powered as there's VCC going through the anode of the LED and buzzer and GND going to the cathode of the buzzer and LED. 

### N Channel MOSFET (connected to ESP32) and J3 Explanation
The source goes to GND. When the gate has 0V due to a lack of water touching it the ESP32 is reading 3.3V (which is supplied from the ESP32). This is because there's a pull-up resistor connecting the ESP32 3.3V to a GPIO pin (in J3). Because it's a pull-up resistor, the automatic voltage would be on (as it's being pulled up) rather than 0 (unlike a pull-down resistor, which is what is used for the gate that connects to each MOSFET). The water signal reads 1 (for on) as there's no way to GND (the circuit is incomplete). Once the gate has a voltage the water signal now has a way to GND, so it reads GND or 0 (for off). The ESP32 must have the same GND reference as the GND across the PCB, this is because "ground" is just a reference point of where electrical potential energy begins (sort of like how height=0 in physics 1 isn't an absolute concept but rather just a starting reference point) and not some absolute value. If they aren't connected to the same GND, like how pin 3 of J3 allows, then they would not have the same reference point. 

### Jack DC
The power is being supplied in the Jack DC by the house. It is sent through a USB. This uses 12V DC.

### Capacitor
A capacitor stores energy in an electric field. The capacitor is for supply decoupling, which means maintaining the supply voltage when the circuit abruptly changes what it's doing. The circuit is unideal; suppose the gate node has a voltage and the MOSFET connected to the red LED and buzzer turns on. That could cause the buzzer to "demand" 100mA, and since the circuit is unideal there could be a resistance of roughly 0.5 Ohms. Using Ohm's law you obtain V=0.1*0.5, which is 11.95. The capacitor provides current to the component that needs it, which in this case is the active buzzer. That makes it so that the supply voltage, 12V, remains consistent and doesn't undergo rapid flunctuations. 0.1 µF was used as it's a standard value used for decoupling.

## Solving Issues As They Came Up
### The Entire PCB Didn't Work at First
When I first completed the PCB the entire thing didn't work. I knew it didn't work because simply plugging in the USB didn't power the green LED, and putting the J2 connector's wires into water likewise didn't power the red LED or active buzzer (this observation ruled out the possibility of the issue _only_ being caused by the PMOS, which is what my original thought was). One thing I realized was that instead of soldering everything and then testing the PCB only after soldering each part, I could've instead soldered certain areas first and tested it component by component. Doing things this way would've been much easier to troubleshoot problems as they come up. In order to figure out that the Jack DC wasn't properly soldered I used a multimeter to measure the voltage difference across the Jack DC when the Jack DC was plugged in; it became immediately apparent that the Jack DC was at fault for at least _some_ of the issues (how I mistakingly soldered the MOSFETs would be to thanks for there being more than just this problem at first). The multimeter read sporadic voltage values, not 12V, so there was an obvious issue. When I further tinkered with the Jack DC by just applying a small force back and forth the Jack DC was moving back and forth. This gave two clear red flags.

### Blobbing Parts
When I first soldered several parts I accidentally blobbed certain parts at the bottom, so there was a massive spherical-ish blob from soldering that gave the illusion on the components of the PCB that the components weren't moving. However, the solder never actually got into the hole of the footprint to allow the component to solder to the PCB. This became immediately apparent with the Jack DC (J1) when I ran a multimeter across it (using DC setting on 20) to access the voltage drop across the DC Jack and the DC Jack, even when plugged in, had sporadic voltages. This explained why literally everything refused to work, from the green LED to the active buzzer. I got the soldering iron out and connected the DC Jack such that the DC Jack no longer moved when pushed or pulled (this is a great way to see if you actually soldered components correctly!).

### Troubleshooting the Wrong Part
Prior to discovering the issue was the DC Jack, which if unfixed the entire project wouldn't work as there wouldn't be a power supply, I suspected it was the PMOS that was malfunctioning. This was my third PMOS when I was fixing it, so two PMOSs where destroyed in the process of making this project. The reason why I suspected it was the PMOS is because the PMOS would shift when any force was applied to it, and the PMOS is integral to supplying voltage to the green LED when water isn't touching both wires of the connectors.

### Tactics for Resolving Issues
I identified a strategy for finding out where soldering mistakes where made. If you press down on components necessary for a specific part to power up and the part suddenly starts to work, then failing to properly solder that component is almost certainly the issue. This is how I identified almost all of the hardware issues. For instance, there was an issue with the green LED not powering up even after fixing the DC Jack. Once I (accidentally) pressed down on the resistor that the green LED uses to prevent the LED from burning up the LED magically started to work. I then soldered the resistor connected to the green LED once again. This also worked for a MOSFET that wasn't supplying power to the buzzer or red LED. This time, however, was much more intentional. What I did was press down on all of the components, so I tried pressing down on the resistor the red LED uses, which didn't work. I tried pressing down on the red LED and buzzer as well, which didn't work. Finally I tried pressing down on the MOSFET that each of those components rely on for power, and that was the only thing that magically allowed the other components to turn on. I then resoldered the MOSFET connected to the red LED, active buzzer, and resistor for the red LED. It worked like a charm.

### Green LED remains on while water is being detected
One issue is the green LED remains on while the water is being detected unless the two wires for J2 connect each other. To resolve this I used a multimeter to determine the resistance the water has, supposing the water has a very high resistance then even 10M Ohms will fail to bias the voltage drop across that resistor. Once tested, however, the multimeter read 011 for the 2000K ohm range. Meaning the resistance of the water is roughly 11k Ohm's, much less than 10M Ohm's. This issue, however, has since been resolved by resoldering the PMOS that controls the green LED. This issue comes back up much later on, and is addressed later on--I'm trying to keep these issues in chronological order.

## How I finally figured out the resistance of the water

When I tried to use the multimeter to measure the resistance of the water directly (by this I mean I put the probes of the multimeter directly into water and tried varying the depth and length of said probes as the resistance of the water can easily change based on these factors), the resistance was all over the place. The resistance was 180K Ohms, 20K Ohms, etc. This change happened as I changed the prefix on the multimeter from 20 Ohms all the way to 2000k Ohms; the resistance magically jumped. Initially, I didn't know why this was happening, but after researching the reason it became much more apparent: whenever you measure the resistance of water using a multimeter the resistance is only being indirectly measured. The multimeter sends out a current and a voltage and uses Ohm's law to calculate the resistance. Whenever the multimeter sends out said current and voltage that can easily alter the water's actual resistance when that test current and test voltage isn't present. 

So, I measured the resistance a different way that doesn't allow the test voltage to alter what is read for the resistance of the water. I used the voltage divider equation with Vm = Vg (voltage at the gate node). I solved for R1, which in this case R1=Rwater_resistance (R1=the resistance of the water). R2=Rpull_down_resistor (R2=the resistance of the pull-down resistor). Then I solved for R1:

R1=R2(Vs/Vm-1)

The pull-down's resistance was 10K Ohms when I measured Vm (or just Vg). Vg was measured as 7V, and Vs is always 12V. So you'd get:

R1=10,000(12/7-1)

This gives R1=7,142 Ohms. 

WARNING: The resistance of the water is highly variable due to the equation R=pL/A, so take this with some grain of salt. This was, however, across an average of 3 separate probe placements, so this is somewhat more accurate than just one measurement.

### Red LED and Buzzer are on even when no water is being detected -- my biggest technical hurdle with the PCB

Here's a video of the issue:

https://youtube.com/shorts/DKw-fuo1TAE?feature=share


The red LED and buzzer started turning on when no water was being detected by the wires (even while the green LED was still on). Initially, I thought it was because the connector had a solder bridge, which would make it so that the red LED and buzzer would turn on. After resoldering the connector that connects to the two water probes, nothing changed. I realized it was possible that the flux on the bottom of the PCB had made it so that several components (i.e., the MOSFET that turns on the red led and buzzer) were turning on that shouldn't have been turning on. Flux, although it isn't super conductive, is at least slightly conductive. This small amount of conductivity can make it so that a gate of a MOSFET switches on as NMOS gates do not require much voltage to turn on. So then why doesn't the green LED switch off then? Because a PMOS's gate, when it hovers around roughly 0V, can still be on as the voltage at the gate relative to the voltage at the source difference can still be massive whereas for the NMOS the voltage difference between gate and source is just enough to have the NMOS be powered. Think an example: the PMOS voltage at gate relative to voltage at source could be -11V (supposing the gate only has 1V across it), which is a lot. While the PMOS has -11V the NMOS has 1 across it, which is enough to turn on the MOSFET.

Another issue is the 10M Ohm Resistor is so high that any residue (dirt, flux, etc.) on the connector could easily make the J2 connector conduct such that the gate turns on (as a very small voltage can "overpower" the resistor--the voltage drop across the resistor is incredibly large due to how many ohms it is.) The 10M Ohm resistor, therefore, is a weak pull-down resistor. In essence this means the initial voltage across the resistor is very high, so there's less of a safety net from flux turning on the gate node when the flux conducts between parts that can mess up how the board functions. I replaced the 10M ohm resistor with a 10K ohm resistor, plugged in the DC cable, and the problem persists (when I originally wrote this sentence that's what I thought I did, but I accidentally replaced the pull-up resistor for the ESP32 with a resistor of the exact same value. Of course nothing happened). Once I ACTUALLY resolved the issue by replacing R1's footrpint with a 10K Ohm resistor the exact OPPOSITE problem occurred, and that's currently how the project is as I have yet to replace R1 with a 100K Ohm resistor. Let's think of an example that explains why having an extremely weak pull-down can cause issues: if the current leakage from flux or residue on the bottom of the board was a mere 0.5µA then, using Ohm's law, that would cause a voltage of 5V (V=IR-->I=0.0000005 --> R=10,000,000 therefore V=5V). For clarity by "weak pull-down" I mean a pull-down resistor with a relatively high resistance such that the pull-down is poorer at keeping the node it's attached to to 0V.

Here's a picture of me resoldering the 10K Ohm resistor to R4 instead of R1;I replaced the original pull-up resistance for the ESP32 value with what it originally was.

<img width="3024" height="4032" alt="IMG_3819" src="https://github.com/user-attachments/assets/c9049f94-9142-46cf-a0ab-48e5929c3266" />

### Red LED Won't Turn on After Finally Fixing Last Issue

Now that I fixed the problem of the red LED and active buzzer turning on when the probes aren't in water, the red LED won't work at all. I do not think this is because I changed the resistance of R1 to 100k Ohms, as the active buzzer is still on when the probes are in the water. I initially thought it was a soldering issue, so I resoldered the red LED. Nothing changed. Then I ran a multimeter across the red LED, and it read 11.8V (this is definitely not normal!). I believe the red LED may need to be replaced. After I replaced the red LED, making sure that the polarity was taken into account when soldering the new LED, the issue resolved. The PCB now works.

### The Exact Opposite Problem as the Red LED and Active Buzzer turning on

Now there's the exact opposite problem of the Red LED and Active Buzzer turning on: the green LED turns on even when the probes are touching the water. I checked the datasheets for the PMOS and NMOS I'm working with: the PMOS (IRF9540) turns on when Vgs=-2 to -4V, and the NMOS (RFP30N06LE) turns on when Vgs=+1 to +2V. I suspect the green LED is remaining on when the probes are in the water because pull-down for the gate node is too strong; in other words, the opposite problem is caused by the opposite issue. That makes intuitive sense. But prior to destroying yet another resistor for the pull-down (this will be the fourth resistor destroyed as a result of my carelessness in accidentally replacing R4 rather than R1; I should've checked the schematic prior to making any hardware changes!), I think I should make some measurements using the multimeter. I'm going to measure the Vgs for the PMOS first, because the math I did suggests that the PMOS should be off. Here's that math:

Using the voltage divider equation and R1=7,126 Ohms (R1=Rwater_resistance), R2=100k Ohms (R2=Rpull_down_resistance) and Vs=12V (the source is always 12V) we get:
Vg=12(100)/(100+7)=11.21V
Vgs=11.21V-12V=-0.79V

But -0.79V isn't enough for the PMOS to turn on. I'm first going to measure the Vgs for the PMOS; I suspect that it will almost certainly be above -2V. Then, I will measure anything that could explain why that is. The most likely explanation is a component is varying (e.g., the pull-down resistor's resistance isn't quite 100k Ohms), or that the water's resistance is higher than initially calculated, which would mean the Vg is much lower than calculated using the voltage divider equation.

While I would do this first, I have another hypothesis: the wires attached to the connector have "gone bad"; in other words, the wires need to be replaced. This hypothesis, while less plausible than the first hypothesis, is much easier to resolve and would likewise explain why this issue is happening. Why does it make sense? If the probes aren't conducting properly because the wires have been worn out then the Jack DC's supplied current of 12V isn't really getting sent to the gate node as 12V; in other words Vs could be much lower than anticipated. If this is true, then using the voltage divider equation:

Vm=Vs*R2/(R1+R2) --> Vs would be much lower than anticipated, therefore Vm, which equals Vg, would likewise be much lower than anticipated.

The issue resolved itself after changing the wires; this worked for reasons stated above.


## Calculating the Resistances
### For the LEDs (1K Ohm Resistors)
The resistance for the LEDs, 1k ohms, was calculated using ohm's law and KVL, which states that the summation of voltages across a closed loop is equal to 0. The summation of voltages here is Voltage Supply (Vs or 12 V) + Voltage Drop across the LED (2.2 V for a SunFounder LED that was used) + Voltage drop across the resistor. This gives the equation 0=Vs-VLED-VResistor or just 0=12V-2.2V-VResistor. Giving VResistor=9.8V (the voltage drop across the resistor is 9.8V). The maximum current that a SunFounder LED can take is 10mA, which is 0.01A. Using Ohm's law (V=IR) we algebraically get R=V/I. Then we get R=9.8V/0.01A or just R=980 Ohms. The closest resistor I had to 980 Ohms was a 1k Ohm resistor, so that resistor was used.
### For the pull-down resistor (10M Ohm resistor)
The 10M ohm resistor exists for two reasons. Reason one:  Without a resistance the gate would "float", which is essentially whenever a MOSFET gate doesn't have a reference to GND. Because gates often act as capacitors insofar as the gate can store energy, this can cause the gate to act unpredictably (which would cause the buzzer to sometimes buzz and the red led to sometimes be on). 

#### The text below is my first rendition of the project:
Reason two: water has its own resistance of roughly 500k-1M ohms. So the 10M ohm resistor is put there to act as a voltage divider (which is when two resistors are in series). If the 10M resistor were much less, say 1k ohms, then this could make it so that the PCB no longer accurately detects whether or not water is present at the connector. This is because the voltage drop across a resistor of much less ohms would be far less, and if the voltage drop across the resistor were much less than the gate would have much less voltage supplied to it (in turn this could cause the gate to not turn on as MOSFETs have to have a certain threshold voltage met at the gate). 
#### Updated Explanation (After changing the resistance of the resistor on R1)
The water has a resistance of 11K Ohms (at least the tap water I used)--again this resistance is highly variable due to ions in the water, the length between the probes in the water, and the depth of the probes in the water (which is why we use a MOSFET instead of a BJT). The water's resistance acts in series with the pull-down attached to the node connects to the gates of each MOSFET and supplies voltage (or no voltage) to said MOSFETs. We want the resistance of this pull-down to neither be too weak nor too strong. If the pull-down is too weak then issues like before can come up where even a fraction of a microampere of current leakage, caused by flux on the bottom of the PCB, can cause the voltage at the gate to be as high as 5V. On the other hand, if the pull-down is too strong then the MOSFETs won't be able to reliably detect if water is touching the probes. This is because the resistance would be "too good" at its job of bringing the gate node to GND, hence the word "strong" in "strong pull-down". So we need to pick something in between, since a 10K Ohm resistor was tested on R1 and caused the reverse issue (the green LED remains on even when the probes are submerged in water), we need a resistor of roughly 100K Ohms or more (in between 10K and 10M Ohms). The specific resistance of said resistor will be tested soon.
### For the pull-up resistor (The 10k Ohm Resistor)
This resistor's value easily could've been a different value (e.g., 4.7k Ohms). However, the resistor's value here should be within a certain range. Let's think about what this circuit is doing. A GPIO pin from the ESP32 is sending a 3.3V to pin 1 of J3. Pin 1 sends that 3.3V to pin 2 of J2, and J2 is also connected to the drain of an NMOS. Why do we even need a resistor? Suppose that the NMOS turns on, as it is of course supposed to, then pin 2 of J3 will go straight to gnd (from drain-->source) when the gate turns on. This will cause a short circuit as the 3.3V will go from basically 0 resistance to gnd, and as the resistance approaches 0 the current increases exponentially (due to I=V/R). But why did I choose 10k? Two reasons: 1. we don't want to waste current from the ESP32 when the NMOS turns on. If the resistance were much smaller then the current supplied from the ESP32 would be much higher (again Ohm's law). We don't want to waste energy, and unnecessarily high currents will waste energy due to energy being P*time=energy and P=VI (so as I increases power increases and as power increases energy increases). For practical purposes we should conserve energy. So if this is the case then why not just have a super high resistor (like why don't not use another 10M Ohm resistor instead of the 10k Ohm resistor?). Reason 2 explains this. 2. if the resistance of the resistor between pin 1 and pin 2 of J3 is too high then the voltage drop across the resistor will be incredibly high (it isn't possible for me to calculate this as I don't have the current for extremely high values. In fact I tried to calculate it and got the voltage drop as 50V when 5microamperes = I, which is standard for how much current ESP32s leak, and R=10M ohms. This voltage drop is clearly impossible, so it seems the manufacturer said that ESP32s generally leak 5 microamps because that's true for a set range of values, which didn't encompass very large resistances like 10M ohms). If you use the 10K ohm resistor then you will have a voltage drop (Vd=Vi-Vf) of 0.05. So the voltage being supplied to the water sensor will be 3.25V. This is perfectly sufficient for the ESP32 to use to connect to pin 2 (the water sensor) and use the pin as a digital input. If the digital input reads 1 then the water is connecting a wire connected to J2. If the digital input reads 0 then the water isn't connecting a wire connected to J2. For clarity: I have yet to code the ESP32 for this project, and I will do this soon! Clarification on voltage drop for the 10K Ohm resistor: this is only the voltage drop if the manufacture's specification that the amount of Amps the ESP32 leaks is roughly 5μA; in reality the voltage drop could be significantly different.

# ESP-32 Part

## Verifying continuity for the J3 Connector and MOSFET
I used a multimeter, put it in continuity mode, and verified the continuity between the Jack DC's GND and the GND in the j3 Connector. The multimeter beeped, so there is continuity. I then checked continuity between the drain of NMOS 2 and pin 2 of the J3 connector; again, it beeped. So the J3 doesn't need to be resoldered. Then I checked for continuity between pin 3 of NMOS 2 and GND of J3 (i.e., pin 1); again, it beeped. Finally, I checked for continuity between the NMOS gate pin (pin 1 of that NMOS) and J2 pin 2, which is connected to the gate node.

What is continuity and why did I do this? Continuity means there's a low resistance path between two points. I did this because I didn't want to resolder NMOS 2 or J3, and these continuity checks verify I don't have to. You ONLY have to resolder the board if some points lack continuity, but these points DON'T lack continuity, so the board doesn't have to be resoldered.

## Coding

I am currently coding the ESP32 in C using ESP-IDF, and I will add the .c file alongside extensive commentary in said .c file once I have finished. 
