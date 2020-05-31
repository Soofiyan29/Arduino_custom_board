[Drive link](https://drive.google.com/open?id=1bKhigrruck6pl0mA11rRQiQc2liFzoFC) for all files,
# Arduino_custom_board
 
The boards.txt file contains various parameters for various menu options for different Arduino supported microcontrollers. These options for the board are some of the parameters which are only specific for some board or they are shown only when the microcontroller is selected. The boards.txt file is situated in the directory **./Arduino15/packages/arduino/hardware/avr/1.8.2/boards.txt.** As these are hidden in the home directory for linux we have to press ctrl+H thus to show hidden files.

There are other files as shown in the below column along with directories, 

Name          | Description   | Type
------------- | ------------- | -------------
boards.txt    | Contains the build instructions and configurations for each board  | Files
platform.txt  | Contains the compiler instructions used for building programs for the board and we have to change only the version and type of the board  | File
bootloaders    | Contains bootloader for each board and bootloader is important and custom for each board  | direcotry
varients    | Contains the board layout and pin configuration for each board in the package. This is where we will change most of the configuration for our custom board  | Directory

These are the various files situated in the 1.8.2 folder, for now we have to edit boards.txt file,

```ruby
uno.name=e-Yantra eYFi_Mega  
uno.vid.0=0x2341
uno.pid.0=0x0043
uno.vid.1=0x2341
uno.pid.1=0x0001
uno.vid.2=0x2A03
uno.pid.2=0x0043
uno.vid.3=0x2341
uno.pid.3=0x0243
uno.upload.tool=avrdude
uno.upload.protocol=arduino
uno.upload.maximum_size=32256
uno.upload.maximum_data_size=2048
uno.upload.speed=115200
uno.bootloader.tool=avrdude
uno.bootloader.low_fuses=0xFF
uno.bootloader.high_fuses=0xDE
uno.bootloader.extended_fuses=0xFD
uno.bootloader.unlock_bits=0x3
uno.bootloader.lock_bits=0x0F
uno.bootloader.file=optiboot/optiboot_atmega328.hex
uno.build.mcu=atmega328p
uno.build.f_cpu=16000000L
uno.build.board=AVR_UNO
uno.build.core=arduino
uno.build.variant = standard
```

Here we have added the name as **e-Yantra eYFi_Mega**

as our board name so we have to just change the name now whenever you will be plugging the uno in the pc there you will see this name instead of Arduino/Genuine Uno.

Installing Arduino CLI:
- MacOS/Linux:
```
	brew update
	brew install arduino-cli
```
- Windows:
	- Link : [Windows 64 bit](https://downloads.arduino.cc/arduino-cli/arduino-cli_latest_Windows_64bit.zip)
	- Link : [Windows 32 bit](https://downloads.arduino.cc/arduino-cli/arduino-cli_latest_Windows_32bit.zip)

Commands:
1. arduino-cli is a container of commands and each command has its own dedicated help text that can be shown with the help command like this:
```
$ arduino-cli help core
```
2. Arduino CLI doesn't strictly require a configuration file to work because the command line interface provides any possible functionality. However, having one can spare you a lot of typing when issuing a command, so let's go ahead and create it with:
```
$ arduino-cli config init
```
3. To create a new sketch named MyFirstSketch in the current directory, run the following command:
```
$ arduino-cli sketch new MyFirstSketch
```
This file will be saved in the /home/username/ directory
4. The first thing to do upon a fresh install is to update the local cache of available platforms and libraries by running:
```
$ arduino-cli core update-index
```
5. After connecting the board to your PC by using the USB cable, you should be able to check whether it's been recognized by running:
```
$ arduino-cli board list
```
6. For compiling the uno board which is situated in e-Yantra folder we have to run:
```
$ arduino-cli compile --fqbn e-Yantra:avr:eyfi MyFirstSketch
```
7. For uploading the code on arduino uno first plug the board and run this command:
```
$ arduino-cli board list
``` 
And then note the Port of the particular board and now paste here by running:
```
$ arduino-cli upload -p /dev/ttyACM0 --fqbn e-Yantra:avr:eyfi MyFirstSketch
```
/dev/ttyACM0 is the port of the board.	

**Now we have to add the board on arduino ide:**

First we have to create a package which contains the same architecture files and same board files, for example we have to add Atmega2560 board thus we have to add all the files related to avr architecture by copying or finding it on the official github repo of arduino.
Let's edit the package for Atmega2560 controller first,
First we have to add all files from the avr package, which contains all these files as shown in the github repo,
![Githu package info](/Images/1st.png)
Now we have to edit the boards file for adding our eYFi-Mega custom board, now we have to add the name of the board and board id i.e. eYFi-Mega Atmega2560 and eyfi respectively, we have to delete all other board configuration except mega one and edit on that configuration as shown,
We also have to change the board name and version of the board in the platform.txt file as shown below,

Now coming to the esp32 controller,
We have to copy all the package files fom the official esspresif esp32 github repo which contains all the files,
Now we have to follow same procedure as done in Atmega2560 such as changing board file and platform files,
Now for board file we have to remove all the boards configuration except esp32 dev module and change the name of the board and board id as shown below,

Also changing the platform.txt file as shown below for respective name and version of the controller,
Also for both the controller we have to delete all the variants except standard for avr controller and esp32 for esp32 controller as shown below,

Now coming to the json file we have to first add the platform specs in the json file with the following configuration as shown, but in tools and tools dependencies we dont have to put any tools as it is default getting the path from the arduino pre installed tools from the bin folder,

For esp32 we have to add the tools as shown below and also give the **packager** name same as that one of the platform name which is customized by us, as shown below,

Adding custom bootloader for esp32 and custom partition for esp32,
To add custom bootloader we have to add these files in directory as packages/eYFi-Mega-ESP32-board/hardware/esp32/1.0.0/tools/partition, here we have to add the eyfi_mega_default.csv which is our custom partition, now we have to add the directory in the platform file and also add one extra menu for custom partition for our board in the menu section of arduino ide,
This is the addition in the platform.txt file,

This is the addition in the board.txt file which we have just added one partition menu option,

We also have to add custom bootloader for esp32 in the directory packages/eYFi-Mega-ESP32-board/hardware/esp32/1.0.0/tools/ here we have to add a folder eyfi-mga and add all these files,

Now we have to add tools for esp32 and Atmega2560 to upload and flash our program to both the controller through OTA, thus we have to modify the json file for each board and also add the compressed file for these tools,
For Atmega2560 we have to first store these three files in a folder as shown and then zip this folder and copy the github directory of th file,

Now we have to paste the link in the url of the tools and checksum value and archive filename of the zip as shown below,

For esp32 we have to create one folder named esptool_py and then paste all the flashers and then zip the file as shown below,

Now we have to edit the platform and json files, for platform.txt we have to add the directory for flashing tools as shown below 
For Atmega250,

For esp32,

We then also have to edit the json file for the same,

