# BTT-SKR-1.3
Utilisation de la carte SKR en version 1.3

 - How to use the card BigTreeTeck SKR 1.3 with Windows 10
 ---------------------------------------------------------

	Creation date : 18 octobre 2020
	Update :

 - Before all things :

	Debuguing session :
		=> board with nothing. (no drivers, no endstop, no jumper - NOTHING_²)
		=> the board is placed on an insulating surface and we discharge ourselves electrically.
		=> Check the position of the power supply jumper.

		-------------
		IMPORTANT_² :
		-------------
		Only the jumper concerning the power supply must be on the board and correctly positioned on [5v + USB].
		Only the card reader [ONBOARD] can be used for firmware update.
		All micro SDHC card do not necessarily work for firmware update.

			|-> Mine is a Verbatim micro SDHC Class 4 8GB
			|-> https://static.rapidonline.com/catalogueimages/product/49/36/s49-3634p01wl.jpg

 ---------------------------------------------------------------------------------------------------------------------------
 

	Connect the board to the PC...
	Open Device Manager and check :

		1. Disk drives
		Marlin SDCard 01 USB Device

		2. Port (COM & LPT)
		Marlin USB Serial (COM4)

		If the Marlin driver is not used :

			Right click on the port COM4
			Update driver
			Browse my computer for drivers
			Let me pick from a list...
			Choose => Marlin USB Serial

		Speed of port == #define BAUDRATE in Configuration.h

			Right click on the port COM4
			Properties
			Port Settings
			Bits per second : 115200
			

 ---------------------------------------------------------------------------------------------------------------------------

 - Then we have to know if #define SDCARD_CONNECTION [LCD - ONBOARD - CUSTOM_CABLE] is activated or not.

	1. Enabled : The card reader of the SKR 1.3 will not appear in Windows 10 Explorer.
	2. Disabled : The card reader of the SKR 1.3 will appear in Windows 10 Explorer.

	If "SDCARD_CONNECTION ONBOARD" is enabled, we must detach the SD card via the printer management interface (pronterface,pronsole,octoprint...)
	so that Windows 10 Explorer can take over.

		Command : M20 (List SD Card) M21 (Init SD Card) M22 (Release SD Card)

	Once detached and if there is an SDHC card in the reader, properly partitioned and formatted*, then we can access this device.

	* Partitioning and formatting

	A. Partitioning => 1st partition under 32GB
	B. Formatting => FAT16 or FAT32 - Label == REARM
	C. Create a file "FIRMWARE.CUR" on this partition.
	D. Check that the file "FIRMWARE.CUR" has the extension ".CUR" (explorer Windows 10 => View - File name extensions must be selected)


 - Download Marlin-bugfix-2.0 and unzip it.
 - Download Visual Studio Code and install the extension "PlateformIO IDE".

	In VSCode, Open a folder (Marlin-bugfix-2.0) and edit the following 3 files :

		1. plateformeio.ini
		2. Configuration.h
		3. Configuration_adv.h

	1. plateformeio.ini

	[platformio]
	[...]
	default_envs = LPC1768
	[...]

	[env:LPC1768]
	[...]
	upload_port = COM4
	upload_speed = 115200

	2. Configuration.h

	#define SERIAL_PORT -1
	#define SERIAL_PORT_2 0
	#define BAUDRATE 115200
	#define MOTHERBOARD BOARD_BTT_SKR_V1_3
	#define SDSUPPORT

	3. Configuration_adv.h

	#define SD_DETECT_STATE HIGH
	//#define SDCARD_CONNECTION ONBOARD (TEST)

	or
	#define SDCARD_CONNECTION ONBOARD (PRODUCTION)		

 - Save all changes.

 ---------------------------------------------------------------------------------------------------------------------------

	Check that the SD card is present in Windows 10 Explorer.
	In VSCode, click on the icon "Alien" in the panel on the left.

	- Build All => (About 5 minutes)
		upload disk:  E:
		Building .pio\build\LPC1768\firmware.bin
		SUCCESS

	- Upload All => (About 5 secondes)
		Looking for upload disk...
		Use manually specified: E:
		Uploading .pio\build\LPC1768\firmware.bin
		Firmware has been successfully uploaded.
		(Some boards may require manual hard reset)

 - Resetting the card:

	Check that on disk E: the 2 files "FIRMWARE.CUR" et "firmware.bin" are present.
	Briefly press the reset button on the card. (clic)
	Windows 10 plays a sound to notify that it has detected the board.
	Open the disque E: in Windows 10 explorer.

		1. The file "firmware.bin" has disappeared
		2. The file "FIRMWARE.CUR" is updated (date and hours modified)

 NB :
 ---------------------------------------------------------------------------------------------------------------------------
	- Show file extensions.
	- Show empty disks.
	- The letter identifying the drive may be different
	- activate windows 10 sounds.
	- Only the card reader [ONBOARD] can be used for firmware update.
