# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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