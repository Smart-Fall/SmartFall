# SmartFall

SmartFall is a wearable fall detection system. A sensor device worn by the patient detects falls in real time and reports them, along with health readings, to a web platform where patients, caregivers and admins can follow what is happening.

This repository is the single entry point for the project. It holds no code of its own: the two parts live in their own repositories and are included here as git submodules.

| Folder | Repository | What it is |
|--------|------------|------------|
| [`Hardware/`](https://github.com/Smart-Fall/Hardware) | `Smart-Fall/Hardware` | ESP32 firmware for the wearable (MPU6050, BMP280, MAX30102, FSR, audio). Runs the fall detection algorithm and sends alerts over WiFi, with BLE as fallback. |
| [`Software/`](https://github.com/Smart-Fall/Software) | `Smart-Fall/Software` | Next.js web app and API: patient and caregiver dashboards, fall history, health logs and device management. Live at [smartfall.vercel.app](https://smartfall.vercel.app). |

## How the parts fit together

```
 ┌──────────────────────┐   HTTPS (WiFi)    ┌──────────────────────────┐
 │  Hardware (ESP32)    │ ────────────────▶ │  Software (Next.js app)   │
 │  sensors + fall      │   /api/falls      │  API + database           │
 │  detection algorithm │   /api/device/*   │  patient / caregiver UI   │
 └──────────────────────┘   /api/health     └──────────────────────────┘
```

1. The wearable samples its sensors and scores each event through a 5-stage algorithm (free fall, impact, rotation, inactivity, validation).
2. When the confidence score is high enough, it posts an emergency alert to the web app. It also streams sensor data and device status.
3. The web app stores falls and health data, and shows them on the patient and caregiver dashboards.

The server URL used by the firmware is set in `Hardware/SmartFall/Config.h` (`SERVER_BASE_URL`), and the device API key there must match `DEVICE_INGEST_API_KEY` on the server.

## Getting the code

Clone with `--recursive` so both submodules are fetched:

```bash
git clone --recursive https://github.com/Smart-Fall/SmartFall.git
```

If you already cloned without it:

```bash
git submodule update --init --recursive
```

To pull the latest commit of each part's `main` branch:

```bash
git submodule update --remote --merge
```

A submodule is pinned to a specific commit. After updating, commit the new pointers in this repository so everyone gets the same versions:

```bash
git add Hardware Software
git commit -m "Update submodules"
```

## Working on a part

Each folder is a full git repository. Make changes, commit and open pull requests inside `Hardware/` or `Software/` as you would in a normal clone; those changes go to that part's own repository. Setup instructions are in each part's README:

- [Hardware README](https://github.com/Smart-Fall/Hardware#readme) and the [full firmware docs](https://smart-fall.github.io/Hardware)
- [Software README](https://github.com/Smart-Fall/Software#readme)
