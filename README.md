# BMS Desktop Monitor

Monitor and configure a **JK BMS** (battery management system) from your computer over **Bluetooth**, right in Chrome or Edge. You don't need to install anything or run an Android emulator.

**▶ Open the app: https://ryhan0912-arch.github.io/bms-monitor/**

> An independent, unofficial tool. It isn't made by, affiliated with or supported by Jikong / JK BMS.

---

## Features

- **Live status**: state of charge, pack voltage, current, power, remaining capacity, cycles, state of health, run time and estimated time to full or empty
- **Cell view**: voltage and wire resistance of every cell, with the highest and lowest cells highlighted, plus cell difference (delta) and average
- **Temperatures**: sensor 1 and 2, and MOSFET temperature
- **Live charts** of pack voltage and current for the last 10 minutes
- **Settings**: read and change protection settings (cell voltage limits, current limits, temperature limits, balancing, cell count, capacity). Every change asks you to confirm first.
- **Control**: switch Charge, Discharge and Balance on or off
- **Device info**: model, hardware/software version, serial number, uptime
- **Alarms & log**: 16 alarm types, an event log, and **Export CSV** of recorded data
- **Remembers paired BMS** and connects automatically. If you have several, it lets you choose one.
- **Reconnects automatically** if the connection drops
- **Demo mode** with simulated data, so you can try it without a BMS
- Light and dark theme, and it also works on small screens

## Requirements

| | |
|---|---|
| **Browser** | Google Chrome or Microsoft Edge (desktop or Android). Firefox and Safari don't support Web Bluetooth. |
| **Computer** | Windows 10/11, macOS or Linux with Bluetooth (built-in or a USB Bluetooth adapter) |
| **BMS** | JK BMS with Bluetooth, using the JK02 protocol (24-cell and 32-cell frame layouts are detected automatically) |

## Quick start

1. Turn on Bluetooth on your computer.
2. **Close the JK app on your phone.** The BMS accepts only one Bluetooth connection at a time.
3. Open **https://ryhan0912-arch.github.io/bms-monitor/** in Chrome or Edge.
4. Click **Pair new BMS**, choose your BMS (names usually start with `JK`), then click **Pair**.
5. The status page fills in after a few seconds.

> If your BMS was renamed and doesn't show up in the list, tick **All devices** before clicking **Pair new BMS**.

Want to look around first? Click **Demo**.

## Auto-connect (optional, one-time setup)

By default the browser forgets the BMS when you close the page. To make the app **remember your BMS and connect by itself**, turn on two browser settings once:

1. Open `chrome://flags/#enable-web-bluetooth-new-permissions-backend` and set it to **Enabled**.
2. Open `chrome://flags/#enable-experimental-web-platform-features` and set it to **Enabled**.
3. Click the **Relaunch** button at the bottom right of the page. *The settings only take effect after Chrome restarts.*

In Edge, use `edge://flags/...` instead of `chrome://flags/...`.

After that:

- **One saved BMS:** the app connects to it as soon as it opens.
- **Several saved BMS:** it shows a list and connects to the one you used last after 5 seconds. Click **Connect** on a different one to switch, or **Cancel**.
- **BMS off or out of range:** it waits and connects when the BMS appears.
- **Forget** removes a BMS from the list.

Bookmark the page (**Ctrl + D**) and open it from the bookmark from now on.

## Using the app

| Page | What it does |
|---|---|
| **Status** | Overview, all cells, temperatures and live charts |
| **Settings** | Click **Read settings from BMS**, type a new value, then click **Write** |
| **Control** | Charge, Discharge and Balance switches |
| **Device info** | Model, versions, serial number. Also lets you choose the protocol layout and shows the raw data |
| **Alarms & log** | Active alarms, connection log and **Export CSV** |

### ⚠ Safety

Changing protection settings or switches changes your BMS **immediately**. Wrong values can damage your battery or create a fire risk. Before writing anything:

- First check that the numbers in the app match the official JK app.
- Change one setting at a time, and only if you know what it does.
- You use this tool at your own risk.

## Privacy

- The app is a single static web page. **Your battery data never leaves your computer**: it goes straight from the BMS to your browser over Bluetooth.
- No accounts, no tracking, no servers. Saved BMS names and settings are stored only in your browser.
- Bluetooth only reaches a few metres, so nobody can reach your BMS through this website.

## Troubleshooting

| Problem | What to try |
|---|---|
| "Bluetooth is not available" | Use Chrome or Edge, and turn on Bluetooth in Windows Settings. |
| BMS doesn't appear in the list | Close the JK app on your phone, move closer, or tick **All devices**. |
| Connects but values look wrong | **Device info → Protocol**: try the *32 cells* or *24 cells* option. |
| Temperatures show 0 °C or odd numbers | Same as above: switch the protocol layout. |
| Doesn't auto-connect | Check that both flags are **Enabled** *and* that you clicked **Relaunch**. Then pair the BMS once more. |
| Connection keeps dropping | Move the computer closer to the BMS, or use a better USB Bluetooth adapter. |

If something still doesn't work, open an issue and include the text from **Device info → Last cell frame (hex)** and your BMS hardware version.

## How it works

- Built with plain HTML, CSS and JavaScript in one file ([`index.html`](index.html)), with no libraries.
- Uses the **Web Bluetooth API** to talk to the BMS through BLE service `FFE0`, characteristic `FFE1`.
- Decodes the JK02 protocol frames: device info (`0x03`), settings (`0x01`) and cell data (`0x02`). It writes settings with `AA 55 90 EB` command frames.
- The protocol details come from public community documentation of the JK BMS Bluetooth protocol.

### Run it locally

Download `index.html` and serve it from any local web server, for example:

```bash
python -m http.server 8000
```

Then open `http://localhost:8000`. Web Bluetooth needs `https://` or `localhost`.

## Disclaimer

This software is provided "as is", without warranty of any kind. JK BMS is a trademark of its owner. This project isn't affiliated with or endorsed by Jikong.
