# Wazuh SIEM - Nmap Port Scan Detection

![Wazuh](https://img.shields.io/badge/Wazuh-v4.14.3-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Platform](https://img.shields.io/badge/platform-Linux-lightgrey)
![Author](https://img.shields.io/badge/author-Satheesh-orange)

Real-time Nmap port scan detection using custom Wazuh SIEM rules and iptables logging.

## Author
**Satheesh Nithiananthan**
satheeshnithiananthan@gmail.com

## Lab Setup
| Machine | OS | IP | Role |
|---------|----|----|------|
| Attacker | BlackArch | 192.168.8.185 | Nmap scans |
| Agent | Debian/Ubuntu | 192.168.8.186 | Target |
| Manager | Kali Linux | - | Wazuh Manager |

## How It Works
```
Nmap scan → iptables logs it → journald captures it
→ Wazuh agent forwards it → Custom rules fire at level 15
→ Alert appears in dashboard
```

## Rules
| Rule ID | Level | Description |
|---------|-------|-------------|
| 100099 | 3 | Bridge rule - catches PORTSCAN events |
| 100100 | 15 | TCP SYN scan detected |
| 100101 | 15 | UDP scan detected |
| 100102 | 15 | ICMP scan detected |

## Quick Start
```bash
# Copy rules to your Wazuh manager
cp rules/local_rules.xml /var/ossec/etc/rules/local_rules.xml

# Add iptables rule on your agent
iptables -A INPUT -m state --state NEW -j LOG --log-prefix "PORTSCAN: " --log-level 4

# Restart Wazuh manager
systemctl restart wazuh-manager
```

## Screenshots

### iptables PORTSCAN Rule Active
![iptables](screenshots/01_iptables_rule.png)

### Wazuh Logtest - Rule 100100 Firing at Level 15
![logtest](screenshots/03_logtest_output.png)

### Live Alert Captured in alerts.log
![alert](screenshots/05_live_alert.png)

### Wazuh Dashboard - Security Events
![dashboard](screenshots/06_dashboard.png)

## Full Case Study
Read the complete step-by-step guide on Medium.

