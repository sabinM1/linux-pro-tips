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
