# Ansible Role: HAProxy

Installs Tableau Server on Debian/Ubuntu Linux servers.

**Note**: This role supports (already tested) with tableau server versions 2018.2 and 2018.3 Future versions may require some rework.

## Requirements

None

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

```
tableau_version_major: 2018
tableau_version_minor: 3
tableau_version_patch: 0
tableau_download_deb_url: "https://downloads.tableau.com/esdalt/{{ tableau_version_major }}.{{ tableau_version_minor }}.{{ tableau_version_patch }}/tableau-server-{{ tableau_version_major }}-{{ tableau_version_minor }}-{{ tableau_version_patch }}_amd64.deb"
tableau_download_rpm_url: "https://downloads.tableau.com/esdalt/{{ tableau_version_major }}.{{ tableau_version_minor }}.{{ tableau_version_patch }}/tableau-server-{{ tableau_version_major }}-{{ tableau_version_minor }}-{{ tableau_version_patch }}.x86_64.rpm"

tsm_executable: "/opt/tableau/tableau_server/packages/customer-bin.20183.18.1019.1426/tsm"
tabcmd_executable: "/opt/tableau/tableau_server/packages/customer-bin.20183.18.1019.1426/tabcmd"

tableau_user: "ubuntu"
vault_file: ".vault"

init_admin_user: "tableau_admin"
```

The credentials are better stored in `ansible-vault`

```
  tsm_password: somepassword
  init_admin_pass: somepassword

```


## Dependencies

None.

## Example Playbook

```
- name: Install tableau server
  hosts: tableau-cluster-initial
  remote_user: someuser
  become_method: sudo
  become: yes
  vars_files:
    - "{{ vault_file }}"
  roles:
    - tableau-initial

- name: Install tableau cluster
  hosts: tableau-cluster-additional
  remote_user: someuser
  become_method: sudo
  become: yes
  vars_files:
    - "{{ vault_file }}"
  roles:
    - tableau-additional

- name: Config Tableau Cluster
  hosts: tableau-cluster-initial
  remote_user: someuser
  become_method: sudo
  become: yes
  vars_files:
    - "{{ vault_file }}"
  roles:
    - tableau-cluster
```

