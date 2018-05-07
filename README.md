# Ansible Role: SQL Prepare DB

[![Build Status](https://api.travis-ci.org/fourforbusiness/ansible-role-mysql-prepare-db.svg?branch=master)](https://api.travis-ci.org/fourforbusiness/ansible-role-mysql-prepare-de) [![Ansible-Galaxy](https://img.shields.io/ansible/role/24231.svg)](https://galaxy.ansible.com/fourforbusiness/mysql-prepare-db/) ![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)

A collection of tasks that can be used as a toolset to prepare the target machines MySQL-Database.

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

* `remote_tmp_folder`            
The folder on the remote system where to copy the dumps
`local_dump_folder`     
The folder in the hosts file system with the dumps
* `mysql_import_dumps`              
The config variables for each dump.     
    * **Mandatory** sub-key/values are:
        * `filename` (string)
        * `database` (string) (database must exist on the given mysql-server)           
    
    * **Optional** sub-key/values are:
        * `replacements` (List)
            * `pattern` (string) + `replacement` (string)       
        The inclusion of `replacements` cause the role to replace all listed `pattern` found in the sql-file with `replacement`.        
          
* `mysql_additional_scripts`              
The additional mysql-scripts to execute (this is only executed if at least 1 dump was given for import and if it failes, it will not stop the playbook from provisioning)

## Dependencies

- [geerlingguy.mysql](https://galaxy.ansible.com/geerlingguy/mysql/)

## Example Playbook

    - hosts: db-servers
      become: yes
      vars_files:
        - vars/main.yml
      roles:
        - geerlingguy.mysql
        - fourforbusiness.mysql-prepare-db

*Inside `vars/main.yml`*:

        mysql_import_dumps:
          - filename: test_dump.sql
            database: test_database
            replacements:
              - pattern: xyz.de
                replacement: xyz.local
              - pattern: https
                replacement: http

## License

MIT / BSD

## Author Information

This role was created in 2018 by [four for business AG](https://www.4fb.de/).