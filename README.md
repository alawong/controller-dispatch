# controller-dispatch

AAP2 Automation Controller Dispatch playbook, that runs configuration updates for Automation Controller.

This lies in a separate repository tp prevent an issue when running the playbook from the originally centralised configuration repository, which held the dispatch playbook as well as the base configuration. The existence of variables, e.g. users, within the central repository caused the same variables found in other organisation repositories to be ignored.

## Getting Started

### Prerequirements

* Ansible Automation Platform 2 installed
* Bastion node with SSH access to Automation Controller hosts
* Execution environment downloaded with ansible.controller and redhat_cop.controller configuration collections installed, OR bastion node with ansible.controller and redhat_cop.controller configuration collections installed. [EE used](https://github.com/alawong/ee-aap-utils)

### Initial Controller Configuration and Configuration as Code Setup

1. SSH into the bastion node.

2. Clone the controller-org-default repository into the bastion node. This contains all of the default configuration for Automation Controller

    `git clone https://github.com/alawong/controller-org-default.git`

3. Move the `controller_config.yml` playbook into the controller-org-default directory. Change values as needed.
    `mv controller-dispatch/controller_config.yml controller-org-default/`

4. Change directory into the controller-org-default directory
    `cd controller-org-default`

5. Create an inventory with a host pointing to the controller

    ```bash
    controller ansible_host=<< controller_hostname >> ansible_connection=local
    ```

6. Run the `controller_config.yml` playbook using ansible-navigator and the Execution Environment. This will require a vault password to use any secrets.

    `ansible-navigator run controller_config.yml --eei << EE name >> --ask-vault-pass -m stdout --playbook-artifact-enable false -vvv`

7. Copy the webhook URL and key for each organisation workflow template and add them into the webhook details for their respective repositories, using the `application/json` encoding

8. Push the Execution Environment to Private Automation Hub with the name `controller-ee-rhel8:v1.0`.
