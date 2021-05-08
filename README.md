# Operating Cisco ACI with Ansible

## Install

1. Install Python. I.e.

```
sudo apt-get install python
```

2.  Install Ansible using pip which is Python's package manager

```
pip install ansible
```

Ensure Ansible is installed

```
ansible --version
```

3. Install Ansible's ACI Collection

```
ansible-galaxy collection install cisco.aci
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

  - Interface_Policies              # CDP, LLDP, Fibre Channel, MCP, Link Level, Port Channel, Spanning Tree Interface policies
  - Switch_and_Interface_Profiles   # Add Switches Profiles and Interface Profiles (needed when adding new switches to ACI)

  - Interface_Selectors             # Assigns the previous items to specific ports on the Switches
  - Interface_Policy_Groups         # Creates Interface Access Port, Port-Channels and vPCs
  - enable_or_disable_ports         # Interfaces (regular operation)

  - AAEPs                           # AAEPs, Domains and VLAN Pools
  - Domains
  - VLAN_Pools

  ###############################
  #                             #
  #           Tenants           #
  #                             #
  ###############################

  - Tenants_and_VRFs
  - BridgeDomains_and_Subnets
  - AppProfiles_and_EPGs
  - Domains_and_EPG_Static_Bindings
```

3. Run the Ansible Playbook with:

```
ansible-playbook aci_playbook.yaml -i inventory
```

## Directory tree

```
$ tree OperatingCiscoACIwithAnsible/

OperatingCiscoACIwithAnsible/
├── CHANGELOG.md
├── README.md
├── aci_playbook.yaml
├── host_vars
│   └── apicsim_sandbox_example.yaml
├── inventory
└── roles
    ├── AAEPs
    │   └── tasks
    │       └── main.yaml
    ├── AppProfiles_and_EPGs
    │   └── tasks
    │       └── main.yaml
    ├── BridgeDomains_and_Subnets
    │   └── tasks
    │       └── main.yaml
    ├── Domains
    │   └── tasks
    │       └── main.yaml
    ├── Domains_and_EPG_Static_Bindings
    │   └── tasks
    │       └── main.yaml
    ├── Interface_Policies
    │   └── tasks
    │       └── main.yaml
    ├── Interface_Policy_Groups
    │   └── tasks
    │       └── main.yaml
    ├── Interface_Selectors
    │   └── tasks
    │       └── main.yaml
    ├── Switch_and_Interface_Profiles
    │   └── tasks
    │       └── main.yaml
    ├── Tenants_and_VRFs
    │   └── tasks
    │       └── main.yaml
    ├── VLAN_Pools
    │   └── tasks
    │       └── main.yaml
    └── enable_or_disable_ports
        └── tasks
            └── main.yaml

26 directories, 17 files
```


Disclaimer

*THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NON INFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.*
