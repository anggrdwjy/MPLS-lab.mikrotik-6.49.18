# Overview Lab

This MPLS lab runs on Proxmox and PNETLAB with specifications of 4Core CPU, 16GB RAM and 100GB Disk. It uses MikroTik CHRx86 6.49.18 running on PNETLAB.

## Topology Target

Topology Lab MPLS

<p align="center">
<img src="Topology/Topology-MPLS-MikroTik-v2.png">
</p>

## OSPFv2

OSPF (Open Shortest Path First) is a link-state routing protocol that allows routers to build a full map of the network topology to determine the fastest paths. It operates by discovering neighbors via Hello packets, exchanging Link-State Advertisements (LSAs) to synchronize a Link-State Database (LSDB), and running Dijkstra's Shortest Path First (SPF) algorithm.

This MPLS lab uses OSPFv2 routing

<p align="center">
<img src="OSPFv2/Topology OSPF Routing.png">
</p>

Verification OSPF Example

```
routing ospf export
routing ospf area print
routing ospf network print
routing ospf interface print 
routing ospf neighbor print
routing ospf route print
```

Summary Verification OSPF Example 

```
[admin@PE1] > interface print
Flags: D - dynamic, X - disabled, R - running, S - slave 
 #     NAME                                TYPE       ACTUAL-MTU L2MTU  MAX-L2MTU MAC-ADDRESS      
 0  R  ;;; PE5
       ether1                              ether            9000                  50:C8:14:00:1C:00
 1  R  ;;; PE5
       ether2                              ether            9000                  50:C8:14:00:1C:01
 2  R  ;;; PE2
       ether3                              ether            9000                  50:C8:14:00:1C:02
 3  R  ;;; PE2
       ether4                              ether            9000                  50:C8:14:00:1C:03
 4  R  ;;; CE12
       ether5                              ether            9000                  50:C8:14:00:1C:04
 5  R  ether6                              ether            1500                  50:C8:14:00:1C:05
 6  R  ether7                              ether            1500                  50:C8:14:00:1C:06
 7  R  ether8                              ether            1500                  50:C8:14:00:1C:07
 8  R  lo0                                 bridge           1500 65535            DA:31:C1:D6:B3:70
[admin@PE1] > ip address print
Flags: X - disabled, I - invalid, D - dynamic 
 #   ADDRESS            NETWORK         INTERFACE                                                                                                                                                                                     
 0   ;;; PE1
     192.168.150.1/32   192.168.150.1   lo0                                                                                                                                                                                           
 1   ;;; PE5
     172.16.20.53/30    172.16.20.52    ether1                                                                                                                                                                                        
 2   ;;; PE5
     172.16.20.49/30    172.16.20.48    ether2                                                                                                                                                                                        
 3   ;;; PE2
     172.16.20.1/30     172.16.20.0     ether3                                                                                                                                                                                        
 4   ;;; PE2
     172.16.20.5/30     172.16.20.4     ether4                                                                                                                                                                                        
 5   ;;; CE12
     172.16.20.149/30   172.16.20.148   ether5                                                                                                                                                                                        
[admin@PE1] > ip neighbor print
 # INTERFACE ADDRESS                                                                                                   MAC-ADDRESS       IDENTITY   VERSION    BOARD                                                                  
 0 ether1    172.16.20.54                                                                                              50:6A:3B:00:20:00 PE5        6.49.19... CHR                                                                    
 1 ether2    172.16.20.50                                                                                              50:6A:3B:00:20:01 PE5        6.49.19... CHR                                                                    
 2 ether3    172.16.20.2                                                                                               50:16:72:00:1D:02 PE2        6.49.19... CHR                                                                    
 3 ether4    172.16.20.6                                                                                               50:16:72:00:1D:03 PE2        6.49.19... CHR                                                                    
 4 ether5    172.16.20.150                                                                                             50:65:2D:00:2F:00 CE12       6.49.19... CHR                                                                    
[admin@PE1] > routing ospf network print
Flags: X - disabled, I - invalid 
 #   NETWORK            AREA                                                                                                                                                                                                          
 0   192.168.150.1/32   backbone                                                                                                                                                                                                      
 1   172.16.20.48/30    backbone                                                                                                                                                                                                      
 2   172.16.20.52/30    backbone                                                                                                                                                                                                      
 3   172.16.20.0/30     backbone                                                                                                                                                                                                      
 4   172.16.20.148/30   area60                                                                                                                                                                                                        
 5   172.16.20.4/30     backbone                                                                                                                                                                                                      
[admin@PE1] > routing ospf interface print 
Flags: X - disabled, I - inactive, D - dynamic, P - passive 
 #    INTERFACE                                                                                                                                                         COST PRIORITY NETWORK-TYPE   AUTHENTICATION AUTHENTICATION-KEY
 0  P lo0                                                                                                                                                              65000        1 point-to-point none                             
 1    ether1                                                                                                                                                               1        1 point-to-point md5            multimedia123     
 2    ether2                                                                                                                                                               1        1 point-to-point md5            multimedia123     
 3    ether3                                                                                                                                                               1        1 point-to-point md5            multimedia123     
 4    ether4                                                                                                                                                               1        1 point-to-point md5            multimedia123     
 5    ether5                                                                                                                                                               1        1 point-to-point md5            multimedia123     
[admin@PE1] > routing ospf neighbor print
 0 instance=ospf100 router-id=192.168.10.13 address=172.16.20.150 interface=ether5 priority=1 dr-address=0.0.0.0 backup-dr-address=0.0.0.0 state="Full" state-changes=4 ls-retransmits=0 ls-requests=0 db-summaries=0 
   adjacency=4h37m41s 

 1 instance=ospf100 router-id=192.168.150.2 address=172.16.20.6 interface=ether4 priority=1 dr-address=0.0.0.0 backup-dr-address=0.0.0.0 state="Full" state-changes=4 ls-retransmits=0 ls-requests=0 db-summaries=0 
   adjacency=19h22m15s 

 2 instance=ospf100 router-id=192.168.150.2 address=172.16.20.2 interface=ether3 priority=1 dr-address=0.0.0.0 backup-dr-address=0.0.0.0 state="Full" state-changes=5 ls-retransmits=0 ls-requests=0 db-summaries=0 
   adjacency=19h30m41s 

 3 instance=ospf100 router-id=192.168.150.8 address=172.16.20.54 interface=ether1 priority=1 dr-address=0.0.0.0 backup-dr-address=0.0.0.0 state="Full" state-changes=5 ls-retransmits=0 ls-requests=0 db-summaries=0 
   adjacency=20h38m21s 

 4 instance=ospf100 router-id=192.168.150.8 address=172.16.20.50 interface=ether2 priority=1 dr-address=0.0.0.0 backup-dr-address=0.0.0.0 state="Full" state-changes=5 ls-retransmits=0 ls-requests=0 db-summaries=0 
   adjacency=20h38m21s 
[admin@PE1] > routing ospf route print
 # DST-ADDRESS        STATE          COST                                                                                    GATEWAY         INTERFACE                                                                                
 0 172.16.20.0/30     intra-area     1                                                                                       0.0.0.0         ether3                                                                                   
 1 172.16.20.4/30     intra-area     1                                                                                       0.0.0.0         ether4                                                                                   
 2 172.16.20.8/30     intra-area     2                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
 3 172.16.20.12/30    intra-area     2                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
 4 172.16.20.16/30    intra-area     3                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
 5 172.16.20.20/30    intra-area     3                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
 6 172.16.20.24/30    intra-area     2                                                                                       172.16.20.50    ether2                                                                                   
                                                                                                                             172.16.20.54    ether1                                                                                   
 7 172.16.20.28/30    intra-area     2                                                                                       172.16.20.50    ether2                                                                                   
                                                                                                                             172.16.20.54    ether1                                                                                   
 8 172.16.20.32/30    intra-area     3                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
                                                                                                                             172.16.20.50    ether2                                                                                   
                                                                                                                             172.16.20.54    ether1                                                                                   
 9 172.16.20.36/30    intra-area     3                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
                                                                                                                             172.16.20.50    ether2                                                                                   
                                                                                                                             172.16.20.54    ether1                                                                                   
10 172.16.20.40/30    intra-area     4                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
                                                                                                                             172.16.20.50    ether2                                                                                   
                                                                                                                             172.16.20.54    ether1                                                                                   
11 172.16.20.44/30    intra-area     4                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
                                                                                                                             172.16.20.50    ether2                                                                                   
                                                                                                                             172.16.20.54    ether1                                                                                   
12 172.16.20.48/30    intra-area     1                                                                                       0.0.0.0         ether2                                                                                   
13 172.16.20.52/30    intra-area     1                                                                                       0.0.0.0         ether1                                                                                   
14 172.16.20.56/30    intra-area     2                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
15 172.16.20.60/30    intra-area     2                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
16 172.16.20.64/30    intra-area     3                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
17 172.16.20.68/30    intra-area     3                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
18 172.16.20.72/30    intra-area     4                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
19 172.16.20.76/30    intra-area     4                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
20 172.16.20.80/30    inter-area     2                                                                                       172.16.20.50    ether2                                                                                   
                                                                                                                             172.16.20.54    ether1                                                                                   
21 172.16.20.84/30    inter-area     3                                                                                       172.16.20.50    ether2                                                                                   
                                                                                                                             172.16.20.54    ether1                                                                                   
22 172.16.20.88/30    inter-area     3                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
                                                                                                                             172.16.20.50    ether2                                                                                   
                                                                                                                             172.16.20.54    ether1                                                                                   
23 172.16.20.92/30    inter-area     3                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
                                                                                                                             172.16.20.50    ether2                                                                                   
                                                                                                                             172.16.20.54    ether1                                                                                   
24 172.16.20.96/30    inter-area     4                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
                                                                                                                             172.16.20.50    ether2                                                                                   
                                                                                                                             172.16.20.54    ether1                                                                                   
25 172.16.20.100/30   inter-area     4                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
                                                                                                                             172.16.20.50    ether2                                                                                   
                                                                                                                             172.16.20.54    ether1                                                                                   
26 172.16.20.104/30   inter-area     4                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
                                                                                                                             172.16.20.50    ether2                                                                                   
                                                                                                                             172.16.20.54    ether1                                                                                   
27 172.16.20.108/30   inter-area     5                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
                                                                                                                             172.16.20.50    ether2                                                                                   
                                                                                                                             172.16.20.54    ether1                                                                                   
28 172.16.20.112/30   inter-area     5                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
                                                                                                                             172.16.20.50    ether2                                                                                   
                                                                                                                             172.16.20.54    ether1                                                                                   
29 172.16.20.116/30   inter-area     4                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
30 172.16.20.120/30   inter-area     5                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
31 172.16.20.124/30   inter-area     3                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
32 172.16.20.128/30   inter-area     3                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
33 172.16.20.132/30   inter-area     3                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
34 172.16.20.136/30   inter-area     2                                                                                       172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
35 172.16.20.140/30   intra-area     3                                                                                       172.16.20.150   ether5                                                                                   
36 172.16.20.144/30   intra-area     2                                                                                       172.16.20.150   ether5                                                                                   
37 172.16.20.148/30   intra-area     1                                                                                       0.0.0.0         ether5                                                                                   
38 192.168.10.2/32    inter-area     65002                                                                                   172.16.20.50    ether2                                                                                   
                                                                                                                             172.16.20.54    ether1                                                                                   
39 192.168.10.3/32    inter-area     65003                                                                                   172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
                                                                                                                             172.16.20.50    ether2                                                                                   
                                                                                                                             172.16.20.54    ether1                                                                                   
40 192.168.10.4/32    inter-area     65003                                                                                   172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
                                                                                                                             172.16.20.50    ether2                                                                                   
                                                                                                                             172.16.20.54    ether1                                                                                   
41 192.168.10.5/32    inter-area     65004                                                                                   172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
                                                                                                                             172.16.20.50    ether2                                                                                   
                                                                                                                             172.16.20.54    ether1                                                                                   
42 192.168.10.6/32    inter-area     65004                                                                                   172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
                                                                                                                             172.16.20.50    ether2                                                                                   
                                                                                                                             172.16.20.54    ether1                                                                                   
43 192.168.10.7/32    inter-area     65005                                                                                   172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
                                                                                                                             172.16.20.50    ether2                                                                                   
                                                                                                                             172.16.20.54    ether1                                                                                   
44 192.168.10.8/32    inter-area     65004                                                                                   172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
45 192.168.10.9/32    inter-area     65003                                                                                   172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
46 192.168.10.10/32   inter-area     65003                                                                                   172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
47 192.168.10.11/32   inter-area     65002                                                                                   172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
48 192.168.10.12/32   intra-area     65002                                                                                   172.16.20.150   ether5                                                                                   
49 192.168.10.13/32   intra-area     65001                                                                                   172.16.20.150   ether5                                                                                   
50 192.168.150.1/32   intra-area     65000                                                                                   0.0.0.0         lo0                                                                                      
51 192.168.150.2/32   intra-area     65001                                                                                   172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
52 192.168.150.3/32   intra-area     65002                                                                                   172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
53 192.168.150.4/32   intra-area     65003                                                                                   172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
54 192.168.150.5/32   intra-area     65004                                                                                   172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
                                                                                                                             172.16.20.50    ether2                                                                                   
                                                                                                                             172.16.20.54    ether1                                                                                   
55 192.168.150.6/32   intra-area     65003                                                                                   172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
                                                                                                                             172.16.20.50    ether2                                                                                   
                                                                                                                             172.16.20.54    ether1                                                                                   
56 192.168.150.7/32   intra-area     65002                                                                                   172.16.20.2     ether3                                                                                   
                                                                                                                             172.16.20.6     ether4                                                                                   
                                                                                                                             172.16.20.50    ether2                                                                                   
                                                                                                                             172.16.20.54    ether1                                                                                   
57 192.168.150.8/32   intra-area     65001                                                                                   172.16.20.50    ether2                                                                                   
                                                                                                                             172.16.20.54    ether1                                                                                   

```

