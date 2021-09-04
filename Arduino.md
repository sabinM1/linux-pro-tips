# Arduino/Arduino IDE specific tips

1. Installing and preparing Arduino IDE
 - Install it using your desired package manager (e.g. `pacman -S arduino`, `apt install arduino`) or [download the tarball](https://www.arduino.cc/en/software) and run the `install.sh` script;
 - you will have permission problems when trying to upload code to a board, but this be resolved easily using the `arduino-linux-setup.sh` script at https://github.com/arduino/Arduino/blob/master/build/linux/dist/arduino-linux-setup.sh
 - you can also run the following:
```bash
curl https://raw.githubusercontent.com/arduino/Arduino/master/build/linux/dist/arduino-linux-setup.sh | bash -s $USER
```
