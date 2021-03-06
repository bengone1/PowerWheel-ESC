PowerWheel ESC

*Nano version not tested 
	-it will allow ArduinoDroid app (available on the Playstore) to make Sketch changes to acceleration and speed
        	limiters and upload via your phone

*Brake relay/Resistor function not tested
	

Specs
	-IBT-2 Motor driver capable of 27VDC and 43amps each (according to propaganda) 86Amps * 26V = 2,236Watts but why
		stop there? You can drive a handful of these things.
	-Arduino ProMicro and Nano are both 5v IO but will need a DC-DC buck convertor to input 6VDC to the RAW or VIN
		This allows the Arduino internal regualtor to function and provide 5v output to sensors.
	-Unit designed to be used with a 0-5v Hall pedal/twist grip/potentiometer but can also be used with a standard
		switch pedal to act as a Soft Start system. The throttle values can be adjusted to match many throttle values.
	-Shifter and Brake inputs send a ground signal to the Arduino and only motor and battery wiring needs to be of 
		heavy gauge. Quality CAT5 network cable can be used for the remainder of the wiring to reduce costs.
	
Functions  *changes ONLY need to be made to the Pindefs.h file

	-Throttle Input
		*Calibrated by using the Arduino IDE serial monitor. Leave the motors unplugged or use the "Debug" function 
			to disable them. Watch the Throttle input at OFF and WOT and set your THROTTLE_OFF_VAL to be slightly
			higher than the lowest value seen to allow for a "deadspot". THROTTLE_MAX_VAL set slightly lower than
			max value seen with pedal fully depressed ensuring the pedal does not have to be mashed for max speed.
		
		*Acceleration is done by increasing motor drive(PWM) in steps starting at SPEED_MINIMUM up to SPEED_xxx_MAX 
			setting shifter is set at. 
			-The motor power ramps up from a start value SPEED_MINIMUM. Too low a value and the motor will not 
			have enough power to move generating excess heat and is generally bad for the motors. 
			0=0v, 255=Battery Voltage, 125=Half of Battery.
					
		*Deceleration is performed when the throttle request is reduced by ramping down motor PWM to allow for smooth
			stopping. Adjust for faster decel by increasing the value of DECEL_STEP and slower by decreasing value.
					
	-Brake
		-Activating the brake switch cuts motor power (PWM) and sends ground to 5v coil relays which short 1ohm 100w
			resistors across each motor for braking. Leaving the relays/resistors off just cuts motor power and 
			results in reasonable stopping distances. Possible room for improvement would be to have REN and LEN 
			output being driven LOW to activate relays while allowing a moment for PWM to reach zero. 
		
	 -Shifter
		-Ground from the Arduino passed back to the Arduino inputs from the shifter switches sets speed/direction
			Arduino GND to the center terminal of the Fwd/Rev switch. Rev terminal returns back to Arduino. Fwd 
			position
		
		~If the throttle is lifted and depressed again the ramp up resets from SPEED_MINIMUM to SPEED_xxx_MAX
		
		~If the Shifter is moved from Low to High the motor ramps up to SPEED_HIGH_MAX
		
		~If the Shifter is moved from High to Low the motor ramps down to SPEED_LOW_MAX
		
		~If the shifter is moved from Rev to either forward gear or opposite the motor ramps down per the DECEL_STEP
			to zero then ramps up in the opposite direction.
