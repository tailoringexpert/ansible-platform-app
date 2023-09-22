# Ansible Role: tailoringexpert-app

[![CI](https://github.com/geerlingguy/ansible-role-mysql/workflows/CI/badge.svg?event=push)](https://github.com/geerlingguy/ansible-role-mysql/actions?query=workflow%3ACI)

Installs and configures tailoringexpert app part of the platform.

## Requirements

Some commands require a privileged user. This requires a user in the sudo group.
This user must be configured using the sudo_user variable.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
app_version: 0.0.5
app_snapshot: "-SNAPSHOT"

app_src: "{{ app_version }}/tailoringexpert-bootapp-{{ app_version }}{{app_snapshot}}-exec.jar"
app_serverport: 8443
app_logconfig_file: log4j2.xml


app_home: "/app"
app_lib: "{{ app_home }}/lib"
app_service: "{{ app_home }}/service"
app_config: "{{ app_home }}/config"
app_log: "{{ app_home }}/log"
app_template: "{{ app_home }}/templates"
app_db: "{{ app_home }}/db"


directories: [ 
  "{{ app_home }}", 
  "{{ app_lib }}", 
  "{{ app_service }}", 
  "{{ app_config }}",
  "{{ app_log }}",
  "{{ app_template }}",
  "{{ app_db }}"
]  
  
app:
  home: "{{ app_home }}"
  version: "{{ app_version }}"
  serverport: "{{ app_serverport }}"  
  src: "{{ app_src }}"
  config: "{{ app_config }}"
  lib:  "{{ app_lib }}"
  db: "{{ app_db }}"
  service: "{{ app_service }}"
  template: "{{ app_template }}"
  password: "{{ app_password }}"
  remote_src: no
  log: "{{ app_log }}"
  logconfig_file: "{{ app_logconfig_file }}"
  ssl:
    enabled: true
    src: "{{ inventory_hostname | lower}}/{{ ansible_host }}.p12"
    type: PKCS12 
    store: "{{ app_lib }}/{{ ansible_host }}.p12"
    password: DasIstDasHausVomNikolaus!
    alias: tailoringexpert
    protocol: TLS
    enabledProctols: TLSv1.2  
  libs:
    - src: "{{ app_version }}/lib/mariadb-java-client-3.0.9.jar"
      remote_src: no
      force: false
    - src: "{{ app_version }}/lib/tailoringexpert-security-{{ app_version }}{{app_snapshot}}.jar"
      remote_src: no
      force: true    
  datasource: "{{ platform.datasource }}"  


tenants: []  
service_dir: /etc/systemd/system 
```

## Dependencies

If you have `ansible` installed (e.g. `pip3 install ansible`), none.

## Example Playbook

	- hosts: app
	    ignore_errors: no
  
      roles:
        - role: tailoringexpert-app
	
*Inside `vars/main.yml`*:

    mysql_root_password: super-secure-password
    mysql_databases:
      - name: example_db
        encoding: latin1
        collation: latin1_general_ci
    mysql_users:
      - name: example_user
        host: "%"
        password: similarly-secure-password
        priv: "example_db.*:ALL"

## License

Apache-2.0 licence

## Author Information

This role was created in 2023 by [Michael Bädorf](https://www.tailoringexpert.eu/).
