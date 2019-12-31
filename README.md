# docker_volume_backup

This role helps with making backups of your docker volumes for you.
All files are created as .gz files.
Currently, this will only backup a single volume at a time.

## Requirements

None

## Variables

``` yaml

# Required

# Defines what container to look for to copy the mount points for the backup job.
# Defaults: Null
dvb_container_name: string

# Defines what volume to backup
# Example: '/config'
# Defaults: Null
dvb_backup_volume: string

# Defines where the backup file will be generated
# Defaults: /tmp/docker/backup
dvb_tmp_backup: string

# Defines the location where the backup file will be stored.  Best result, external to your host.
# Defaults: Null
dvb_path_backup: string

# Optional

# Stops the container to run the backup on the data.  Once the backup is finished it will start the container again.
# Defaults: true
dvb_restart_container: bool

```

## Examples

``` yaml

- name: Backup single container data
  hosts: localhost
  vars:
    dvb_path_backup:      '/mnt/nfs/backups'
    dvb_backup_volume:    '/config'
    dvb_container_name:   'name'

  roles:
    - luther38.docker_volume_backup

```

``` yaml

- name: Backup multiple containers data
  hosts: localhost
  vars:
    dvb_path_backup:    '/mnt/nfs/backups'
    dvb_backup_volume:  '/config'

  tasks:
    - set_fact:
        dvb_container_name:   'name2'

    - include_role:
        name: luther38.docker_volume_backup

    - set_fact:
        dvb_container_name:   'name3'

    - include_role:
        name: luther38.docker_volume_backup




```

## Requirements.yml

As this repo is not published in galaxy and might never be.  You will need to either clone or add these lines to your requirements.yml file.

``` yml

roles:
  - src: https://github.com/luther38/ansible-role-docker-volume-backup
    name: luther38.docker_volume_backup
    version: master

```
