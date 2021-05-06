# OperatingCiscoACIwithAnsible

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

4. Run the Ansible Playbook

```
ansible-playbook AccessPolicies.yaml -i inventory
ansible-playbook TenantPolicies.yaml -i inventory
```


By default, this Ansible playbook is pointing to [ACI Simulator AlwaysOn Sandbox](https://devnetsandbox.cisco.com/RM/Diagram/Index/5a229a7c-95d5-4cfd-a651-5ee9bc1b30e2?diagramType=Topology) which is free to use for testing purposes.

```
URL:      https://sandboxapicdc.cisco.com/
Username: admin
Password: ciscopsdt
```
[Cisco DevNet Sandbox labs](https://devnetsandbox.cisco.com/RM/Topology)




Disclaimer

*THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NON INFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.*
