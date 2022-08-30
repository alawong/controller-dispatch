# controller-dispatch

AAP2 Automation Controller Dispatch playbook, that runs config updates for Automation Controller.

This was done to prevent an issue when running the playbook from the originally centralised repository, which held the dispatch playbook as well as the controller inventory and base configuration, where the existence of variables, e.g. users, within the central repository caused the same variables found in other organisation repositories to be ignored.


## Getting Started

### Requirements

. Bastion node with SSH access to Automation Controller hosts
. Execution environment with ansible.controller and redhat_cop.controller configuration collections installed, OR bastion node with ansible.controller and redhat_cop.controller configuration collections installed

### Configuration Installation

Run the controller_config playbook
