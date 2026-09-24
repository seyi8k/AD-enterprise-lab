# active-directory-enterprise-lab

```mermaid
flowchart TD
    Internet([Internet]) <--> Host[Windows 11 Host Machine]
    
    subgraph HyperV [Hyper-V Virtualization Layer]
        NAT[Hyper-V NAT Gateway\n10.10.10.1]
    end

    Host <--> NAT

    subgraph Subnet [Internal Virtual Network: 10.10.10.0/24]
        direction LR
        
        DC01[DC01: Domain Controller\nIP: 10.10.10.10\nOS: Server 2025\nRoles: AD DS, DNS\nDomain: corp.adlab.test]
        
        SRV01[SRV01: Member Server\nIP: 10.10.10.20\nOS: Server 2025\nRoles: DHCP, File Server]
        
        CLIENT01[CLIENT01: Workstation\nIP: DHCP 10.10.10.100+\nOS: Windows 11 Enterprise\nStatus: Domain Joined]
    end

    NAT <--> DC01
    NAT <--> SRV01
    NAT <--> CLIENT01
    
    CLIENT01 -- Authentication / DNS --> DC01
    CLIENT01 -- File Shares / DHCP --> SRV01