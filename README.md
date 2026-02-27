# Overview Lab

This MPLS lab runs on Proxmox and PNETLAB with specifications of 4Core CPU, 16GB RAM and 100GB Disk. It uses MikroTik CHRx86 6.49.18 running on PNETLAB.

### Topology Target

Topology Lab MPLS

<p align="center">
<img src="Topology/Topology-MPLS-MikroTik-v2.png">
</p>

### OSPFv2

OSPF (Open Shortest Path First) is a link-state routing protocol that allows routers to build a full map of the network topology to determine the fastest paths. It operates by discovering neighbors via Hello packets, exchanging Link-State Advertisements (LSAs) to synchronize a Link-State Database (LSDB), and running Dijkstra's Shortest Path First (SPF) algorithm.

This MPLS lab uses OSPFv2 routing

<p align="center">
<img src="OSPFv2/Topology OSPF Routing.png">
</p>

### MPLS LDP

Multi-Protocol Label Switching (MPLS) is an advanced packet-forwarding technique used in modern networks. Instead of making routers look into complex Layer 3 routing tables for every IP packet, MPLS uses labels for forwarding decisions. These labels create pre-defined, efficient paths across the network, which enhances speed, scalability and traffic management.

LDP is a protocol that automatically generates and exchanges labels between routers. Each router will locally generate labels for its prefixes and will then advertise the label values to its neighbors.

<p align="center">
<img src="MPLS-LDP/Topology MPLS LDP Signaling.png">
</p>


### L2VPN Virtual Private LAN Services

VPLS is an Ethernet-based point-to-multipoint Layer 2 VPN. It allows you to connect geographically dispersed Ethernet local area networks (LAN) sites to each other across an MPLS backbone. For customers who implement VPLS, all sites appear to be in the same Ethernet LAN even though traffic travels across the service provider's network.

VPLS, in its implementation and configuration, has much in common with a Layer 2 VPN. In VPLS, a packet originating within a service provider customer’s network is sent first to a customer edge (CE) device (for example, a router or Ethernet switch). It is then sent to a provider edge (PE) router within the service provider’s network. The packet traverses the service provider’s network over a MPLS label-switched path (LSP). It arrives at the egress PE router, which then forwards the traffic to the CE device at the destination customer site.

<p align="center">
<img src="L2VPN-VPLS/Topology VPLS CE1-CE7.png">
</p>


### IBGP Router Reflector Concept

A BGP Route Reflector (RR) reduces iBGP full-mesh requirements by acting as a central hub that "reflects" routes between client routers within an Autonomous System (AS). It breaks the split-horizon rule (iBGP-to-iBGP), allowing clients to peer only with the RR, simplifying configuration and reducing CPU/network overhead.

Router Reflector (RR1)

<p align="center">
<img src="IBGP-RouteReflector/Topology Route-Reflector RR1.png">
</p>

Router Reflector (RR2)

<p align="center">
<img src="IBGP-RouteReflector/Topology Route-Reflector RR2.png">
</p>

### L3VPN Virtual Routing Forwarding

L3VPN in BGP (specifically BGP/MPLS IP VPN) works by using Multiprotocol BGP (MP-BGP) to distribute customer routes between Provider Edge (PE) routers, while using MPLS to tunnel traffic across the backbone. PE routers use VRFs to maintain separate routing tables per customer, assign Route Distinguishers (RDs) to make routes unique, and use Route Targets (RTs) to control route import/export

<p align="center">
<img src="L3VPN-VRF/Topology VRF CE6-CE12.png">
</p>

### Verification with Script

Example Verification (Traceroute)

<p align="center">
<img src="Verification/Traceroute Test-3.png">
</p>

Step Verification via SecureCRT

  1. Click File -> Log Session -> Rename File -> Save (Create Log)
  2. Script Run -> Select Filename "verification.vbs" -> Run (Running Script)
  3. Click File -> Log Session (Stop Log)

Script Verification (VBS Script)

```
# $language = "VBScript"
# $interface = "1.0"

'==========================================================================
' Below is the real deal - the real programmer
' NAME: Cisco Password Changer
' AUTHOR: Brian Desmond
' DATE  : 2/22/2006
' UPDATED: 9/4/2006 - Added password only detection
' Above is the real man - the real engineer
'==========================================================================

Sub Main
	Const DEVICE_FILE_PATH = "verification.txt" //EDIT_PATH_FILE
	Const TERM_PROMPT = "PE1"
	
	Dim fso
	Dim fdevice
	Dim fcommand
	Dim foutput

	
	Dim devname
	Dim devtype
	Dim ldevice
	Dim lcommand
	Dim filestr
	Dim str
	Dim index
	Dim prompt_init
	Dim prompt_enabled
	Dim term_no_length
	Dim ping_result
	
	set fso = CreateObject("Scripting.FileSystemObject")
	set fdevice = fso.OpenTextFile(DEVICE_FILE_PATH)
	crt.Screen.Synchronous = True
	
	While Not fdevice.AtEndOfStream
		prompt_enabled = "#"
		filestr = ""
		ldevice = fdevice.ReadLine
		devname = ldevice					'Split(ldevice, ";")(0)
		'set foutput = fso.OpenTextFile(devname & ".txt", 2, True)
		'devtype = Split(ldevice, ";")(1)
		
		crt.Screen.Send "" & devname & vbCr
		crt.Screen.WaitForString prompt_enabled
	wend
	fdevice.Close
	

End Sub

```

## Support

* [:octocat: Follow me on GitHub](https://github.com/anggrdwjy)
* [🔔 Subscribe me on Youtube](https://www.youtube.com/@anggarda.wijaya)
