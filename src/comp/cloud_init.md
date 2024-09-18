# cloud-init

Cloud-init is an open source initialisation tool. Designed to make it easier to get systems up and running with a minimum of effort, configured according to needs.

2 separate phasess during boot phase:

- early boot, pre-networking phase.
    - identify datasource
    - fetch configuration
    - write network configuration
- late boot, after network has been configured.
    - configuration management
    - installing software
    - user accounts
    - execute user scripts

## Configuration

- **Configuration directory** `/etc/cloud/cloud.cfg`, `/etc/cloud/cloud.cfg.d/*.cfg`
- **Runtime config** `/run/cloud-init/cloud.cfg`

## First Boot

On instance's first boot it should run all `per_instance` configuration.

On subsequent boots it should run only `per_boot` configuration.

cloud-init stores a cache of its internal state for use across stages and boots. If the cache is present, cloud-init `check` it is a subsequent boot (hence runs only "per-boot" configurations).

Cache is present if either

- instance has been rebooted, or
- filesystem has been attached to a new instance.

`cloud-init clean`

## Boot Stages

- Detect `ds-identify`: detects which platform instance is running on
- Local: locates local data sources, applies network config (can be disabled with `network: {config: disabled}`)
- Network: `cloud_init_modules` in */etc/cloud/cloud.cfg*
- Config: `cloud_config_modules` in */etc/cloud/cloud.cfg*
- Final: `cloud_final_modules` in */etc/cloud/cloud.cfg*