## MPLS LDP

Multi-Protocol Label Switching (MPLS) is an advanced packet-forwarding technique used in modern networks. Instead of making routers look into complex Layer 3 routing tables for every IP packet, MPLS uses labels for forwarding decisions. These labels create pre-defined, efficient paths across the network, which enhances speed, scalability and traffic management.

LDP is a protocol that automatically generates and exchanges labels between routers. Each router will locally generate labels for its prefixes and will then advertise the label values to its neighbors.

<p align="center">
<img src="MPLS-LDP/Topology MPLS LDP Signaling.png">
</p>

Verification MPLS LDP Example

```
mpls export
mpls interface print
mpls ldp interface print
mpls ldp neighbor print
mpls forwarding-table print
```

Summary Verification MPLS LDP Example 

```
[admin@PE1] > mpls interface print
Flags: X - disabled, * - default 
 #    INTERFACE                                                        MPLS-MTU
 0  * all                                                                  9000
[admin@PE1] > mpls ldp interface print
Flags: X - disabled, I - invalid 
 #   INTERFACE                        HELLO-INTERVAL       HOLD-TIME           
 0   ether1                           5s                   15s                 
 1   ether2                           5s                   15s                 
 2   ether3                           5s                   15s                 
 3   ether4                           5s                   15s                 
 4   ether5                           5s                   15s                 
[admin@PE1] > mpls ldp neighbor print
Flags: X - disabled, D - dynamic, O - operational, T - sending-targeted-hello, 
V - vpls 
 #      TRANSPORT       LOCAL-TRANSPORT PEER                       SEN
 0  OT  192.168.10.13   192.168.150.1   192.168.10.13:0            yes
 1  OT  192.168.150.8   192.168.150.1   192.168.150.8:0            yes
 2  OT  192.168.150.2   192.168.150.1   192.168.150.2:0            yes
[admin@PE1] > mpls forwarding-table print
Flags: H - hw-offload, L - ldp, V - vpls, T - traffic-eng 
 #    IN-LABEL           OUT-LABELS       DESTINATION                    INT...
 0    expl-null         
 1  L 16                                  192.168.150.8/32               ether2
 2  L 17                                  172.16.20.56/30                ether3
 3  L 18                 41               192.168.10.2/32                ether2
 4  L 19                 22               192.168.150.7/32               ether2
 5  L 20                 46               172.16.20.32/30                ether2
 6  L 21                 46               172.16.20.72/30                ether3
 7  L 22                 45               192.168.150.3/32               ether3
 8  L 23                 38               192.168.150.6/32               ether2
 9  L 24                 24               192.168.10.12/32               ether5
10  L 25                 40               172.16.20.112/30               ether2
11  L 26                                  172.16.20.28/30                ether2
12  L 27                 17               172.16.20.20/30                ether3
13  L 28                 29               172.16.20.64/30                ether3
14  L 29                 31               192.168.150.4/32               ether3
15  L 30                 58               192.168.10.3/32                ether2
16  L 31                                  172.16.20.144/30               ether5
17  L 32                 20               172.16.20.16/30                ether3
18  L 33                                  172.16.20.12/30                ether3
19  L 34                                  172.16.20.80/30                ether2
20  L 35                 58               172.16.20.116/30               ether3
21  L 36                 52               172.16.20.36/30                ether2
22  L 37                 34               172.16.20.40/30                ether2
23  L 38                 49               172.16.20.100/30               ether2
24  L 39                 51               192.168.150.5/32               ether2
25  L 40                 21               192.168.10.11/32               ether3
26  L 41                 37               172.16.20.132/30               ether3
27  L 42                 43               192.168.10.8/32                ether3
28  L 43                 60               192.168.10.5/32                ether2
29  L 44                 66               172.16.20.44/30                ether2
30  L 45                 33               192.168.10.7/32                ether2
31  L 46                 40               192.168.10.10/32               ether3
32  L 47                 59               192.168.10.4/32                ether2
33  L 48                 42               172.16.20.96/30                ether2
34  L 49                 26               172.16.20.124/30               ether3
35  L 50                 33               172.16.20.128/30               ether3
36  L 51                 22               172.16.20.140/30               ether5
37  L 52                 18               192.168.10.6/32                ether2
38  L 53                                  172.16.20.60/30                ether3
39  L 54                 39               172.16.20.76/30                ether3
40  L 55                 65               172.16.20.92/30                ether2
41  L 56                 62               172.16.20.108/30               ether2
42  L 57                 51               172.16.20.120/30               ether3
43  L 58                                  172.16.20.136/30               ether3
44  L 59                                  192.168.150.2/32               ether3
45  L 60                                  192.168.10.13/32               ether5
46  L 61                 54               172.16.20.68/30                ether3
47  L 62                 57               172.16.20.88/30                ether2
48  L 63                 20               172.16.20.104/30               ether2
49  L 64                                  172.16.20.8/30                 ether3
50  L 65                                  172.16.20.24/30                ether2
51  L 66                 44               192.168.10.9/32                ether3
52  L 67                 32               172.16.20.84/30                ether2

[admin@PE1] > 
```

