Galaxy Cloud Role Name
======================

Install Galaxy Production environment [1].
This role has been specifically developed to be used in the INDIGO project.

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

```
# Path to install the Galaxy software
galaxy_install_path: /home/galaxy
# Galaxy version to install
GALAXY_VERSION: "-b master origin/master"
# E-mail of the admin user
galaxy_admin: admin@admin.com
# User to launch the Galaxy portal
galaxy_user: galaxy
```

```
galaxycloud/              # this hierarchy represents a "role"
        tasks/            #
            main.yml      #  <-- tasks file can include smaller files if warranted
        handlers/         #
            main.yml      #  <-- handlers file
        templates/        #  <-- files for use with the template resource
            ntp.conf.j2   #  <------- templates end in .j2
        files/            #
            bar.txt       #  <-- files for use with the copy resource
            foo.sh        #  <-- script files for use with the script resource
        vars/             #
            main.yml      #  <-- variables associated with this role
        defaults/         #
            main.yml      #  <-- default lower priority variables for this role
        meta/             #
            main.yml      #  <-- role dependencies

```

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: galaxycloud }

License
-------

Apache Licence v2 [1]

References
-------

[1] https://galaxyproject.org/

[2] http://www.apache.org/licenses/LICENSE-2.0



