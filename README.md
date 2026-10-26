# Cyber Attack Simulation Dashboard

### Cyber Defense Command Center (CDCC)

A cyberpunk-inspired dashboard for exploring cybersecurity concepts through simulated attack events, threat severity indicators, animated network visualizations, and a C++ threat-analysis simulator.

> **Academic project:** College-level group project completed under **Tinkering and Mentoring**. The project demonstrates front-end dashboard design, C++ object-oriented programming, and rule-based analysis of synthetic security events.

## Overview

The Cyber Attack Simulation Dashboard presents a visual command-center experience for learning about cyber-threat monitoring. It combines a static web interface with a standalone C++ program that generates synthetic packet-like records and scores them using simple heuristics.

The project is intended for **education and demonstration**, not for monitoring or protecting a real network.

## Features

- **Live-style attack feed** — displays periodically generated sample events with IP addresses, threat labels, severity levels, countries, and timestamps.
- **Animated global threat map** — visualizes simulated connections and attack paths; it does not geolocate real activity.
- **System monitor panels** — animated circular indicators and a scrolling network-traffic graph using demonstration values.
- **Threat-analysis visualization** — animated indicators for example threat categories such as malware and DDoS.
- **Interactive cyber terminal** — responds to preset commands with scripted demonstration output.
- **C++ threat simulation engine** — generates synthetic packet data, classifies it with heuristics, calculates severity scores, prioritizes events, and writes a JSON report.
- **OOP and algorithm demonstrations** — classes, structs, enums, STL containers, a priority queue, sorting/binary search, entropy calculation, file I/O, timestamps, and random data generation.
- **Responsive cyberpunk interface** — custom CSS, canvas effects, animated counters, and a mobile navigation menu.
- **Static deployment configuration** — includes a `vercel.json` file for serving the static site.

## Technology Stack

| Area | Technologies |
|---|---|
| Frontend | HTML5, CSS3, JavaScript |
| Utility styling | Tailwind CSS Play CDN |
| Visual effects | HTML Canvas, SVG, CSS animations |
| Simulator | C++17 |
| C++ standard library | `priority_queue`, `unordered_map`, `vector`, `algorithm`, `random`, `chrono`, file streams |
| Deployment configuration | Vercel static hosting (`vercel.json`) |

No frontend package manager or build step is required for the current static website. The Tailwind CSS CDN is loaded remotely, so an internet connection is needed for those utility styles.

## Project Structure

```text
Cyber-Attack-Simulation-Dashboard/
├── index.html       # Dashboard structure and page sections
├── style.css        # Cyberpunk theme, layout, and animations
├── main.js          # Simulated feed, charts, counters, map effects, and terminal
├── Analyzer.h       # C++ types, enums, and Analyzer interface
├── Analyzer.cpp     # Rule-based threat classification and scoring
├── Simulator.cpp    # Synthetic event generation and JSON output
├── vercel.json      # Static deployment and response-header configuration
├── .gitignore       # Ignores generated files and local tooling directories
└── README.md        # Project documentation
```

## Run the Dashboard

### Option 1: Open it directly

1. Download or clone this repository.
2. Open the project folder.
3. Open `index.html` in a modern browser.

### Option 2: Run a local web server (recommended)

From the project root, use Python if it is installed:

```bash
python -m http.server 8000
```

