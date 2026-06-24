# wigle-wardriver

portable ESP32-based WiFi and Bluetooth scanner for Wigle.net. scans all 802.11 and BLE beacons in range, uploads geolocation results to Wigle (with user auth), and logs local cache for offline use.

## hardware

this is the rev3 dual-board design: two ESP32s split the work, linked over serial at 115200.

- **Side A** (`A/A.ino`) — GPS (MicroNMEA), SD card, 802.11 scan, OTA update
- **Side B** (`B/B.ino`) — BLE scan (NimBLE), SIM800L cellular
- **GPS module** — location tagging via NMEA
- **OLED display** — scan/upload status
- **SD card** — local log store

A and B are the two halves of one device, not alternative builds.

## features

- **802.11 beacon scan** — SSID, BSSID, channel, signal strength, encryption type
- **BLE advertisement capture** — address, manufacturer data, RSSI
- **Wigle upload** — authenticated submission with timestamp and GPS location
- **offline cache** — local JSON log if WiFi unavailable
- **display** — optional OLED for live scan count and upload status

## usage

1. flash Side A and Side B to their respective ESP32s
2. set WiFi and Wigle credentials via the device's config interface
3. power on — the unit scans 802.11 and BLE continuously, tags observations with GPS, logs to SD
4. logged observations feed Wigle for crowdsourced 802.11 geolocation

## wigle integration

- requires Wigle account and API key
- each upload includes: BSSID, SSID, frequency band, signal level, timestamp, GPS (if available)
- Wigle uses crowdsourced data for 802.11 geolocation accuracy improvement

## lineage

based on Joseph Hewitt's wardriver (hardware revision 3). location comes from the onboard GPS via NMEA; there is no IP-geolocation path.
