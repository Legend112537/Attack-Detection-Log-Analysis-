# 🛡️ Attack Detection & Log Analysis System

<div align="center">

![Python](https://img.shields.io/badge/Python-3.9+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)
![Security](https://img.shields.io/badge/Security-Focused-red?style=for-the-badge&logo=shield&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

**A modular, real-time log analysis and intrusion detection system with an interactive web dashboard.**

[Features](#-features) • [Installation](#-installation) • [Usage](#-usage) • [Dashboard](#-dashboard) • [Detection Rules](#-detection-rules) • [Contributing](#-contributing)

</div>

---

## 📋 Overview

The **Attack Detection & Log Analysis System** is a Python-based security tool that parses, analyzes, and visualizes system/web server logs to detect malicious activity in real-time. It supports multiple log formats, implements detection rules for common attack patterns, and provides a beautiful interactive dashboard.

```
┌─────────────────────────────────────────────────────────────┐
│              Attack Detection & Log Analysis                 │
│                                                             │
│  Log Files ──► Parser ──► Detector ──► Alerts & Reports    │
│                              │                              │
│                              ▼                              │
│                      Web Dashboard                          │
└─────────────────────────────────────────────────────────────┘
```

---

## ✨ Features

| Feature | Description |
|--------|-------------|
| 🔍 **Multi-Format Parsing** | Apache, Nginx, Syslog, JSON, Windows Event Logs |
| 🚨 **Attack Detection** | Brute Force, SQLi, XSS, Path Traversal, DDoS, Port Scan |
| 📊 **Interactive Dashboard** | Real-time charts, IP geolocation, threat timeline |
| 🌍 **IP Reputation** | Flags known malicious IPs against threat intel feeds |
| 📧 **Alerting** | Email and Webhook (Slack/Discord) notifications |
| 📁 **Report Generation** | HTML, JSON, and CSV reports |
| ⚡ **Real-time Monitoring** | File tailing with live dashboard updates |
| 🔌 **Extensible Rules** | YAML-based custom detection rules |

---

## 🚀 Installation

### Prerequisites
- Python 3.9+
- pip

### Quick Start

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/attack-detection-log-analysis.git
cd attack-detection-log-analysis

# Create virtual environment
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Run setup
python setup.py install

# Generate sample logs (optional, for testing)
python scripts/generate_sample_logs.py

# Start the analyzer
python main.py --log logs/samples/apache_sample.log --format apache

# Launch the dashboard
python dashboard/app.py
```

---

## 💻 Usage

### Command Line Interface

```bash
# Analyze a single log file
python main.py --log /var/log/apache2/access.log --format apache

# Watch a log file in real-time
python main.py --log /var/log/nginx/access.log --format nginx --watch

# Analyze with custom rules
python main.py --log myapp.log --format json --rules config/custom_rules.yaml

# Generate an HTML report
python main.py --log access.log --format apache --report html --output report.html

# Set alert threshold
python main.py --log access.log --format apache --threshold 10

# Analyze multiple files
python main.py --log "logs/*.log" --format apache --glob
```

### Python API

```python
from src.parsers import LogParser
from src.detectors import AttackDetector
from src.utils.reporter import Reporter

# Parse logs
parser = LogParser(log_format="apache")
entries = parser.parse_file("access.log")

# Detect attacks
detector = AttackDetector()
alerts = detector.analyze(entries)

# Generate report
reporter = Reporter()
reporter.generate_html(alerts, output="report.html")

print(f"Found {len(alerts)} potential attacks!")
for alert in alerts:
    print(f"  [{alert.severity}] {alert.attack_type} from {alert.ip}")
```

---

## 📊 Dashboard

Launch the interactive web dashboard:

```bash
python dashboard/app.py
# Open http://localhost:5000
```

**Dashboard Features:**
- 📈 Live request timeline
- 🗺️ Attack origin world map
- 🔴 Top attacking IPs
- 📋 Alert feed with severity levels
- 🥧 Attack type distribution chart
- 🔎 Log search & filter

---

## 🔍 Detection Rules

### Built-in Detectors

| Detector | What it Catches |
|----------|----------------|
| `BruteForceDetector` | Multiple failed logins from same IP |
| `SQLInjectionDetector` | SQL keywords in URL params / POST body |
| `XSSDetector` | Script injection attempts in requests |
| `PathTraversalDetector` | `../` sequences, `/etc/passwd` patterns |
| `DDoSDetector` | Abnormal request volume per IP/timeframe |
| `ScannerDetector` | Rapid sequential port or endpoint probing |
| `CommandInjectionDetector` | Shell metacharacters in inputs |
| `UserAgentDetector` | Known malicious/scanner user agents |

### Custom YAML Rules

```yaml
# config/custom_rules.yaml
rules:
  - name: "Admin Panel Probing"
    description: "Detects repeated access attempts to admin endpoints"
    pattern: "/(admin|wp-admin|phpmyadmin|manager|console)"
    threshold: 5
    window_seconds: 60
    severity: HIGH
    action: alert

  - name: "Suspicious File Extension"
    description: "Access to potentially dangerous file types"
    pattern: "\.(php|asp|aspx|jsp|cgi|pl)$"
    threshold: 1
    severity: MEDIUM
    action: log
```

---

## 📁 Project Structure

```
attack-detection-log-analysis/
├── main.py                    # CLI entry point
├── requirements.txt           # Python dependencies
├── setup.py                   # Package setup
├── config/
│   ├── settings.yaml          # Global configuration
│   └── custom_rules.yaml      # Custom detection rules
├── src/
│   ├── parsers/
│   │   ├── __init__.py
│   │   ├── base_parser.py     # Abstract base parser
│   │   ├── apache_parser.py   # Apache/CLF log parser
│   │   ├── nginx_parser.py    # Nginx log parser
│   │   ├── syslog_parser.py   # Syslog parser
│   │   └── json_parser.py     # JSON log parser
│   ├── detectors/
│   │   ├── __init__.py
│   │   ├── base_detector.py   # Abstract base detector
│   │   ├── brute_force.py     # Brute force detection
│   │   ├── sql_injection.py   # SQLi detection
│   │   ├── xss_detector.py    # XSS detection
│   │   ├── path_traversal.py  # Path traversal detection
│   │   ├── ddos_detector.py   # DDoS/flood detection
│   │   └── scanner.py         # Port/endpoint scan detection
│   └── utils/
│       ├── __init__.py
│       ├── alert.py           # Alert model
│       ├── reporter.py        # Report generation
│       ├── notifier.py        # Email/webhook alerts
│       └── geoip.py           # IP geolocation
├── dashboard/
│   ├── app.py                 # Flask web server
│   ├── templates/
│   │   └── index.html         # Dashboard HTML
│   └── static/
│       └── dashboard.js       # Dashboard JS
├── logs/
│   └── samples/               # Sample log files
├── tests/
│   ├── test_parsers.py
│   ├── test_detectors.py
│   └── test_reporter.py
├── scripts/
│   └── generate_sample_logs.py
└── docs/
    └── DETECTION_RULES.md
```

---

## ⚙️ Configuration

Edit `config/settings.yaml`:

```yaml
analyzer:
  max_workers: 4
  chunk_size: 1000

detection:
  brute_force:
    threshold: 5          # Failed attempts
    window_seconds: 300   # 5 minutes
  ddos:
    threshold: 1000       # Requests per minute
  
alerts:
  email:
    enabled: false
    smtp_host: smtp.gmail.com
    smtp_port: 587
    from: alerts@yourdomain.com
    to: security@yourdomain.com
  webhook:
    enabled: false
    url: https://hooks.slack.com/services/YOUR/WEBHOOK/URL

reporting:
  output_dir: reports/
  formats: [html, json, csv]
```

---

## 🧪 Running Tests

```bash
# Run all tests
pytest tests/ -v

# Run with coverage
pytest tests/ --cov=src --cov-report=html

# Run specific test module
pytest tests/test_detectors.py -v
```

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/new-detector`
3. Commit your changes: `git commit -m 'Add new detector for XYZ'`
4. Push to branch: `git push origin feature/new-detector`
5. Open a Pull Request

See [CONTRIBUTING.md](docs/CONTRIBUTING.md) for detailed guidelines.

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## ⚠️ Disclaimer

This tool is intended for **defensive security purposes only** — monitoring your own systems and networks. Do not use it to analyze systems you do not own or have explicit permission to test.

---

<div align="center">
Made with ❤️ for the security community
</div>
