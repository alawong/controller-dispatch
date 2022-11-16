# controller-dispatch

AAP2 Automation Controller Dispatch playbook, that runs configuration updates for Automation Controller.

This lies in a separate repository to our default organisation's, as the existence of variables, e.g. users, within the default organisation's repository caused the same variables found in other organisation's repositories to be ignored when running the dispatch playbook.

## Getting Started

### Prerequisites

* Ansible Automation Platform 2 installed
* Bastion node with SSH access to Automation Controller hosts
* Execution environment downloaded with ansible.controller and redhat_cop.controller configuration collections installed, OR bastion node with ansible.controller and redhat_cop.controller_configuration collections installed. [EE used](https://github.com/alawong/ee-aap-utils). It is highly recommended that an EE is used, as this EE will be used within the configuration as code workflow templates too.

### Initial Controller Configuration and Configuration as Code Setup

1. SSH into the bastion node.

2. Clone the controller-dispatch and controller-org-default repository. This contains the configuration playbook `controller_config.yml` and all of the default configuration for Automation Controller, which includes organisations and their configuration as code workflows.

    `git clone https://github.com/alawong/controller-org-default.git`
    `git clone https://github.com/alawong/controller-dispatch.git`

3. Edit the inventory within `controller-org-default` to point to your controller host

    ```bash
    controller ansible_host=<< controller_hostname >> ansible_connection=local
    ```

4. Change directory into the controller-dispatch directory
    `cd controller-dispatch`

5. Run the `controller_config.yml` playbook using ansible-navigator and the Execution Environment. This will also require a vault password to decrypt Ansible Vault encrypted secrets.

    `ansible-navigator run controller_config.yml --eei localhost/ee-aap-utils:v1.0 --ask-vault-pass -m stdout --playbook-artifact-enable false -vvv -i ../controller-org-default/inventory`

6. Copy the webhook URL and key for each organisation workflow template and add them into the webhook details for their respective repositories workflows, using the `application/json` encoding

7. Push the Execution Environment to Private Automation Hub with the name `ee-aap-utils:v1.0` (what is currently used by Automation Controller).
