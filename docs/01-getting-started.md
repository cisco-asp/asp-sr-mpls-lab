# ASP Containerlab
ASP Containerlab is a pre-built environment meant to be run on Cisco's dcloud infrastructure.  The dcloud VM can access both public docker repos and has read access to pull from a central repository of images stored on a private repo.  There is a collection of pre-built labs that leverage the VM's setup to reduce the amount of time to create/build/configure a working lab/demo. These labs are stored in GitHub and accessible via the dcloud VM. 

The ASP Containerlab environment consists of the following:
- A single large VM for running the containers using Containerlab (http://containerlab.dev)
    - The VM has been specifically configured for the additional resources needed by Cisco's XRd.
    - The local docker repo contains a few containers used by some pre-built labs, including Cisco's T-Rex Traffic Generator 
- Small/Medium VM for running a preconfigured telemetry stack (Telegraf/Influx/Grafana)
- A Windows VM if you wish to access the devices using a remote desktop client

## Management Network
Cisco's dcloud has a flat management network for which the virtual machines attach via the host VM.  
- All containers on the host have at least 1 management interface and those are bridged to the flat management network (198.18.128.0/18)
- Routers (containers or VMs wrapped in containers) have a dedicated management interface. 
- All router management interfaces are in a management VRF called Mgmt-intf
- Management IP addresses are directly accessible from either the VPN client or any host. 

The management network configuration allows for direct access of the containers via their management interface without any additional configuration or routing. This implies that you can access the containerized NOS directly via SSH, NETCONF, gRPC, etc. 

## Pre-built Demos and Labs
There are a few automated labs that are prebuilt and will get you quickly up and running with working configurations. They consist of the following:   
- clab yaml file that describes the lab topology
- Ansible playbook that builds the configuration for each device based upon device-level (hostvars) or global variables (global vars)
- Uses NAPALM to test or deploy the assembled configuration to the devices using NETCONF.
- The ansible playbook/napalm module does a full commit replace via NETCONF when run. 

## Getting Started
1. Start your dcloud instance

2. Use the Cisco Secure Client to VPN into the dcloud instance.  

3. Access the Containerlab VM via ssh or using the remote-ssh functionality in VS Code
```
ssh admin@198.18.130.5
```

4. Change directories to the desired lab

    example: 
    admin@clab-host:~$ cd asp_sr_mpls_lab/

```
cd asp_sr_mpls_lab/
```

5. Start the lab topology 
```
sudo clab deploy
```

## Refreshing the list of labs
When the ASP Containerlab environment boots, it automatically retrieves a list of labs using lab_controller.py.  If you need to refresh the labs you can simply re-run this script. 

The lab controller is a simple python script that ensures that you have both any new labs that have been created and the most up to date versions of those labs.
Each lab has it's own GitHub repository which contains at minimum the Containerlab topology file, management network configuration, node definitions, and versions of code

admin@clab-host:~$ ./lab_controller.py

```
./lab_controller.py
```

## Stopping the lab

```
sudo clab destroy
```

## Automated Lab Configuration 
Once the lab topology is fully running, you can build the configurations and commit them to each device using the ansible playbook. 

From the lab directory run:

```
./config_lab
```