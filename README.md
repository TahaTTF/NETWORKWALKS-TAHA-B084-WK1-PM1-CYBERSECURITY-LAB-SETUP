# NetworkWalks Cybersecurity Internship — Week 1

**Participant:** Taha Chebil  
**Batch:** B084  
**Project:** PM1 — Cybersecurity Lab Setup  
**Platform:** Oracle VirtualBox + Kali Linux  
**Status:** Core Kali lab and network configuration completed; guest integration features require further verification.

## Project overview

This project documents my Week 1 cybersecurity lab setup for the NetworkWalks internship. I prepared a Kali Linux virtual machine in Oracle VirtualBox, configured a dedicated NAT Network and a static network configuration, verified internet access, and created a recovery snapshot. I also participated in the YouTube discussion by sharing the fix for a connection issue I encountered.

## Objectives

- Set up a Kali Linux virtual machine in VirtualBox.
- Configure the virtual machine to use a dedicated **NAT Network** rather than the default NAT mode.
- Assign a **static IPv4 configuration** inside Kali Linux.
- Confirm network connectivity and DNS resolution.
- Save a VirtualBox snapshot of the working lab.
- Document troubleshooting and support other learners.
- Verify shared clipboard, drag-and-drop, and a Windows shared folder when Guest Additions integration is available.

## Tools and environment

| Component | Setup |
| --- | --- |
| Host operating system | Windows |
| Hypervisor | Oracle VirtualBox |
| Guest operating system | Kali Linux 2026.2 (VirtualBox image) |
| Network mode | NAT Network |
| Guest networking | Static IPv4 configuration via NetworkManager |
| Network interface | `eth0` |
| Verification tools | `nmcli`, `ip`, `ping` |
| Recovery | VirtualBox snapshot |

## Setup and configuration

### 1. Prepare VirtualBox and Kali Linux

I imported the Kali Linux VirtualBox image and reviewed the VM configuration before starting the guest operating system.

### 2. Configure the NAT Network

In VirtualBox's **Network Manager**, I created a dedicated NAT Network. I then opened the Kali VM's **Settings → Network**, enabled **Adapter 1**, selected **NAT Network**, and attached it to the network I created. I also confirmed that **Virtual Cable Connected** was enabled.

![VirtualBox NAT Network configuration](screenshots/03-nat-network.png)

![Kali VM network adapter settings](screenshots/04-kali-settings.png)

### 3. Configure static networking in Kali

Inside Kali Linux, I configured **Wired connection 1** to use the required static IPv4 settings and checked that NetworkManager could activate the connection. The exact numerical network settings are intentionally omitted from this public README.

### 4. Troubleshoot network disconnection

**Problem:** After a reboot, the Kali network interface appeared disconnected in NetworkManager. The VM could not resolve website names, and the connection initially failed to activate even though the virtual network cable was connected.

**Fix:** I enabled automatic connection and adjusted the IPv4 duplicate-address detection timeout, then reactivated the NetworkManager profile:

```bash
sudo nmcli connection modify "Wired connection 1" \
  connection.autoconnect yes \
  ipv4.dad-timeout 0

sudo nmcli connection up "Wired connection 1"
```

For the documented reactivation test, I also disconnected and reconnected the profile:

```bash
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
```

**Result:** NetworkManager reported the connection as active, and the connectivity test succeeded.

### 5. Verify connectivity

I checked the interface state, the assigned IPv4 configuration, and external connectivity:

```bash
nmcli device status
ip -4 addr show eth0
ping -c 4 google.com
```

The test returned **four replies with zero packet loss**. DNS resolution also worked. The screenshot below shows the actual terminal output from my lab.

![Kali network activation and connectivity verification](screenshots/06-network-connectivity.png)

### 6. Create a recovery snapshot

After confirming the working network configuration, I shut down Kali and created a VirtualBox snapshot named **`WK1-Kali-Lab-Configured`**. This provides a recovery point before making additional changes to the virtual machine.

![Snapshot configuration](screenshots/snapshot_1.png)

![Snapshot successfully created](screenshots/snapshot_2.png)


## YouTube community participation

I reviewed other students' troubleshooting comments and replied to a student experiencing a similar Kali internet connectivity problem. My reply described the NetworkManager configuration change that worked in my own lab.

![YouTube troubleshooting reply](screenshots/09-youtube-community-support.png)

## Evidence summary

| Evidence | File |
| --- | --- |
| NAT Network configuration | `screenshots/03-nat-network.png` |
| Kali VM adapter settings | `screenshots/04-kali-settings.png` |
| Successful network test | `screenshots/06-network-connectivity.png` |
| Snapshot details | `screenshots/snapshot_1.png` |
| Snapshot created | `screenshots/snapshot_2.png` |
| YouTube community reply | `screenshots/09-youtube-community-support.png` |

## Lessons learned

- **NAT** and **NAT Network** are different VirtualBox adapter modes; the correct mode matters for the assignment.
- A virtual network cable showing as connected does not necessarily mean NetworkManager has activated the guest connection.
- Testing both interface state and domain-name connectivity helps distinguish connection problems from DNS problems.
- Keeping a snapshot after a successful setup makes later experimentation safer.
- Sharing a tested solution can help other students facing the same issue.

## Next steps

- Resolve and verify VirtualBox Guest Additions integration.
- Test bidirectional clipboard and drag-and-drop.
- Verify access to the Windows Downloads shared folder from Kali.
- Add any further verified evidence to this repository.

---

**NetworkWalks Cybersecurity Internship | Batch B084 | Week 1**  
*Documented by Taha Chebil.*
