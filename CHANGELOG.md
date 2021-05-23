# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).



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