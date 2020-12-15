# TrafficSignal-AssemblyLanguage

Components: 
        - Red, Yellow, Green LED
        - Five Wired
        - Push Button Switch
        - 4 resistors (3 330 Ohms, 1 10k Ohms)
        
Prior To Pressing Switch: (Daytime)
        - Green LED lasts 6 seconds
        - Yellow LED lasts 2 seconds
        - Red LED lasts 4 seconds
After Switch Pressed: (Nighttime)
        - Green LED lasts 12 seconds
        - Yellow LED lasts 4 seconds
        - Red LED lasts 4 seconds

Code Explanation:
 
	My code begins with me configuring the interrupt vector table so I would later be able to conduct an external interrupt request. In my main code, I initialize the stack pointer, initialize I/O ports, configure interrupt for push button, configure interrupt sense control bits and more. Then I enable global interrupts using SEI. Then I have the daytime logic of my code that include nested loops. I have a red and green LED loop that 0just provide the delay needed between each other and a simple SBI and CBI for a yellow LED. This overall “traffic_lp”, is an infinite loop that can only be changed when there is an external interrupt. For my external interrupt, I use “SBIS as my condition to determine which LED is currently on at the time of pressing the button switch. When that occurs, I call for a second delay because I do not want the light to switch colors too fast which could cause an accident in real life. After I call the timer delay, I close the current LED and “rjmp” to the next color in line. For example, if you pressed the push button switch when it is on green. No matter how much time it had before turning yellow, it will remain on for 2 seconds, then turn off. After turning off it will jump to a “yellow_lp_night_time” subroutine where is will then go through the LED’s just as if it was nighttime.
	As for my timer, I chose to do just a regular timer with out an interrupt. I did include an external interrupt as mentioned in the paragraph earlier but did not understand how timer interrupts truly worked. I decided to have a 2 second delay as my base length by using timer1 ctc. First I set the counter to 0 ( TCNT1). Then using the excel sheet I determined what to set my timer high and low byte to. Following that I set the mode in timer counter control register A and B. Since I used a 1024 prescaler, I put 1’s in WGM12, CS12, and CS10. Then I watched for OCF1A in TIFR1, cleared the clock select and mode, and wrote 1 to OCF1A to clear its flag.
