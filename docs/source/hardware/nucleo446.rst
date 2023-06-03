Nucleo64 F446 with GRBL 3.x CNC shield
====================

Nucleo64 F446 with GRBL/protoneer 3.x CNC shield with Remora firmware. 



	
.. image:: ../_static/nucleo446_cnc.png
    :align: center

Nucleo64-F446RE with Protoneer CNC shield 3.xx

This document covers running Remora firmware on LinuxCNC with a fixed configuration, and information reguarding the SD card configuration firmware. Information reguarding the configuration of Remora via SD card is covered in detail in the main Remora Documents. 
This page is specific to the Nucleo64 F446RE hardware. The Firmware is specific to the Nucleo board running the STM32F446RE mcu with a Arduino CNC shield v3.xx as its default configuration.  

The config includes : 

* 4x stepgens for XYZA axis 
* 3x lower resolution encoders with channels A/B
* 1x Highspeed encoder for spindle or high resolution encoder
* 7x inputs for limit switches and buttons
* 2x outputs  
* 1x PWM
* PRU Reset pin



Firmware and Config
-------------------

This firmware is specific to the Nucleo64 F446RE in combination with the classic grbl cnc shield. There are 2 versions of the firmware. One is a static configuration,
the pinout cannot be changed unless recompiled in the firmware. The second is the SD card configuration firmware, the pinout is configured using an SD card which needs to be connected to the Nucleo SPI pins.
The Stepgens and limit switches are configured to match the pins on the grbl cnc shield. Hardware related configuration for the grbl shield may be required for some jumper related things. Pins not found on the grbl shield are found on the Nucleo Morpho headers. You do not need to use the cnc shield, but this project is default configured for the cnc shield. 


+--------+------------------------------+----------------+
| PIN    |   FUNCTION  	 	  	| LinuxCNC PIN   |
+--------+------------------------------+----------------+
| PA_10  |	X AXIS STEP 		| remora.joint.0 |
+--------+------------------------------+----------------+
| PB_4   |	X AXIS DIR  		| remora.joint.0 | 
+--------+------------------------------+----------------+
| PB_3   | 	Y AXIS STEPGEN    	| remora.joint.1 | 
+--------+------------------------------+----------------+
| PB_10  |	Y AXIS DIR    		| remora.joint.1 | 
+--------+------------------------------+----------------+
| PB_5   | 	Z AXIS STEPGEN 		| remora.joint.2 | 
+--------+------------------------------+----------------+
| PA_8   |	Z AXIS DIR     	  	| remora.joint.2 | 
+--------+------------------------------+----------------+
| PA_6   |	A AXIS STEPGEN   	| remora.joint.3 |
+--------+------------------------------+----------------+
| PA_5   |	A AXIS DIR	 	| remora.joint.3 |
+--------+------------------------------+----------------+
| PC_10	 | X AXIS ENCODER CHANNEL A 	| remora.PV.0    | 
+--------+------------------------------+----------------+
| PC_12	 | X AXIS ENCODER CHANNEL B	| remora.PV.0 	 |
+--------+------------------------------+----------------+
| PA_11  | Y AXIS ENCODER CHANNEL A	| remora.PV.1  	 |
+--------+------------------------------+----------------+
| PD_2   | Y AXIS ENCODER CHANNEL B 	| remora.PV.1  	 | 
+--------+------------------------------+----------------+
| PC_2   | Z AXIS ENCODER CHANNEL A 	| remora.PV.2    | 
+--------+------------------------------+----------------+
| PC_3   | Z AXIS ENCODER CHANNEL B 	| remora.PV.2    | 
+--------+------------------------------+----------------+
| PB_8   | QEI  ENCODER CHANNEL A	| remora.PV.5    | 
+--------+------------------------------+----------------+
| PB_9   | QEI ENCODER CHANNEL B	| remora.PV.5    | 
+--------+------------------------------+----------------+
| PA_12  | QEI ENCODER CHANNEL INDEX	| remora.input.15| 
+--------+------------------------------+----------------+
| PC_6   | PWM OUTPUT 			| remora.SP.0  	 | 
+--------+------------------------------+----------------+
| PC_7   | X-LIMIT			| remora.input.0 |
+--------+------------------------------+----------------+
| PB_6   | Y-LIMIT 			| remora.input.1 |
+--------+------------------------------+----------------+
| PA_7   | Z-LIMIT			| remora.input.2 |
+--------+------------------------------+----------------+
| PB_0   | COOLANT			| remora.input.3 |
+--------+------------------------------+----------------+
| PA_0   | ABORT			| remora.input.4 |
+--------+------------------------------+----------------+
| PA_1   | HOLD				| remora.input.5 |
+--------+------------------------------+----------------+
| PA_4   | RESUME			| remora.input.6 |
+--------+------------------------------+----------------+
| PA_9   | STEPPER ENABLE	  	| remora.output.0| 
+--------+------------------------------+----------------+
| PC_8   | OUTPUT 1			| remora.output.1|
+--------+------------------------------+----------------+
| PC_9   | OUTPUT 2			| remora.output.2|
+--------+------------------------------+----------------+


.. image:: ../_static/nucleo446_pinout_shield.png
    :align: center

