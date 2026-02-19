<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>DHCP Configuration Guide</title>
</head>
<body>

<h1>DHCP Configuration Guide</h1>

<h2>Overview</h2>
<p>
Dynamic Host Configuration Protocol (DHCP) automatically assigns IP addresses,
Subnet Masks, Default Gateways, and DNS Servers to devices on a network.
This eliminates manual configuration and reduces errors.
</p>

<hr>

<h2>Network Requirements</h2>
<ul>
    <li>Server machine (Linux/Windows Server)</li>
    <li>At least one client machine</li>
    <li>Administrative/root privileges</li>
    <li>Static IP configured on the DHCP Server</li>
    <li>Network connectivity between server and clients</li>
</ul>

<hr>

<h2>Method 1: DHCP Configuration on Linux (Ubuntu Server)</h2>

<h3>Step 1: Update System Packages</h3>
<pre><code>sudo apt update
sudo apt upgrade -y</code></pre>

<h3>Step 2: Install DHCP Server</h3>
<pre><code>sudo apt install isc-dhcp-server -y</code></pre>

<h3>Step 3: Configure Network Interface</h3>
<pre><code>sudo nano /etc/default/isc-dhcp-server</code></pre>

<p>Set your network interface (example: ens33):</p>
<pre><code>INTERFACESv4="ens33"</code></pre>

<h3>Step 4: Configure DHCP Settings</h3>
<pre><code>sudo nano /etc/dhcp/dhcpd.conf</code></pre>

<pre><code>subnet 192.168.1.0 netmask 255.255.255.0 {
  range 192.168.1.100 192.168.1.200;
  option routers 192.168.1.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 8.8.8.8, 8.8.4.4;
  default-lease-time 600;
  max-lease-time 7200;
}</code></pre>

<h3>Step 5: Restart and Enable DHCP Service</h3>
<pre><code>sudo systemctl restart isc-dhcp-server
sudo systemctl enable isc-dhcp-server</code></pre>

<h3>Step 6: Check DHCP Server Status</h3>
<pre><code>sudo systemctl status isc-dhcp-server</code></pre>

<h3>Step 7: Configure Client to Obtain IP Automatically</h3>

<p><strong>Linux Client:</strong></p>
<pre><code>sudo dhclient</code></pre>

<p><strong>Windows Client:</strong></p>
<ul>
    <li>Go to Network Settings</li>
    <li>Select "Obtain an IP address automatically"</li>
</ul>

<h3>Step 8: Verify IP Assignment</h3>

<p><strong>Linux:</strong></p>
<pre><code>ip a</code></pre>

<p><strong>Windows:</strong></p>
<pre><code>ipconfig</code></pre>

<hr>

<h2>Method 2: DHCP Configuration on Windows Server</h2>

<h3>Step 1: Install DHCP Role</h3>
<ol>
    <li>Open Server Manager</li>
    <li>Click "Add Roles and Features"</li>
    <li>Select "DHCP Server"</li>
    <li>Complete installation</li>
</ol>

<h3>Step 2: Configure DHCP Scope</h3>
<ol>
    <li>Open DHCP Manager</li>
    <li>Right-click IPv4 → New Scope</li>
    <li>Configure IP Range, Subnet Mask, Gateway, DNS</li>
    <li>Activate the scope</li>
</ol>

<h3>Step 3: Authorize DHCP Server</h3>
<p>Right-click the server name and select Authorize.</p>

<h3>Step 4: Verify Client Assignment</h3>
<pre><code>ipconfig /release
ipconfig /renew</code></pre>

<hr>

<h2>Screenshot of DHCP Configuration</h2>

<p>Below is a screenshot showing the DHCP server configuration and IP assignment:</p>

<img src="topology1.png" alt="DHCP Configuration Screenshot" width="800">

<p><em>Figure 1: DHCP Server Successfully Assigning IP Address</em></p>

<hr>

<h2>Troubleshooting</h2>

<table border="1">
    <tr>
        <th>Issue</th>
        <th>Solution</th>
    </tr>
    <tr>
        <td>DHCP service not starting</td>
        <td>Check configuration file for syntax errors</td>
    </tr>
    <tr>
        <td>Client not getting IP</td>
        <td>Ensure server and client are in same network</td>
    </tr>
    <tr>
        <td>Firewall blocking DHCP</td>
        <td>Allow UDP ports 67 and 68</td>
    </tr>
    <tr>
        <td>Wrong interface selected</td>
        <td>Verify correct network interface name</td>
    </tr>
</table>

<hr>

<h2>DHCP Port Information</h2>
<ul>
    <li>UDP Port 67 → DHCP Server</li>
    <li>UDP Port 68 → DHCP Client</li>
</ul>

<hr>

<h2>Conclusion</h2>
<p>
DHCP simplifies network management by dynamically assigning IP addresses,
reducing configuration errors, and improving scalability.
</p>

</body>
</html>
