# Firewall Configuration Report

## Firewall Configuration

### Enabling Firewall on Startup

To ensure the firewall rules persist after a reboot, we use `ufw` (Uncomplicated Firewall), which provides a simplified interface for managing firewall rules.

```sh
sudo ufw enable # Activate the firewall
```

### Allowing Essential Services

The following rules enable incoming connections for essential services while blocking all other unauthorized access.

#### OpenSSH (Port 22)

```sh
sudo ufw allow 22/tcp
```

**Reason:** This rule allows secure SSH access while preventing unauthorized access attempts.

#### HTTP and HTTPS (Ports 80 and 443)

```sh
sudo ufw allow 80/tcp  # Allow HTTP
sudo ufw allow 443/tcp  # Allow HTTPS
```

**Reason:** These rules allow access to web services via HTTP and HTTPS while maintaining security.

### Logging Rules

To track blocked and allowed connections, logging is enabled.

#### Enable Logging

```sh
sudo ufw logging on
```

**Reason:** This enables logging off all firewall activity for auditing and security monitoring.

### SYN Flood Attack Mitigation

```sh
sudo ufw limit proto tcp from any to any port 22
```
**Reason:** Limits the rate of new TCP connections to prevent SYN flood attacks.

![Screenshot 2025-02-24 154124](https://github.com/user-attachments/assets/5f1d4961-ee20-4c16-8918-f02755ac0215)

### Additional Protection

```sh
sudo ufw limit ssh  #Rate-limit SSH connections (6 attempts every 30s)
sudo ufw allow in on lo   # Allow all traffic on the loopback interface
sudo ufw logging high  # Log all allowed/blocked traffic at high verbosity
sudo ufw deny 137/udp # NetBIOS
sudo ufw deny 138/udp  # NetBIOS
sudo ufw deny 445/udp  # SMB

```

**Reason:** Limits ICMP requests to mitigate ping flood attacks.

### Default Drop Rule

```sh
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

**Reason:** Blocks all incoming traffic unless explicitly allowed, following a least-privilege security model.

### UFW rules persist automatically after reboot on most systems. 
### To ensure persistence:
```sh
sudo systemctl enable ufw   #Enable UFW service on boot (if not already enabled)
sudo ufw status verbose   # Verify rules
```

![Screenshot 2025-02-24 154158](https://github.com/user-attachments/assets/639c2f7d-3d2b-47e5-9ab1-83a3f3a5e82c)

