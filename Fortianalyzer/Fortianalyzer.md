==Fortianallyzer works as a central repository for fortinet devices==

==El dbms de fortianalyzer es postgres==
![[Pasted image 20241017171649.png]]

- Fortigate
- FortiAnalllyzer
- FortiCache
- FortiClient
- FortiDDoS
- FortiMail
- FortiManager
- FortiNAC
- FortiSandbox
- FortiSOAR
- FortiWeb
- Syslog
- Chassis

![[Pasted image 20241019133447.png]]



#### Usos del fortianalyzer
- Reportar
	Los reportes dan un panorama claro de eventos, actividades y modas en los dispositivos
- Eventos
	Fortianalyzer puede generar alertas en base condiciones especificas
- Archivo de contenido
	Loggear y archivar contenido riesgoso


### Modos de operación
Dashboard > system information

#### Analyzer
==default==
Actúa como un agregador de logs central para uno o más collectores
![[Pasted image 20241019134910.png]]

#### Collector
Funciona para collectar logs de varios dispositivos, para así reenviarlos a otro fortianalyzer o en su defecto dispositivo de logs 
Por lo mismo, no cuenta con caracteristicas de reporte, solo con archivado

Los eventos se reenvian as is en formato binario

Puede enviar a syslog servers en formato CEF (common event format)

![[Pasted image 20241019135651.png]]

Ejemplo de colaboración de analyzer y collector

El collector se optimiza en operación  y con espacio en disco (manual) para dedicarse a recibir y reenviar logs, así como almacenar en caso de ser necesario para redes de alta latencia

Y el analyzer se encarga de analyzar



### Security fabric
- Almacenar logs por grupos de security fabric
- Solo se logea la primer entrada de una sesión para la sec (ya que conocen la mac de sus peers) fabric a menos que:
	- Un FGT upstream NATee
	- Un FGT levante un utm log

- Los logs de utm y tráfico se correlaciónan a las sesiones


### Fortianalyzer fabric
==Members DO NOT send logs to supervisor==
==Centralized viewing of devices, incidents and events across fortianalyzer==

==Supervisor and members must be in the same time zone==

==Supervisor and members==
==Collectors cant be members==

Supervisors are the root device and there are only one per fabric
Supervisors cannot be HA

![[Pasted image 20241019150929.png]]


### ADOMs
==Dashboard > System information > ADOMs

```
config system global
	set adom status {enable | disable}
end
```
ADOMs are like VDOMs, that are used for limit administration access, manage data polici, disk space allocation


### Default settings
- admin
- \<blank>
- port1
- 192.168.1.99
- 255.255.255.0
- http, ssh

```
config system interface
```


### Initial config
As usual
You have to assign an interface with protocols for admin access, as well with ip, dns, gateway

All this in network page
![[Pasted image 20241019163414.png]]

Also may need to setup ntp, link aggregation and vlans


##### Commands
```
execute reset all-settings
```

conserves ip and routes
```
execute reset all-except-ip
```

Conserves only ip and route
```
execute format disk
```

```
get system status
```

```
get system interface
```

```
show system dns
```

```
get system ntp
```

```
show system route
```

```
execute ping
```

```
diagnose system ntp status
```
```
diagnose system print cpuinfp
```
```
diagnose system print df
```
```
diagnose system hosts
```
```
diagnose system print netstat
```

```
diagnose system print route
```


### Admin access controls
##### admin pass
==FAZ has no way to recover passwords, so you should remember it==
==If ever forgotten, you may migrate config with execute migrate==

System settings > administrators > change password

System settings > settings > password policy

##### 2fa is under system settings > administrators > token

### Obvious security recommendations

![[Pasted image 20241019164816.png]]

Obvious security recommendations

![[Pasted image 20241019164850.png]]


Least privilege principle for the default user  role types

![[Pasted image 20241019165000.png]]

Trusted hosts are ips recognized for an admin user
![[Pasted image 20241019165116.png]]


Just like VDOMS, ADOMS serve to seggregate permission, super_admins just like FGT, have access to all ADOMs

![[Pasted image 20241019165232.png]]


### Remote authentication

System settings > remote authentication server

- LDAP
- Radius
- TACACS
- PKI

The wild card (match all users on remote) feature allows to indetify users from one or more groups 

For instance admin1 in both; radius and ldap server

You can set remote authentication server groups, which are listed as GROUP in the Admin Type field, to
extend administrator access. Usually, you create a wildcard administrator for only a single server. However, if
you group multiple servers, you can apply a wildcard administrator to all the servers in the group. If you added
an LDAP and RADIUS server to your authentication group, and the administrator has login credentials on both
servers, then the administrator can authenticate on FortiAnalyzer using either their LDAP or RADIUS
credentials.

