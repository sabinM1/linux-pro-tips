# Xfce specific tips

1. Fonts on Xfce (Chinese chars and emojis)<br>
sources:<br>- [maketecheasier.com](https://www.maketecheasier.com/use-emojis-in-linux/)<br>- [zeerd.com](https://blog.zeerd.com/support-CN-under-xfce/)

Chinese characters:
- Ubuntu/Debian:
  - `sudo apt install ibus-sunpinyin`
  - `sudo apt install ttf-mscorefonts-installer`
  - `sudo apt install fonts-wqy-zenhei`
  - `ibus-daemon -d`
  - Log out and log back in
- Arch (some packages from AUR via `yay`):
  - `yay -S ibus-sunpinyin`
  - `ttf-ms-fonts`
  - `yay -S wqy-zenhei`
  - Log out and log back in

Emojis:
- Ubuntu/Debian:
  - `sudo apt install ttf-ancient-fonts`
  - `sudo apt-add-repository ppa:eosrei/fonts`
  - `sudo apt update`
  - `sudo apt install fonts-emojione-svginot`
  - Log out and log back in
- Arch (some packages from AUR via `yay`):
  - `yay -S ttf-ancient-fonts`
  - `yay -S ttf-joypixels`
  - Log out and log back in

Note 1: Japanese characters are also rendering properly, but I only tested this after installing the above packages.<br>
Note 2: I also tried to change fontconfig's configuration files and also added my own with no success.

---
