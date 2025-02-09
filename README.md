[Versión en Español](README_es.md)

# Nsniffer – Network Analysis and Security Tool

## Description

[Nsniffer](https://github.com/vorosdev/Nsniffer) is a multifunctional network tool 
designed to capture and analyze network traffic, perform Man-in-the-Middle (MITM) attacks, 
scan devices on the network, and carry out other tasks related to network security and 
auditing. It is particularly aimed at educational purposes and penetration testing in 
controlled environments.

**Important:** This software is intended for ethical and educational use. Make sure you have 
the necessary permissions before using it.

---

## Features

- **Real-time traffic capture** using TShark.
- **Saving captures** in `.pcap` or `.pcapng` format.
- **Extraction of data in hexadecimal format** and conversion to readable text.
- **ARP spoofing attacks** and **Man-in-the-Middle** capabilities.
- **Scanning of devices** on the local network.
- **Analysis of capture files**.
- **Listing available network interfaces**.
- **Interval-based capture and logging** of extracted data.
- **Automated dependency installation** (on supported systems).

---

## General Usage

In this update, `nsniffer` uses **subcommands** instead of a single command with multiple flags.  
The general usage is:

```bash
nsniffer <subcommand> [options]
```

Where `<subcommand>` can be one of the following:

- **capture**: Capture traffic with TShark.  
- **analyze**: Analyze a `.pcap` file.  
- **mitm**: Perform a MITM attack between two targets.  
- **arp**: Carry out simple ARP spoofing between a target and a victim.  
- **scan**: Scan devices on the local network.  
- **list**: List available network interfaces.  
- **extract**: Extract hexadecimal data and convert it into readable text from a pcap file.  
- **log**: Capture traffic at defined intervals and save the extracted data into a log file.  
- **setup**: Install or verify necessary dependencies.  
- **help**: Show the tool’s help information.

To see a description and available options for each subcommand, you can run:
```bash
nsniffer help
```

---

## Requirements

- **Linux** operating system.
- **Superuser (root)** privileges to perform most sniffing and spoofing operations.
- Necessary dependencies:
  - **TShark**
  - **arpspoof** (included in the **dsniff** suite)
  - **arp-scan**

To install or verify dependencies on supported distributions (Debian, RedHat, Arch), run:
```bash
nsniffer setup
```
This will attempt to install any missing dependencies.

---

## Installation

1. **Clone this repository**:
   ```bash
   git clone https://github.com/vorosdev/Nsniffer.git
   cd Nsniffer
   ```
2. **Grant execution permissions**:
   ```bash
   chmod +x nsniffer
   ```
3. **(Optional) Add it to your PATH** so you can use it from anywhere:
   ```bash
   sudo cp nsniffer /usr/local/bin/nsniffer
   ```

---

## Main Subcommands and Examples

### 1. Traffic Capture (capture)

```bash
nsniffer capture -i <interface> [-o <file.pcap>] [-f <filter>] [--live]
```
- **-i, --interface**: Network interface, e.g., `eth0`.
- **-o, --output**: Output file. Defaults to `output.pcap`.
- **-f, --filter**: TShark filter (e.g., `"host 172.10.23.111 and port 2222 and tcp"`).
- **--live**: Displays captured packets in real-time (`-x` in TShark).

**Example**:
```bash
nsniffer capture -i eth0 -o capture.pcap -f "port 443 and tcp" --live
```

### 2. Analyze a pcap File (analyze)

```bash
nsniffer analyze <file.pcap>
```
Outputs a textual analysis of the specified file.

**Example**:
```bash
nsniffer analyze capture.pcap
```

### 3. Man-in-the-Middle (mitm)

```bash
nsniffer mitm <interface> <target_IP1> <target_IP2>
```
Enables IP forwarding and runs `arpspoof` for both addresses.

**Example**:
```bash
nsniffer mitm eth0 192.168.0.10 192.168.0.20
```

### 4. Simple ARP Spoofing (arp)

```bash
nsniffer arp <interface> <target_IP> <victim_IP>
```
Starts `arpspoof` from one target to a specific victim, without full MITM.

**Example**:
```bash
nsniffer arp eth0 192.168.0.1 192.168.0.50
```

### 5. Network Scan (scan)

```bash
nsniffer scan <interface>
```
Performs an ARP scan using `arp-scan`.

**Example**:
```bash
nsniffer scan eth0
```

### 6. List Interfaces (list)

```bash
nsniffer list
```
Displays the network interfaces available on the system.

### 7. Hexadecimal Data Extraction (extract)

```bash
nsniffer extract <file.pcap>
```
Looks for frames larger than 100 bytes and converts hexadecimal data to readable text.

**Example**:
```bash
nsniffer extract capture.pcap
```

### 8. Interval-Based Capture and Logging (log)

```bash
nsniffer log <interface> <log_file> [interval_in_seconds=5]
```
Starts a capture loop: every specified interval (5s by default), it stops the capture, extracts hex data, and writes it to `log_file`.

**Example**:
```bash
nsniffer log eth0 mycapture.log 10
```
Every 10 seconds, it temporarily stops the capture, extracts data, and saves it to `mycapture.log`, then restarts.

### 9. Setup (setup)

```bash
nsniffer setup
```
Checks and installs required dependencies (TShark, arpspoof, arp-scan) if the operating system is supported.

### 10. Help (help)

```bash
nsniffer help
```
Displays a general description of all subcommands and their usage.

---

## Additional Examples

#### Scan the Local Network

```bash
nsniffer scan eth0
```
Finds active hosts in the subnet assigned to `eth0` using `arp-scan`.

#### Perform a MITM Attack

```bash
nsniffer mitm eth0 192.168.1.10 192.168.1.20
```
Attacks the communication between IP 192.168.1.10 and 192.168.1.20 via the `eth0` interface.

#### Capture and Convert Data in "log" Mode

```bash
nsniffer log eth0 capture.log
```
Every 5 seconds (by default), it stops the capture, extracts hexadecimal data, and appends it to `capture.log`.  
Press **Enter** to restart the cycle or **Ctrl+C** to exit.

---

> **Warning**  
> - Use this software **only** on networks under your control or with explicit authorization.  
> - Malicious use of this tool can be **illegal** and may lead to serious consequences.

---

## License

This project is licensed under the **MIT** license. See the [LICENSE](LICENSE) file for more information.

---

## Disclaimer

This project is intended for educational and ethical purposes only. The author is not responsible for 
any misuse of this tool. For more information, see the [DISCLAIMER.md](DISCLAIMER.md) file.
