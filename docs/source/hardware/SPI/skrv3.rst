Bigtreetech SKR V2.0
====================

Wiring for the SKR boards are very straight forward with all pins directly available on the EXP2 header.

The SKR V2.0 can also power the Raspberry Pi from the TFT connector.

Remora Details
--------------
| **Board:**   BTT SKR3
| **MCU:**	STM32H743, STM32H723
| **Communication:**	SPI
| **Firmware:**	      STM32H743/BTT_723, STM32H723/BTT_723 https://github.com/scottalford75/Remora/tree/main/Firmware/FirmwareBin
| **Firmware Source:**		https://github.com/scottalford75/Remora/tree/main/Firmware/FirmwareSource/Remora-OS6
| **LinuxCNC Driver:**      "remora-spi"
| **PRU Base Frequency:** 40000 - 80000
| **Supported Modules:**    


Firmware
-------------------
The SKR V2.0 has 2 different versions, earlier versions used a STM32H743 and newer versions use the STM32H723.
Hardware wise they are the same, but they require diferent firmware. Note which board version you have and choose
the matching firmware.

- Firmware for the STM32H743 version is STM32H743/BTT_743
- Firmware for the STM32H723 version is STM32H723/BTT_723

In your .hal file, you will need to load the Remora driver

.. code-block::

		loadrt remora-spi

Config
-------
A sample config.txt for the BTT SKR3 is located in the Remora repo under FIrmware/ConfigSamples/SKR3

The config must be named config.txt and must be stored on the SD card. It must remain in the board. 

The config file for the SKR2 v2 will be the same for both versions. The SKR2 V2 has a motor power enable feature 
and needs to be included in the config. 

.. code-block::

	{
	"Thread": "On load",
	"Type": "Motor Power",
	"Comment": "Enable motor power SKR2",
	"Pin": "PC_13"
	}



Hardware Pins
-------------
Remora firmware has some features available only on specific hardware pins. These pins can vary between STM32 boards.

Available PWM Hardware pins:

-  PA_1 PA_2 PA_3 PA_5 PA_6 PA_7 PA_8  PA_9 PA_10 PA_11 PA_15
- PB_0 PB_1 PB_3 PB_4 PB_5 PB_6 PB_7 PB_8 PB_9 PB_10 PB_11 PB_13 PB_14 PB_15
- PC_6 PC_7 PC_8 PC_9
- PD_12 PD_13 PD_14 PD_15
- PE_5 PE_6 PE_8 PE_9 PE_10 PE_11

Available QEI Encoder Hardware pins:

- PE_9
- PE_11
- PE_13 is used as index

Wiring
------

Wiring requires the following components:

* 100mm Female-Female Dupont ribbon jumper
* 10 way (2x5) Dupont connector
* 8 way (2x4) Dupont connector


The pinout for the BTT SKR v2 is standard for both versions and keeps the footprint of the SKR v1.4 and Octopus. 


+--------+----------+----------------------+-------------+
| PIN    | COLOR    |   FUNCTION  	   | RPI PIN     |
+--------+----------+----------------------+-------------+
| PA_7   | RED      | SPI_MOSI   	   | RPI_PIN_19  |
+--------+----------+----------------------+-------------+
| PA_6   | ORANGE   |  SPI_MISO 	   | RPI_PIN_21  | 
+--------+----------+----------------------+-------------+
| PA_5   | GREEN    | SPI_SCK		   | RPI_PIN_23  | 
+--------+----------+----------------------+-------------+
| PA_4   | YELLOW   |  SPI_SSEL  	   | RPI_PIN_24  | 
+--------+----------+----------------------+-------------+
| PC_4   | BROWN    | PRU Reset	  	   | RPI_PIN_22  | 
+--------+----------+----------------------+-------------+
| PA_9   | PURPLE   | MCU TX to RPI RXD    | RPI_PIN_10  |
+--------+----------+----------------------+-------------+
| PA_10  | GREY     | MCU RX to RPI TXD    | RPI_PIN_8   |
+--------+----------+----------------------+-------------+


.. image:: ../../_static/SKRv2-wiring-diag.png
    :align: center
	
.. image:: ../../_static/SKRv14-wiring-diag2.png
    :align: center
	
To power the Raspberry Pi from the SKR V2.0 the follwoing components are requried:

* 150mm or 200mm Female-Female Dupont ribbon jumper
* 5 way (1x5) Dupont connector
* 5 way (1x5) Dupont connector
	
.. image:: ../../_static/SKRv2-wiring-diag3.png
    :align: center
	
The diagram above includes the optional serial debug interface. Note that TX <-> RXD and RX <-> TXD.
