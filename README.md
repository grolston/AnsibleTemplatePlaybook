# Template Playbook


The following contains the directory structure to start an Ansible Playbook which would contain multiple roles. The playbook currently outlines a master playbook called `master_template.yml` which in turn will be executed to call all the roles defined in the role directory. The template only delivers one role in the role directory called `common`. 


Roles Directory
---------------

The following details each subdirectory in a role directory.

### defaults
Within the defaults directory there is a main.yml file that contains the default variables used by a role. If you do not use variables in a role, you are not required to create the defaults directory.

### files
The files directory stores the files that need to be added to the device that is provisioned and do not required any modifications. Usually, files stored in this directory are references by copy tasks.

If you do not use such files in a role, you are not required to create the files directory.

### handlers
The handlers directory holds handlers that usually contain the targets of notify directives and are in most cases associated with services. For example, when creating a role that configures a network switch, the main.yml file in the handlers directory might have an entry that overwrites the startup configuration of the switch with its current running configuration and then restarts the device.

### meta
The meta directory stores the metadata of a role. The main.yml file of the meta directory holds metadata attributes such as the author of the role the supported platforms, and the role’s dependencies. For the most part, this file is commented out by default.

### tasks
The tasks directory holds various Ansible playbooks to install, configure, and run software. The majority of the Ansible activity is stored in this directory.

### templates
The templates directory stores files in a similar way to the files directory, except that the files held can be modified as they are added to the devices that are provisioned.
Modification to these files are done using the Jinja2 templating language. Most software configuration files become templates.

### vars
The vars and defaults directories both hold variables, the variables stored in the vars directory have a higher priority. Variables with a higher priority are more difficult to overwrite than variables with a lower priority. The variables stored in the defaults directory have the lowest priority available, meaning they are easy to overwrite.

Inside the vars directory there is a main.yml file that will contain the variables that you will define. You can also define variables in a playbook.

Best Practices Directory Layout
----------------------------
The following details additional directories a playbook can have. The example below is based on [Ansible Best Practices](https://docs.ansible.com/ansible/playbooks_best_practices.html#directory-layout) and may be for more advanced Ansible solutions.

```
production                # inventory file for production servers
staging                   # inventory file for staging environment

group_vars/
   group1                 # here we assign variables to particular groups
   group2                 # ""
host_vars/
   hostname1              # if systems need specific variables, put them here
   hostname2              # ""

library/                  # if any custom modules, put them here (optional)
filter_plugins/           # if any custom filter plugins, put them here (optional)

site.yml                  # master playbook
webservers.yml            # playbook for webserver tier
dbservers.yml             # playbook for dbserver tier

roles/
    common/               # this hierarchy represents a "role"
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

    webtier/              # same kind of structure as "common" was above, done for the webtier role
    monitoring/           # ""
    fooapp/               # ""
```
