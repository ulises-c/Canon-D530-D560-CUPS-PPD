# Canon D530/D560 CUPS PPD
A way to use a Canon D530/D560 with CUPS print server via a PPD for Linux ARM distros

## Info
I used this to connect my Canon imageCLASS D530 to my Raspberry Pi 4 using CUPS

## Installation
Install CUPS as you usually would, but then use the PPD from this repo when configuring the printer.

## Printing
Device drivers are still needed on the device sending the print job. Drivers can be found on the official Canon website - https://www.usa.canon.com/internet/portal/us/home/products/details/printers/black-and-white-laser/d530

## Known Issues With This Method
AirPrint does not work

---

# UPDATE (2026-Feb-13)
Canon has released drivers for Linux so the setup on CUPS is pretty seamless now!
- Driver: https://www.usa.canon.com/support/p/imageclass-d530
- Version: v6.20


# Raspberry Pi 64-bit Print Server Guide (Canon D530)

**Driver Version:** v6.20

## 1. System Setup

```bash
sudo apt update
sudo apt install cups libxml2 libcups2 avahi-daemon -y
sudo usermod -a -G lpadmin $USER
sudo cupsctl --remote-any
sudo systemctl restart cups

```

## 2. Driver Installation (The "Ollie" Method)

To avoid shell errors with special characters, always use single quotes around the URL.

```bash
# 1. Setup folder
mkdir -p ~/CANON_D530_DRIVER && cd ~/CANON_D530_DRIVER

# 2. Download with quotes to handle the '&' character correctly
wget -O canon_v620.tar.gz 'https://pdisp01.c-wss.com/gdl/WWUFORedirectTarget.do?id=MDEwMDAwOTIzNjIy&cmp=ABR&lang=EN'

# 3. Extract
tar -zxvf canon_v620.tar.gz

# 4. Install via the ARM64 Subdirectory
cd linux-UFRII-drv-v620-us/ARM64/Debian/
sudo dpkg -i cnrdrvcups-ufr2-us_6.20-1.01_arm64.deb
sudo apt install -f

```

## 3. Printer Discovery

Ensure the `avahi-daemon` is running so your other devices can find the printer without an IP address:

```bash
sudo systemctl enable --now avahi-daemon

```

## 4. Final CUPS Step

1. Go to `http://<pi-ip>:631`.
2. Select **Add Printer**.
3. Choose the **CNUSBUFR2** entry.
4. Select the **Canon D530/D560 UFRII LT (en)** driver.
