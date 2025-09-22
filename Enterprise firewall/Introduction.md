The whole idea is to cover every area, modern day networks have this problem of rogue stuff everywhere, byod, usb sticks, rogue devices

Thats why we have forti-everything (altough theyve become a target themselves for how easy it is to screw any forti-shits)

### Architectures
#### DCFW
data center firewall
![[Pasted image 20250909123305.png]]
- network filtering tupple
- ngfw stuff
	- app control
	- web control 
	- ssl inspection
- microsegmentiation
- the 1000 series

#### ISFW
Internet service firewall
![[Pasted image 20250909125050.png]]

The 100s series

Same advantages plus forti ap



### Cloud
![[Pasted image 20250918141516.png]]
The same as having a fortigate unit, but running on an vm, that sits in between as if it was a physical device, also we have fortiweb cloud, forticwp for workload security, forticasb for cloud application control and fortisandbox cloud, fortimail, fortimanager and fortianalyzer. Also the fortisase saas solution

### Distribuido
![[Pasted image 20250918152529.png]]
Combines the previous topologies into a complete arch
Traditional firewall
• VPNs
• NGFW
• Segmentation
• Distributed NGFW
• Low latency
• Integrated wireless LAN
• HW SSL inspection (on-premise devices)
• Secure SD-WAN
• Hyperscale
• DDoS protection to detect and mitigate malicious traffic aimed at overwhelming network resources,
ensuring continuous availability and service
• 5G/LTE for secure wireless WAN connectivity for remote and mobile deployments, supported by specific
models and the FortiExtender series support this feature
• ZTNA to enforce strict identity verification for network access, adhering to the "never trust, always verify"
principle. Key elements include FortiGate, FortiClient, FortiAuthenticator, FortiToken, and FortiNAC

### DEFW
![[Pasted image 20250918152634.png]]
The most common of them all, this uses the 40-90s series, sometimes the 100 series might be needed for larger branches

### Hybrid mesh
![[Pasted image 20250918152845.png]]

Uses a variety of models to construct a large network that connects datacenters and other networks

![[Pasted image 20250918153007.png]]

Each model has its own NP with its own capabilites, such as offloading already created sessions, handling ipsec encryption
	Similarly there is the Content processor, that acts as a co processor to the cpu, it handles pattern macthing, ips engine, ssl/tls decrpytion, and sometimes ipsec encryption

# Life of a packet

1. The NP (network processor) handles initial security checks, such as ACL -> Host protection engine HPE -> IP header integrity check -> IPSec decryption.
2. Afther that, the packet gets passed to the kernel, which handles
	1. Destination nat
	2. routing, spf check, sdwan, stateful inspection, policy lookups, session managment
	3. Session helpers
	4. User authentication
	5. Device identification
	6. SSL VPN
	7. Local managment traffic
	
	Pretty much all of the routing processes and l3 checks
	
3. Then it goes through the UTM
	1. Flow based inspection
	2. Proxy based inspection
	3. Explicit web proxy
	4. botnet check
4. Then again to l3, but this time for forwarding
	1. forwarding
	2. snat
5. ipsec encryption
6. traffic shaping
7. wan optimization
8. network iface







