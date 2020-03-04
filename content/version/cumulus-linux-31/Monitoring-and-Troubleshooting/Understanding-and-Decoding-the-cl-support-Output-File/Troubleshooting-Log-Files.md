---
title: Troubleshooting Log Files
author: Cumulus Networks
weight: 317
aliases:
 - /display/CL31/Troubleshooting+Log+Files
 - /pages/viewpage.action?pageId=5121958
pageID: 5121958
---
The only real unique entity for logging on Cumulus Linux compared to any
other Linux distribution is `switchd.log`, which logs the HAL (hardware
abstraction layer) from hardware like the Broadcom or Mellanox ASIC.

[This guide on
NixCraft](http://www.cyberciti.biz/faq/linux-log-files-location-and-how-do-i-view-logs-files/)
is amazing for understanding how `/var/log` works. The green highlighted
rows below are the most important logs and usually looked at first when
debugging.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Log</p></th>
<th><p>Description</p></th>
<th><p>Why is this important?</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>/var/log/alternatives.log</p></td>
<td><p>Information from the update-alternatives are logged into this log file.</p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>/var/log/apt</p></td>
<td><p>Information the <code>apt</code> utility can send logs here; for example, from <code>apt-get install</code> and <code>apt-get remove</code>.</p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>/var/log/audit/*</p></td>
<td><p>Contains log information stored by the Linux audit daemon, <code>auditd</code>.</p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>/var/log/auth.log</p></td>
<td><p>Authentication logs.</p>
<p>Note that Cumulus Linux does not write to this log file; but because it's a standard file, Cumulus Linux creates it as a zero length file.</p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>/var/log/autoprovision</p></td>
<td><p>Logs output generated by running the <a href="/cumulus-linux-31/Installation-Upgrading-and-Package-Management/Zero-Touch-Provisioning-ZTP">zero touch provisioning</a> script.</p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>/var/log/boot.log</p></td>
<td><p>Contains information that is logged when the system boots.</p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>/var/log/btmp</p></td>
<td><p>This file contains information about failed login attempts. Use the <code>last</code> command to view the <code>btmp</code> file. For example:</p>
<pre><code>cumulus@switch:~$ last -f /var/log/btmp | more</code></pre></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>/var/log/clagd.log</p></td>
<td><p>Logs status of the <a href="/cumulus-linux-31/Layer-1-and-Layer-2-Features/Multi-Chassis-Link-Aggregation-MLAG"><code>clagd</code> service</a>.</p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>/var/log/cron.log</p></td>
<td><p>Log file for cron jobs.</p>
<p>Note that Cumulus Linux does not write to this log file; but because it's a standard file, Cumulus Linux creates it as a zero length file.</p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>/var/log/daemon.log</p></td>
<td><p>Contains information logged by the various background daemons that run on the system.</p>
<p>Note that Cumulus Linux does not write to this log file; but because it's a standard file, Cumulus Linux creates it as a zero length file.</p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>/var/log/debug</p></td>
<td><p>Debugging information.</p>
<p>Note that Cumulus Linux does not write to this log file; but because it's a standard file, Cumulus Linux creates it as a zero length file.</p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>/var/log/dmesg</p></td>
<td><p>Contains kernel ring buffer information. When the system boots up, it prints number of messages on the screen that display information about the hardware devices that the kernel detects during boot process. These messages are available in the kernel ring buffer and whenever a new message arrives, the old message gets overwritten. You can also view the content of this file using the <code>dmesg</code> command.</p></td>
<td><p><code>dmesg</code> is one of the few places to determine hardware errors.</p></td>
</tr>
<tr class="odd">
<td><p>/var/log/dpkg.log</p></td>
<td><p>Contains information that is logged when a package is installed or removed using the <code>dpkg</code> command.</p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>/var/log/faillog</p></td>
<td><p>Contains failed user login attempts. Use the <code>faillog</code> command to display the contents of this file.</p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>/var/log/fsck/*</p></td>
<td><p>The <code>fsck</code> utility is used to check and optionally repair one or more Linux filesystems.</p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>/var/log/kern.log</p></td>
<td><p>Logs produced by the kernel and handled by <code>syslog</code>.</p>
<p>Note that Cumulus Linux does not write to this log file; but because it's a standard file, Cumulus Linux creates it as a zero length file.</p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>/var/log/lastlog</p></td>
<td><p>Formats and prints the contents of the last login log file.</p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>/var/log/lpr.log</p></td>
<td><p>Printer logs.</p>
<p>Note that Cumulus Linux does not write to this log file; but because it's a standard file, Cumulus Linux creates it as a zero length file.</p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>/var/log/mail.log</p></td>
<td><p>Mail server logs. Also includes <code>mail.err</code>, <code>mail.info</code> and <code>mail.warn</code>.</p>
<p>Note that Cumulus Linux does not write to this log file; but because it's a standard file, Cumulus Linux creates it as a zero length file.</p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>/var/log/messages</p></td>
<td><p>General messages and system-related information.</p>
<p>Note that Cumulus Linux does not write to this log file; but because it's a standard file, Cumulus Linux creates it as a zero length file.</p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>/var/log/news/*</p></td>
<td><p>The <code>news</code> command keeps you informed of news concerning the system.</p>
<p>Note that Cumulus Linux does not write to this log file; but because it's a standard file, Cumulus Linux creates it as a zero length file.</p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>/var/log/ntpstats</p></td>
<td><p>Logs for network configuration protocol.</p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>/var/log/openvswitch/*</p></td>
<td><p><code>ovsdb-server</code> logs.</p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>/var/log/quagga/*</p></td>
<td><p>Where Quagga logs to once enabled.</p></td>
<td><p>This is how Cumulus Networks troubleshoots routing. For example an md5 or mtu mismatch with OSPF.</p></td>
</tr>
<tr class="odd">
<td><p>/var/log/switchd.log</p></td>
<td><p>The HAL log for Cumulus Linux.</p></td>
<td><p>This is specific to Cumulus Linux. Any <code>switchd</code> crashes are logged here.</p></td>
</tr>
<tr class="even">
<td><p>/var/log/syslog</p></td>
<td><p>The main system log, which logs everything except auth-related messages.</p></td>
<td><p>The primary log; it's easiest to <code>grep</code> this file to see what occurred during a problem.</p></td>
</tr>
<tr class="odd">
<td><p>/var/log/user.log</p></td>
<td><p>Note that Cumulus Linux does not write to this log file; but because it's a standard file, Cumulus Linux creates it as a zero length file.</p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>/var/log/watchdog</p></td>
<td><p><a href="/cumulus-linux-31/Monitoring-and-Troubleshooting/Monitoring-System-Hardware/">Hardware watchdog</a> log files.</p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>/var/log/wtmp</p></td>
<td><p>Login records file.</p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>
