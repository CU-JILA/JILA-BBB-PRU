# Turn on led using PRU

# Woking of PRU in Beaglebone Black --> "https://beagleboard.org/pru"


# References:

1. BBB PRU GPIO --> "https://credentiality2.blogspot.com/2015/09/beaglebone-pru-gpio-example.html"

2. am335x_pru_package --> "https://github.com/DTJF/fb_prussdrv"



# Image --> "https://beagleboard.org/latest-images"

	DOWNLOAD --> AM3358 Debian 10.3 2020-04-06 4GB SD IoT 


# Procedure:

$ sudo apt-get update && sudo apt-get dist-upgrade
$ sudo apt-get install

# Including libraries

	//this zip folder is also availabe in folder named files

	$ wget https://github.com/DTJF/fb_prussdrv/archive/master.zip

	$ unzip master.zip

	$ cd fb_prussdrv-master/

	$ sudo su

	$ cp bin/pasm /usr/local/bin/

	$ cp bin/libprussdrv.* /usr/local/lib/

	$ ldconfig

	$ cp include/* /usr/include/

# creating device tree overlay 

	//save this file in /lib/firmware
	// We can find this file in the folder named files

	"PRU-GPIO-EXAMPLE-00A0.dts"	

# compile

	root@beaglebone:/lib/firmware# dtc -O dtb -I dts -o /lib/firmware/PRU-GPIO-EXAMPLE-00A0.dtbo -b 0 -@ PRU-GPIO-EXAMPLE-00A0.dts 

# loading dto 

	*In kernel v4.19 --> we can't load using the below command
	*$echo PRU-GPIO-EXAMPLE > /sys/devices/bone_capemgr.?/slots

	# Alternative method
	root@beaglebone# sudo nano /boot/uEnv.txt

	// Add our dtbo to the additional custom capes and uncomment that line
	// uEnv.txt is shown below after editing
	*******************************************************************
		###Master Enable
	enable_uboot_overlays=1
	###
	###Overide capes with eeprom
	#uboot_overlay_addr0=/lib/firmware/.dtbo
	#uboot_overlay_addr1=/lib/firmware/.dtbo
	#uboot_overlay_addr2=/lib/firmware/.dtbo
	#uboot_overlay_addr3=/lib/firmware/.dtbo
	###
	###Additional custom capes
	uboot_overlay_addr4=/lib/firmware/PRU-GPIO-EXAMPLE-00A0.dtbo
	#uboot_overlay_addr5=/lib/firmware/.dtbo
	#uboot_overlay_addr6=/lib/firmware/.dtbo
	#uboot_overlay_addr7=/lib/firmware/.dtbo
	###
	###Custom Cape
	#dtb_overlay=/lib/firmware/.dtbo
	###
 	*******************************************************************
	
*** uncomment UIO and comment RPROC in /boot/uEnv.txt



# We use a C program to load .bin code to PRU

	//.c file can be found in files

# compiling c filr 

	**if error see the bottom of this document
 	$gcc -o pru_loader pru_loader.c -lprussdrv -lpthread

# Assembly code for PRU

	//.p file can be found in files
	
# Assembling the .p file using PASM assembler
	
	$pasm -b pru_egp_output.p  
	
	//generates pru_egp_output.bin

# loading .bin to pru

	$ ./pru_loader pru_egp_output.bin

#Additional:
** kernel version error:
	"prussdrv_open() failed with -1" during the execution

 	$ cd /opt/scripts/tools
	$ ./update_kernel.sh --bone-rt-kernel --lts-4_1   

