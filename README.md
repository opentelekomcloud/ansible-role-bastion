OpenTelekomCloud Bastion role
=============================

A quick role to create a bastion server in the requested VPC

Requirements
------------

It is required, that openstacksdk is installed on the execution host and connection to the OTC is provided.

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):
  prefix: test- # This should be overridden in inventory
  domain_name: example.com # This also should be overridden in inventory

  server_subnet: "default-subnet" # should be on pair with default value from network_infra
  server_net: "{{ (prefix + 'otc-net') }}" # should be on pair with default value from network_infra
  security_group: "{{ (prefix + 'bastion_sg') }}"
  server_fqdn: "{{ ('bastion.' + domain_name) }}"
  server_image: "Standard_Fedora_29_latest"
  server_flavor: "s2.large.1"
  server_ssh_user: "linux"
  server_volume_size: 10
  assign_floating_ip: True
  # ssh_key_name: "{{ (infra_prefix + 'KeyPair')}}"
  server_keypair_name: "{{ (prefix + 'common-KeyPair') }}"

  # Path to private key file will be added to runtime inventory
  ansible_ssh_private_key_file: "{{ ('~/.ssh/' + server_keypair_name + '.pem') }}"

  # Optional python version for Ansible to use on the bastion for Ansible connection (take effect in runtime inventory)
  bastion_python: "/usr/bin/env python3"

  # State (`present` for creation, `absent` for deletion)
  state: present


Dependencies
------------

None.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: localhost
      roles:
         - otc_bastion

Cleanup of the resources is as easy, as it's creation. For that a variable 'state': 'false' should be passed:

    - hosts: localhost
      roles:
        - { role: otc_bastion, state: 'absent'}


License
-------

Apache


Author Information
------------------

OpenTelekomCloud
