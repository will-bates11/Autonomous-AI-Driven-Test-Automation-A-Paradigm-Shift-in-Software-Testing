# Network-Security-Guide

Home Network Security Guide
Overview
This repository contains a comprehensive guide and necessary scripts for securing a home network. It's tailored for technically adept users and involves advanced, open-source tools and methodologies. 
The guide covers everything from router configuration, network segmentation, to intrusion detection and prevention.

Table of Contents
Router Configuration
1.1 Firmware Upgrade
1.2 Strong Passwords
1.3 Disable WPS
Network Segmentation
2.1 Separate IoT Devices
2.2 Firewall Configuration
Intrusion Detection and Prevention
3.1 Snort Installation
3.2 Rule Configuration
Secure DNS
4.1 DNS over HTTPS (DoH)
Regular Audits and Monitoring
5.1 Network Scanning
5.2 Log Analysis
VPN for Secure Remote Access
6.1 OpenVPN Setup
Regular Updates
7.1 Automated Update Scripts
Conclusion

Router Configuration
Firmware Upgrade
Replace the default firmware with OpenWrt for enhanced features and security.

Strong Passwords
Use the provided Python script to generate strong passwords for your network devices.

# Python Script for Password Generation
import secrets
import string

def generate_password(length=16):
    alphabet = string.ascii_letters + string.digits + string.punctuation
    password = ''.join(secrets.choice(alphabet) for i in range(length))
    return password

print(generate_password())

Disable WPS
For enhanced security, disable Wi-Fi Protected Setup (WPS) in your router settings.

Network Segmentation
Separate IoT Devices
Create a separate VLAN for IoT devices using OpenWrt.

# OpenWrt VLAN Configuration Script
uci set network.iot=interface
uci set network.iot.ifname='eth0.3'
uci set network.iot.type='bridge'
uci set network.iot.proto='static'
uci set network.iot.ipaddr='192.168.3.1'
uci set network.iot.netmask='255.255.255.0'
uci commit network
/etc/init.d/network restart
Firewall Configuration
Isolate network segments with firewall rules.

# OpenWrt Firewall Rule to Isolate VLANs
uci add firewall rule
uci set firewall.@rule[-1].src='iot'
uci set firewall.@rule[-1].dest='lan'
uci set firewall.@rule[-1].target='REJECT'
uci commit firewall
/etc/init.d/firewall restart

Intrusion Detection and Prevention
Snort Installation
Install Snort, an open-source intrusion detection system.

Rule Configuration
Configure Snort to monitor and protect your network.

# Snort Rule Example
alert tcp any any -> $HOME_NET 23 (msg:"TELNET login attempt"; sid:1000001;)
Secure DNS
DNS over HTTPS (DoH)
Encrypt DNS queries using dnscrypt-proxy.

# dnscrypt-proxy Configuration Snippet
server_names = ['cloudflare', 'google']
listen_addresses = ['127.0.0.1:53']
max_clients = 250

Regular Audits and Monitoring
Network Scanning
Use Nmap to periodically scan your network for unauthorized devices.

Log Analysis
Analyze network logs with goaccess.

# GoAccess Command for Log Analysis
zcat /var/log/syslog.*.gz | goaccess --log-format=COMBINED

VPN for Secure Remote Access
OpenVPN Setup
Configure OpenVPN for secure remote access to your home network.

Regular Updates
Automated Update Scripts
Ensure your network devices are regularly updated with the latest security patches.

# Cron Job for Automated Updates
0 4 * * * apt-get update && apt-get upgrade -y

Conclusion
This guide provides a robust framework for securing a home network. Regular maintenance and updates are crucial. The configurations and scripts should be adapted to your specific network configuration.
