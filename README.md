OpenTelekomCloud Bastion role
=============================

A quick role to create a bastion server in the requested VPC

Requirements
------------

It is required, that openstacksdk is installed on the execution host and connection to the OTC is provided.

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

    prefix: test- # should be overriden
    domain_name: example.com # used to create server_fqdn and meta-data, should be overridden
    server_name: bastion
    server_subnet: "default-subnet" # should be on pair with default value from network_infra
    server_net: "{{ (prefix + 'otc-net') }}" # should be on pair with default value from network_infra
    security_group: "{{ (prefix + 'bastion_sg') }}" # uses existing security group if the names are equal, otherwise a new sg will be created
    server_fqdn: "{{ (server_name + '.' + domain_name) }}"
    server_image: "Standard_Fedora_29_latest"
    server_flavor: "s2.large.1"
    server_ssh_user: "linux"
    server_volume_size: 10
    assign_floating_ip: True
    # ssh_key_name: "{{ (infra_prefix + 'KeyPair')}}"
    server_keypair_name: "{{ (prefix + 'common-KeyPair') }}" # use existing key pair or create new keypair from file, consider ansible_ssh_private_key_file variable
    
    # Path to private key file will be added to runtime inventory
    ansible_ssh_private_key_file: "{{ ('~/.ssh/' + server_keypair_name + '.pem') }}"
    
    # Optional python version for Ansible to use on the bastion for Ansible connection (take effect in runtime inventory)
    bastion_python: "/usr/bin/env python3"

    # State (`present` for creation, `absent` for deletion)
    state: present

Also you can specify availability zone by providing variable:

    availability_zone: "eu-de-03" # omitted if not defined

Dependencies
------------

None.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: localhost
      roles:
         - opentelekomcloud.bastion

Cleanup of the resources is as easy, as it's creation. For that a variable 'state': 'false' should be passed:

    - hosts: localhost
      roles:
        - { role: opentelekomcloud.bastion, state: 'absent'}

More advanced example:

    - hosts: localhost
      vars:
        security_group: my_bastion_sg
        server_net: my_network_name #openstack network list
        server_keypair_name: my_existing_public_key
        server_name: 'my_bastion_host'
        domain_name: 'my-domain.com'
      roles:
        - { role: opentelekomcloud.bastion, state: 'present'}


License
-------

Apache


Author Information
------------------

OpenTelekomCloud
