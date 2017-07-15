# Bootstrap role

This role can be used to bootstrap systems which require configuration prior to running normal Ansible playbooks against them.

## Installation
In order to use this role across multiple projects without having to explicitly include it in each project it should be made available in the default role path, typically `/etc/ansible/roles`:

    git clone git@github.com:schlueter/ansible-bootstrap-role
    sudo mv ansible-bootstrop-role /etc/ansible/roles/bootstrap

## Use

To include in an existing playbook call it in the first play, and do not gather_facts. It does not need become, though the ssh user does need sudo available (the [raw module](http://docs.ansible.com/ansible/raw_module.html) does not use become, but sudo may is used in the call in this role with it).

    - hosts: all
      gather_facts: no
      roles: [ bootstrap ]

Alternatively, the `bootstrap.yml` playbook may be executed against a host:

    ansible-playbook -i target-host, /etc/ansible/roles/bootstrap/bootstrap.yml

or in a Vagrantfile:

    Vagrant.configure(2) do |config|
      ...
      config.vm.provision :ansible,
        playbook: '/etc/ansible/roles/bootstrap/bootstrap.yml'
      config.vm.provision :ansible,
        playbook: 'playbooks/project-playbook.yml'
      ...