remote auth server groups are listed as administrator type groups on admin users

### SAML
Fortianalyzer can play the role of identity provider IP , service provider SP or fabric sp

Fabric sp registeri tself to fabric root of fortigate 


==Wildcard administration allows administrators to login from remote servers==


### monitroing login status

diagnose system admin-session status
or system settings > administrators

LLogged users are ids by a checkmark
by default this is only availlable to super_admin, same goes for login events under system > event logs

Same goes to task monitor under 
System settings > advanced > task monitor

System events shows all system and admin invoked events



### Commands

Check admins
diagnose system admin-session status






### ADOMs
==Not enabled by defo==
##### enable adom

config system gllobal
	set adom-status {enable | disable}
end


##### Operation modes

###### Normal (default) (global)
A fortigate device and all of its vdoms must be assigned to a single adom

###### advanced
A fortigate device and all of its vdoms can belong to multiple adoms to use forti viewm reports, event managment and analyze data for individual vdoms

![[Pasted image 20241021215240.png]]


### Creating ADOM
==System settings > ADOM==

diagnose dvm adom list

- Can create custom adom in case the preexistent ones dont fille the bill
- Custom adoms cant be deleted if there is one or more devices of them in existence
- Disk quota is configured by adom
- Default adom  type is fabric





### Security fabirc adom
![[Pasted image 20241021215610.png]]

Para contener todos los dispositivos de un security fabric en un solo adom para

- Procesamiento de datos rapido
- Correlacion de logs
- Reportes
- Fortivuew
- Incidente y eventos
- Device manager
- Log view


### Finite disk
==When space is full, the older logs are overwritten==
==An automatic alert gets generated==

Can config to stop logging when full

config system locallog disk setting
	set diskfull nolog
end


![[Pasted image 20241021230443.png]]

There are two types of logs

- Archive logs . Compressed on hard disks and offline
- Analytic - Stored in sql database and online

An indexed analytic db log avg size is 600 bytes, while archive is 80 bytes, so we need to keep that in mind for the radio

The default radio is 70-30

### Quotas

==By default each ADOM allows 1000 mb (1 gb) to store log data==

==Cant set the minimum below 100 mb==

The system reserves 5 to 20% of disk to unexpected quota an overflow, leaving only 80 to 95 %  of disk for devices

![[Pasted image 20241021230804.png]]

small < 500 gb
medium 500 - 10000 gb
large 1000 gb - 3000 gb

very large 3000 - 5000 gb

you can get this info with
diagnose log device

##### Processes

- logfiled
	- Monitors filesize, sql database size, archive size and sends commands to toher daemons
	- If space is higher than 95%, it removes older files until 85%
- sqlplugind
	- Enforces sql databasesize
- oftpd
	- Enforces archive size

Increase ADOM quota as needed, othwerwise you might run into problems storing logs and cpu usage due to the deamons enforcing space


##### Adding space 

execute lvm extend

By hardware you need to increase raid



### Backup
==System > dashboard==

System backups contain

System information
Device list
Report info

Of course, logs and generatd reports are not included

==You can only restore to the same model and firmware version==

execute backup


### Best practices

shutdown gracefully
 execute shutdown

Run on an ups

save unencrypted backup to a secure lcoation


Sync with ntp

Backup logs and create a plan

HA and link aggregation

Check compatibility

Check and follow upgrade path



### RAID
==Reduntandt array of indepenedent disks==

- Reliability of data on critical events
- Capacity and performance
- NOT all raid leevels provide fault tolerance
- NOT all devices support raid

RAID combines physical drives into a logical drive distributing space among them



#### modes
basic
	mirrroring
		makes copies of the data in two or more physical drives
	stripping
		combines two or more physical drives into a logical unit and distributes the chunks of data among them



RAID can be hardware and software, hardware is preferred as it gets handled by a dedicated controller card, faster opossed to software which the os has to handle

##### Raid levels

![[Pasted image 20241021233413.png]]

- Raid 0 - stripping
	- no fault tolerance because no redundance
	- if one disk fails allll others do
- Raid 1 - mirroring
	- fault tolerant oriented
- raid 5 - block level stripping distributed
	- Can tolerate the fault  of a single drive as data and parity are stripped across three or more disk, making it poossible to calculate subsequent reads from the distributed party
- Raid 6 - Adds another disk parity, making it so even if two disks fail

![[Pasted image 20241021233713.png]]

- raid 10
	- combines 1 and 0
	- stripping + mirroring
	- can withstand two hd fails, but depends on which ones did
- raid 50 
	- stripping + sitribuited  party
- raid 60
	- raid 6+0