## L2VPN Virtual Private LAN Services

VPLS is an Ethernet-based point-to-multipoint Layer 2 VPN. It allows you to connect geographically dispersed Ethernet local area networks (LAN) sites to each other across an MPLS backbone. For customers who implement VPLS, all sites appear to be in the same Ethernet LAN even though traffic travels across the service provider's network.

VPLS, in its implementation and configuration, has much in common with a Layer 2 VPN. In VPLS, a packet originating within a service provider customer’s network is sent first to a customer edge (CE) device (for example, a router or Ethernet switch). It is then sent to a provider edge (PE) router within the service provider’s network. The packet traverses the service provider’s network over a MPLS label-switched path (LSP). It arrives at the egress PE router, which then forwards the traffic to the CE device at the destination customer site.

<p align="center">
<img src="L2VPN-VPLS/Topology VPLS CE1-CE7.png">
</p>

Verification VPLS Example

```
interface vpls export
interface vpls print
interface bridge port export
interface bridge print
interface bridge host print
mpls ldp neighbor print brief
```

Summary Verification L2VPN VPLS Example 

```
[admin@CE7] > interface bridge print
Flags: X - disabled, R - running 
 0 R name="l2c-2024" mtu=1998 actual-mtu=1500 l2mtu=1500 arp=enabled arp-timeout=auto 
     mac-address=50:02:81:00:2A:02 protocol-mode=rstp fast-forward=yes igmp-snooping=no auto-mac=yes 
     ageing-time=5m priority=0x8000 max-message-age=20s forward-delay=15s transmit-hold-count=6 
     vlan-filtering=no dhcp-snooping=no 

 1 R name="lo0" mtu=auto actual-mtu=1500 l2mtu=65535 arp=enabled arp-timeout=auto 
     mac-address=C6:5E:88:CF:FC:D7 protocol-mode=rstp fast-forward=yes igmp-snooping=no auto-mac=yes 
     ageing-time=5m priority=0x8000 max-message-age=20s forward-delay=15s transmit-hold-count=6 
     vlan-filtering=no dhcp-snooping=no 
[admin@CE7] > interface bridge host print
Flags: X - disabled, I - invalid, D - dynamic, L - local, E - external 
 #       MAC-ADDRESS        VID ON-INTERFACE                 BRIDGE                 AGE                 
 0   DL  C6:5E:88:CF:FC:D7      lo0                          lo0                   
 1   D   00:50:79:66:68:35      ether3                       l2c-2024               2m26s               
 2   D   00:50:79:66:68:36      vpls-l2c-2024                l2c-2024               2m26s               
 3   D   02:4C:08:CB:1F:B3      vpls-l2c-2024                l2c-2024               25s                 
 4   DL  02:BF:8C:E7:0C:1B      vpls-l2c-2024                l2c-2024              
 5   DL  50:02:81:00:2A:02      ether3                       l2c-2024              
[admin@CE7] > mpls ldp neighbor print brief
Flags: X - disabled, D - dynamic, O - operational, T - sending-targeted-hello, V - vpls 
 #      TRANSPORT       LOCAL-TRANSPORT PEER                       SEND-TARGETED ADDRESSES      
 0  OT  192.168.10.9    192.168.10.8    192.168.10.9:0             yes           172.16.20.121  
                                                                                 172.16.20.126  
                                                                                 192.168.10.9   
 1  OT  192.168.150.4   192.168.10.8    192.168.150.4:0            yes           172.16.20.18   
                                                                                 172.16.20.22   
                                                                                 172.16.20.73   
                                                                                 172.16.20.77   
                                                                                 172.16.20.117  
                                                                                 192.168.150.4  
 2 DOTV 192.168.10.2    192.168.10.8    192.168.10.2:0             yes           172.16.20.82   
                                                                                 172.16.20.85   
                                                                                 192.168.10.2   
[admin@CE7] > 
[admin@CE7] > 
```

