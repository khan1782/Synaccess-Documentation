# SynLink CLI for Serial, SSH, and Telnet
###### tags: `documentation` `ssh` `telnet` `serial`
<!-- -----
- [Overview](#Overview)
- [Accessing CLI](#Accessing-CLI)
  - [Accessing CLI via SSH](#Accessing-CLI-via-SSH)
  - [Accessing CLI via Telnet](#Accessing-CLI-via-Telnet)
  - [Accessing CLI via USB-Serial](#Accessing-CLI-via-USB-Serial)
- [SynLink Commands](#List-Of-Commands)
  - [Network](#network)
  - [System](#system)
  - [Logs](#logs)
  - [Settings](#settings)
- [All Commands](#All-Commands)
- [Reference](#Reference)
  - [Valid Entries](#Valid-Entries)
  - [Event Actions](#Event-Actions)
  - [All Configuration Values](#All-Configuration-Values) -->
  [TOC]

# Overview
SynLink Command Line Interface is a text based terminal shell which shows information and controls PDU functions.
Typing in commands from the list of commands, will provide further details and additional commands which can be appended to previous commands separated by spaces.

## Accessing CLI
Access to SynLink Command Line Interface can be achieved with with SSH (Secure Shell), Telnet, or USB-Serial local connection.

###  Accessing CLI via SSH
Execute the `ssh` command with username and IP address separated by a @. A prompt for admin password will appear. Once properly authenticated, SynLink Shell Help Menu will appear.

**Example Ubuntu/Debian OS** 
```shell
$ ssh admin@192.168.1.100
The authenticity of host '192.168.1.100 (192.168.1.100)' can't be established.
ECDSA key fingerprint is SHA256:VZMUnPw2MrIWR9y9pBbKlEzuWZRxJAo9bLMMUdsO/Jw.
Are you sure you want to continue connecting (yes/no)? 
admin@192.168.1.100's password: 
-----------------------
SynLink Shell Help Menu
-----------------------

Usage: [command] [options...]

Commands 
  status       print status reports for all Syn processess
  network      see options for networking configurations and status
  system       see options for PDU power, control, sensors, etc
  logs         see options for logs
  settings     see options for managing users settings
  help         returns this message
  [exit]       Exits SynLink Shell

SynLink> 

```
**Example Windows 10 OS** 

**PuTTY** is a useful tool which has an SSH Client. You can download PuTTY on their organization's website

[https://www.putty.org/](https://www.putty.org/)

Once downloaded open the application. The following screen will likely appear.

![Putty-Screen](https://i.imgur.com/s9fRrTu.png)

Under `Host Name (or IP address)` you can add your username and IP address.
Example: `admin@192.168.1.100`

![Putty-Screen-prompt](https://i.imgur.com/HpZ5B5C.png)


### Accessing CLI via Telnet
Follows  similar steps to SSH for Ubuntu/Debian Linux OS and Windows 10 OS

**Ubuntu/Debian Linux OS**
```shell
telnet admin@192.168.1.100
```
**Windows 10 OS**
Same steps as SSH, on PuTTY select telnet instead. 

### Accessing CLI via USB-Serial
TODO

# List of Commands

## `network`
`network` command returns a list of commands to access networking related subcategories. To see a general overview of networking configurations and status use command `status`. Options to change configurations can be found within each subcategory.

```shell
SynLink> network
---------------------------
SynLink PDU Networking Menu
---------------------------

Usage: network [command] [options...]

Commands:    
   status    Print all network status and configurations
   restart   Restart all networking related activity.    
   ip        Print options to show and configure PDU IP
   web       Print options to show and configure HTTP(S) Webserver
   ssh       Print options to show and configure ssh info
   telnet    Print options to show and configure telnet info
   snmp      Print options to show and configure SNMP info
   syslog    Print options to show and configure syslog info
   smtp      Print options to show and configure smtp info
   ntp       Print options to show and configure NTP info
```

### `network status`
`network status` will print in human readable format networking information and configurations.
```shell
SynLink> network status
----------------
Network Settings
----------------

Process        Control  Status   Port Proto    
-------------------------------------------
Webserver      Enabled  Running  80   TCP
SSH Server     Enabled  Running  22   TCP
Telnet Server  Enabled  Running  23   TCP 
SNMP Agent     Enabled  Running  161  UDP
NTP Sync       Enabled  Running  123  UDP
Syslog Server  Enabled  Running  514  UDP

IP Settings
-----------
IP Address Assignment:  DHCP
DHCP Assigned IP:       192.168.1.126       // only shows with ipAssign DHCP
Static Assigned IP:     192.168.1.100
Mac Address:            80:1F:12:41:ED:28
Gateway IP:             192.168.1.1
Source IP:              0.0.0.0
Source Subnet:          255.255.255.0
Primary DNS IP:         8.8.8.8
Secondary DNS IP:       8.8.4.4

Web Settings
------------
Availability:           Enabled
Status:                 Running
Port:                   80
SSL Security:           Enabled
Max Users:              0 users   (No Max Limit)
Web Idle Timeout:       0 seconds (No Timeout)

SSH Settings
------------
Availability            Enabled
Status:                 Running
Port:                   22
SSH Idle Timeout:       90 seconds
Auth Key Login:         Enabled
Password Login:         Enabled

Telnet Settings
---------------
Availability:           Enabled
Status:                 Running
Port:                   23

Syslog Settings
---------------
Availability            Enabled
Status:                 Running
Port:                   514
Protocol:               RFC5424
Remote Logging:         Enabled
Remote Log IP:          192.168.1.181

SMTP Settings
-------------
TODO

SNMP Settings
-------------
TODO

AutoPing Reboot Settings
------------------------
TODO

NTP Settings
------------
TODO

```

### `network restart`
`network restart` will restart any networking related processes with most recently changed settings.
```shell
SynLink> network restart
Are you sure you want to restart the network? 
If certain IP settings are changed, you may lose connectivity.
[y/n]
>y
Network settings successfully restarted

```

### `network ip`

`network ip` is a subcategory of `network` section which deals with IP assignment of PDU, and other related configurations necessary to reach PDU Host via Internet Protocol. 
```shell
SynLink> network ip
---------------
Network IP Menu
---------------

Usage: network ip [command] [options...]

Commands:
  status       Show current status and configurations for IP
  set          See configuration options to change IP settings

```

#### `network ip status`
`network ip status` will print in human readable format networking information and configurations related to IP of PDU.

```shell
SynLink> network ip status
-----------
IP Settings
-----------
IP Address Assignment:  DHCP
DHCP Assigned IP:       192.168.1.126       // only shows with ipAssign DHCP
Static Assigned IP:     192.168.1.100
Mac Address:            80:1F:12:41:ED:28
Gateway IP:             192.168.1.1
Source IP:              0.0.0.0
Source Subnet:          255.255.255.0
Primary DNS IP:         8.8.8.8
Secondary DNS IP:       8.8.4.4
```

#### `network ip set`
`network ip set` will show all available configurations to change. Follow 'Usage' example to change values of interest.
```shell
SynLink> network ip set
-------------------------
Network IP Configurations
-------------------------

Usage: network ip set [key] [value]

Key           | Values      | Default Value | Description
---------------------------------------------------------------------------------------------------------------------------
static_ip     | IP_ADDRESS  | 192.168.1.100 | IP Address that unit will respond to if ip_assign is set to 'dhcp'
subnet_mask   | SUBNET_MASK | 255.255.255.0 | Subnet Mask that divides the IP address into network address and host address
gateway_ip    | IP_ADDRESS  | 192.168.1.1   | IP Address for forwarding host (router) to other networks
ip_assign     | static,dhcp | static        | Choosing DHCP will have DHCP Server at 'gateway_ip' assign IP address to PDU
primary_dns   | IP_ADDRESS  | 8.8.8.8       | Default DNS Server from Google
secondary_dns | IP_ADDRESS  | 8.8.4.4       | Default Secondary DNS Server from Google
source_ip     | IP_ADDRESS  | 0.0.0.0       | Allows one specific IP to access PDU, or a group depending on source_subnet setting. 0.0.0.0 allows all.
source_subnet | SUBNET_MASK | 255.255.255.0 | Can be set to allow a range of IP's to access PDU or a single IP. 255.255.255.255 for the exact source_ip match. 255.255.255.0 for all IP address starting with first 3 octets of source_ip
--------------------------------------------------------------------------------------------------------------------------
```
##### `network ip set static_ip`
IP input must follow [Valid IP](#ip-validation) requirement.
Requires running `syn network restart`. May lose connection depending on network settings changed, and connection method.
```shell
SynLink> network ip set static_ip 192.168.1.101
Successfully saved setting. 
Please restart network to change IP address.
```

##### `network ip set subnet_mask`
Subnet Mask input must [Valid Subnet Mask](#subnet-mask-validation) requirement.
Requires running `syn network restart`. May lose connection depending on network settings changed, and connection method.
```shell
SynLink> network ip set subnet_mask 255.255.0.0
Successfully saved setting.
Please restart network to change Subnet Mask.
```
##### `network ip set gateway_ip`
IP input must follow [Valid IP](#ip-validation) requirement.
Requires running `syn network restart`. May lose connection depending on network settings changed, and connection method.
```shell
SynLink> network ip set gateway_ip 192.168.1.2
Successfully saved setting.
Please restart network to change Gateway IP
```

##### `network ip set ip_assign`
IP Assign input must follow be either "`dhcp`" or "`static`".
Requires running `syn network restart`. May lose connection depending on network settings changed, and connection method.

```shell
SynLink> network ip set ip_assign dhcp
Successfully saved setting.
Please restart network to change IP Assignment.
```
##### `network ip set primary_dns`
IP input must follow [Valid IP](#IP-Validation) requirement.
Requires running `syn network restart`. May lose connection depending on network settings changed, and connection method.
```shell
SynLink> network ip set primary_dns 1.1.1.1
Successfully saved setting.
Please restart network to change Primary DNS
```
##### `network ip set secondary_dns`
IP input must follow [Valid IP](#IP-Validation) requirement.
Requires running `syn network restart`. May lose connection depending on network settings changed, and connection method.
```shell
SynLink> network ip set secondary_dns 1.0.0.1
Successfully saved setting.
Please restart network to change Secondary DNS
```
##### `network ip set source_ip`
IP input must follow [Valid IP](#valid-ip) requirement. If source_ip is not 0.0.0.0, Only IP's allowed to access PDU are source_ip with subnet mask of source_subnet.

To allow one specific Source IP:
Set `source_ip` to IP of interest. Ex: 192.168.1.191. 
Set `source_subnet` to 255.255.255.255.

To allow a group of IPs:
Set `source_ip` to IP of interest. Ex: 192.168.1.8. 
Set `source_subnet` to 255.255.255.248. 
Will allow 192.168.1.8 to 192.168.1.15 IP range.

```shell
SynLink> network ip set source_ip 192.168.1.191
Successfully saved setting.
Please restart network to change Source IP filtering.
```

##### `network ip set source_subnet`
Subnet Mask input must follow [Valid Subnet](#valid_subnet) requirement. 
```shell
SynLink> network ip set source_ip 255.255.255.255
Successfully saved setting.
Please restart network to change Source IP filtering.
```

### `network web`
`network web` is a subcategory of `network` section which deals with the HTTP(S) webserver configurations.
```shell
SynLink> network web
----------------
Network Web Menu
----------------

Usage: network web [command] [options...]

Commands:
  status   Show current status and configurations for Web HTTP(S) Server
  set      See configuration options to change Web HTTP(S) Server settings

```

#### `network web status`
`network web status` will print in human readable format webserver configurations
```shell
SynLink> network web status
------------
Web Settings
------------
Availability:           Enabled
Status:                 Running
Port:                   80
SSL Security:           Enabled
Max Users:              0 users   (No Max Limit)
Web Idle Timeout:       0 seconds (No Timeout)
```

#### `network web set`
`network web set` will show all available configurations to change. Follow 'Usage' section to change values of interest.
```shell
SynLink> network web set
-------------------------
Network Web Configurations
-------------------------

Usage: network web set [key] [value]

Key                 | Values    | Default | Description
----------------------------------------------------------------------------------------------------------------------
web_enabled         | on,off    | on      | Turns on or off HTTP(S) Server
web_ssl_enabled     | on,off    | off     | Enforces SSL certificate for HTTPS
web_port            | PORT_NUM  | 80      | Port to reach Webserver
web_max_users       | NUM_USERS | 0       | Max number concurrent users on web browser. 0 is no limit
web_timeout         | SECONDS   | 120     | Number seconds on web browser before auto log off. 0 is no timeout
webhook_log_enabled | on,off    | on      | Enable or disables logging for web hook alerts without HTTP 200 responses.
----------------------------------------------------------------------------------------------------------------------
```
##### `network web set web_enabled`
Turn on or off Webserver HTTP(S) server. Valid input are either "`on`" or "`off`".
```shell
SynLink> network web set web_enabled off
Webserver process stopped.
```
```shell
SynLink> network web set web_enabled on
Webserver started at port 80.
```

##### `network web set web_ssl_enabled`
Enable SSL (Secure Sockets Layer) security on set port.
```shell
SynLink> network web set web_ssl_enabled on
Successfully saved setting.
HTTPS on port 80 enabled.
Please restart network to use SSL security.
```

##### `network web set web_port`
Port input must follow [Valid Port](#valid-port) requirement.
```shell
SynLink> network web set web_port 8080
Successfully saved setting.
Please restart network to change webserver port.
```

##### `network web set web_max_users`
Set max number of users allowed to connect via web browser at a given moment. 0 will allow unlimited connections. Must be a number between 0 and 20.
```shell
SynLink> network web set web_max_users 0
Successfully saved setting.
Please restart network to set max number of web browser users.
```

##### `network web set web_timeout`
Number of seconds before a user is automatically logged off while connected to PDU on a web browser. Must be between 0 and 3600.  0 for no timeout.
```shell
SynLink> network web set web_timeout 120
Successfully saved setting.
Please restart network to change web timeout.
```

##### `network web set webhook_log_enabled`
Enable whether webhook http requests log into syslog when unsuccessfully sending HTTP Post request to specified IP.
```shell
SynLink> network web set webhook_log_enabled on
Successfully saved setting.
Future Web hook HTTP Request will log when not successful.
```


### `network ssh`
`network ssh` is a subcategory of `network` section which deals with SSH server configurations. To manage ssh keys, use `ssh keys` command.
```shell
SynLink> network ssh
----------------
Network SSH Menu
----------------

Usage: network ssh [command] [options...]

Commands:
  status   Show current status and configurations for SSH Server
  keys     Print options to manage SSH Keys
  set      See configuration options to change SSH Server settings

```

#### `network ssh status`
`network ssh status` will print in human readable format ssh configurations
```shell
SynLink> network ssh status
------------
SSH Settings
------------
Availability            Enabled
Status:                 Running
Port:                   22
SSH Idle Timeout:       90 seconds
Auth Key Login:         Enabled
Password Login:         Enabled
```

#### `network ssh keys`
`network ssh keys` will print a list of commands to manage ssh keys
```shell
SynLink> network ssh keys
---------------------
Network SSH Keys Menu
---------------------

Usage: ssh keys [command] [options..]

Commands:
  list    List all current SSH Keys
  add     Add a SSH Key for password logins. Prompts user for key name and key
  remove  Remove a single SSH Key

```

##### `network ssh keys list`
`network ssh keys list` will print a list of ssh keys with the key name and RSA key footprint.
```shell
SynLink> network ssh keys list
---------------------
Network SSH Keys List
---------------------

Key 1: lab_pc_2 - 97:f0:86:10:02:96:8f:24:e6:df:38:f2:c8:de:4b:69
Key 2: lab_pc_1 - 4a:dc:9a:76:c3:04:30:c3:88:9a:9e:a6:ed:36:ca:b5

```

##### `network ssh keys add`
`network ssh keys add` will prompt for a public ssh key, and a name to remember it by.
RSA SSH Keys are acceptable key types, and are typically found in ~/.ssh/id_rsa.pub for Ubuntu/Debian Linux OS and Mac. Once found, copy the key to your clipboard and paste or copy manually. No uploading options exist on SynLink CLI.
The name you provide for the keys must be unique, as well as the keys themselves.
```shell
SynLink> ssh keys add
Paste RSA SSH Key Here:
> ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCEj9MGWzy5LGDKGM50GgOpncbcuTgp4gJ2XVjbqicbguXljQt+hfiMAb2IIJg7vXtMp6yr/8DYoqI5cwv1pDqxPweMcvOKMgv1HFX28Ng2kzJorEF6f6Xw6K6iXaKcXH3xRm2ZAfiv4qM6UT/jxgA2OmHhP0HptH4Kp2ScwlccUs7CP2srxf+hAQ7Mh2HP0mtjgLrQvqGtYw+hSr9XuygTKzOSuTVA0GIj+udF1mlIln+uWXKnupocdJvuvpfp38FefMATmR4h5M4ydDF+ngYazgv1qm7U37IXoLbBvnzoCtZbnLkina6EdiL+jGZvSPNpcbHaW7lAoeH8ujCStJUx 
Please provide a name for your key:
> lab_pc_3
Key Successfully Added
```

##### `network ssh keys remove`
`network ssh keys remove` will list ssh keys by names provided and prompt user for the name of ssh key to remove.
User input but come from the list provided.
TODO spaces allowed???
```shell
SynLink> ssh keys remove
Which of the following keys would you like to delete forever?
lab_pc_1
lab_pc_2
lab_pc_3
> lab_pc_1

```


#### `network ssh set`
`network ssh set` will show all available configurations to change. Follow ‘Usage’ section to change values of interest.
```shell
SynLink> network ssh set
-------------------------
Network SSH Configurations
-------------------------

Usage: network ssh set [key] [value]

Key              | Values   | Default | Description
-----------------------------------------------------------------------------------
ssh_enabled      | on,off   | on      | Turns on or off SSH Server
ssh_port         | PORT_NUM | 22      | Port to reach SSH Server
ssh_idle_timeout | SECONDS  | 30      | Number seconds before SSH Server auto exits
ssh_disable_keys | on,off   | off     | Disable key based Login
ssh_disable_pass | on,off   | off     | Disable password logins
-----------------------------------------------------------------------------------
```
##### `network ssh set ssh_enabled`
Turn on or off SSH Server. Valid inputs are either `on` or `off`
```shell
SynLink> network ssh set ssh_enabled on
SSH Server stopped.
```

##### `network ssh set ssh_port`
Port input must follow [Valid Port](#valid-port) requirement.
```shell
SynLink> network ssh set ssh_port 1022
Successfully saved setting.
Please restart network to change ssh port.
```

##### `network ssh set ssh_idle_timeout`
Number of seconds before a user is automatically logged off while connected to PDU on a SSH session. 0 for no timeout. Must be a number between 0 and 3600. 
```shell
SynLink> network ssh set ssh_idle_timeout 0
Successfully saved setting.
Please restart network to change ssh timeout.
```

##### `network ssh set ssh_disable_pass`
If enabled, password authentication is disabled for ssh connection requests.
```shell
SynLink> network ssh set ssh_disable_pass on
Successfully saved setting.
Please restart network to change SSH password authentication setting.
```

##### `network ssh set ssh_disable_keys`
If enabled, key based authentication is disabled for ssh connection requests.
```shell
SynLink> network ssh set ssh_disable_keys on
Successfully saved setting.
Please restart network to change SSH Key based authentication setting.
```



### `network telnet`
`network telnet` is a subcategory of `network` section which deals with Telnet Server configurations.
```shell
SynLink> network telnet
----------------
Network Telnet Menu
----------------

Usage: network telnet [command] [options...]

Commands:
  status   Show current status and configurations for Telnet Server
  set      See configuration options to change Telnet Server settings

```

#### `network telnet status`
`network ssh status` will print in human readable format telnet configurations
```shell
SynLink> network telnet status
---------------
Telnet Settings
---------------
Availability:           Enabled
Status:                 Running
Port:                   23
```

#### `network telnet set`
`network telnet set` will show all available configurations to change. Follow ‘Usage’ section to change values of interest.
```shell
SynLink> network telnet set
-------------------------
Network Telnet Configurations
-------------------------

Usage: network telnet set [key] [value]

Key            | Values   | Default | Description
-------------------------------------------------------------------
telnet_enabled | on,off   | on      | Turns on or off Telnet Server
telnet_port    | PORT_NUM | 23      | Port to reach Telnet Server
-------------------------------------------------------------------

```

##### `network telnet set telnet_enabled`
Turn on or off Telnet Server. Valid inputs are either `on` or `off`
```shell
SynLink> network telnet set telnet_enabled on
Telnet Server stopped.
```

##### `network telnet set telnet_port`
Port input must follow [Valid Port](#valid-port) requirement.
```shell
SynLink> network telnet set telnet_port 1023
Successfully saved setting.
Please restart network to change telnet port.
```

### `network snmp`


```shell
SynLink> network 
----------------
Network SNMP Menu
----------------

Usage: network snmp [command] [options...]

Commands:
  status   Show current status and configurations for SNMP 
  set      See configuration options to change SNMP settings

```

#### `network snmp status`
```shell
SynLink> network snmp status
------------------
Network SNMP Status
------------------
TODO

```

#### `network snmp set`

```shell
SynLink> network snmp set
-------------------------
Network SNMP Configurations
-------------------------

Usage: network snmp set [key] [value]

Key               | Values     | Default | Description
-------------------------------------------------------------------------------------------
snmp_enabled      | on,off     | on      | Turns on or off SNMP Agent
snmp_trap_enabled | on,off     | off     | Turns on or off outgoing SNMP Trap Notifications
snmp_trap_ip      | IP_ADDRESS |         | SNMP Trap Receiver IP
-------------------------------------------------------------------------------------------

```


### `network syslog`
`network syslog` is a subcategory of `network` section which deals with Syslog related settings.

```shell
SynLink> network syslog
----------------
Network  Menu
----------------

Usage: network syslog [command] [options...]

Commands:
  status   Show current status and configurations for Syslog
  set      See configuration options to change Syslog settings

```

#### `network syslog status`
`network syslog status` will print in human readable format syslog configurations.
```shell
SynLink> network syslog status
---------------
Syslog Settings
---------------
Availability            Enabled
Status:                 Running
Port:                   514
Protocol:               RFC5424
Remote Logging:         Enabled
Remote Log IP:          192.168.1.181

```

#### `network syslog set`
`network syslog set` will show all available configurations to change. Follow 'Usage' section to change values of interest.
```shell
SynLink> network syslog set
-------------------------
Network Syslog Configurations
-------------------------

Usage: network syslog set [key] [value]

Key                       | Values   | Default | Description
--------------------------------------------------------------------------------------------------------
syslog_enabled            | on, off   | on      | Turns on or off syslog logging               
syslog_remote_log_enabled | on, off   | off     | Enable or disables sending logs to remote syslog server
syslog_remote_log_ip      | IP_ADDR   |         | Host IP for Remote Syslog Server               
syslog_port               | PORT      | 514     | UDP Port for Syslog Server               
syslog_protocol           | RFC_PROTO | RFC3164 | Can be either RFC3164 or RFC5424           
--------------------------------------------------------------------------------------------------------
```
##### `network syslog set syslog_enabled`
Enable or disables syslog logging.
```shell
SynLink> network syslog set syslog_enabled on
Successfully saved setting.
Syslog logging enabled.
```

##### `network syslog set syslog_remote_log_enabled`
Enable or disables remote logging. If no remote log IP configured, ignores on command.
```shell
SynLink> network syslog set syslog_remote_log_enabled on
No Remote Log IP set. Please set Remote Log IP with network syslog set syslog_remote_log_ip.
```
```shell
SynLink> network syslog set syslog_remote_log_enabled on
Successfully saved setting.
Syslog logging to remote log 192.168.1.205 enabled.
```

##### `network syslog set syslog_remote_log_ip`
Set IP address for remote log server to send syslog logs to. Must follow [Valid IP](#valid_ip) requirement.
```shell
SynLink> network syslog set syslog_remote_log_ip 192.168.1.205
Successfully saved setting.
Syslog remote log IP successfully set to 192.168.1.205.
To start logging, please enable syslog_remote_log_enabled setting.
```



### `network smtp`
`network smtp` is a subcategory of `network` section which deals with SMTP related notification settings.
```shell
SynLink> network smtp
----------------
Network SMTP Menu
----------------

Usage: network smtp [command] [options...]

Commands:
  status      Show current status and configurations for SMTP Notifications
  test_email  Sends test email to specified email address.
  set         See configuration options to change SMTP settings

```

#### `network smtp status`
`network smtp status` will print in human readable format SMTP configurations.
```shell
SynLink> network smtp status
------------------
Network SMTP Status
------------------
SMTP Account Email:    example@email.domain
SMTP Account Password: [Exists]
SMTP Port :            25
SMTP Authentication:   TODO

Events:
SMTP Message to test@email.domain triggered on Outlet 2-62893731 Outlet Max Current Threshold of 8.2 Amps
SMTP Message to test2@email.domain triggered on 192.168.1.186 Autoping Timeout of 5 times.
SMTP Message to test@email.domain triggered on Device Circuit Breaker Trip.

```
#### `network smtp test_email`
`network smtp test_email` will send a test message to specified email address.
```shell
SynLink> network smtp test_email
Which email address would you like to send a test_email to?
>test@email.domain
Sending Message on port 25 from example@email.domain to test@email.domain.
Successfully sent
```
```shell
SynLink> network smtp test_email test@email.domain
Sending Message on port 25 from example@email.domain to test@email.domain.
Successfully sent
```
```shell
SynLink> network smtp test_email test@email.domain
No SMTP settings configured. Please configure SMTP settings at network smtp set.
```
#### `network smtp set`
`network smtp set` will show all available configurations to change. Follow 'Usage' section to change values of interest.
```shell
SynLink> network smtp set
-------------------------
Network SMTP Configurations
-------------------------

Usage: network smtp set [key] [value]

Key                         | Values   | Default | Description
-------------------------------------------------------------------
smtp_notifications_enabled  Turns on or off any smtp notifications.
smtp_account_email          SMTP email address to send messages through
smtp_account_password       SMTP email password to authorize messages to be sent with
smtp_port                   Port for SMTP email
smtp_authentication_type    TODO
-------------------------------------------------------------------

```

### `network ntp`
`network ntp` is a subcategory of `network` section with deals with synchronizing time with a specified NTP server. Network Time Protocol (NTP).
```shell
SynLink> network ntp
----------------
Network NTP Menu
----------------

Usage: network ntp [command] [options...]

Commands:
  status   Show current status and configurations for NTP Info
  set      See configuration options to change NTP settings

```

#### `network ntp status`
`network ntp status` will print in human readable format NTP related configurations.
```shell
SynLink> network ntp status
------------------
Network NTP Status
------------------

Date is:          19/03/2020
Time is:          17:11:37
NTP Time:         ENABLED
NTP Host:         pool.ntp.org
Timezone:         TODO
Daylight Savings: TODO
```

#### `network ntp set`
`network ntp set` will show all available configurations to change. Follow 'Usage' section to change values of interest.
```shell
SynLink> network ntp set
--------------------------
Network NTP Configurations
--------------------------

Usage: network ntp set [key] [value]  

Key                   | Values    | Default | Description
-------------------------------------------------------------------------------------------
ntp_enabled           | on,off    | on           | Turns on or off NTP time synchronization
ntp_host              | on,off    | pool.ntp.org | NTP Host to reference for time
ntp_timezone          | TODO      | TODO         | Timezone string
ntp_daylight_savings  | TODO      | TODO         | TODO
-------------------------------------------------------------------------------------------
```

##### `network ntp set ntp_enabled`
Turns on or off NTP synchronization with specified NTP Host. Valid input is `on` or `off` keywords
```shell
SynLink> network ntp set ntp_enabled on
Synchronizing NTP with ntp host pool.ntp.org...
Synchronization successfully connected.
```

##### `network ntp set ntp_host`
Sets NTP Host to to reference for time on PDU.
```shell
SynLink> network ntp set ntp_host ntp-b.nist.gov
Successfully saved setting.
Synchronizing NTP with ntp host ntp-b.nist.gov...
Synchronization successfully connected
```

##### `network ntp set ntp_timezone`
TODO

##### `network ntp set ntp_daylight_savings`
TODO

### `network autoping`


```shell
SynLink> network autoping
-------------------------------
Network AutoPing Reboot Feature
-------------------------------

Usage: network autoping [command] [options...]

Commands:
  status   Show current status and configurations for AutoPing Reboot
  set      See configuration options to change AutoPing Reboot settings

```
#### `network autoping status`
```shell
SynLink> network autoping status
------------------------------
Network AutoPing Reboot Status
------------------------------

AutoPing Interval length is 3 seconds
AutoPing Timeout length is 3 seconds
AutoPing is ignored when no network connection found

AutoPing Events

1. 192.168.1.181, 5 timeout attempts.
   Action: 2-62893731 Outlet Reboot
   
1. 192.168.1.181, 5 timeout attempts.
   Action: Syslog logging
   
2. 192.168.1.171, 3 timeout attempts.
   Action: 3-62893731 Outlet Reboot
```

#### `network autoping test_ping`
If no IP address specified, prompt for IP address will appear.
```shell
SynLink> network autoping test_ping 192.168.1.1

[Press any key to stop ping]

PING 192.168.1.1 (192.168.1.1): 56 data bytes
64 bytes from 192.168.1.1: seq=0 ttl=64 time=0.742 ms
64 bytes from 192.168.1.1: seq=1 ttl=64 time=0.695 ms
64 bytes from 192.168.1.1: seq=2 ttl=64 time=0.702 ms
64 bytes from 192.168.1.1: seq=3 ttl=64 time=0.635 ms
```

#### `network autoping events`
```shell
SynLink> network autoping events
-----------------------
Network AutoPing Events
-----------------------

Usage: network autoping events [command] [options...]

Commands:
  list       Print all events which are triggered by autoping timeouts
  add        Add an event which is triggered by autoping timeouts
  remove     Remove an autoping event
```
##### `network autoping events list`
```shell
SynLink> network autoping events list
----------------------------
Network AutoPing Events List
----------------------------

AutoPing Events

1. 192.168.1.181, 5 timeout attempts.
   Action: 2-62893731 Outlet Reboot
   
1. 192.168.1.181, 5 timeout attempts.
   Action: Syslog logging
   
2. 192.168.1.171, 3 timeout attempts.
   Action: 3-62893731 Outlet Reboot
```
##### `network autoping events add`
```shell
SynLink> network autoping events add
--------------------------------
Network AutoPing Events Triggers
--------------------------------

Usage: network autoping events add [key] [arg1]

Key              | arg1       | arg2         | Description
------------------------------------------------------------------------------------------------------------------------
autoping_timeout | IP_ADDRESS | NUM_FAILURES | Turns on or off NTP time synchronization
--------------------------------------------------------------------------------------------------------------------------

```

##### `network autoping events add autoping_timeout`
```shell
SynLink> network autoping events add autoping_timeout 192.168.1.186 5
Which action to trigger?
[1] Set specified outlet to specified state (on,off)
[2] Reboot specified outlet
[3] Set specified outlet group to specified state (on,off)
[4] Reboot specified outlet group
[5] Email specified email address
[6] Outgoing HTTP Request (Web Hook) to specified IP Address
[7] Save to Syslog. Remote logging option found network syslog section
[8] SNMP Trap notification. SNMP Trap destination setting found in network snmp section.
```
##### `network autoping events remove`
```shell
SynLink> network autoping events remove

```
#### `network autoping set`
```shell
SynLink> network autoping set

```
##### `network autoping set autoping_interval`
```shell
SynLink> network autoping set autoping_interval

```
##### `network autoping set autoping_timeout`
```shell
SynLink> network autoping set autoping_timeout

```
##### `network autoping set autoping_network_active_enabled`
```shell
SynLink> network autoping set autoping_network_active_enabled

```
---

## `system`

### `system device`
#### `system device status`
return firmware and hardware versions

#### `system device events`
##### `system device events list`
##### `system device events add`
##### `system device events add device_3phase_imbalance`
##### `system device events add device_any_breaker_trip`

### `system banks`
#### `system banks status`
#### `system banks events`
##### `system banks events list`
##### `system banks events add`
###### `system banks events add bank_current_max_threshold`
###### `system banks events add bank_current_min_threshold`
###### `system banks events add bank_voltage_max_threshold`
###### `system banks events add bank_voltage_min_threshold`
###### `system banks events add bank_powerfactor_max_threshold`
###### `system banks events add bank_powerfactor_min_threshold`
###### `system banks events add bank_breaker_trip`
##### `system banks events remove`
#### `system banks set`
##### `system banks set bank_name`

### `system outlets`
#### `system outlets status`
#### `system outlets events`
##### `system outlets events list`
##### `system outlets events add`
###### `system outlets events add outlet_current_max_threshold`
###### `system outlets events add outlet_current_min_threshold`
##### `system outlets events remove`
#### `system outlets set`
##### `system outlets set outlet_name`
##### `system outlets set outlet_pwr_on_state`
##### `system outlets set outlet_state`
##### `system outlets set outlet_reboot`

### `system groups`
#### `system groups status`
#### `system groups list`
#### `system groups add`
#### `system groups edit`
#### `system groups remove`
#### `system groups events`
##### `system groups events list`
##### `system groups events add`
###### `system groups events add group_current_max_threshold`
###### `system groups events add group_current_min_threshold`
##### `system groups events remove`
#### `system groups set`
##### `system groups set group_name`
##### `system groups set group_pwr_on_state`
##### `system groups set group_state`
##### `system groups set group_reboot`

### `system sensors`
#### `system sensors status`
#### `system sensors events`
##### `system sensors events list`
##### `system sensors events add`
###### `system sensors events add temperature_1_max_threshold`
###### `system sensors events add temperature_1_min_threshold`
###### `system sensors events add temperature_1_max_humidity`
###### `system sensors events add temperature_1_min_humidity`
###### `system sensors events add temperature_2_max_threshold`
###### `system sensors events add temperature_2_min_threshold`
###### `system sensors events add temperature_2_max_humidity`
###### `system sensors events add temperature_2_min_humidity`
##### `system sensors events remove`
  
### `system lcd`
#### `system lcd status`
#### `system lcd set`
##### `system lcd set lcd_enabled`
##### `system lcd set lcd_orientation`
##### `system lcd set lcd_timeout`
##### `system lcd set lcd_backlight_timeout`
##### `system lcd set lcd_brightness`

### `system scheduling`
#### `system scheduling status`
#### `system scheduling events`
##### `system scheduling events scheduled_time`
##### `system scheduling events scheduled_interval`
#### `system scheduling set`

## `logs`
### `logs syslog`
### `logs banks`
### `logs outlets`
### `logs groups`

## `settings`
### `settings users`
#### `settings users list`
#### `settings users add`
#### `settings users edit`
#### `settings users remove`
#### `settings users events`
##### `settings users events list`
##### `settings users events add`
###### `settings users events add user_web_login`
###### `settings users events add user_web_logout`
###### `settings users events add user_http_pat_used`
###### `settings users events add user_added`
###### `settings users events add user_removed`
### `settings restart`
### `settings factory_reset`
### `settings firmware_update`

## `help`

# All Commands

Outlet or group related commands are unavailable if the PDU does not have any outlet switching or outlet current metering.


```
status          Print overview for PDU Status information
network         Print options to show and configure network info  
  status          Print all network status and configurations
  restart         Restart all networking related activity.
  ip              Print options to show and configure PDU IP
    status          Show current status and configurations for IP
    set             See configuration options to change IP settings
      static_ip       IP Address that unit will respond to if ip_assign is set to 'dhcp'
      subnet_mask     Subnet Mask that divides the IP address into network address and host address
      gateway_ip      IP Address for forwarding host (router) to other networks
      ip_assign       Choosing DHCP will have DHCP Server at 'gateway_ip' assign IP address to PDU
      primary_dns     Default DNS Server from Google
      secondary_dns   Default Secondary DNS Server from Google
      source_ip       Allows one specific IP to access PDU, or a group depending on source_subnet setting
      source_subnet   Can be set to allow a range of IP's to access PDU or a single IP
  web             Print options to show and configure HTTP(S) Webserver
    status          Show current status and configurations for HTTP(S) Webserver
    set             See configuration options to change HTTP(S) Webserver settings 
      web_enabled          Turns on or off HTTP(S) Server
      web_ssl_enabled      Enforces SSL certificate for HTTPS
      web_port             Port to reach Webserver
      web_max_users        Max number concurrent users on web browser. 0 is no limit
      web_timeout          Number seconds on web browser before auto log off. 0 is no timeout
      webhook_log_enabled  Enable or disables logging for web hook alerts without HTTP 200 responses.
  ssh             Print options to show and configure ssh info
    status          Show current status and configurations for SSH Server
    keys            Print options to manage SSH Keys
      list            List all current SSH Keys
      add             Add a SSH Key for passwordless logins. Prompts user for key name and key
      remove          Remove a single SSH Key
    set             See configuration options to change SSH settings
      ssh_enabled       Turns on or off SSH Server
      ssh_port          Port to reach SSH Server
      ssh_idle_timeout  Number seconds before SSH Server auto disconnects
      ssh_disable_keys  Disable key based Login
      ssh_disable_pass  Disable password logins
  telnet          Print options to show and configure telnet info
    status          Show current status and configurations for Telnet Server
    set             See configuration options to change Telnet settings
      telnet_enabled  Turns on or off Telnet Server
      telnet_port     Port to reach Telnet Server
  snmp            Print options to show and configure SNMP info
    status          Show current status and configurations for SNMP
    set             See configuration options to change SNMP settings
      snmp_enabled       Turns on or off SNMP Agent
      snmp_trap_enabled  Turns on or off outgoing SNMP Trap Notifications
      snmp_trap_ip       SNMP Trap Receiver IP
  syslog          Print options to show and configure syslog info
    status          Show current status and configurations for Syslog
    set             See configuration options to change Syslog settings
      syslog_enabled            Turns on or off syslog logging
      syslog_remote_log_enabled Enable or disables sending logs to remote syslog server
      syslog_remote_log_ip      Host IP for Remote Syslog Server
      syslog_port               UDP Port for Syslog Server               
      syslog_protocol           Can be either RFC3164 or RFC5424
  smtp            Print options to show and configure smtp info
    status          Show current status and configurations for SMTP Info
    test_email      Sends test email to specified email address. 
    set             See configuration options to change SMTP settings
      smtp_notifications_enabled  Turns on or off any smtp notifications.
      smtp_account_email          SMTP email address to send messages through
      smtp_account_password       SMTP email password to authorize messages to be sent with
      smtp_port                   Port for SMTP email
      smtp_authentication_type    TODO
  ntp             Print options to show and configure NTP info
    status          Show current status and configurations for NTP Info
    set             See configuration options to change NTP settings
      ntp_enable            Turns on or off NTP time synchronization
      ntp_host              NTP Host to reference for time
      ntp_timezone          Timezone string TODO
      ntp_daylight_savings  TODO
  autoping          See options to configure autoping reboot feature for PDU
    status            Show current status and configuration for autoping reboot feature
    test_ping         Send test_ping to specified ip address
    events            Print options to manage triggers from autoping related features
      list              Print all events which are triggered by autoping timeouts
      add               Add an event which is triggered by autoping timeouts
        autoping_timeout  Set trigger for autoping_timeout to specified ip address, with specified number of failures before action, and action
      remove            Remove an autoping event
    set               See configuration options to change AutoPing Reboot Settings
      autoping_interval                Interval in seconds between each individual ping request.
      autoping_timeout                 Timeout in seconds for a nonresponsive ping
      autoping_network_active_enabled  Turns on or off autoping requests if network is not currently active
system          Print options for PDU power, control, sensors, etc
  device          Print PDU Energy/Power Information and Configuration
    status          Print device configurations and related info
    events          Print options to manage triggers from PDU device activity
      list            Print all events triggered by PDU device activity
      add             Add an event which is triggered by 3 phase imbalance, breaker trips, etc.
        device_3phase_imbalance  Set trigger if 3 phase imbalance is detected at specified percentage
        device_any_breaker_trip  Set trigger if any breaker is tripped for a particular bank.
    set             See configuration options to change AutoPing Reboot Settings
      device_reboot_sequential_delay  On boot or when group reboots outlets turn on relays sequentially with a delay for number of seconds specified.
      device_host_name                Name for device which shows up on network searches
  banks           Print options for configuring and showing Bank (Branch) related info
    status          Print Power Stats, Energy Stats, and Overload Protection Info for bank(s)
    events          Print all events which trigger actions for bank related stats
      list            List all events triggered by bank activity
      add             Add an event and action to a particular bank activity
        bank_current_max_threshold      Prompts for max current value, bank_id, num seconds above threshold, and action. Set maximum threshold for specified current value in milli-amps. 
        bank_current_min_threshold      Prompts for min current value, bank_id, num seconds below threshold, and action. Set minimum threshold for specified current value in milli-amps, and number of seconds below threshold.  
        bank_voltage_max_threshold      Prompts for max voltage value, bank_id, num seconds above threshold,and action. Set maximum threshold for specified voltage value in volts. 
        bank_voltage_min_threshold      Prompts for min voltage value, bank_id, num seconds below threshold,and action. Set maximum threshold for specified voltage value in volts. 
        bank_powerfactor_max_threshold  Prompts for max powerfactor value (between 0-100), num seconds above threshold, bank_id, and action. Set maximum threshold for specified power factor value between 0 and 100. 
        bank_powerfactor_min_threshold  Prompts for min powerfactor value (between 0-100), num seconds below threshold, bank_id, and action. Set maximum threshold for specified power factor value between 0 and 100. 
        bank_breaker_trip               Prompts for bank_id, and action. Set trigger for bank_breaker tripping, and specified action.        
      remove          Prompt to remove an event and action
    set             See options to configure each bank
      bank_name       Prompts for bank_id and bank_name
  outlets         Print options for configuring and showing Outlet Related Info
    status          Print Outlet configurations and related info
    events          Print options to manage events which are trigged by outlet activity
      list            List all events triggered by outlet activity
      add             Add an event and action to an outlet's activity
        outlet_current_max_threshold  Prompts for max current value, outlet_id, num seconds above threshold, and action
        outlet_current_min_threshold  Prompts for min current value, outlet_id, num seconds below threshold, and action        
      remove          List all events triggered by outlet activity, prompts event_action_id to remove permanently    
    set             See options to configure individual outlets
      outlet_name          Change outlet name for a specified outlet. Lists Outlets, and prompts for outlet_id and outlet_name if no argument provided
      outlet_pwr_on_state  Change power on state for specified outlet. Lists Outlets, and prompts for outlet_id and pwr_on_state if no argument provided
      outlet_state         Change outlet relay state to specified state. Lists Outlets, and prompts for outlet_id and outlet_state if no argument provided
      outlet_reboot        Switches relay for specified outlet on and off at specified duration in seconds. Lists Outlets, and prompts for outlet_id if no argument provided.
  groups          Print options for configuring and showing group related info
    status          Print relevant information about outlet groups and their power consumption  
    list            Print a list of all groups, include name and outlets for each group 
    add             Prompts for group name, and which outlets to include
    edit            Prints a list of group names, and prompts to select one, then prompts for which outlets to include/exclude
    remove          Prints a list of group names, and prompts to select one to remove forever
    events          Prints options for events which trigger actions for group related stats
      list            List all events triggered by group activity
      add             Add an event and action to a group's activity. Prints list of possible event triggers
        group_current_max_threshold  Prompts for max current value, group_id, num seconds above threshold, and action
        group_current_min_threshold  Prompts for min current value, group_id, num seconds below threshold, and action
      remove          List all events triggered by group activity, Prompts for event_action_id to remove permanently
    set             See options to configure groups of outlets
      group_name          Change group name for a specified group. Lists groups, prompts for group_id and group name if no argument provided
      group_pwr_on_state  Change the power on state for all outlets of a group. Lists groups, prompts for group_id, and pwr_on_state  if no argument provided. Prompts user to warn all outlets will be changed
      group_state         Switch relay for outlet to specified state if outlet switching supported in PDU. Lists groups, prompts for group_id, and state  if no argument provided. Prompts warning.
      group_reboot        Switches specified relay on and off at specified duration in seconds. Lists groups, prompts for group_id  if no argument provided. Prompts warning.
  sensors         Print options for configuring and showing sensor related info
    status          Print relevant information about any attached sensors
    events          Prints options for events which trigger actions for sensor related info
      list            Print a list of all events triggered by sensor related info
      add             Add an event and action triggered by sensor activity
        temperature_1_max_threshold  Prompts for max temperature value in celsius for port 1, num seconds above threshold, and action
        temperature_1_min_threshold  Prompts for min temperature value in celsius for port 1, num seconds below threshold, and action
        humidity_1_max_threshold     Prompts for max humidity value value between 0-100 for port 1, num seconds above threshold, and action
        humidity_1_min_threshold     Prompts for min humidity value value between 0-100 for port 1, num seconds below threshold, and action
        temperature_2_max_threshold  Prompts for max temperature value in celsius for port 2, num seconds above threshold, and action
        temperature_2_min_threshold  Prompts for min temperature value in celsius for port 2, num seconds below threshold, and action
        humidity_2_max_threshold     Prompts for max humidity value value between 0-100 for port 2, num seconds above threshold, and action
        humidity_2_min_threshold     Prompts for min humidity value value between 0-100 for port 2, num seconds below threshold, and action
      remove          List all events triggered by sensor activity, prompts for event_action_id to remove permanently
    set             See options to configure sensor settings.
      temp_sensor_offset    Degrees in celsius to offset. Can be negative.
      temp_humidity_offset  Percentage units to offset. Can be negative.
  lcd             Print options to configure local LCD screen
    status          Print current LCD Configuration and status 
    set             See configuration options to change LCD settings
      lcd_enabled            Turns on or off LCD Screen
      lcd_orientation        Rotate screen between 90, 180, 270, and 0/360 deg
      lcd_timeout            Number of seconds before overview screensaver appears
      lcd_backlight_timeout  Number of seconds before backlight turns off
      lcd_brightness         Change brightness of LCD
  scheduling      Print options to configure any scheduled actions
    status          Print information on current scheduled actions
    events          Print options for events which are triggered on scheduled timer
      scheduled_time      Create scheduled event for specified time once per day, for specified number of times (0, for infinite) 
      scheduled_interval  Create scheduled event for specified interval in seconds, for specified number of times (0 for infinite)
    set
      scheduled_events_enabled  Turn on or off for scheduled events to trigger specified actions. 
logs            Print options to configure and see any relevant logs
  syslog          Print options to configure and see syslog info
    list            Print a list of all syslogs
    clear           Clear all syslog entries. Prompts for confirmation
    events          Print options to manage syslog actions triggered by specified events
      list            Print a list of all events which result in syslog actions
      remove          List all events which result in syslog logging actions, prompts for event_action_id
    set             See configuration options to change syslog settings
      syslog_enabled             Turn on or off syslog logging
      syslog_remote_log_enabled  Enable or disable sending logs to remote syslog server
      syslog_remote_log_ip       Host IP for Remote Syslog Server
  banks           Print options to configure and see bank power/energy logging
    list            Print a list of relevant power/energy logs for PDU Banks 
    clear           Clear all bank related power/energy logs. Prompts for confirmation
    set             See configuration options to change bank power/energy logging
      bank_logging_enabled               Turns on or off logging for bank power/energy stats
      bank_logging_voltage_enabled       Turn on or off logging for voltage stats
      bank_logging_power_factor_enabled  Turn on or off logging for power factor stats
      bank_logging_current_enabled       Turn on or off logging for current stats
  outlets         Print options to configure and see outlet power logging
    list            Print a list of relevant outlet power logs
    clear           Clear all outlet logging logs. Prompts for confirmation
    set             See configuration options to change outlet power logging
      outlet_logging_enabled  Turns on or off logging for individual outlet current stats
      outlet_logging_outlets  Prompts for which outlets to log current stats
  groups         Print options to configure and see outlet power logging
    list            Print a list of relevant group power logs
    clear           Clear all group logging logs. Prompts for confirmation
    set             See configuration options to change group power logging
      group_logging_enabled  Turns on or off logging for individual group current stats
      group_logging_groups  Prompts for which groups to log current stats
settings        Print all SynLink settings to manage
  users           Print options to manage SynLink users
    list            List all SynLink Users
    add             Add a user. Prompts for username, password, and permissions             
    edit            Lists all users, Prompts for username, and specified attribute to change
    remove          Remove a user permanently. Lists all usernames, prompts for username, and confirmation
    events          Print options to manage events triggered by user activity
      list            Print a list of all events triggered by user activity
      add             Add event triggered by user activity. Prints list of possible event triggers
        user_web_login      Prompts for user_id (or all users) and action
        user_web_logout     Prompts for user_id (or all users) and action
        user_http_pat_used  Prompts for user_id (or all users) and action
        user_added          Logs whenever a user is created. Prompts for action
        user_removed        Logs whenever a user is removed. Prompts for action
      remove          List all events triggered by user activity. Prompts for event_action_id
  restart         Restart SynLink PDU Controller. Prompts for confirmation.
  factory_reset   Reset SynLink PDU to factory default. Will restart all network connections and change IP if not 192.168.1.100.
  firmware_update Updates Synlink Controller when appropriate USB Drive is inserted.
help            Print help menu with top level list of commands.
```

---

# Reference

## Valid Entries

### IP Validation
Valid IP4 includes 4 octets separated by. TODO

## Event Actions
```shell
SynLink> network autoping events add autoping_timeout 192.168.1.186 5
Which action to trigger?
[1] Set specified outlet to specified state (on,off)
  Which Outlet would you like to switch?
  [1] 1-62893731 Outlet 1 for Bank 1 
  [2] 2-62893731 Outlet 2 for Bank 1
  [3] 3-62893731 Outlet 3 for Bank 1
  [4] 4-62893731 Outlet 4 for Bank 1
  [5]...
  >
    Which state would you like the outlet to be at after action is triggered?
    [1] on
    [2] off
    >
[2] Reboot specified outlet
  Which Outlet would you like to reboot?
  [1] 1-62893731 Outlet 1 for Bank 1 
  [2] 2-62893731 Outlet 2 for Bank 1
  [3] 3-62893731 Outlet 3 for Bank 1
  [4] 4-62893731 Outlet 4 for Bank 1
  [5]...
  >
    How long should the reboot cycle take?
    Valid response: Between 1-10 in seconds
    >
[3] Set specified outlet group to specified state (on,off)
  Which Group would you like to switch?
  [1] nas_servers
  [2] switches
  [3] web_servers
  >
    Which state would you like the group outlets to be at after action is triggered?
    [1] on
    [2] off
    >
[4] Reboot specified outlet group
  No Outlet Groups have been created. To create a group of outlets go to system groups
  --
  Which Group would you like to reboot?
  [1] nas_servers
  [2] switches
  [3] web_servers
  >
    How long should the reboot cycle take?
    Valid response: Between 1-10 in seconds
    >
[5] Email specified email address
  Which email address would you like to notify?
  >
  --
  No Outgoing Email Settings Found. Please configure network smtp settings first.
  --
[6] Outgoing HTTP Request (Web Hook) to specified IP Address
  Which IP or URL would you like to send a POST request to?
  >
[7] Save to Syslog. Remote logging option found network syslog section
  Syslog logging for specified event trigger created. Please enable syslog to register action.
  --
  Syslog logging for specified event trigger created and enabled.
[8] SNMP Trap notification. SNMP Trap destination setting found in network snmp section.
  No SNMP Trap Receiver specified. Please specify configuration in network snmp.
  --
  SNMP Trap notification for event trigger created. Please enable SNMP to register actions.
  --
  SNMP Trap notification for event trigger created and enabled.
```

## All Configuration Values
```
static_ip                          IP Address that unit will respond to if ip_assign is set to 'dhcp'
subnet_mask                        Subnet Mask that divides the IP address into network address and host address
gateway_ip                         IP Address for forwarding host (router) to other networks
ip_assign                          Choosing DHCP will have DHCP Server at 'gateway_ip' assign IP address to PDU
primary_dns                        Default DNS Server from Google
secondary_dns                      Default Secondary DNS Server from Google
source_ip                          Allows one specific IP to access PDU, or a group depending on source_subnet setting
source_subnet                      Can be set to allow a range of IP's to access PDU or a single IP
web_enabled                        Turns on or off HTTP(S) Server
web_ssl_enabled                    Enforces SSL certificate for HTTPS
web_port                           Port to reach Webserver
web_max_users                      Max number concurrent users on web browser. 0 is no limit
web_timeout                        Number seconds on web browser before auto log off. 0 is no timeout
webhook_log_enabled                Enable or disables logging for web hook alerts without HTTP 200 responses.
ssh_enabled                        Turns on or off SSH Server
ssh_port                           Port to reach SSH Server
ssh_idle_timeout                   Number seconds before SSH Server auto disconnects
ssh_disable_keys                   Disable key based Login
ssh_disable_pass                   Disable password logins
telnet_enabled                     Turns on or off Telnet Server
telnet_port                        Port to reach Telnet Server
snmp_enabled                       Turns on or off SNMP Agent
snmp_trap_enabled                  Turns on or off outgoing SNMP Trap Notifications
snmp_trap_ip                       SNMP Trap Receiver IP
syslog_enabled                     Turns on or off syslog logging
syslog_remote_log_enabled          Enable or disables sending logs to remote syslog server
syslog_remote_log_ip               Host IP for Remote Syslog Server
syslog_port                        UDP Port for Syslog Server               
syslog_protocol                    Can be either RFC3164 or RFC5424
smtp_notifications_enabled         Turns on or off any smtp notifications.
smtp_account_email                 SMTP email address to send messages through
smtp_account_password              SMTP email password to authorize messages to be sent with
smtp_port                          Port for SMTP email
smtp_authentication_type           TODO
ntp_enable                         Turns on or off NTP time synchronization
ntp_host                           NTP Host to reference for time
ntp_timezone                       Timezone string TODO
ntp_daylight_savings               TODO
autoping_interval                  Interval in seconds between each individual ping request.
autoping_timeout                   Timeout in seconds for a nonresponsive ping
autoping_network_active_enabled    Turns on or off autoping requests if network is not currently active
device_reboot_sequential_delay     On boot or when group reboots outlets turn on relays sequentially with a delay for number of seconds specified.
device_host_name                   Name for device which shows up on network searches
temp_1_sensor_offset               Degrees in celsius to offset. Can be negative.
temp_1_humidity_offset             Percentage units to offset. Can be negative.
temp_2_sensor_offset               Degrees in celsius to offset. Can be negative.
temp_2_humidity_offset             Percentage units to offset. Can be negative.
lcd_enabled                        Turns on or off LCD Screen
lcd_orientation                    Rotate screen between 90, 180, 270, and 0/360 deg
lcd_timeout                        Number of seconds before overview screensaver appears
lcd_backlight_timeout              Number of seconds before backlight turns off
lcd_brightness                     Change brightness of LCD
scheduled_events_enabled           Turn on or off for scheduled events to trigger specified actions. 
bank_logging_enabled               Turns on or off logging for bank power/energy stats
bank_logging_voltage_enabled       Turn on or off logging for voltage stats
bank_logging_power_factor_enabled  Turn on or off logging for power factor stats
bank_logging_current_enabled       Turn on or off logging for current stats
outlet_logging_enabled             Turns on or off logging for individual outlet current stats  ???? keep?
outlet_logging_outlets             Prompts for which outlets to log current stats            ????? keep?
group_logging_enabled              Turns on or off logging for individual group current stats ????? keep?
group_logging_groups               Prompts for which groups to log current stats               ????? keep?
```


### All Event Triggers
```
autoping_timeout                Set trigger for autoping_timeout to specified ip address, with specified number of failures before action, and action
device_3phase_imbalance         Set trigger if 3 phase imbalance is detected at specified percentage
device_any_breaker_trip         Set trigger if any breaker is tripped for a particular bank.
bank_current_max_threshold      Prompts for max current value, bank_id, num seconds above threshold, and action. Set maximum threshold for specified current value in milli-amps. 
bank_current_min_threshold      Prompts for min current value, bank_id, num seconds below threshold, and action. Set minimum threshold for specified current value in milli-amps, and number of seconds below threshold.  
bank_voltage_max_threshold      Prompts for max voltage value, bank_id, num seconds above threshold,and action. Set maximum threshold for specified voltage value in volts. 
bank_voltage_min_threshold      Prompts for min voltage value, bank_id, num seconds below threshold,and action. Set maximum threshold for specified voltage value in volts. 
bank_powerfactor_max_threshold  Prompts for max powerfactor value (between 0-100), num seconds above threshold, bank_id, and action. Set maximum threshold for specified power factor value between 0 and 100. 
bank_powerfactor_min_threshold  Prompts for min powerfactor value (between 0-100), num seconds below threshold, bank_id, and action. Set maximum threshold for specified power factor value between 0 and 100. 
bank_breaker_trip               Prompts for bank_id, and action. Set trigger for bank_breaker tripping, and specified action.        
outlet_current_max_threshold    Prompts for max current value, outlet_id, num seconds above threshold, and action
outlet_current_min_threshold    Prompts for min current value, outlet_id, num seconds below threshold, and action        
group_current_max_threshold     Prompts for max current value, group_id, num seconds above threshold, and action
group_current_min_threshold     Prompts for min current value, group_id, num seconds below threshold, and action
temperature_1_max_threshold     Prompts for max temperature value in celsius for port 1, num seconds above threshold, and action
temperature_1_min_threshold     Prompts for min temperature value in celsius for port 1, num seconds below threshold, and action
humidity_1_max_threshold        Prompts for max humidity value value between 0-100 for port 1, num seconds above threshold, and action
humidity_1_min_threshold        Prompts for min humidity value value between 0-100 for port 1, num seconds below threshold, and action
temperature_2_max_threshold     Prompts for max temperature value in celsius for port 2, num seconds above threshold, and action
temperature_2_min_threshold     Prompts for min temperature value in celsius for port 2, num seconds below threshold, and action
humidity_2_max_threshold        Prompts for max humidity value value between 0-100 for port 2, num seconds above threshold, and action
humidity_2_min_threshold        Prompts for min humidity value value between 0-100 for port 2, num seconds below threshold, and action
scheduled_time                  Create scheduled event for specified time once per day, for specified number of times (0, for infinite) 
scheduled_interval              Create scheduled event for specified interval in seconds, for specified number of times (0 for infinite)
user_web_login                  Prompts for user_id (or all users) and action
user_web_logout                 Prompts for user_id (or all users) and action
user_http_pat_used              Prompts for user_id (or all users) and action
user_added                      Logs whenever a user is created. Prompts for action
user_removed                    Logs whenever a user is removed. Prompts for action
```