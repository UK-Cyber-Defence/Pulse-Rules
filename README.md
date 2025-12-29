# Pulse-Rules

Open-source detection rules and decoders for use in OSSEC, Wazuh and Pulse SIEM.

## Overview

This repository contains a collection of open-source detection rules and decoders developed by UK Cyber Defence for enhanced security monitoring and threat detection. These rules are designed to work with OSSEC, Wazuh, and Pulse SIEM platforms, providing advanced detection capabilities for various security threats.

## Repository Structure

```
Pulse-Rules/
├── decoders/          # Custom log decoders
│   └── mongobleed.xml # MongoDB log decoder for CVE-2025-14847 detection
└── rules/             # Detection rules
    └── 701001-mongobleed.xml # MongoBleed exploitation detection rules
```

## Available Rules and Decoders

### MongoBleed (CVE-2025-14847) Detection

**Decoders:** `decoders/mongobleed.xml`
- MongoDB JSON connection accepted decoder
- MongoDB JSON client metadata decoder
- MongoDB JSON connection closed decoder

**Rules:** `rules/701001-mongobleed.xml`
- **Rule 100510**: High-confidence detection for extreme connection bursts (≥400 connections/min) - Level 12
- **Rule 100511**: Sustained abnormal connection volume detection (≥600 connections/5min) - Level 10
- MITRE ATT&CK mapping: T1190 (Exploit Public-Facing Application)

## Installation Instructions

### Installing in Wazuh

#### Prerequisites
- Wazuh Manager installed and running
- SSH or local access to the Wazuh Manager server
- Appropriate permissions to modify Wazuh configuration files

#### Installation Steps

1. **Clone the repository**
   ```bash
   git clone https://github.com/UK-Cyber-Defence/Pulse-Rules.git
   cd Pulse-Rules
   ```

2. **Install Decoders**
   
   Copy the decoder files to the Wazuh custom decoders directory:
   ```bash
   sudo cp decoders/*.xml /var/ossec/etc/decoders/
   ```

3. **Install Rules**
   
   Copy the rule files to the Wazuh custom rules directory:
   ```bash
   sudo cp rules/*.xml /var/ossec/etc/rules/
   ```

4. **Set Correct Permissions**
   
   Ensure the files have the correct ownership and permissions:
   ```bash
   sudo chown wazuh:wazuh /var/ossec/etc/decoders/*.xml
   sudo chown wazuh:wazuh /var/ossec/etc/rules/*.xml
   sudo chmod 640 /var/ossec/etc/decoders/*.xml
   sudo chmod 640 /var/ossec/etc/rules/*.xml
   ```

5. **Verify Configuration**
   
   Test the Wazuh configuration to ensure there are no syntax errors:
   ```bash
   sudo /var/ossec/bin/wazuh-logtest
   ```
   
   Or verify the configuration files:
   ```bash
   sudo /var/ossec/bin/wazuh-control check
   ```

6. **Restart Wazuh Manager**
   
   Restart the Wazuh Manager to load the new rules and decoders:
   ```bash
   sudo systemctl restart wazuh-manager
   ```

7. **Verify Installation**
   
   Check the Wazuh Manager logs to confirm successful loading:
   ```bash
   sudo tail -f /var/ossec/logs/ossec.log
   ```
   
   You can also test specific rules using wazuh-logtest:
   ```bash
   sudo /var/ossec/bin/wazuh-logtest
   ```

#### Alternative: Individual File Installation

If you prefer to install specific rules or decoders individually:

```bash
# Install specific decoder
sudo cp decoders/mongobleed.xml /var/ossec/etc/decoders/

# Install specific rule
sudo cp rules/701001-mongobleed.xml /var/ossec/etc/rules/

# Set permissions
sudo chown wazuh:wazuh /var/ossec/etc/decoders/mongobleed.xml
sudo chown wazuh:wazuh /var/ossec/etc/rules/701001-mongobleed.xml
sudo chmod 640 /var/ossec/etc/decoders/mongobleed.xml
sudo chmod 640 /var/ossec/etc/rules/701001-mongobleed.xml

# Restart Wazuh Manager
sudo systemctl restart wazuh-manager
```

### Installing in OSSEC

The installation process for OSSEC is similar to Wazuh:

```bash
# Install decoders
sudo cp decoders/*.xml /var/ossec/etc/decoders/

# Install rules
sudo cp rules/*.xml /var/ossec/rules/

# Set permissions
sudo chown ossec:ossec /var/ossec/etc/decoders/*.xml
sudo chown ossec:ossec /var/ossec/rules/*.xml
sudo chmod 640 /var/ossec/etc/decoders/*.xml
sudo chmod 640 /var/ossec/rules/*.xml

# Restart OSSEC
sudo /var/ossec/bin/ossec-control restart
```

## Usage

Once installed, the rules will automatically process logs and generate alerts when suspicious activity is detected. Alerts can be viewed through:

- Wazuh Dashboard (Kibana)
- Wazuh API
- Alert logs: `/var/ossec/logs/alerts/alerts.log`
- JSON alerts: `/var/ossec/logs/alerts/alerts.json`

## Troubleshooting

### Rules Not Loading

If rules are not loading, check:
1. XML syntax is valid
2. File permissions are correct (640 with wazuh:wazuh ownership)
3. Files are in the correct directories
4. Wazuh Manager service is running
5. Check logs for errors: `sudo tail -f /var/ossec/logs/ossec.log`

### Testing Rules

Use `wazuh-logtest` to test if your rules are working:

```bash
sudo /var/ossec/bin/wazuh-logtest
```

Then paste sample log entries to see if they match the decoders and trigger rules.

## Contributing

Contributions are welcome! If you have additional rules or decoders to share:

1. Fork the repository
2. Create a feature branch
3. Add your rules/decoders following the existing structure
4. Test thoroughly with Wazuh/OSSEC
5. Submit a pull request with a clear description

## Support

For issues, questions, or contributions, please open an issue on the [GitHub repository](https://github.com/UK-Cyber-Defence/Pulse-Rules).

## License

Please refer to the repository license file for licensing information.

## Acknowledgments

Developed and maintained by UK Cyber Defence team.