Then visit [http://localhost:8000](http://localhost:8000).

On Windows, `py -m http.server 8000` may be used if the `python` command is not available. Stop the server with `Ctrl+C`.

## Run the C++ Simulator

### Requirements

- A C++ compiler with C++17 support, such as GCC/G++ through MinGW-w64 on Windows or GCC/Clang on Linux/macOS.

### Compile

**Windows (PowerShell):**

```powershell
g++ Simulator.cpp Analyzer.cpp -o cdcc_sim.exe -std=c++17
```

**Linux/macOS:**

```bash
g++ Simulator.cpp Analyzer.cpp -o cdcc_sim -std=c++17
```

### Execute

**Windows (PowerShell):**

```powershell
.\cdcc_sim.exe          # Generate 50 events by default
.\cdcc_sim.exe 100      # Generate 100 events
```

**Linux/macOS:**

```bash
./cdcc_sim              # Generate 50 events by default
./cdcc_sim 100           # Generate 100 events
```

The simulator prioritizes generated events by threat score and writes a JSON report. In the current source, the output path is `../attack_data.json`, relative to the directory from which the executable is run. If you want the file written into the project root when running the command there, change the output filename in `Simulator.cpp` from `"../attack_data.json"` to `"attack_data.json"`.

### Current connection between the simulator and dashboard

The frontend attack feed in `main.js` currently generates random sample events directly in JavaScript. It does **not** fetch or parse the JSON written by the C++ simulator. The C++ simulator and the browser dashboard are therefore separate demonstration components in this version. Connecting them would require adding a data-loading step and serving the generated JSON over HTTP (rather than relying on a browser opening a local file).

## How the Threat Analyzer Works

The C++ `Analyzer` uses simple heuristic checks against synthetic packet-like fields. Examples include:

- SQL-like keywords in payload text.
- Script tags or `javascript:` strings as an XSS-like indicator.
- ICMP packets above a configured size threshold as a DDoS-like indicator.
- Certain destination ports and packet sizes as indicators of brute-force or port-scan-like activity.
- A predefined sample IP blacklist.
- Payload Shannon entropy as one input to analysis/scoring.

Events receive a numerical score and severity classification. A `std::priority_queue` orders events so higher-scored events are processed first; critical events are added to the simulator's in-memory blacklist.

These are teaching heuristics, not a validated detection engine. They can produce false positives and false negatives and should not be used to make real security decisions.

## C++ Concepts Demonstrated

| Concept | Project use |
|---|---|
| Classes and objects | `Analyzer` encapsulates analysis behavior and blacklist data |
| Encapsulation | Private blacklist and helper methods |
| Structs | `Packet` and `ThreatEvent` data structures |
| Enums | `Severity` and `ThreatType` categories |
| Operator overloading | `ThreatEvent::operator<` supports priority-queue ordering |
| STL priority queue | Processes high-score events first |
| Hash map | Blacklist lookup with `std::unordered_map` |
| Sorting and binary search | Maintains and searches a sorted blacklist |
| Algorithmic analysis | Shannon entropy calculation for payload strings |
| File I/O | Writes simulator results as JSON |
| Random number generation | Creates synthetic packet-like events |
| Chrono and threading | Timestamps and brief simulated arrival delays |

## Deployment

The project includes a `vercel.json` configuration for static hosting.

1. Push the repository to GitHub.
2. Import the repository into Vercel.
3. Deploy it as a static site; no build command is required for the current frontend.
4. Confirm that `index.html`, `style.css`, and `main.js` are located at the project root and are included in the deployment.

The C++ simulator is not deployed or executed by this static hosting configuration; run it separately in a local development environment.

## Academic Project Team

**Project:** Cyber Attack Simulation Dashboard (Cyber Defense Command Center)  
**Course / Program:** Tinkering and Mentoring  
**Project type:** College-level group project  
**Contributor:** Dhruv

Add your actual college/university name, teammates' names, GitHub profile links, and individual contributions before publishing. Do not list unconfirmed contributors.

## Limitations and Safety Notice

- All attacks, IP addresses, countries, counters, system metrics, and terminal responses shown in the browser are demonstration data.
- The dashboard does not capture real network packets, scan devices, block attacks, or provide live threat intelligence.
- The “AI” / threat-analysis panels are visual demonstrations; no trained machine-learning model is integrated in this repository.
- The C++ simulator generates synthetic input and applies basic heuristics. It is not connected to a real traffic source.
- Use this project for learning and authorized demonstrations only. Do not interpret its output as a real security alert or incident report.

## Future Improvements

- Connect the C++ JSON output to the dashboard through a local API or an explicit file-loading workflow.
- Add automated tests for the analyzer and simulator.
- Separate configuration from UI code and document expected data schemas.
- Replace demonstration values with a clearly documented, authorized data source if real telemetry is ever added.
- Add screenshots or a demo recording to the repository.

## License

No license file was included in the project archive. Add a `LICENSE` file before describing the repository as open source. Without an explicit license, others do not automatically receive permission to reuse or redistribute the code.

---

<p align="center">
  Built for learning, simulation, and cyber-defense awareness.
</p>