## IBGP Router Reflector Concept

A BGP Route Reflector (RR) reduces iBGP full-mesh requirements by acting as a central hub that "reflects" routes between client routers within an Autonomous System (AS). It breaks the split-horizon rule (iBGP-to-iBGP), allowing clients to peer only with the RR, simplifying configuration and reducing CPU/network overhead.

Router Reflector (RR1)

<p align="center">
<img src="IBGP-RouteReflector/Topology Route-Reflector RR1.png">
</p>

Router Reflector (RR2)

<p align="center">
<img src="IBGP-RouteReflector/Topology Route-Reflector RR2.png">
</p>

Verification IBGP Example

```
routing bgp export
routing bgp instance print
routing bgp peer print
```

Summary Verification IBGP PE1 Example 

```
[admin@PE1] > routing bgp instance print
Flags: * - default, X - disabled 
 0 *  name="default" as=65000 router-id=192.168.150.1 redistribute-connected=no 
      redistribute-static=no redistribute-rip=no redistribute-ospf=no 
      redistribute-other-bgp=no out-filter="" cluster-id=192.168.150.1 
      client-to-client-reflection=yes ignore-as-path-len=no routing-table="" 
[admin@PE1] > routing bgp peer print
Flags: X - disabled, E - established 
 #   INSTANCE              REMOTE-ADDRESS                                       REMOTE-AS  
 0 E default               192.168.254.1                                        65000      
 1 E default               192.168.254.2                                        65000    
```

