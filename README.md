# active-directory-enterprise-lab

```mermaid
flowchart TD

    Internet([Internet])
    Router[Router<br/>10.0.0.1]
    Host[Windows 11 Pro Host<br/>Hyper-V]

    Internet <--> Router
    Router <--> Host

    subgraph HyperV["Hyper-V Virtualization Layer"]
        NAT["WinNAT<br/>Lab Network: 10.10.10.0/24"]
        Switch["ADLab-Switch<br/>Internal Virtual Switch<br/>Host Interface: 10.10.10.1"]

        NAT <--> Switch

        subgraph Lab["AD Lab"]

            DC01["DC01<br/>Windows Server 2025<br/>10.10.10.10<br/>AD DS + DNS<br/>corp.adlab.test"]

            SRV01["SRV01<br/>Windows Server 2025<br/>10.10.10.20<br/>DHCP + File Services"]

            CLIENT01["CLIENT01<br/>Windows 11 Enterprise<br/>DHCP: 10.10.10.100+<br/>Domain Joined"]
        end

        Switch <--> DC01
        Switch <--> SRV01
        Switch <--> CLIENT01
    end

    Host <--> NAT

    CLIENT01 -->|"Authentication / DNS"| DC01
    CLIENT01 -->|"SMB File Access"| SRV01
    CLIENT01 -.->|"DHCP Lease"| SRV01