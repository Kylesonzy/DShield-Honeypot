
# DShield Honeypot

## Description
This guide walks through setting up a Raspberry Pi as a honeypot using the DShield project. By the end of the process, you'll have your Raspberry Pi running as an Internet-facing honeypot, sending logs to the DShield dashboard. At least I did haha.

---

## Table of Contents
- [Project Setup](#project-setup)
- [Steps to Setup the Raspberry Pi](#steps-to-setup-the-raspberry-pi)
- [Fetching API Key](#fetching-api-key)
- [Generating an SSL Certificate](#generating-an-ssl-certificate)
- [Finishing Up](#finishing-up)
- [Troubleshooting](#troubleshooting)
- [License](#license)

---

## Project Setup

### Requirements
- Raspberry Pi
- SD Card
- Raspberry Pi Imager
- SSH enabled on Raspberry Pi
- Internet connection

---
## Raspberry Pi Build Overview

I built this setup using the CanaKit Raspberry Pi 4 kit. The equipment used includes:

- **Raspberry Pi 4 Model B** with 4GB RAM
- **CanaKit heatsinks** applied to the CPU and other critical components to help with cooling and performance.
- **Raspberry Pi camera module**, which I mounted for capturing images or video.
- **Micro HDMI cables** connected to an external display.
- **USB keyboard and mouse** for initial configuration.
- **SD card** preloaded with Raspberry Pi OS.
- Various **power and USB cables** to ensure connectivity and power to all components.

The build focuses on efficiently cooling the Raspberry Pi and utilizing the camera module for future projects or monitoring tasks.

![alt text](/images/raspsetup.jpg)

## Steps to Setup the Raspberry Pi

### 1. Download and Install Raspberry Pi OS
Use the Raspberry Pi Imager to download and install a fresh version of 
**Raspberry Pi OS 64-bit Lite** onto the SD card.
- Insert the SD card into your computer, select the OS and destination, then start the installation.
![usb](/images/start3.jpg)

### 2. Insert SD Card into Raspberry Pi
Eject the SD card from your computer and insert it into the Raspberry Pi. Power on the Raspberry Pi.
![os](/images/start1.jpg)
### 3. Enable SSH and Connect to Raspberry Pi
After setting up the Raspberry Pi, enable SSH so you can connect to it from another machine.
- Connect to the Raspberry Pi using SSH:
    \`\`\`bash
    ssh pi@raspberrypi
    \`\`\`

### 4. Update the Raspberry Pi
Before proceeding, ensure the Raspberry Pi is up-to-date.
- bash
sudo apt update && sudo apt-get -uy dist-upgrade


![alt text](/images/image.png)
### 5. Reboot the Raspberry Pi
Reboot the Raspberry Pi after updating:
\`\`\`bash
sudo reboot
\`\`\`

### 6. Reconnect via SSH
After the reboot, SSH back into the Raspberry Pi:
\`\`\`bash
ssh pi@raspberrypi
\`\`\`

### 7. Install Git
Git is required for cloning the DShield repository. Install Git using the following command:
\`\`\`bash
sudo apt-get -y install git
\`\`\`
![alt text](/images/image-1.png)
### 8. Clone the DShield Repository
Run the installation script after cloning the DShield repository:
\`\`\`bash
git clone https://github.com/DShield-ISC/dshield/
cd dshield
sudo ./install.sh
\`\`\`

---
![alt text](/images/image-2.png)

## Fetching API Key

### 9. Obtain the API Key
To complete the installation, fetch the API key from [DShield Honeypot API Key](https://isc.sans.edu/honeypot.html).

---

## Generating an SSL Certificate

### 10. Generating SSL Certificate for Decoy
To create a decoy for attackers, generate an SSL certificate for your honeypot:
\`\`\`bash
sudo apt-get install openssl
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/ssl-cert.key -out /etc/ssl/certs/ssl-cert.crt
\`\`\`
Fill in the required fields such as country, state, and organization as you generate the certificate.

---

## Finishing Up

### 11. Expose Raspberry Pi to the Internet
Once installation is complete, expose your Raspberry Pi to the Internet. Monitor the dashboard as logs are sent to DShield.

---
## Testing

### 12. Nmap Scans with Chat Analysis

#### Regular Scan (-A 192.168.X.X)

For the first scan, I used the `-A` flag, which enables OS detection, version detection, script scanning, and traceroute. This scan provided a comprehensive analysis of the target at `192.168.X.X`, detailing the open ports, services running, and OS information. It's a useful scan for gathering detailed information about a host while maintaining a moderate speed.

![Regular Nmap Scan](/images/reg_scan.png)

#### Insane Scan with Timing Switch (-T5) and Full Port Scan (-p-)

In this scan, I performed a much more aggressive approach by using the `-T5` timing switch, which runs the scan at an "insane" speed. I also added the `-p-` flag to scan all 65,535 ports on the target. This method can generate a large amount of logs and data in a short time, but at the cost of stealth. This type of scan is more likely to be detected, but it's effective for quickly gathering information from the target.

![Insane Nmap Scan](/images/insane_scan.png)

**Again, this is a honeypot setup, so it's designed to appear vulnerable and attract attacks for analysis purposes.**

Now it may take some time for the logs to showup but typically you would want to go visit the SANS dashboard and look in the firewall report section to find enumeration scans of your system or any type of scan really..

![Insane Nmap Scan](/images/end.png)

## Troubleshooting

Here are some potential issues you may encounter while setting up your Raspberry Pi honeypot and solutions for each:

- **Issue 1: SSH Connection Refused**

  *Description:* After enabling SSH, the connection to the Raspberry Pi may be refused.

  *Solution:* 
  - Ensure that SSH is properly enabled on the Raspberry Pi. You can check this by running:
    ```bash
    sudo raspi-config
    ```
    Under "Interfacing Options," make sure SSH is enabled.
  - Ensure the Raspberry Pi and host machine are on the same local network.
  - Verify the IP address of the Raspberry Pi using `hostname -I` and use that in the SSH command:
    ```bash
    ssh pi@<raspberry_pi_ip>
    ```

- **Issue 2: SSL Certificate Generation Errors**

  *Description:* SSL certificate generation may fail due to incorrect openssl configuration or missing dependencies.

  *Solution:* 
  - Ensure that the `openssl` package is properly installed on your system:
    ```bash
    sudo apt-get install openssl
    ```
  - Double-check the directory paths where the key and certificate files are being generated. Ensure you have the necessary permissions to write to the specified directories.
  - Review the prompts carefully when generating the certificate, and ensure all required fields are filled in.

- **Issue 3: Raspberry Pi Honeypot Not Logging Data**

  *Description:* After completing the honeypot setup, you may find that no data is being logged to the DShield dashboard.

  *Solution:*
  - Confirm that the honeypot is exposed to the Internet and port forwarding is configured correctly on your router.
  - Run the following command to check if the honeypot service is running:
    ```bash
    sudo systemctl status dshield
    ```
  - Verify the API key was entered correctly during setup and the honeypot is communicating with DShield servers. You can check the logs using:
    ```bash
    tail -f /var/log/dshield.log
    ```


## License
This project is licensed under the MIT License.
