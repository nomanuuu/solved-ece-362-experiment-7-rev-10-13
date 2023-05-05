Download Link: https://assignmentchef.com/product/solved-ece-362-experiment-7-rev-10-13
<br>
ECE 362 – Experiment 7 Rev 10/13

Instructional Objectives:· To illustrate I/O expansion using a shift register· To illustrate an ATD application· To illustrate an RTI applicationParts Required:· One GAL22V10 (from ECE 270 parts kit, also available from Instrument Room)· Two 10 KW potentiometers (from ECE 270 parts kit, also available from Instrument Room)· Four RED, four YELLOW, and two GREEN LEDs (from ECE 270 parts kit, also availablefrom Instrument Room)· Six 150 W resistors (from ECE 270 parts kit, also available from Instrument Room)· Breadboard and wire (from ECE 270 parts kit)· BONUS CREDIT OPTION: various diodes, resistors, and capacitors along with aconnector/cable compatible with your favorite “personal listening device”Prelab Preparation:· Read this document in its entirety· Review the material on the RTI and ATD subsystems· Wire and test GAL22V10 shift register/bar graph displays· Read the Programming Your Microcontroller in C documentOverviewThe objective of this lab is to create a two-channel LED bar-graph display (BGD) consisting of a 9S12C32 development kit interfaced to two potentiometers (referenced to 5 VDC). The BGD will sample the two analog input channels every 0.1 second (approx.) and display the converted values on two 5-segment LED displays, respectively.The left pushbutton on the docking board will be used to “freeze” (stop) the BGD, while the right pushbutton will be used to start/ resume the BGD. The left LED on the docking board should be on when the BGD is stopped, while the right LED should be on when the BGD is running.A set of five LEDs (two RED, two YELLOW, and one GREEN) will be used to indicate the approximate range of the voltage input on each channel (0 and 1), together creating an LED “bar graph” display (BGD). The GREEN LED for a given channel should be illuminated if the input voltage on that channel is greater than or equal to THRESH1. The“bottom” YELLOW LED for a given channel should be illuminated if the input voltage on that channel is greater than or equal to THRESH2. The “top” YELLOW LED for a given channel should be illuminated if the input voltage on that channel is greater than or equal to THRESH3. The “bottom” RED LED for a given channel should be illuminatedif the input voltage on that channel is greater than or equal to THRESH4. Finally, the “top” RED LED for a given channel should be illuminated if the input voltage on that channel is greater than or equal to THRESH5. The threshold values (THRESH1 – THRESH5) are constants that you will define.NOTE: All “C” code for this experiment must be written and debugged during your scheduledlab period.ECE 362 – Experiment 7 Rev 10/13

Step 1. InterfacingA 10-bit shift register, realized using a GAL22V10, will be used to interface the two LED bar graph displays to your microcontroller kit, as illustrated in Figure 1. The basic underlying concept was introduced in problems 4, 5, and 6 of the Module 2 homework set. Here, however, you will be configuring the GAL22V10 as a 10-bit shift register (instead of 8-bit), and driving individual LEDs (instead of a 7-segment display). Note that the GAL22V10 output pins shouldbe configured as active low, i.e., to sink current when the corresponding shift register bit is “1”. Use pin 3 of Port T (“PT3”) to provide the shift data to your GAL22V10, and pin 4 of Port T (“PT4”) to provide the shift clocking signal. Write an ABEL program that realizes a 10-bit shift register for your GAL22V10 (this should be a simple adaptation of the code shown at the bottom of page 5 on the Module 2 homework solution). Test the operation of your shift register using DIP switches before interfacing it to your microcontroller kit, and demonstrate your hardwarefunctionality to your lab T.A. before you begin coding.Figure 1. Interface of 10KW potentiometers and LEDs to 9S12C32 Development Kit.I/CLK

NOTE: The locally-available GREEN and YELLOW LEDs are NON-RESISTOR, while theRED LEDs have an integrated current limiting resistor. Use a 150W current limitingresistor in series with each GREEN/YELLOW LED (anode connect to +5 VDC, cathodeconnected to GAL22V10 output pin).ECE 362 – Experiment 7 Rev 10/13

