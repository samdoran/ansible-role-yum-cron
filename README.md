yum-cron
=========
[![Galaxy](https://img.shields.io/badge/galaxy-samdoran.yum--cron-blue.svg?style=flat)](https://galaxy.ansible.com/samdoran/yum_cron)
[![Build Status](https://travis-ci.com/samdoran/ansible-role-yum-cron.svg?branch=master)](https://travis-ci.com/samdoran/ansible-role-yum-cron)

Install and configure `yum-cron` or `dnf-automatic` to automatically install updates on RHEL.

Requirements
------------

None

Role Variables
--------------

The configuration options for RHEL 6 and RHEL 7 are different. The options for RHEL 7 and 8 are mostly the same.

On RHEL 7, there are `daily` and `hourly` configuration files. You may use one option for both, or define `daily` and `hourly` keys within the variable and it will be used in the appropriate template. See `defaults/main.yml` for examples.

Note that not all options are independently configurable. Options that take independent `daily` and `hourly` commands are indicated by by a `*`.

Also note that boolean values, such as `true` and `false`, must be quoted to ensure they are literal strings since the underlying config files are expecting `true` and `false`, not `True` and `False`.

### RHEL 7/8 Variables ###


| Name              | Default Value       | Description          |
|-------------------|---------------------|----------------------|
| `yumcron_update_cmd` | `default` | What kind of update to run. `*` |
| `yumcron_update_messages` | `{daily: 'yes', hourly: 'no'}` | Whether message should be emitted when updates are available. `*` |
| `yumcron_download_updates` | `{daily: 'yes', hourly: 'no'}` | Whether updates should be downloaded if available. `*` |
| `yumcron_apply_updates` | `false` | Whether to install updates if available. |
| `yumcron_random_sleep` | `{daily: 360, hourly: 15}` | Max time to randomly sleep in minutes. |
| `yumcron_system_name` | `None` | Name to use for the system when messages are emitted. `*` |
| `yumcron_emit_via` | `stdio` | How to send messages. Valid options are `stdio` and `email`. |
| `yumcron_output_width` | `80` | Width in characters of emitted messages. |
| `yumcron_email_from` | `root@localhost` | Email to send messages from. |
| `yumcron_email_to` | `['root']` | List of email addresses to send messages to. |
| `yumcron_email_host` | `localhost` | Name of host to connect to to send email messages. |
| `yumcron_group_list` | `None` | List of groups to update. |
| `yumcron_group_package_types` | `['mandatory, 'default']` | Types of group packages to install. |
| `yumcron_debuglevel` | `-2` | Use this to filter yum core messages. |
| `yumcron_skip_broken` | `[undefined]` | |
| `yumcron_mdpolicy` | `group:main` | |
| `yumcron_assumeyes` | `[undefined]` | Auto-import new gpg keys (dangerous). |
| `yumcron_command_format` | `cat` |  |
| `yumcron_stdin_format` | `{body}` |  |

### RHEL 6 Variables ###

| Name              | Default Value       | Description          |
|-------------------|---------------------|----------------------|
| `yumcron_yum_parameter` | `''` |  |
| `yumcron_check_only` | `'no'` | Just run `check-update` and do not download or install any packages. |
| `yumcron_check_first` | `'no'` | Ensure repos are reachable before doing anything. |
| `yumcron_download_only` | `'no'` | Only download updates but do not install them. |
| `yumcron_error_level` | `0` | Value passed to `--errorlevel` yum command line option. |
| `yumcron_debug_level` | `0` | Value passed to `--debuglevel` yum command line option. |
| `yumcron_randomwait` | `60` | Value passed to `--randomwait` yum command line option. |
| `yumcron_mailto` | `''` | Address to send messages to. |
| `yumcron_systemname` | `''` | Name of system to use in messages. |
| `yumcron_days_of_week` | `'0123456'` | Day numbers to run. |
| `yumcron_cleanday` | `'0'` | Day to clean `yum` cache. |
| `yumcron_service_waits` | `'yes'` | Whether or not to wait for service to complete before shutting down in the event the service manually is stopped while it's running. |
| `yumcron_service_wait_time` | `300` | Max wait time in seconds for the service to wait before returning failure. |

Dependencies
------------

None

Example Playbook
----------------

```yaml
    - hosts: all

      vars:
        yumcron_apply_updates:
          daily: 'yes'
          hourly: 'no'

      roles:
         - samdoran.yum-cron
```

License
-------

Apache 2.0
