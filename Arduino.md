# Arduino/Arduino IDE specific tips

## Installing and preparing Arduino IDE
 - Install it using your desired package manager (e.g. `pacman -S arduino`, `apt install arduino`) or [download the tarball](https://www.arduino.cc/en/software) and run the `install.sh` script
 - you will have permission problems when trying to upload code to a board, but this be resolved easily using the `arduino-linux-setup.sh` script at https://github.com/arduino/Arduino/blob/master/build/linux/dist/arduino-linux-setup.sh [¹]
 - you can also run the following:
```bash
curl https://raw.githubusercontent.com/arduino/Arduino/master/build/linux/dist/arduino-linux-setup.sh | bash -s $USER
```
 - reboot or log out

[¹]: https://github.com/arduino/Arduino/issues/7690

## Making the Raspberry Pi Pico work

 - In Arduino IDE go to File -> Preferences -> Additional Boards Manager URLs and add `https://github.com/earlephilhower/arduino-pico/releases/download/global/package_rp2040_index.json` in order to add support for the Pico
 - Go to Tools -> Board -> Boards Manager, search for *Raspberry Pi Pico/RP2040* and install the package
 - The board probably won't be recognized, but this has a 5 minute fix:
    - Install [Thonny](https://thonny.org/) a *Python IDE for beginners* (Note that this program can also be used for development on the Pico using MicroPython) (install on Arch based distros: `yay -S thonny`; on Debian based distros: `apt install thonny`)
    - After selecting the standard installation, go to Tools -> Options -> Interpreter and change the interpreter to *MicroPython (Raspberry Pi Pico)*
    - Select *Install or update firmware* after unplugging the board and plugging it in via USB with the *BOOTSEL* button pressed until it appears as a storage device
    - The board should be detected in a file manager and in Thonny, so go ahead and click the *Install* button after verifying the device location and the model
 - The uploading procedure is as follows:
 	- plug the board with the *BOOTSEL* button pressed until it appears as a storage device
 	- select the `/dev/ttyACM0` port in the Arduino IDE and upload the desired program
 	- unplug and plug the board in again

## Micronucleus (in order for the MH-Tiny ATTINY88 to work)

TL;DR The micronucleus version from https://github.com/MHEtLive/arduino-boards-index was too old (2015, latest=2021) to run my MH-Tiny ATTINY88 development module (running v2.2) so I had to replace that and do some other things.

 - Install the board from the Arduino IDE:
   - go to File -> Preferences -> Additional Boards Manager URLs
   - add ` https://raw.githubusercontent.com/MHEtLive/arduino-boards-index/master/package_mhetlive_index.json`
 - Go to `$HOME/.arduino15/packages/mhetlive/tools/micronucleus/2.0a4`
   - replace micronucleus (the executable) with the LATEST one from https://github.com/micronucleus/micronucleus/releases (in my case the one from *micronucleus-cli-master-c2c7fa09-x86_64-Linux.zip*)
 - Create a rule (text file) at `/etc/udev/rules.d` called `49-micronucleus.rules` with the contents from https://github.com/micronucleus/micronucleus/blob/master/commandline/49-micronucleus.rules
 - Reload rules: `sudo udevadm control --reload-rules`
 - (optional) You MAY need to add `$HOME/.arduino15/packages/mhetlive/tools/micronucleus/2.0a4` to PATH
 - **You don't need to select the port from Arduino IDE, only the board; micronucleus will find the device**
 - Sample Arduino IDE output:
```
Sketch uses 898 bytes (14%) of program storage space. Maximum is 6012 bytes.
Global variables use 9 bytes of dynamic memory.
Running Digispark Uploader...
Plug in device now... (will timeout in 60 seconds)
> Please plug in the device (will time out in 60 seconds) ... 
> Device is found!
connecting: 16% complete
connecting: 22% complete
connecting: 28% complete
connecting: 33% complete
> Device has firmware version 2.2
> Device signature: 0x1e9311 
> Available space for user applications: 6650 bytes
> Suggested sleep time between sending pages: 7ms
> Whole page count: 208  page size: 32
> Erase function sleep duration: 1456ms
parsing: 50% complete
> Erasing the memory ...
erasing: 55% complete
erasing: 60% complete
erasing: 65% complete
> Starting to upload ...
writing: 70% complete
writing: 75% complete
writing: 80% complete
> Starting the user app ...
running: 100% complete
>> Micronucleus done. Thank you!
```
