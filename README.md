# NOTE:

This repository is no longer maintained - role has been moved to [Ansible
collection](https://docs.ansible.com/ansible/latest/collections_guide/index.html)
now ➡️ https://github.com/stackhpc/ansible-collection-openstack

OpenStack Volumes
=================

[![Build Status](https://www.travis-ci.org/stackhpc/ansible-role-os-volumes.svg?branch=master)](https://www.travis-ci.org/stackhpc/ansible-role-os-volumes)

This role can be used to register volumes and volume types in cinder using the
os\_volume module.

Requirements
------------

The OpenStack cinder API should be accessible from the target host.

Role Variables
--------------

`os_volumes_venv` is a path to a directory in which to create a virtualenv.

`os_volumes_auth_type` is an authentication type compatible with the
`auth_type` argument of `os_*` Ansible modules.

`os_volumes_auth` is a dict containing authentication information
compatible with the `auth` argument of `os_*` Ansible modules.

`os_volumes` is a list of volumes to register. Each item should be a dict
containing the following items:
- `display_description`: Optional description of the volume.
- `display_name`: Name of the volume.
- `image`: Optional image name or ID for boot from volume.
- `scheduler_hints`: Optional dict of scheduler hints pass to the volume API.
- `size`: Size of the volume in GB.
- `snapshot_id`: Optional ID of a volume snapshot from which to create the
  volume.
- `state`: Optional state of the volume, default is `present`.
- `volume`: Optional name or ID of a volume from which to create the volume.
- `volume_type`: Optional type of the volume.

`os_volumes_types` is a list of volume types to register. Each item should be a
dict containing the following items:
- `name`: Name of the volume type.
- `description`: Optional description of the volume type.
- `public`: Whether the volume type is public, default value is `True`.
- `extra_specs`: Optional dict of additional specifications for the volume
  type.

Dependencies
------------

This role depends on the `stackhpc.os_openstacksdk` role.

Example Playbook
----------------

The following playbook registers a cinder volume and volume type.

    ---
    - name: Ensure volumes and volume types are registered
      hosts: cinder-api
      roles:
        - role: os-volumes
          os_volumes_venv: "~/os-volumes-venv"
          os_volumes_auth_type: "password"
          os_volumes_auth:
            project_name: <keystone project>
            username: <keystone user>
            password: <keystone password>
            auth_url: <keystone auth URL>
          os_volumes:
            - display_name: my-volume
              size: 3
          os_volumes_types:
            - name: type-1
              extra_specs:
                volume_backend_name: my-backend

Author Information
------------------

- Mark Goddard (<mark@stackhpc.com>)
