---
layout: post
title:  "Multi-Cloud Monitoring"
date:   2021-06-21 05:58:00 +1100
categories: cysa 
permalink: /:categories/:title
published: false
---

## Steps

1. Create a log analytics workspace
2. Create an AWS test VM
3. Log into the AWS test VM
4. In Azure, navigate to Log Analytics > Settings > Agents Management > Linux Servers > Copy the script for "Download and onboard agent for Linux"
5. Return to the AWS test VM. Copy and paste the command into the terminal.
6. Confirm the "omsagent" service is running "systemctl --type=service"
7. Copy the name of the service "systemctl status omsagent-8f548a84-51cb-4c7d-95d6-29cf0689fef2.service" to check if the service is running
8. Set the service to start automatically at boot. "sudo systemctl enable omsagent-8f548a84-51cb-4c7d-95d6-29cf0689fef2.service"
9. Check the linux agent is running from the log analytics workspace. 
    - Logs > Table > Heartbeat
    - Agents Management > Linux Servers
    /opt/microsoft/omsagent/
10. Conf files are located in:
 sudo cat /etc/opt/microsoft/omsagent/conf/omsagent.conf
 
## Links


Configure syslog:
https://docs.microsoft.com/en-us/azure/azure-monitor/agents/data-sources-syslog

Troubleshooting Guide:
https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/oms-linux?toc=%2Fazure%2Fazure-monitor%2Ftoc.json