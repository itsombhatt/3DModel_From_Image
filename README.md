# 3DModel_From_Image


**Description**

## ESP32 CAM FTP Client
**Requirements**
The ESP32 CAM needs to take a photo on startup and save it to the SD card. Then it will send this photo to the jetson as a client through FTP.
**Work**
First download zip from https://github.com/ldab/ESP32_FTPClient
Then install Arduino library from the downloaded zip file.
Upload the Arduino code in _insert my github here_ to the ESP32 CAM.
Side note: The first photo taken after bootup seems to have a green tint. Following photos have regular lighting.



## Jetson FTP Server and AI Image Processing
**Requirements**
The jetson will have a FTP server on it which receives images from the ESP32 CAM and saves them to a local directory. Then it will run a NERF or other image to 3d scene algorithm on it and convert it to a video of the scene.
**Work**
Starting the FTP Server
```sh
sudo apt update
sudo apt upgrade
# Install vsftpd
sudo apt install vsftpd
# Configure FTP server
sudo nano /etc/vsftpd.conf
# Add/modify these lines:
write_enable=YES
local_enable=YES
chroot_local_user=YES
allow_writeable_chroot=YES
# Create user
sudo useradd -m esp32cam
sudo passwd esp32cam
# Create photos directory
sudo mkdir -p /home/esp32cam/photos
sudo chown -R esp32cam:esp32cam /home/esp32cam/photos
# Restart FTP server
sudo systemctl restart vsftpd
```

Using Nerfstudio
```sh
#install
curl -fsSL https://pixi.sh/install.sh | bash

git clone https://github.com/nerfstudio-project/nerfstudio.git
cd nerfstudio
pixi run post-install
pixi shell

ns-process-data images --date /home/user/data --output-dir /home/user/output
ns-train nerfacto --data /home/user/output
```
