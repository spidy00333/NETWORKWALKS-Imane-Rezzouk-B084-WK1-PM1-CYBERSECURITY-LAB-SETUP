# Cybersecurity Lab Environment Setup

Building an isolated virtual lab for ethical hacking and penetration testing practice.

## Project Overview
This project sets up a virtual cybersecurity lab on my own PC using VirtualBox and Kali Linux. The lab runs on a private virtual network, so more machines can be added later and used as targets for authorized security testing only.

## Lab Environment
- Host OS: [Windows 11]
- Hypervisor: VirtualBox [version]
- Attacker VM: Kali Linux 2026.3 (VirtualBox image)
- Network: NAT Network "NatNetwork", 10.0.0.0/24, DHCP enabled
- Kali static IP: 10.0.0.2/24

## Setup Steps
1. Installed VirtualBox and 7-Zip on the host.
2. Created a NAT Network (10.0.0.0/24) under File > Tools > Network.
   ![NAT Network]<img width="730" height="382" alt="Screenshot 2026-11-04 185406" src="https://github.com/user-attachments/assets/a86c6f66-aeb2-4e72-96fe-23445a184288" />

3. Downloaded the Kali VirtualBox image, extracted it with 7-Zip, and added the .vbox file to VirtualBox.
4. Configured the VM: Adapter 1 attached to NAT Network "NatNetwork", Promiscuous Mode set to Allow All.
   ![VM network settings]<img width="980" height="697" alt="Screenshot 2026-11-04 190850" src="https://github.com/user-attachments/assets/e7fb24dd-38e7-4e0c-a54b-e56551fe1c79" />

5. Enabled Shared Clipboard and Drag'n'Drop (Bidirectional) and shared the host Downloads folder with Auto-mount and Full access.

6. Set the static IP inside Kali (10.0.0.2/24, gateway 10.0.0.1, DNS 8.8.8.8).
7. Took a snapshot of the clean installation.
   ![Snapshot]<img width="1920" height="1080" alt="Screenshot 2026-11-04 194430" src="https://github.com/user-attachments/assets/27c9d558-29b5-483e-bf4b-1561919caa29" />


## Verification
- IP address check (`ip a`) shows 10.0.0.2/24 on eth0
  ![ip a]<img width="475" height="201" alt="image" src="https://github.com/user-attachments/assets/6ae9e103-39ef-44bb-adac-315991b802e0" />

- Internet check: ping to 8.8.8.8 and google.com both succeed
  ![ping]<img width="485" height="151" alt="image" src="https://github.com/user-attachments/assets/1663b4b1-5449-45df-a5c4-4f7ac02476c7" />
<img width="516" height="246" alt="image" src="https://github.com/user-attachments/assets/b151bdc3-514f-478c-a101-8714b28c7b3c" />

- Shared folder is visible in Kali as sf_Downloads
- Clipboard copy/paste works from host to Kali

## Troubleshooting
**Problem:** Kali had internet, but `ip a` showed 10.0.0.3 marked "dynamic" instead of the required 10.0.0.2. The static IP had not been applied and the connection was still using DHCP.

**Solution:** I set the static address from the terminal with `nmcli` (method manual, address 10.0.0.2/24, gateway 10.0.0.1, DNS 8.8.8.8, dad-timeout 0), then restarted the connection with `nmcli connection down` and `up`. `ip a` then showed 10.0.0.2/24 without "dynamic" and internet still worked.

**Problem:** Paste did not seem to work in the Kali terminal.
**Solution:** The terminal uses Ctrl+Shift+V instead of Ctrl+V. The clipboard itself was working.

## Snapshot
Snapshot "Fresh Kali after installation" taken after the configuration was verified.

## Disclaimer
This lab is for education only. All testing is done inside my own virtual lab.
