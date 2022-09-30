[![Galaxy](https://raw.githubusercontent.com/roles-ansible/ansible_role_prometheus_openstack_exporter/main/.github/galaxy.svg)](https://galaxy.ansible.com/do1jlr/prometheus_openstack_exporter)
[![License](https://raw.githubusercontent.com/roles-ansible/ansible_role_prometheus_openstack_exporter/main/.github/license.svg)](https://github.com/roles-ansible/ansible_role_prometheus_openstack_exporter/blob/main/LICENSE)

ansible_role_prometheus_openstack_exporter
============================================
Ansible role to deploy the [https://github.com/openstack-exporter/openstack-exporter.git](https://github.com/openstack-exporter/openstack-exporter.git).

Theoretically, the exporter should run in a Systemd session without much effort. But since I somehow didn't get that error free it is now nested in a tmux session. A bit ugly, but at least it works.

Improvements are very very welcome!

 What do you need to get it running?
------------------------------------
What is absolutely needed is the clouds.yaml configuration.
Please put it in the ``openstack_exporter__clouds`` variable like this:
```yaml
openstack_exporter__clouds:
  clouds:
    default:
      region_name: {{ openstack_region_name }}
      identity_api_version: 3
      identity_interface: internal
      auth:
        username: {{ keystone_admin_user }}
        password: {{ keystone_admin_password }}
        project_name: {{ keystone_admin_project }}
        project_domain_name: 'Default'
        user_domain_name: 'Default'
        auth_url: {{ admin_protocol }}://{{ kolla_internal_fqdn }}:{{ keystone_admin_port }}/v3
```

By default the clouds: content will be deployed to ``/etc/openstack/clouds.yaml``

 Variables
-----------

| vaiable name | default value | description |
| ------------ | ------------- | ----------- |
| openstack_exporter__version: | ``'1.6.0'`` | the current used version of the openstack_exporter |
| openstack_exporter__releases: | *(see [defaults/main.yml](defaults/main.yml))* | the Download path of the released go binary |
| openstack_exporter__filename: | "openstack-exporter-{{ openstack_exporter__version }}.linux-{{ _arch }}" | the filename used to find and store the binary |
| openstack_exporter__user: | 'openstackexporter' | The user to run the openstack_exporter with |
| openstack_exporter__group: | 'openstackexporter' | The group to run the openstacl_exporter with |
| openstack_exporter__version_check: | ``true`` | Check if installed version != ``openstack_exporter__version`` before initiating binary download |
| openstack_exporter__systemd: | ``true`` | run systemd tasks *(currently the only option)* |
| openstack_exporter__clouds:  | ``clouds: {}`` | as described earlier the variable for the openstack clouds.yaml config |
| openstack_exporter__write_clouds_yaml: | ``true`` | deploy the ``/etc/openstack/clouds.yaml`` with this ansible role |
| openstack_exporter__first_port: | ``9123`` | first port we use to listen for the openstack exporter *(off by 1)* |
| openstack_exporter__no_log: | ``true`` | hide secrets from log |
| openstack_exporter__extra_arguments: | '' | optional additional parameter to start openstack-exporter with |
| openstack_exporter__os_user_domain_id: | 'default' | os_user_domain_id variable |
| openstack_exporter__os_project_domain_id: | 'default' | os_project_domain_id variable |
| submodules_versioncheck: | ``false`` | run optional versionscheck (true is recomended) |

## Other Tips
There is an example Dashboard in the ``.github`` Folder. ANother one that may work is the [9701](https://grafana.com/grafana/dashboards/9701) Dashboard.
