[![Build Status](https://travis-ci.org/cimon-io/ansible-role-gobackup.svg?branch=master)](https://travis-ci.org/cimon-io/ansible-role-gobackup)

# Ansible Role for GoBackup

An ansible role that enables backups using [gobackup](https://gobackup.github.io/). It allows you to configure the backup system for MySQL/PostgreSQL/MongoDB/Redis databases and automate it with the **Cron** utility. Various storages are available.

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see defaults/main.yml):

```yaml
gobackup_user: "deploy"
gobackup_version: "0.6.2"
gobackup_url: "https://github.com/huacnlee/gobackup/releases/download/{{ gobackup_version }}/gobackup-linux-amd64.tar.gz"

gobackup_bin_path: "/usr/local/bin"
gobackup_config_path: "/etc/gobackup"
gobackup_data_path: "/var/backups"

gobackup_config: {}
gobackup_cron: {}
```

You are able to describe any number of backup models. Each of them requires the following information to be specified:

```yaml
## Models:
gobackup_config:
  models:
    my_app:
      compress_with:
        type: tgz
      store_with:
        type: local
        keep: 20
        path: /data/backups
      databases:
        my_app:
          database: my_app_production
          type: mysql
          host: localhost
          port: 3306
          username: root
      archive:
        includes:
          - /var/www/my_app/uploads
          - /var/www/my_app/shared/ssl
```

The cron job can include the following parameters:
- minute - minute when the job should run (0-59, \*, \*/2, etc);
- hour - hour when the job should run (0-23, \*', \*/2, etc);
- day - day of the month when the job should run (1-31, \*, \*/2, etc);
- weekday - day of the week when the job should run (0-6 for Sunday-Saturday, \*, etc).

```yaml
gobackup_cron:
  model_name:
    minute:
    hour:
    day:
    weekday:
```

### Databases

The role provides support for databases:
- MySQL
- PostgreSQL
- MongoDB
- Redis

## License

Licensed under the [MIT License](https://opensource.org/licenses/MIT).
