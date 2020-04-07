
# NetBooter/NetCommander Support

[toc]

## Troubleshooting
Below are a few guides and walkthroughs to help you get connected with your devices.


### Basic Troubleshooting Tasks and Startup Issues

Common issues related to initial connection typically relate to varying network setups.

If your network uses static IP for its clients, make sure no other client is using 192.168.1.100 on your local network to access any factory default PDU unit through that IP address.

If your network set up uses DHCP to assign IP addresses, then you can [restart the device in DHCP mode](#How-do-I-boot-the-PDU-in-DHCP-default-mode). Once you restart the device in DHCP mode, check your router for the connection made for your device's IP address.
Once you have a working IP address, you can connect to your device with [telnet](#Using-Telnet-to-access-your-device) or [web page](#Accessing-Your-device-from-the-Web-Interface) to further tweak any settings to your needs.

If none of the following works to access your device, then [accessing the PDU via serial port is a reliable way connect](#Accessing-your-device-with-serial-port). If accessing the PDU through the network is problematic, then serial port is a consistent alternative.
	

### Configuring Device IP Address

Configuring your devices IP address will allow you to access it's features from within your local network. The default factory settings for every device is set to:
- Static IP - 192.168.1.100
- Mask - 255.255.0.0
- Gateway - 192.168.1.1

To access the PDU in it's default mode, make sure static IP's are allowed in your network setup. Also make sure that no other clients are using that IP address (192.168.1.100). 

If you would like your router to specify the IP address, you can [restart the device in DHCP mode](#How-do-I-boot-the-PDU-in-DHCP-default-mode).

Once the device boots in DHCP, you can check your router for connected clients, and identify the IP address assigned. The default name for the device is REMOTEPDU.
	
	

### Using Telnet to access your device

**Telnet** is a network protocol that allows a user on one computer to log into another computer *(a power distribution unit in our case)*.

To use Telnet, follow the procedure below.

1. [Find IP Address of the PDU](#Configuring-Device-IP-Address). The factory default is set to a static 192.168.1.100. 
2. Make sure you have the Telnet Application on your operating system.
    - For Windows Users, we recommend using PuTTY. [You can learn about and download PuTTY here](https://www.ssh.com/ssh/putty/).
    - For Ubuntu Linux (14.04+) users, Telnet should be included with your OS.
3. With a new terminal window (from PuTTY or Linux CLI). Use the IP address of your device as an argument to run Telnet. The default PORT for Telnet is 23. If you have previously changed the telnet port, add an additional argument of port to the IP address. The syntax for the additional argument will depend on which Telnet application you are using. 
4. If you see a blank cursor and the following Welcome Message, then the connection is fine.
<!-- TODO screenshot -->
5. Once you are inside the telnet session. You can type `?` to see all available options
6. To exit, type `logout` and you will exit your current telnet session. 

	

### Accessing Your device from the Web Interface

To access your device from a web page, you must first [identify the IP address](#Configuring-Device-IP-Address).

With your device's IP address, go to the web browser and type it into the address bar. If you have specified a different port than the default 80 then append a ":your_port" to the IP address in the address bar.
<!-- TODO screenshot -->


You will  be prompted for authorization credentials. The default username and password are admin and admin. Once you have logged in, you will have access all the web interface related features.
	

	

### Programming Interface and Scripting

There is a programmable HTTP API you can utilize for any scripting needs. You can access them through any programming language that can receive and issue HTTP requests. Visit our [GitHub](https://github.com/synaccess-networks) for code examples. Additionally, you can utilize telnet inside scripts to create programmable functions for the device.

<!-- ![api_screenshot](https://api.synaccess-net.com/api_screenshot.png) -->

	

### Accessing your device with serial port

To  access your device through serial port you will need an RS232 connector (usb cable for newer models).
The other end of the RS232 connector (USB or RS232) will depend on the computer you will connect the device to.

** Accessing with Windows **

If are using a Windows Computer to access the serial port. We recommend using [PuTTY](https://www.ssh.com/ssh/putty/).


** Accessing with Ubuntu Linux **

If you are using a Linux Ubuntu Machine, we use Minicom or Screen to access the device.

To install minicom

`$ sudo apt-get install minicom`

Be sure to use the following settings in the application you are using to access the serial port:

- Baud Rate: 9600
- Data Bits: 8
- Parity - None
- Stop Bits - 1
- Hardware Handshaking - None

	

### Using Simple Network Management Protocol (SNMP) with PDU's

We recommend using a MIB Browser to access information and commands. You can find our MIB files [here](https://synaccess-net.com/support).


## Frequently Asked Questions


### My device is connected to the network but I cannot access my device's webpage
If you can't access the webserver of your PDU but it's connected to the network then sometimes restarting the PDU's network interface card will fix that.

Try using the command `nwset` inside a telnet session to restore inactive networking applications. 

Using nwset will not reset your outlets.

	

### How do I boot the PDU in DHCP default mode?
Hold reset button when you supply power to the device. Depending on your router, you can check the active connections and identify the device with the default name: REMOTEPDU. 
	To access your router's clients and IP addresses see your router's instruction manual.
	

### Will resetting the PDU power cycle the outlets?
No it will not, your device will maintain their power state through a reset.
	

### What are the default network settings?

- Static IP - 192.168.1.100
- Mask - 255.255.0.0
- Gateway - 192.168.1.1    

	

### What is the default Username and Password?
`admin` `admin`
	

### Why am I getting a 414 error when accessing the PDU webpage from a domain name?
Depending on the model, the longest allowable domain name is no longer then 15 characters. Be sure to clear all cookies and cache. If a previous left cookie exceeds a certain length, a 414 error can happen. To fix this, clear all cookies and cache on your browser/http client.
	

 
 ### My Windows PC is not recognizing my USB-Serial Connection
 Most versions of Windows (7,8,109)
 https://www.microchip.com/wwwproducts/en/MCP2200
 under documents
 http://ww1.microchip.com/downloads/en/DeviceDoc/MCP2221%20Windows%20Driver%202014-10-09.zip
 
 
 ###### tags: `documentation` `faq`