.. image:: ../_static/nucleo446_cnc_pinout.png
    :align: center



Static Configuration : 
------------------------

The firmware for the Nucleo/CNC shield static configuration is based around the pinout of the CNC shield. This means the setup does not need an SD card to load the configuration, but it also cannot be changed. 
It is a static configuration, so to change any pinout, it would be required to compile firmware with adjustments. That will not be covered in the scope of this documentation at this time.  You do not need to use all the pins but they included in the firmware.

SD Card Configuration : 
--------------------------

The firmware for the  Nucleo/CNC shield SD card configuration is more inline with standard Remora firmware. 
A default configuration based on the CNC shield pinout is provided, but if you do not wish to use the cnc shield, 
you can configure your pinout to be something else. To use this firmware, you are required to attatch an SD card module, wired as shown below. 

Note : SD cards running over SPI can be finicky. It is recomended to use an SD card under 1gb

Note : The Nucleo 446 can tolerate 5v but its IO are 3.3v, It may be required that your SD module some kind of a resistor



Pinout Considerations :
----------------------

* The X+ and X- pins are connected to the same Nucleo IO pin
* The Y+ and Y- pins are connected to the same Nucleo IO pin
* The Z+ and Z- pins are connected to the same Nucleo IO pin
* The SpnEn and SpnDir pins are connected to the same Nucleo IO pin as A axis step and dir
* The E-STOP pin is connected to the mcu reset pin. I do not recoment using it as an estop pin. 

.. image:: ../_static/nucleo446_cnc_pinout2.png
    :align: center

Hardware Pins
-------------
Remora firmware has some features available only on specific hardware pins. These pins can vary between STM32 boards.
If you are using the SD config firmware, you can configure the pins different than the default, but some functions are tied to specific pins.

Available PWM Hardware pins:

-  PA_1 PA_2 PA_3 PA_5 PA_6 PA_7 PA_8  PA_9 PA_10 PA_11 PA_15
- PB_0 PB_1 PB_3 PB_4 PB_5 PB_6 PB_7 PB_8 PB_9 PB_10 PB_11 
- PC_6 PC_7 PC_8 PC_9


Available QEI Encoder Hardware pins:

- PB_8
- PB_9
- PA_12 is used as index


Wiring to SD Module
-------------------

Wiring the SD Card Module requires it share SPI with our RPI communication SPI. 
You can use the bottom side pins on the morpho header to access the SPI pins for the SD card module

+--------+----------+----------------------+-------------+
| PIN    | COLOR    |   FUNCTION  	   | SD card PIN |
+--------+----------+----------------------+-------------+
| PB_15  | RED      | SPI_MOSI   	   | MOSI  	 |
+--------+----------+----------------------+-------------+
| PB_14  | ORANGE   | SPI_MISO  	   | MISO        | 
+--------+----------+----------------------+-------------+
| PB_13  | GREEN    | SPI_SCK		   | SCK         | 
+--------+----------+----------------------+-------------+
| PC_4   | YELLOW   | SPI_SSEL  	   | CS          | 
+--------+----------+----------------------+-------------+
| 5/3.3v | BROWN    | POWER  	           | 5 or 3.3v   | 
+--------+----------+----------------------+-------------+
| GND    | BLACK    | GROUND	   	   | GND         | 
+--------+----------+----------------------+-------------+

.. image:: ../_static/nucleo446_sd.png
    :align: center
Nucleo connected to SD Card Module


Wiring to Raspberry Pi
----------------------

Wiring requires the following components:

* 100mm or shorter Female-Female Dupont ribbon jumper
* 6 way (1x6) Dupont connector
* 8 way (2x4) Dupont connector


+--------+----------+----------------------+-------------+
| PIN    | COLOR    |   FUNCTION  	   | RPI PIN     |
+--------+----------+----------------------+-------------+
| PB_15  | RED      | SPI_MOSI   	   | RPI_PIN_19  |
+--------+----------+----------------------+-------------+
| PB_14  | ORANGE   | SPI_MISO  	   | RPI_PIN_21  | 
+--------+----------+----------------------+-------------+
| PB_13  | GREEN    | SPI_SCK		   | RPI_PIN_23  | 
+--------+----------+----------------------+-------------+
| PB_1   | YELLOW   | SPI_SSEL  	   | RPI_PIN_24  | 
+--------+----------+----------------------+-------------+
| PB_2   | BROWN    | PRU Reset	  	   | RPI_PIN_22  | 
+--------+----------+----------------------+-------------+
| GND    | BLACK    | GROUND	   	   | GND         | 
+--------+----------+----------------------+-------------+
| USB    | 	    | MCU TX to RPI RXD    | USB	 |
+--------+----------+----------------------+-------------+
| USB    | 	    | MCU RX to RPI TXD    | USB	 |
+--------+----------+----------------------+-------------+

.. image:: ../_static/nucleo446_pi.png
    :align: center
Nucleo connected to Raspberry Pi 4
	
.. image:: ../_static/nucleo446_sch.png
    :align: center
Nucleo to Raspberry Pi 4 schmatic

	
To UART from the Raspberry Pi to the Nucleo, you can use the usb port on the Nucleo to RPI usb



