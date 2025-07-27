# 🚀 Splunk Universal Forwarder Setup Guide

This guide walks you through installing, configuring, and managing the **Splunk Universal Forwarder (UF)** on a Linux machine to forward logs to a Splunk Enterprise server.

---

## 🔍 What is a Universal Forwarder?

The Splunk Universal Forwarder (UF) is a lightweight agent that collects and forwards log or event data from remote systems to a central Splunk instance. It's optimized for low resource usage and commonly used on servers and endpoints for real-time data streaming.

### Key Features:
- Collects local logs and events
- Forwards data securely over TCP/SSL
- Low resource footprint
- Easily configurable and managed via CLI

Use cases include:
- Forwarding syslog, application, and audit logs
- Centralizing security and system monitoring
- Feeding data into SIEM pipelines for analysis

---

## 📦 Prerequisites

- [AWS](https://aws.amazon.com/) Account
- A Linux-based server created via LightSail (e.g., Ubuntu, CentOS)
- Admin or root access
- Splunk Enterprise instance configured to receive data on port `9997`
- Splunk Universal Forwarder download link from [Splunk Downloads](https://www.splunk.com/en_us/download/universal-forwarder.html)

---

## 🔧 Installation Steps

1. **Switch to root user**:
   ```bash
   sudo su

2. Create Splunk user and target directory
   ```bash
    useradd splunk
    mkdir /opt/splunkforwarder/

3. Download the Splunk UF installer
Replace <download-link> with the wget link provided by Splunk.
   ```bash
    cd /opt/
    wget <download-link>

4. Verify download and extract
   ```bash
    ls  # to confirm the package is there
    
    # Extract the package (replace the .tgz filename with your actual file)
    tar xvzf splunkforwarder-<version>-Linux-x86_64.tgz -C /opt


5. Set ownership of Splunk UF files
   ```bash
    chown -R splunk:splunk /opt/splunkforwarder

6. Start the Splunk Universal Forwarder
   ```bash
   cd /opt/splunkforwarder/bin/
   ./splunk start --accept-license


⚙️ Log Forwarding Configuration

1. Enable receiving on Splunk Enterprise
On your Splunk Enterprise instance:
 - Go to: Settings > Forwarding and Receiving > Configure Receiving
 - Enable TCP port 9997

2. Copy default config files to local directory
   ```bash
   cd /opt/splunkforwarder/etc/system/default/
   cp props.conf inputs.conf outputs.conf /opt/splunkforwarder/etc/system/local/

3. Navigate to the local config directory
   ```bash
   cd /opt/splunkforwarder/etc/system/local/

4. Check forwarder status
   ```bash
   cd /opt/splunkforwarder/bin/
   ./splunk status

5. Add the Splunk Enterprise server as a forward target
   ```bash
   ./splunk add forward-server <splunk-enterprise-ip>:9997
  - You will be prompted to enter the Splunk admin username and password.

🧹 Removing a Forwarder
To remove a specific forwarder target:
   ```bash
   ./splunk remove forward-server <splunk-enterprise-ip>:9997  
