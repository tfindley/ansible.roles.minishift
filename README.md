Role Name
=========

A simple role which will install Minishift and its dependencies, including libvirt.

Requirements
------------

This role has been tested against CentOS 7 and Ubuntu 18.04. Later verisons of both should work fine as should Debian.

It is worth noting that while Minishift provides a portable development environment which could be used on a laptop, it does appear to require an internet connection to function as it calls out to 8.8.8.8 and gitlab.com, presumably for it's git integration.

Warnings
--------

This role installs libvirtd and adds a user to the libvirt group. This will enable the hypervisor capabilities for a user.

Worth noting for corporate users that its use of 8.8.8.8 might fail if calls to external DNS servers are blocked by your Network or Cyber Security teams. 

Role Variables
--------------

There are two main variables that can be used with this role, however both have caviats.

- init: This variable is currently broken - Do not use! - See Bugs - True / False (bool) - When set to True init will trigger tasks/init.yml which will start up minishift on a machine. This is incredibly useful with new deployments as the first start can take upwards of 15 minutes. 
- user_name: prompt (string) - The role file will prompt for a username of a user to be added to libvirt group so they can start/stop minishift. 

Dependencies
------------

This role assumes the target system can run ansible commands up to version 2.7,t ansible dependencies have been installed, and that an ansible user exists on the system in addition to the user that will be using minishift. If this is not the case then simply remove the ansible_env.SUDO_USER entry from vars/main.yml::user_list

Bugs
----

Although there is an tasks/init.yml task to allow the first-run initialisation of Minishift, this is currently not working as Ansible does not know how ot handle the large amount of output created by the '$ minishift start' command.
For now I have commented out the tasks/init.yml task file in tasks/main.yml until I can resolve this issue. For now, ask users to run '$ minishift start' and then go and make a coffee (or go for lunch - it can take a stupidly long time on it's first run)

Roadmap
-----------
- Fix init script to allow first-time initlialisation
- Extract IP Address from first run of minishift start
- Consider external metric gathering from Prometheus


Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: kitsune
      become: yes
      gather_facts: true
      roles:
        - role: '/home/tristan/ansible/roles/minishift'
      vars:
        init: False # Do not change this for now - Currently broken. See README.md
      vars_prompt:
    # Use the following code if you want to prompt for the username to be added to the libvirt group,
      - name: user_name
        prompt: A user will need to be added to libvirt group so they can start Minishift. Please enter a username
        private: no


License
-------

BSD

Author Information
------------------

Tristan Findley - Please visit https://tfindley.co.uk to find out more.