==System settings > managment==

You can see the states of the dissk here

- Good - normal
- Rebuilding - adding data to a newly added device, not fault tolerant until finishe
- Initializing - Writing to all the drives in the device to make it fault tolerant
- Veryfing - Checking paritiy of data
- degraded - No longer used for raid
- Inoperable - cant be accesed


Hot swapping hd is only supported for hardware level raid


diagnose system raid status
diagnose system raid hwinfo
diagnose system disk info
diagnose system disk health
diagnose system disk errors
diagnose system disk attributes


### HA
Creates redundancy
Syncs llogs,data among fortiananalyzer devices, all devices must run in the same mode with the same model an firmware


Modes of operation

Active-passive

Only the ha can receive logs and archive from directly  connected devices 
Only primary can forward logs

Active - active
All peers can archive and receive from connected devices
All peers can forward lgos

==Settings > HA==

The primary device and all secondary devices must have the same Group Name, Group ID, and Password.
The Priority setting is used during the selection of the primary device in the cluster. You can assign a value
from 80 to 120, where a higher number has higher priority. The Log Data Sync option is enabled by default. It
provides real-time log synchronization among cluster members, after the initial log synchronization


#### Primary election process

Initial

Preferred role > Primary already elected

Failure

Priority (80,120 default 100,highest priority the better) > secndary with higher ip becomes primary



#### SYNC
- initail sync
	log sync with new devices added to the cluster
- Real time sync
	log data sync (enabled by default)
	the porimary forwards logs to all secondary if not possible, the next to be promtoed does this

![[Pasted image 20241022010525.png]]


Load balacning is available for some featrues, such as reporting

diagnose ha status


### Licencing

- registered 
	authorized to store logs on fortianalyzer
- unregistered
	- Reuqesting to store logs


##### Method 1 request from a supprted device

sec fabric > fabric connectors  > logs and annallitics

If using ADOMS you need to do this on the adom


### Method 2 device manager with serial nuimber


### method 3 device wizard with a pre shared key


### device groups
default device group

### Fortigate collected logs

- logs
	- Traffic
	- Events 
	- Security
- Data loss prevention
	- email
	- im
	- Web traffic
	- FTP
	- NNTP
- Quarintine
- IPS  packet log


![[Pasted image 20241022012113.png]]

![[Pasted image 20241022012156.png]]

### Moving between adoms
Dont do unless needed, by default only super admin can do so

. ensure the new adom has enough quota

rebuild the new adom database if there are new reports (migrate archived)

execute sql-local rebuild-adom (new adom)

also appliable for the old adom database (analythic llogs)


on HA fortigates, the primary is the reponsible of sending logs and logs are created for every fgt by serial number


### logging flow
1. Logs are received and decompressed and saved in disk.  Logs have a log file extension, for instance fgt has tlog.log and elog.log for traffic/security events and events respectively
2. At the same time logs are indexed into the sql database, these are the analytic logs available trough fortiview, log view, incident and events and reports
3. Finally when the log reaches a configured size, these are rolled. Compressing it on tar with a timestamp as the name. These are the archive logs. available in log browse

### Log backup
execute backup logs 

log view > log type > donwload

also configurable per roll in

system settings > advanced > device log setting

trough ftp, scp, sftp


Also available to cloud services such as amazon and google, or to fortigate clloud with a storage connector service license, with a quota on how much you can upload

### log forwarding
config system log-forward
	edit 1
		set mode aggregation,frowarding,disable
	end

aggregation logs and content files stored and uploaded in schedule
forwarding realtime forwarding

Configure the server (the log recipient). Forwarding mode only requires configuration on the client side. In
aggregation mode, the FortiAnalyzer acting as the server must be configured to accept the logs from the
client.
3. Configure the client (the FortiAnalyzer forwarding the logs). Here you can also specify which device logs
to forward and set log filters to send only logs that match filter criteria.

ortiAnalyzer and FortiGate use OFTP over SSL when information is synchronized between them. OFTP
listens on port TCP/514. Port UDP/514 is used for unencrypted log communication.

To prevent logs from being tampered with while in storage, you can add a log checksum using the config
system global command. You can configure FortiAnalyzer to record a log file hash value, timestamp, and
authentication code when the log is rolled and archived and when the log is uploaded (if that feature is
enabled). This can also help against man-in-the-middle only for the transmission from FortiAnalyzer to an
SSH File Transfer Protocol (SFTP) server during log upload.
The following log checksums are available:
• md5: Record log file MD5 hash value only.
• md5-auth: Record log file MD5 hash value and authentication code.
• none: Do not record the log file checksum.


### SQL hard cache
caches from precompiled hard cache sql data