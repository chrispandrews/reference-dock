OpenXC Dock
===========

This repository is a part of the [OpenXC][] project.

This repository contains the documentation and open source design files for an
example OpenXC accessory dock, originally created by Ford.

The OpenXC Dock is a hub for connecting multiple USB accessories to an Android
tablet or phone. Since each accessory uses USB to communicate with the host, the
Dock is essentially a USB hub. The Dock receives power from the vehicle, and
regulates this power to a steady +5V for use by the accessory.

There is a [gallery of
photos](https://plus.google.com/photos/108408483770573977605/albums/5989365070748059233)
of the prototype dock.

### Buttons, Headers, Jumpers

*	JP2 - This jumper connects the +5V pin of the USB Micro connector to the +5V
	supply on the Dock. Connected by default. This enables charging of the
	Android device as well as standalone Dock usage with a PC.
*	SJ1 - USB hub reset - short this jumper to keep the USB hub in reset.
	Normally open.
*	SJ2 - Allows the USB hub to sense it's power state from the USB Micro
	connector. Normally closed by trace.
*	J10 - I2C - Exposes the i2c bus used by the USB hub for configuration
*	R35 - ID Resistor - For use with an Android device that expects a
	non-standard ID resistor in order to charge from USB OTG. The surface mount
	resistor R35 can be removed and a full-size 1/4W through hole resistor can
	be added instead.

### Known Issues

#### v0.3

*	USB D+ and D- swapped for downstream ports - An i2c EEPROM (AT24C02) was
	added to the board and soldered to J10. This allowed access to the PORT_SWAP
	register within the USB2517 hub, which enables the USB data lines to be
	swapped within the hub. Also
*	Downstream power ports are not enabled on hub poweron - The accessory USB
	connectors are not powered until a valid USB host has connected to the hub.
	Hub should be upgraded to a part with full Battery Charging support to
	enable this feature. The downstream power switches (U1, U2, U6, U7) were all
	reworked to remain permanently ON (each EN input trace was cut and the pin
	connected to +5V).
*	Downstream power ports will only flag a power fault when a host is
	connected. If an accessory malfunctions and draws too much power, the power
	switches will protect the accessory and Dock by limiting the current to
	500mA. When a host is connected, the LED for that port will light Red to
	indicate the fault. This does not occur when no host has been connected, so
	it's possible that a malfunctioning accessory will continue to draw ~500mA
	from the vehicle without alerting the user.

## License

Copyright (c) 2014 Ford Motor Company

The electrical and mechanical designs in this repository are available under the
[Creative Commons Attribution 4.0
International](http://creativecommons.org/licenses/by/4.0/deed.en_US) license.

[OpenXC]: http://openxcplatform.com
