# Operating Cisco ACI with Ansible

## Install

1. Install Python. I.e.

```
sudo apt install python3
```

2.  Install Ansible using apt or yum package manager

```
sudo apt install ansible
```

Ensure Ansible is installed

```
ansible --version
```

3. Install Ansible's ACI Collection

```
ansible-galaxy collection install cisco.aci
```

4. Ensure that the below Python libraries are installed

```
pip3 install lxml
pip3 install xmljson
```

Otherwise you will get the below error message (depending on which of the Python libraries is missing) at runtime when running the Ansible Playbook for some of the roles

```
"The lxml python library is missing, or lacks etree support."

$ pip3 install lxml

"The xmljson python library is missing, or lacks cobra support."

$ pip3 install xmljson
```

Note: If installing a python module with the **pip3 install** command gets stuck in Ubuntu, set the following environment variable in bash and try again:

```
export PYTHON_KEYRING_BACKEND=keyring.backends.null.Keyring
```

## Configure

By default, this Ansible playbook is pointing to [ACI Simulator AlwaysOn Sandbox](https://devnetsandbox.cisco.com/RM/Diagram/Index/5a229a7c-95d5-4cfd-a651-5ee9bc1b30e2?diagramType=Topology) which is free to use for testing purposes.

```
URL:      https://sandboxapicdc.cisco.com/
Username: admin
Password: ciscopsdt
```
[Cisco DevNet Sandbox labs](https://devnetsandbox.cisco.com/RM/Topology)

If you would like to use this Ansiible playbook to your own APIC controller, you should modify the following:

1. In aci_playbook.yaml file, modify the **hosts:** value with any name you wish

```
- name: ACI playbook
  hosts: my_apic_controller   <<< define your own relevant name here
```

2. In inventory file, add an entry with the value you specified for **hosts:** in the previious step and use the credentials to authenticate to your APIC controller

```
my_apic_controller ansible_host=url_or_IP_address username=yourusername password=yourpassword
```

3. Copy or replace host_vars/apicsim_sandbox_example.yaml to host_vars/my_apic_controller.yaml meaning using the name you chosed for **host:** instead.

## Run

1. In **host_vars/apicsim_sandbox_example.yaml** (or the file you have created in **host_vars**)you can follow the structure and replace it with your own and desired configurations.

2. In **aci_playbook.yaml** under **roles:** are located the configurations that Ansible will perform in ACI. For a certain group of tasks that you want to ommit, you can simple remove it or even better prepend the entry with a hash (#) to tell Ansible to ignore it.

```
  roles:
  ###############################
  #                             #
  #       Access Policies       #
  #                             #
  ###############################

  - { role: Interface_Policies,              tags: intpolicies     }    # create all policies like CDP, LLDP, Fibre Channel, MCP, Link Level, Port Channel, MCP, Layer 2 Interface, Spanning Tree Interface
  - { role: Switch_and_Interface_Profiles,   tags: switchprofiles  }    # create Switch Profiles and Interface Profiles (needed only when adding new switches to ACI)

  - { role: Interface_Selectors,             tags: intselectors    }    # provision ports defined in Switches_and_Interfaces_map: ports_list with a Policy Group
  - { role: Interface_Policy_Groups,         tags: intpolicygroups }    # create Interface Access Port, Port-Channels and vPCs
  - { role: enable_or_disable_ports,         tags: shutdownports   }    # enable or disable interfaces defined in Switches_and_Interfaces_map

  - { role: AAEPs,                           tags: aaeps           }    # create AAEPs and attach Domains to AAEPs
  - { role: Domains,                         tags: domains         }    # create Domains and attach VLAN pools to Domains
  - { role: VLAN_Pools,                      tags: vlanpools       }    # create VLAN Pools and its corresponding blocks of VLANs 

  ###############################
  #                             #
  #           Tenants           #
  #                             #
  ###############################

  - { role: Tenants_and_VRFs,                        tags: tenants               }   # create Tenants and VRFs. It can optionally deploy a complete vzAny configuration for a given VRF
  - { role: BridgeDomains_and_Subnets,               tags: bds                   }   # create Bridge Domains and Bridge Domain subnets
  - { role: AppProfiles_and_EPGs,                    tags: epgs                  }   # create Application Profiles and EPGs
  - { role: Domains_and_EPG_Static_Bindings,         tags: staticbindings        }   # attach Domain to EPGs and deploy the Static Bindings to the EPG
  - { role: Storage_Domains_and_EPG_Static_Bindings, tags: storagestaticbindings }   # attach Storage Domain to the EPG and deploy FC/FCoE Static Bindings to the EPG

  ############ L3Outs ############

  - { role: L3Outs,                                  tags: l3outs                }   # create L3Outs

  ########### Contracts ###########

  - { role: EPG_to_EPG_Contracts,                    tags: contracts             }   # create contracts between EPGs or External EPGs
```

3. Run the Ansible Playbook with:

```
ansible-playbook aci_playbook.yaml -i inventory
```

You can also call only specific roles by specifying tags

```
ansible-playbook aci_playbook.yaml -i inventory --tags tenants,l3outs
```

Another way to see the available tags is with the commmand

```
$ ansible-playbook aci_playbook.yaml -i inventory --list-tags

playbook: aci_playbook.yaml

  play #1 (apicsim_sandbox_example): ACI playbook       TAGS: []
      TASK TAGS: [aaeps, bds, contracts, domains, epgs, intpolicies, intpolicygroups, intselectors, l3outs, shutdownports, staticbindings, storagestaticbindings, switchprofiles, tenants, vlanpools]
```

## Directory tree

```
$ tree OperatingCiscoACIwithAnsible/
OperatingCiscoACIwithAnsible/
├── CHANGELOG.md
├── README.md
├── aci_playbook.yaml
├── host_vars
│   └── apicsim_sandbox_example.yaml
├── inventory
└── roles
    ├── AAEPs
    │   └── tasks
    │       └── main.yaml
    ├── AppProfiles_and_EPGs
    │   └── tasks
    │       └── main.yaml
    ├── BridgeDomains_and_Subnets
    │   └── tasks
    │       └── main.yaml
    ├── Domains
    │   └── tasks
    │       └── main.yaml
    ├── Domains_and_EPG_Static_Bindings
    │   └── tasks
    │       └── main.yaml
    ├── EPG_to_EPG_Contracts
    │   └── tasks
    │       └── main.yaml
    ├── Interface_Policies
    │   └── tasks
    │       └── main.yaml
    ├── Interface_Policy_Groups
    │   └── tasks
    │       └── main.yaml
    ├── Interface_Selectors
    │   ├── defaults
    │   │   └── main.yaml
    │   └── tasks
    │       └── main.yaml
    ├── L3Outs
    │   ├── tasks
    │   │   └── main.yaml
    │   └── templates
    │       └── L3Outs.xml.j2
    ├── MSO_Schemas_Templates
    ├── MSO_Tenants_Sites
    │   └── tasks
    ├── Storage_Domains_and_EPG_Static_Bindings
    │   ├── tasks
    │   │   └── main.yaml
    │   └── templates
    │       └── Storage_Domain_to_EPG_and_StaticBindings.j2
    ├── Switch_and_Interface_Profiles
    │   ├── defaults
    │   │   └── main.yaml
    │   └── tasks
    │       └── main.yaml
    ├── Tenants_and_VRFs
    │   └── tasks
    │       └── main.yaml
    ├── VLAN_Pools
    │   └── tasks
    │       └── main.yaml
    ├── enable_or_disable_ports
    │   └── tasks
    │       └── main.yaml
    └── shutdown_unused_ports
```


Disclaimer

*THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NON INFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.*