Summary Verification IBGP RR1 Example 

```
[admin@RR1] > routing bgp instance print
Flags: * - default, X - disabled 
 0 *  name="default" as=65000 router-id=192.168.254.1 redistribute-connected=no redistribute-static=no redistribute-rip=no redistribute-ospf=no 
      redistribute-other-bgp=no out-filter="" cluster-id=192.168.254.1 client-to-client-reflection=yes ignore-as-path-len=no routing-table="" 
[admin@RR1] > routing bgp peer print
Flags: X - disabled, E - established 
 #   INSTANCE                                           REMOTE-ADDRESS                                                                     REMOTE-AS  
 0 E default                                            192.168.150.1                                                                      65000      
 1   default                                            192.168.150.2                                                                      65000      
 2 E default                                            192.168.150.3                                                                      65000      
 3 E default                                            192.168.150.4                                                                      65000      
 4 E default                                            192.168.150.5                                                                      65000      
 5 E default                                            192.168.150.6                                                                      65000      
 6 E default                                            192.168.150.7                                                                      65000      
 7 E default                                            192.168.150.8                                                                      65000      
 8 E default                                            192.168.10.2                                                                       65000      
 9 E default                                            192.168.10.3                                                                       65000      
10 E default                                            192.168.10.4                                                                       65000      
11 E default                                            192.168.10.5                                                                       65000      
12 E default                                            192.168.10.6                                                                       65000      
13 E default                                            192.168.10.7                                                                       65000      
14 E default                                            192.168.10.8                                                                       65000      
15 E default                                            192.168.10.9                                                                       65000      
16 E default                                            192.168.10.10                                                                      65000      
17 E default                                            192.168.10.11                                                                      65000      
18 E default                                            192.168.10.12                                                                      65000      
19 E default                                            192.168.10.13                                                                      65000      
```

