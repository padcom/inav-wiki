# The basic of getting iNav working on airplane

* For more detailed Howto and for CC3D see [this link.](https://github.com/iNavFlight/inav/wiki/XHowto:-CC3D-flight-controller,-minimOSD-and-GPS-for-fixed-wing#howto-setup-inav-for-fixed-wing)

* One limitation of iNav(And all other Cleanflight based) is it cannot be flown longer than 1 hour and 15 minutes. This is expected to be corrected by iNav 1.6

### Step 1, getting your flight controller ready.

* Flash newest iNav. _(Warning: iNav default mixer is Quadcopter, so it sends out high frequency signal that is meant for an ESC. This can damage servo if servos are powered up. If flashing with an FTID adapter the 5v line is powered up, and its required to disconnect any servo.)_

* Do entire [sensor calibration](https://github.com/iNavFlight/inav/wiki/4.-Sensor-calibration). (Level should be the angle of the plane itself when flying straight)

* Select your Mixer. (Airplane, Flying Wing or custom airplane)


### Step 2, hooking everything up.

* Servo and ESC/MOTOR. 

Output 1 Motor/ESC

Output 2 Empty

Output 3 Elevator

Output 4 Aileron

Output 5 Aileron

Output 6 Rudder

* If using GPS connect it to UART 2.
* If using Sbus connect it to UART 3. (Note this requires newer board like spracing f3)
* If using regular PPM connect it to IO 1 pin 1.
* If using telemetry connect it with softserial.

### Step 3, Setting up your remote, endpoints and reverse of servos.

* Setup receiver/transmitter according to what you are using.
* If using GPS setup UART2 for GPS at baud 57500 and enable GPS in configurations.
* Your TX should use NO mixing at all, check that when moving the sticks, the right channels moves in the receiver window. Also everything should be centered at 1500us, and full stick movement should be 1000-2000us. Use subtrim and travel range on TX to set this up. 

The correct way is:

Throttle stick push away - increased value

Yaw (Rudder) stick right - increased value

Pitch (Elevator) stick push away - increased value

Roll (Ailerons) stick right - increased value

* Next up is making sure the servos are moving the correct way when moving the sticks on the TX. If something is not right, use the Servo tab to get them center, adjust movement, range and reverse of servo.

Servo 2: Elevator

Servo 3&4: Ailerons

Servo 5: Rudder

(Note: In the Servos tab servos are counted from 0-7 while in the Motors tab they run from 1-8.)

### Step 4, Replace default PIDs.

* Default PIDs in iNav are mainly for multirotors. Find some PIDs [here](https://github.com/iNavFlight/inav/wiki/7.-PID-tuning-and-PID-examples#fixed-wing) to use instead and tune from there.
* If you are flying a plane with rudder, use "set i_yaw = 0".
* If your plane over corrects when RTH is engaged, try increasing "nav_navr_p" and/or increasing "nav_navr_i". Good values to start: "set nav_navr_p = 50"; "set nav_navr_i = 5". Also you can lower "nav_navr_d". The behaviour of the plane is very different with or w/o wind, so it is necessary to test and tweak parameters in both scenarios.
* In "Angle" Mode you don't need high steering surface deflection. Set "p_roll" and "p_pitch" just as high that you have 25% of full travel. That is a good point to start tweaking the gains from. If you use to high values, the plane will oscillate.
* A good tool to optimize your system is the blackbox logger. Some FCs have it and in others it is an accessory. After the flight you can analyze in 'Chrome blackbox explorer' the behaviour of many parameters of your plane and later solve issues or ask for help in the forums. With a blackbox log you can also replay all your flight in linux with [mwp tools](https://github.com/stronnag/mwptools).

//TODO: explain how rates work with airplane servos.

### Step 5, optional but recommended:

* Use Airmode mode to get full stabilitation and servo throw with no throttle applied.
* [Setting up failsafe with return to home.](https://github.com/iNavFlight/inav/wiki/9.-Failsafe#setting-up-failsafe-with-return-to-home)
* Disable compass (if present) so it uses GPS heading instead. Cli command `set mag_hardware = 1`
* Disable barometer (if present) (`set baro_hardware = 1`) due to critical bug that can make airplane dive to ground. Issue [543](https://github.com/iNavFlight/inav/pull/543)


### Optional / Guides related to Fixed Wing:

* Using a seperate BEC for servos. [Example](http://www.rcgroups.com/forums/showpost.php?p=34254665&postcount=4006)

* [Using a minimosd](https://github.com/iNavFlight/inav/wiki/Howto:-CC3D-flight-controller,-minimOSD-and-GPS-for-fixed-wing#osd-setup)

* Setting up flaps in iNav. [Youtube Link](https://www.youtube.com/watch?v=Ui7Y0UVedDQ)

* Howto in flight trim servos. [Aileron example at rcgroups.com](http://www.rcgroups.com/forums/showpost.php?p=35059861&postcount=6741) [Fixed wing example](https://www.rcgroups.com/forums/showpost.php?p=36039077&postcount=8732)