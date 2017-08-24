Galaxy Cloud Role Name
======================

Install Galaxy Production environment.
This role has been specifically developed to be used in the INDIGO project.

Requirements
------------
This ansible role supports CentOS 7, Ubuntu 14.04 Trusty and Ubuntu 16.04 Xenial

Minimum ansible version: 2.1.2.0

Role Variables
--------------

### Path ###

`galaxy_instance_description`: set Galaxy brand

`galaxy_user`: set linux user to launch the Galaxy portal (default: ``galaxy``).

`GALAXY_UID`: set user UID (default: ``4001``).

`galaxy_FS_path`: path to install Galaxy (default: ``/home/galaxy``).

`galaxy_directory`: Galaxy directory (usually galaxy or galaxy-dist, default ``galaxy``).

`galaxy_install_path`: Galaxy installation directory (default: ``/home/galaxy/galaxy``).

`galaxy_config_path`: Galaxy config pat location.

`galaxy_config_file`: Galaxy primary configuration file.

`galaxy_venv_path`:  Galaxy virtual environment directory (usually located to ``<galaxy_install_path>/.venv``).

`galaxy_custom_config_path`: Galaxy custom configuration files path (default: ``/etc/galaxy``).

`galaxy_custom_script_path`: Galaxy custom script path (defautl: ``/usr/local/bin``).

`galaxy_log_path`: log file directory (default: ``/var/log/galaxy``).

`galaxy_instance_url`: instance url (default:  ``http://<ipv4_address>/galaxy/``).

`galaxy_instance_key_pub`: instance ssh public key to configure <galaxy_user> access.

`galaxy_lrms`: enable  Galaxy virtual elastic cluster support. Currently supported local and slurm (default: ``local``, possible values: ``local, slurm``).


### https mode ###

The Galaxy portal runs through an `nginx` http proxy by default. The following
variables enable you to set nginx in https mode:

```
nginx_https: true
ssl_cert: /etc/certs/cert.pem
ssl_key: /etc/certs/key.pem
ssl_dhparam: /etc/certs/dhparam.pem
```

If `nginx_https` is set to `true`, the other ssl variables are required.
You can either request a signed trusted certificate or generated self-signed
certificate. An ansible role to generate self-signed certificate can be found
in https://galaxy.ansible.com/LIP-Computing/ssl-certs/

```
# Galaxy administrator username
GALAXY_ADMIN_USERNAME: "{{galaxy_admin_username}}"

# Galaxy administrator password.
# It is hard coded. To be changed by the administrator.
GALAXY_ADMIN_PASSWORD: "galaxy_admin_password"

# Galaxy administrator API_KEY. https://wiki.galaxyproject.org/Admin/API
# Please note that this key acts as an alternate means to access your account, and should be treated with the same care as your login password. To be changed by the administrator.
GALAXY_ADMIN_API_KEY: "GALAXY_ADMIN_API_KEY"

# Galaxy administrator e-mail address
GALAXY_ADMIN_EMAIL: "{{galaxy_admin_mail}}"
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

Apache Licence v2 [2]

References
-------

[1] https://galaxyproject.org/

[2] http://www.apache.org/licenses/LICENSE-2.0



