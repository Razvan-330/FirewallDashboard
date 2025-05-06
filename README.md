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

 <h3><b><p align="center">
Global Map for blocked connections Panel <br/> 
</b></h3>

<b><p align="center">
SPL Query: <br/> 
</b>
```
index=botsv2 sourcetype="pan:*" (action=blocked)
| table src_ip
| iplocation src_ip
| stats count by Country
| geom geo_countries featureIdField="Country"
```
<b>Connections are filtered by the action “blocked,” and the results are displayed in a table showing only the src_ip (source IP) column. Using the iplocation command, we extract geographical information for each source IP. The results are then filtered by the Country field to obtain the total number of blocked connections per country.

We use the geom command to generate a geographic visualization, combined with geo_countries, a predefined dataset in Splunk that includes country boundary data. The featureIdField="Country" option ensures that the data is correctly mapped to the world map based on country names.

After presenting the data in a statistics table panel, the visualization is switched to a global map.
This process results in a clear, geographical view of the origin of blocked connections.</b>
  
<p align="center"><br/>
<img src="https://i.imgur.com/VFDvcsT.png" height="30%" width="30%" alt="Panel"/>
<br />

<p align="center"><br/>
<img src="https://i.imgur.com/aLAPybB.png" height="80%" width="80%" alt="Panel"/>
<br />  

<h3><b><p align="center">
Source IP Monitoring Panel to identify abnormally high numbers of connections based on destination IP and port. <br/> 
</b></h3>

<b><p align="center">
SPL Query: <br/> 
</b>
```
index=botsv2 sourcetype="pan:*"
| stats count(dest_ip) as numar_destinatii_ip count(dest_port) as numar_destinatii_port by src_ip action
| iplocation src_ip
| search numar_destinatii_ip > 100 AND numar_destinatii_port > 100
| table src_ip numar_destinatii_ip numar_destinatii_port action Country
```
The number of distinct destination IP addresses and destination ports connected to by each source IP is counted and grouped by the source IP and the firewall action. The resulting numerical values are labeled as numar_destinatii_ip (number of destination IPs) and numar_destinatii_port (number of destination ports).

We use the iplocation command to determine the country of origin for all source IPs.
A filter is applied to display only source IPs that have initiated connections to more than 100 distinct destination IPs and more than 100 different ports.

<b>For testing purposes, this threshold is currently set to 0 in the dataset, since the dashboard contains little to no port scanning activity (few blocked connections, mostly from private network IPs).
This filtering mechanism is designed to help detect port scanning attempts, botnet-related activities, or other abnormal network behaviors.
</b>   

The results are displayed in a table showing key columns: source IP, total number of destination IPs and ports, firewall action, and country of origin.

<p align="center"><br/>
<img src="https://i.imgur.com/iN5iA7s.png" height="80%" width="80%" alt="Panel"/>
<br />
  
<h3><b><p align="center">
Final Dashboard Overview <br/> 
</b></h3>
<p align="center"><br/>
<img src="https://i.imgur.com/jQI9izv.png" height="80%" width="80%" alt="Panel"/>
<br />

<h2>Conclusion:</h2>

This dashboard is designed for monitoring firewall activity, providing detailed analysis of allowed and blocked connections. Users can filter data by time and source IP address. The dashboard includes several informative panels:

-Total, Allowed, and Blocked Connections: Displays the total number of connections processed by the firewall, highlighting allowed and blocked actions.

-Blocked Connections – Location: Uses a map to visualize the geographic distribution of blocked connections.

-Suspicious Port Scanning Detection: Identifies potential scanning activity by displaying source IPs, the number of destination IPs contacted, and the country of origin.


   
   <!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
