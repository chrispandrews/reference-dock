Dock Assembly Instructions
==========================

Special Assembly Instructions
-----------------------------

*	See NOTES: on ASSY drawings for general notes
*	Ensure no excess solder is wicked into J0 (USB Micro Connector).  Verify a
	USB Micro cable can successfully connect
*	Ensure J0 (USB Micro Connector) is completely flush to the PCB before/after
	reflow.  Ensure pins at rear of connector are not lifted off pads.
*	Apply epoxy to J0 for additional structural rigidity

Gerber output instructions
--------------------------

*	Verify silkscreen and PCB Frame artwork reflects accurate revision
*	Delete all content on layer (144) DrillLegend
*	run "drillegend.ulp": Use all default values, click OK.
*	Reselect layer (144) and move the newly created drill legend back onto the
	frame
*	Verify SSC-EAGLE-4Lyr_v1.0.0.3.dru is loaded as current design rules file
*	Turn on all copper and stopmask layers, turn off silkscreen layers (skip
	silkscreen error checking, silkscreen will be clipped by assembly house)
*	Preform DRC/ERC check
*	run "drillcfg.ulp": select "Inch" as output format.  Click OK.  Save.
*	run "excellon.cam" in the CAM processor, click "Process Job"
*	run "4LPlus-Sunstone.cam" in the CAM processor, click "Process Job"
*	Double check all gerber files using a gerber file viewer (like gerbv).
*	Deselect all layers, select only the following layers:
	(17)Pads, (18)Vias, (20)Dimension, (21)tPlace, (22)bPlace, (23)tOrigins,
	(24)bOrigins, (25)tNames, (49)Reference, (51)tDocu, (52)bDocu
*	run "dxf.ulp": deselect "Fill Areas", make sure "mm" is selected under the
	"units" header.  Save the DXF file.
*	Deselect all layers, select only the following layers:
	(17)Pads, (18)Vias, (20)Dimension, (21)tPlace, (23)tOrigins, (25)tNames,
	(48)Document, (49)Reference, (51)tDocu, (144)DrillLegend
*	Hit print (Ctrl+P), select "Print to File (PDF)" as the printer, select
	Dock_ASSY.pdf, paper size A4, scale factor 1.  Click OK.
*	Deselect all layers, select only the following layers:
	(1)Top, (16)Bottom, (17)Pads, (18)Vias, (20)Dimension, (21)tPlace,
	(22)bPlace, (25)tNames, (51)tDocu, (52)bDocu
*	Zoom in so that PCB is centered in the screen and fills the window as much
	as possible.
*	Select File->Export->Image.  Select Dock.brd.png.  Set "Resolution" to 600.
	Set "Area" to "Window".
*	In the schematic window, hit print (Ctrl+P), select "Print to File (PDF)" as
	the printer, select Dock.sch.pdf, paper size A4, scale factor 1, page limit
	1  *	lick OK.
*	In the schematic window, select File->Export->Partlist.  Select
	"Dock.parts.txt".  Click OK.
*	Manually compare "Dock.parts.txt" to "Dock_bom.xls".  Make sure the two
	lists are consistent.
*	Open "CHANGELOG.xls".  Update the file to reflect the new revision.
*	Create a zip archive with the following files (at least):
	*	Dock_sch.pdf
	*	Dock.brd.png
	*	Dock_ASSY.pdf
	*	Dock.tsp
	*	Dock.L1
	*	Dock.L2
	*	Dock.L3
	*	Dock.L4
	*	Dock.smt
	*	Dock.smb
	*	Dock.slk
	*	Dock.oln
	*	Dock.gpi
	*	Dock.dxf
	*	Dock.drl
	*	Dock.dri
	*	Dock.drd
	*	Dock.bsp
	*	Dock.bsk
	*	Dock_bom.xls
	*	Dock.sch
	*	Dock.brd
	*	CHANGELOG.xls

Gerber file descriptions:
-------------------------

*	.L1 	Top Copper Layer
*	.L2		Layer 2 (Ground)
*	.L3		Layer 3 (VCC)
*	.L4		Bottom Copper Layer
*	.smt 	Top Soldermask
*	.slk 	Top Silkscreen
*	.tsp 	Top Solderpaste
*	.smb 	Bottom Soldermask
*	.bsk 	Bottom Silkscreen
*	.bsp 	Bottom Solderpaste
*	.oln	PCB Outline
*	.drd 	Excellon Drill Data
*	.drl 	Basic Drill tool information
*	.dri 	Extended Drill tool information

Factory Testing Plan
--------------------

A test jig can be created using [Spring Pogo
Pins](https://www.adafruit.com/products/394).  At a minimum, the test jig should
contain pogo pins and mounting holes at the PCB locations outlined in the
Dock_polog_locations.xls spreadsheet. A rooted linux host is necessary for this
test - perhaps an embedded android device like a Raspberry Pi, Beagleboard or
pcDuino.

1.	Apply 12V power to board
1.	Measure all power rails, verify they are within 0.1V of target
1.	Read voltage level at PWR0-PWR6 pins, verify accessory ports have valid
	power
1.	Read logic level at ERR0-ERR6 pins, verify all pins are HIGH
1.	Remove +12V power, attach +5V supply to the Micro USB port
1.	Re-verify power rails are within range
1.	Re-apply 12V power to board
1. 	Connect USB to linux host
1.	Examine lsusb, /var/log/kern.log and /sys/bus/usb/ to verify Hub has
	enumerated successfully
1.	Attach a valid USB device to each port, for each port Examine lsusb,
	/var/log/kern.log and /sys/bus/usb/ to verify USB device has enumerated
	successfully
1.	Attach a shorted USB device to each port, verify LED turns red, verify the
	associated ERRX pin has fallen, verify lsusb and /sys/bus/usb notices the
	power fault
