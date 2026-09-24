# Huawei E8372h-608 IMEI Restoration Guide

This guide documents the procedure to restore the original IMEI on a Huawei E8372h-608 LTE USB modem via Telnet (commonly needed after firmware updates or NVRAM corruption).

---

> [!WARNING]
> **Legal & Compliance Notice**: Altering or spoofing an IMEI number is illegal in many jurisdictions. Always restore the device's original IMEI located on the physical sticker beneath the modem casing. Do not use this procedure to clone or disguise devices on cellular networks.

---

## Prerequisites

- Huawei E8372h-608 modem connected to PC
- Telnet access enabled on the modem firmware
- Telnet client (e.g., PuTTY, Windows Telnet, or terminal)

---

## Step-by-Step Instructions

### 1. Connect via Telnet

Connect to your modem's IP address (default is usually `192.168.8.1` or `192.168.1.1` on port `23`):

```bash
telnet 192.168.8.1
```

Log in if prompted (defaults are typically `admin` / `admin` or root access without a password, depending on the custom firmware).

---

### 2. Write the IMEI

Send the AT command through the modem's virtual serial port (`/dev/appvcom`):

```bash
busybox echo -e "AT^PHYNUM=IMEI,867792051181331\r" > /dev/appvcom
```

> [!NOTE]
> Replace `867792051181331` with your device's original 15-digit IMEI printed on the label under the cap.

---

### 3. Wait 3 Seconds

Allow the modem's NVRAM to commit the change:

```bash
sleep 3
```

---

### 4. Verify the IMEI

Query the modem to confirm the updated IMEI has been registered:

```bash
busybox echo -e "AT+CGSN\r" > /dev/appvcom
```

---

### 5. Wait 3 Seconds

Allow the query to finish:

```bash
sleep 3
```

---

### 6. Reboot the Modem

Reboot the device to apply changes and reinitialize cellular radios:

```bash
reboot
```

---

## Quick Shell Script

You can also run all steps sequentially as a one-liner inside the Telnet session:

```bash
busybox echo -e "AT^PHYNUM=IMEI,867792051181331\r" > /dev/appvcom && sleep 3 && busybox echo -e "AT+CGSN\r" > /dev/appvcom && sleep 3 && reboot
```
