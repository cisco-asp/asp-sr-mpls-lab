
# Introduction to SR-MPLS Lab
This lab is meant for use with the ASP Containerlab enviornment in Cisco's dcloud.  Please refer to the following (https://github.com/mfierbaugh/asp-containerlab)

## Lab Objectives

  * Understanding basic Segment Routing configuration
  * How to verify and troubleshoot the control plane
  * How to verify and troubleshoot the data plane
  * Become familiar with SR OAM as a tool for troubleshooting
  * Overview of Topology Independent Loop Free Alternate \(TI\-LFA\)
  * How to verify primary and backup paths of TI\-LFA
  * How to migrate to SR\-MPLS from LDP

## Features 
Features in this playbook:
 - Segment Routing using ISIS
 - Transport Independant Loop Free Alternate (TI-LFA)
 - Segment Routing Performance Measurement (SR-PM)
 - Flexable Algorithm

## Devices 
- Cisco XRd running IOS-XR 25.2.1 (PE and CE)
- Cisco 8212-48FH-M running IOS-XR 25.2.1 (CORE.101, CORE.102)
- Cisco 8711-32FH-M running IOS-XR 25.2.1 (CORE.103, CORE.104)
- Cisco T-Rex ver 3.06 (Trex-1, Trex-2)

Configurations are working examples of Cisco Converged SDN Transport (https://xrdocs.io/design/blogs/latest-converged-sdn-transport-ig)

## Lab Topology
![Topology](docs/images/topology.png)

## Usage
This project is a full CI/CD pipline for configuration management. All configuration changes should be made by logging an issue then merging the changes to the Development branch then Test branch.

WARNING - this is a full commit replace and will restore the Converged SDN Transport configuration.

```
./config_lab
```

## Lab Guide
The lab guide can be found under the docs folder here [LINK](docs/Intro_to_SR_MPLS.md)

## Start T-REX 
Once the lab configuration has been committed (config_lab), open a terminal and ssh to each of the trex nodes (trex-1 and trex-2)

Start the interactive trex console which will make a local connection to the running interactive daemon
```
./trex-console
```

Start generating traffic using the built-in imix profile
```
start -f stl/imix.py
```

To interact with and view statistics for the current stream launch the text-based user interface (tui)
```
tui
```

Attempt to increase per interface traffic rate to 100mbps (200mbps rx/tx total). Throughput achievable in the Docker environment is dependent primarily on single core\thread CPU performance.

```
update -m 100mbps
```

## Run playbooks directly

To run the playbook without committing changes:

    ansible-playbook -i hosts.yml lab_master_playbook.yml -e "commit_changes=0"

To run the playbook committing the changes:

    ansible-playbook -i hosts.yml lab_master_playbook.yml -e "commit_changes=1"