Step 2. SoftwareThe “C” code for this application must be written, debugged, and demonstrated during yourscheduled lab period. The software modules needed are as follows:1. Peripheral device initialization (RTI and ATD) – done before main loop commences.2. Main program that checks left/right pushbutton flags as well as the tenths (of second) flagand takes appropriate actions – this should be performed in a continuous non-terminatingloop, as outlined below:If the “tenth second” flag is set, then– clear the “tenth second” flag– if “run/stop” flag is set, then– initiate ATD coversion sequence– apply thresholds to converted values– determine 5-bit bar graph bit settings for each input channel– transmit 10-bit data to external shift register– endifEndifIf the left pushbutton (“stop BGD”) flag is set, then:– clear the left pushbutton flag– clear the “run/stop” flag (“freeze” BGD)– turn on left LED/turn off right LED (on docking module)EndifIf the right pushbutton (“start BGD”) flag is set, then– clear the right pushbutton flag– set the “run/stop” flag (enable BGD updates)– turn off left LED/turn on right LED (on docking module)Endif3. RTI interrupt service routine that detects pushbutton presses and sets left/right pushbuttonflags (similar to Lab 6); also, determines when 0.1 second of RTI interrupts haveoccurred (track in global variable RTICNT), and sets “tenths” flag accordingly. The RTIinterrupt rate should be on the order of 5-10 ms (approx.) to ensure accurate sampling ofpushbuttons.4. ATD device driver that performs a conversion sequence on input channels 0 and 1 (i.e.,sequence length should be 2).5. Shift register device driver, similar to the code outlined in problem 5 of the Module 2homework – should shift out 10 bits of data to the GAL22V10 shift register/LED BGD.Complete the “C” skeleton file provided on the course website during your scheduled labperiod. Note that the “finished product” should work in a “turn key” fashion, i.e., yourapplication code should be stored in flash memory and begin running upon power-on or reset.Demonstrate your completed system to your lab T.A.Step 3. SubmissionZip the ABEL program you wrote for Part 1 (.abl file only) together with the C code you wrotefor Part 2 (.c file only) and submit your solution file on-line after demonstrating it to your labT.A. (but before leaving lab). Be sure identifying information (i.e., name, class number, and labdivision) is included in the files you submit – credit will not be awarded if identifyinginformation is omitted.ECE 362 – Experiment 7 Rev 10/13

Step 4. Bonus CreditTo make the two-channel BGD more interesting (“socially redeeming”), you can convert it into a stereo VU (“volume units”) meter for use with your favorite “low information music player” (LIMP) with the addition of some simple analog circuitry. Basically, the AC waveforms produced when playing music (fed by your LIMP to low-impedance “earbuds”) need to be rectified and low-pass filtered in order to create a useful/meaningful VU meter. Also, the voltagethresholds used by your C program need to be adjusted based on the realization that your typicalLIMP produces at most a 1-volt PP (peak-to-peak) AC output.One possible realization is illustrated in Figure 2. Here, the resistor Ra and capacitor C determinethe “attack time” (quickness of response time constant) to the fluctuating audio signal and Rddetermines the “decay time” (persistence of display to an audio impulse). Typically, Rd issignificantly larger than Ra (i.e., attack time has a smaller time constant than decay time), butthese are often based on user preference (so some experimentation may be necessary). Use your10KW potentiometers to adjust the attack time (Ra) for each channel, and resistors about an orderof magnitude greater for the decay time (Rd). Generic PN junction diodes can be used as therectifiers. The value of C should be on the order of several microfarads (e.g., 10 μF).You can test your VU input circuit in advance by connecting its outputs (PAD0 and PAD1) to anoscilloscope and playing various genre of music on your LIMP and watching the attack/decay ofthe corresponding DC envelope (this also provides an opportunity to determine the peak DCvoltage produced by your circuit, so that you can determine the appropriate BGD voltagethresholds). Note: “High information” VU meters are calibrated in decibels. The maximumvoltage produced by your circuit should correspond to illuminating the top RED LED (alongwith all those below it) and be designated “0 dB”. This will serve as the reference voltage forcalculating the thresholds at which the “lower” LEDs illuminate (V0dB). So, for example, if eachLED in the BGD represents 3 dB VU steps, the corresponding threshold voltage for the“-3 dB RED LED” can be calculated based on solving the equation:-3 dB = 20 log10 (Vthresh-3dB / V 0dB)The remaining thresholds (-6 dB, -9 dB, and -12 dB) can be calculated in a similar fashion.Figure 2. Circuit to Convert BGD into a Stereo VU Meter.RdRdRaRaPAD11 PAD023PHONEJACK STEREO CC