
# <p align="center">Windows-System-Admin-Power-shell-Toolkit

<img align="right" src="https://visitor-badge.laobi.icu/badge?page_id=noetovar5.Windows-System-Admin-Power-shell-Toolkit"/>
<p align="center"> Windows-System-Admin-Power-shell-Toolkit

<p align="center">
  <a href="#">
    <img src="https://skillicons.dev/icons?i=windows,powershell" />
  </a>
</p>
Windows Server PowerShell Toolkit for Administrators
Author: Your Name
Date: 2025-11-04
<p align="left"> 
Check System Uptime
Purpose: Quickly see how long the server has been running.
Script:
(Get-Date) - (gcim Win32_OperatingSystem).LastBootUpTime
Best Practices:
Use this to confirm if recent reboots occurred after patching or troubleshooting.
</p>
  Scan for Open Ports
Purpose: Identify listening ports on the server.
Script:
Get-NetTCPConnection | Where-Object { $_.State -eq 'Listen' } | Select-Object LocalAddress, LocalPort
Best Practices:
Combine with Export-Csv to save results for documentation or audits.
Check Disk Space
Purpose: Monitor free space on all drives.
Script:
Get-PSDrive -PSProvider 'FileSystem' | Select-Object Name, @{Name='Free(GB)';Expression={[math]::Round($_.Free/1GB,2)}}, @{Name='Used(GB)';Expression={[math]::Round(($_.Used)/1GB,2)}}
Best Practices:
Automate alerts if free space drops below a threshold (e.g., 10 GB).
Find High CPU Processes
Purpose: Identify processes consuming the most CPU.
Script:
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10 Name, CPU
Best Practices:
Use this before restarting services or terminating processes.
Check Event Logs for Errors
Purpose: Pull recent critical errors from System log.
Script:
Get-EventLog -LogName System -EntryType Error -Newest 20 | Format-Table TimeGenerated, Source, EventID, Message -AutoSize
Best Practices:
Change 'Error' to 'Warning' or 'Information' as needed for broader analysis.
Network Adapter Status
Purpose: List all network adapters and their status.
Script:
Get-NetAdapter | Select Name, Status, MacAddress, LinkSpeed
Best Practices:
Use to verify if network interfaces are up and running.
Restart a Windows Service
Purpose: Restart a specific service by name.
Script:
Restart-Service -Name 'Spooler' -Force
Best Practices:
Useful for resolving stuck services like Print Spooler or IIS.
List Pending Windows Updates
Purpose: Check for updates that are pending installation.
Script:
Get-WindowsUpdate -MicrosoftUpdate -AcceptAll -IgnoreReboot
Best Practices:
Requires PSWindowsUpdate module. Schedule regular checks.
Check Last Patch Install Date
Purpose: Find the last installed update.
Script:
Get-HotFix | Sort-Object InstalledOn -Descending | Select-Object -First 1
Best Practices:
Use to verify patch compliance and recent updates.
List All Scheduled Tasks
Purpose: Display all scheduled tasks on the server.
Script:
Get-ScheduledTask | Select TaskName, State, LastRunTime, NextRunTime
Best Practices:
Monitor for failed or disabled tasks that impact operations.
Check DNS Resolution
Purpose: Test DNS resolution for a domain.
Script:
Resolve-DnsName google.com
Best Practices:
Use to verify DNS functionality and troubleshoot name resolution issues.
Test Network Latency
Purpose: Ping a remote host and measure response time.
Script:
Test-Connection -ComputerName 8.8.8.8 -Count 4
Best Practices:
Use to check connectivity and latency to external or internal hosts.
Export Installed Programs
Purpose: Export a list of installed programs.
Script:
Get-WmiObject -Class Win32_Product | Select Name, Version | Export-Csv C:\InstalledApps.csv -NoTypeInformation
Best Practices:
Useful for audits and application inventory.
Monitor CPU Usage Over Time
Purpose: Log CPU usage every 10 seconds.
Script:
$log = 'C:\cpu_log.txt'; while ($true) { (Get-Date).ToString() + ' - ' + (Get-Counter '\Processor(_Total)\% Processor Time').CounterSamples.CookedValue >> $log; Start-Sleep -Seconds 10 }
Best Practices:
Use for performance monitoring and historical analysis.
Check Active User Sessions
Purpose: List users currently logged into the server.
Script:
quser
Best Practices:
Use to identify active sessions and troubleshoot access issues.
