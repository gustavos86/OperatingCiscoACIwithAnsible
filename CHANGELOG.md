# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [v0.28] - 2021-06-12

### Added

- Tag to run all Fabric > Access Policies related roles
- Tag to run all Tenants related roles
- monitoring_policy to Port Groups

### Changed

- better validation for Port Group access ports to ignore port_channel_policy and avoid an error

## [v0.27] - 2021-06-11

### Added

- Some more documentation to the vars yaml file in host_vars

### Changed

- migrated role: bdtol3out to now be part of role: BridgeDomains_and_Subnets
- migrated role: vzAny     to now be part of role: Tenants_and_VRFs

## [v0.26] - 2021-06-10

### Added

- some comments to the main playbook

### Changed

- Default parameters for arp_flooding and l2_unknown_unicast for Bridge Domains
- vzAny tasks omits when vzAny parameter is not defined
- reviewed vzAny behavior. vzAny set to 'no' removes association at VRF level but keeps Contracts and Filters, etc

## [v0.25] - 2021-06-10

### Added

- Parameter to delete a VRF
- some comments to the main playbook

### Changed

- now BD unicast routing defaults to 'no'

## [v0.24] - 2021-06-09

### Changed

- interface_type: switch_port or port_channel as required parameter in Storage static binding
- cisco.aci.aci_bd_subnet scope: "{{ item.scope | default(omit) }}"
- BD type fc is deployed with Unicast Routing set to disabled
- cisco.aci.aci_epg bd: "{{ item.BD_name | default(omit) }}"
- default(omit) for different parameters for tasks in role: Domains_and_EPG_Static_Bindings
- cisco.aci.aci_static_binding_to_epg interface_mode jinja2 slightly modified
- host_vars example some small re-arrangements

## [v0.23] - 2021-06-08

### Added

- fibre_channel_interface_policy parameter to Policy Group
- priority_flow_control_policy paremeter to Policy Group

### Changed

- Made optional not required parameters in BD task
- Made optional BD parameter in EPG task

## [v0.22] - 2021-06-08

### Added

- In Interface_Policies, added L2 Interface Policy
- l2_interface_policy parameters PolicyGroups

### Changed

- Spanning Tree Interface policies status parameter
- Ommited missing parameters in PolicyGroups

## [v0.21] - 2021-06-08

### Added

- In L3Out option to configure BGP Local ASN
- In L3Out option to enable BFD in BGP

## [v0.20] - 2021-06-07

### Added

- bd_state: absent as an optional parameter to delete the Bridge Domain
- epg_state: absent as an optional parameter to delete the EPG
- role: Storage_Domains_and_EPG_Static_Bindings with tag: storagestaticbindings to attach Storage Domain to EPG and configure Storage Static Bindings in the EPG

### Changed

- Static Route optional paremeter to delete the static route is state: 'absent' instead of status: 'deleted' to be aligned with all other similar used variables 

## [v0.19] - 2021-06-07

### Added

- ExtEPG custom name in L3Out

## [v0.18] - 2021-06-05

### Added

- tags to all roles defined in aci_playbook.yaml. Use the below command to see the list of tags available
```
$ ansible-playbook aci_playbook.yaml -i inventory --list-tags

playbook: aci_playbook.yaml

  play #1 (apicsim_sandbox_example): ACI playbook       TAGS: []
      TASK TAGS: [aaeps, bds, bdtol3out, contracts, domains, epgs, intpolicies, intpolicygroups, intselectors, l3outs, shutdownports, staticbindings, switchprofiles, tenants, vlanpools, vzany]
```

### Removed

- L3Out_PortChannel_SubIntf_OSPF role

### Note to myself

Tested 'async: 60 poll: 0' in tasks but found that sometimes changes do not happen in APIC.

## [v0.17] - 2021-06-04

### Added

- L3Outs role for unified L3Out configuration. Supports some parameters for BGP and OSPF. Supports 'Routed sub-interfaces' and SVIs. Supports single-ports, port-channels and vPCs.

### Removed

- L3Out_vPC_SVI role
- L3Out_Port_SVI_BGP role

## [v0.16] - 2021-06-02

### Added

- ignore_errors: yes on all tasks
- optional parameter to remove entry in static binding
- optional parameter to remove entry in interface selector
- option for access port 801.p in static binding with parameter name "native_vlan"
- parameter 'skip_missing: True' to enable_or_disable_ports

### Changed

- Remoted "port_profile_dummy" workaround in Static Bindings section

## [v0.15] - 2021-06-01

### Added

- L3Out_Port_SVI_BGP parameter to set the BGP password

## [v0.14] - 2021-05-27

### Added

- EPG_to_EPG_Contracts

### Changed

- Removed "- continue" workaround in Switches_and_Interfaces_map:

## [v0.13] - 2021-05-26

### Added

- Added the global variable loop_control_pause: 1
  It has been added to include a delay between loop iterations. Required when the APIC cannot handle the rate of all the connections

## [v0.12] - 2021-05-25

### Added

- VLAN pool description as optional parameters

## [v0.11] - 2021-05-25

### Added

- comments on file in host_vars
- vars in L3Out_PortChannel_SubIntf_OSPF to control OSPF parameters OSPF_Area_ID and OSPF_Area_Type

## [v0.10] - 2021-05-24

### Added

- Associate_BridgeDomains_to_L3Out
- L3Out_PortChannel_SubIntf_OSPF
- L3Out_Port_SVI_BGP
- validate_certs: no to all Ansible tasks

### Changed

- Bridge Domain Subnets - "bd_subnet" and "bd_subnet_mask" parameters are optional parameters
- Interface_Policy_Group_task_config - "cdp_policy", "lldp_policy", "mcp_policy" and "link_level_policy" are optional parameters
- Switch Leaf Profile > Leaf Selectors, the policy_group is set to default(omit)
- L3Outs renamed to L3Out_vPC_SVI
- In L3Out_vPC_SVI, the dictionary Static_Routes is optional
- In L3Out_vPC_SVI, the MTU is now a variable instead of being hardcoded
- In L3Out_vPC_SVI, added variable for rtrIdLoopBack


## [v0.9] - 2021-05-23

### Added

- vzAny parameter per VRF in Tenant



## [v0.8] - 2021-05-11

### Added

- Variable to control a suffix for switch profiles and interface profiles
- Variable to control if node ids should be included in switch profiles and interface profiles



## [v0.7] - 2021-05-11

### Changed

- L3Outs by modifying the task and information layout



## [v0.6] - 2021-05-11

### Added

- L3Outs



## [v0.5] - 2021-05-08

### Changed

- re-estructure Ansible to use Roles
- 'state:' is commented and in such case the default is 'present'



## [AccessPolicies.yaml v0.4] - 2021-05-06

### Added

- run_enable_or_disable_ports_task and port_state to enable or disable the state of ports



## [AccessPolicies.yaml v0.3] - 2021-05-05

### Changed
- stp_interface_policy is optional in cisco.aci.aci_interface_policy_leaf_policy_group

### Fixed

- fixed typo in a comment