## L3VPN Virtual Routing Forwarding

L3VPN in BGP (specifically BGP/MPLS IP VPN) works by using Multiprotocol BGP (MP-BGP) to distribute customer routes between Provider Edge (PE) routers, while using MPLS to tunnel traffic across the backbone. PE routers use VRFs to maintain separate routing tables per customer, assign Route Distinguishers (RDs) to make routes unique, and use Route Targets (RTs) to control route import/export

<p align="center">
<img src="L3VPN-VRF/Topology VRF CE6-CE12.png">
</p>

Verification L3VPN VRF Example

```
ip address print
interface bridge port export
interface bridge print
ip route vrf export
ip route vrf print brief
routing bgp vpnv4-route print
```
Summary Verification L3VPN VRF Example

```
[admin@CE12] > ip route vrf export
# nov/09/2025 08:22:58 by RouterOS 6.49.19
# software id = 
#
#
#
/ip route vrf
add export-route-targets=65000:2025 import-route-targets=65000:2025 \
    interfaces=l3vpn-2025 route-distinguisher=65000:2025 routing-mark=\
    l3vpn-2025
[admin@CE12] > ip route vrf print brief
Flags: X - disabled, I - inactive 
 #   ROUTING-... IN ROUTE-DISTINGUISHER          IMPORT-ROUTE-TARGETS        
 0   l3vpn-2025  l3 65000:2025                   65000:2025                  
[admin@CE12] > routing bgp vpnv4-route print
Flags: L - label-present 
 #   ROUTE-DISTINGUISHER            DST-ADDRESS        GATEWAY             IN..
 0 L 65000:2025                     10.200.0.0/30      192.168.10.7        et..
 1 L 65000:2025                     10.200.0.0/30      192.168.10.7        et..
 2 L 65000:2025                     10.100.0.0/30                          l3..
 ```

## Verification with Script

Example Verification (Traceroute)

<p align="center">
<img src="Verification/Traceroute Test-3.png">
</p>

Verification All Example

```
#Base
export
interface print
ip address print
ip neighbor print
ip route print

#OSPF
routing ospf export
routing ospf area print
routing ospf network print
routing ospf interface print 
routing ospf neighbor print
routing ospf route print

#MPLS-LDP
mpls export
mpls interface print
mpls ldp interface print
mpls ldp neighbor print
mpls forwarding-table print

#IBGP
routing bgp export
routing bgp instance print
routing bgp peer print

#VPLS
interface vpls export
interface vpls print
interface bridge port export
interface bridge print
interface bridge host print
mpls ldp neighbor print brief

#VRF
ip address print
interface bridge port export
interface bridge print
ip route vrf export
ip route vrf print
routing bgp vpnv4-route print
```

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


### Bugs

Please open an issue on GitHub with as much information as possible if you found a bug.
* Your Proxmox and PNETLAB Version
* All the logs and message outputted
* etc
