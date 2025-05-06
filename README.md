<h1>Firewall Dashboard- Splunk</h1>

<h2>Description</h2>

This dashboard is designed for monitoring firewall activity, providing a detailed analysis of allowed and blocked connections. Users can filter data by time and source IP address. The dashboard features several informative panels:

-<b>Total, Allowed, and Blocked Connections:</b> Displays the total number of connections processed by the firewall, highlighting those categorized as allowed or blocked.

-<b>Blocked Connections – Location:</b> Uses a map to visualize the geographical distribution of blocked connections.

-<b>Suspicious Port Scanning Activity:</b> Detects potential scanning behavior by displaying source IPs, the number of contacted destinations, and the originating country.


<h2>Languages and Utilities Used</h2>

- <b>SPL-Search Processing Language</b>
- <b>BotsV2-[Dataset](https://github.com/splunk/botsv2)</b>
  
<h2>Environments Used </h2>

- <b>SIEM-Splunk</b>
- <b>OS-Kali Linux</b>

<h2>Dashboard Setup</h2>

<h3><b><p align="center">
All Connections Panel <br/> 
</b></h3>

<b><p align="center">
SPL Query: <br/> 
</b>
```
index=botsv2 sourcetype="pan:*" action=blocked OR action=allowed
| stats count by action
| stats sum(count)
```
<b>It retrieves all blocked and allowed actions, counts them, and then aggregates the values to provide a total numerical result.</b>

<br /><p align="center">
Total Connections <br/>
<img src="https://i.imgur.com/SbzuJIg.png" height="80%" width="80%" alt="Panel"/>
<br />

<h3><b><p align="center">
Blocked/Allowed Connections Panel <br/> 
</b></h3>

<b><p align="center">
SPL Query: <br/> 
</b>
```
index=botsv2 sourcetype="pan:*"
| stats count by action
| search action=blocked  <--#switch with allowed and repeat procees
| table count
```
<b>The process is similar to the first panel, but the SPL queries differ, as they do not compute a combined sum of allowed and blocked actions, but rather handle them separately.</b>

<br /><p align="center">
Blocked Connections <br/>
<img src="https://i.imgur.com/kIzbCD0.png" height="80%" width="80%" alt="Panel"/>
<br />

<br /><p align="center">
Allowed Connections <br/>
<img src="https://i.imgur.com/8h274ti.png" height="80%" width="80%" alt="Panel"/>
<br />

<h3><b><p align="center">
All Pannels combined <br/> 
</b></h3>
<br />
 <p align="center"><br/>
<img src="https://i.imgur.com/i4HGW9Y.png" height="80%" width="80%" alt="Panel"/>
<br />
<b>This allows for better organization and provides a clearer overall view of the connections.</b>
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
